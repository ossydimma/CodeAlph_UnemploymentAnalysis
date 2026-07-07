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