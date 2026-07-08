# Unemployment Analysis (India, 2019-2020)

## Project Overview

India's labour market went through a historic shock in 2020 when nationwide
COVID-19 lockdowns disrupted employment across every state and union territory.
This project analyzes monthly unemployment data to understand how the pandemic
affected employment, whether unemployment follows any seasonal pattern
independent of COVID, and what these trends suggest for economic or social
policy responses.

This is an exploratory and analytical project, not a predictive modelling one —
the goal is to extract and communicate clear, evidence-backed insights rather
than train a model.

---

## Dataset

**Source:** CodeAlpha Internship — Task 2

| File | Rows | Description |
|------|------|-------------|
| Unemployment_data.csv | 768 | Monthly unemployment estimates by Indian state/UT, split by Rural/Urban area |

> Raw data files are not tracked in this repository.
> Download from the source and place in `data/raw/`.

**Note:** A second candidate dataset (`Unemployment_Rate_upto_11_2020.csv`) was
evaluated and set aside during data understanding — it disagreed with this
dataset on 305 of 306 overlapping Region+Date records, indicating a different
data source/methodology rather than duplication. Using two disagreeing sources
risked contaminating the analysis, so this project proceeds with a single,
internally consistent dataset.

**Key columns:**
- `Region` — Indian state or union territory
- `Date` — month-end reporting date (May 2019 – June 2020)
- `Estimated Unemployment Rate (%)` — primary variable of interest
- `Estimated Employed` — count of employed individuals
- `Estimated Labour Participation Rate (%)` — % of working-age population in the labour force
- `Area` — Rural / Urban (each Region+Date is reported separately for both)

---

## Project Structure

```
Unemployment_Analysis/
│
├── data/
│   ├── raw/                        # Original CSV (not tracked by git)
│   └── processed/                  # Cleaned outputs (not tracked by git)
│
├── docs/
│   └── project_journal.md
│
├── images/                         # Charts saved from notebooks
│
├── notebooks/
│   ├── 01_data_understanding.ipynb
│   ├── 02_data_cleaning.ipynb
│   ├── 03_eda.ipynb
│   ├── 04_covid_impact_analysis.ipynb
│   ├── 05_seasonal_trends.ipynb
│   └── 06_insights_and_recommendations.ipynb
│
├── requirements.txt
├── .gitignore
└── README.md

```

> Notebook breakdown may adjust as the project develops — this structure
> reflects the task brief's four asks (cleaning/exploration, COVID impact,
> seasonal trends, policy insights) mapped roughly one-to-one onto notebooks.

---

## Key Findings from Data Understanding

- 768 rows x 7 columns; 28 fully empty rows to be dropped in cleaning (740 valid rows)
- Date range: May 2019 – June 2020 (14 months)
- Row granularity is Region + Date + Area (Rural/Urban) — not unique by Region+Date alone
- Frequency column has zero variance ("Monthly" for every row) — likely dropped in feature engineering
- 27 flagged "duplicate" rows are entirely the empty rows already identified — no genuine duplicate content exists
- **Clear COVID-19 signal found:** the three most extreme values in the dataset (highest unemployment, lowest employed count, lowest participation rate) all fall on April 2020 — Puducherry saw 74-77% unemployment across both Rural and Urban areas simultaneously, consistent with India's nationwide lockdown (began March 24, 2020)
- A second unemployment dataset was evaluated and set aside (see Dataset note above)

---

## Data Cleaning Summary

- 28 fully empty rows dropped (768 → 740 rows)
- 0 genuine duplicate rows — the 27 initially flagged were the empty rows themselves
- Frequency column had inconsistent whitespace masking a constant value; cleaned and dropped (zero variance)
- Date converted from string to datetime64
- 20 of 28 regions have complete reporting; 8 have gaps, most notably Chandigarh (Rural entirely absent) and Sikkim (Rural mostly absent). Verified with a systematic groupby check rather than manual inspection. Gaps preserved as-is rather than imputed, to protect the integrity of the COVID-impact analysis
- Final dataset: 740 rows x 6 columns (7 → 6 after dropping the constant Frequency column), saved to `data/processed/unemployment_clean.csv`

---

## Exploratory Data Analysis

*To be updated after EDA notebook is complete.*

---

## COVID-19 Impact Analysis

*To be updated after the COVID impact notebook is complete.*

---

## Seasonal Trends

*To be updated after the seasonal trends notebook is complete.*

---

## Insights and Policy Recommendations

*To be updated after the final notebook is complete.*

---

## Workflow

| Phase | Notebook | Status |
|-------|----------|--------|
| Data understanding | 01_data_understanding.ipynb | ✅ Complete |
| Data cleaning | 02_data_cleaning.ipynb | ⏳ Pending |
| Exploratory analysis | 03_eda.ipynb | ⏳ Pending |
| COVID-19 impact analysis | 04_covid_impact_analysis.ipynb | ⏳ Pending |
| Seasonal trends | 05_seasonal_trends.ipynb | ⏳ Pending |
| Insights & recommendations | 06_insights_and_recommendations.ipynb | ⏳ Pending |

---

## Tools and Libraries

Python · pandas · numpy · matplotlib · seaborn

---

## Author

Built as part of the CodeAlpha Data Science Internship (June–July 2026).

[LinkedIn](https://www.linkedin.com/in/osita-jerry) ·
[GitHub](https://github.com/ossydimma/CodeAlpha_UnemploymentAnalysis)