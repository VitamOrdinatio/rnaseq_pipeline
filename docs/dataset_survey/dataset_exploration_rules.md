# Dataset Exploration Rules


## Design Goal

Select RNAseq datasets that:
- maximize the ability to detectshared pathway/module/network signals across mechanistically distinct perturbations such as:
  - EBV
  - epilepsy
  - mTOR
  - POLG
  - TSC2
  - POLG

within a comparable biological context.


---

## Primary Axes and Bridge Axes

An axis can exist as a primary axis or bridge axis.

### Primary Axis Types

A primary axis contains the core biology anchorage, including the core perturbation domains in the convergence design.

Rule:
`Each GEO maps to a primary axis.`

For this convergence project, there are four primary axis types:

1. EBV_IMMUNE
2. NEURONAL_EXCITABILITY
3. MTOR_METABOLIC
4. MITOCHONDRIAL


### Bridge Axis Types

A bridge axis contains overlapping elements and is useful to bridge primary axes in an effort to identify shared latent biological space.

For this convergence project, there are four bridge axis types:

1. NEURO_IMMUNE
2. NEURO_MITO
3. IMMUNE_MITO (optional)
4. META_INTEGRATION (optional)

---

## CORE RULESET (with weights)

### Tier 1 — Hard Inclusion Criteria (must pass)

Hard inclusion criteria are  gates and are not scored. 

If a dataset fails any of the following hard inclusion criteria, then it is rejected for further consideration. 

Include the dataset only if it is:
  - Bulk RNA-seq (not scRNA) 
  - ≥ 3 biological replicates per condition 
  - Clear case vs control (or subsettable to one) 
  - Single dominant perturbation (or isolatable) 
  - Same cell type within comparison 

HIC = pass only if all 5 hard criteria pass


### Tier 2 — Matching & Convergence Scoring (0–100)

Each dataset gets a weighted score based on these SEVEN features:

#### 1. Cell Type Alignment (CTA) — Weight: 0.25 (highest)

Controls biological comparability across datasets

- Same relevant lineage (neuronal, immune, fibroblast, etc.) 
- Prefer: 
    - iPSC-derived neurons 
    - primary human cells 
- Penalize: 
    - mixed tissues 
    - organoids (unless tightly controlled) 
    - cross-lineage comparisons within dataset
 
Why: Cell identity dominates transcriptomic signal.

#### 2. Perturbation Type Clarity (PTC) — Weight: 0.15

Ensures interpretable causal signal

- Prefer: 
    - genotype (KO, LOF, mutation) 
    - controlled infection / defined stimulus 
- Accept: 
    - clean chemical perturbations (e.g., ETC inhibitors) 
- Penalize: 
    - multi-factor designs (unless subsettable) 
    - unclear perturbation structure 
  
Why: Clean perturbations produce interpretable downstream signals.

#### 3. Experimental Simplicity (ExS) — Weight: 0.10

Reduces confounding

- Ideal: 
    - single variable (case vs control) 
- Accept: 
    - multi-factor if one clean contrast can be isolated 
- Penalize: 
    - timecourse-heavy designs (for v1) 
    - differentiation + treatment + genotype combined 

Why: Simpler designs → cleaner DEGs → better downstream convergence.

#### 4. Replicate Strength (ReS) — Weight: 0.10

Statistical reliability

- ≥3 per group = full score 
- Balanced design preferred 
- Penalize: 
    - n=2 
    - uneven group sizes 

Why: Weak replication destabilizes DEG → propagates noise into all layers.

#### 5. Signal Quality / Effect Size (SQES) — Weight: 0.15

Determines whether convergence is even detectable

- Strong perturbations: 
    - infection 
    - KO / LOF 
    - ETC disruption 
- Moderate: 
    - subtle regulatory mutations 
- Penalize: 
    - weak or noisy effects 

Why: No signal → no pathway/module/network inference.

#### 6. Biological Axis Relevance (BAR) — Weight: 0.15

Alignment with your convergence hypothesis
Dataset should map to one of:

- Human or human-derived system (preferred) 
- EBV / immune signaling 
- neuronal excitability (SCN2A-like) 
- mitochondrial dysfunction (POLG-like) 
- mTOR / metabolic signaling 
- bridges: 
    - neuron + immune 
    - neuron + mitochondrial 

Why: Convergence requires meaningful biological overlap.

#### 7. Bridge Value (BrV) (Continuity Contribution) — Weight: 0.10

Does this dataset connect otherwise distant axes?

High value if dataset:
  - links neuron ↔ immune 
  - links neuron ↔ mitochondrial 
  - links mitochondrial ↔ metabolism 

