# CDRF: Cross-Domain Robustness Evaluation Framework for Multimodal Harmful Meme Understanding

This repository accompanies the research manuscript:

**CDRF: A Cross-Domain Robustness Evaluation Framework for Multimodal Harmful Meme Understanding**

## Overview

Multimodal harmful meme understanding is challenging because harmful meaning can emerge from interactions between textual and visual content. Furthermore, meme datasets may differ in their data distributions, annotation schemes, and underlying content constructs.

This repository provides the experimental documentation, evaluation methodology, and consolidated research outputs for the proposed **Cross-Domain Robustness Framework (CDRF)**.

CDRF is an evaluation methodology rather than a new multimodal prediction architecture. It provides a structured protocol for examining the behaviour of downstream classifiers when transferred between different meme datasets.

The framework evaluates predictive performance, harmful-class sensitivity, probabilistic prediction quality, adaptation effects, and ranking stability under cross-domain distribution shifts.

The experiments use three publicly available datasets:

- **Memotion** — primary source domain.
- **Hateful Memes** — primary target domain.
- **MultiOFF** — independent external validation domain.

An additional controlled experiment evaluates an alternative frozen multimodal representation using SigLIP 2.

## Proposed Cross-Domain Robustness Framework

CDRF examines cross-domain behaviour through complementary evaluation dimensions:

1. Target-domain predictive performance.
2. Harmful-class sensitivity.
3. Probabilistic prediction quality.
4. Source–target performance consistency.
5. Target-domain adaptation effects.
6. Uncertainty analysis.
7. CDRI3 ranking sensitivity.
8. Transfer-direction analysis.
9. Independent external-domain validation.

The framework distinguishes the composite robustness summary from complementary diagnostics to preserve the interpretability of individual evaluation dimensions.

## Cross-Domain Robustness Index (CDRI3)

The proposed **Cross-Domain Robustness Index (CDRI3)** provides a composite summary of three target-domain dimensions:

- Target-domain Macro-F1.
- Harmful-class recall.
- Brier-derived reliability.

The index is defined as:

\[
CDRI3 =
\frac{F_{1,T}+H_T+(1-\mathrm{Brier}_T)}{3}
\]

where:

- \(F_{1,T}\) denotes target-domain Macro-F1.
- \(H_T\) denotes target-domain harmful-class recall.
- \(1-\mathrm{Brier}_T\) denotes the Brier-derived reliability component.

Equal weighting is used as a transparent reference configuration. Alternative weighting configurations are examined through weight-sensitivity analysis.

CDRI3 is one component of CDRF and does not replace the broader evaluation methodology.

## Experimental Design

### Forward transfer

**Memotion → Hateful Memes**

Two experimental strategies are considered:

- **S1 — Source-only:** downstream classifiers are trained using source-domain training data without labeled target-domain adaptation.
- **S2 — Target-supervised adaptation:** source training is combined with a designated labeled target-adaptation subset.

### Reverse transfer

**Hateful Memes → Memotion**

Reverse transfer is evaluated to examine whether cross-domain behaviour depends on the direction of transfer.

### External validation

Two independent external-domain settings are considered:

- Memotion → MultiOFF.
- Hateful Memes → MultiOFF.

MultiOFF is treated as an external validation dataset rather than as part of the primary source–target pair.

## Multimodal Representations

### Primary representation: CLIP ViT-B/32

The primary experiments use the frozen multimodal representation:

`openai/clip-vit-base-patch32`

The representation comprises:

- Image embedding: 512 dimensions.
- Text embedding: 512 dimensions.
- Combined representation: 1,024 dimensions.

Image and text embeddings are concatenated to form the multimodal feature vector. The representation remains frozen during downstream classifier training.

### Additional representation validation: SigLIP 2

An additional controlled experiment uses:

`google/siglip2-base-patch16-224`

The SigLIP 2 representation comprises:

- Image embedding: 768 dimensions.
- Text embedding: 768 dimensions.
- Combined representation: 1,536 dimensions.

