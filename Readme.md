# Credit Risk Scorecard Model using Lending Club Loan Data

# Dataset Link
https://www.kaggle.com/datasets/adarshsng/lending-club-loan-data-csv

# Credit Risk Scorecard Model with Reject Inferencing

## Project Overview

This project builds a **Credit Risk Scorecard Model** using the Lending Club loan dataset.  
The objective is to estimate the **Probability of Default (PD)** for loan applicants and transform it into a **credit score used for lending decisions**.

The project follows a **traditional banking scorecard modeling approach** widely used in financial institutions.

The model pipeline includes:

- Feature selection using Random Forest
- Monotonic binning
- Weight of Evidence (WOE) transformation
- Information Value (IV) feature selection
- Logistic regression Probability of Default (PD) model
- Scorecard scaling
- Model validation (AUC, KS, Gini)
- Population Stability Index (PSI)
- Reject Inferencing using the Parceling Method

---

# Business Problem

Financial institutions must evaluate whether a borrower is likely to **repay a loan or default**.

Poor credit decisions can lead to:

- Increased loan defaults
- Financial losses
- Higher regulatory capital requirements

A **credit risk scorecard** helps lenders:

- Predict the likelihood of default
- Rank applicants by risk
- Set lending thresholds
- Price loans appropriately

---

# Dataset

The project uses the **Lending Club Loan Dataset**, which contains historical loan performance data.

### Target Variable

The target variable indicates whether a loan **defaulted or was repaid**.

| Loan Status | Target |
|-------------|--------|
| Fully Paid | 0 |
| Charged Off | 1 |
| Default | 1 |

Where:

- **0 = Good borrower**
- **1 = Default (Bad borrower)**

---

# Project Workflow

## 1. Data Cleaning

Steps performed:

- Removed **data leakage variables**
- Dropped columns with **>70% missing values**
- Filtered dataset to include only **relevant loan outcomes**

---

## 2. Train / Validation Split

The dataset is split into:

- **70% Training dataset**
- **30% Validation dataset**

Stratified sampling is used to preserve the **default distribution**.

---

## 3. Missing Value Treatment

Missing values are handled as follows:

Numerical variables  
→ filled using **median**

Categorical variables  
→ filled with **"Missing" category**

---

# Feature Selection

## Random Forest Feature Importance

A **Random Forest classifier** is used to rank variables by predictive importance.

The **top 30 variables** are selected for further modeling.

Benefits:

- Captures nonlinear relationships
- Identifies predictive variables
- Reduces dimensionality

---

# Correlation Removal

Highly correlated variables (>0.7 correlation) are removed to:

- Reduce multicollinearity
- Improve model stability

---

# Exploratory Risk Analysis

Default rates are analyzed by:

- Numerical variables
- Categorical variables

This helps identify variables with **strong risk separation**.

---

# Monotonic Binning

Numerical variables are binned using **monotonic binning**.

The goal is to ensure that:

Default rates follow a **monotonic trend** across bins.

This improves:

- Interpretability
- Scorecard stability
- Regulatory acceptance

---

# Weight of Evidence (WOE) Transformation

WOE transforms variables into a format suitable for logistic regression.

Formula:

```
WOE = ln(Distribution of Good / Distribution of Bad)
```

Benefits of WOE:

- Handles categorical variables
- Handles missing values
- Maintains monotonic risk relationship
- Improves model interpretability

---

# Information Value (IV)

Information Value measures the **predictive power of variables**.

| IV Range | Predictive Strength |
|--------|-------------------|
| < 0.02 | Not useful |
| 0.02 – 0.1 | Weak predictor |
| 0.1 – 0.3 | Medium predictor |
| 0.3 – 0.5 | Strong predictor |
| > 0.5 | Suspicious |

Variables with **IV ≥ 0.03** are selected.

---

# Probability of Default (PD) Model

A **Logistic Regression model** is used to estimate PD.

Model form:

```
logit(PD) = β0 + β1X1 + β2X2 + ... + βnXn
```

Where:

- PD = Probability of Default
- Xi = WOE transformed variables

Logistic regression is widely used in **credit risk scorecards** due to its interpretability.

---

# Model Validation

## AUC (Area Under ROC Curve)

Measures model discrimination ability.

- **AUC = 0.5** → random model
- **AUC = 1.0** → perfect model

---

## Gini Coefficient

```
Gini = 2 × AUC − 1
```

Higher Gini indicates better model performance.

---

## KS Statistic

The **Kolmogorov-Smirnov statistic** measures the separation between good and bad borrowers.

Higher KS indicates stronger discrimination power.

---

# Credit Score Scaling

The predicted PD is converted into a **credit score** using the following parameters:

| Parameter | Value |
|----------|------|
| Base Score | 600 |
| Base Odds | 50 |
| PDO (Points to Double Odds) | 50 |

Score formula:

```
Score = Offset − Factor × log(PD / (1 − PD))
```

This produces a **traditional credit score distribution**.

---

# Risk Segmentation

Borrowers are grouped into risk bands based on PD.

| Risk Band | PD Range |
|----------|----------|
| Very Low | 0–2% |
| Low | 2–5% |
| Medium | 5–10% |
| High | 10–20% |
| Very High | >20% |

This segmentation helps lenders determine:

- Loan approval thresholds
- Pricing strategies
- Risk monitoring

---

# Population Stability Index (PSI)

PSI measures **population drift between development and validation samples**.

Interpretation:

| PSI Value | Interpretation |
|----------|---------------|
| < 0.1 | No population shift |
| 0.1 – 0.25 | Moderate shift |
| > 0.25 | Significant shift |

PSI helps monitor **model stability over time**.

---

# Reject Inferencing

## Problem

Credit risk models are typically trained only on **accepted applicants**.

Rejected applicants are not observed because they were **never granted loans**.

This creates **sample selection bias**.

---

## Solution: Reject Inferencing

Reject inferencing attempts to **estimate the default behavior of rejected applicants**.

This project implements the **Parceling Method**.

---

## Parceling Method

The parceling method works as follows:

1. Train model using accepted applicants
2. Score rejected applicants using the model
3. Divide applicants into **score bands**
4. Calculate **default rate in each band**
5. Assign inferred default probability to rejected applicants
6. Simulate Good/Bad outcomes
7. Retrain the model using **accepted + inferred rejects**

This reduces **selection bias** in the model.

---

# Model Comparison

Two models are compared:

| Model | Description |
|------|-------------|
| Original Model | Trained only on accepted applicants |
| Reject Inference Model | Trained on accepted + inferred rejects |

Performance is evaluated using:

- AUC
- Gini
- ROC curve comparison

---