Low value if:
  - redundant within an already-covered axis 

Why: Bridges enable multi-step convergence inference.

## FINAL SCORE FORMULA (FSF)

Final Score = Σ(weight × feature score)

Range:
  - 85–100 → core dataset (v1 anchor) 
  - 70–85 → strong support / expansion 
  - 55–70 → conditional (use cautiously) 
  - <55 → reject 

## META-RULES FOR MAXIMIZING NETWORK CONVERGENCE TESTING

These are not weights—they’re governing principles.

#### Rule 1 — Never compare raw expression across datasets

- Within-dataset DEG only (i.e., do not pool SRRs from different GEOs)
- cross-dataset comparison occurs only at the pathway/module/network level

#### Rule 2 — Prefer orthogonal perturbations, but ensure continuity

Orthogonal = independent biology

Bridges = shared biological space

#### Rule 3 — Optimize for Level 2–5 (not Level 1)

- Gene overlap is weak and noisy 
- Pathway, module, and network levels carry the signal for hypothesis testing

#### Rule 4 — Penalize “complex but impressive” datasets

Complex ≠ useful

A simple, clean dataset is more valuable than a large, messy one.

#### Rule 5 — Dataset set > individual dataset

We are chasing a system of datasets, not evaluating them independently.

---

## Detailed Info on Primary Axis Types

### 🧠 1. Primary Axis Types

These are the core perturbation domains in our convergence design.
Each GEO should map to one primary axis.

#### 🔴 A. Viral / Immune Axis (EBV)

Axis name: EBV_IMMUNE

Definition

    • Viral infection (EBV specifically preferred) 
    • Interferon signaling 
    • innate/adaptive immune activation 

Examples

    • EBV lytic reactivation 
    • IFNβ stimulation (non-EBV but immune-aligned) 

#### 🔵 B. Neuronal / Excitability Axis

Axis name: NEURONAL_EXCITABILITY

Definition

    • Ion channel mutations (SCN2A, KCNQ2, etc.) 
    • neuronal firing / synaptic biology 
    • epilepsy-relevant neuronal systems 

Important clarification

This is not “epilepsy diagnosis” — it is:
neuronal excitability biology

#### 🟢 C. mTOR / Metabolic Signaling Axis

Axis name: MTOR_METABOLIC

Definition

    • TSC1 / TSC2 perturbations 
    • mTORC1 signaling 
    • nutrient / growth / energy sensing 

#### 🟡 D. Mitochondrial Axis

Axis name: MITOCHONDRIAL

Definition

    • POLG mutations 
    • ETC dysfunction 
    • mtDNA defects 
    • oxidative phosphorylation 

---

## Detailed Info on Bridge Axis Types

### 🧠 2. Bridge Axis Types (continuity layers)

These are NOT primary anchors.

They exist to:
`connect otherwise orthogonal biological spaces`

A GEO can have:
    • one primary axis 
    • AND optionally one bridge axis label 

#### 🟣 E. Neuron + Immune Bridge

Axis name: NEURO_IMMUNE (bridge)

Definition

    • neuronal system 
    • with immune / interferon / inflammatory perturbation 

Connects
`EBV_IMMUNE ↔ NEURONAL_EXCITABILITY`

#### 🟠 F. Neuron + Mitochondrial Bridge

Axis name: NEURO_MITO (bridge)

Definition

    • neuronal system 
    • with mitochondrial stress / dysfunction 

Connects
`NEURONAL_EXCITABILITY ↔ MITOCHONDRIAL`


#### ⚫ G. Immune + Mitochondrial Bridge (optional / rare)

Axis name: IMMUNE_MITO (bridge)

Definition

    • immune system 
    • with explicit mitochondrial perturbation or metabolic rewiring 

Connects

`EBV_IMMUNE ↔ MITOCHONDRIAL`

Not required, but useful if encountered.

#### 🟤 H. Metabolic Integration Bridge (optional)

Axis name: META_INTEGRATION (bridge)

Definition

    • datasets that explicitly link: 
        ◦ mTOR ↔ mitochondrial 
        ◦ metabolism ↔ neuronal biology 

Used sparingly.

---

## 🧠 How axes are assigned in practice

For each GEO:

### Step 1 — Primary axis

Ask:
`What is the dominant perturbation?`

That defines:
`Primary Axis`

### Step 2 — Bridge axis (optional)

Ask:
`Does this dataset live at the interface of two biological domains?`

If yes:
`assign bridge axis`

### 🧬 Examples

#### Example 1 — GSE240008

