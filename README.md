# Zillow Home Price Prediction

This project analyzes Zillow home price and housing-market data to explore price trends and build predictive models for future home prices by city and state.

## Project goals

- Clean and combine Zillow housing datasets into a modeling-ready dataset.
- Explore regional housing trends across cities and states.
- Build regression models to predict home prices.
- Build time-series forecasts for future city and state home prices using `skforecast`.

## Key Findings

- **XGBoost was the strongest supervised model**, achieving an **R² of 0.885** and an **RMSE of approximately $29.4K** on the test set. High and low end values cut off however making it unrealistic for forecasting
- **Ridge Regression was selected for recursive forecasting**, with a test **R² of 0.847** and **RMSE of approximately $34.0K**, because it worked cleanly with the `skforecast` recursive forecasting workflow.
- **Lasso Regression performed slightly better than Ridge by R²**, reaching an approximate **R² of 0.865**, but had a higher RMSE of about **$40.8K**.
- **Historical monthly pricing trends were highly useful for forecasting**, with the recursive forecast model using **24 monthly lags** to project future city- and state-level home prices.
- **Data preparation had a major impact on model reliability**, especially fixing city/state coverage, preserving `mean_sales_price`, and standardizing monthly datetime frequency for time-series modeling.

## Model Performance

| Model | R² Score | RMSE | Notes |
|---|---:|---:|---|
| Linear Regression | 0.985 | Not recorded | Training score only; likely overfit and not directly comparable to test metrics |
| Ridge Regression | 0.847 | $33,987 | Regularized linear model used later in recursive forecasting |
| Lasso Regression | 0.865 | $40,816 | Best tested alpha was approximately 18,000 based on R² comparison |
| XGBoost Regressor | 0.885 | $29,381 | Best-performing supervised regression model based on test R² and RMSE |


## Repository structure

```text
.
├── notebooks/
│   ├── 01_data_cleaning.ipynb
│   ├── 02_EDA.ipynb
│   └── 03_preprocess_modeling.ipynb
├── data/
│   ├── raw/          # raw Zillow source files; not committed if large
│   └── processed/    # cleaned outputs; not committed if large
├── reports/
│   ├── images/       # screenshots from EDA
│   ├── home_price_prediction_pres.pptx
│   └── home_price_prediction_report.pdf
├── README.md
├── requirements.txt
└── .gitignore
```

## Notebook workflow

Run the notebooks in order:

1. **01_data_cleaning.ipynb**  
   Loads and cleans Zillow source files, reshapes monthly data, standardizes dates, and exports the processed dataset.

2. **02_EDA.ipynb**  
   Explores price distributions, location-level trends, missingness, and visual patterns in the cleaned dataset.

3. **03_preprocess_modeling.ipynb**  
   Prepares modeling data, trains regression models, evaluates performance, and forecasts future prices with `ForecasterRecursive`.

## Setup

Create and activate a virtual environment:

```bash
python -m venv .venv
source .venv/bin/activate      # macOS/Linux
# .venv\Scripts\activate       # Windows
```

Install dependencies:

```bash
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

If you are using VS Code notebooks, select the `.venv` interpreter/kernel before running the notebooks.

## Key libraries

- pandas, numpy
- matplotlib, seaborn, plotly
- scikit-learn
- statsmodels
- xgboost
- skforecast
- category-encoders
- geopandas

## Notes

- `.ipynb_checkpoints/` and `.venv/` are ignored by Git.
- Large raw and processed data files should usually stay out of GitHub unless they are small enough and appropriate to share.
- Forecasting notebooks require a regular monthly `DatetimeIndex`; the modeling notebook sets monthly frequency before fitting `skforecast` models.
