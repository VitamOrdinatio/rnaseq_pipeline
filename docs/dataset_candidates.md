# dataset_candidates.md

## Overview

This document summarizes **candidate RNA-seq datasets** for the `rnaseq_pipeline` based on:

- pipeline design constraints
- data quality requirements
- biological relevance to mitochondrial dysfunction, EBV, and epilepsy
- feasibility for initial smoke testing and later network convergence analyses

This version expands the prior dataset plan by adding a **high-priority epilepsy section** and a **high-priority mTOR / GATOR1 / TSC section** for future convergence work.

---

## Dataset Selection Criteria

### Hard Constraints

- RNA-seq (not microarray)
- Public raw data available
- Prefer SRA-accessible FASTQ
- Prefer paired-end reads
- Clear experimental design
- Same tissue / cell type within each dataset
- Human-derived samples strongly preferred
- Small subset usable for smoke testing and early validation

---

### Soft Constraints

- Published study available for benchmarking
- Strong biological signal
- Compatible with downstream DEG / pathway / network analysis
- Relevant to:
  - mitochondrial dysfunction
  - EBV infection
  - epilepsy
  - mTOR / GATOR1 / TSC biology

---

## Recommended Strategic Framing

### Phase 1 — Pipeline Validation

Use a clean, strong-signal dataset to validate:
- FASTQ handling
- count generation
- DEG generation
- iDEP benchmarking

---

### Phase 2 — Disease-Relevant Bulk RNA-seq
Introduce genotype-informed or disease-relevant human datasets

### Phase 3 — Convergence Analysis

Compare:
- EBV perturbation
- mitochondrial dysfunction
- epilepsy / mTOR-pathway perturbation 
Using:
- DEG overlap
- pathway enrichment overlap
- gene_set_consensus enrichment
- network proximity / module convergence

---

## Core Non-Epilepsy Dataset Candidates

Disease GEOs from infectious agent (EBV) or mitochondrial diseases (e.g., POLG-related disorders)

#### 1. EBV Lytic Reactivation Dataset (Primary v1)

**Dataset:** GSE240008  
**Type:** RNA-seq (cell line, EBV reactivation)

##### Why this dataset

- strong transcriptional signal
- clean perturbation design
- multiple SRRs available
- well suited for pipeline validation
- suitable for iDEP benchmarking

##### Limitations
- cell line rather than primary human tissue
- ideal for infrastructure benchmarking, not final biological claims

##### Recommended Use

Select 4–6 samples:
- control
- Zta-induced
- Rta-induced

##### Role in Pipeline
```text
FASTQ → QC → counts → DEG → validate pipeline
```

---

#### 2. Mitochondrial Disease Dataset (v2)

**Dataset:** GSE120854
**Type:** RNA-seq (human fibroblasts)

##### Why this dataset

- human-derived samples 
- mitochondrial-disease relevance 
- cleaner than many tissue biopsy datasets 
- useful for MitoCarta-centered follow-up 

##### Limitations

- not POLG-specific 
- may include heterogeneous mitochondrial disorders 

##### Role in Pipeline

```text
Second disease-relevant dataset → validate cross-dataset processing
```

---

#### 3. Human EBV Infection Dataset (Optional / v2–v3)

**Dataset:** GSE161731
**Type:** RNA-seq (human EBV-related samples)

###### Why this dataset

- more clinically relevant than cell-line EBV models 
- useful as a human validation layer 

###### Limitations

- more heterogeneous than GSE240008 
- weaker choice for initial smoke testing 

###### Role in Pipeline

```text
Human validation dataset for EBV biology
```

---

## High-Priority Epilepsy Dataset Candidates

These are the strongest public GEO candidates for our long-term convergence goal, but they are not equally suitable for the same pipeline stage.

### A. iPSC / Organoid / Assembloid Epilepsy Candidates (5 GEOs)

#### 1. GSE256142 — Dravet syndrome brain organoids
Why it is strong
- human brain organoid system 
- Dravet syndrome / SCN1A context 
- bulk RNA-seq 
- polyA RNA and paired-end sequencing described 
- lower confounding than resected brain tissue 
Use case
- top-tier epilepsy candidate for bulk rnaseq_pipeline follow-up 

---

#### 2. GSE222259 — SCN2A heterozygous deletion in human iPSC-derived induced neurons
Why it is strong
- human iPSC-derived induced neurons 
- bulk RNA-seq 
- genetically defined perturbation 
- direct relevance to developmental epileptic encephalopathy 
Use case
- excellent genotype-anchored bulk RNA-seq dataset 

---

#### 3. GSE107878 — human iPSC-derived neuron panel including KCNQ2 and SCN2A
Why it is strong
- human iPSC → neuronal differentiation 
- neuronal RNA-seq profiles available 
- includes KCNQ2 and SCN2A perturbation arms 
- useful for convergence logic even though the full panel is broader than epilepsy 
Use case
- excellent comparison dataset if you subset only epilepsy-relevant lines 

---

#### 4. GSE215362 — ARX infantile-spasm organoid model
Why it is strong
- human cortical and ganglionic eminence organoids 
- directly models ARX poly-alanine expansion mutations 
- strong relevance to developmental epilepsy / infantile spasms 
Use case
- high-value developmental epilepsy dataset for later-stage convergence analysis 

