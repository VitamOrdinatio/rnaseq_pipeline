# GEO Dataset Unpacked – GSE240008

## Title for GSE240008

Viral reprogramming of host transcription initiation [RNA-seq]

---
## 📚 Citations

### Primary Study (GEO-generating paper)

```text
Ungerleider NA et al. 2024. Viral reprogramming of host transcription initiation. Nucleic Acids Res. PMID: 38471819
```

### Secondary / Downstream Studies

```text
Nguyen TD et al. 2025. Comprehensive resolution and classification of the Epstein Barr virus transcriptome. Nat Commun. PMID: 40640188
```

## 🧠 SAGE Assistance Prompt

Please ingest:
`rnaseq_pipeline/docs/dataset_survey/dataset_exploration_rules.md`

We will:
    1. discuss GEO 
    2. identify contrasts 
    3. subset GSM/SRR 
    4. THEN score (only when explicitly told) 

Do NOT score prematurely.

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

## 🔬 Phase 1 — GEO Understanding

### GEO Summary

    • Organism: Homo sapiens 
    • Assay: bulk RNA-seq 
    • Total samples: 56 
    • General design: EBV+ (Mutu, Akata) and EBV− (DG75) B-cell systems under reactivation or viral TF perturbation 

### Experimental Systems Present

    • Cell lines: 
        ◦ Mutu (EBV+) 
        ◦ Akata (EBV+) 
        ◦ DG75 (EBV−) 
    • Perturbations: 
        ◦ BCR crosslinking (anti-IgG / anti-IgM) 
        ◦ Zta expression 
        ◦ Rta expression 
    • Timepoints: 
        ◦ 24 h (core) 
        ◦ 6 h / 12 h / 24 h (timecourse arm) 
    • Library types: 
        ◦ polyA-selected, stranded (core arms) 
        ◦ unstranded (timecourse) 
        ◦ ribodepleted (DG75) 

### 📖 Literature-Informed Interpretation (SAGE Ingestion)

Key Insights from Papers

#### Ungerleider 2024

    • Defines all RNA-seq arms inside GSE240008 
    • Core arms: 
        ◦ Akata BCR (3 vs 3) 
        ◦ Mutu BCR (3 vs 3) 
        ◦ Mutu Zta (3 vs 3) 
    • Timecourse: Mutu Zta (6/12/24 h) 
    • DG75: Zta/Rta perturbation in EBV-negative system 

Biological conclusion:
    • Defines shared host transcriptional response to EBV lytic reactivation 

#### Nguyen 2025

    • Selects only: 
        ◦ Akata BCR 
        ◦ Mutu Zta 

    • Uses these as: 
        ◦ high-confidence reactivation models 
        ◦ transcriptome reconstruction anchors 

#### Important Methodological Notes

    • DG75: ribodepleted (not comparable to core arms) 
    • Timecourse: unstranded 
    • Core contrasts: stranded, polyA-selected 
    • GFP+ sorting enriches for reactivated cells 

### 🔍 Internal Contrast Enumeration

#### High-Priority Contrasts

    1. Mutu Zta 24 h 
    2. Akata BCR 24 h 
    3. Mutu BCR 24 h 

#### Secondary / Conditional Contrasts

    • DG75 Zta 
    • DG75 Rta 
    • Mutu Zta timecourse 

==========================================================
## 🔬 CONTRAST 1 — Mutu Zta 24h
==========================================================

### 🎯 Selected Contrast

Controls:
MC1, MC2, MC3, MC4
Cases:
MZ1, MZ2, MZ3, MZ5

### 🧠 Axis Assignment

Primary Axis: EBV_IMMUNE
Bridge Axis: none

Justification:
    • manipulated variable = Zta-driven EBV reactivation 

### 🔬 Phase 2 — Contrast Definition

#### Biological Definition

    • cell type: Mutu B-cell line 
    • perturbation: Zta expression 
    • timepoint: 24 h 
    • selection: GFP+ sorting 

#### Technical Definition

    • library: polyA RNA 
    • sequencing: DNBSEQ-T7 
    • stranded: yes 

### 🔗 Phase 3 — GSM → SRX → SRR Mapping

Control
```text
| GSM        | Sample | Treatment | SRX         | SRR         |
|------------|--------|-----------|-------------|-------------|
| GSM7679743 | MC1    | 24h Ctl   | SRX21243564 | SRR25513099 |
| GSM7679744 | MC2    | 24h Ctl   | SRX21243565 | SRR25513098 |
| GSM7679745 | MC3    | 24h Ctl   | SRX21243566 | SRR25513097 |
| GSM7679746 | MC4    | 24h Ctl   | SRX21243567 | SRR25513096 |
```

Case
```text
| GSM        | Sample | Treatment | SRX         | SRR         |
| ---------- | ------ | --------- | ----------- | ----------- |
| GSM7679747 | MZ1    | 24h Zta   | SRX21243568 | SRR25513095 |
| GSM7679748 | MZ2    | 24h Zta   | SRX21243569 | SRR25513094 |
| GSM7679749 | MZ3    | 24h Zta   | SRX21243570 | SRR25513093 |
| GSM7679750 | MZ5    | 24h Zta   | SRX21243571 | SRR25513092 |
```

