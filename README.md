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

- Estimated Unemployment Rate is heavily right-skewed: mean 11.79%, median 8.35%, max 76.74% (Puducherry, April 2020)
- National unemployment rate held steady around 9-10% from May 2019 to February 2020, then spiked to 23-25% by April/May 2020 — a clear COVID-lockdown signature — before starting to recede by June 2020
- Urban areas show a consistently higher median unemployment rate than Rural (~10% vs ~6-7%), with Urban's extreme outliers (~76-77%) marginally exceeding Rural's (~74%)
- Regional unemployment rates vary substantially; some of the most extreme values come from regions with incomplete Rural/Urban reporting (Chandigarh, Sikkim, J&K, Goa), so extremes should be interpreted with that caveat
- Unemployment Rate correlates weakly negatively with Employed count (-0.22); Employed and Participation Rate show no meaningful correlation (0.01), reflecting that Employed is a population-scaled headcount while Participation Rate is population-independent
- National averages in this analysis are unweighted across regions of uneven reporting completeness — treated as directional trends, not precise national statistics

---

## COVID-19 Impact Analysis

### National picture
Unemployment held steady around 9-10% before COVID, then spiked to about 19.7%
during lockdown (March-May 2020) — roughly double. By June 2020, it had come
back down to 11.9%, still a bit above normal but clearly recovering.

### Hardest-hit regions
Puducherry, Jharkhand, and Tamil Nadu saw the biggest jumps (Puducherry: 1.6% → 76.7%; Jharkhand: 14.3% → 70.2%; Tamil Nadu: 2.8% → 53.2%). Puducherry and Tamil Nadu started from very low unemployment and were hit hardest in relative terms, while Jharkhand was already struggling before COVID and got worse on top of that. Chandigarh shows the smallest change, but its data is incomplete (see Data Cleaning Summary), so this likely reflects missing data rather than being unaffected.

### Rural vs Urban
Both areas rose by almost the same raw amount (Rural: +66.4 points, Urban: +65.9 points). But since Rural started from a lower baseline (8.1% vs Urban's 10.8%), the shock was proportionally bigger for Rural — its rate multiplied by about 9x versus Urban's 7x. COVID hit both areas by a similar real amount, but it was a bigger relative shock for Rural.

---

## Seasonal Trends

**Can we prove a real, repeating yearly pattern? No — and that's worth saying
plainly.** We only have about 14 months of data, so almost every calendar
month appears just once. Proving a true "season" needs the same month to
behave the same way across 2-3+ different years, which this dataset can't show.

**What we can honestly say:**
- Before COVID, the unemployment rate stayed fairly steady, roughly 9-10%, with only small normal ups and downs — no big swings
- The only large, sudden change in the whole dataset is the COVID spike in April/May 2020 (national average jumped to 23.6% in April), which is a one-time shock, not a seasonal pattern
- Looking at the data region by region, the COVID spike shows up across almost every region at the same time — confirming it was a nationwide shock, not something limited to a few places (matches the finding in COVID-19 Impact Analysis)

---

## Insights and Policy Recommendations

### Key Insights

1. **This was a genuine emergency, not a normal dip.** National unemployment roughly doubled during lockdown (9.5% → 19.7%), far beyond normal month-to-month movement.

2. **Different regions need different kinds of help.** Puducherry and Tamil Nadu were doing fine before COVID and crashed hard (1.6% → 76.7% and 2.8% → 53.2%) — they need fast-recovery support. Jharkhand was already struggling before COVID (14.3%) and got worse (70.2%) — it needs longer-term support, not just a COVID bounce-back.

3. **Rural areas had less cushion to absorb the shock.** Rural and Urban rose by nearly the same raw amount (+66.4 vs +65.9 points), but Rural's relative shock was bigger (~9x its normal rate vs ~7x for Urban) since it started from a lower baseline.

4. **No proven seasonal pattern — don't plan around one.** Only ~14 months of data means most calendar months appear once; the only large change in the dataset is the COVID shock itself, not a normal yearly cycle.

5. **Some regions' data is less trustworthy.** Chandigarh and a few others have incomplete reporting — their "smallest change" numbers likely reflect missing data, not being unaffected.

### Recommendations

- Treat the COVID period as an emergency response, not routine policy
- Prioritize relief spending toward Puducherry, Jharkhand, and Tamil Nadu first, with different support for sudden-crash regions vs. already-struggling ones
- Design rural-specific support rather than adapting urban programs
- Don't plan future policy around an assumed seasonal unemployment cycle
- Improve data collection completeness for smaller regions

### Limitations
Short time span (14 months, can't prove seasonality), incomplete reporting in
several regions, unweighted national/regional averages, and findings specific
to India's COVID period rather than generalizable to other countries or shocks.

---

## Workflow

| Phase | Notebook | Status |
|-------|----------|--------|
| Data understanding | 01_data_understanding.ipynb | ✅ Complete |
| Data cleaning | 02_data_cleaning.ipynb | ✅ Complete |
| Exploratory analysis | 03_eda.ipynb | ✅ Complete |
| COVID-19 impact analysis | 04_covid_impact_analysis.ipynb | ✅ Complete. |
| Seasonal trends | 05_seasonal_trends.ipynb | ✅ Complete |
| Insights & recommendations | 06_insights_and_recommendations.ipynb | ✅ Complete |

---

## Tools and Libraries

Python · pandas · numpy · matplotlib · seaborn

---

## Author

Built as part of the CodeAlpha Data Science Internship (June–July 2026).

[LinkedIn](https://www.linkedin.com/in/osita-jerry) ·
[GitHub](https://github.com/ossydimma/CodeAlpha_UnemploymentAnalysis)