The SigLIP 2 backbone remains frozen. The experiment retains the Memotion-to-Hateful Memes transfer setting, predefined data partitions, downstream classifier families, and S1–S2 evaluation strategies.

The purpose is to examine whether the observed adaptation patterns persist under an alternative multimodal representation. This experiment does not replace the primary CLIP evaluation or establish universal superiority of either representation.

## Downstream Learning Strategies

Three downstream classifier families are evaluated:

**Logistic Regression**
- C = 0.1
- Solver = liblinear
- Balanced class weighting
- Maximum iterations = 3,000

**Multilayer Perceptron**
- Hidden layers = (256, 128)
- ReLU activation
- Adam optimizer
- Learning rate = 0.001
- Batch size = 32
- L2 regularization α = 0.0001
- Maximum iterations = 100
- Early stopping disabled

**XGBoost**
- Estimators = 200
- Maximum depth = 3
- Learning rate = 0.1
- Subsample = 0.8
- Column subsample = 0.8
- Minimum child weight = 1

Minority-class oversampling is restricted to the relevant training data for the applicable classifiers.

## Datasets

The retained experimental instances are:

| Dataset | Eligible instances | Non-harmful | Harmful |
|---|---:|---:|---:|
| Memotion | 6,986 | 2,710 | 4,276 |
| Hateful Memes | 8,500 | 5,450 | 3,050 |
| MultiOFF | 740 | 440 | 300 |

The datasets do not represent identical content constructs. Memotion and MultiOFF primarily represent offensive content, whereas Hateful Memes focuses on hateful content. These differences are considered when interpreting cross-domain results.

Dataset acquisition and preparation instructions are provided in `data/dataset_instructions.md`.

## Evaluation Metrics

The evaluation includes:

- Accuracy.
- Balanced Accuracy.
- Precision.
- Macro-F1.
- Harmful-class Recall.
- Harmful-class F1.
- ROC-AUC.
- Brier Score.
- Brier-derived Reliability.
- Negative Log-Likelihood.
- Cross-Domain Performance Consistency (\(C_{ST}\)).
- CDRI3.

Complementary analyses include paired bootstrap uncertainty estimation, leave-one-component-out analysis, weight-sensitivity analysis, ranking comparisons, reverse-transfer evaluation, and external-domain validation.

## Reproducibility and Experimental Control

The primary experimental protocol uses predefined data partitions and maintains separation between source training, source validation, target adaptation, target validation, and target testing.

The target test partition is excluded from model training, adaptation, model selection, and threshold selection.

Five stochastic seeds (42–46) are used in the repeated-seed analysis, with seed 42 designated as the primary/master seed for the main point estimates.

For the SigLIP 2 validation experiment, thresholds are selected using target-validation Macro-F1 over a grid from 0.10 to 0.90 at increments of 0.01. Ties are resolved by selecting the threshold closest to 0.50. The held-out target test set is not used for threshold selection.

The primary paired bootstrap analysis uses 2,000 replicates and 95% confidence intervals for the S1–S2 comparison.

## Repository Contents

The repository provides:

- Experimental methodology and protocol documentation.
- Dataset preparation instructions.
- Consolidated evaluation results.
- Research figures and tables.
- The accompanying manuscript.

Training notebooks, raw datasets, intermediate embeddings, model checkpoints, and other private experimental artifacts are not distributed in this repository.

## Scope and Limitations

CDRF provides a structured methodology for evaluating cross-domain behaviour in multimodal harmful meme understanding. The primary evaluation uses frozen CLIP representations, while the additional SigLIP 2 experiment provides an alternative-representation validation.

The SigLIP 2 experiment is limited to the Memotion-to-Hateful Memes transfer setting and the evaluated downstream classifiers. It does not establish generalization across all vision-language models, datasets, or transfer directions.

The framework should therefore be interpreted as an evaluation methodology supported by the reported experiments rather than as a claim of universal cross-domain robustness.

## Citation

If you use this repository or the proposed evaluation methodology, please cite the accompanying manuscript:

**CDRF: A Cross-Domain Robustness Evaluation Framework for Multimodal Harmful Meme Understanding.**

Full bibliographic details will be added following publication.