# SigLIP 2 Representation Validation

## 1. Objective

An additional controlled experiment was conducted to examine whether the cross-domain adaptation patterns observed in the primary CDRF evaluation persist when an alternative frozen multimodal representation is used.

The primary experiments employ frozen CLIP ViT-B/32 representations. The additional experiment uses frozen SigLIP 2 representations while retaining the same Memotion-to-Hateful Memes transfer setting, predefined data partitions, downstream classifier families, and S1–S2 evaluation strategies.

The experiment is intended as representation-level validation rather than a replacement for the primary CLIP evaluation.

## 2. Multimodal Representation

The alternative representation is obtained using:

- Model: SigLIP 2 Base Patch16 224.
- Model identifier: `google/siglip2-base-patch16-224`.
- Image embedding dimension: 768.
- Text embedding dimension: 768.
- Combined representation dimension: 1,536.

Image and text embeddings are concatenated to produce the multimodal feature vector. The SigLIP 2 backbone remains frozen during downstream classification.

The comparison therefore changes the frozen representation while retaining the intended downstream evaluation protocol.

## 3. Dataset and Transfer Setting

The experiment considers:

**Source domain:** Memotion

**Target domain:** Hateful Memes

The predefined forward-transfer partitions are:

| Partition | Number of instances |
|---|---:|
| Memotion source training | 5,588 |
| Memotion source validation | 1,398 |
| Hateful Memes target adaptation | 850 |
| Hateful Memes target validation | 850 |
| Hateful Memes target test | 6,800 |

The feature-alignment audit identified one corrupted Memotion image, `image_5119.png`, associated with sample ID 5118. This record was excluded from model-evaluable feature records. The original split manifest was retained for traceability.

The SigLIP 2 classification experiment uses the same predefined primary transfer partitions. The held-out target test set is not used for model fitting or threshold selection.

## 4. Downstream Classifiers

Three downstream classifier families are evaluated:

1. Logistic Regression.
2. Multilayer Perceptron (MLP).
3. XGBoost.

The classifier configurations follow the primary experimental settings documented in the CDRF protocol.

## 5. Evaluation Strategies

Two strategies are compared:

**S1 — Source-only**

The classifier is trained using source-domain training data and evaluated on the target domain without labeled target adaptation.

**S2 — Target-supervised adaptation**

The source training data are combined with the designated target-adaptation subset. Target-validation data are used for decision-threshold selection.

The target test partition remains held out for final evaluation.

## 6. Threshold Selection and Repeated Runs

Five stochastic runs are specified using seeds 42–46 on fixed data partitions. These runs characterize variability across stochastic training configurations; they do not represent five independent data splits.

Threshold selection uses target-validation Macro-F1 with:

- Search range: 0.10–0.90.
- Increment: 0.01.
- Tie-breaking: threshold closest to 0.50.

No threshold is selected using target-test performance.

Results are summarized using mean and standard deviation across the five runs.

## 7. Evaluation Metrics

The SigLIP 2 experiment reports:

- Accuracy.
- Balanced Accuracy.
- Precision.
- Harmful-class Recall.
- Harmful-class F1.
- Macro-F1.
- ROC-AUC.
- Brier Score.

The Brier score is retained as a measure of probabilistic prediction error. It should not be interpreted as a complete calibration assessment by itself.

Decision-threshold variability is also examined across runs as a complementary indicator of threshold stability.

## 8. Five-Seed Classification Results

The following table reports the SigLIP 2 target-test results as mean ± standard deviation across five runs.

| Strategy | Classifier | Accuracy | Macro-F1 | Harmful Recall | ROC-AUC | Brier |
|---|---|---:|---:|---:|---:|---:|
| S1 | Logistic Regression | 0.5119 ± 0.0000 | 0.4793 ± 0.0000 | 0.3648 ± 0.0000 | 0.4681 ± 0.0000 | 0.3784 ± 0.0000 |
| S1 | MLP | 0.4763 ± 0.0139 | 0.4756 ± 0.0133 | 0.6316 ± 0.0423 | 0.5111 ± 0.0232 | 0.5071 ± 0.0131 |
| S1 | XGBoost | 0.5655 ± 0.0190 | 0.5162 ± 0.0145 | 0.3587 ± 0.0946 | 0.5262 ± 0.0099 | 0.3314 ± 0.0038 |
| S2 | Logistic Regression | 0.6429 ± 0.0000 | 0.5899 ± 0.0000 | 0.3947 ± 0.0000 | 0.6317 ± 0.0000 | 0.2588 ± 0.0000 |
| S2 | MLP | 0.6599 ± 0.0084 | 0.6215 ± 0.0087 | 0.4789 ± 0.0482 | 0.6748 ± 0.0105 | 0.3095 ± 0.0044 |
| S2 | XGBoost | 0.6480 ± 0.0095 | 0.6085 ± 0.0037 | 0.4651 ± 0.0537 | 0.6577 ± 0.0033 | 0.2180 ± 0.0005 |

## 9. Main Observations

The results show that target-supervised adaptation improved accuracy, Macro-F1, and ROC-AUC for all three classifiers.

MLP achieved the highest S2 accuracy and Macro-F1, whereas XGBoost obtained the lowest S2 Brier score.

The adaptation response was not uniform across all evaluation dimensions. In particular, MLP exhibited a reduction in harmful-class recall despite improvements in accuracy and Macro-F1. Its selected decision thresholds also showed greater variability across the five runs.

XGBoost showed improvements across the reported classification metrics, including harmful-class recall and harmful-class F1. Logistic Regression also demonstrated improvements in Macro-F1 and ROC-AUC under S2.

The results indicate that adaptation effects depend on the downstream classifier and the evaluation dimension being considered.

## 10. Interpretation and Scope

The SigLIP 2 experiment provides complementary evidence that the CDRF evaluation protocol can be examined using an alternative frozen multimodal representation.

The results do not establish universal superiority of SigLIP 2 over CLIP. Nor do they demonstrate that cross-domain behaviour is invariant across representation backbones.

The experiment is limited to the forward Memotion-to-Hateful Memes transfer setting and the evaluated downstream classifiers. The primary CLIP-based CDRI3 analysis remains the reference analysis of the framework.

Accordingly, the SigLIP 2 experiment should be interpreted as a controlled representation-validation study rather than as a new architecture or a replacement robustness framework.

## 11. Relationship to the Primary CDRF Evaluation

| Aspect | Primary evaluation | Additional validation |
|---|---|---|
| Representation | Frozen CLIP ViT-B/32 | Frozen SigLIP 2 |
| Combined dimension | 1,024 | 1,536 |
| Primary transfer | Memotion → Hateful Memes | Memotion → Hateful Memes |
| Classifiers | LR, MLP, XGBoost | LR, MLP, XGBoost |
| Strategies | S1 and S2 | S1 and S2 |
| Main role | Primary CDRF evaluation | Alternative-representation validation |
| CDRI3 | Primary reported analysis | Not recalculated |

The two experiments are reported separately to preserve the distinction between the primary framework evaluation and the additional representation-level validation.