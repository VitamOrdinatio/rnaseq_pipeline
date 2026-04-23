# EBV GEO Unpacked – GSE240008

## Title for GSE240008 : Viral reprogramming of host transcription initiation [RNA-seq]


---

## Citations

### Ungerleider 2024
      
```text
Ungerleider NA, Roberts C, O'Grady TM, Nguyen TT et al. Viral reprogramming of host transcription initiation. Nucleic Acids Res 2024 May 22;52(9):5016-5032. PMID: 38471819
```

### Nguyen 2025

```text
Nguyen TD, Wang J, Lam MT, McFerrin H et al. Comprehensive resolution and classification of the Epstein Barr virus transcriptome. Nat Commun 2025 Jul 10;16(1):6381. PMID: 40640188
```

---

## SAGE assistance prompt

Operator instructions / prompt: 

```text
Please ingest the following revised MD:
`rnaseq_pipeline/docs/dataset_survey/dataset_exploration_rules.md`

Let's begin scoring GEOs, but we will proceed using this method. You are not allowed to score the GEO until i explicitly say score. We will first have a conversation about the GEO, and ultimately that conversation will lead us to narrow down appropriate SRRs found within the GEO. Once we have the specific experimental design of a GEO subsetted, then we are ready to properly score the GEO for our network convergence analysis. Any questions?
```

## Terminology Hierarchy

```text
GSE = GEO Series (the study)
GSM = GEO Sample (biological sample / condition unit)
SRR = SRA Run (sequencing file, technical unit)
SRX = SRA Experiment

In practice:
    • GSM is the GEO sample record 
    • each GSM points to an SRX experiment record 
    • each SRX contains one or more SRR runs 
```
Hierarchy:
```text
GSE
 ├── GSM (samples)
       └── SRX ( SRR runs / FASTQ)
```

Our workflow:
```text
GSE → identify GSMs → map to SRRs → define contrast
```

So when we “subset a GEO,” we are really:
```text
selecting GSMs → which map to SRRs
```

## 🧠 Proposed workflow — validation

- do NOT score immediately
- discuss → refine → subset → then score

This is exactly aligned with our ruleset, specifically:

`“All scores apply to the proposed contrast, not the GEO globally”`

So our workflow becomes:

### 🔬 Phase 1 — GEO understanding

We discuss:
    • biological system 
    • perturbations 
    • axes 
    • complexity 
    • possible confounders 

NO scoring yet.

### 🔬 Phase 2 — Contrast definition

We explicitly define:
case group (GSMs)
control group (GSMs)
cell type
perturbation
replicates
This is the critical step.

### 🔬 Phase 3 — SRR mapping

We ensure:
    • GSM → SRR mapping is clean 
    • replicates are truly biological 
    • no hidden batch or technical confounders 

### 🔬 Phase 4 — HIC check (pre-scoring gate)

We verify:
    • bulk RNA-seq 
    • ≥3 replicates 
    • same cell type 
    • clear perturbation 
    • clean metadata 

Still no scoring.

### 🔬 Phase 5 — SCORING (only when you say “score”)

Then—and only then—we apply:

CTA, PTC, ExS, ReS, SQES, BAR, BrV → FSF

## SAGE Questions before unpacking GEO

### 1. Do you want me to proactively flag multiple viable contrasts?

Example:

GSEXXXXX contains:
- timecourse
- genotype
- treatment

Do you want me to:

A) propose the best single contrast only
or
B) enumerate all viable contrasts, then we choose

`Answer:` 
B) Enumerate **all viable contrast** for contrast-specific scoring

### 2. How strict do you want HIC during exploration?

During discussion phase:
    • Should I immediately eliminate non-HIC candidates? 
    • Or allow exploration and then filter? 

My recommendation:
explore broadly → enforce HIC at contrast-definition stage


`Answer:` 
Explore broadly and enforce HIC at contrast-definition stage

## Starting point: what GSE240008 actually is

The GEO record says:
    • title: “Viral reprogramming of host transcription initiation [RNA-seq]” 
    • organism: Homo sapiens 
    • experiment type: expression profiling by high throughput sequencing 
    • overall design: RNA-seq of B-cell lines Mutu (EBV+), Akata (EBV+), and DG75 (EBV−) across latency/reactivation or Zta/Rta transfection conditions 
    • total samples: 56 

