# India Energy Risk Analytics

An end-to-end **Data Analyst portfolio project** that analyzes petroleum consumption, product trade, Brent crude oil prices, and synthetic shipment-risk data to understand energy supply and operational risk.

The project combines **Python/Pandas, exploratory data analysis, an LLM-based AI summary, and Power BI** into a single analytical workflow.

**Project report:** https://canva.link/o2li3v1bqmbrkic

---

## Business Problem

> **How can India reduce energy supply risk while minimizing crude oil import costs during geopolitical disruptions?**

The project uses available historical and synthetic datasets to identify patterns in energy consumption, trade concentration, crude-oil price movement, and shipment risk.

The analysis is descriptive and decision-support oriented. It does not attempt to predict future events or establish causal relationships that the data cannot support.

---

## Project Objectives

- Analyze petroleum consumption trends and product-level demand.
- Understand the composition of reported petroleum imports and exports.
- Analyze Brent crude oil price movement and its relationship with consumption.
- Identify shipment-risk and delay patterns.
- Generate an AI-assisted natural-language energy-risk summary from verified analytical outputs.
- Present the findings through an interactive Power BI dashboard.

---

## Dataset Overview

The project uses four main datasets:

| Dataset | Rows | Purpose |
|---|---:|---|
| `productconsumption` | 492 | Monthly petroleum product consumption |
| `product_import` | 312 | Monthly product imports and exports |
| `DCOILBRENTEU` | 1,305 | Daily Brent crude oil prices |
| `synthetic_oil_shipments_100k` | 100,000 | Synthetic shipment cost, delay, and risk data |

### Important data considerations

- The consumption dataset contains monthly observations from 2020–2023.
- The trade dataset covers 2025–2026 and contains both product-level and aggregate records.
- Aggregate trade records such as `TOTAL IMPORT`, `PRODUCT IMPORT*`, `NET IMPORT`, and `TOTAL PRODUCT EXPORT` were excluded from product-level analysis to avoid double counting.
- Brent data contains 41 unavailable price observations. These were retained as missing rather than artificially imputed.
- Shipment data is synthetic and is used for portfolio-level risk analysis rather than representing verified real-world shipment records.

---

## Technology Stack

- **Python**
- **Pandas**
- **Matplotlib**
- **SQLite** for data storage/ingestion
- **Jupyter Notebook** for EDA and AI analysis
- **OpenRouter API / LLM** for the AI-generated risk summary
- **Power BI** for interactive dashboards
- **Canva** for the final project report

---

## Project Workflow

```text
Raw Data
   ↓
Cleaning & Validation
   ↓
Data Ingestion
   ↓
Python / Pandas Transformation
   ↓
Exploratory Data Analysis
   ↓
EDA Insights
   ↓
AI Risk Summary
   ↓
Dashboard Data Preparation
   ↓
Power BI Dashboard
   ↓
Final Report & Documentation
```

---

## Repository Structure

```text
India-Energy--Risk-Analytics/
│
├── analysis/
│   ├── energy_eda.ipynb
│   ├── ai_risk_summary.ipynb
│   └── dashboard_data_prep.ipynb
│
├── dashboard/
│   ├── India-Energy-Risk-Analytics.pbix
│   └── data/
│       ├── monthly_consumption.csv
│       ├── consumption_by_product.csv
│       ├── trade_by_product.csv
│       ├── monthly_brent.csv
│       ├── shipment_risk.csv
│       ├── supplier_analysis.csv
│       ├── delay_distribution.csv
│       ├── shipment_summary.csv
│       └── ai_risk_summary.csv
│
├── ingestion/
│   └── energyimport.db
│
├── sql/
│   ├── energy_risk_summary.py
│   └── energy_analysis_queries.sql
│
├── docs/
├── README.md
└── ...
```

---

# Exploratory Data Analysis

## 1. Domestic Consumption

The consumption dataset contains **492 records** with no missing values in the analyzed columns.

Key findings:

- Total observed petroleum consumption: **715,048.32 TMT**.
- HSD is the largest consumption product with a **38.15%** share.
- MS accounts for **15.28%** and LPG for **13.41%**.
- HSD, MS, and LPG together account for **66.84%** of observed consumption.
- Monthly consumption increased over the observed period, with noticeable month-to-month variation.
- The highest observed monthly consumption was approximately **21,220 TMT**.

## 2. Imports and Exports

After excluding aggregate records, the product-level trade analysis produced:

- Reported product imports: **291,961.71 TMT**.
- Reported product exports: **61,427.48 TMT**.
- **Crude oil** is the largest product-level import category at **245,768.67 TMT**.
- **HSD** is the largest product-level export category at **27,318.07 TMT**.
- The import and export profiles are therefore concentrated in different product categories within the analyzed trade data.

## 3. Brent Crude Oil

- Available daily Brent prices: **1,264** observations.
- Average available daily Brent price: **$83.54/barrel**.
- Minimum: **$59.93/barrel**.
- Maximum: **$138.21/barrel**.
- **41** Brent price observations were unavailable and were retained as missing.
- During the actual overlapping period between monthly consumption and Brent data, the Pearson correlation was **0.057**, indicating a very weak linear relationship.

The correlation should not be interpreted as evidence of causation.

## 4. Shipment Risk and Delays

The shipment dataset contains **100,000 synthetic records**.

Risk distribution:

| Risk Level | Share |
|---|---:|
| Low | 77.93% |
| Medium | 18.92% |
| High | 3.16% |

