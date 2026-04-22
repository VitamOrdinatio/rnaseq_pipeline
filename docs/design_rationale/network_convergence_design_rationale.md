# 🧠 Network Convergence & Bridging Strategy (RSP Design Rationale)

---

## Overview

This document defines the scientific rationale behind the RNA-seq dataset selection strategy used in the `rnaseq_pipeline (RSP)`.

The goal is to test:

```text
Do distinct biological perturbations converge on shared vulnerable biological systems?
```

This is approached using **multi-dataset transcriptomic convergence analysis**, not single-dataset interpretation.

---

## Key Design Principle

```text
Within-dataset differential expression (DEG)
→ cross-dataset comparison at higher abstraction layers
```

We do **not** attempt to:

* pool datasets across studies
* generate a single DEG model across conditions
* directly compare raw expression values between datasets

Instead, each dataset is analyzed independently, and convergence is evaluated at:

* pathway level
* phenotype-support level
* module level
* network level

This approach reflects best practices in cross-study transcriptomics, where gene-level overlap is often unstable but pathway-level signals are more robust (Subramanian 2005, PMID: 16199517).

---

## Orthogonal Perturbation Strategy

Datasets are selected to represent **independent biological perturbations**, including:

* viral infection (EBV)
* neuronal excitability mutation (SCN2A)
* mitochondrial dysfunction (POLG)
* metabolic pathway dysregulation (TSC2)

These perturbations are **mechanistically distinct**.

### Rationale

This design allows testing:

```text
Do different causes lead to shared downstream biology?
```

This aligns with the concept of **convergent transcriptional pathology**, where distinct etiologies produce overlapping molecular signatures (Gandal 2018, PMID: 30049709; Voineagu 2011, PMID: 21614001).

---

## The Problem: Cross-Dataset “Gaps”

Direct comparison between unrelated datasets is problematic due to:

* cell-type differences
* experimental design differences
* perturbation type differences
* study-specific batch effects

Example:

```text
EBV (immune, B cell) vs POLG (mitochondrial, fibroblast)
```

These datasets do not share a common biological context.

This reflects a well-known challenge in transcriptomics:

```text
context-dependent gene expression limits cross-study comparability
```

(Batch effects and study heterogeneity are major confounders in transcriptomic meta-analysis; Leek 2010, PMID: 20167110).

---

## Bridging Strategy

To address this, we introduce **bridging datasets**.

### Definition

A bridging dataset is one that:

```text
shares biological context with two otherwise mismatched datasets
```

Examples:

* neuron + immune signaling dataset
* neuron + mitochondrial dysfunction dataset

---

## Conceptual Model

Instead of:

```text
Dataset A ↔ Dataset B (direct comparison)
```

we construct:

```text
Dataset A → Bridge → Dataset B
```

This enables comparison through a **shared biological framework**, conceptually similar to comparing perturbations within a **common latent biological space** (Subramanian 2017, PMID: 29195078).

---

## Biological Continuity (Clarified)

The strategy does **not assume direct relationships** between perturbations.

Instead, it tests whether a **biologically plausible chain of effects** is reflected in transcriptomic data.

Example hypothesis chain:

```text
EBV infection
→ interferon / immune signaling
→ neuronal stress
→ altered excitability (SCN2A context)
→ increased metabolic demand
→ mitochondrial stress
→ mitochondrial dysfunction (POLG context)
→ metabolic regulation (mTOR / TSC2)
```

Each step in this chain is supported by existing literature:

* Viral infection alters host metabolism and mitochondrial function (Sanchez & Lagunoff 2015, PMID: 26099687)
* Immune signaling affects neuronal function and excitability (Vezzani 2011, PMID: 21903039)
* Neuronal activity is tightly coupled to mitochondrial energy metabolism (Kann & Kovács 2007, PMID: 17303343)
* Mitochondrial dysfunction contributes to neurological disease (Nunnari & Suomalainen 2012, PMID: 22783344)
* mTOR integrates metabolic and mitochondrial signals (Laplante & Sabatini 2012, PMID: 23151496)

---

## Relation to Established Frameworks

This strategy aligns with several known concepts:

### 1. Convergent Transcriptional Signatures

Distinct perturbations producing similar downstream gene expression patterns
(Gandal 2018, PMID: 30049709)

### 2. Network Medicine

Diseases mapped onto biological networks where distinct perturbations affect shared regions
(Barabási 2011, PMID: 21164525)

### 3. Pathway-Level Comparison

Cross-study comparison using enrichment rather than raw gene overlap
(Subramanian 2005, PMID: 16199517)

### 4. Transcriptomic Signature Similarity

Comparison of ranked gene expression patterns across perturbations (Connectivity Map paradigm)
(Subramanian 2017, PMID: 29195078)

---

## Role of Bridging Datasets

Bridging datasets improve:

* interpretability of cross-dataset comparisons
* stability of pathway-level signals
* ability to detect module-level convergence
* plausibility of network-level inference

They do **not force convergence**, but provide:

```text
biological continuity for testing convergence
```

---

## What This Strategy Is NOT

* Not a claim that EBV causes POLG-related disease
* Not a direct causal model
* Not a replacement for controlled experimental studies

---

## What This Strategy IS

```text
A structured, systems-level hypothesis testing framework
```

to evaluate whether:

```text
independent perturbations converge on shared biological vulnerabilities
```

---

## Expected Outcomes

At different analysis levels:

| Level     | Interpretation                            |
| --------- | ----------------------------------------- |
| Gene      | Weak overlap expected                     |
| Pathway   | Stronger convergence signal               |
| Phenotype | Clinically relevant overlap               |
| Module    | Shared functional systems                 |
| Network   | Convergence on vulnerable biological hubs |

---

## Summary

```text
The bridging strategy enables controlled comparison
of otherwise incomparable transcriptomic datasets
by introducing intermediate biological contexts.
```

This allows testing of convergence hypotheses that would otherwise be obscured by cross-study heterogeneity.

---

# 📚 Works Cited

* Subramanian A et al. (2005) Gene set enrichment analysis. *PNAS*. PMID: 16199517
* Gandal MJ et al. (2018) Shared molecular neuropathology across psychiatric disorders. *Science*. PMID: 30049709
* Voineagu I et al. (2011) Transcriptomic analysis of autistic brain. *Nature*. PMID: 21614001
* Leek JT et al. (2010) Tackling the widespread and critical impact of batch effects. *Nat Rev Genet*. PMID: 20167110
* Subramanian A et al. (2017) The Connectivity Map (L1000 platform). *Cell*. PMID: 29195078
* Sanchez EL, Lagunoff M (2015) Viral activation of cellular metabolism. *J Virol*. PMID: 26099687
* Vezzani A et al. (2011) Inflammation and epilepsy. *Nat Rev Neurol*. PMID: 21903039
* Kann O, Kovács R (2007) Mitochondria and neuronal activity. *Am J Physiol*. PMID: 17303343
* Nunnari J, Suomalainen A (2012) Mitochondria: sickness and health. *Cell*. PMID: 22783344
* Laplante M, Sabatini DM (2012) mTOR signaling. *Cell*. PMID: 23151496
* Barabási AL et al. (2011) Network medicine. *Nat Rev Genet*. PMID: 21164525

---

# End of Document

