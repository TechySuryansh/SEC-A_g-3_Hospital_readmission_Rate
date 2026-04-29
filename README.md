# Hospital Readmission Rate Analysis — DVA Capstone 2

[![Python](https://img.shields.io/badge/Python-3.9+-blue.svg)](https://www.python.org/)
[![Pandas](https://img.shields.io/badge/Pandas-Latest-orange.svg)](https://pandas.pydata.org/)
[![Sector](https://img.shields.io/badge/Sector-Healthcare-red.svg)](https://en.wikipedia.org/wiki/Healthcare_industry)
[![Team](https://img.shields.io/badge/Team-SEC--A_Group--3-green.svg)](#contribution-matrix)

## Project Overview

| Field | Details |
| :--- | :--- |
| **Project Title** | Hospital Readmission Rate Analysis (Diabetes 130-US Hospitals) |
| **Sector** | Healthcare / Clinical Analytics |
| **Team ID** | SEC-A, Group 3 |
| **Section** | Section A |
| **Institute** | Newton School of Technology |
| **Dataset** | Diabetes 130-US Hospitals (1999–2008) |

---


..
## Business Problem
High hospital readmission rates are a critical challenge for healthcare providers, leading to increased costs and suboptimal patient outcomes. For diabetic patients, managing clinical factors, medications, and comorbidities is essential to reducing 30-day readmissions—a key performance indicator for hospital quality.

### Core Business Question
> **Which clinical and demographic factors are the strongest predictors of 30-day hospital readmission for diabetic patients?**

### Decision Supported
This analysis provides healthcare administrators and clinical teams with evidence-based insights to identify high-risk patient segments. By understanding the impact of medication changes, prior hospital utilization, and diagnostic categories, hospitals can implement targeted post-discharge interventions to reduce readmission volume.

---

## Dataset Details

| Attribute | Details |
| :--- | :--- |
| **Source** | UCI Machine Learning Repository / Kaggle |
| **Row Count** | 101,766 (raw) → 71,515 (cleaned, first encounter) |
| **Column Count** | 50 (raw) → 36 (final processed) |
| **Time Period** | 1999–2008 (130 US Hospitals) |
| **Target Variable** | `readmitted` (<30 days, >30 days, No) |

---

## Folder Structure

```text
SEC-A_g-3_Hospital_readmission_Rate/
├── data/
│   ├── raw/                 # Original Raw Data
│   │   └── RAW_HEALTHCARE.csv
│   └── processed/           # Transformation Outputs
│       ├── cleaned_healthcare.csv
│       └── kpi_summary.csv
├── notebooks/
│   ├── 01_extraction.ipynb  # Data loading & profiling
│   ├── 02_cleaning.ipynb    # ETL pipeline & feature engineering
│   ├── 03_eda.ipynb         # Visual exploration & clinical trends
│   ├── 04_statistical_analysis.ipynb # T-tests, ANOVA & Regression
│   └── 05_final_load_prep.ipynb     # Tableau readiness audit
├── scripts/
│   └── etl_pipeline.py      # Automated pipeline orchestrator
├── reports/
│   ├── project_report.pdf   # Detailed technical analysis
│   └── presentation.pdf     # Executive summary deck
├── docs/
│   └── data_dictionary.md   # Comprehensive column metadata
├── requirements.txt
└── README.md
```

---

## Setup Instructions

1. **Create virtual environment**
   ```bash
   python3 -m venv venv
   ```

2. **Activate Environment**
   - **macOS / Linux:** `source venv/bin/activate`
   - **Windows:** `.\venv\Scripts\activate`

3. **Install Dependencies**
   ```bash
   pip install -r requirements.txt
   ```

4. **Run the Automated Pipeline**
   ```bash
   python scripts/etl_pipeline.py
   ```

---

## Data Engineering — Cleaning & Transformation

The project implements a robust **Transform** phase in `02_cleaning.ipynb`:

| Step | Action | Rationale |
| :--- | :--- | :--- |
| **1** | Placeholder Fix | Replaced `?` with `NaN` for standardized handling. |
| **2** | Column Pruning | Dropped `weight` (97% null) and `payer_code` (40% null). |
| **3** | Variance Filter | Dropped 15 medications with >99% "No" variance. |
| **4** | Deduplication | Kept only the **first encounter** per patient to prevent data leakage. |
| **5** | Hierarchical Mapping | Converted IDs to labels for `admission_type` and `discharge_disposition`. |
| **6** | Feature Engineering | Created `age_midpoint`, `total_prior_visits`, and `readmitted_binary`. |

---

## KPI Framework

| KPI | Definition | Purpose |
| :--- | :--- | :--- |
| **30-Day Readmission Rate** | % of patients readmitted within 30 days | Primary clinical quality metric. |
| **Prior Utilisation Index** | Sum of outpatient, inpatient, and emergency visits | Predicts "High Utiliser" status. |
| **Medication Stability** | Count of medication dosage changes (`Up`/`Down`) | Analyzes treatment complexity vs. risk. |
| **Comorbidity Count** | Number of unique diagnosis categories per encounter | Measures patient complexity. |

---

## Key EDA & Statistical Insights
- **Utilisation Correlation**: Patients with high prior hospital visits show a **statistically significant** increase in readmission risk (p < 0.05).
- **Age Distribution**: The 60–80 age group represents the largest volume of diabetic encounters.
- **Diagnosis Impact**: Circulatory and Respiratory primary diagnoses correlate with higher early readmission rates.
- **Medication Change**: Dosage adjustments during stay are associated with higher readmission probability, indicating unstable patient status.

---

## Tech Stack
- **Languages**: Python (Pandas, Numpy, Scikit-learn, Scipy, Statsmodels)
- **Visualisation**: Matplotlib, Seaborn, Tableau Public
- **Project Mgmt**: GitHub, Jupyter Notebooks

---

## Contribution Matrix

| Team Member | Data Sourcing | ETL & Cleaning | EDA & Analysis | Statistical Analysis | Tableau Dashboard | Report Writing |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| **Suryansh Singh** | Owner | - | - | - | Owner | - |
| **Pankaj Baid** | Owner | Owner | Owner | - | - | - |
| **Pranjal Modi** | - | Owner | - | - | - | - |
| **Aditya Kishore Singh** | - | - | - | Owner | - | Owner |
| **Vaibhav Singh** | Owner | - | - | - | - | Owner |
| **Avishkar Meher** | Owner | - | - | - | - | - |

---

### 🔗 Quick Links
- **Team Portfolios:** [View Team Member Portfolios](./DVA-focused-Portfolio/team_portfolios.md)
- **Live Tableau Dashboard:** [View Dashboard](https://public.tableau.com/app/profile/suryansh.singh4549/viz/capstone1_17774803915970/Dashboard3?publish=yes)
- **Data Dictionary:** [docs/data_dictionary.md](./docs/data_dictionary.md)
- **Project Reports:** [reports/project_report.pdf](./reports/project_report.pdf)

**Team Lead:** Suryansh Singh  
**Date:** 29/04/2026