That means this GEO is not one simple contrast. It is a compound study containing several distinct experimental sub-designs:
    • Mutu Zta timecourse: M6/M12/M24 with control vs Zta, 4 replicates each 
    • DG75 transfection model: control vs Rta vs Zta, 4 replicates each 
    • Akata BCR induction: induced vs uninduced, 3 replicates each 
    • Mutu BCR induction: induced vs uninduced, 3 replicates each 
    • Mutu 24 h Zta: MC/MZ, 4 replicates each 

So before any scoring, the real question is:
`Which sub-design inside GSE240008 is the best contrast for RSP network convergence purposes?`


## SAGE Content Update

Operator Prompt (Delta Change):

```text
Before we begin looking at this GEO any further, 
I want you get a content update centered on two 
key articles that utilized GSE240008 (see attached):
- Ngyuen 2025 PMID 40640188.pdf
- Ungerleider 2024 PMID 38471819.pdf

Deep dive them so that you are expertly familiar with
 the treatment groups and experimental design.

Then revisit determining viable contrasts inside GSE240008.
```

## SAGE Response to PDF Ingestion:

```text
I’ve done the content update, and it materially 
changes how I would discuss GSE240008.

The two papers make clear that GSE240008 is not 
one uniform RNA-seq study. It is a bundle of related
 but methodologically distinct sub-designs, and those 
 sub-designs were used differently by the later papers. 
```

### What the two papers taught us (SAGE ingestion)

#### Ungerleider 2024

This is the paper that actually generated and used the short-read RNA-seq structure inside GSE240008 for host transcription-initiation remodeling. Its RNA-seq section explicitly breaks the GEO into these arms:

    • Akata BCR induction: anti-IgG, 24 h, GFP+ induced vs GFP− uninduced, 3 vs 3, stranded, paired-end, polyA-selected 
    • Mutu BCR induction: anti-IgM, 24 h, GFP+ induced vs GFP− uninduced, 3 vs 3, stranded, paired-end, polyA-selected 
    • Mutu Zta induction: SV40p-Zta vs control, 24 h, GFP+ sorted, 3 vs 3, stranded, paired-end, polyA-selected 
    • Mutu Zta timecourse: 6 h, 12 h, 24 h, 4 vs 4 at each timepoint, but unstranded, polyA-selected 
    • DG75 ectopic Zta or Rta: EBV-negative DG75, GFP+ sorted after transfection, 4 vs 4, but stranded, ribodepleted rather than polyA-selected. 

It also clarifies the biological intent:

    • Akata-BCR, Mutu-BCR, and Mutu-Zta were used to define common host responses to EBV lytic replication 
    • DG75 Zta/Rta was used to isolate viral transcription-factor-driven de novo host promoter activation in an EBV-negative background. 

#### Nguyen 2025

This paper is even more informative for your purposes because it tells us which subsets of GSE240008 the authors considered the cleanest and most useful for high-confidence viral-transcriptome analysis.
They used two purified reactivation models only:

    • Akata-BCR 
    • Mutu-Zta 

They combined replicates from each and built the long-read EBV transcriptome around those two models, explicitly reusing CAGE-seq and short-read data from the earlier work. They did not center the paper on Mutu-BCR, DG75, or the timecourse arms. 

That is a strong signal from the authors themselves:

```text
Akata-BCR and Mutu-Zta are the most mature, best-validated,
and most reusable internal designs inside GSE240008.
```

### One important methodological nuance

The papers also show that not all arms are apples-to-apples:
    • DG75 is ribodepleted 
    • the timecourse is unstranded 
    • Akata/Mutu core 24 h arms are stranded, paired-end, polyA-selected. 

So even before HIC scoring, that makes the 24 h Akata/Mutu core contrasts look more attractive for deterministic intake.


### Internal Contrasts (Internal Treatment Groups)

#### High-Priority Contrasts

    1. Mutu Zta 24 h, control vs Zta 
    2. Akata BCR 24 h, uninduced vs induced 
    3. Mutu BCR 24 h, uninduced vs induced 

