# Milestone Map: rnaseq_pipeline (RSP)

## Purpose
Provide a reproducible RNA-seq analysis pipeline that transforms public sequencing data into gene-level functional evidence relevant to disease hypotheses.
Outputs should support:
- differential expression analysis 
- network convergence or pathway-level interpretation 
- later integration with: 
    - GSC (direct, as gene-level support)
    - VDB (optional / indirect / overlay attachment only)
    - RDGP (direct, as functional weighting)

---


## Terminology

- "functional evidence": transcriptomic support derived from expression, pathway, or network-level signals
- "gene-level functional evidence": expression-derived or network-derived support associated with a gene
- "network convergence": coordinated signal across genes or pathways suggesting shared biological perturbation

---

## Position in the Overall System

RSP is the upstream functional-evidence pipeline for the broader genomics system.

Flow:

public RNA-seq data → RSP → GSC and/or RDGP
                           ↘ indirect integration via VDB when gene-linked overlays are needed

## Strategic Value (High)
Signals:
  - RNA-seq workflow competence 
  - public data reuse (SRA/GEO) 
  - transcriptomics analysis 
  - hypothesis-driven dataset selection 
  - functional evidence integration 
👉 This repo shows:
“I can move beyond variants and evaluate downstream biological impact.”

---

## Core Output Data Model (v1)

RSP produces dataset-linked gene-level functional evidence suitable for integration into GSC and RDGP.

Fields:
- dataset_id
- contrast_id
- gene_symbol
- gene_id (if available)
- log2_fold_change
- adjusted_p_value
- differential_expression_status
- pathway_support (if available)
- network_support (if available)
- evidence_source
- analysis_version
- run_id

---

### Example Record (v1)

- dataset_id: GSE240008
- contrast_id: EBV_lytic_vs_control
- gene_symbol: POLG
- gene_id: ENSG00000140521
- log2_fold_change: -1.2
- adjusted_p_value: 0.004
- differential_expression_status: downregulated
- pathway_support: mitochondrial_function
- network_support: moderate
- evidence_source: RSP
- analysis_version: 2026-04
- run_id: RUN_2026_001

---

## Downstream Contract

RSP must produce gene-level functional evidence in a form that can be integrated into GSC and RDGP without ambiguity.

At minimum, RSP outputs must preserve:
- dataset or contrast identity
- gene identity
- effect direction and magnitude
- significance metrics
- provenance metadata
- analysis version / run_id

---

## Milestones

### M1 — Dataset Strategy + Input Triage

Define:
- target biological question 
- inclusion/exclusion criteria for datasets 
- candidate GEO/SRA studies 
- confounders to watch for: 
    - cell type 
    - treatment 
    - timepoint 
    - batch effects 

Output:
- curated input dataset shortlist 
- rationale for why these datasets are appropriate 

Goal:
RSP begins with a scientifically defensible dataset strategy, not random RNA-seq files

---


### M2 — Pipeline Skeleton (Acquisition → Counts)

Implement reproducible structure for:
- input metadata tracking 
- SRA/GEO download handling 
- QC hooks 
- alignment or pseudoalignment strategy 
- count matrix generation 

Use:
- one benchmark dataset first 

Goal:
End-to-end RNA-seq workflow exists from public input to count matrix

---


### M3 — Differential Expression Layer

Implement:
- sample grouping / contrasts 
- normalization approach 
- DEG generation 
- structured output tables 

Document:
- assumptions of chosen normalization/statistical approach 

Goal:
Pipeline produces defensible DEG outputs

---


### M4 — Functional Interpretation Layer

Add:
- pathway / gene-set overlay 
- candidate network convergence logic 

Outputs may include:
- DEG-linked gene evidence tables
- pathway-enriched gene sets
- network-convergence support tables

Goal:
RSP becomes more than DEG generation; it produces interpretable functional evidence

---


### M5 — Provenance + Reproducibility

Track:
- dataset accession 
- run metadata 
- sample inclusion decisions 
- contrasts tested 
- parameter settings 
- pipeline version / run ID 

Goal:
Every output can be traced to:
- which dataset 
- which comparison 
- which pipeline settings 

---


### M6 — Edge Cases + Confounder Handling

Explicitly address:
- batch effects 
- low replicate counts 
- inconsistent sample metadata 
- mixed treatment/timepoint designs 
- poor-quality runs 
- incomplete metadata 

Define:
- assumptions 
- limitations 

Goal:
Pipeline acknowledges where transcriptomics conclusions can break down

---


### M7 — Validation Strategy

Define and demonstrate:
- sanity checks: 
    - known biology recovered? 
    - expected control/treatment differences visible? 
- reproducibility checks across runs 
- consistency of top signals across similar datasets if applicable 
- output tables preserve the fields required for GSC / RDGP integration
- identical inputs and parameters produce identical structured outputs

Reference:
- benchmark datasets or known-positive biology where possible 

Goal:
RSP outputs are credible, not just computationally generated

---


### M8 — Integration Readiness

Prepare outputs for downstream use in:
- GSC: 
    - gene overlays 
    - functional support scores 
- RDGP: 
    - gene-level evidence 
    - network convergence signals 

This may include standardized output tables like:
- gene_expression_evidence.tsv 
- network_support.tsv 

Goal:
RSP produces reusable evidence, not isolated reports

---


## Release Gate (Public v1.0)
RSP is portfolio-ready when:
- at least one well-justified dataset has been processed end-to-end 
- count matrix and DEG outputs are reproducibly generated 
- functional interpretation layer exists 
- provenance is tracked 
- documentation includes: 
    - assumptions 
    - limitations 
    - edge cases 
    - validation strategy 
    - implementation details 
- outputs are structured for later integration with GSC / RDGP and preserve provenance required for those integrations
👉 At this point:
RSP v1.0 (public)

---


## Future Upgrades (Post v1.0)
- additional datasets 
- stronger network convergence methods 
- multi-dataset meta-analysis 
- disease-specific expression signatures 
- tighter integration with GSC and VDB 
- optional visualization/reporting layer 

---


## Critical Teaching Point
This repo is NOT:
“an RNA-seq pipeline for its own sake”
It is:
a functional evidence engine for your rare disease interpretation system

---


## How It Fits Within System
```text
RSP → produces:
    DEG evidence
    pathway evidence
    network convergence evidence
Feeds:
    GSC (gene-level support)
    RDGP (expression_support / network support)
Interacts with:
    VDB indirectly via gene-level integration
```

## One Sentence Summary
RSP answers: “Do the expression data provide functional support for candidate disease genes or pathways?”

---


