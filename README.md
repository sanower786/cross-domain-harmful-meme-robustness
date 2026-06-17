# cross-domain-harmful-meme-robustness



This repository accompanies the paper:



\# Understanding Cross-Domain Robustness in Harmful Meme Detection with Vision-Language Models



This repository accompanies the manuscript:



\*\*"Understanding Cross-Domain Robustness in Harmful Meme Detection with Vision-Language Models"\*\*



\## Overview



Detecting harmful content in internet memes remains challenging due to implicit semantics, multimodal interactions, and distribution shifts across heterogeneous online environments. This repository provides the experimental framework used to investigate cross-domain robustness, semantic transferability, and reasoning limitations of modern multimodal learning systems and vision-language models.



The study evaluates multiple multimodal paradigms under cross-domain settings using \*\*Memotion\*\* as the source domain and \*\*Hateful Memes\*\* as the target domain.



\---



\## Implemented Frameworks



\* CLIP Baseline

\* Domain Adaptation

\* Contrastive Representation Alignment

\* Fusion-Based Multimodal Learning

\* BLIP-2 Evaluation

\* LLaVA-1.5 Evaluation

\* Robustness Analysis

\* Calibration and Brier Score Evaluation

\* Failure Analysis



\---



\## Repository Structure



```text

cross-domain-harmful-meme-robustness

│

├── README.md

├── requirements.txt

│

├── src

│   └── main\_experiments.ipynb

│

├── data

│   └── dataset\_instructions.md

│

├── paper

│   └── manuscript.pdf

│

└── results

&#x20;   ├── figures

&#x20;   

&#x20;  

\## Datasets



Experiments utilize publicly available datasets:



1\. \*\*Memotion Dataset\*\*

2\. \*\*Hateful Memes Dataset\*\*



Please refer to `data/dataset\_instructions.md` for dataset preparation.



\---



\## Evaluation Metrics



The following metrics are reported:



\* Accuracy

\* Precision

\* Recall

\* F1-score

\* Harmful-content Recall

\* Brier Score

\* Confusion Matrix

\* Cross-domain Robustness Analysis



\---



\## Main Findings



The experiments reveal:



\* Different multimodal learning paradigms exhibit distinct robustness characteristics.

\* Domain adaptation improves overall classification accuracy.

\* Contrastive representation alignment enhances harmful-content sensitivity.

\* BLIP-2 achieves stronger robustness under semantic distribution shifts.

\* LLaVA-1.5 exhibits reasoning instability and lower harmful recall despite strong multimodal reasoning capability.



\---



\## Environment



Python ≥ 3.10



Main libraries:



\* PyTorch

\* Transformers

\* Scikit-learn

\* NumPy

\* Pandas

\* Matplotlib



Install dependencies:



pip install -r requirements.txt



\## Reproducibility



The notebook `src/main\_experiments.ipynb` contains the complete experimental pipeline used in this study, including:



\* Data preparation

\* Feature extraction

\* Model training

\* Domain adaptation

\* Contrastive learning

\* Fusion experiments

\* Vision-language model evaluation

\* Robustness analysis



