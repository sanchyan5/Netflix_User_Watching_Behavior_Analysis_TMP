# Netflix User Watching Behavior: Exploratory Data Analysis

## Problem Statement
A streaming platform loses about a quarter of its subscribers. Using a 50,000-user behavioural dataset (20 features), this project asks **who churns, why, and when**, and turns the answers into a sized retention plan.

**Approach:** data-quality checks, feature engineering (watch time, engagement index, tenure cohorts, watch intensity, recency bands), then univariate, bivariate, multivariate and tenure/cohort analysis, statistical hypothesis tests with effect sizes, K-Means behavioural segmentation, a logistic-regression driver model, and scenario-based revenue sizing of five recommendations.

> **Data note:** the original CSV was not available when this repo was built, so `data/netflix_user_behavior.csv` is a **simulated file with the exact schema from the brief**, created by `src/generate_dataset.py`. To use the real data, overwrite that CSV (same column names) and re-run the notebook: every number in the notebook's narrative is computed live. The figures below describe the bundled file.

## Summary of Findings
- **Churn baseline:** 23.9% of users (11,939) have churned, about $128,595/month or 22.4% of monthly recurring revenue.
- **Tier matters:** Basic churns at 29.8% vs 16.7% for Premium (1.8x). Premium is 22% of users but 32% of revenue.
- **Watch time is the clearest lever:** churn falls from 50% for users under 30 min/day to 7% for 150+ min/day, dropping below the platform average at 60-90 min/day.
- **Segments:** 34% of users are *Disengaged* and churn at 37.9%, versus 7.4% for *Super-fans* (about 5x).
- **Devices:** Smart-TV users complete 64.3% of content vs 52.5% on mobile; mobile churn is 28.6% vs 20.9% on Smart TV. Users on 3+ devices churn less (20.9% vs 30.4%).
- **Recommendations as a loyalty signal:** users who click recommendations and finish content ("Satisfied advocates") churn at 13.2%; "Detached" users churn at 36.3%.
- **Lifecycle:** churn peaks in month 1 and around the month-12 renewal; new accounts churn 26.7% vs 23.3% for veterans, and watch time falls 28% from the newest to the oldest accounts (engagement fatigue).
- **Recency is the best early-warning signal:** churn rises from 7% (active) to 89% (30+ days inactive). 4,362 still-paying users ($51,347/month) are already 14+ days inactive.
- **Statistics:** all six hypothesis tests are significant, but effect sizes show behaviour (watch time, engagement) matters far more than tenure (Cramer's V about 0.02). The driver model reaches AUC 0.71 (behaviour and plan only) and 0.87 with recency.

## Business Recommendations (illustrative annual revenue protected)
| Priority | Initiative | Users saved | Annual revenue |
|---|---|---|---|
| 1 | Personalised discovery for Disengaged users | 1,588 | $218,614 |
| 2 | Early-warning win-back (14+ days inactive) | 654 | $92,425 |
| 3 | Mobile completion & cast-to-TV nudges | 504 | $69,636 |
| 4 | Basic-tier retention & upgrade path | 638 | $55,937 |
| 5 | New-subscriber onboarding (first 6 months) | 83 | $11,496 |

Scenario assumptions: 50% of the churn gap to a benchmark group closes (15% win-back for inactive users); initiatives overlap, so values are not additive. Associations are not proof of causation, so validate with A/B tests.

## Repository Structure
```
Netflix_User_Behavior_EDA_TMP/
├── README.md
├── requirements.txt
├── data/netflix_user_behavior.csv        # dataset (50,000 rows x 20 columns)
├── notebooks/
│   ├── Netflix_User_Behavior_EDA.ipynb   # full analysis with outputs
│   └── netflix_eda_script.py             # same code as one plain script
├── presentation/
│   ├── Netflix_Churn_Insights.pptx       # 12-slide pitch deck
│   └── metrics.json                      # headline numbers exported by the notebook
├── images/                               # 20 charts saved by the notebook
└── src/                                  # code that generated everything
    ├── generate_dataset.py               # builds the CSV
    ├── cells_*.py, cellkit.py            # notebook cell definitions
    ├── build_notebook.py                 # executes cells and writes the .ipynb
    ├── build_deck.js                     # builds the .pptx (pptxgenjs)
    ├── build_readme.py                   # writes this README
    └── export_notebook_code.py
```

## How to Run
```bash
pip install -r requirements.txt
jupyter notebook notebooks/Netflix_User_Behavior_EDA.ipynb     # run all cells
# optional: regenerate every artifact
python src/generate_dataset.py && python src/build_notebook.py
node src/build_deck.js && python src/build_readme.py
```
