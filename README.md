# Banking Stress Testing Framework
 
## Overview
 
This project demonstrates an end-to-end Banking Stress Testing Framework designed to assess the resilience of a loan portfolio under adverse economic conditions. The solution simulates macroeconomic shocks and estimates their impact on portfolio credit quality, probability of default (PD), loss given default (LGD), exposure at default (EAD), expected loss (EL), and overall capital requirements.
 
The project showcases a practical banking risk analytics use case using SAS, SQL, Python, and statistical modeling techniques commonly employed by financial institutions for internal risk management and regulatory reporting.
 
---
 
## Business Problem
 
Banks operate in uncertain economic environments where adverse events such as recessions, rising unemployment, high inflation, or financial crises can significantly impact borrower repayment behavior.
 
Regulators require banks to assess:
 
- Portfolio resilience under stress
- Capital adequacy under adverse conditions
- Changes in credit risk exposure
- Potential increase in expected losses
- Risk concentration across customer segments
 
Without a structured stress testing framework, institutions may underestimate future risks and maintain insufficient capital buffers.
 
---
 
## Project Objectives
 
The primary objectives of this project are:
 
- Evaluate portfolio performance under stress scenarios.
- Quantify the impact of macroeconomic shocks.
- Estimate stressed Probability of Default (PD).
- Estimate stressed Loss Given Default (LGD).
- Calculate stressed Expected Loss (EL).
- Support risk management decision-making.
- Demonstrate an end-to-end banking analytics workflow.
 
---
 
## Regulatory Context
 
Stress testing has become an essential risk management practice for banks worldwide.
 
Common regulatory frameworks include:
 
- Basel III
- IFRS 9
- CCAR (Comprehensive Capital Analysis and Review)
- ICAAP (Internal Capital Adequacy Assessment Process)
- Stress Testing Guidelines issued by central banks
 
The framework developed in this project reflects common industry practices used for risk assessment and capital planning.
 
---
 
## Business Scenario
 
A retail bank maintains a portfolio containing:
 
- Mortgage Loans
- Personal Loans
- Auto Loans
- Credit Cards
- SME Loans
 
Management wants to understand how portfolio performance will change under adverse economic conditions and whether existing capital reserves are sufficient to absorb future losses.
 
---

### Analytics & Development Tools
 
- SAS (Primary Modeling and Reporting Platform)
- SQL (Data Extraction and Validation)
- Python (Future Enhancement and Model Comparison)
- Microsoft Excel
- Power BI
 
### Statistical Techniques
 
- Logistic Regression
- Scenario Analysis
- Sensitivity Analysis
- Portfolio Segmentation
- Risk Quantification


## Key Risk Metrics
 
The framework evaluates the impact of economic stress on:
 
- Probability of Default (PD)
- Loss Given Default (LGD)
- Exposure at Default (EAD)
- Expected Loss (EL)
- Non-Performing Assets (NPA)
- Delinquency Rates
- Capital Adequacy Impact
- Portfolio Concentration Risk

 
## Stress Testing Methodology
 
### Step 1: Portfolio Data Preparation
 
Customer and loan information are collected from multiple source systems.
 
Examples:
 
- Customer Demographics
- Loan Characteristics
- Payment History
- Delinquency Information
- Credit Scores
- Existing Exposure
 
---
 
### Step 2: Macroeconomic Variable Identification
 
Key economic drivers influencing default rates are identified.
 
Examples:
 
- GDP Growth Rate
- Inflation Rate
- Interest Rate
- Unemployment Rate
- Housing Price Index
- Consumer Confidence Index
 
---
 
### Step 3: Scenario Design
 
Multiple economic scenarios are developed.
 
#### Baseline Scenario
 
Normal economic environment.
 
#### Mild Stress Scenario
 
- GDP decreases moderately
- Inflation increases
- Slight unemployment rise
 
#### Severe Stress Scenario
 
- Significant GDP contraction
- Sharp unemployment increase
- Declining consumer income
- Widespread credit deterioration
 
---
 
### Step 4: Probability of Default Modeling
 
Historical relationships between economic conditions and borrower defaults are analyzed.
 
Techniques:
 
- Logistic Regression
- Scorecards
- Time Series Analysis
- Statistical Risk Models
