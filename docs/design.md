# ================================
# docs/design.md
# ================================
# Design Document — rnaseq_pipeline (Convergence Framework)

## Overview

The **rnaseq_pipeline** repository is designed to perform **reproducible RNA-seq analysis** with a specific downstream goal:

```text
To test whether distinct biological perturbations (e.g., genetic mutation and viral infection) converge on shared transcriptomic modules.
```

This repository serves as the transcriptomics layer in a broader system:

```text
variant_annotation_pipeline → genetic variants
gene_set_consensus         → phenotype-aware gene sets
rnaseq_pipeline            → transcriptomic convergence
```

---

## Core Hypothesis

Distinct perturbations (e.g., POLG-related mitochondrial dysfunction and EBV infection) converge on shared biological modules (e.g., mitochondrial stress, immune signaling, apoptosis), consistent with a two-hit stressor model.

---

## Design Philosophy

- Reproducibility over convenience 
- Convergence over pairwise gene relationships 
- Multi-layer evidence over single-statistic claims 
- Provenance preservation at every stage 
- No hidden transformations or silent corrections 

---

## Architecture

### High-Level Workflow

```text
FASTQ
  ↓
QC
  ↓
Alignment / Quantification
  ↓
Count Matrix
  ↓
Differential Expression (per dataset)
  ↓
Multi-layer Convergence Analysis
  ↓
Biological Interpretation
```

---

## Resource Constraints

The pipeline is designed for execution on:
- a developer workstation (Sys76 laptop)
- multiple independent worker nodes (Ubuntu desktops)

Constraints:
- limited RAM per node (~8–32 GB typical)
- limited CPU cores per node
- no shared high-speed filesystem
- no job scheduler

Design implications:
- per-sample processing must be memory bounded
- alignment tools must support low-memory modes
- intermediate files must be cleaned aggressively
- no stage assumes shared disk across nodes

---

## Repository Structure

```text
rnaseq_pipeline/
├── data/
│   ├── raw/
│   ├── interim/
│   └── processed/
├── docs/
│   └── design.md
├── results/
│   └── runs/
│       └── {run_id}/
│           ├── qc/
│           ├── counts/
│           ├── deg/
│           ├── enrichment/
│           └── convergence/
├── scripts/
├── src/
│   ├── qc/
│   ├── alignment/
│   ├── quantification/
│   ├── deg/
│   ├── enrichment/
│   └── convergence/
└── tests/
```

---

## Data Model

### Sample Metadata

```text
sample_id
study_id
condition
tissue
disease
batch
platform
fastq_1
fastq_2
```

### Count Matrix

```text
rows: genes
columns: samples
values: raw counts (primary)
normalized expression matrices are derived in downstream steps
```

### DEG Table

```text
gene_id
gene_symbol
logFC
p_value
adj_p_value
direction
```

---

## Processing Stages

### Stage 1 — Input and QC

**QC:**
- FASTQ validation 
- quality metrics (per sample) 
- read count summaries 
- optional trimming 

**Outputs:**
- QC reports
- validated FASTQ files

### Stage 2 — Alignment / Quantification

**Options (global choice per project not per dataset):**
- alignment-based (e.g., STAR + featureCounts)
OR
- pseudoalignment (e.g., Salmon)

**Constraint:**
A single quantification strategy must be used across all datasets within a given analysis to ensure comparability.

**Outputs:**
`gene-level count matrix`

#### Reference Requirements

All analyses must use a consistent reference genome and annotation:

- genome build (e.g., GRCh38)
- annotation source (e.g., GENCODE vXX or Ensembl vXX)

These must be recorded in the run configuration and enforced during aggregation.

### Stage 3 — Normalization

- library size normalization 
- transformation (e.g., log or variance-stabilized) 

No cross-study normalization at this stage.

Cross-study normalization, if performed, occurs only after aggregation and must explicitly model batch effects.

### Stage 4 — Differential Expression (within dataset)

Performed independently for each dataset:
`condition A vs condition B`

**Outputs:**
- DEG tables 
- ranked gene lists 

---

## Convergence Analysis Framework
This is the core of the repository.

### Layer 1 — DEG Overlap

Compare DEG sets across datasets.

**Methods:**
- Fisher / hypergeometric test 
- Jaccard index 
- permutation testing 

**Limitations:**
- threshold-sensitive 
- not sufficient alone 

### Layer 2 — Pathway Enrichment Overlap

For each dataset, get **pathway enrichment**:
- GO Biological Process 
- Reactome 
- KEGG (optional) 

**Compare:**
- shared enriched pathways 
- directionality 
- rank concordance 

**Key expectation:**
`shared enrichment in mitochondrial, immune, and stress-response pathways`

### Layer 3 — gene_set_consensus Enrichment

Integrate phenotype-aware gene sets from:
`gene_set_consensus repository`

**Test:**
- enrichment of consensus gene sets in DEG lists 
- enrichment in ranked gene signatures 