Additional findings:

- **83.24%** of shipments experienced at least one day of delay.
- Overall average delay: **2.04 days**.
- Maximum observed delay: **13 days**.
- Average delay by risk level:
  - Low: **1.43 days**
  - Medium: **3.76 days**
  - High: **6.69 days**
- Supplier-country average delays were tightly clustered at approximately **2.01–2.07 days** in the analyzed data.

The shipment analysis shows an observed association between risk classification and delay duration, but it does not establish that risk classification causes delay.

---

# AI Integration

The project includes an AI-assisted **Energy Risk Summary**.

### Architecture

```text
Pandas Analysis
      ↓
Verified KPIs and Findings
      ↓
Structured `ai_input`
      ↓
OpenRouter LLM
      ↓
AI-generated Business Summary
      ↓
`ai_risk_summary.csv`
      ↓
Power BI
```

The LLM is **not** used to calculate the raw data. Numerical calculations and analytical metrics are produced in Python first. The verified outputs are then passed to the LLM to produce a concise business-oriented summary.

### AI guardrails

The prompt explicitly instructs the model to:

- use only verified project values;
- avoid inventing external facts or statistics;
- avoid unsupported causal claims;
- distinguish observations from implications;
- avoid presenting the project as a predictive model.

This keeps the AI component useful while maintaining analytical transparency.

---

# Power BI Dashboard

The Power BI report contains three pages.

## Page 1 — Energy Overview

Includes:

- Total Consumption KPI
- Total Imports KPI
- Total Exports KPI
- Average Monthly Brent Price KPI
- Delayed Shipments KPI
- Monthly petroleum consumption trend
- Consumption by product
- Monthly Brent price trend
- AI Energy Risk Summary

## Page 2 — Trade & Supply

Includes:

- Import vs Export quantity comparison
- Import quantity by product
- Export quantity by product
- Trade composition

## Page 3 — Shipment Risk

Includes:

- Shipment distribution by risk level
- Average shipment delay by risk level
- Shipment distribution by delay days
- Supplier risk overview

The Power BI report uses prepared analytical CSVs so that Python remains responsible for data preparation and analysis while Power BI focuses on visualization and reporting.

---

# Key Business Observations

1. **Consumption is concentrated in a small set of petroleum products.** HSD alone represents 38.15% of observed consumption, while HSD, MS, and LPG together represent 66.84%.

2. **Crude oil is the dominant product-level import category** in the analyzed trade dataset, while HSD is the largest product-level export category.

3. **Brent prices show substantial variation**, but the analyzed overlapping monthly data shows only a very weak linear relationship between Brent prices and total petroleum consumption (r = 0.057).

4. **Shipment risk and delay show a clear observed pattern** in the synthetic shipment data. High-risk shipments have a higher average delay than medium- and low-risk shipments.

5. **Supplier country alone does not show a large difference in average delay** in the analyzed shipment data, with country-level averages clustered around two days.

---

# Limitations

- The datasets cover different time periods, so they should not be treated as one fully synchronized historical dataset.
- The trade analysis is based on the available reported product-level records after aggregate records were removed.
- Missing Brent observations are retained as unavailable rather than estimated.
- The shipment dataset is synthetic and should not be interpreted as verified real-world shipment data.
- The Brent-consumption analysis uses correlation and does not establish causation.
- The project is designed for portfolio and analyst-learning purposes, not as an operational energy-security forecasting system.

---

# How to Run the Project

## 1. Python environment

Install the main dependencies:

```bash
pip install pandas matplotlib sqlalchemy requests
```

## 2. Database

The project uses:

```text
ingestion/energyimport.db
```

The notebooks connect to this database and load the required tables.

## 3. EDA

Open:

```text
analysis/energy_eda.ipynb
```

Run the notebook to reproduce the exploratory analysis and visualizations.

## 4. AI summary

Open:

```text
analysis/ai_risk_summary.ipynb
```

An OpenRouter API key is required for the LLM component. The key should be stored in an environment variable and **must not be committed to GitHub**.

Example on Windows:

```cmd
setx OPENROUTER_API_KEY "YOUR_API_KEY"
```

Then restart the Jupyter process so the environment variable is available.

## 5. Dashboard data

Open:

```text
analysis/dashboard_data_prep.ipynb
```

This generates the dashboard-ready CSV files under:

```text
dashboard/data/
```

## 6. Power BI

Open:

```text
dashboard/India-Energy-Risk-Analytics.pbix
```

Load/refresh the prepared CSV sources to view the interactive dashboard.

---

# Portfolio Skills Demonstrated

- Data cleaning and validation
- Missing-data handling
- Data quality checks
- Data ingestion with SQLite
- Data transformation with Pandas
- GroupBy and aggregation
- Time-series analysis
- Product-level analysis
- Trade analysis
- Correlation analysis
- Operational risk analysis
- Dashboard data preparation
- Power BI dashboard development
- Basic DAX measure creation
- LLM/API integration
- Prompt design and AI output validation
- Business-oriented insight communication

---

# Project Outcome

This project demonstrates an end-to-end workflow in which **Python performs data preparation and analysis, an LLM converts verified analytical outputs into a business-readable risk summary, and Power BI presents the results through an interactive dashboard**.

The focus is on producing a project that is explainable at an entry-level Data Analyst interview while still demonstrating practical integration of analytics, BI, and AI.
