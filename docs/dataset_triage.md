# 🧠 TRIAGE FRAMEWORK (applied)
Each dataset is evaluated across:
1. Read Type (paired-end vs single-end)
2. FASTQ Size / Feasibility
3. Replicates (statistical validity)
4. Cell-Type Consistency (confounding)
5. Contrast Clarity (case vs control)
6. Pipeline Compatibility (bulk vs scRNA)

##🔬 RESULTS — TIERED OUTPUT
### 🟢 TIER 1 — IMMEDIATE EXECUTION (Mark-ready, repo4 v1 candidates)
These are clean, bulk-compatible, low-confounding, and actionable now.

#### 🧠 Epilepsy
##### ✅ GSE222259 — SCN2A iPSC neurons
Read type: paired-end (expected, confirm at SRR level)
Replicates: YES
Cell type: homogeneous neurons
Contrast: WT vs SCN2A+/−
Pipeline: PERFECT (bulk)
Verdict
#1 epilepsy dataset — run this first

##### ✅ GSE107878 — KCNQ2 / SCN2A neurons (subset required)
Replicates: YES
Cell type: consistent neurons
Contrast: KO vs control (subset)
Risk: mixed study — must subset
Verdict
#2 epilepsy dataset — after subsetting

#### 🧠 mTOR / TSC
##### ✅ GSE264590 — TSC2 isogenic NPCs
Replicates: YES
Cell type: NPC (clean)
Contrast: WT vs het vs KO
Pipeline: PERFECT
Verdict
#1 mTOR dataset — highest quality overall

##### ✅ GSE239412 — TSC1 NPCs + rapamycin
Replicates: YES
Cell type: NPC
Contrast: genotype + treatment
Strength: mechanistic clarity
Verdict
#2 mTOR dataset — mechanistic goldmine

### 🟡 TIER 2 — EXECUTE AFTER PIPELINE STABILIZATION
These are strong but require subsetting, filtering, or interpretation care.

#### 🧠 Epilepsy
##### ⚠️ GSE256142 — Dravet organoids
Cell type: organoid (variable)
Replicates: likely OK
Contrast: disease vs control
Risk: differentiation variability
Use
Phase 2 biology dataset

#### 🧠 mTOR
##### ⚠️ GSE137614 — TSC2 differentiation series
Problem: time-course + multiple lineages
Risk: confounding if not subset
Use
Select 1 lineage + 1 timepoint ONLY

##### ⚠️ GSE247367 — TSC2 organoids
Same issues as epilepsy organoids

### 🔴 TIER 3 — DO NOT USE (for bulk pipeline)
These will break or distort your initial analysis.

##### ❌ GSE215362 — ARX (scRNA-seq)
##### ❌ GSE245113 — Dravet GABA neurons (scRNA-seq)
Pipeline mismatch:
scRNA ≠ bulk pipeline

##### ❌ GSE235862 — neurovascular unit
Cell-type mismatch vs rest of pipeline

##### ❌ GSE286912 — unclear design / weak contrast
Low signal clarity

## 🧬 EBV + MITO QUICK TRIAGE (important context)
From your file:

### 🟢 KEEP
GSE240008 — EBV (gold standard v1)
GSE120854 — mito fibroblasts
GSE171537 — POLG fibroblasts

### 🟡 SECONDARY
GSE103198 — primary B cells
GSE125974 — latency vs lytic

### 🔴 LOWER VALUE
GSE118145 — LCL (transformation bias)
GSE51546 — time-course (complex)

## 🧠 FINAL EXECUTION SET (THIS IS YOUR REAL PLAN)
### 🧪 Phase 1 — pipeline validation
GSE240008

### 🧪 Phase 2 — first convergence triangle
Epilepsy:     GSE222259
mTOR:         GSE264590
Mito:         GSE171537
This is your cleanest convergence test possible.

### 🧪 Phase 3 — mechanistic expansion
GSE239412 (rapamycin)
GSE107878 (subset)
GSE120854 (mito diversity)

## 🧠 CRITICAL INSIGHT (this is the payoff)
Your triaged dataset set now forms:
GENOTYPE → TRANSCRIPTOME → NETWORK
across:
SCN2A (epilepsy)
TSC2 (mTOR)
POLG (mitochondrial)
EBV (viral perturbation)
That is extremely strong.

## ⚠️ RISKS (explicit per research standard)
1. Cell-type mismatch
NPC vs neuron vs fibroblast vs B cell
→ must interpret at pathway level, not gene level

2. Batch effects (cross-GEO)
cannot be fully corrected
→ use within-dataset DEG → cross-dataset enrichment

3. Effect size mismatch
EBV >> mito > epilepsy
→ normalize interpretation, not raw counts

## 🧠 VALIDATION STRATEGY
You must verify:
1. Each dataset reproduces its paper’s key findings
2. Known pathways are enriched:
   - mTOR (TSC)
   - ion channels (SCN2A)
   - ETC (POLG)
   - immune/metabolic (EBV)
Then:
3. Test convergence:
   overlap OR enrichment OR network proximity

## 🏁 Bottom line
After triage:
You now have:
- 4 gold-standard datasets (EBV, POLG, SCN2A, TSC2)
- 3 expansion datasets
- 4 datasets to ignore (for now)
This is a publishable-quality experimental design if executed correctly.

## 👉 Next step (recommended)
Say:
“build SRR shortlist”
and I will:
    • pick exact SRRs 
    • verify pairing (you got burned once—won’t happen again) 
    • estimate runtime on Mark 
    • generate your first manifest.yaml 
That’s the moment this becomes a real pipeline run.
