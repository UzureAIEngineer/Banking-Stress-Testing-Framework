┌─────────────────────────────────────────────┐
│           Source Portfolio Data         
│
│  Loans | Customers | Accounts | History  
│
└─────────────────────┬───────────────────────┘

                      │
                      
                      ▼
                      
┌─────────────────────────────────────────────┐
│         01_Input_Validation.sas             │
│  Data Quality Checks                        │
│  Missing Values Validation                  │
│  Duplicate Detection                        │
│  Business Rule Verification                 │
└─────────────────────┬───────────────────────┘
                      │
                      ▼
┌─────────────────────────────────────────────┐
│         02_Data_Preparation.sas             │
│  Data Cleansing                             │
│  Standardization                            │
│  Feature Engineering                        │
│  Analytical Dataset Creation                │
└─────────────────────┬───────────────────────┘
                      │
                      ▼
┌─────────────────────────────────────────────┐
│         03_Risk_Segmentation.sas            │
│  Portfolio Segmentation                     │
│  Customer Risk Buckets                      │
│  Product Risk Classification                │
└─────────────────────┬───────────────────────┘
                      │
                      ▼
┌─────────────────────────────────────────────┐
│         04_Scenario_Creation.sas            │
│                                             │
│  Baseline Scenario                          │
│  Adverse Scenario                           │
│  Severe Stress Scenario                     │
└─────────────────────┬───────────────────────┘
                      │
                      ▼
┌─────────────────────────────────────────────┐
│        05_Scenario_Application.sas          │
│                                             │
│  Apply Macroeconomic Shocks                 │
│  GDP Stress                                 │
│  Unemployment Stress                        │
│  Interest Rate Stress                       │
└──────────────┬───────────────────┬──────────┘
               │                   │
               ▼                   ▼

┌───────────────────────┐    ┌───────────────────────┐
│ 06_PD_Stress_Impact   │    │ 07_LGD_Stress_Impact  │
│                       │    │                       │
│ Probability of Default│    │ Loss Given Default    │
│ Stress Calculations   │    │ Stress Calculations   │
└───────────┬───────────┘    └───────────┬───────────┘
            │                            │
            └──────────────┬─────────────┘
                           ▼

┌─────────────────────────────────────────────┐
│          08_ECL_Calculation.sas             │
│                                             │
│ ECL = PD × LGD × EAD                        │
│ Scenario-Level Expected Loss                │
└─────────────────────┬───────────────────────┘
                      │
                      ▼

┌─────────────────────────────────────────────┐
│     09_Portfolio_Stress_Reporting.sas       │
│                                             │
│ Portfolio Impact Analysis                   │
│ Stress Loss Reporting                       │
│ Management Reporting                        │
└─────────────────────┬───────────────────────┘
                      │
                      ▼

┌─────────────────────────────────────────────┐
│      10_Export_For_PowerBI.sas              │
│                                             │
│ Risk Dashboard Dataset                      │
│ Executive Reporting Output                  │
│ Power BI Consumption Layer                  │
└─────────────────────────────────────────────┘
