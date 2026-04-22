# dataset_matching_rank_spec.md

# Dataset Matching Rank Specification

## Status
Proposed pre-processing specification for RNA-seq dataset triage in `rnaseq_pipeline`.

---

## Purpose

Define a reproducible, explainable pre-processing method for evaluating how well candidate RNA-seq datasets match one another **for downstream convergence analysis**.

This specification is intended to operate **before SRR-level selection** and before pipeline execution.

Its role is to help answer:

```text
Which datasets are sufficiently comparable to support meaningful pathway,
phenotype-support, module, or network convergence analyses?
```

---

## Why This Exists

The `rnaseq_pipeline` is not designed for naïve cross-dataset DEG reciprocity.

Instead, the current scientific strategy is:

```text
within-dataset DEG
→ cross-dataset comparison at higher abstraction layers
```

Those higher layers include:
- Level 1 — direct gene overlap
- Level 2 — pathway overlap
- Level 3 — phenotype support overlap
- Level 4 — module-level convergence
- Level 5 — explicit network inference / network convergence

To support those analyses, candidate datasets must first be evaluated for **design comparability**.

---

## Important Clarification

`matching_rank` is **not** a standard off-the-shelf transcriptomics metric.

It is a **project-specific heuristic scoring framework** designed to formalize dataset triage for the current portfolio architecture.

It is informed by common transcriptomics best practices, including:
- within-study contrast validity
- batch-awareness
- replicate sufficiency
- cell-type comparability
- confounder control
- preference for clean experimental design

But the final weighted scoring system is custom-built for this project.

---

## What Is Standard vs Custom

### Standard transcriptomics practice
The following are common and expected:
- requiring clear case vs control contrasts
- preferring same-cell-type comparisons within datasets
- requiring sufficient biological replicates
- excluding or deferring single-cell datasets when building a bulk RNA-seq pipeline
- avoiding direct pooling of highly heterogeneous studies
- comparing studies at the pathway/module level when raw DEG comparability is weak

### Custom project layer
The following are custom to this project:
- a formal pairwise `matching_rank`
- explicit weighted scoring across design factors
- use of the score to decide which datasets should anchor Level 4–5 convergence analyses
- integration of matching logic with the repo ecosystem (GSC, RSP, RDGP)

---

## Core Principle

The goal is not to identify the “best” dataset in isolation.

The goal is to identify the **best-matched dataset set** for a convergence-oriented experimental design.

```text
Good individual datasets may still form a poor convergence design
if they are too mismatched in cell type, perturbation structure, or model system.
```

---

## Matching Rank Definition

Define:

```text
matching_rank(dataset_A, dataset_B) = weighted similarity score from 0 to 100
```

The score represents how suitable two datasets are for later cross-dataset convergence interpretation.

Higher scores indicate stronger design comparability.

---

## Scoring Components

### 1. Cell Type Similarity
This is the highest-priority component because cell identity is often the strongest hidden confounder in transcriptomics.

Suggested rubric:
- same cell type = 1.0
- same lineage / closely related differentiated state = 0.7
- different tissue but plausibly related biology = 0.3
- unrelated = 0.0

Examples:
- neuron vs neuron = 1.0
- NPC vs neuron = 0.7
- fibroblast vs neuron = 0.3
- B cell vs fibroblast = 0.0–0.2

---

### 2. Perturbation Type Alignment
Measures whether the causal structure of the contrast is comparable.

Suggested rubric:
- same perturbation class (e.g., genotype vs genotype) = 1.0
- closely related perturbation classes = 0.8–0.9
- infection vs genotype = 0.6
- drug vs genotype = 0.5
- weakly comparable = 0.2–0.4

Examples:
- POLG mutant vs TSC2 KO = genotype vs genotype = high
- EBV infection vs POLG mutant = infection vs genotype = moderate

---

### 3. Model System Similarity
Measures how similar the biological system is.

Suggested rubric:
- human primary = 1.0
- human iPSC-derived = 0.9
- human established cell line = 0.7
- mouse / non-human mammalian = 0.4
- mixed or heavily transformed systems = 0.2–0.5

This is separate from cell type because two datasets may share a lineage but differ greatly in model realism.

---

### 4. Experimental Complexity / Confounding Load
Rewards designs with fewer competing variables.

Suggested rubric:
- single clean variable = 1.0
- subsettable two-factor design = 0.7
- time-course or multi-factor design = 0.3–0.5
- highly entangled design = 0.0–0.2

Examples:
- WT vs KO = 1.0
- genotype + treatment, but subsettable = 0.7
- full developmental time-course with multiple lineages = low

---

### 5. Replicate Strength
Captures whether both datasets support statistically meaningful within-dataset DEG generation.

Suggested rubric:
- >= 3 replicates per group = 1.0
- exactly 2 replicates per group = 0.6
- unbalanced or ambiguous = 0.3–0.5
- inadequate = 0.0

For the current project, the working minimum is:

