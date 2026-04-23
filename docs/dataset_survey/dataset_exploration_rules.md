# Dataset Exploration Rules

**Repository:** rnaseq_pipeline (RSP)  

**Purpose:** deterministic GEO triage and scoring for convergence-oriented RNA-seq dataset selection

---

## 1. Design Goal

Select RNA-seq datasets that maximize the ability to detect **shared pathway-, module-, and network-level signals** across mechanistically distinct perturbations, including:

- EBV / immune signaling
- neuronal excitability biology
- mTOR / metabolic signaling
- mitochondrial dysfunction / POLG-related biology
within a biologically interpretable and transcriptomically comparable framework.

This rulebook exists so that multiple evaluators can score GEO datasets **consistently, transparently, and auditably** for the RSP portfolio goal.

---

## 2. Core Design Principle

RSP is designed around the following rule:

```text
Within-dataset differential expression
→ cross-dataset comparison only at higher abstraction layers
```

This means:
      
    • DEGs are generated within a single GEO-defined experimental system 
    • GEOs are not pooled together for joint DEG modeling 
    • cross-dataset comparison is performed at the level of: 
        ◦ pathways 
        ◦ modules 
        ◦ networks 
        ◦ phenotype-support layers 

---

## 3. Analytical Context

Dataset exploration must support the full RSP scientific ladder:

    • Level 1 — direct gene overlap 
    • Level 2 — pathway overlap 
    • Level 3 — phenotype support overlap 
    • Level 4 — module-level convergence 
    • Level 5 — explicit network convergence 

Practical interpretation:

    • Level 1 is weak and noisy across heterogeneous GEOs 
    • Level 2–5 carry the main signal for the convergence hypothesis 
    • GEO selection should therefore optimize primarily for Levels 2–5 


---

## 4. Axis System

Each GEO must be assigned:

    • exactly one Primary Axis 
    • optionally one Bridge Axis 

A GEO must never have multiple primary axes.

### 4.1 Primary Axes

Primary axes represent the dominant perturbation domain.

#### A. EBV_IMMUNE

Definition:

    • EBV infection or reactivation 
    • interferon signaling 
    • innate / adaptive immune activation 
    • viral-host response biology 

Examples:

    • EBV lytic reactivation 
    • IFN-treated systems where immune signaling is the dominant perturbation 

#### B. NEURONAL_EXCITABILITY

Definition:

    • ion channel mutations 
    • neuronal firing / synaptic biology 
    • epilepsy-relevant neuronal excitability systems 

Important note:

This axis refers to neuronal excitability biology, not merely an autism or epilepsy diagnosis label.

Examples:
    • SCN2A neuronal models 
    • KCNQ2 neuronal models 

#### C. MTOR_METABOLIC

Definition:

    • TSC1 / TSC2 perturbations 
    • mTORC1 signaling 
    • nutrient sensing 
    • growth / energy integration 

Examples:

    • TSC2 isogenic models 
    • rapamycin / mTOR-response designs where mTOR biology is dominant 

#### D. MITOCHONDRIAL

Definition:

    • POLG mutation or depletion 
    • oxidative phosphorylation defects 
    • ETC dysfunction 
    • mtDNA maintenance defects 
    • primary mitochondrial stress biology 

Examples:

    • POLG mutant fibroblasts 
    • ETC inhibitor models where mitochondrial impairment is dominant 

### 4.2 Bridge Axes

Bridge axes are optional labels used when a GEO connects two otherwise orthogonal biological spaces.

#### E. NEURO_IMMUNE

Definition:

    • neuronal system 
    • immune / interferon / inflammatory perturbation 

Connects:

`EBV_IMMUNE ↔ NEURONAL_EXCITABILITY`

#### F. NEURO_MITO

Definition:

    • neuronal system 
    • mitochondrial stress / dysfunction 

Connects:

`NEURONAL_EXCITABILITY ↔ MITOCHONDRIAL`

#### G. IMMUNE_MITO (optional)

Definition:

    • immune system 
    • explicit mitochondrial rewiring or mitochondrial stress 

Connects:

`EBV_IMMUNE ↔ MITOCHONDRIAL`

Assign only when:
- the GEO clearly spans both named domains, and
- the overlap is central to the proposed contrast rather than incidental context.

#### H. META_INTEGRATION (optional)

