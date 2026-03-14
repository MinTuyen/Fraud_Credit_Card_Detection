# Fraud_Credit_Card_Detection
A cost-sensitive credit card fraud detection project that maximizes financial utility using fraud probabilities, transaction amounts, and threshold or top-K review policies.


# Profit-Aware Credit Card Fraud Detection

This project develops a **profit-aware credit card fraud detection framework** that goes beyond traditional metrics such as AUC-PR by incorporating **transaction amount**, **review cost**, and **operational review constraints** into the final decision policy.

Instead of asking only which model predicts fraud best, this project asks:

**Which combination of probabilistic fraud model and decision policy maximizes expected financial utility under operational review constraints?**

## Overview

In highly imbalanced fraud data, strong classification metrics do not always lead to the best business outcome. A model can achieve good recall or AUC-PR while still missing the most financially damaging fraud transactions.

This project addresses that gap by combining machine learning with a profit-based decision framework that prioritizes **financial impact**, not just statistical accuracy.

The framework compares machine learning models that output fraud probabilities, then applies decision rules that maximize expected profit while controlling operational constraints such as recall, false positive rate, and review capacity.

## Motivation

Traditional evaluation metrics like AUC-PR treat all fraud cases similarly, even though a missed small fraud and a missed high-value fraud do not have the same business impact.

This project is motivated by the idea that fraud detection should account for:

- **Transaction amount**
- **Cost of reviewing flagged transactions**
- **Limited fraud-team investigation capacity**
- **Business utility rather than classification performance alone**

## Problem Formulation

The framework evaluates transaction-level outcomes using a profit-based perspective:

- **True Positive (TP):** prevents fraud loss and incurs review cost
- **False Positive (FP):** incurs review cost and possible disruption cost
- **False Negative (FN):** loses the fraud amount
- **True Negative (TN):** no gain or cost

The final decision rule determines whether a transaction should be flagged for review based on its predicted fraud probability and expected financial value.

The framework also supports a **top-K review policy**, where only the most economically important transactions are selected when review capacity is limited.

## Methodology

### 1. Data Preprocessing
- Clean and inspect the fraud dataset
- Remove duplicates if needed
- Handle class imbalance carefully
- Reserve final evaluation on realistic class distributions

### 2. Feature Selection and Dimensionality Reduction
- Use feature ranking methods such as Random Forest importance or TOPSIS
- Apply PCA when appropriate to reduce dimensionality and noise
- Compare reduced and unreduced feature spaces

### 3. Candidate Models
- **Logistic Regression** as an interpretable baseline
- **Random Forest** for nonlinear ensemble learning
- **XGBoost** for powerful boosted tree modeling
- Optional ensemble methods if further improvement is needed

### 4. Model Pipelines
Different models use different preprocessing strategies:

- **Logistic Regression pipeline**
  - RobustScaler
  - LogisticRegression

- **Tree-based pipeline**
  - SMOTEENN
  - Random Forest / XGBoost

This avoids unnecessary scaling for tree models while still handling class imbalance appropriately inside the training pipeline.

### 5. Hyperparameter Tuning
For each hyperparameter configuration:

1. Train a probabilistic fraud model
2. Generate validation probabilities
3. Search for the threshold or review rule that maximizes estimated profit
4. Enforce business constraints such as minimum recall and maximum false positive rate

## Decision Policies

### Threshold-Based Policy
A transaction is flagged if its predicted fraud probability exceeds a threshold `t`.

The threshold is selected to maximize expected profit while satisfying business constraints.

### Top-K Review Policy
Transactions are ranked by **expected economic value of review**, and only the top `K` transactions are sent for investigation.

This is useful when fraud analysts can only review a limited number of alerts per day.

## Evaluation Metrics

The framework is evaluated using:

- Total estimated profit
- Recall
- Precision
- False positive rate
- Number of flagged transactions
- Percentage of fraud loss prevented

The main comparison criterion is **profit**, since it aligns most directly with the real business objective.

## Key Insight

AUC-PR is useful for imbalanced classification, but it does not reflect transaction value.

A model that performs well statistically may still fail to maximize financial benefit. By incorporating transaction-level loss and review cost, this framework aims to make fraud detection more aligned with real-world decision-making.

