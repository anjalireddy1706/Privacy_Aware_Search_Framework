# Secure Search Systems: Filtering Sensitive Documents

[cite_start]This repository contains the implementation and evaluation framework for **Sensitivity-Aware Search (SAS)**, a paradigm designed to optimize information retrieval while mitigating the exposure of sensitive documents[cite: 11]. [cite_start]This project integrates state-of-the-art neural retrieval models—**ColBERT** and **SPLADE**—with **SVM-based sensitivity classification** to build a responsible search pipeline[cite: 13, 17].

---

## ## Project Overview

[cite_start]The core challenge addressed is the "fundamental tension" between information utility and privacy protection[cite: 44]. [cite_start]Traditional Information Retrieval (IR) systems assume all documents are equally accessible, which fails in collections containing corporate emails, legal documents, or medical records[cite: 43, 46]. 

[cite_start]This framework systematically evaluates single and multi-model pipelines using various fusion strategies to demote sensitive content below visibility thresholds (e.g., Rank $K=10$) while maintaining high relevance for safe documents[cite: 14, 175].

### ### Key Features
* [cite_start]**Neural Retrieval Integration:** Support for dense (ColBERT) and sparse (SPLADE) models[cite: 13].
* [cite_start]**Multi-Model Coordination:** A two-stage pipeline where ColBERT performs initial candidate generation and SPLADE handles re-ranking[cite: 208, 209].
* [cite_start]**Sensitivity Classification:** An SVM classifier trained on the SARA dataset, utilizing Word2Vec-based data augmentation to handle class imbalance[cite: 213, 214].
* [cite_start]**Cost-Sensitive Evaluation:** Implements **nCS-DCG**, a metric that applies additive penalties for retrieving sensitive content[cite: 246, 254].

---

## ## System Architecture

The project explores two primary architectural patterns:

1.  [cite_start]**Single-Model Integration:** A sequential pipeline combining one neural retriever (ColBERT or SPLADE) with the SVM classifier and a score fusion module[cite: 181, 182].
2.  [cite_start]**Multi-Model Coordination:** A sophisticated two-stage strategy leveraging ColBERT’s efficiency for broad retrieval and SPLADE’s precision for re-ranking before sensitivity analysis[cite: 195, 208, 209].


---

## ## Fusion Methodologies

We compare three strategies for combining relevance scores ($R$) and sensitivity predictions ($S$):

| Strategy | Description | Formula |
| :--- | :--- | :--- |
| **Linear Combination** | [cite_start]A weighted average of relevance and non-sensitivity[cite: 219]. | [cite_start]$f_{linear} = \alpha \cdot R + (1 - \alpha) \cdot (1 - S)$ [cite: 222] |
| **Harmonic Mean** | [cite_start]An aggressive strategy that heavily penalizes documents failing in either relevance or privacy[cite: 225, 227]. | [cite_start]$f_{harmonic} = \frac{1}{\frac{\alpha}{R} + \frac{1 - \alpha}{1 - S}}$ [cite: 228] |
| **Basis Function (Ridge)** | [cite_start]A data-driven approach using Ridge Regression to learn optimal weights for 11 non-linear features[cite: 231, 235]. | [cite_start]$f_{ridge} = \sum w_i \cdot \phi_i(R, S)$ [cite: 238] |

---

## ## Key Findings

* [cite_start]**Superior Privacy:** The Harmonic Mean fusion reduced sensitive document exposure by **over 95%** compared to the baseline[cite: 15].
* [cite_start]**Model Affinity:** SPLADE’s sparse lexical representations proved highly effective at reducing sensitive exposure while preserving quality[cite: 16, 567].
* [cite_start]**Simplicity Wins:** Simple, controllable methods (Linear and Harmonic) outperformed complex learned models, which tended to converge on trivial, relevance-sacrificing solutions[cite: 15, 565].
* [cite_start]**Optimal Pipeline:** The **ColBERT → SPLADE** multi-model pipeline achieved the highest overall balance of utility and privacy, reaching an **nCS-DCG@10 of 0.959**[cite: 15, 384].

---

## Dataset

Experiments were conducted on the **SARA Collection**:
* **Source:** 1,702 Enron email documents[cite: 273].
**Imbalance:** 12.4% sensitive, 87.6% non-sensitive[cite: 275].
**Queries:** 150 diverse topics[cite: 276].


Files Overview - 

01_Colbert.ipynb - ColBERT model implementation
02_Splade.ipynb - SPLADE model implementation
03_Colbert+Splade.ipynb - Combined model (ColBERT+SPLADE) approach
04_SVM_Classification.ipynb - SVM classifier implementation
05_Evaluation.ipynb - Model evaluation using three fusion methods


Folders -

experiments/ - Sara indexing used for ColBERT retrieval
svm-dataset/ - Training data (augmented) and test dataset
trained_classifier/ - Saved classifier model
retrieval_scores/ - Raw retrieval scores for each of the three models

Usage
Run notebooks 01-05 in sequence.