Definition:

    • explicit integration of metabolism with either mitochondrial or neuronal biology 
    • often useful when mTOR and energy-sensing are tightly coupled 

Assign only when:
- the GEO clearly spans both named domains, and
- the overlap is central to the proposed contrast rather than incidental context.


---

## 5. Axis Assignment Rules

For each GEO:

### Step 1 — Assign Primary Axis

Ask:
`What is the dominant experimental perturbation?`

That perturbation determines the Primary Axis.

### Step 2 — Assign Bridge Axis (optional)

Ask:
`Does this dataset meaningfully connect two biological domains?`

If yes, assign one `Bridge Axis`.

### Examples
    • EBV lytic reactivation 
        ◦ Primary Axis: EBV_IMMUNE 
        ◦ Bridge Axis: none 
    • SCN2A iPSC neurons 
        ◦ Primary Axis: NEURONAL_EXCITABILITY 
        ◦ Bridge Axis: none 
    • IFN-treated neurons 
        ◦ Primary Axis: EBV_IMMUNE 
        ◦ Bridge Axis: NEURO_IMMUNE 
    • rotenone-treated neurons 
        ◦ Primary Axis: MITOCHONDRIAL 
        ◦ Bridge Axis: NEURO_MITO 
    • POLG fibroblasts 
        ◦ Primary Axis: MITOCHONDRIAL 
        ◦ Bridge Axis: none 

### Step 3 — Axis Tie-Break Rule

If a GEO could plausibly fit more than one Primary Axis, apply the following deterministic rule:

1. Identify the **manipulated experimental variable**.
2. Assign the **Primary Axis** based on that manipulated variable.
3. Treat the biological system or cellular context as a **Bridge Axis** only if it clearly links two domains.

Examples:
- IFN-treated neurons  
  - manipulated variable = immune / interferon stimulus  
  - Primary Axis = `EBV_IMMUNE`  
  - Bridge Axis = `NEURO_IMMUNE`

- rotenone-treated neurons  
  - manipulated variable = mitochondrial insult  
  - Primary Axis = `MITOCHONDRIAL`  
  - Bridge Axis = `NEURO_MITO`

- rapamycin-treated neurons in a TSC2 background  
  - if the dominant perturbation under evaluation is TSC2 genotype, Primary Axis = `MTOR_METABOLIC`
  - if the dominant perturbation under evaluation is drug response only, Primary Axis remains the pathway axis most directly represented by the manipulated variable

Important:
- cell type alone does not determine Primary Axis
- diagnosis label alone does not determine Primary Axis
- Bridge Axis never replaces Primary Axis


---

## 6. Hard Inclusion Criteria (HIC)

HIC are gates, not scored features.

A GEO passes HIC only if the proposed contrast satisfies all of the following:

    1. Bulk RNA-seq (not scRNA-seq) 
    2. At least 3 biological replicates per condition 
    3. Same cell type within the proposed contrast 
    4. Clear perturbation / case-vs-control structure 
    5. Metadata clean enough to define the contrast unambiguously 

If any one of these fails:
- HIC = FAIL

If all pass:
- HIC = PASS

### Required audit output for HIC

For every scored GEO, the evaluator must explicitly state:

    • Bulk RNA-seq? yes / no 
    • ≥3 replicates per condition? yes / no 
    • Same cell type within contrast? yes / no 
    • Clear perturbation? yes / no 
    • Clean metadata? yes / no 
    • Final HIC: PASS / FAIL 

### Important scoring scope

All HIC and feature scores are assigned to the **proposed contrast within the GEO**, not to the GEO title or accession in the abstract.

If a GEO contains multiple designs, timepoints, lineages, or perturbations, the evaluator must first define the exact intended contrast and then score that contrast only.

---

## 7. Scored Features

All scored features use a 0.00–1.00 scale.

### Feature list

    • CTA — Cell Type Alignment 
    • PTC — Perturbation Type Clarity 
    • ExS — Experimental Simplicity 
    • ReS — Replicate Strength 
    • SQES — Signal Quality / Effect Size 
    • BAR — Biological Axis Relevance 
    • BrV — Bridge Value 


---

## 8. Feature Definitions and Deterministic Scoring Rules

### 8.1 CTA — Cell Type Alignment

#### Weight: 0.25

Purpose:

    • measures internal cell-type coherence of the proposed contrast 
    • highest weight because cell identity strongly shapes transcriptomic signal 

