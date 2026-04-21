# rnaseq_pipeline (RSP)

## Overview

`rnaseq_pipeline` (RSP) is a bioinformatics system for generating **structured, gene-level functional evidence from transcriptomic data** and evaluating whether distinct biological perturbations converge on **shared vulnerable biological systems**.

RSP is not simply an RNA-seq processing pipeline. It is a **staged functional evidence engine** designed to progress from:

```text
gene-level perturbation → pathway interpretation → phenotype relevance → network convergence
```

---

## Motivation

Standard RNA-seq workflows often stop at:

```text
FASTQ → counts → differential expression → enrichment
```

This produces descriptive results, but does not address a deeper question:

```text
Do distinct perturbations (e.g., infection, genetic disease)
converge on shared biological systems in a way that is clinically meaningful?
```

RSP is designed to answer that question by producing **layered, interpretable outputs** that can be integrated with:

* phenotype-scoped gene evidence (GSC)
* variant-level data (VDB)
* patient-level prioritization (RDGP)

---

## Core Concept

RSP converts transcriptomic data into **gene-level functional evidence models** that are:

* dataset-aware
* contrast-specific
* provenance-preserving
* forward-compatible with higher-order analyses

Conceptually:

```text
RNA-seq → DEG → functional interpretation → structured gene-level evidence
```

---

## Scientific Analysis Ladder

RSP develops across five analytical strata:

### Level 1 — Direct Gene Overlap

Compare gene-level perturbations across conditions.

```text
Are the same genes affected?
```

---

### Level 2 — Pathway Overlap

Evaluate shared biological processes using ontology-driven methods (e.g., GO, pathways).

```text
Are the same biological systems affected?
```

---

### Level 3 — Phenotype Support Overlap

Evaluate whether perturbations affect **phenotype-relevant genes**, using gene-level evidence models (e.g., GSC).

```text
Do these changes disproportionately affect genes linked to phenotype X?
```

---

### Level 4 — Module-Level Convergence

Identify overlapping co-expression modules or coordinated functional units.

```text
Do different perturbations affect the same functional modules?
```

---

### Level 5 — Network Inference / Network Convergence

Test whether independent perturbations converge on shared biological network structure.

```text
Do distinct perturbations converge on shared vulnerable biological systems?
```

---

## Version Strategy

These analytical strata are implemented across staged releases:

### v1 — Transcriptomic Evidence Foundation

* Levels 1–2
* RNA-seq processing
* DEG generation
* pathway / GO interpretation
* structured, reproducible outputs

---

### v2 — Phenotype-Aware Interpretation

* Level 3
* integration with GSC-derived gene-level evidence models
* phenotype-relevant functional interpretation

---

### v3 — Convergence Engine

* Levels 4–5
* module-level convergence
* explicit network inference and convergence analysis
* multi-dataset comparison

---

## Outputs

RSP produces structured outputs aligned to the analytical ladder:

| Layer   | Output                                                         |
| ------- | -------------------------------------------------------------- |
| Level 1 | DEG tables, ranked gene lists                                  |
| Level 2 | pathway_support annotations                                    |
| Level 3 | phenotype-compatible gene-level evidence (via GSC integration) |
| Level 4 | module-aware signals (future)                                  |
| Level 5 | network models and convergence scores (future)                 |

All outputs are:

* reproducible
* provenance-aware
* structured for downstream integration

---

## Relationship to Other Repositories

RSP operates within a larger system:

```text
variant_annotation_pipeline (VDB) → What variants are present?
gene_set_consensus (GSC)          → Which genes matter for a phenotype?
rnaseq_pipeline (RSP)             → What biology is perturbed?
rare_disease_gene_prioritization (RDGP) → What matters for this patient?
```

### Key Principle

```text
RSP produces functional evidence.
It does NOT perform variant interpretation or patient-level prioritization.
```

---

## Integration with GSC

RSP and GSC interact bidirectionally:

```text
RSP → GSC:
    functional signals may be incorporated into gene-level evidence models

GSC → RSP:
    phenotype-scoped evidence informs interpretation of transcriptomic perturbation
```

GSC-derived support is interpreted as:

```text
gene-level evidence model (consensus_score + provenance),
NOT binary gene-set membership
```

---

## Integration with RDGP

RSP contributes **expression_support** to RDGP:

```text
gene-level functional evidence
+ variant-level evidence (VDB)
+ phenotype-level evidence (GSC)
→ patient-specific prioritization
```

RSP outputs are:

* dataset-linked
* contrast-specific
* not patient-specific by themselves

---

## Design Principles

```text
Reproducibility > convenience
Traceability > automation
Signal > noise
Evidence > assumption
```

---

## Scope (v1)

RSP v1 must:

* process RNA-seq datasets reproducibly
* generate high-quality DEG results
* perform pathway-level interpretation
* produce structured outputs suitable for downstream integration
* remain forward-compatible with phenotype-support and network analyses

RSP v1 must NOT:

* perform full network inference
* claim convergence across perturbations
* collapse into gene-set-only logic

---

## Disclaimer

```text
RSP does NOT establish causality.
It provides structured functional evidence derived from transcriptomic perturbation.
```

---

## Future Direction

* phenotype-aware functional modeling (v2)
* module-level convergence analysis (v3)
* network inference and convergence testing (v3)
* integration with multiple perturbation classes (e.g., EBV, POLG, epilepsy)
* identification of shared vulnerable biological systems

---

## Author Philosophy

```text
Biological insight emerges from structured, layered evidence—
not from isolated analyses.
```

---

## License

See LICENSE file.