**Outputs:**
- enrichment scores 
- FDR 
- leading-edge genes 

This is the primary interpretation layer.

### Layer 4 — Ranked Signature Concordance

Compare full ranked gene lists across datasets.

**Methods:**
- Spearman correlation 
- top-k overlap 
- rank-based similarity metrics 

**Purpose:**
- reduce dependence on DEG thresholds 

### Layer 5 — Network Proximity

**Map genes onto interaction networks:**
- STRING 
- BioGRID 
- Reactome FI 

**Test:**
- proximity between DEG sets 
- clustering of consensus genes 
- permutation-based significance 

**Outputs:**
- network clustering metrics 
- distance distributions 

### Layer 6 — Module Inference (optional)

**If sample size allows:**
- co-expression module detection 
- module preservation across datasets 

**Goal:**
`identify shared perturbed modules rather than individual genes`

### Layer 7 — Topological Data Analysis (optional)

Used for:
- visualization 
- structure exploration 

Not used as primary statistical evidence.

---

## Cross-Study Strategy

### Preferred Approach

1. Reprocess all datasets from FASTQ
2. Perform DE within each study
3. Compare studies via:
    - overlap
    - enrichment
    - network proximity

Avoid naive pooling of count matrices across studies.

Naive pooling is permitted only when ALL of the following hold:
- batch effects are explicitly modeled
- study is not confounded with condition
- sufficient sample size exists within each group
- samples are derived from comparable tissue or cell type

---

## Statistical Framework

### Null Hypothesis
Distinct perturbations affect unrelated transcriptomic programs

### Alternative Hypothesis
Distinct perturbations converge on shared biological modules

### Evidence Requirements

Strong support requires agreement across:
- DEG overlap 
- pathway enrichment 
- gene_set_consensus enrichment 
- network proximity 
- ranked signature concordance

---

## Benchmarking Requirements

Before convergence claims:
[ ] QC metrics acceptable
[ ] sample clustering behaves as expected
[ ] DEG results consistent with source study
[ ] enrichment results biologically plausible
[ ] iDEP results broadly agree

---

## Distributed Computing Design

The pipeline is designed for:
`per-sample parallel execution`

### Execution Unit

The fundamental unit of execution is a single sample,
defined as a paired FASTQ dataset:

`sample_id → (fastq_1, fastq_2)`

Each worker node processes one sample independently.

```text
machine 1 → sample A
machine 2 → sample B
...
```

Aggregation occurs post-processing.

No inter-process communication required.

---

## Aggregation and Result Integration

All per-sample and per-study outputs generated on distributed worker nodes
must be centrally aggregated prior to convergence analysis.

### Aggregation Responsibilities
- collect QC outputs
- merge count matrices (per dataset, not across datasets)
- merge DEG tables (per dataset)
- verify sample completeness
- enforce consistent sample metadata

### Aggregation Location
Aggregation is performed on the developer unit (Sys76 laptop).

### Data Integrity Checks
- verify expected sample count
- verify consistent gene identifiers
- verify no duplicate sample IDs
- verify matching annotation versions

No convergence analysis is performed until aggregation checks pass.

---

## Limitations
- RNA-seq cannot establish causality 
- cross-study comparisons are sensitive to batch effects 
- rare disease datasets are often underpowered 
- tissue heterogeneity limits interpretation 

---

## Expected Outputs
- QC reports 
- count matrices 
- DEG tables 
- enrichment tables 
- convergence metrics 
- network analysis results 

---

## The Relationship Map for Key Repositories

**variant_annotation_pipeline**
- provides: `genetic variant context`

**gene_set_consensus**
- provides: `phenotype-aware gene ranking`

**rnaseq_pipeline** (this design document)
- provides: `transcriptomic convergence analysis`

---

## Run Tracking and Reproducibility

Each pipeline execution is assigned a unique `run_id`.

For each run, the following are recorded:
- configuration parameters
- reference genome version
- annotation version
- pipeline version
- input sample manifest
- execution timestamps

All outputs are stored under:

results/runs/{run_id}/

This enables:
- reproducibility
- debugging
- comparison across runs

---

## Sample Manifest

All runs are driven by a manifest file (e.g., CSV or YAML) containing:

- sample_id
- study_id
- condition
- fastq_1
- fastq_2
- metadata fields (tissue, disease, batch)

This manifest is the authoritative input to the pipeline.

---

## Failure Handling

The pipeline must tolerate partial execution across distributed nodes.

Failure scenarios:
- incomplete FASTQ processing
- failed alignment
- truncated output files

Mitigation:
- per-sample completion flags
- QC checks at aggregation stage
- ability to re-run individual samples without rerunning entire dataset

---

## Summary

```text
This repository tests whether genetic and infectious perturbations
converge on shared transcriptomic modules using a layered,
statistically defensible framework combining differential expression,
pathway enrichment, consensus gene sets, and network analysis.
```

---

# End of design.md