#### Scoring rubric

- **1.00** = same named cell type, same defined population, no mixing, no broad tissue pooling
- **0.85** = same lineage and single defined population, with minor expected heterogeneity from differentiation state or culture variation
- **0.70** = related but broader biological system (e.g., broader neural lineage, mixed but constrained population) that remains usable for pathway/module interpretation
- **0.50** = mixed or partially mismatched cellular context, including systems where more than one relevant lineage contributes materially to the signal
- **0.25** = major heterogeneity, organoid/tissue complexity, or poorly constrained mixed populations
- **0.00** = unusable or undefined cell context for convergence work

#### Decision anchors

Use the following deterministic anchors:

- assign **1.00** only when the contrast is between the same clearly defined cell population on both sides
- assign **0.85** when both groups are still the same intended population, but differentiation or culture introduces mild expected variability
- assign **0.70** when the contrast stays within a broader lineage but cannot be described as one sharply bounded cell population
- assign **0.50** when the transcriptomic signal likely reflects multiple materially contributing populations
- assign **0.25** when cell heterogeneity itself is a dominant confounder

#### Assignment rule

CTA is assigned from the proposed comparison, not the whole GEO title.

Evaluator must state:

    • what the cell type is 
    • whether the comparison is homogeneous 
    • why the assigned value was chosen 

### 8.2 PTC — Perturbation Type Clarity

#### Weight: 0.15

Purpose:

    • measures how clearly the GEO isolates the main perturbation 

#### Scoring rubric

    • 1.00 = single dominant perturbation, directly interpretable 
    • 0.85 = strong primary perturbation with minor added complexity 
    • 0.70 = interpretable but requires subsetting 
    • 0.50 = multiple simultaneous variables 
    • 0.25 = ambiguous perturbation structure 
    • 0.00 = no clean perturbation contrast 

#### Assignment rule

PTC is based on whether a single intended contrast can be explained clearly and reproducibly.

Evaluator must state:

    • dominant perturbation 
    • whether subsetting is required 
    • why the final value was assigned 

#### Decision anchors

- assign **1.00** when a single comparison can be directly extracted without filtering or subsetting
- assign **0.85** when one additional factor exists but does not require removal to interpret the primary perturbation
- assign **0.70** when the intended contrast requires explicit subsetting (e.g., selecting one timepoint or one condition from a multi-factor design)
- assign **0.50** when multiple variables are entangled and cannot be cleanly separated without ambiguity


### 8.3 ExS — Experimental Simplicity

#### Weight: 0.10

Purpose:

    • measures confounding burden 

#### Scoring rubric

    • 1.00 = simple case vs control 
    • 0.85 = one extra manageable factor 
    • 0.70 = subsettable design with moderate complexity 
    • 0.50 = timecourse / multi-factor but partly usable 
    • 0.25 = heavy confounding 
    • 0.00 = too complex for current RSP stage 

#### Assignment rule

ExS asks:

How much non-essential complexity must be stripped away before the GEO is usable?

Evaluator must state:

    • key confounders 
    • whether a simple subset exists 
    • why the final value was assigned 

#### Decision anchors

- assign **1.00** when the GEO directly provides a simple case vs control comparison
- assign **0.85** when one additional variable exists but does not materially complicate interpretation
- assign **0.70** when a clean comparison can be obtained only after selecting a subset (e.g., one timepoint)
- assign **0.50** when multiple interacting variables remain even after subsetting
- assign **0.25** when confounding structure is a dominant feature of the design

### 8.4 ReS — Replicate Strength

#### Weight: 0.10

Purpose:

    • measures statistical robustness of the proposed contrast 

#### Scoring rubric

    • 1.00 = ≥5 replicates per condition and balanced 
    • 0.90 = 4 per condition 
    • 0.80 = 3 per condition 
    • 0.60 = 2 per condition 
    • 0.30 = highly unbalanced or unclear replication 
    • 0.00 = no usable replication 

#### Assignment rule

For a two-group comparison:

ReS = lookup(min(case_n, control_n))

Then apply:
    • subtract 0.05 if mildly unbalanced 
    • subtract 0.10 if strongly unbalanced 

Evaluator must show:
    • case_n 
    • control_n 
    • base lookup value 
    • any penalty applied 
    • final ReS 

### 8.5 SQES — Signal Quality / Effect Size

#### Weight: 0.15

