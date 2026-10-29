# Banking Stress Testing Framework

## Overview

This project demonstrates an end-to-end Banking Stress Testing Framework designed to assess the resilience of a loan portfolio under adverse economic conditions.

The solution simulates macroeconomic shocks and estimates their impact on portfolio credit quality, Probability of Default (PD), Loss Given Default (LGD), Exposure at Default (EAD), Expected Credit Loss (ECL), and overall capital requirements.

The project showcases a practical banking risk analytics use case using SAS, SQL and Python techniques commonly employed by financial institutions for internal risk management and regulatory reporting.

---

## Business Problem

Banks operate in uncertain economic environments where adverse events such as recessions, rising unemployment, high inflation, or financial crises can significantly impact borrower repayment behavior.

Regulators require banks to assess:

- Portfolio resilience under stress
- Capital adequacy under adverse conditions
- Changes in credit risk exposure
- Potential increase in expected credit losses
- Risk concentration across customer segments

Without a structured stress testing framework, institutions may underestimate future risks and maintain insufficient capital buffers.

---

## Project Objectives

The primary objectives of this project are:

- Evaluate portfolio performance under stress scenarios
- Quantify the impact of macroeconomic shocks
- Estimate stressed Probability of Default (PD)
- Estimate stressed Loss Given Default (LGD)
- Calculate stressed Expected Credit Loss (ECL)
- Support risk management decision-making
- Demonstrate an end-to-end banking analytics workflow

---

## Key Skills Demonstrated

- Credit Risk Analytics
- Stress Testing
- Retail Credit Portfolio Modelling
- SAS Development
- Python Development
- SAS to Python Migration
- Forecasting & Scenario Analysis
- Expected Credit Loss (ECL)
- Probability of Default (PD)
- Loss Given Default (LGD)
- Regulatory Reporting
- Data Validation Frameworks
- Portfolio Risk Segmentation
- Power BI Reporting
  
---


## End-to-End Architecture

```text
Portfolio Input Data
        ↓

Input Validation
        ↓

Data Preparation
        ↓

Risk Segmentation
        ↓

Scenario Creation
        ↓

Scenario Application
        ↓

PD Stress Impact
        ↓

LGD Stress Impact
        ↓

ECL Calculation
        ↓

Portfolio Stress Reporting
        ↓

Power BI Export Layer
        ↓

Power BI Dashboard
```

---

## Current Project Status

### Version 1 Completed ✅

#### SAS Backend

✅ Input Validation

✅ Data Preparation

✅ Risk Segmentation

✅ Scenario Creation

✅ Scenario Application

✅ PD Stress Impact

✅ LGD Stress Impact

✅ ECL Calculation

✅ Portfolio Stress Reporting

---

#### Power BI Dashboard

✅ Executive Overview

✅ Scenario Analysis

✅ Product Risk Analysis

✅ Risk Segment Analysis

---

#### Reporting Layer

✅ ecl_summary.csv

✅ product_ecl_summary.csv

✅ segment_ecl_summary.csv

---

### Upcoming Enhancements

#### Version 2

- PostgreSQL Migration
- SQL-Based Processing

#### Version 3

- Python Migration
- Pandas Validation Framework

#### Version 4

- Azure AI Integration
- Microsoft Fabric
- Automated Risk Insights

---

## Regulatory Context

Stress testing has become an essential risk management practice for banks worldwide.

Common regulatory frameworks include:

- Basel III
- IFRS 9
- CCAR (Comprehensive Capital Analysis and Review)
- ICAAP (Internal Capital Adequacy Assessment Process)
- CECL (Current Expected Credit Loss)
- Stress Testing Guidelines issued by Central Banks

The framework developed in this project reflects common industry practices used for risk assessment and capital planning.

---

## Business Scenario

A retail bank maintains a portfolio containing:

- Mortgage Loans
- Personal Loans
- Auto Loans
- Credit Cards
- SME Loans
- Corporate Loans

Management wants to understand how portfolio performance will change under adverse economic conditions and whether existing capital reserves are sufficient to absorb future losses.

---

## Analytics & Development Tools

### Backend

- SAS
- PROC SQL
- DATA Step

### Reporting Layer

- CSV Export Files

### Visualization

- Power BI

### Planned Technology Stack

- PostgreSQL
- Python
- Microsoft Fabric
- Azure AI

---

## Business Rules/Techniques

- Scenario Analysis
- Sensitivity Analysis
- Portfolio Segmentation
- Risk Quantification

---

## Key Risk Metrics

The framework evaluates the impact of economic stress on:

- Probability of Default (PD)
- Loss Given Default (LGD)
- Exposure at Default (EAD)
- Expected Credit Loss (ECL)
- Non-Performing Assets (NPA)
- Delinquency Rates
- Capital Adequacy Impact
- Portfolio Concentration Risk

---

## Business KPIs Monitored

- Total Portfolio Exposure
- Base ECL
- Stressed ECL
- Incremental Stress Loss
- Default Rate
- Delinquency Ratio
- Risk Segment Distribution
- Product-wise Risk Contribution
- Regulatory Capital Impact

---

## Sample Data Sources

The project uses representative banking datasets including:

- Customer Master Data
- Loan Portfolio Data
- Repayment History
- Delinquency Information
- Credit Scores
- Macroeconomic Indicators

Sample files and layouts are available within the data folder.

---

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

### Step 4: Probability of Default Modelling

Historical relationships between economic conditions and borrower defaults are analysed.

Techniques:

- Logistic Regression
- Scorecards
- Time Series Analysis
- Statistical Risk Models

---

## Power BI Dashboard

The Power BI Dashboard provides executive reporting and visualization for the Banking Stress Testing Framework.

### Dashboard Pages

#### Executive Overview

- Total Exposure
- Base ECL
- Stressed ECL
- Incremental Stress Loss

#### Scenario Analysis

- Baseline
- Mild Stress
- Severe Stress

#### Product Risk Analysis

- Corporate Loan
- SME Loan
- Mortgage
- Auto Loan
- Personal Loan
- Credit Card

#### Risk Segment Analysis

- High Risk
- Medium Risk
- Low Risk

### Dashboard Screenshots

See:

```text
PowerBI/Dashboard_Screenshots/
```

Available Screenshots:

- Executive_Overview.png
- Scenario_Analysis.png
- Product_Risk_Analysis.png
- Risk_Segment_Analysis.png

---

## Repository Structure

```text
data/
docs/
output/
PowerBI/
python/
sas/
screenshots/
sql/
README.md
```

---

## Author

Senior Analytics Professional

### Skills

- SAS
- SQL
- Banking Analytics
- Risk Analytics
- Business Analysis
- SAS
- SQL
- Python
- Stress Testing
- Data Validation
- Reconciliation
- Statistical Modeling
- Power BI
- Azure AI (Learning Path)
- Generative AI (Learning Path)

---

## Disclaimer

This repository is intended for educational, portfolio, and demonstration purposes.

The datasets, scenarios, assumptions, and outputs are designed to illustrate banking stress testing concepts and may not represent actual banking portfolios or regulatory submissions.
