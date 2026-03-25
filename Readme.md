# 🔐 Secure Search Systems: Filtering Sensitive Documents

This repository contains the implementation and evaluation framework for **Sensitivity-Aware Search (SAS)** — a paradigm designed to optimize retrieval effectiveness while mitigating the exposure of sensitive documents.

This project integrates state-of-the-art neural retrieval models - **ColBERT** and **SPLADE** with an **SVM-based sensitivity classifier** to build a responsible search pipeline.


## Project Overview

The core challenge addressed is the **trade-off between information utility and privacy protection**.

Traditional Information Retrieval (IR) systems assume all documents are equally accessible. This assumption breaks down when dealing with:
- Corporate emails  
- Legal documents  
- Medical records  

This framework evaluates both **single-model** and **multi-model pipelines**, applying fusion strategies to demote sensitive documents below visibility thresholds (e.g., top K = 10), while preserving relevance for safe content.


## Key Features

- **Neural Retrieval Integration**  
  Supports dense (**ColBERT**) and sparse (**SPLADE**) retrieval models  

- **Multi-Model Coordination**  
  Two-stage pipeline:  
  - ColBERT → candidate generation  
  - SPLADE → re-ranking  

- **Sensitivity Classification**  
  SVM classifier trained on the SARA dataset with Word2Vec-based augmentation  

- **Cost-Sensitive Evaluation**  
  Uses **nCS-DCG**, penalizing retrieval of sensitive documents  


## System Architecture

### 1. Single-Model Integration
Sequential pipeline combining:
- One retriever (ColBERT or SPLADE)
- SVM classifier
- Score fusion module

### 2. Multi-Model Coordination
Two-stage pipeline:
1. ColBERT for efficient retrieval  
2. SPLADE for precise re-ranking  
3. Sensitivity filtering

![Architecture Diagram](Architecture%20Diagram.jpeg)

## Fusion Methodologies

| Strategy | Description | Formula |
|----------|------------|---------|
| **Linear Combination** | Weighted balance between relevance and non-sensitivity | `f(R,S,α) = α·R + (1−α)·(1−S)` |
| **Harmonic Mean** | Strong penalty if either relevance or privacy is low | `f(R,S,α) = 1 / (α/R + (1−α)/(1−S))` |
| **Ridge (Basis Function)** | Learns optimal weights from features | `f(R,S) = Σ wᵢ·φᵢ(R,S)` |


## Key Findings

- **Privacy Improvement**  
  Harmonic Mean reduced sensitive exposure by **>95%**

- **Model Effectiveness**  
  SPLADE performed strongly in preserving relevance while filtering sensitive content  

- **Simplicity Wins**  
  Linear & Harmonic methods outperformed complex learned models  

- **Best Pipeline**  
  **ColBERT → SPLADE** achieved:
  - **nCS-DCG@10 = 0.959**


## Dataset

Experiments were conducted on the **SARA Collection**:

- **Source:** 1,702 Enron email documents  
- **Class Imbalance:**  
  - 12.4% sensitive  
  - 87.6% non-sensitive  
- **Queries:** 150 topics (business, legal, personal)


## Usage

Run notebooks in the following order:
- `01_Colbert.ipynb` → Dense retrieval (ColBERT)  
- `02_Splade.ipynb` → Sparse retrieval (SPLADE)  
- `03_Colbert+Splade.ipynb` → Two-stage pipeline  
- `04_SVM_Classification.ipynb` → Sensitivity classifier  
- `05_Evaluation.ipynb` → Evaluation & metrics  

