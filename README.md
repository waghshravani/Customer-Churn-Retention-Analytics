# Customer Churn & Retention Analytics

*Understanding Customer Behavior, Churn Patterns & Retention Opportunities*

A data analytics project that studies customer churn in a subscription-type customer dataset (8,000 customers) and presents the results in an interactive dashboard, a Jupyter notebook and a written report.

![Dashboard preview](screenshots/dashboard_preview.png)

## Overview

The project looks at how churn is spread across different customer groups: subscription type, location, account age, spending, satisfaction score and support calls. The aim is to describe what the data shows and to point out segments that may be worth a closer look. It does not build a prediction model.

## Business Problem

A business that sells subscriptions loses revenue every time a customer leaves. Before deciding where to spend retention effort, it helps to know how many customers churn, which groups churn more, and whether any single factor stands out.

## Objectives

- Measure how many customers are retained and churned, and the overall churn rate
- Compare churn across subscription type, location, account age, spending, satisfaction score and support calls
- Check whether any numerical column has a strong relationship with churn
- Build an interactive dashboard where these views can be filtered
- Suggest retention actions that are supported by the observed patterns

## Dataset

| Item | Detail |
|---|---|
| File | `data/train.csv` |
| Rows | 8,000 customers |
| Columns | 17 |
| Target | `Churn` (0 = retained, 1 = churned) |

Columns: `Customer_ID`, `Age`, `Gender`, `Location`, `Subscription_Type`, `Account_Age_Months`, `Monthly_Spending`, `Total_Usage_Hours`, `Support_Calls`, `Late_Payments`, `Streaming_Usage`, `Discount_Used`, `Satisfaction_Score`, `Last_Interaction_Type`, `Complaint_Tickets`, `Promo_Opted_In`, `Churn`.

The dataset was provided for this project. Its original source and licence are not recorded in this repository, so the file is included for academic and learning use only. Only `train.csv` is used; no other file was used and no values were changed or generated.

## Data Quality

- Missing values: 0
- Duplicate rows: 0
- `Churn` contains only the values 0 and 1
- Numeric columns have sensible ranges (for example satisfaction score 1–10, account age 1–59 months), so no cleaning was needed

## Methodology

1. Load the CSV and check shape, data types, missing values and duplicates
2. Calculate the KPIs from the data
3. Create groups for account age (12-month bands) and monthly spending (Low, Medium, High, Very High)
4. Calculate the churn rate for each group (churned customers / customers in the group)
5. Calculate the correlation between each numeric column and `Churn`
6. Build the dashboard so that all of these numbers are recalculated in the browser when a filter changes

All results are observed patterns. The project does not test statistical significance and does not claim that any variable causes churn.

## Key KPIs

| KPI | Value |
|---|---|
| Total customers | 8,000 |
| Churned customers | 2,505 |
| Retained customers | 5,495 |
| Overall churn rate | 31.31% |
| Average monthly spending | 104.80 |
| Average usage hours | 254.33 |
| Average satisfaction score | 5.46 |

## Key Findings

| Segment | Churn rate |
|---|---|
| Subscription: Basic / Enterprise / Premium | 31.50% / 31.21% / 31.09% |
| Location: Florida (highest) | 32.37% |
| Location: Illinois (lowest) | 29.43% |
| Account age: 25–36 months / 37–48 months | 32.58% / 32.18% |
| Account age: 0–12 months | 30.03% |
| Spending: Low / Medium / High / Very High | 30.45% / 31.15% / 31.88% / 31.59% |
| Satisfaction score 10 (highest among scores) | 33.29% |
| Support calls = 5 (highest among call counts) | 34.10% |

- Roughly 1 in 3 customers has churned.
- Churn is spread widely. Most group-level rates sit between about 29% and 34%.
- Subscription types are almost the same.
- Correlations with churn are all between about -0.02 and +0.02 (for example Discount_Used 0.0204, Satisfaction_Score 0.0198, Late_Payments -0.0166, Promo_Opted_In -0.0172).
- Churned and retained customers are mixed together in the spending vs usage scatter plot.

**There is no strong standalone churn driver in this dataset.** The differences above are small, and they are observations, not evidence of cause.

## Business Recommendations

