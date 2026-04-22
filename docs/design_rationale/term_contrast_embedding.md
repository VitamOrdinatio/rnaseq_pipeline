# 🧠 Term-Based Multidimensional Embedding for Cross-Dataset Convergence

**Repository:** rnaseq_pipeline (RSP)
**Location:** `/docs/design_rationale/term_contrast_embedding.md`

---

## Overview

This document defines the framework for representing RNA-seq dataset contrasts in a shared **multidimensional biological space** using enrichment-derived features.

The goal is to enable:

```text
robust, cross-dataset comparison of biological perturbations
without relying on direct gene-level overlap
```

This approach supports downstream:

* clustering of datasets
* convergence detection
* network proximity analysis
* path assembly across GEO datasets

---

## Core Design Principle

```text
The fundamental unit of comparison is:
dataset contrast → enrichment-derived feature vector
```

NOT:

```text
individual DEG → GO term mapping
```

---

## Rationale

Gene-level comparisons across datasets are unstable due to:

* batch effects
* context-dependent expression
* noise in DEG thresholding

Instead, we compare datasets at higher levels of abstraction:

* pathways
* biological processes
* phenotypes
* network modules

This aligns with established practices in transcriptomics:

```text
pathway-level signals are more robust than gene-level overlap
```

---

## Data Flow

For each dataset contrast:

```text
RNA-seq dataset
→ differential expression (DEG or ranked list)
→ enrichment analysis
→ feature vector construction
→ embedding in shared term space
```

---

## Feature Space Definition

### Term Universe

The multidimensional space is defined as:

```text
the union of all enriched terms across all dataset contrasts
```

Each term represents one axis in the space.

---

### Feature Types

The framework supports multiple term categories:

#### 1. Pathway / Process Space

* GO Biological Process
* Reactome
* KEGG

#### 2. Phenotype Space

* Human Phenotype Ontology (HPO)
* disease-associated gene sets

#### 3. GSC Evidence Space (future integration)

* phenotype-weighted gene support models

---

## Feature Encoding

Each dataset contrast is represented as a vector:

```text
V = [t₁, t₂, t₃, ..., tₙ]
```

Where each dimension corresponds to a term.

---

### Value Encoding (Critical)

Binary encoding is NOT recommended:

```text
term present = 1
term absent = 0
```

Instead, use **signed continuous values**:

Examples:

* normalized enrichment score (NES)
* signed −log10(FDR)
* enrichment score with direction

```text
strong up-regulation → positive value
strong down-regulation → negative value
no enrichment → 0
```

---

### Why Continuous Encoding

```text
Preserves:
✔ effect size
✔ directionality
✔ statistical strength
```

Binary encoding discards critical biological information.

---

## Matrix Structure

After processing all datasets:

```text
Matrix M:
rows    = dataset contrasts
columns = union of all terms
values  = signed enrichment scores
```

---

## Preprocessing Considerations

### 1. Redundancy Reduction

GO terms are hierarchical and redundant.

Mitigation strategies:

* semantic similarity pruning
* term clustering
* selecting representative terms

---

### 2. Use Ranked Gene Lists

Preferred over hard DEG thresholds:

```text
ranked gene list → GSEA-style enrichment
```

This improves stability and sensitivity.

---

### 3. Separate Up/Down Signals

Do NOT collapse direction:

```text
up-regulated pathways ≠ down-regulated pathways
```

---

## Analytical Methods

Once the matrix is constructed:

### 1. Distance / Similarity Metrics

* cosine similarity
* correlation
* Euclidean distance

---

### 2. Dimensionality Reduction

#### PCA (recommended first)

* captures major axes of variation
* interpretable

#### UMAP / t-SNE

* captures local neighborhood structure
* useful for clustering visualization

#### TDA (optional, advanced)

* captures topological structure
* use only if justified

---

### 3. Clustering

* hierarchical clustering
* k-means (with caution)
* graph-based clustering

---

## Role in Convergence Framework

This embedding supports multiple RSP levels:

| Level   | Role                      |
| ------- | ------------------------- |
| Level 1 | input (DEGs)              |
| Level 2 | pathway features          |
| Level 3 | phenotype features        |
| Level 4 | module inference          |
| Level 5 | convergence path assembly |

---

## Integration with Path Assembly

The embedding enables:

```text
quantification of similarity between datasets
```

which feeds into:

```text
edge scoring between GEO nodes
→ path inference
→ convergence mapping
```

---

## Multi-Space Strategy (Recommended)

Instead of a single embedding, maintain:

```text
1. pathway/process space
2. phenotype space
3. GSC evidence space
```

Then evaluate:

```text
Does convergence occur consistently across spaces?
```

---

## Interpretation Framework

```text
Convergence is supported when:
✔ datasets cluster in term space
✔ proximity is consistent across multiple spaces
✔ signals align with biological expectations
```

---

## What This Framework Is NOT

* Not a direct gene-level comparison
* Not dependent on exact DEG overlap
* Not a claim of causality

---

## What This Framework Enables

```text
✔ robust cross-study comparison
✔ noise reduction
✔ biologically interpretable clustering
✔ foundation for network convergence analysis
```

---

## Summary

```text
Dataset contrasts are embedded in a shared,
continuous biological feature space defined by enrichment signals.

This enables comparison across heterogeneous datasets
and supports downstream inference of convergence pathways.
```

---

# End of Document
