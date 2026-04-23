# GEO Dataset Unpacked – GSE173955

## Title for GSE173955

Integrative analysis of altered genes expressed in human Alzheimer’s disease brain (RNA-Seq)

---

## 📚 Citations

### Primary Study (GEO-generating paper)

```text
Mizuno Y, Abolhassani N, Mazzei G, Sakumi K, Saito T, Saido TC, Ninomiya T, Iwaki T, Yamasaki R, Kira JI, Nakabeppu Y. MUTYH Actively Contributes to Microglial Activation and Impaired Neurogenesis in the Pathogenesis of Alzheimer’s Disease. Oxidative Medicine and Cellular Longevity. 2021.
```

### Dataset record

`GSE173955 – Integrative analysis of altered genes expressed in human Alzheimer’s disease brain (RNA-Seq)`


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
    • Total samples: 18 
    • General design: postmortem human hippocampus from Alzheimer’s disease (AD) and non-AD subjects 

### Experimental Systems Present

    • Tissue: postmortem human hippocampus 
    • Biological groups: 
        ◦ Alzheimer’s disease 
        ◦ non-Alzheimer’s disease 
    • Platform: 
        ◦ Illumina HiSeq 1500 
    • Library type: 
        ◦ stranded mRNA-seq / polyA(+) RNA workflow 
    • Technical note: 
        ◦ one AD and one non-AD sample were sequenced twice to increase read depth 


---

## 📖 Literature-Informed Interpretation (SAGE Ingestion)

### Key Insights from Papers

#### Study 1 — Mizuno 2021

    • Key experimental design elements: 
        ◦ 10 non-AD and 8 AD hippocampal RNA samples 
        ◦ RNA-seq used for integrative molecular pathology analysis 
        ◦ same human hippocampal source context as the broader AD pathology study 
    • Which arm was emphasized: 
        ◦ whole-cohort AD vs non-AD hippocampus 
    • Biological conclusions: 
        ◦ altered hippocampal gene expression in AD 
        ◦ links to microglial activation, oxidative stress biology, and impaired neurogenesis 

#### Important Methodological Notes

    • This is postmortem bulk hippocampal tissue, not a defined neuronal cell model 
    • This is not an immune perturbation experiment 
    • This is not a bridge dataset in the same sense as cytokine-treated astrocytes or IFN-treated neurons 
    • Tissue complexity will matter later for CTA and ExS if scored 


---

## 🔍 Internal Contrast Enumeration

### High-Priority Contrasts

    1. AD vs non-AD whole cohort 

### Secondary / Conditional Contrasts

    • sex-specific contrasts (only if metadata completeness supports it) 
    • technical-depth aware reprocessing of the duplicated-sequencing subjects 
    • age-restricted subgroup contrasts (only if metadata completeness supports it) 

### Contrast assessment note

At present, only one clearly supported biological contrast is obvious from the GEO and paper:

`AD hippocampus vs non-AD hippocampus`

This GEO does not appear to contain multiple mechanistically distinct internal perturbation arms like GSE240008.


---

## 🎯 Selected Contrast for Analysis

Contrast Name: AD vs non-AD postmortem human hippocampus

Controls:
- non-Alzheimer's Disease_Biological replicate 1–10

Cases:
- Alzheimer's Disease_Biological replicate 1–8


---

## 🧠 Axis Assignment (pre-scoring)

Primary Axis: not yet assigned for scoring
Bridge Axis: none

Justification:
    • This GEO is a human disease tissue cohort, not a clean mechanistic perturbation model 
    • It may later fit better as a human validation / disease-context comparator 
    • It is not a strong candidate for NEURO_IMMUNE bridging 


---

## 🔬 Phase 2 — Contrast Definition

### Biological Definition

    • tissue: postmortem human hippocampus 
    • perturbation/state: Alzheimer’s disease vs non-AD 
    • timepoint: terminal human tissue, not experimental timecourse 
    • selection method: none; cohort-based tissue comparison 

### Technical Definition

    • library prep: polyA(+) RNA / stranded mRNA-seq workflow 
    • sequencing platform: Illumina HiSeq 1500 
    • technical note: one AD and one non-AD subject were sequenced twice for more reads 


---

## 🔗 Phase 3 — GSM → SRX → SRR Mapping