Purpose:

    • estimates whether the GEO is likely to yield strong, interpretable biological signal 

#### Scoring rubric

    • 1.00 = strong expected signal from infection / KO / LOF / clear mitochondrial insult 
    • 0.85 = strong genotype or treatment signal 
    • 0.70 = moderate but interpretable signal 
    • 0.50 = subtle signal expected 
    • 0.25 = weak or noisy signal expected 
    • 0.00 = unlikely to support convergence inference 

#### Assignment rule

SQES is a design-informed prior, not a measured result.

Evaluator must justify using:
- perturbation strength
- model system
- expected downstream biology

#### Decision anchors

Use the following deterministic anchors before making only small judgment adjustments:

- **1.00** = strong expected system-wide signal from viral infection, strong immune stimulus, clean KO/LOF, or explicit mitochondrial insult with broad downstream consequences
- **0.85** = strong genotype-driven or treatment-driven signal expected to produce robust DEG and pathway output, but slightly less system-wide than the 1.00 class
- **0.70** = moderate but still interpretable signal, including perturbations expected to shift defined pathways without massive transcriptome-wide response
- **0.50** = subtle perturbation where biologically meaningful signal may exist but DEG yield or pathway strength may be modest
- **0.25** = weak or noisy perturbation with a high chance of unstable downstream inference
- **0.00** = perturbation unlikely to yield usable convergence signal

If multiple categories apply, assign the highest applicable class unless the perturbation is known to produce only localized rather than system-wide effects.

#### Allowed adjustment range

After choosing the base class above, evaluator may adjust by at most **±0.05** if:
- the model system is especially signal-dampening or signal-amplifying
- the perturbation is known to be unusually subtle or unusually broad in effect

The evaluator must explicitly state both:
- the base SQES class
- the reason for any ±0.05 adjustment

### 8.6 BAR — Biological Axis Relevance

#### Weight: 0.15

Purpose:

    • measures how directly the GEO maps onto the convergence framework 

#### Scoring rubric

- **1.00** = direct anchor dataset for one Primary Axis
- **0.85** = strong bridge dataset or dataset tightly aligned to one Primary Axis plus a clearly relevant Bridge Axis
- **0.70** = biologically relevant but indirect to the convergence framework
- **0.50** = weak, generic, or only partially informative relevance
- **0.25** = very indirect relevance
- **0.00** = not relevant to the convergence question

#### Decision anchors

Use the following deterministic ladder:

- assign **1.00** when the GEO directly instantiates one of the defined Primary Axes as its central perturbation
- assign **0.85** when the GEO is not a direct anchor but is a strong and intentional bridge between two defined axes
- assign **0.70** when the GEO supports one axis indirectly or captures biology adjacent to, but not centered on, the target framework
- assign **0.50** when the GEO is only generically related to stress, development, or signaling without strong axis specificity
- assign **0.25** when relevance is mostly speculative

#### Assignment rule

BAR is driven primarily by Primary Axis.

A valid Bridge Axis may justify a mild upward adjustment within the rubric.

Evaluator must state:

    • assigned primary axis 
    • optional bridge axis 
    • why the dataset is direct vs indirect for this project 

### 8.7 BrV — Bridge Value

#### Weight: 0.10

Purpose:

    • measures how much the GEO improves continuity across the overall dataset graph 

BrV must be evaluated relative to the currently selected dataset panel, not in isolation.

If evaluating GEOs prior to assembling a dataset panel, BrV should be estimated relative to the expected convergence path (Section 13) and updated once the panel is finalized.

#### Scoring rubric

- **1.00** = uniquely valuable bridge between two major axes in the current panel
- **0.85** = strong bridge with clear continuity role and few substitutes
- **0.70** = useful supporting bridge between axes already partly connected
- **0.50** = modest bridge value; helpful but not essential
- **0.25** = mostly redundant within the existing panel
- **0.00** = no bridge value

#### Decision anchors

Use the following deterministic ladder:

- assign **1.00** only when removing this GEO would leave a major axis-to-axis gap in the current dataset graph
- assign **0.85** when the GEO strongly connects two axes and materially improves continuity, but one or more partial alternatives exist
- assign **0.70** when the GEO supports an already connected pair and improves confidence rather than creating the connection
- assign **0.50** when the GEO offers some continuity value but does not substantially change the overall graph
- assign **0.25** when the GEO is largely redundant with already-selected datasets
- assign **0.00** when the GEO does not function as a bridge at all