---

#### 5. GSE245113 — Dravet patient iPSC-derived GABAergic neurons
Why it is strong
- human iPSC-derived GABAergic neurons 
- patient-derived SCN1A pathogenic variant 
- directly disease-anchored 
Important limitation
- this dataset is single-cell RNA-seq, not bulk RNA-seq 
Use case
- biologically excellent 
- not a first-pass bulk rnaseq_pipeline dataset 
- better as a second-pass validation / orthogonal reference 

---

### B. mTOR / GATOR1 / TSC Epilepsy-Relevant Dataset Candidates (5 GEOs)

#### 1. GSE264590 — patient TSC2 mutant cells with early neurodevelopmental aberrations
Why it is strong
- human 
- patient-derived / isogenic framing 
- TSC2 heterozygous and loss-of-function comparisons 
- 10 samples 
- directly relevant to mTOR-driven neurodevelopment 
Use case
- one of the best first mTOR/TSC candidates 

---

#### 2. GSE137614 — TSC2 knockout differentiation into neural progenitor and neural crest lineages
Why it is strong
- human pluripotent stem cell system 
- transcriptomic profiling during lineage differentiation 
- 96 samples 
- rich temporal design 
- bulk RNA-seq 
Use case
- high-value mechanistic mTOR/TSC resource 
- potentially too large for v1, but excellent for selective subsetting 

---

#### 3. GSE239412 — TSC1 patient-derived isogenic neural progenitor cells
Why it is strong
- human patient-derived isogenic NPCs 
- TSC1 loss, WT rescue/control, rapamycin arm 
- triplicate structure 
- highly informative for mTOR-dependent vs mTOR-independent programs 
Use case
- excellent convergence dataset for transcriptome + treatment response 

---

#### 4. GSE240337 — DEPDC5 KO human neurons
Why it is strong
- human neuronal model 
- directly relevant to DEPDC5 / GATOR1 / mTOR 
- explicitly positioned around transcriptomic signature and rapamycin response 
Use case
- one of the most directly relevant GATOR1 / epilepsy convergence datasets 

---

#### 5. GSE286912 — TSC neuronal hyperactivity / transcriptional changes
Why it is strong
- TSC-focused 
- mechanistically aligned with mTOR-pathway dysregulation 
- useful for later network-layer follow-up 
Use case
- follow-up dataset after bulk pipeline stabilization 

---

## Practical Ranking for rnaseq_pipeline

### Best bulk-ready epilepsy candidates
1. GSE256142 
2. GSE222259 
3. GSE107878 

---

### Best bulk-ready mTOR / TSC candidates
1. GSE264590 
2. GSE137614 
3. GSE239412 

---

### Biologically excellent but analytically second-pass
1. GSE245113 (single-cell) 
2. GSE215362 (organoid complexity) 
3. GSE286912 (later-stage mechanistic follow-up) 

---

## Datasets to Avoid for First-Pass Bulk Pipeline Work
- highly heterogeneous resected epileptic brain tissue 
- single-condition datasets 
- processed-only datasets with no raw access 
- extremely large cohorts before pipeline stabilization 
- single-cell datasets as first-pass pipeline targets

---

## SRA-Level Validation Checklist

Before committing to any dataset:

### 1. Read Layout

```bash
fasterq-dump --split-files
```

Ensure:
- paired-end reads

---

### 2. File Size

Preferred:
** 3–8 GB per FASTQ.gz

Avoid:

```text
> 20 GB per file (too slow for worker nodes)
```

Note: We probably can get rid of this since we have Mark, the HPC now.

---

### 3. Replicates

Minimum:
- 2 vs 2 (acceptable)
- 3 vs 3 (ideal)

---

## Updated Recommended Execution Plan

### Step 1 — v1 RNA-seq pipeline validation

Use:
- GSE240008 (subset)
Goal:
- validate infrastructure 
- benchmark against iDEP 
- confirm DEG workflow 

---

### Step 2 — first disease-relevant bulk epilepsy follow-up

Use one of:
- GSE256142
- GSE222259
- GSE107878

Goal:
- validate genotype-anchored epilepsy transcriptomics 

---

### Step 3 — first mTOR / TSC follow-up

Use one of:
- GSE264590
- GSE239412
- GSE137614

Goal:
- establish mTOR-centered disease arm for convergence testing 

---

### Step 4 — convergence analysis

Compare:
- EBV 
- mitochondrial dysfunction 
- epilepsy / mTOR datasets 

Apply:
- DEG overlap 
- pathway overlap 
- gene_set_consensus enrichment 
- network proximity / module convergence 

---

## Summary

Use GSE240008 to validate the RNA-seq pipeline.
Then anchor epilepsy with bulk-ready human iPSC/organoid datasets.
Then anchor mTOR biology with TSC/DEPDC5 datasets.
Only after pipeline stabilization should single-cell or higher-complexity epilepsy datasets be folded into the convergence framework.

---

## Next Steps

1. Select exact SRR accessions for GSE240008
2. Verify FASTQ size and pairing
3. Build `manifest.yaml` for rnaseq_pipeline
4. Execute v1 pipeline on 1–2 samples
5. Expand to full subset once stable

---

# End of dataset_candidates.md

