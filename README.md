# Walmart Future Sales Forecasting

> A store-level time-series forecasting project for improving retail demand planning, inventory decisions, and operational readiness.

![Python](https://img.shields.io/badge/Python-3.9%2B-3776AB?logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-data%20analysis-150458?logo=pandas&logoColor=white)
![Prophet](https://img.shields.io/badge/Forecasting-Prophet-2E75B6)
![Status](https://img.shields.io/badge/Project-Completed-2EA44F)

## Overview

Retail demand changes from week to week and differs significantly across stores. This project analyzes historical Walmart sales data and forecasts the next **12 weeks of sales for each store**.

The project combines:

- Data quality assessment and preparation
- Exploratory data analysis
- Store-level performance comparison
- Holiday and economic-factor analysis
- Prophet time-series forecasting
- Time-series cross-validation
- Feature selection and hyperparameter tuning
- Final evaluation using WMAPE, RMSE, and MAE

The final model achieved an overall **4.17% WMAPE**, demonstrating a strong forecasting result for the evaluated 12-week horizon.

## Business Problem

Walmart operates stores in different markets, and each store has its own demand profile. Poor demand estimates can lead to:

- Overstocking and higher holding costs
- Stockouts and lost sales
- Inefficient staffing
- Poor promotional and inventory planning

The project answers two primary questions:

1. What factors and patterns are associated with weekly sales?
2. Can sales be forecast accurately for every store over the next 12 weeks?

## Dataset

The project uses `Walmart.csv`, containing **6,435 observations**, **45 stores**, and **143 weekly observations per store** from **February 2010 through October 2012**.

| Column | Description |
|---|---|
| `Store` | Store identifier |
| `Date` | Weekly sales date |
| `Weekly_Sales` | Sales for the store during the week |
| `Holiday_Flag` | Whether the week was a holiday week |
| `Temperature` | Recorded temperature |
| `Fuel_Price` | Regional fuel price |
| `CPI` | Consumer Price Index |
| `Unemployment` | Regional unemployment rate |

### Data quality checks

- No missing values were found.
- No duplicate rows were found.
- All observations occur on Friday.
- Every store has the same 143-week date range.
- Dates were converted to a consistent datetime format.

## Key Findings

### Store performance

| Rank | Store | Historical sales |
|---:|---:|---:|
| 1 | Store 20 | Approximately $301.40M |
| 2 | Store 4 | Approximately $299.54M |
| 3 | Store 14 | Approximately $289.00M |
| Lowest | Store 33 | Approximately $37.16M |

The difference between the highest- and lowest-performing stores is approximately **$264.24M**, representing a **711.08% difference relative to Store 33**.

### Holiday demand

Holiday weeks have higher average sales:

- Holiday weeks: approximately **$1.123M**
- Non-holiday weeks: approximately **$1.041M**

The notebook identifies notable sales spikes around the weeks of **26 November** and **31 December**.

### Economic and environmental relationships

- Store 38 and Store 44 show the strongest negative store-level associations between unemployment and sales.
- Temperature has a negative slope in the sales regression analysis.
- CPI relationships vary substantially by store; Store 36 shows the strongest negative association, while Stores 38 and 44 show strong positive associations.

> These relationships are descriptive correlations and should not be interpreted as proof of causation.

## Forecasting Approach

The project uses **Prophet**, fitted independently for each store.

### Forecast design

1. Sort observations chronologically for each store.
2. Reserve the final 12 weeks as an untouched test set.
3. Train the model on the earlier observations.
4. Compare candidate feature configurations using time-series cross-validation.
5. Tune Prophet parameters.
6. Refit and evaluate the final model across all 45 stores.

The final configuration uses the historical sales series as the core forecasting signal, with yearly seasonality enabled and weekly and daily seasonality disabled.

### Hyperparameter search

The notebook evaluates combinations of:

- `changepoint_prior_scale`: `0.001`, `0.01`, `0.05`, `0.1`
- `seasonality_prior_scale`: `0.1`, `1`, `5`, `10`
- `seasonality_mode`: `additive`, `multiplicative`

## Model Performance

| Metric | Baseline average | Final result |
|---|---:|---:|
| WMAPE | 6.55% | **4.17%** |
| RMSE | 92,018.39 | **62,451.76** |
| MAE | 72,216.33 | **42,849.87** |

The improvement indicates that the validation and tuning stages reduced forecast error compared with the initial baseline.

## Project Workflow

```text
Load data
   ↓
Assess data quality
   ↓
Clean and standardize dates
   ↓
Explore sales, holidays, weather, and economic variables
   ↓
Split each store into training and 12-week testing periods
   ↓
Fit baseline Prophet models
   ↓
Feature selection and time-series cross-validation
   ↓
Hyperparameter tuning
   ↓
Evaluate final forecasts
```

## Repository Contents

| File | Purpose |
|---|---|
| `forcast.ipynb` | Main analysis, visualization, modeling, and evaluation notebook |
| `Walmart.csv` | Historical Walmart weekly-sales dataset |
| `Submission Report Draft Revised.docx` | Detailed project submission report |
| `Submission Report Draft.pdf` | PDF version of the submission report |
| `Capstone Project - Walmart.pdf` | Project brief/reference document |

## Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/Rajwork99/Future-Sales-Forecasting.git
cd Future-Sales-Forecasting
```

### 2. Create an environment

```bash
python -m venv .venv
```

Activate it:

**Windows PowerShell**

```powershell
.venv\Scripts\Activate.ps1
```

**macOS/Linux**

```bash
source .venv/bin/activate
```

### 3. Install dependencies

```bash
pip install pandas numpy matplotlib seaborn plotly scikit-learn prophet jupyter
```

### 4. Run the notebook

```bash
jupyter notebook forcast.ipynb
```

Run the notebook cells from top to bottom to reproduce the data preparation, exploratory analysis, model selection, tuning, and evaluation workflow.

## Practical Applications

The forecasts can support:

- Store-specific inventory replenishment
- Holiday stock planning
- Workforce and staffing decisions
- Regional demand monitoring
- Sales target setting
- Exception reporting for unusual store behavior

## Limitations and Future Improvements

The current dataset does not include promotions, markdowns, inventory levels, competitor activity, local events, or store metadata. Future versions could:

- Add known-future promotional and holiday regressors.
- Produce prediction intervals for risk-aware planning.
- Compare Prophet with SARIMA, gradient boosting, and ensemble models.
- Retrain on a rolling schedule as new data arrives.
- Monitor model drift and store-specific forecast error.
- Extend the analysis beyond the current October 2012 endpoint.

## Conclusion

This project demonstrates a complete retail forecasting workflow, from data validation and business insight generation to tuned store-level forecasting. The results show that sales behavior differs meaningfully between stores and that a structured time-series approach can provide useful 12-week demand forecasts for operational planning.

## Author

**Rajwork99**

If this project is useful, consider giving the repository a star.
