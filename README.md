# CDRF: Cross-Domain Robustness Evaluation for Multimodal Harmful Meme Understanding

This repository accompanies the manuscript:

**"CDRF: A Cross-Domain Robustness Evaluation Framework for Multimodal Harmful Meme Understanding"**

## Overview

Multimodal harmful meme understanding is challenging because harmful meaning can emerge from interactions between textual and visual content, while meme datasets may differ in their data distributions and annotation constructs.

This repository provides the experimental implementation and evaluation resources for the proposed **Cross-Domain Robustness Framework (CDRF)**. CDRF is an evaluation methodology rather than a new multimodal prediction architecture. It provides a structured protocol for examining model behaviour when a multimodal harmful-content detector is transferred from a source domain to an unseen target domain.

The framework evaluates multiple downstream learning strategies using a **common frozen multimodal representation**, allowing cross-domain behaviour to be examined under controlled experimental conditions.

The primary evaluation uses:

- **Memotion** as the source domain
- **Hateful Memes** as the primary target domain
- **MultiOFF** as an independent external validation domain

---

## Proposed Cross-Domain Robustness Framework

CDRF evaluates cross-domain behaviour through multiple complementary dimensions:

1. Target-domain predictive performance
2. Harmful-class sensitivity
3. Probabilistic prediction quality
4. Source–target performance consistency
5. Target-domain adaptation effects
6. Uncertainty analysis
7. CDRI3 ranking sensitivity
8. Transfer-direction analysis
9. Independent external-domain validation

The framework separates the composite robustness summary from its complementary diagnostics so that individual aspects of model behaviour remain interpretable.

---

## Cross-Domain Robustness Index (CDRI3)

The proposed **Cross-Domain Robustness Index (CDRI3)** provides a transparent composite summary of three target-domain dimensions:

- Target-domain Macro-F1
- Harmful-class recall
- Brier-derived reliability

The index is defined as:

\[
CDRI3 =
\frac{F_{1,T} + H_T + (1-\mathrm{Brier}_T)}{3}
\]

where:

- \(F_{1,T}\) is target-domain Macro-F1
- \(H_T\) is target-domain harmful-class recall
- \(1-\mathrm{Brier}_T\) is the Brier-derived reliability component

Equal weighting is used as a transparent reference configuration. Alternative weighting configurations are examined through a weight-sensitivity analysis.

CDRI3 is a component of CDRF rather than a replacement for the complete evaluation framework.

---

## Experimental Design

The primary cross-domain evaluation considers two directions:

### Forward Transfer

**Memotion → Hateful Memes**

Two strategies are evaluated:

- **S1 — Source-only:** model trained without labeled target-domain adaptation data
- **S2 — Target-supervised adaptation:** source training combined with a designated labeled target-adaptation subset

### Reverse Transfer

**Hateful Memes → Memotion**

The reverse direction is evaluated to examine the dependence of observed robustness behaviour on transfer direction.

### External Validation

**Memotion → MultiOFF**

**Hateful Memes → MultiOFF**

MultiOFF is used as an independent external validation domain rather than as part of the primary source–target pair.

---

## Multimodal Representation

All primary experiments use a common frozen multimodal representation obtained from:

**CLIP ViT-B/32**

`openai/clip-vit-base-patch32`

For each meme:

- Image embedding: 512 dimensions
- Text embedding: 512 dimensions
- Combined representation: 1024 dimensions

The image and text embeddings are concatenated to form the multimodal representation.

The representation is kept frozen throughout the downstream experiments. This controlled design allows the evaluation to focus on differences between downstream learning strategies rather than simultaneously changing the multimodal representation.

---

## Downstream Models

Three downstream learning strategies are evaluated:

### Logistic Regression

- C = 0.1
- liblinear solver
- Balanced class weighting
- Maximum iterations = 3000

### Multilayer Perceptron (MLP)

- Hidden layers: (256, 128)
- ReLU activation
- Adam optimizer
- Learning rate = 0.001
- Batch size = 32
- L2 regularization α = 10⁻⁴
- Maximum iterations = 100
- Early stopping disabled

### XGBoost

- Estimators = 200
- Maximum depth = 3
- Learning rate = 0.1
- Subsample = 0.8
- Column subsample = 0.8
- Minimum child weight = 1

Deterministic minority-class oversampling is applied only to the training data for MLP and XGBoost.

---

## Datasets

The experiments use three publicly available multimodal meme datasets:

1. **Memotion**
2. **Hateful Memes**
3. **MultiOFF**

The retained experimental instances are:

| Dataset | Eligible Instances | Non-harmful | Harmful |
|---|---:|---:|---:|
| Memotion | 6,986 | 2,710 | 4,276 |
| Hateful Memes | 8,500 | 5,450 | 3,050 |
| MultiOFF | 740 | 440 | 300 |

The datasets do not represent identical content constructs. Memotion and MultiOFF primarily represent offensive content, whereas Hateful Memes focuses on hateful content. The paper therefore treats construct differences as an important aspect of the cross-domain evaluation.

Dataset preparation instructions are provided in:

`data/dataset_instructions.md`

---

## Evaluation Metrics

The experimental analysis reports:

- Accuracy
- Macro-F1
- Harmful-class Recall
- Brier Score
- Brier-derived Reliability \(1-\mathrm{Brier}\)
- Negative Log-Likelihood (NLL)
- Cross-Domain Performance Consistency (\(C_{ST}\))
- CDRI3

Additional robustness diagnostics include:

- Paired bootstrap uncertainty analysis
- Leave-one-component-out analysis
- Metric–CDRI3 ranking comparison
- CDRI3 weight-sensitivity analysis
- Reverse-transfer evaluation
- Independent external-domain validation

---

## Reproducibility

The experimental protocol maintains strict separation between:

- Source training data
- Source validation data
- Target adaptation data
- Target validation data
- Target test data

The target test partition is strictly held out from:

- Model training
- Target adaptation
- Model selection
- Threshold selection

Five random seeds (42–46) are used for the repeated-seed analysis, with seed 42 designated as the primary/master seed for the main point estimates.

Paired bootstrap analysis uses 2,000 replicates with 95% confidence intervals for the primary S1–S2 comparison.

---

## Repository Structure

```text
cross-domain-harmful-meme-robustness/
│
├── README.md
├── requirements.txt
│
├── src/
│   └── main_experiments.ipynb
│
├── data/
│   └── dataset_instructions.md
│
├── paper/
│   └── manuscript.pdf
│
└── results/
    └── figures/