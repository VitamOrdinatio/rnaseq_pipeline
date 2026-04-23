# 📄 gse_dataset_unpacked_template.md

This GSE scoring template lives here:
`rnaseq_pipeline/docs/dataset_survey/templates/`

Have SAGE use this template in conjunction with the scoring rubric which lives here:
`rnaseq_pipeline/docs/dataset_survey/dataset_exploration_rules.md`

## Template Usage below:

---

# GEO Dataset Unpacked – <GSE_ID>

## Title for <GSE_ID>

<Insert GEO title>

---

## 📚 Citations

### Primary Study (GEO-generating paper)

```text
<Author et al. Year. Title. Journal. PMID>
```

### Secondary / Downstream Studies (optional)

```text
<Author et al. Year. Title. Journal. PMID>
```


---

## 🧠 SAGE Assistance Prompt

Please ingest:

`rnaseq_pipeline/docs/dataset_survey/dataset_exploration_rules.md`

We will:
1) discuss GEO
2) identify contrasts
3) subset GSM/SRR
4) THEN score (only when explicitly told)

Do NOT score prematurely.


---

## 🧬 Terminology Hierarchy

GSE = GEO Series
GSM = GEO Sample
SRX = SRA Experiment
SRR = SRA Run

```text
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

    • Organism: 
    • Assay: 
    • Total samples: 
    • General design: 

### Experimental Systems Present

List all major arms:

    • Cell lines / tissues: 
    • Perturbations: 
    • Timepoints: 
    • Library types: 

### 📖 Literature-Informed Interpretation (SAGE Ingestion)

#### Key Insights from Papers

##### Study 1

    • Key experimental design elements: 
    • Which arms were emphasized: 
    • Biological conclusions: 

##### Study 2 (if applicable)

    • Same structure 

##### Important Methodological Notes

    • stranded vs unstranded: 
    • polyA vs ribodepletion: 
    • sorting / enrichment: 
    • any known caveats: 


---

### 🔍 Internal Contrast Enumeration

#### High-Priority Contrasts

    1. <contrast name> 
    2. <contrast name> 

#### Secondary / Conditional Contrasts

    • <contrast> 
    • <contrast> 


---

### 🎯 Selected Contrast for Analysis

```text
Contrast Name: <e.g., Mutu Zta 24h>

Controls:
- GSMXXXX
- GSMXXXX

Cases:
- GSMXXXX
- GSMXXXX
```


---

### 🧠 Axis Assignment (pre-scoring)

```text
Primary Axis:

Bridge Axis:

Justification:
    • manipulated variable: 
    • biological interpretation: 
```

---

## 🔬 Phase 2 — Contrast Definition

### Biological Definition

    • cell type: 
    • perturbation: 
    • timepoint: 
    • selection method (if any): 

### Technical Definition
    • library prep: 
    • sequencing platform: 
    • strandedness: 


---

## 🔗 Phase 3 — GSM → SRX → SRR Mapping

### Control Group

```text
| GSM | Sample | Treatment | SRX | SRR |
|-----|--------|-----------|-----|-----|
```

### Case Group

```text
| GSM | Sample | Treatment | SRX | SRR |
|-----|--------|-----------|-----|-----|
```


---

## 🔎 Phase 4 — HIC Check (Pre-Scoring Gate)

```text
Bulk RNA-seq = 
≥3 replicates/condition = 
Same cell type = 
Clear perturbation = 
Clean metadata = 
Final:
HIC = PASS / FAIL
```


---

## 📊 Phase 5 — SCORING (ONLY AFTER EXPLICIT "score")

### Feature Scoring

#### CTA (Cell Type Alignment) =

- calculation basis:

- justification: 

#### PTC (Perturbation Type Clarity) =

- calculation basis:

- justification: 

#### ExS (Experimental Simplicity) =

- calculation basis:

- justification: 

#### ReS (Replicate Strength) =

- calculation basis:

calculation =

- experiment parameters:
  1. case_n =
  2. control_n =

- justification: 


#### SQES (Signal Quality / Effect Size) =

- calculation basis:

base =
adjustment =
final =

- justification: 

#### BAR (Biological Axis Relevance) =

- calculation basis:

- justification: 

#### BrV (Bridge Value) =

- calculation basis:

- justification: 

---

## 🧮 FSF (Final Score Formula) Calculation

```text
FSF_raw =
  0.25(CTA) +
  0.15(PTC) +
  0.10(ExS) +
  0.10(ReS) +
  0.15(SQES) +
  0.15(BAR) +
  0.10(BrV)
FSF = 100 * FSF_raw
```


---

## 🏁 Final Output

```text
GEO contrast:
Primary Axis:
Bridge Axis:
HIC:
CTA:
PTC:
ExS:
ReS:
SQES:
BAR:
BrV:
FSF:

Interpretation:
```


---

## 🧠 Notes / Anomalies

• naming inconsistencies: 
• missing samples: 
• QC concerns: 


---

## 📊 Cross-Contrast Comparison (optional)

If there are different internal contrasts with the GEO, generate a comparison table below:

```text
| Contrast | FSF | Notes |
```


---

## 🔬 System-Level Interpretation

    • role in axis: 
    • redundancy: 
    • complementarity: 
    • expected contribution to convergence: 

---


---

## 🏁 Final Thoughts

This template preserves:

```text
✔ deterministic workflow
✔ literature grounding
✔ contrast-first logic
✔ full audit trail
✔ reproducible scoring
```


---

## 🚀 Recommendation

Use this pattern:

- one GSE = one MD file
- multiple contrasts inside it

That way:
- GSE file = exploration + justification
- RSP pipeline = execution


# End of Template

---

