# dataset_candidates.md

## Overview

This document summarizes **candidate RNA-seq datasets** for the `rnaseq_pipeline` (v1) based on:

* Pipeline design constraints (distributed, no HPC)
* Data quality requirements
* Biological relevance to mitochondrial dysfunction and EBV
* Feasibility for initial smoke testing and benchmarking

---

## Dataset Selection Criteria

### Hard Constraints

* RNA-seq (NOT microarray)
* Public FASTQ available (SRA-accessible)
* Paired-end reads
* Manageable size (≤ ~10–15 GB per sample preferred)
* Clear experimental design (case vs control)
* Same tissue/cell type within dataset
* Small subset usable (≤ 4–6 samples for v1)

---

### Soft Constraints

* Published study available (for benchmarking)
* Strong biological signal (clear perturbation)
* Compatible with iDEP validation
* Relevant to:

  * mitochondrial function
  * EBV infection
  * immune / metabolic stress

---

## Recommended Dataset Strategy

### Phase 1 (v1 — Pipeline Validation)

**Goal:** Validate end-to-end RNA-seq pipeline execution

Use:

* Clean dataset
* Strong signal
* Minimal biological complexity

---

### Phase 2 (v2 — Biological Relevance)

**Goal:** Introduce disease-relevant transcriptomics

Add:

* Human mitochondrial disease dataset

---

### Phase 3 (v3 — Convergence Analysis)

**Goal:** Test shared network/module hypothesis

Compare:

* EBV dataset vs mitochondrial dataset

---

## Dataset Candidates

---

### 1. EBV Lytic Reactivation Dataset (Primary v1)

**Dataset:** GSE240008
**Type:** RNA-seq (cell line, EBV reactivation)

#### Why this dataset

* Strong transcriptional signal (Zta/Rta induction)
* Clean experimental design
* Multiple SRRs available (subset easily)
* Excellent for benchmarking pipeline behavior
* Compatible with iDEP

#### Limitations

* Cell line (not primary human tissue)
* Not ideal for final biological claims

#### Recommended Use

Select 4–6 samples:

* Control (uninduced)
* Zta-induced
* Rta-induced

#### Role in Pipeline

```text
FASTQ → QC → counts → DEG → validate pipeline
```

---

### 2. Mitochondrial Disease Dataset (v2)

**Dataset:** GSE120854
**Type:** RNA-seq (human fibroblasts)

#### Why this dataset

* Human-derived samples
* Directly relevant to mitochondrial dysfunction
* Cleaner than tissue biopsies
* Aligns with POLG hypothesis space

#### Limitations

* Not POLG-specific
* May include heterogeneous mitochondrial disorders

#### Role in Pipeline

```text
Second dataset → validate cross-dataset processing
```

---

### 3. Human EBV Infection Dataset (Optional / v2–v3)

**Dataset:** GSE161731
**Type:** RNA-seq (human samples)

#### Why this dataset

* Human-derived (more clinically relevant)
* EBV infection context
* Captures immune + metabolic effects

#### Limitations

* More heterogeneous than GSE240008
* Weaker signal for initial pipeline validation

#### Role in Pipeline

```text
Human validation dataset for EBV biology
```

---

## Datasets to Avoid (for v1)

* Brain tissue epilepsy RNA-seq (high heterogeneity)
* Large cohorts (>20 samples)
* Single-condition datasets (no DE comparison)
* Processed-only datasets (no FASTQ)
* Extremely large FASTQ datasets (>20–30 GB per sample)

---

## SRA-Level Validation Checklist

Before committing to any dataset:

### 1. Read Layout

```bash
fasterq-dump --split-files
```

Ensure:

* paired-end reads

---

### 2. File Size

Preferred:

* 3–8 GB per FASTQ.gz

Avoid:

* > 20 GB per file (too slow for worker nodes)

---

### 3. Replicates

Minimum:

* 2 vs 2 (acceptable)
* 3 vs 3 (ideal)

---

## Recommended Execution Plan

### Step 1 — v1 Dataset

Use:

```text
GSE240008 (subset of 4–6 SRRs)
```

Goal:

* Validate pipeline
* Confirm QC → counts → DEG workflow
* Compare results with iDEP

---

### Step 2 — v2 Dataset

Add:

```text
GSE120854
```

Goal:

* Validate second dataset processing
* Ensure aggregation works correctly

---

### Step 3 — v3 Analysis

Perform:

```text
EBV vs mitochondrial comparison
```

Apply:

* DEG overlap
* pathway enrichment overlap
* gene_set_consensus enrichment
* network proximity analysis

---

## Summary

```text
Start simple → validate pipeline → scale biologically
```

* Use EBV dataset first (clean, strong signal)
* Add mitochondrial dataset second (biological relevance)
* Perform convergence analysis only after pipeline is stable

---

## Next Steps

1. Select exact SRR accessions for GSE240008
2. Verify FASTQ size and pairing
3. Build `manifest.yaml` for rnaseq_pipeline
4. Execute v1 pipeline on 1–2 samples
5. Expand to full subset once stable

---

# End of dataset_candidates.md