```text
>= 3 replicates per condition
```

---

### 6. Signal Type Compatibility
Estimates whether the datasets are likely to yield interpretable outputs at Level 2–5.

Suggested rubric:
- strong pathway-compatible signal = 1.0
- moderate signal = 0.7
- weak / noisy / low expected interpretability = 0.3–0.5

Examples:
- EBV lytic reactivation = strong signal
- subtle neuronal genotype effect = moderate

---

### 7. Convergence Relevance
This is the project-specific layer that asks whether the pair is scientifically useful for the convergence hypothesis.

Suggested rubric:
- direct convergence relevance = 1.0
- plausible shared-system relevance = 0.7
- weak or speculative relevance = 0.3–0.5
- little relevance = 0.0–0.2

Examples:
- POLG vs broader mitochondrial dysfunction = direct
- EBV vs mitochondrial dysfunction = plausible shared metabolic / immune stress
- SCN2A vs TSC2 = strong neuronal pathway relevance

---

## Suggested Weights

These weights are heuristic, not canonical.

They reflect the current project logic that:
- cell type matters most
- clean perturbation structure matters strongly
- convergence analyses should be biologically meaningful, not merely technically comparable

Suggested weights:

- Cell type similarity = 0.25
- Perturbation type alignment = 0.15
- Model system similarity = 0.10
- Experimental complexity = 0.10
- Replicate strength = 0.10
- Signal type compatibility = 0.15
- Convergence relevance = 0.15

These sum to 1.00.

---

## Calculation

For each pair of datasets:

```text
matching_rank = 100 × Σ(weight_i × component_score_i)
```

Where each component score ranges from 0.0 to 1.0.

---

## Interpretation Bands

Suggested interpretation:

- 80–100 = strong convergence candidate
- 60–79 = usable with caution, especially for pathway-level comparisons
- 40–59 = weak match; use only for exploratory convergence questions
- <40 = poor match for convergence claims

---

## Example Pairwise Logic

### SCN2A neurons vs TSC2 NPCs
Likely high-moderate / high score because:
- same broad developmental lineage
- genotype-driven contrasts
- human-derived systems
- strong convergence relevance for neuronal systems biology

### POLG fibroblasts vs broader mitochondrial fibroblasts
Likely high score because:
- same cell type
- same broad biological axis
- strong mitochondrial convergence relevance

### EBV B-cell system vs POLG fibroblasts
Likely moderate score because:
- major cell-type mismatch
- infection vs genotype mismatch
- but scientifically valuable convergence hypothesis at pathway / network level

---

## Relationship to the RSP Analysis Ladder

`matching_rank` is most important for:
- Level 2 — pathway overlap
- Level 3 — phenotype support overlap
- Level 4 — module-level convergence
- Level 5 — network convergence

It is less important for Level 1, where direct gene overlap is already expected to be weak across heterogeneous biological systems.

---

## Recommended Use in Workflow

### Stage 1 — Dataset triage
Score all candidate dataset pairs.

### Stage 2 — Anchor set selection
Choose the dataset set that maximizes:
- biological coverage across axes
- pairwise matching quality
- convergence relevance

### Stage 3 — Gap detection
Identify weakly matched but scientifically important pairs.

### Stage 4 — Targeted expansion search
Search for 1–3 additional datasets that specifically improve weak links.

---

## What This Method Is Not

This method is not:
- a formal batch-correction algorithm
- a substitute for within-dataset QC
- a replacement for proper differential expression modeling
- a community standard benchmark metric

It is:

```text
a structured pre-processing decision framework for selecting datasets
that can support meaningful convergence analyses.
```

---

## Strengths

- forces explicit reasoning about comparability
- prevents impulsive dataset collection
- documents why certain datasets are favored
- supports reproducible triage logic
- aligns dataset selection with the repo ecosystem and release roadmap

---

## Limitations

- heuristic, not inferential
- dependent on metadata quality
- some component scores require subjective judgment
- cannot rescue biologically incompatible datasets
- does not eliminate downstream batch effects

---

## Future Improvements

Possible later upgrades:
- empirical calibration using actual DEG/pathway outputs
- separate matching ranks for Level 2 vs Level 5 use cases
- metadata extraction template for automated scoring
- weighted uncertainty ranges instead of point estimates

---

## Recommended Storage Location

This document should live with the dataset triage materials for `rnaseq_pipeline`, for example:

```text
rnaseq_pipeline/docs/maps/dataset_matching_rank_spec.md
```

or, if you want it treated as shared triage methodology:

```text
shared/research/dataset_matching_rank_spec.md
```

For now, the cleaner choice is:

```text
rnaseq_pipeline/docs/maps/dataset_matching_rank_spec.md
```

because it is tightly coupled to the current RSP triage problem.

---

## One-Sentence Summary

`matching_rank` is a project-specific heuristic for scoring how well two RNA-seq datasets match in experimental-design space for downstream convergence analysis.

---

# End of dataset_matching_rank_spec.md