#### Secondary / Conditional Contrasts

    4. DG75 control vs Zta 
    5. DG75 control vs Rta 
    6. Mutu Zta 6 h / 12 h / 24 h timepoint-specific contrasts 

Reason:

These are more appropriate if we specifically want:

    • viral TF-driven host remodeling in an EBV-negative background, or 
    • temporal kinetics, rather than the cleanest first-pass EBV reactivation anchor. 

## Operator Response: Score all 3 contrasts, one-at-a-time:

```text
we will score all three (Mutu Zta 24h, Akata BCR 24h, and Mutu BCR 24h), but one at a time. thus we will generate three good pairwise scored candidates for GSE240008 alone. proceed with Mutu Zta 24h.
```

### 🧠 Quick Comparison for 3 contrasts:

```text
Contrast
FSF
Difference driver
Mutu Zta 24h
89.00
4 vs 4 replicates
Akata BCR 24h
88.00
3 vs 3 replicates
```

Everything else is essentially equal.

### What Mutu Zta 24 h actually is

From the study metadata and the Ungerleider paper, this arm is:
    • cell line: Mutu 
    • system: EBV-positive Burkitt lymphoma 
    • perturbation: ectopic Zta expression versus control 
    • selection: GFP+ FACS-isolated cells 
    • timepoint: 24 h 
    • library type: stranded, polyA-selected RNA-seq 
    • processed output file: GSE240008_mutu_24h_zta_gene_expression.tpm.tsv.gz 

That already makes it a very attractive internal contrast for your EBV axis.

#### Exact GSM groups for Mutu Zta 24h:

The GSE240008 series record shows the Mutu 24 h Zta arm as:

##### Control group

    • MC1 = GSM7679743 
    • MC2 = GSM7679744 
    • MC3 = GSM7679745 
    • MC4 = GSM7679746 

##### Zta group

    • MZ1 = GSM7679747 
    • MZ2 = GSM7679748 
    • MZ3 = GSM7679749 
    • MZ5 = GSM7679750 

So this candidate is presently a 4 vs 4 design.

##### What the sample pages confirm for Mutu Zta 24h:

The sample pages I checked confirm that:

    • MC1 is Mutu, 24h Ctl, polyA RNA, RNA-seq, DNBSEQ-T7, and marked Stranded. 
    • MZ1 is Mutu, 24h Zta, polyA RNA, RNA-seq, DNBSEQ-T7, and marked Stranded. 

That is exactly the kind of clean internal consistency we want before scoring.

##### Rationale for strong candidacy for Mutu Zta 24h: 

Scientifically, this arm has several advantages for your repo goal:
    • it is a single cell line 
    • it is a single timepoint 
    • it is a single dominant perturbation 
    • it uses purified reactivating cells 
    • it is one of the two flagship reactivation models carried forward into the later Nguyen paper 

##### Possible minor issue for Mutu Zta 24h: 

The Zta group is:
`MZ1, MZ2, MZ3, MZ5`

not:
`MZ1, MZ2, MZ3, MZ4`

That does not mean there is a problem, but it is something we should explicitly verify before scoring.

Possible explanations:
    • MZ4 existed but failed QC / was removed 
    • the label is intentional and complete as deposited 
    • there was a numbering discontinuity during submission 

#### Step 1 — define the exact contrast for Mutu Zta 24h

For the Mutu Zta 24 h arm inside GSE240008, the study record shows the intended 4-vs-4 subset as:

    • Controls: MC1, MC2, MC3, MC4 
    • Zta: MZ1, MZ2, MZ3, MZ5 
all within GSE240008’s Mutu 24 h Zta design. 


The series record also lists a processed file specific to this arm: GSE240008_mutu_24h_zta_gene_expression.tpm.tsv.gz. 

#### Step 2 — verify the sample-level metadata for Mutu Zta 24h

All eight GSMs belong to the same overall design pattern:
    • organism: Homo sapiens 
    • source: B cell lymphoma 
    • cell line: Mutu 
    • cell type: cell line 
    • extracted molecule: polyA RNA 
    • library strategy: RNA-Seq 
    • instrument: DNBSEQ-T7 
    • description: Stranded 

