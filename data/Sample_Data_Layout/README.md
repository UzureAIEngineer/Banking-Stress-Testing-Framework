# Sample Data Layout

## Overview

This document illustrates the sample data structures used within the Banking Stress Testing Framework.

The datasets are representative examples designed to demonstrate portfolio-level stress testing, Probability of Default (PD), Loss Given Default (LGD), Exposure at Default (EAD), and Expected Credit Loss (ECL) calculations.

---

# Portfolio Data Layout

## Portfolio_Master

| Column Name | Data Type | Description |
|------------|-----------|-------------|
| Customer_ID | Numeric | Unique customer identifier |
| Loan_ID | Numeric | Unique loan identifier |
| Customer_Segment | Character | Retail, SME, Corporate |
| Product_Type | Character | Mortgage, Personal Loan, Auto Loan, Credit Card, SME Loan |
| Outstanding_Balance | Numeric | Current outstanding balance |
| Credit_Score | Numeric | Latest available customer credit score |
| Previous_Credit_Score | Numeric | Previous reporting period credit score |
| Current_DPD | Numeric | Current Days Past Due |
| Previous_DPD | Numeric | Previous reporting period DPD |
| Interest_Rate | Numeric | Loan interest rate |
| Loan_Term_Months | Numeric | Original loan tenure |
| Remaining_Term_Months | Numeric | Remaining loan tenure |
| Collateral_Value | Numeric | Current collateral value |
| Region | Character | Geographic region |
| Reporting_Date | Date | Reporting snapshot date |

---

# Sample Records

| Customer_ID | Loan_ID | Product_Type | Outstanding_Balance | Credit_Score | Current_DPD |
|------------|----------|-------------|--------------------|-------------|------------|
| 1001 | 50001 | Mortgage | 2500000 | 790 | 0 |
| 1002 | 50002 | Personal Loan | 450000 | 710 | 15 |
| 1003 | 50003 | Credit Card | 120000 | 640 | 45 |
| 1004 | 50004 | SME Loan | 5000000 | 620 | 75 |

---

# Macroeconomic Factors Layout

## Economic_Indicators

| Column Name | Data Type | Description |
|------------|-----------|-------------|
| Scenario_Name | Character | Baseline, Mild Stress, Severe Stress |
| GDP_Growth_Rate | Numeric | GDP growth percentage |
| Inflation_Rate | Numeric | CPI inflation percentage |
| Unemployment_Rate | Numeric | Unemployment percentage |
| Benchmark_Interest_Rate | Numeric | Policy interest rate |
| Housing_Price_Index | Numeric | Housing market index |
| Consumer_Confidence_Index | Numeric | Consumer confidence measure |
| Scenario_Date | Date | Scenario reporting date |

---

# Example Economic Scenarios

| Scenario | GDP | Inflation | Unemployment | Interest Rate |
|-----------|------|-----------|-------------|--------------|
| Baseline | 6.0% | 5.0% | 4.0% | 6.5% |
| Mild Stress | 3.0% | 7.0% | 6.0% | 8.0% |
| Severe Stress | -2.0% | 9.0% | 10.0% | 10.0% |

---

# Probability of Default Inputs

The PD model uses:

- Credit Score
- Current DPD
- Customer Segment
- Product Type
- GDP Growth Rate
- Unemployment Rate
- Interest Rate

---

# Loss Given Default Inputs

The LGD model uses:

- Product Type
- Outstanding Balance
- Collateral Value
- Recovery Rate
- Current DPD
- Scenario Factors

---

# Exposure at Default Inputs

The EAD calculation uses:

- Outstanding Principal
- Accrued Interest
- Undrawn Commitments
- Credit Conversion Factors

---

# Expected Credit Loss Calculation

Expected Credit Loss (ECL) is calculated as:

```text
ECL = PD × LGD × EAD
```

Where:

- PD = Probability of Default
- LGD = Loss Given Default
- EAD = Exposure at Default

---

# Data Quality Rules

The framework performs validation checks for:

## Mandatory Fields

- Customer_ID
- Loan_ID
- Product_Type
- Outstanding_Balance

## Credit Score Validation

- Current score retained when available
- Missing scores imputed using most recent available historical score
- Records flagged when score history is unavailable

## DPD Validation

- Recalculate DPD when due date information exists
- Historical DPD used when appropriate
- Data quality exceptions flagged for investigation

---

# Data Refresh Frequency

| Dataset | Frequency |
|----------|-----------|
| Portfolio Data | Monthly |
| Credit Scores | Monthly |
| DPD Data | Daily / Monthly |
| Economic Indicators | Quarterly |
| Stress Testing Results | Quarterly |

---

# Expected Outputs

The datasets support generation of:

- Stressed PD Estimates
- Stressed LGD Estimates
- EAD Calculations
- Expected Credit Loss Reports
- Product-Level Risk Analysis
- Segment-Level Risk Analysis
- Capital Impact Assessments
- Management Dashboards

---

# Conclusion

This sample data layout provides the foundation for stress testing, credit risk modeling, expected loss estimation, and regulatory reporting within the Banking Stress Testing Framework.
`
