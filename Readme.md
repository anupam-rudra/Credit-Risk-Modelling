# Credit Risk Scorecard Model using Lending Club Loan Data

# Dataset Link
https://www.kaggle.com/datasets/adarshsng/lending-club-loan-data-csv


## Project Overview

This project builds a credit risk scorecard model to estimate the Probability of Default (PD) for borrowers using the Lending Club loan dataset.

The objective is to replicate a bank-style credit risk modelling pipeline used in financial institutions for loan underwriting and credit scoring.

## The workflow follows an industry-standard methodology:

Data cleaning and preprocessing

Data leakage removal

Feature selection

Monotonic binning

Weight of Evidence (WOE) transformation

Information Value (IV) variable selection

Logistic regression PD modelling

Model performance evaluation (AUC, Gini, KS)

Scorecard scaling

Risk band creation

Population Stability Index (PSI) monitoring

The final output is a credit score and risk band classification for each borrower.

## Dataset

The project uses the LendingClub Loan Dataset, a widely used dataset in credit risk modelling research.

## Dataset characteristics:

Peer-to-peer lending data

Loan performance information

Borrower financial characteristics

Loan repayment outcomes

Target variable:

Loan Status	Target
Fully Paid	0
Charged Off	1
Default	1

The target represents whether the borrower defaulted on the loan.

## Project Workflow
1. Data Preprocessing

Steps performed:

Filter loans with valid status

Create binary default target

Remove data leakage variables

Remove features with >70% missing values

Train / validation split (70 / 30 stratified)

2. Missing Value Treatment

Handling strategy:

Numeric variables

Imputed with median

Categorical variables

Imputed with "Missing" category

3. Feature Selection

Initial feature filtering was performed using Random Forest feature importance.

Model used:

Random Forest

300 trees

Maximum depth = 10

Minimum leaf samples = 50

Top 30 most important variables were selected.

4. Multicollinearity Removal

Highly correlated numerical variables were removed using:

Correlation threshold:

|Correlation| > 0.7

This helps improve model stability and interpretability.

5. Monotonic Binning

Numerical variables were binned using quantile binning with monotonic constraint.

Spearman rank correlation was used to ensure:

Monotonic relationship between variable and default rate

Stability of credit scorecards

6. Weight of Evidence (WOE)

All selected variables were transformed using WOE encoding.

Benefits of WOE transformation:

Handles categorical variables naturally

Creates monotonic relationship with default probability

Improves logistic regression interpretability

7. Information Value (IV) Feature Selection

Variables were selected based on Information Value.
Selection rule:IV >= 0.03
| IV | Predictive Power |
|------|-------------|
| <0.02 | Weak |
| 0.02–0.1 | Medium |
| 0.1–0.3 | Strong |
| >0.3| Very strong |


8. Logistic Regression PD Model

Final Probability of Default model built using:

Logistic Regression via Statsmodels

Advantages:

Interpretable coefficients

Industry standard for credit scoring

Regulatory friendly

Statistically insignificant variables were removed after the first model run.

Model Performance

Evaluation metrics:

| Metric | Description |
|------|-------------|
| AUC | Area Under ROC Curve |
| Gini | 2 × AUC − 1 |
| KS | Kolmogorov–Smirnov statistic |

ROC curve was used to evaluate discriminatory power of the model.

Credit Score Scaling

The model probabilities were converted to a credit score.

Scorecard parameters:

| Parameter | Value |
|----------|-------|
| Base Score | 600 |
| Base Odds | 50 |
| PDO (Points to Double Odds) | 50 |
| Factor | PDO / ln(2) |
| Offset | Base Score − Factor × ln(Base Odds) |

Score formula:

Score = Offset - Factor * log(odds)

This produces a traditional credit bureau style scorecard.

Risk Band Segmentation



Borrowers were segmented into **risk bands based on Probability of Default (PD)**.

| PD Range | Risk Band |
|---------|----------|
| 0 – 2% | Very Low |
| 2 – 5% | Low |
| 5 – 10% | Medium |
| 10 – 20% | High |
| > 20% | Very High |
Default rate increases monotonically across risk bands.

Model Monitoring

Population Stability Index (PSI) was used to check distribution stability between development and validation samples.

PSI interpretation:

PSI	Interpretation
<0.1	Stable
0.1 – 0.25	Moderate shift
>0.25	Significant shift
Visualizations

The project includes several visual diagnostics:

Target distribution

Default rate by variable

Monotonic bin plots

ROC curve

Score distribution

Risk band default rates

PSI distribution comparison