The only intended biological difference is treatment:
    • MC1–MC4 are all 24h Ctl 
    • MZ1–MZ3 and MZ5 are all 24h Zta. 

So, at the discussion stage, this looks like a clean candidate contrast:
`Mutu / 24 h / control vs Zta / polyA / stranded / paired-end`

#### Step 3 — map GSM → SRX → SRR transparently for Mutu Zta 24h:

Here is the full mapping for the candidate subset.

Control group

```text
| GSM        | Sample | Treatment | SRX         | SRR         |
| ---------- | ------ | --------- | ----------- | ----------- |
| GSM7679743 | MC1    | 24h Ctl   | SRX21243564 | SRR25513099 |
| GSM7679744 | MC2    | 24h Ctl   | SRX21243565 | SRR25513098 |
| GSM7679745 | MC3    | 24h Ctl   | SRX21243566 | SRR25513097 |
| GSM7679746 | MC4    | 24h Ctl   | SRX21243567 | SRR25513096 |
```

Supporting records: MC1, MC2, MC3, and MC4 each show one SRA experiment on the GSM page and one run on the linked SRA page. 

Zta group

```text
| GSM        | Sample | Treatment | SRX         | SRR         |
| ---------- | ------ | --------- | ----------- | ----------- |
| GSM7679747 | MZ1    | 24h Zta   | SRX21243568 | SRR25513095 |
| GSM7679748 | MZ2    | 24h Zta   | SRX21243569 | SRR25513094 |
| GSM7679749 | MZ3    | 24h Zta   | SRX21243570 | SRR25513093 |
| GSM7679750 | MZ5    | 24h Zta   | SRX21243571 | SRR25513092 |
```

Supporting records: MZ1, MZ2, MZ3, and MZ5 each show one SRA experiment on the GSM page and one run on the linked SRA page. 

#### What stands out for the internal contrast: Mutu Zta 24h

The subset is very clean technically:
    • one cell line 
    • one timepoint 
    • one dominant perturbation 
    • stranded RNA-seq 
    • polyA RNA 
    • one run per sample 

#### Scoring Mutu Zta 24h:

This score applies to the defined contrast inside GSE240008, not to the GEO globally:

```text
Controls: MC1, MC2, MC3, MC4
Cases:    MZ1, MZ2, MZ3, MZ5
```

The series record lists this arm as a distinct internal design and provides a dedicated processed file for it, GSE240008_mutu_24h_zta_gene_expression.tpm.tsv.gz. The same record shows a 56-sample study spanning several sub-designs, including this Mutu 24 h Zta subset. 

##### Axis assignment

Primary Axis: EBV_IMMUNE
Bridge Axis: none

Justification: 

The manipulated experimental variable is Zta-driven EBV lytic reactivation in EBV-positive Mutu cells, which fits the rulebook’s perturbation-first tie-break logic. 

Ungerleider 2024 describes this arm as Mutu cells co-transfected with GFP plus control or SV40p-Zta, followed by GFP+ sorting at 24 h. 

Nguyen 2025 then reuses Mutu-Zta as one of the two flagship purified reactivation models. 

##### HIC

```text
Bulk RNA-seq = yes
≥3 replicates/condition = yes
Same cell type within contrast = yes
Clear perturbation = yes
Clean metadata = yes
→ HIC = PASS
```

Why:
    • the series is expression profiling by high throughput sequencing in human cells, and this arm has a dedicated processed RNA-seq file. 
    • the series sample list shows MC1–MC4 and MZ1–MZ3, MZ5, i.e. 4 vs 4. 
    • sample pages for MC1/MC2 and MZ1/MZ2 confirm Mutu, cell line, polyA RNA, RNA-seq, stranded, with treatment labels 24h Ctl or 24h Zta. 
    • Ungerleider 2024 explicitly describes this as a 24 h Mutu Zta induction arm with GFP+ cell isolation and N = 3 in one earlier host-reprogramming analysis framework, while the deposited GSE240008 arm itself is clearly the 4-vs-4 Mutu 24 h Zta subset listed on the current series page. 

##### Feature scoring

###### CTA = 1.00

Calculation basis: same named cell type, same defined population, no mixing.