#### Assignment rule

BrV is high only if the GEO connects otherwise mismatched axes.

Important:

    • direct anchors can have high BAR and low BrV 
    • bridges can have high BAR and high BrV 

Evaluator must state:

    • what two domains are being connected 
    • whether that connection is unique or redundant 
    • why the final value was assigned 



---

## 9. BAR vs BrV Relationship

These two features are related but not interchangeable.

### BAR

asks:

`How relevant is this GEO to the convergence question?`

### BrV

asks:

`How much does this GEO improve biological continuity across the full panel?`

### Examples

```text
| GEO Type            |  BAR |  BrV |
| ------------------- | ---: | ---: |
| EBV anchor          | high |  low |
| SCN2A anchor        | high |  low |
| NEURO_IMMUNE bridge | high | high |
| NEURO_MITO bridge   | high | high |
```


---

## 10. Final Score Formula (FSF)

All intermediate values should be calculated to at least two decimal places.
Final FSF should be reported to two decimal places.

If two GEOs produce identical FSF scores, prioritize the GEO with higher BrV, then higher BAR.

### Weights

    • CTA = 0.25 
    • PTC = 0.15 
    • ExS = 0.10 
    • ReS = 0.10 
    • SQES = 0.15 
    • BAR = 0.15 
    • BrV = 0.10 

### Formula

```text
FSF_raw =
  0.25*CTA +
  0.15*PTC +
  0.10*ExS +
  0.10*ReS +
  0.15*SQES +
  0.15*BAR +
  0.10*BrV
FSF = 100 * FSF_raw
```

### Interpretation bands

    • 85.00–100.00 = core dataset 
    • 70.00–84.99 = strong support / expansion candidate 
    • 55.00–69.99 = conditional / caution 
    • <55.00 = reject for current RSP scope 


---

## 11. Required Output Format for Each GEO

For every GEO, the evaluator must return:

```text
GEO: GSEXXXXX
Primary Axis: ...
Bridge Axis: ... or none
HIC:
  Bulk RNA-seq = yes/no
  ≥3 replicates/condition = yes/no
  Same cell type = yes/no
  Clear perturbation = yes/no
  Clean metadata = yes/no
  → HIC = PASS/FAIL
CTA = ...
  justification: ...
PTC = ...
  justification: ...
ExS = ...
  justification: ...
ReS = ...
  calculation: min(case_n, control_n)=... → base=...
  balance penalty = ...
  final ReS = ...
SQES = ...
  justification: ...
BAR = ...
  justification: ...
BrV = ...
  justification: ...
FSF_raw =
  0.25(CTA) + 0.15(PTC) + 0.10(ExS) + 0.10(ReS) +
  0.15(SQES) + 0.15(BAR) + 0.10(BrV)
= ...
FSF = ... / 100
```

This format is mandatory for auditability.


---

## 12. Meta-Rules for Maximizing Convergence Testing

### Rule 1 — Never compare raw expression across GEOs

    • within-dataset DEG only 
    • cross-dataset comparison only at higher abstraction levels 

### Rule 2 — Prefer orthogonal perturbations, but ensure continuity

    • orthogonal = independent biology 
    • bridges = shared biological space 

Both are required.

### Rule 3 — Optimize for Levels 2–5, not Level 1

    • raw gene overlap is weak and noisy 
    • pathways, modules, and networks carry the useful signal 

### Rule 4 — Penalize “complex but impressive” datasets

    • complex does not mean useful 
    • simpler, cleaner contrasts are usually more valuable 

### Rule 5 — Evaluate the dataset panel as a system

    • GEO quality is important 
    • but final utility depends on how the GEO fits into the broader convergence set 



---

## 13. Ideal Panel Structure

The dataset panel should ideally support a biologically plausible and transcriptomically comparable progression such as:

```text
EBV_IMMUNE
↓
NEURO_IMMUNE
↓
NEURONAL_EXCITABILITY
↓
NEURO_MITO
↓
MITOCHONDRIAL
↓
MTOR_METABOLIC
```

This is a target pattern, not a hard requirement.


---

## 14. Bottom Line

Maximize convergence detectability by selecting GEOs that are:

    • internally clean 
    • biologically aligned 
    • statistically robust 
    • connected through shared biological context 
    • auditable under this rulebook 

# End of File

---