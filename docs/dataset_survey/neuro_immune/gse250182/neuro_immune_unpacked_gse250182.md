# GEO Dataset Unpacked – GSE250182

## Title for GSE250182
Human iPSC-derived astrocytes with GBA knockout under inflammatory stimulation

---

## 📚 Citations

### Primary Study (GEO-generating)

`GSE250182 (GEO Series record)`

### Secondary / Downstream Studies

`None ingested for this scoring pass (by instruction)`


---

## 🧠 SAGE Assistance Prompt

Please ingest:

`rnaseq_pipeline/docs/dataset_survey/dataset_exploration_rules.md`

We will:
    1. discuss GEO 
    2. identify contrasts 
    3. subset GSM/SRR 
    4. THEN score (only when explicitly told) 

Do NOT score prematurely.


---

## 🧬 Terminology Hierarchy

```text
GSE = GEO Series
GSM = GEO Sample
SRX = SRA Experiment
SRR = SRA Run
Hierarchy:
GSE
 ├── GSM
       └── SRX
             └── SRR
```

Workflow:

`GSE → GSM selection → SRR mapping → contrast definition → scoring`


---

## 🔬 Phase 1 — GEO Understanding

### GEO Summary

    • Organism: Homo sapiens 
    • Assay: bulk RNA-seq 
    • Total samples: 9 
    • System: hPSC-derived astrocytes 
    • General design: genotype (WT vs GBA-KO) × inflammation (Sham vs ITC) 

### Experimental Systems Present

    • Cell lineage: astrocytes (CNS lineage) 
    • Perturbations: 
        ◦ **ITC** cocktail: IL-1 + TNFα + C1q 
    • Design: 2×2 matrix (WT/KO × Sham/ITC) 


### 🔍 Internal Contrast Enumeration

#### High-Priority (for bridge use)

    1. WT Sham vs WT ITC ← selected 

#### Secondary / Conditional

    • WT Sham vs KO Sham (genotype effect) 
    • WT ITC vs KO ITC (genotype under stress) 
    • Mixed contrasts (rejected for scoring) 

---


==========================================================
## 🔬 SELECTED CONTRAST — WT Sham vs WT ITC
==========================================================

### 🎯 Selected Contrast

Controls:
WT_sham_1, WT_sham_2, WT_sham_3

Cases:
WT_ITC_1, WT_ITC_2, WT_ITC_3

### 🧠 Axis Assignment

Primary Axis: NEURO_IMMUNE
Bridge Axis: none (acts as bridge dataset itself)

Justification:

    • manipulated variable = immune stimulation (ITC) 
    • biological system = human CNS-lineage astrocytes 
    • satisfies rule: perturbation defines primary axis 

### 🔬 Phase 2 — Contrast Definition

#### Biological Definition

    • cell type: human iPSC-derived astrocytes 
    • perturbation: inflammatory cytokine cocktail (ITC) 
    • comparison: Sham vs ITC 
    • genotype: WT only (controlled) 

#### Technical Definition

    • assay: bulk RNA-seq 
    • design: 3 vs 3 
    • system: in vitro differentiated human cells 


### 🔗 Phase 3 — GSM → SRX → SRR Mapping

Control Group (WT Sham)
```text
| GSM        | Sample    | Treatment | SRX         | SRR         |
| ---------- | --------- | --------- | ----------- | ----------- |
| GSM6259817 | WT_sham_1 | WT Sham   | SRX18101362 | SRR23906759 |
| GSM6259818 | WT_sham_2 | WT Sham   | SRX18101363 | SRR23906758 |
| GSM6259819 | WT_sham_3 | WT Sham   | SRX18101364 | SRR23906757 |

```
Case Group (WT ITC)
```text
| GSM        | Sample   | Treatment | SRX         | SRR         |
| ---------- | -------- | --------- | ----------- | ----------- |
| GSM6259820 | WT_ITC_1 | WT ITC    | SRX18101365 | SRR23906756 |
| GSM6259821 | WT_ITC_2 | WT ITC    | SRX18101366 | SRR23906755 |
| GSM6259822 | WT_ITC_3 | WT ITC    | SRX18101367 | SRR23906754 |
```

### 🔎 Phase 4 — HIC (Pre-Scoring Gate)

Bulk RNA-seq = YES
≥3 replicates = YES (3 vs 3)
Same cell type = YES
Clear perturbation = YES
Clean metadata = YES

HIC = PASS

### 📊 Phase 5 — SCORING

CTA = 1.00

    • identical cell population (WT astrocytes) 
    • no mixing or lineage ambiguity 

### PTC = 1.00

    • single perturbation (ITC vs Sham) 
    • no subsetting required 

### ExS = 1.00

    • simple 2-group comparison 
    • no timecourse, no genotype confounding 

### ReS = 0.80

case_n = 3
control_n = 3
min = 3 → lookup = 0.80
penalty = 0.00
final ReS = 0.80

### SQES = 0.95

base = 1.00 (strong immune perturbation)
adjustment = −0.05
final = 0.95

Justification:

    • strong inflammatory stimulus expected to produce broad transcriptional changes 
    • slight downgrade because: 
        ◦ cytokine cocktail is exogenous / artificial 
        ◦ not pathogen-driven or endogenous immune activation 

### BAR = 0.85

Justification:

    • strong bridge between: 
        ◦ immune perturbation 
        ◦ neural lineage system 
    • not a direct anchor (like EBV dataset) 
    • fits “strong bridge” class 

### BrV = 1.00

Justification:

    • uniquely fills gap between: 
        ◦ immune axis (EBV) 
        ◦ neural system 
    • removing it would leave no clean bridge 
    • no equivalent dataset currently selected 

## 🧮 FSF Calculation

FSF_raw =
  0.25(1.00) +
  0.15(1.00) +
  0.10(1.00) +
  0.10(0.80) +
  0.15(0.95) +
  0.15(0.85) +
  0.10(1.00)
= 0.25 + 0.15 + 0.10 + 0.08 + 0.1425 + 0.1275 + 0.10

= 0.95

FSF = 95.00 / 100

## 🏁 Final Output

GEO contrast: GSE250182 / WT astrocytes / Sham vs ITC
Primary Axis: NEURO_IMMUNE
Bridge Axis: none (functional bridge dataset)

HIC: PASS
CTA: 1.00
PTC: 1.00
ExS: 1.00
ReS: 0.80
SQES: 0.95
BAR: 0.85
BrV: 1.00
FSF: 95.00 / 100

Interpretation: critical bridge dataset

## 🧠 Notes / Limitations

    • **astrocytes** ≠ neurons 
    • in vitro differentiation system 
    • cytokine cocktail is artificial stimulus 
    • still acceptable as first bridge layer 

## 🔬 System-Level Interpretation

Role:
`EBV (immune) → neuro-immune bridge → neuronal systems`

Strength:

    • provides controlled entry into CNS context 
    • high signal, low confounding 

## 🚀 Recommendation

KEEP as primary NEURO_IMMUNE bridge dataset
until/unless replaced by:
`clean neuronal + immune perturbation GEO`