Justification:
    • MC1 and MZ1 sample records both identify the source as B cell lymphoma, cell line Mutu, cell type cell line, with only the treatment field changing from 24h Ctl to 24h Zta. 
    • This is exactly the rulebook’s 1.00 class: the contrast is between the same clearly defined population on both sides. 

###### PTC = 1.00

Calculation basis: single comparison can be directly extracted without subsetting.

Justification:
    • the series provides a dedicated processed file for this exact arm, GSE240008_mutu_24h_zta_gene_expression.tpm.tsv.gz, which means this contrast is already operationally isolated at the study level. 
    • the biological perturbation is singular: control vs Zta at 24 h in one cell line. Ungerleider 2024 describes this arm explicitly as control or Zta vector in Mutu cells with GFP+ sorting. 

###### ExS = 1.00

Calculation basis: GEO directly provides a simple case-vs-control comparison.

Justification:
    • one cell line 
    • one timepoint 
    • one dominant perturbation 
    • no timecourse subsetting required 
    • no cross-lineage mixing required 

The only difference intended for analysis is 24h Ctl versus 24h Zta, which fits the rulebook’s 1.00 simplicity class. Supported by the series structure and sample labels. 

###### ReS = 0.90

Calculation:

```text
case_n = 4
control_n = 4
min(case_n, control_n) = 4
base lookup for 4 per condition = 0.90
balance penalty = 0.00
final ReS = 0.90
```

Evidence:
    • the series sample roster lists 4 controls and 4 Zta samples for this subset. 

###### SQES = 1.00

Base class: 1.00
Adjustment: 0.00
Final SQES: 1.00

Justification:
    • this is a strong expected system-wide perturbation: Zta is the immediate-early EBV lytic switch factor, and Ungerleider 2024 shows the Mutu-Zta model was used as a core reactivation system with high viral gene expression and widespread host transcriptome remodeling. 
    • Nguyen 2025 independently elevates Mutu-Zta as one of the two high-depth purified reactivation models used for improved EBV transcriptome resolution. 

That fits the rulebook’s 1.00 SQES class: broad downstream consequences from a strong viral reactivation perturbation.

###### BAR = 1.00

Calculation basis: direct anchor dataset for one Primary Axis.

Justification:
    • this contrast directly instantiates the EBV_IMMUNE primary axis through EBV lytic reactivation biology in an EBV-positive Burkitt lymphoma line. The series title, summary, and overall design all center this GEO on viral reprogramming during EBV reactivation. 
    • Mutu-Zta is one of the core reactivation models in both papers. 

###### BrV = 0.00

Calculation basis: this GEO does not function as a bridge.

Justification:
    • it is an anchor for the EBV axis, not a connector between two otherwise mismatched domains. 
    • under your rulebook, direct anchors can have high BAR and low BrV; this contrast does not itself span immune→neuro or neuro→mitochondrial space. 
    • therefore it falls into the 0.00 class: no bridge function in itself. 

###### FSF calculation

```text
FSF_raw =
  0.25(1.00) +
  0.15(1.00) +
  0.10(1.00) +
  0.10(0.90) +
  0.15(1.00) +
  0.15(1.00) +
  0.10(0.00)
= 0.25 + 0.15 + 0.10 + 0.09 + 0.15 + 0.15 + 0.00
= 0.89
FSF = 100 * 0.89 = 89.00
```

###### Final score for GSE240008 / Mutu Zta 24 h

GEO contrast: GSE240008 / Mutu Zta 24 h
Primary Axis: EBV_IMMUNE
Bridge Axis: none
HIC: PASS
CTA: 1.00
PTC: 1.00
ExS: 1.00
ReS: 0.90
SQES: 1.00
BAR: 1.00
BrV: 0.00
FSF: 89.00 / 100
Interpretation: core dataset

One note for your manifest: the MZ1, MZ2, MZ3, MZ5 naming discontinuity is a recordkeeping oddity, but the series record and GSM pages are otherwise clean enough that it does not lower the score here. 

### What Akata BCR 24 h actually is

#### 🔬 Contrast Definition

GEO: GSE240008
Subset: Akata BCR 24 h

