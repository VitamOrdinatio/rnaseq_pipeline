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
  - Human or human-derived system (preferred) 

### Tier 2 — Matching & Convergence Scoring (0–100)

Each dataset gets a weighted score based on these SEVEN features:

#### 1. Cell Type Alignment — Weight: 0.25 (highest)

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

#### 2. Perturbation Type Clarity — Weight: 0.15

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

#### 3. Experimental Simplicity — Weight: 0.10

Reduces confounding

  - Ideal: 
      - single variable (case vs control) 
  - Accept: 
      - multi-factor if one clean contrast can be isolated 
  - Penalize: 
      - timecourse-heavy designs (for v1) 
      - differentiation + treatment + genotype combined 

Why: Simpler designs → cleaner DEGs → better downstream convergence.

#### 4. Replicate Strength — Weight: 0.10

Statistical reliability

  - ≥3 per group = full score 
  - Balanced design preferred 
  - Penalize: 
      - n=2 
      - uneven group sizes 

Why: Weak replication destabilizes DEG → propagates noise into all layers.

#### 5. Signal Quality / Effect Size — Weight: 0.15

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

#### 6. Biological Axis Relevance — Weight: 0.15

Alignment with your convergence hypothesis
Dataset should map to one of:

  - EBV / immune signaling 
  - neuronal excitability (SCN2A-like) 
  - mitochondrial dysfunction (POLG-like) 
  - mTOR / metabolic signaling 
  - bridges: 
      - neuron + immune 
      - neuron + mitochondrial 

Why: Convergence requires meaningful biological overlap.

#### 7. Bridge Value (Continuity Contribution) — Weight: 0.10

Does this dataset connect otherwise distant axes?

High value if dataset:
  - links neuron ↔ immune 
  - links neuron ↔ mitochondrial 
  - links mitochondrial ↔ metabolism 

Low value if:
  - redundant within an already-covered axis 

Why: Bridges enable multi-step convergence inference.

## FINAL SCORE FORMULA

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