### 🔎 Phase 4 — HIC

```text
Bulk RNA-seq = yes
≥3 replicates = yes
Same cell type = yes
Clear perturbation = yes
Clean metadata = yes
HIC = PASS
```

### 📊 Phase 5 — SCORING

CTA = 1.00
PTC = 1.00
ExS = 1.00
ReS = 0.90
SQES = 1.00
BAR = 1.00
BrV = 0.00

### 🧮 FSF

FSF = 89.00

==========================================================
## 🔬 CONTRAST 2 — Akata BCR 24h
==========================================================

### 🎯 Selected Contrast

Controls:
24hrUninduced-1, 2, 3

Cases:
24hrInduced-1, 2, 3

Axis:
Primary: EBV_IMMUNE
Bridge: none

### 🔗 Phase 3 — GSM → SRX → SRR Mapping

Control
```text
| GSM        | Sample             | Treatment      | SRX         | SRR         |
|------------|--------------------|----------------|-------------|-------------|
| GSM7679740 | 24hrUninduced-1    | 24h Ctl        | SRX21243561 | SRR25513102 |
| GSM7679741 | 24hrUninduced-2    | 24h Ctl        | SRX21243562 | SRR25513101 |
| GSM7679742 | 24hrUninduced-3    | 24h Ctl        | SRX21243563 | SRR25513100 |
```

Case
```text
| GSM        | Sample             | Treatment      | SRX         | SRR         |
|------------|--------------------|----------------|-------------|-------------|
| GSM7679737 | 24hrInduced-1      | 24h anti-IgG   | SRX21243558 | SRR25513105 |
| GSM7679738 | 24hrInduced-2      | 24h anti-IgG   | SRX21243559 | SRR25513104 |
| GSM7679739 | 24hrInduced-3      | 24h anti-IgG   | SRX21243560 | SRR25513103 |
```

### Scoring

HIC: PASS

Scores:
CTA = 1.00
PTC = 1.00
ExS = 1.00
ReS = 0.80
SQES = 1.00
BAR = 1.00
BrV = 0.00

FSF:
FSF = 88.00

==========================================================
## 🔬 CONTRAST 3 — Mutu BCR 24h
==========================================================

## 🎯 Selected Contrast

Controls:
MU1, MU2, MU3

Cases:
MI1, MI2, MI3

Axis:
Primary: EBV_IMMUNE
Bridge: none

### 🔗 Phase 3 — GSM → SRX → SRR Mapping

Control
```text
| GSM        | Sample | Treatment | SRX         | SRR         |
|------------|--------|-----------|-------------|-------------|
| GSM7679754 | MU1    | 24h Ctl   | SRX21243575 | SRR25513088 |
| GSM7679755 | MU2    | 24h Ctl   | SRX21243576 | SRR25513087 |
| GSM7679756 | MU3    | 24h Ctl   | SRX21243577 | SRR25513086 |
```

Case
```text
| GSM        | Sample | Treatment      | SRX         | SRR         |
|------------|--------|----------------|-------------|-------------|
| GSM7679751 | MI1    | 24h anti-IgM   | SRX21243572 | SRR25513091 |
| GSM7679752 | MI2    | 24h anti-IgM   | SRX21243573 | SRR25513090 |
| GSM7679753 | MI3    | 24h anti-IgM   | SRX21243574 | SRR25513089 |
```

### Scoring



HIC: PASS

Scores:
CTA = 1.00
PTC = 1.00
ExS = 1.00
ReS = 0.80
SQES = 0.95
BAR = 1.00
BrV = 0.00

FSF:
FSF = 87.25

## 📊 Cross-Contrast Comparison

```text
| Contrast      | FSF   | Notes                          |
|---------------|-------|--------------------------------|
| Mutu Zta      | 89.00 | strongest perturbation         |
| Akata BCR     | 88.00 | canonical model                |
| Mutu BCR      | 87.25 | slightly weaker signal         |
```

## 🔬 System-Level Interpretation

    • role in axis: EBV anchor layer 
    • redundancy: beneficial 
    • complementarity: 
        ◦ Zta = direct viral switch 
        ◦ BCR = physiological trigger 
    • convergence strength: 

shared signals across all three = high-confidence EBV biology

## 🧠 Notes / Anomalies

    • MZ4 missing → likely labeling discontinuity 
    • DG75 excluded (method mismatch) 

## 🏁 Final Thoughts

This GEO provides:
✔ 3 independent EBV perturbation models
✔ strong internal validation across studies
✔ ideal anchor layer for RSP

## 🚀 Recommendation

Retain all three contrasts as EBV anchor set.

---

# 🏁 Final assessment

This is now:
```text
✔ Template compliant
✔ Fully auditable
✔ Multi-contrast structured
✔ Ready for repo drop-in
✔ “DEX smile” level clean