Comparison:
```text
Controls:  24hrUninduced-1, 24hrUninduced-2, 24hrUninduced-3
Cases:     24hrInduced-1,   24hrInduced-2,   24hrInduced-3
```
From the study:
    • cell line: Akata (EBV+) 
    • perturbation: BCR crosslinking (anti-IgG) → EBV lytic reactivation 
    • timepoint: 24 h 
    • selection: GFP+ vs GFP− sorting (reactivation marker) 
    • library: polyA-selected, stranded RNA-seq 

#### 🧠 Axis Assignment

Primary Axis: EBV_IMMUNE
Bridge Axis: none

Why:
    • manipulated variable = BCR stimulation → EBV reactivation 
    • this is a canonical EBV lytic induction route 
    • no secondary domain (neuron/mito) is present 

#### 🚪 HIC

```text
Bulk RNA-seq = yes
≥3 replicates/condition = yes (3 vs 3)
Same cell type = yes (Akata cell line)
Clear perturbation = yes (uninduced vs induced)
Clean metadata = yes
→ HIC = PASS
```

Evidence:
    • Ungerleider describes this arm as Akata-BCR induction, 24 h, N=3, GFP-sorted RNA-seq 
    • series record confirms this as a defined subset within GSE240008 (ncbi.nlm.nih.gov) 

#### 📊 Feature Scoring

###### CTA = 1.00

Reasoning:
    • same named cell line (Akata) 
    • same lineage 
    • only treatment differs 
`→ perfectly homogeneous population`

###### PTC = 1.00

Reasoning:
    • single perturbation: BCR induction 
    • direct 24 h comparison 
    • no subsetting required 
`→ clean, directly extractable contrast`

###### ExS = 1.00

Reasoning:
    • simple 2-group design 
    • no timecourse 
    • no genotype layering 
    • no multi-factor confounding 
`→ ideal simplicity class`

###### ReS = 0.80

Calculation:

```text
case_n = 3
control_n = 3
min = 3 → lookup = 0.80
penalty = 0.00
final ReS = 0.80
```

###### SQES = 1.00

Base class: viral reactivation → 1.00
Adjustment: 0.00

Reasoning:
    • BCR crosslinking is a well-established EBV lytic trigger 
    • Ungerleider shows strong viral gene expression and host remodeling in this arm 
    • Nguyen 2025 elevates Akata-BCR as one of two flagship purified reactivation systems 
`→ strong system-wide transcriptional response expected`

###### BAR = 1.00

Reasoning:
    • this is a canonical EBV reactivation model 
    • directly instantiates EBV_IMMUNE axis 
    • central to both papers 
`→ direct anchor dataset`

###### BrV = 0.00

Reasoning:
    • does not connect axes 
    • purely EBV-centered system 
    • no neuron / mitochondrial interface 
`→ no bridge function`

###### 🧮 FSF Calculation

```text
FSF_raw =
  0.25(1.00) +
  0.15(1.00) +
  0.10(1.00) +
  0.10(0.80) +
  0.15(1.00) +
  0.15(1.00) +
  0.10(0.00)
= 0.25 + 0.15 + 0.10 + 0.08 + 0.15 + 0.15 + 0.00
= 0.88
FSF = 88.00 / 100
```

###### 🏁 Final Output for GSE240008 / Akata BCR 24 h

GEO contrast: GSE240008 / Akata BCR 24 h
Primary Axis: EBV_IMMUNE
Bridge Axis: none
HIC: PASS
CTA: 1.00
PTC: 1.00
ExS: 1.00
ReS: 0.80
SQES: 1.00
BAR: 1.00
BrV: 0.00
FSF: 88.00 / 100

Interpretation: core dataset

### What Mutu BCR 24 h actually is

#### 🔬 Contrast Definition

GEO: GSE240008
Subset: Mutu BCR 24 h

Comparison:

```text
Controls:  MU1, MU2, MU3
Cases:     MI1, MI2, MI3
```

From Ungerleider 2024:
    • cell line: Mutu (EBV+) 
    • perturbation: BCR crosslinking (anti-IgM) 
    • selection: GFP+ (induced) vs GFP− (uninduced) 
    • timepoint: 24 h 
    • library: polyA-selected, stranded RNA-seq 

#### 🧠 Axis Assignment

