Stress Testing Methodology

Overview

This document outlines the end-to-end stress testing methodology used to assess the impact of adverse economic conditions on a banking loan portfolio.

The framework follows a structured approach consisting of:

Data Collection
Data Preparation
Macroeconomic Scenario Design
Probability of Default (PD) Modeling
Loss Given Default (LGD) Modeling
Exposure at Default (EAD) Assessment
Expected Credit Loss Estimation
Portfolio Impact Assessment
Regulatory and Management Reporting


Methodology Framework


Portfolio Data
      │
      ▼
Data Validation & Preparation
      │
      ▼
Macroeconomic Variables
      │
      ▼
Scenario Development
      │
      ▼
PD Modeling
      │
      ▼
LGD Modeling
      │
      ▼
EAD Calculation
      │
      ▼
Expected Loss Estimation
      │
      ▼
Stress Testing Results
      │
      ▼
Management & Regulatory Reporting


Step 1: Portfolio Data Collection

The first stage involves collecting relevant portfolio and customer-level information.

Customer Data
Customer ID
Customer Segment
Age
Occupation
Income
Geography
Loan Data
Loan ID
Loan Type
Outstanding Balance
Interest Rate
Loan Tenure
Product Type
Credit Data
Credit Score
Historical Delinquencies
Current DPD
Collection History


Step 2: Data Quality Assessment

Data quality is critical for reliable stress testing.

Validation checks are performed for:

Completeness
Missing Customer Records
Missing Loan Details
Missing Economic Factors
Accuracy
Invalid Balances
Incorrect Dates
Duplicate Records
Consistency
Product Mapping
Customer Classification
Risk Segment Assignment


Step 3: Macroeconomic Variable Selection
Stress testing relies on key economic drivers.

Economic Variables
GDP Growth
Inflation Rate
Unemployment Rate
Interest Rate
Housing Price Index
Consumer Confidence Index
These variables are selected based on their historical relationship with portfolio credit performance.


Step 4: Scenario Development
Three primary scenarios are considered.

Baseline Scenario
Represents expected future economic conditions.

Example:

GDP Growth = 6%
Inflation = 5%
Unemployment = 4%
Mild Stress Scenario
Represents a moderate economic slowdown.

Example:

GDP Growth = 3%
Inflation = 7%
Unemployment = 6%
Severe Stress Scenario
Represents a significant economic downturn.

Example:

GDP Growth = -2%
Inflation = 9%
Unemployment = 10%


Step 5: Probability of Default (PD) Estimation
PD represents the likelihood that a borrower will default within a specified period.

Modeling Techniques
Logistic Regression
Scorecard Models
Transition Matrix Models
Machine Learning Models (Future Enhancement)
Key Drivers
Credit Score
Debt-to-Income Ratio
Delinquency History
Macroeconomic Factors
Output
Customer-level and segment-level stressed PD estimates.

Step 6: Loss Given Default (LGD) Estimation
LGD represents the percentage of exposure expected to be lost if default occurs.

Influencing Factors
Collateral Value
Loan Type
Recovery Rates
Collection Efficiency
Example
Exposure = ₹10,00,000

Recovery = ₹6,00,000

LGD = (10,00,000 - 6,00,000)
/ 10,00,000

LGD = 40%


Step 7: Exposure at Default (EAD)

EAD represents the total amount outstanding at the time of default.

Examples
Outstanding Principal
Accrued Interest
Undrawn Commitments
Product Types
Mortgage Loans
Auto Loans
Credit Cards
SME Loans


Step 8: Expected Credit Loss Calculation
Expected Credit Loss (ECL) is calculated as:

ECL = PD × LGD × EAD
Where:

PD = Probability of Default
LGD = Loss Given Default
EAD = Exposure at Default
Relationship to IFRS 9 and CECL
Stress testing scenarios support:
IFRS 9
Forward Looking ECL Estimation
Scenario Weighted Provisioning
Lifetime Loss Forecasting
CECL
Lifetime Expected Loss Estimation
Forward Looking Economic Assessment
Credit Loss Forecasting


Step 9: Portfolio Impact Assessment

Expected loss estimates are aggregated across:

Customer Segments
Retail
SME
Corporate
Products
Mortgages
Personal Loans
Auto Loans
Credit Cards
Regions
Geographic Concentrations
Branch Networks


Step 10: Capital Impact Assessment
The framework evaluates:

Capital Adequacy Impact
Capital Buffer Requirements
Risk Appetite Alignment
This supports:

Basel III
ICAAP
CCAR Requirements
Internal Capital Planning


Step 11: Reporting and Visualization
Results are summarized through:

Management Reports
Portfolio Risk Summary
Segment Risk Assessment
Scenario Comparison
Regulatory Reports
Stress Testing Results
Capital Impact Analysis
Expected Loss Reporting
Dashboards
Portfolio Heat Maps
Risk Trending
Scenario Comparison Dashboards
Key Outputs
The framework generates:

Stressed PD Estimates
Stressed LGD Estimates
EAD Calculations
Expected Credit Loss Estimates
Portfolio Risk Assessments
Capital Impact Analysis
Management Recommendations


Assumptions
Key assumptions include:

Historical relationships remain broadly relevant.
Economic scenarios are plausible and internally consistent.
Portfolio structure remains largely unchanged during the forecasting horizon.
Recovery patterns remain reasonably stable.


Future Enhancements
Future enhancements may include:

Machine Learning Models
Monte Carlo Simulations
Climate Risk Stress Testing
Real-Time Monitoring
Azure AI Integration
Python Model Validation
Databricks Implementation

Conclusion
The Stress Testing Methodology provides a structured and repeatable framework for evaluating portfolio resilience under adverse economic conditions. By integrating macroeconomic scenarios with portfolio risk models, institutions can improve risk management, strengthen capital planning and support regulatory compliance requirements.