1. **Look at mid-lifecycle customers.** Accounts aged 25–48 months show slightly higher churn. Check whether a targeted retention offer is worth testing.
2. **Investigate geographic differences.** Florida is higher than Illinois. Find out whether pricing, service, competition or customer behaviour differ.
3. **Review support interactions.** Some support-call counts show higher churn. Look at the reasons for the calls and how they were resolved before drawing conclusions.
4. **Segment retention campaigns.** Combine location, subscription, account age, spending and engagement instead of using one strategy for everyone.
5. **Do deeper analysis.** See Future Scope below.

## Interactive Dashboard

The dashboard is a single HTML file (`dashboard/index.html`) that reads `data/train.csv` in the browser and draws the charts with Plotly.js. It has:

- KPI cards (total, churned, retained, churn rate, average spending, average satisfaction)
- Retained vs churned donut chart
- Churn rate by subscription type, location, account age, spending group, satisfaction score and support calls
- Monthly spending vs total usage scatter plot
- Correlation with churn
- Filters for gender, location, subscription type, account age group, spending group, satisfaction score, churn status, promo opt-in and last interaction type, plus a reset button
- Insights and recommendations that are generated from the currently filtered data
- A customer table with search and pagination

The KPIs and charts all recalculate when a filter changes. If a filter combination matches no customers, the page shows a message instead of empty charts.

## Technologies Used

- Python, pandas, NumPy, Matplotlib, Jupyter Notebook (analysis)
- HTML, CSS, JavaScript and Plotly.js (dashboard)
- Git and GitHub

## Project Structure

```
customer-churn-retention-analytics/
├── README.md
├── LICENSE
├── .gitignore
├── .gitattributes
├── requirements.txt
├── data/
│   └── train.csv
├── dashboard/
│   └── index.html
├── notebooks/
│   └── customer_churn_analysis.ipynb
├── report/
│   └── project_report.pdf
└── screenshots/
    └── dashboard_preview.png
```

## How to Run the Notebook

```bash
git clone https://github.com/<your-username>/customer-churn-retention-analytics.git
cd customer-churn-retention-analytics
pip install -r requirements.txt
jupyter notebook notebooks/customer_churn_analysis.ipynb
```

The notebook reads the data from `../data/train.csv`, so keep the folder structure as it is. The saved notebook already contains its outputs, so it can also be read directly on GitHub.

## How to Run the Dashboard

The dashboard needs an internet connection the first time because it loads Plotly.js from a CDN (jsDelivr, with unpkg as a backup).

**Option 1: VS Code (loads the data automatically)**
1. Open the project folder in VS Code.
2. Install the *Live Server* extension.
3. Right-click `dashboard/index.html` and choose *Open with Live Server*.

**Option 2: Python (loads the data automatically)**
```bash
python -m http.server 8000
```
Then open `http://localhost:8000/dashboard/`.

**Option 3: Open the file directly**
Double-click `dashboard/index.html`. Browsers block a page from reading a file on disk, so the data does not load by itself. Click *Choose train.csv* and select `data/train.csv`. The file is read in your browser and is not uploaded anywhere.

## GitHub Pages

The dashboard can be hosted for free:

1. Push the whole repository to GitHub (keep `data/` and `dashboard/` together).
2. Go to **Settings → Pages**.
3. Under *Build and deployment*, choose **Deploy from a branch**, select the `main` branch and the `/ (root)` folder, then save.
4. After a minute or two the dashboard is available at
   `https://<your-username>.github.io/customer-churn-retention-analytics/dashboard/`

The page loads `../data/train.csv`, which works on GitHub Pages because both folders are published from the repository root.

## Future Scope

These have **not** been done in this project:

- Customer segmentation (for example clustering)
- Classification models to predict churn
- Feature importance
- Cohort analysis
- Survival analysis
- Testing whether the differences between groups are statistically significant

## Limitations

- The data is one snapshot with no dates, so changes over time cannot be studied.
- The reasons why customers left are not recorded.
- Group differences are small and no significance testing was done, so some of them may be chance.
- Groups such as individual support-call counts are small (about 770 to 870 customers each).
- The analysis looks at one variable at a time (plus the filters in the dashboard); combined effects were not modelled.
- The dataset's original source is not documented here.

## License

This project is released under the MIT License (see `LICENSE`). The MIT License covers the code and written material. It does not change the terms of the original dataset.

Author: Shravani Wagh