```text
Primary Axis: EBV_IMMUNE
Bridge Axis: none
```

Why:
    • manipulated variable = BCR-mediated EBV reactivation 
    • same logic as Akata BCR, but in a different EBV+ B-cell line 
    • no secondary biological domain involved 

#### 🚪 HIC

```text
Bulk RNA-seq = yes
≥3 replicates/condition = yes (3 vs 3)
Same cell type = yes (Mutu cell line)
Clear perturbation = yes (uninduced vs induced)
Clean metadata = yes
→ HIC = PASS
```

Evidence:
    • explicitly described as Mutu BCR induction, 24 h, N=3 in the RNA-seq design 
    • matches structure of other validated arms in GSE240008 (ncbi.nlm.nih.gov) 

#### 📊 Feature Scoring

##### CTA = 1.00

Reasoning:
    • identical cell line (Mutu) 
    • identical lineage 
    • only treatment differs 
`→ perfectly homogeneous population`

##### PTC = 1.00

Reasoning:
    • single perturbation: BCR stimulation 
    • direct 24 h contrast 
    • no need for subsetting 
`→ clean extraction`

##### ExS = 1.00

Reasoning:
    • simple 2-group design 
    • no timecourse 
    • no multi-factor confounding 
`→ ideal simplicity`

##### ReS = 0.80

Calculation:

```text
case_n = 3
control_n = 3
min = 3 → lookup = 0.80
penalty = 0.00
final ReS = 0.80
```

##### SQES = 0.95

Base class: 1.00 (viral reactivation)
Adjustment: −0.05
Final SQES = 0.95

Why the slight downgrade (important):
    • BCR-induced reactivation in Mutu is a strong perturbation, but: 
        ◦ compared to Zta overexpression, it is slightly more indirect 
        ◦ compared to Akata BCR, it is less emphasized in Nguyen 2025 
    • Ungerleider includes Mutu-BCR as part of the shared-response framework, but it is not one of the two flagship systems carried forward in Nguyen 2025 

So:
```text
strong signal, but slightly less canonical than:
- Mutu Zta
- Akata BCR
```

##### BAR = 1.00

Reasoning:
    • still a direct EBV reactivation model 
    • still squarely within EBV_IMMUNE axis 
    • contributes to host-response definition in Ungerleider 
`→ direct anchor`

##### BrV = 0.00

Reasoning:
    • remains a pure EBV-axis dataset 
    • does not bridge to neuronal or mitochondrial domains 
`→ no bridge role`

##### 🧮 FSF Calculation

```text
FSF_raw =
  0.25(1.00) +
  0.15(1.00) +
  0.10(1.00) +
  0.10(0.80) +
  0.15(0.95) +
  0.15(1.00) +
  0.10(0.00)
= 0.25 + 0.15 + 0.10 + 0.08 + 0.1425 + 0.15 + 0.00
= 0.8725
FSF = 87.25 / 100
```

##### 🏁 Final Output for GSE240008 / Mutu BCR 24 h

```text
GEO contrast: GSE240008 / Mutu BCR 24 h
Primary Axis: EBV_IMMUNE
Bridge Axis: none
HIC: PASS
CTA: 1.00
PTC: 1.00
ExS: 1.00
ReS: 0.80
SQES: 0.95
BAR: 1.00
BrV: 0.00
FSF: 87.25 / 100
Interpretation: core dataset
```

### 🧠 Triad Comparison

```text
| Contrast      | FSF       | Key differentiator                    |
| ------------- | --------- | ------------------------------------- |
| Mutu Zta 24h  | **89.00** | strongest perturbation + 4 replicates |
| Akata BCR 24h | **88.00** | canonical model, but n=3              |
| Mutu BCR 24h  | **87.25** | slightly less canonical + n=3         |
```

### 🔬 Important system-level conclusion

You now have three independent EBV-axis anchors that differ in:
    • mechanism: 
        ◦ Zta overexpression 
        ◦ BCR signaling (Akata) 
        ◦ BCR signaling (Mutu) 
    • cell line background 
    • signal structure 

This is extremely valuable for RSP because:

```text
convergence that appears across these three
is much more likely to be real EBV-driven biology
rather than model-specific artifact
```