Primary Axis: EBV_IMMUNE
Bridge Axis: none

#### Example 2 — SCN2A neurons

Primary Axis: NEURONAL_EXCITABILITY
Bridge Axis: none

#### Example 3 — IFN-treated neurons

Primary Axis: EBV_IMMUNE (immune perturbation dominates)
Bridge Axis: N_IMMUNE

#### Example 4 — rotenone-treated neurons

Primary Axis: MITOCHONDRIAL (mitochondrial insult dominates)
Bridge Axis: N_MITO

#### Example 5 — POLG fibroblasts

Primary Axis: MITOCHONDRIAL
Bridge Axis: none

---

## Scoring Weights:


## 🧠 4. Scoring Relationship between BAR and BrV:

### BAR (Biological Axis Relevance)

    • Primary axis alignment drives BAR 
    • Bridge axis can boost BAR slightly if highly relevant 

### BrV (Bridge Value)

    • High only if: 
`dataset meaningfully connects two axes`

So:

```text
| Type             | BAR  | BrV  |
| ---------------- | ---- | ---- |
| EBV anchor       | high | low  |
| SCN2A anchor     | high | low  |
| NEURO+immune     | high | high |
| NEURO+mito       | high | high |
```

## 🧠 5. Important rule
Every GEO must have exactly ONE primary axis.
Bridge axis is optional.
Do NOT:
    • assign multiple primary axes 
    • let bridge labels replace primary identity 

## 🏁 Final Axis Set

### Primary axes
EBV_IMMUNE
NEURONAL_EXCITABILITY
MTOR_METABOLIC
MITOCHONDRIAL

### Bridge axes
NEURO_IMMUNE
NEURO_MITO
IMMUNE_MITO (optional)
META_INTEGRATION (optional)


## GEO SCORING RULES:

Formalized scoring ruleset for one-GEO-at-a-time verification

For each GEO, SAGE returns:
- Axis
- HIC: Pass / Fail
- CTA
- PTC
- ExS
- ReS
- SQES
- BAR
- BrV
- FSF

All feature scores will be on a 0.00–1.00 scale, and FSF will be on a 0–100 scale.

An FSF value of 100 is a perfect GEO.

### 1. HIC — Hard Inclusion Criteria

A GEO passes HIC only if all of the following are true for the proposed contrast:

    • Bulk RNA-seq, not scRNA-seq 
    • ≥3 biological replicates per condition 
    • Same cell type within the contrast 
    • Clear perturbation / case-vs-control structure 
    • Metadata sufficiently clean to identify the contrast 

#### HIC output format

```text
HIC = PASS
or
HIC = FAIL
```

#### Audit format

SAGE will show:
- Bulk RNA-seq? yes/no
- ≥3 replicates per group? yes/no
- Same cell type? yes/no
- Clear perturbation? yes/no
- Clean metadata? yes/no
- If any item is “no,” HIC = FAIL.


### 2. CTA — Cell Type Alignment

This score measures how internally coherent the cell type is within the proposed contrast and how useful it is for convergence work.

Scoring
    • 1.00 = single, homogeneous relevant cell type 
    • 0.85 = same lineage, minor expected heterogeneity 
    • 0.70 = related but broader system, still usable 
    • 0.50 = mixed or partially mismatched cellular context 
    • 0.25 = major heterogeneity or tissue complexity 
    • 0.00 = unusable / undefined for convergence work 

Calculation rule

SAGE assigns CTA using:

`CTA = cell-type coherence score`

with explicit justification from the GEO design.


### 3. PTC — Perturbation Type Clarity

This measures how clearly the GEO isolates the main perturbation.

Scoring
    • 1.00 = single dominant perturbation, directly interpretable 
    • 0.85 = strong primary perturbation with minor complexity 
    • 0.70 = interpretable but needs subsetting 
    • 0.50 = multiple simultaneous variables 
    • 0.25 = ambiguous perturbation structure 
    • 0.00 = no clean perturbation contrast 

Calculation rule

`PTC = perturbation clarity score`

based on how directly the GEO supports one contrast.

### 4. ExS — Experimental Simplicity

This measures confounding burden.

Scoring
    • 1.00 = simple case vs control 
    • 0.85 = one extra manageable factor 
    • 0.70 = subsettable design with moderate complexity 
    • 0.50 = timecourse / multi-factor but still partly usable 
    • 0.25 = heavy confounding 
    • 0.00 = design too complex for current RSP stage 

Calculation rule

`ExS = simplicity / confounding score`


### 5. ReS — Replicate Strength

This is based on the smallest group size in the chosen contrast.

