## 2026-07-07

Completed data understanding for Unemployment Analysis (01_data_understanding.ipynb).

### Dataset decision
Two candidate datasets existed. Found 305/306 overlapping Region+Date records
disagreed on every metric — near-total disagreement, not simple duplication,
pointing to different methodology between the two sources. Chose to work with
a single dataset (Unemployment_data.csv), which also has the longer time span
(May 2019-June 2020) needed for the task's seasonal-trend and COVID-impact
requirements, including real pre-COVID baseline months.

### Data quality
768 rows x 7 columns; 28 fully empty rows found, will be dropped in cleaning.
Column headers and Frequency values both had inconsistent leading whitespace —
fixed with str.strip(). Frequency turned out to have zero variance ("Monthly"
for every row) once cleaned.

Checked for duplicates: 27 flagged rows turned out to be entirely the empty
rows already found via null check (pandas treats all-NaN rows as duplicates of
each other) — no genuine duplicate content in the dataset.

Row granularity is Region + Date + Area, not just Region + Date — each region
reports separate Rural/Urban estimates per month. Region row counts range from
12 to 28 — some states/UTs (Chandigarh, Sikkim, Jammu & Kashmir, Goa) have
noticeably incomplete reporting, flagged for cleaning.

### COVID-19 finding
The three most extreme values in the dataset (max unemployment rate 76.74%,
min employed count 49,420, min participation rate 13.33%) all fall on April
2020. Puducherry shows 74-77% unemployment in both Rural and Urban areas
simultaneously that month — a genuine severe lockdown effect, not a data
error. This gives the project a strong, concrete COVID-impact story to build
on in EDA, directly addressing the task brief.

### Next step
Data cleaning — drop empty rows, drop or retain Frequency (zero variance),
investigate incomplete regions.

## 2026-07-08

Completed data cleaning for Unemployment Analysis (02_data_cleaning.ipynb).

### Nulls and duplicates
28 fully empty rows dropped (768 → 740). Confirmed 0 genuine duplicates — the
27 flagged by .duplicated() were entirely the empty rows.

### Frequency
Confirmed the 'Monthly'/' Monthly' whitespace split (381/359) from data
understanding was real. Cleaned and dropped — constant value, zero variance.

### Date
Converted to datetime64 with dayfirst=True, required for all later time-based
analysis.

### Region completeness
Initially checked region-by-region by eye, which produced two inaccurate notes
(Sikkim's gap was attributed entirely to Rural, and Chandigarh's Urban shortfall
was missed). Replaced with a systematic groupby(['Region','Area']).size() check
across all 28 regions at once — good reminder that manual inspection of printed
row dumps doesn't scale and is easy to miscount. Found 8 regions with gaps,
Chandigarh (Rural entirely absent) and Sikkim (Rural mostly absent) being the
most significant. Decided to preserve gaps rather than impute, given how
central the COVID-period months are to this project's purpose.

### Final dataset
740 rows x 6 columns — down from 7, after dropping Frequency once confirmed
constant. 0 missing values. Saved to ../data/processed/unemployment_clean.csv.

### Next step
EDA — trends over time, Rural vs Urban comparison, regional distribution.