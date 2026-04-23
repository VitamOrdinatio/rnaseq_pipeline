# dataset_triage.md

## Note this document needs revision and is currently out of date

# 🔬 Dataset Triage Framework (Aligned to RSP)

## Purpose

Select RNA-seq datasets that enable:

```text
Level 1–2 (v1): valid within-dataset DEG + pathway interpretation  
Level 3 (v2): phenotype-support overlap via GSC  
Level 4–5 (v3): module/network convergence across perturbations
```

---

## 🧠 Core Strategy

DO:
- Perform within-dataset DEG
- Compare across datasets at higher abstraction layers

DO NOT:
- Pool datasets for joint DEG modeling
- Compare raw DEG lists across studies

---

## 🧬 Analytical Ladder Mapping

```text
Layer	Goal	Dataset Requirement
Level 1	Gene overlap	Clean DEG contrast
Level 2	Pathway overlap	Strong perturbation signal
Level 3	Phenotype support	Relevant disease/biology context
Level 4	Module convergence	Sufficient gene-level signal
Level 5	Network convergence	Multiple orthogonal perturbations
```

---

## 🎯 Dataset Selection Criteria

### Tier 1 (Required for v1)

- Same cell type within dataset
- ≥3 replicates per condition
- Clear perturbation (KO, infection, mutation)
- Bulk RNA-seq
- Clean metadata

### Tier 2 (Strongly Preferred)

- Human or human-derived models
- Disease relevance (epilepsy, mitochondrial, viral)
- Minimal multi-factor confounding
- Single primary perturbation

### Tier 3 (Future-Proofing for v2/v3)

- Perturbations that map to:
  - mitochondrial biology
  - neuronal function
  - immune response

- Compatibility with GSC phenotype modeling

---

## 🧪 Current Execution Set (v1)

```text
Axis	Dataset	Role
EBV	GSE240008	Viral perturbation anchor
Epilepsy	GSE222259	Neuronal disease anchor
mTOR/TSC	GSE264590	Pathway perturbation anchor
POLG/Mito	GSE171537	Mitochondrial anchor
```

---

## ⚠️ Known Constraints

```text
Cell-type mismatch is unavoidable
→ enforce within-dataset DEG
→ cross-dataset comparison only at pathway/module/network level
```

---

## 🚀 Immediate Next Step

```text
→ SRR shortlist
→ manifest.yaml
→ v1 pipeline execution
```