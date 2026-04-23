# 🧬 dataset_candidates.md (RSP-Aligned)

## Note this document needs revision and is currently out of date

---

## Overview

This document defines candidate RNA-seq datasets for the `rnaseq_pipeline` (RSP), aligned to:

* the 5-layer scientific analysis ladder
* the v1 / v2 / v3 release strategy
* convergence-oriented experimental design

Datasets are not selected for individual merit alone, but for their ability to support:

```text
cross-dataset convergence analysis across orthogonal perturbations
```

---

## 🧠 Core Design Principle

```text
Within-dataset DEG → cross-dataset comparison at higher abstraction layers
```

---

## 🧬 Analytical Ladder Alignment

| Level   | Goal                | Dataset Requirement          |
| ------- | ------------------- | ---------------------------- |
| Level 1 | Gene overlap        | Clean internal DEG contrast  |
| Level 2 | Pathway overlap     | Strong perturbation signal   |
| Level 3 | Phenotype support   | Disease-relevant biology     |
| Level 4 | Module convergence  | Sufficient gene-level signal |
| Level 5 | Network convergence | Orthogonal perturbation axes |

---

## 🎯 Dataset Selection Criteria

### Tier 1 — Required (v1 execution)

* Bulk RNA-seq (NOT scRNA)
* ≥3 replicates per condition
* Same cell type within contrast
* Clear perturbation (genotype, infection, treatment)
* Minimal confounding variables
* FASTQ available (SRA)

---

### Tier 2 — Strongly Preferred

* Human or human-derived systems
* Genotype-driven perturbation
* Single dominant variable per contrast
* Clean case vs control framing

---

### Tier 3 — Convergence Readiness (v2/v3)

Datasets should map onto at least one axis:

* neuronal function
* mitochondrial biology
* immune / viral response
* mTOR / metabolic signaling

---

# 🧪 CURRENT EXECUTION SET (v1)

These datasets define your **first convergence experiment**.

---

## 🔴 Viral Axis (EBV)

### ✅ GSE240008 — EBV Lytic Reactivation

**Role**

```text
Pipeline validation + viral perturbation anchor
```

**Strengths**

* strong perturbation signal
* clean experimental design
* ideal for v1 pipeline testing

**Limitations**

* cell line context

---

## 🔵 Neuronal / Epilepsy Axis

### ✅ GSE222259 — SCN2A iPSC Neurons

**Role**

```text
Primary neuronal disease anchor
```

**Strengths**

* clean genotype contrast
* homogeneous cell type
* bulk-compatible
* strong relevance to epilepsy

---

### ✅ GSE107878 — KCNQ2 / SCN2A Neurons (subset)

**Role**

```text
Secondary epilepsy dataset (v1.5 / v2)
```

**Constraint**

```text
MUST subset to clean genotype contrast
```

---

## 🟢 mTOR / TSC Axis

### ✅ GSE264590 — TSC2 Isogenic NPCs

**Role**

```text
Primary pathway perturbation anchor
```

**Strengths**

* isogenic system
* clean genotype structure
* excellent replicates
* minimal confounding

---

### ✅ GSE239412 — TSC1 NPCs + Rapamycin

**Role**

```text
Mechanistic expansion dataset (v2)
```

**Strengths**

* separates intrinsic vs treatment effects
* strong pathway interpretability

---

## 🟡 Mitochondrial Axis

### ✅ GSE171537 — POLG Mutant Fibroblasts

**Role**

```text
Primary mitochondrial disease anchor
```

**Strengths**

* direct genotype-driven mitochondrial defect
* strong biological interpretability

**Limitation**

* non-neuronal system

---

### ✅ GSE120854 — Mitochondrial Disease Fibroblasts

**Role**

```text
Mitochondrial generalization dataset (v2)
```

**Strengths**

* human-derived
* broader mitochondrial dysfunction

---

# 🧠 CONVERGENCE STRUCTURE

Your current dataset set forms:

```text
GENOTYPE → TRANSCRIPTOME → PATHWAY → NETWORK
```

across:

* EBV (viral perturbation)
* SCN2A (neuronal disease)
* TSC2 (pathway dysregulation)
* POLG (mitochondrial dysfunction)

This is:

```text
orthogonal perturbation design for convergence testing
```

---

# 🧬 EXTENDED DATASET PANEL (v2 / v3)

These datasets should be used only after pipeline stabilization.

---

## 🧠 Epilepsy Expansion

### ⚠️ GSE256142 — Dravet Organoids

* biologically strong
* high variability (organoid system)

---

## 🟢 mTOR Expansion

### ⚠️ GSE137614 — TSC2 Differentiation Series

* requires strict subsetting

### ⚠️ GSE247367 — TSC2 Organoids

* developmental variability

---

## 🟡 Mito Expansion

### ⚠️ GSE184400 — iPSC Neuron Mito Dysfunction

* valuable neuronal bridge
* differentiation confounding

---

### ⚠️ GSE148602 — mtDNA depletion

* controlled but non-genotype-driven

---

# 🔴 EXCLUDED (v1 pipeline incompatible)

### ❌ scRNA-seq datasets

* GSE215362
* GSE245113

### ❌ mixed / unclear designs

* GSE286912
* GSE235862

---

# 🧠 CRITICAL RISKS

### 1. Cell-Type Mismatch

```text
NPC vs neuron vs fibroblast vs B cell
→ interpret only at pathway/module level
```

---

### 2. Cross-Dataset Batch Effects

```text
Cannot be corrected directly
→ use within-dataset DEG only
→ compare at higher abstraction layers
```

---

### 3. Effect Size Imbalance

```text
EBV >> mitochondrial > epilepsy
→ normalize interpretation, NOT raw values
```

---

# 🧠 VALIDATION STRATEGY

Each dataset must:

1. Reproduce expected biology:

   * mTOR (TSC2)
   * ion channel (SCN2A)
   * ETC (POLG)
   * immune/metabolic (EBV)

2. Produce stable DEG results

3. Generate interpretable pathway outputs

---

# 🚀 EXECUTION PLAN

### Phase 1 — Pipeline Validation

```text
GSE240008
```

---

### Phase 2 — First Convergence Triangle

```text
Epilepsy: GSE222259
mTOR:     GSE264590
Mito:     GSE171537
```

---

### Phase 3 — Expansion

```text
GSE239412
GSE107878
GSE120854
```

---

# 🧠 CORE HYPOTHESIS

```text
Do EBV infection, mitochondrial dysfunction,
and epilepsy-associated mutations converge
on shared vulnerable biological systems?
```

---

# 🏁 Bottom Line

You now have:

* 4 primary anchor datasets (v1)
* 3 expansion datasets (v2)
* clear exclusions
* a convergence-ready design

This is:

```text
publishable-quality experimental structure
```

---

# 👉 Next Step

```text
→ Build SRR shortlist
→ Verify pairing + replicates
→ Generate manifest.yaml
→ Execute first pipeline run on MARK
```

---

# End of dataset_candidates.md
