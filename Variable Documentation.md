# Lending Club Dataset – Variable Documentation

## Overview

This document describes the key variables used in the credit risk modelling project built using the Lending Club loan dataset.
## Also Refer LCDataDictionary.xlsx
The variables represent different aspects of a borrower's financial profile, credit behaviour, and loan characteristics.

For clarity, the variables are grouped into the following categories:

- Loan Characteristics  
- Borrower Demographics  
- Employment & Income  
- Credit History  
- Debt & Financial Obligations  
- Delinquency Indicators  
- Target Variable  
- Model Derived Variables  

---

# 1. Loan Characteristics

These variables describe the **structure and terms of the loan**.

| Variable | Description |
|----------|-------------|
| loan_amnt | Total loan amount requested by the borrower |
| funded_amnt | Total amount funded for the loan |
| funded_amnt_inv | Portion of the loan funded by investors |
| term | Loan duration (typically 36 or 60 months) |
| int_rate | Interest rate charged on the loan |
| installment | Monthly payment amount |
| grade | Credit grade assigned by the platform |
| sub_grade | Sub-category within the credit grade |
| purpose | Purpose of the loan (debt consolidation, credit card, etc.) |
| application_type | Indicates whether the loan is individual or joint |

These variables influence **loan pricing and repayment structure**.

---

# 2. Borrower Demographics

These variables represent the **borrower's personal and geographic profile**.

| Variable | Description |
|----------|-------------|
| home_ownership | Housing status (Rent, Own, Mortgage, Other) |
| addr_state | Borrower's state of residence |
| zip_code | Borrower's ZIP code (partially masked) |

These variables can capture **regional or housing-related risk patterns**.

---

# 3. Employment and Income

These variables measure the **borrower's financial capacity to repay the loan**.

| Variable | Description |
|----------|-------------|
| annual_inc | Borrower's annual income |
| emp_length | Number of years of employment |
| verification_status | Indicates whether the borrower's income was verified |

These variables are important indicators of **repayment ability**.

---

# 4. Credit History Variables

These variables describe the **borrower's historical credit behaviour**.

| Variable | Description |
|----------|-------------|
| earliest_cr_line | Date of the borrower's earliest credit line |
| open_acc | Number of currently open credit accounts |
| total_acc | Total number of credit accounts |
| pub_rec | Number of derogatory public records |
| revol_bal | Total revolving credit balance |
| revol_util | Revolving credit utilization rate |

These variables reflect **long-term credit discipline**.

---

# 5. Debt and Financial Obligations

These variables capture the **borrower's current financial leverage**.

| Variable | Description |
|----------|-------------|
| dti | Debt-to-income ratio |
| mort_acc | Number of mortgage accounts |
| total_bal_ex_mort | Total balance excluding mortgage |
| total_bc_limit | Total credit card limit |

These features measure the **borrower's financial burden**.

---

# 6. Delinquency and Risk Indicators

These variables capture **recent credit stress or repayment issues**.

| Variable | Description |
|----------|-------------|
| delinq_2yrs | Number of delinquencies in the past two years |
| inq_last_6mths | Number of credit inquiries in the past six months |
| collections_12_mths_ex_med | Collections reported in the past 12 months |
| acc_now_delinq | Number of accounts currently delinquent |

These are often **strong predictors of default risk**.

---

# 7. Target Variable

The model predicts whether a borrower **defaults on a loan**.

| Variable | Description |
|----------|-------------|
| loan_status | Current status of the loan (Fully Paid, Charged Off, Default, etc.) |
| target | Binary default indicator derived from loan_status |

### Target Definition

| Loan Status | Target |
|-------------|--------|
| Fully Paid | 0 |
| Charged Off | 1 |
| Default | 1 |

The target variable represents **borrower default behaviour**.

---

# 8. Model Derived Variables

During model development, several **derived variables** are created.

| Variable | Description |
|----------|-------------|
| WOE variables | Weight of Evidence transformed features |
| PD | Predicted Probability of Default |
| score | Credit score calculated from PD |
| risk_band | Risk segment assigned based on PD |

These variables are used to **translate model outputs into practical credit scores and risk segments**.

---

# Variable Categories Summary

| Category | Purpose |
|----------|---------|
| Loan Characteristics | Defines loan structure and pricing |
| Borrower Demographics | Describes borrower location and housing status |
| Employment & Income | Measures repayment capacity |
| Credit History | Captures long-term borrowing behaviour |
| Debt & Financial Obligations | Measures financial leverage |
| Delinquency Indicators | Detects recent credit stress |
| Model Variables | Represent model outputs and transformations |

---

# Role of Variables in Credit Risk Modelling

Credit risk models combine variables from multiple categories to estimate **Probability of Default (PD)**.

Common high-impact predictors include:

- Debt-to-income ratio  
- Credit utilization rate  
- Delinquency history  
- Loan grade  
- Income level  

These variables allow lenders to **assess borrower risk and make lending decisions**.

---

# Notes

Some variables were excluded during model development due to:

- Data leakage  
- High missing values  
- High correlation with other variables  
- Low predictive power  

The final model uses only **statistically significant and predictive variables**.
