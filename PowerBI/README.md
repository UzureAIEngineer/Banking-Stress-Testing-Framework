# Power BI Dashboard

## Overview

This Power BI dashboard provides the visualization layer for the Banking Stress Testing Framework.

The dashboard consumes reporting datasets generated from the framework implementation layer, which may be SAS, SQL, or Python.

---

## Dashboard Objectives

The dashboard is designed to help stakeholders:

- Monitor portfolio exposure
- Compare stress testing scenarios
- Identify high-risk products
- Assess risk-segment concentration
- Support executive credit-risk review

---

## Data Flow

Portfolio Data
        ↓

Stress Testing Framework
(SAS / SQL / Python)

        ↓

Reporting Layer

- ecl_summary.csv
- product_ecl_summary.csv
- segment_ecl_summary.csv

        ↓

Power BI Dashboard

---

## Dashboard Pages

### 1. Executive Overview

Provides:

- Total Exposure
- Base ECL
- Stressed ECL
- Incremental Stress Loss

Features:

- Interactive Scenario Filtering
- Executive KPI Monitoring

---

### 2. Scenario Analysis

Compares portfolio losses across:

- Baseline
- Mild Stress
- Severe Stress

Purpose:

Evaluate portfolio sensitivity to macroeconomic stress.

---

### 3. Product Risk Analysis

Compares stressed losses across:

- Corporate Loan
- SME Loan
- Mortgage
- Auto Loan
- Personal Loan
- Credit Card

Purpose:

Identify product-level risk concentration.

---

### 4. Risk Segment Analysis

Compares losses across:

- High Risk
- Medium Risk
- Low Risk

Purpose:

Assess concentration of risk by customer segment.

---

## Dashboard Screenshots

See:

PowerBI/Dashboard_Screenshots/

for dashboard execution evidence and visual outputs.

---

## Version

Power BI Dashboard V1

Status: Completed
