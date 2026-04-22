# 🧠 Edge Scoring Framework for Network Convergence Path Assembly

**Repository:** rnaseq_pipeline (RSP)
**Location:** `/docs/design_rationale/edge_scoring_framework.md`

---

## Overview

This document defines the **edge scoring framework** used to assemble convergence paths across RNA-seq datasets (GEOs) in the RSP system.

The goal is to support:

```text
data-driven inference of biologically plausible convergence paths
across distinct perturbation classes
```

Rather than imposing a fixed biological route (e.g., EBV → neuron → mitochondria), this framework allows:

```text
each dataset to define its own network footprint,
and convergence paths to emerge from shared or proximal network structure
```

---

## Conceptual Model

Each GEO dataset is treated as a **node** in a graph:

```text
G = (V, E)
```

* **V (nodes)** = GEO datasets
* **E (edges)** = similarity between datasets

Edges are weighted based on **multi-layer biological similarity**, not a single metric.

---

## Design Principle

```text
No single metric captures biological similarity across datasets.
```

Therefore, edge weights are defined as a **composite, decomposable score**:

```text
edge_weight = f(pathway, module, network, phenotype, context)
```

Each component reflects a different level of biological organization.

---

## Edge Components

### 1. Pathway Similarity (Weight: 0.30)

**Definition:**
Similarity of enriched pathways between two datasets.

**Implementation options:**

* Jaccard overlap of significant pathways
* Correlation of pathway enrichment scores (e.g., NES from GSEA)

**Rationale:**

```text
Pathways provide a robust, noise-resistant representation of transcriptomic change.
```

---

### 2. Module Overlap (Weight: 0.25)

**Definition:**
Overlap or similarity of co-expression modules or network communities.

**Implementation options:**

* WGCNA module overlap
* shared gene clusters
* module activation similarity

**Rationale:**

```text
Modules capture coordinated biological programs beyond individual pathways.
```

---

### 3. Network Proximity (Weight: 0.25)

**Definition:**
Topological closeness of DEG sets within a biological network.

**Implementation options:**

* average shortest path distance in PPI network
* network separation score
* diffusion-based proximity

**Rationale:**

```text
Biologically related perturbations often affect nearby regions of molecular networks.
```

---

### 4. Phenotype Similarity (Weight: 0.10)

**Definition:**
Similarity of phenotype-associated gene support (e.g., GSC output).

**Implementation options:**

* overlap of phenotype-associated genes
* correlation of phenotype support scores

**Rationale:**

```text
Anchors convergence in clinically interpretable biology.
```

---

### 5. Context Penalty (Weight: -0.10)

**Definition:**
Penalty applied for biological mismatch.

**Examples:**

* neuron vs immune cell mismatch
* species mismatch
* highly divergent experimental systems

**Rationale:**

```text
Transcriptomic similarity across incompatible contexts is less reliable.
```

---

## Composite Edge Score

```text
edge_weight =
    0.30 * pathway_similarity
  + 0.25 * module_overlap
  + 0.25 * network_proximity_score
  + 0.10 * phenotype_similarity
  - 0.10 * context_penalty
```

All component scores are normalized to [0,1].

---

## Critical Design Constraint

```text
Edge weights must remain decomposable and auditable.
```

We do NOT allow:

* black-box scoring
* hidden weighting schemes
* single aggregated scores without component visibility

Each edge must be reportable as:

```text
edge(A,B):
  pathway = X
  module = Y
  network = Z
  phenotype = W
  penalty = P
  total = T
```

---

## Avoiding Subjectivity

A key concern is:

```text
Do edge weights reflect biological truth or researcher bias?
```

This framework addresses that through three mechanisms.

---

### 1. Sensitivity Analysis

We test robustness to weight variation:

```text
vary weights across plausible ranges:
- pathway-heavy
- network-heavy
- equal-weight models
```

Then evaluate:

```text
Does the inferred convergence path remain stable?
```

**Interpretation:**

* Stable path → robust biological signal
* Unstable path → weak or ambiguous convergence

---

### 2. Multi-Metric Agreement

We require agreement across independent signals:

```text
Strong edges show consistency across:
✔ pathway similarity
✔ module overlap
✔ network proximity
✔ phenotype support
```

Edges supported by only one metric are down-weighted in interpretation.

---

### 3. Null / Permutation Testing

To assess statistical significance:

* shuffle gene labels or DEG sets
* recompute similarity metrics
* generate null distributions

Then evaluate:

```text
Is observed similarity greater than random expectation?
```

This converts similarity into **statistically grounded evidence**.

---

## Path Assembly

Once edge weights are defined, path inference becomes a graph problem.

### Methods:

* **Shortest path (minimize 1 - weight)**
* **Maximum weight path**
* **Top-k path enumeration**

---

## Output Strategy

We do NOT report a single path.

Instead:

```text
Report top N candidate convergence paths
```

Example:

| Path                                   | Score |
| -------------------------------------- | ----- |
| EBV → N+immune → SCN2A → N+mito → POLG | 0.82  |
| EBV → mito → POLG → mTOR → SCN2A       | 0.78  |

---

## Interpretation Framework

```text
Truth ≠ one correct path
Truth = reproducible convergence signal across independent representations
```

A convergence path is considered credible if:

* it is stable across weighting schemes
* it exceeds null expectations
* it is supported by multiple metrics
* it aligns with known biological relationships

---

## Relationship to RSP Analysis Levels

| Level                | Role in Edge Scoring               |
| -------------------- | ---------------------------------- |
| Level 1 (DEGs)       | input only                         |
| Level 2 (Pathways)   | pathway similarity                 |
| Level 3 (Phenotypes) | phenotype similarity               |
| Level 4 (Modules)    | module overlap                     |
| Level 5 (Networks)   | network proximity + path inference |

---

## Summary

```text
The edge scoring framework enables
data-driven, auditable, and biologically grounded
inference of convergence paths across heterogeneous transcriptomic datasets.
```

It replaces:

```text
subjective pathway assumptions
```

with:

```text
multi-layer similarity + robustness validation
```

---

# End of Document
