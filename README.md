# Falcon-7B Embeddings for Cyberbullying Detection

**Research code and visualizations — anonymous supplementary materials for peer review**

This repository accompanies the manuscript **“LLM-Driven Cyberbullying Detection Using Falcon-7B Embeddings and Explainable ML.”** It studies binary classification of cyber-aggressive social-media text by combining text representations with ensemble classifiers and post-hoc explanations.

> **Peer-review note:** This repository is presented without author names or affiliations for anonymous review. Results reported below refer to the manuscript's experiment; reviewers should consult the notebook for the implementation and saved outputs. The repository does not, by itself, establish independent reproduction of every reported result.

## Study overview

- **Dataset:** Cyber-Troll (DataTurks), English-language Twitter text.
- **Preprocessing:** Text cleaning followed by removal of 167 samples with empty cleaned text, resulting in **19,834** samples. Labels of identical cleaned texts were reconciled by majority vote before splitting, as described in the manuscript.
- **Split:** Stratified 80/20 train/test partition: **15,867 training** and **3,967 test** samples.
- **Representations:** TF-IDF, Word2Vec, and frozen Falcon-7B last-token hidden-state embeddings (**4,544 dimensions**).
- **Classifiers:** Extra Trees (ET), Random Forest (RF), and XGBoost (XGB), giving nine representation/classifier combinations.
- **Validation and interpretation:** Training-set 10-fold stratified cross-validation; held-out test evaluation; SHAP and LIME explanations over latent embedding dimensions.

## Reported results

Percentages are taken from the updated manuscript experiment; **MCC is shown on a 0–100 scale**.

| Representation + classifier | Accuracy (%) | F1 (%) | ROC-AUC (%) | AP (%) | MCC (%) |
| --- | ---: | ---: | ---: | ---: | ---: |
| TF-IDF + XGB | 88.88 | 86.86 | 94.73 | 90.52 | 77.70 |
| TF-IDF + RF | 93.19 | 91.84 | 97.83 | 96.54 | 86.30 |
| TF-IDF + ET | 95.19 | 94.05 | 98.31 | 97.85 | 90.04 |
| Word2Vec + XGB | 94.10 | 92.85 | 97.26 | 95.42 | 88.01 |
| Word2Vec + RF | 95.59 | 94.53 | 97.81 | 97.04 | 90.86 |
| Word2Vec + ET | 96.87 | 96.04 | 98.09 | 98.03 | 93.46 |
| Falcon-7B + XGB | 95.89 | 94.88 | 98.41 | 98.53 | 91.46 |
| Falcon-7B + RF | 97.20 | 96.43 | **98.55** | **98.65** | 94.16 |
| **Falcon-7B + ET** | **97.83** | **97.20** | 98.39 | 98.48 | **95.53** |

The Falcon-7B + ET model has the highest held-out accuracy, F1, and MCC among the nine evaluated configurations. Falcon-7B + RF has the highest ROC-AUC and average precision. Reported 10-fold CV F1 for Falcon-7B + ET is **93.80 ± 0.57%**. Its held-out confusion matrix is `[[2389, 1], [85, 1492]]`, ordered as `[[TN, FP], [FN, TP]]`.

## Repository structure

```text
falcon7b-cyberbullying-detection/
├── README.md
├── notebook/
│   └── iccit-2026.ipynb
└── figures/
    ├── ROC-PR/
    │   ├── Figure3a_ROC_600dpi (1).png
    │   └── Figure3b_PR_600dpi (1).png
    ├── SHAP/
    │   ├── shap_summary_falcon (1).png
    │   ├── shap_bar_falcon (1).png
    │   ├── shap_waterfall_troll (1).png
    │   └── shap_waterfall_nontroll (1).png
    └── LIME/
        ├── lime_troll (1).html
        ├── lime_non_troll (1).html
        └── lime_non_troll (1).png
```

## Viewing the materials

1. Open [`notebook/iccit-2026.ipynb`](notebook/iccit-2026.ipynb) on GitHub to inspect the code and saved outputs. Alternatively, download the notebook and open it in JupyterLab or Google Colab.
2. Review [ROC and precision–recall plots](figures/ROC-PR/), [SHAP visualizations](figures/SHAP/), and [LIME explanations](figures/LIME/).
3. To rerun the notebook, provide the dataset and any model weights or embeddings required by its cells, then execute the cells in order. **Hardware, library versions, file paths, and external assets must be checked in the notebook before execution.** This README does not assert a fully automated one-command reproduction workflow.

The LIME HTML files are interactive explanations; download and open them in a browser if GitHub does not render them inline.

## Evaluation and interpretation notes

- The held-out test data should not be used to select model hyperparameters.
- Majority-vote reconciliation of labels for identical cleaned texts occurred before the reported split. Cross-partition duplicate overlap and independent evaluation should be checked when reproducing or extending the experiments.
- SHAP and LIME explanations operate on Falcon-7B **embedding dimensions**, not directly on words; latent dimensions should not be assigned unsupported linguistic meanings.
- Baseline comparisons from other papers may use different datasets and evaluation protocols and should not be taken as controlled head-to-head evidence.
- The reported findings are based on one English-language Twitter dataset; cross-dataset and multilingual validation remain future work.

## Citation and access

For peer review, refer to the accompanying anonymized manuscript. A formal author-attributed citation can be added after the double-blind review process, subject to the venue's policies.

**Repository:** https://github.com/agenticai21/falcon7b-cyberbullying-detection