Scoring
    • 1.00 = ≥5 replicates per condition and balanced 
    • 0.90 = 4 per condition 
    • 0.80 = 3 per condition 
    • 0.60 = 2 per condition 
    • 0.30 = highly unbalanced or unclear replication 
    • 0.00 = no usable replication 

Calculation rule

For a clean two-group comparison:

`ReS = lookup(min(case_n, control_n))`
using the table above.

If group sizes are unbalanced, SAGE may subtract 0.05–0.10.


### 6. SQES — Signal Quality / Effect Size

This estimates whether the GEO is likely to yield strong, interpretable downstream biology.

Scoring
    • 1.00 = strong expected signal from infection / KO / clear mitochondrial insult 
    • 0.85 = strong genotype or treatment signal 
    • 0.70 = moderate but still likely interpretable 
    • 0.50 = subtle signal expected 
    • 0.25 = weak or noisy expected signal 
    • 0.00 = unlikely to support meaningful convergence inference 

Calculation rule

This is a design-informed prior, not a measured post hoc result:

`SQES = expected signal strength based on perturbation class + model system`

SAGE will show the rationale explicitly.

### 7. BAR — Biological Axis Relevance

This measures how strongly the GEO maps onto your convergence system:

    • EBV / immune 
    • epilepsy / neuronal excitability 
    • mTOR / metabolic signaling 
    • POLG / mitochondrial biology 
    • bridge classes: N+immune, N+mito 

Scoring

    • 1.00 = direct anchor dataset for one axis 
    • 0.85 = strong bridge or highly relevant secondary axis 
    • 0.70 = biologically relevant but indirect 
    • 0.50 = weak or generic connection 
    • 0.25 = very indirect 
    • 0.00 = not relevant to the convergence question 

Calculation rule

`BAR = axis relevance score`

based on directness of biological fit.


### 8. BrV — Bridge Value

This measures how much the GEO helps connect otherwise mismatched axes.

Scoring

    • 1.00 = uniquely valuable bridge between two major axes 
    • 0.85 = strong bridge with clear continuity role 
    • 0.70 = useful supporting bridge 
    • 0.50 = modest bridge value 
    • 0.25 = mostly redundant 
    • 0.00 = no bridge value 

Calculation rule

`BrV = incremental continuity value to the overall dataset graph`

Important:

A direct anchor GEO can have high BAR but low BrV if it does not bridge.

### 9. FSF — Final Score Formula

Weights:
    • CTA = 0.25 
    • PTC = 0.15 
    • ExS = 0.10 
    • ReS = 0.10 
    • SQES = 0.15 
    • BAR = 0.15 
    • BrV = 0.10 

Formula

FSF_raw =
  0.25*CTA +
  0.15*PTC +
  0.10*ExS +
  0.10*ReS +
  0.15*SQES +
  0.15*BAR +
  0.10*BrV

FSF = 100 * FSF_raw

Interpretation bands
    • 85–100 = core dataset 
    • 70–84.99 = strong support / expansion candidate 
    • 55–69.99 = conditional / caution 
    • <55 = reject for current RSP scope

### 10. Output Format that SAGE uses for each scored GEO

For every GEO sent to SAGE, SAGE will return something like this:

```text
GEO: GSEXXXXX
Primary Axis: ...
Secondary Axis: ...
HIC:
  Bulk RNA-seq = yes
  ≥3 replicates/condition = yes
  Same cell type = yes
  Clear perturbation = yes
  Clean metadata = yes
  → HIC = PASS

CTA = 0.85
  justification: ...

PTC = 1.00
  justification: ...

ExS = 0.70
  justification: ...

ReS = 0.80
  calculation: min(case_n, control_n)=3 → ReS=0.80

SQES = 0.85
  justification: ...

BAR = 1.00
  justification: ...

BrV = 0.50
  justification: ...

FSF_raw =
  0.25(0.85) + 0.15(1.00) + 0.10(0.70) + 0.10(0.80) +
  0.15(0.85) + 0.15(1.00) + 0.10(0.50)
= ...

FSF = ... / 100
```

## IDEAL FINAL STRUCTURE

Your dataset panel should satisfy:

```text
EBV (immune)
↓
neuron + immune (bridge)
↓
SCN2A neurons
↓
neuron + mitochondrial (bridge)
↓
POLG / mitochondrial
↓
TSC2 / mTOR
```

Each edge should be:
`biologically plausible` + `transcriptomically comparable`

## Bottom Line

Maximize convergence detectability by selecting datasets that are:

- internally clean
- biologically aligned
- statistically robust
- connected through shared biological context
