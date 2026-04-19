# dataset_candidates.md

## Overview

This document summarizes candidate RNA-seq datasets for the `rnaseq_pipeline` based on:
- pipeline design constraints
- biological relevance to mitochondrial dysfunction, EBV, and epilepsy
- feasibility for smoke testing and convergence analysis

---

## Dataset Selection Criteria

### Hard Constraints
- RNA-seq (not microarray)
- Raw data available (SRA preferred)
- Paired-end preferred
- Same cell type per comparison
- Human-derived preferred
- Subsettable for pilot runs

---


### Soft Constraints
- Published benchmark
- Strong biological signal
- Compatible with DEG + network analysis

---

## Strategic Framing

### Phase 1 — Pipeline Validation
- clean perturbation dataset
- minimal confounding

---


### Phase 2 — Disease-Relevant Datasets
- genotype-driven preferred

---


### Phase 3 — Convergence Analysis
Compare:
- EBV
- mitochondrial dysfunction
- epilepsy / mTOR

Using:
- DEG overlap
- pathway enrichment
- gene_set_consensus
- network proximity

---

# Core Dataset Anchors

## EBV Anchor

#### GSE240008 — EBV Lytic Reactivation

**Type:** Bulk RNA-seq (cell line)

**Role**

```text
Primary pipeline validation dataset
```

**Strengths**
    • strong signal 
    • clean perturbation 
    • ideal for iDEP benchmarking 
**Limitations**
    • cell line 

---

#### GSE161731 — Human EBV Infection
**Role**
Human validation layer
**Limitations**
    • heterogeneous 

---

## Mitochondrial Anchor
#### GSE120854 — Mitochondrial Disease Fibroblasts
**Type:** Bulk RNA-seq
**Strengths**
    • human 
    • clean fibroblast system 
**Limitations**
    • mixed etiologies 

---

#### GSE171537 — POLG Mutant Fibroblasts
**Strengths**
    • direct POLG relevance 
    • genotype-driven 
**Limitations**
    • non-neuronal 

---

#### GSE184400 — iPSC-derived Neurons (Mito Dysfunction)
**Strengths**
    • neuron-relevant 
**Limitations**
    • differentiation variability 

---

#### GSE148602 — mtDNA Depletion Model
**Strengths**
    • controlled perturbation 
**Limitations**
    • non-genotype driven 

---

#### GSE104827 — OXPHOS Deficiency
**Strengths**
    • strong metabolic signal 

---

## Epilepsy Dataset Candidates

### Bulk / Bulk-Compatible (First-Pass)

#### GSE222259 — SCN2A iPSC Neurons
    • clean genotype 
    • bulk RNA-seq 
    • minimal confounding 

---

#### GSE107878 — KCNQ2 / SCN2A Neurons
    • must subset epilepsy-relevant arms 
    • network-relevant 

---

#### GSE256142 — Dravet Organoids
    • strong biological relevance 
    • bulk RNA-seq 

---

### Second-Pass / High-Complexity

#### GSE215362 — ARX Organoids (scRNA-seq)
    • developmental epilepsy 
    • single-cell 

---

#### GSE245113 — Dravet GABAergic Neurons (scRNA-seq)
    • disease anchored 
    • inhibitory neuron biology 

---

## mTOR / GATOR1 / TSC Candidates

### First-Pass (Best)

##### GSE264590 — TSC2 Isogenic NPCs
    • best dataset overall 
    • clean genotype + replicates 

---

##### GSE239412 — TSC1 NPCs + Rapamycin
    • treatment vs intrinsic signal separation 

---

##### GSE137614 — TSC2 Differentiation Series
    • rich dataset 
    • requires subsetting 

---

### Second-Pass

##### GSE247367 — TSC2 Organoids
    • developmental relevance 

---

##### GSE235862 — TSC Neurovascular Model
    • niche but valuable 

---

##### GSE240337 — DEPDC5 KO Neurons
    • direct GATOR1 relevance 

---

##### GSE286912 — TSC Hyperactivity Model
    • mechanistic follow-up dataset 


---

## EBV Extended Dataset Panel

##### GSE103198 — Primary B Cell Infection
    • human primary system 

---

##### GSE125974 — Latent vs Lytic
    • state transition analysis 

---

##### GSE118145 — LCLs
    • chronic EBV reference 

---

##### GSE51546 — Time-Course Infection
    • dynamic response 

---

## SRA Validation Checklist
    • paired-end confirmation 
    • FASTQ size sanity check 
    • replicate count ≥2 (≥3 ideal) 
    • same cell type within contrast 
NOTE:
Mark removes size constraints, but design quality still dominates.

---

## Execution Plan
Step 1
GSE240008 → pipeline validation
Step 2
GSE222259 or GSE107878 → epilepsy anchor
Step 3
GSE264590 → mTOR anchor
Step 4
Integrate mito datasets
Step 5
Perform convergence analysis

---

## Core Hypothesis
Do EBV infection, mitochondrial dysfunction, and epilepsy mutations
converge on shared metabolic / neuronal / immune networks?

---

# End of dataset_candidates.md

---