Case Group (AD)
```text
| GSM        | Sample                                     | Treatment      | SRX         | SRR                         |
| ---------- | ------------------------------------------ | -------------- | ----------- | --------------------------- |
| GSM5283449 | Alzheimer's Disease_Biological replicate 1 | AD hippocampus | SRX10787707 | 2 runs present under SRX    |
| GSM5283450 | Alzheimer's Disease_Biological replicate 2 | AD hippocampus | SRX10787708 | pending run-level expansion |
| GSM5283451 | Alzheimer's Disease_Biological replicate 3 | AD hippocampus | SRX10787709 | pending run-level expansion |
| GSM5283452 | Alzheimer's Disease_Biological replicate 4 | AD hippocampus | SRX10787710 | pending run-level expansion |
| GSM5283453 | Alzheimer's Disease_Biological replicate 5 | AD hippocampus | SRX10787711 | pending run-level expansion |
| GSM5283454 | Alzheimer's Disease_Biological replicate 6 | AD hippocampus | SRX10787712 | pending run-level expansion |
| GSM5283455 | Alzheimer's Disease_Biological replicate 7 | AD hippocampus | SRX10787713 | pending run-level expansion |
| GSM5283456 | Alzheimer's Disease_Biological replicate 8 | AD hippocampus | SRX10787714 | pending run-level expansion |
```

Control Group (non-AD)
```text
| GSM        | Sample                                          | Treatment          | SRX         | SRR                         |
| ---------- | ----------------------------------------------- | ------------------ | ----------- | --------------------------- |
| GSM5283457 | non-Alzheimer's Disease_Biological replicate 1  | non-AD hippocampus | SRX10787715 | pending run-level expansion |
| GSM5283458 | non-Alzheimer's Disease_Biological replicate 2  | non-AD hippocampus | SRX10787716 | pending run-level expansion |
| GSM5283459 | non-Alzheimer's Disease_Biological replicate 3  | non-AD hippocampus | SRX10787717 | pending run-level expansion |
| GSM5283460 | non-Alzheimer's Disease_Biological replicate 4  | non-AD hippocampus | SRX10787718 | pending run-level expansion |
| GSM5283461 | non-Alzheimer's Disease_Biological replicate 5  | non-AD hippocampus | SRX10787719 | pending run-level expansion |
| GSM5283462 | non-Alzheimer's Disease_Biological replicate 6  | non-AD hippocampus | SRX10787720 | pending run-level expansion |
| GSM5283463 | non-Alzheimer's Disease_Biological replicate 7  | non-AD hippocampus | SRX10787721 | pending run-level expansion |
| GSM5283464 | non-Alzheimer's Disease_Biological replicate 8  | non-AD hippocampus | SRX10787722 | pending run-level expansion |
| GSM5283465 | non-Alzheimer's Disease_Biological replicate 9  | non-AD hippocampus | SRX10787723 | pending run-level expansion |
| GSM5283466 | non-Alzheimer's Disease_Biological replicate 10 | non-AD hippocampus | SRX10787724 | pending run-level expansion |

```

###  Run-level note
One AD sample page explicitly shows 2 runs under SRX10787707. The GEO record and paper also state that one AD and one non-AD sample were sequenced twice to obtain more reads. The full SRR expansion can be done in a run-selector pass if you want implementation-ready download tables. 

---

## 🔎 Phase 4 — HIC Check (Pre-Scoring Gate)

Not scored yet.

Preliminary observations:
    • Bulk RNA-seq = yes 
    • ≥3 replicates/condition = yes 
    • Same tissue class = yes 
    • Clear disease-state contrast = yes 
    • Clean metadata = mostly yes 

Important caveat:
    • This is bulk postmortem hippocampus, so CTA and ExS will not look like cell-line perturbation datasets 

---

## 🧠 Notes / Anomalies

    • one AD and one non-AD subject were sequenced twice 
    • tissue is heterogeneous bulk hippocampus 
    • this GEO lacks multiple mechanistically distinct internal arms 
    • likely stronger as a human disease-context comparator than as a bridge dataset 

---

## 📊 Cross-Contrast Comparison (optional)

At present, no strong multi-arm internal contrast structure is evident beyond:
    • AD vs non-AD whole cohort 

---

## 🔬 System-Level Interpretation
    • role in axis: 
        ◦ not a clean mechanistic bridge 
        ◦ potentially a human disease-context validation layer 
    • redundancy: 
        ◦ low redundancy with perturbation datasets 
    • complementarity: 
        ◦ could complement EBV, bridge, or mitochondrial datasets later by providing human hippocampal disease relevance 
    • expected contribution to convergence: 
        ◦ better for downstream validation than for early bridge construction 

---

## 🏁 Final Thoughts

This GEO is biologically valuable, but for your current RSP architecture:

GSE173955 is not a Tier-1 neuro-immune bridge candidate.

It is a human postmortem hippocampal AD cohort dataset.

Its strongest future role is likely:

`human disease-context validation / comparator`

rather than:

`primary bridge-layer construction`

# End of File

---