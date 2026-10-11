# Kuala Lumpur & Selangor Rental Market Analysis

**Academic Course Project — ADS1001, Monash University Malaysia**

This repository is a portfolio revision of a university group project analysing rental properties in Kuala Lumpur and Selangor.

My original responsibilities included data cleaning, missing-value handling, notebook integration and presenting the data-cleaning section. The portfolio revision reorganises the project into a single reproducible workflow and improves the modelling methodology.

## Project Questions

1. How do rental-price levels differ between Kuala Lumpur and Selangor?
2. How much predictive value does location alone provide?
3. How much does prediction improve when physical property attributes are added?

## Dataset

The original course notebook loaded **19,990 listings** and exported **18,299 cleaned records** after imputation, duplicate removal and basic range validation.

The original notebook did not document a public dataset URL, so this repository does not invent one. `data/cleaned_dataset.csv` is the cleaned dataset used by this portfolio notebook.

For modelling, a city-specific 99th-percentile filter on monthly rent and size is used to reduce the influence of extreme records. This retains **18,014 rows (98.4%)** and does not overwrite the source CSV.

## Key Results

| Model | Test R² | RMSE | MAE |
|---|---:|---:|---:|
| Mean Baseline | -0.001 | 736.7 | 530.9 |
| Area-only Linear Regression | 0.330 | 602.6 | 434.9 |
| Unified Linear Regression | 0.667 | 425.1 | 296.4 |
| Ridge Regression | 0.667 | 424.9 | 296.4 |
| **Random Forest** | **0.688** | **411.5** | **277.7** |

### Market findings

- Median monthly rent in the modelling subset:
  - **Kuala Lumpur: 1,600**
  - **Selangor: 1,200**
- Kuala Lumpur also shows a wider rental-price distribution.
- Location alone explains some variation, but adding size, rooms, bathrooms, parking, property type and furnishing substantially improves prediction.
- Random Forest performs best on the held-out test set.

### Random Forest feature-group importance

Approximate share of model importance:

- Furnishing: **24.9%**
- Area: **19.4%**
- Size: **16.6%**
- Property type: **14.5%**
- City: **10.5%**
- Bathroom: **6.3%**
- Rooms: **4.2%**
- Parking: **3.7%**

These are predictive importance measures, not causal effects.

## Methodological Improvements in the Portfolio Revision

The original course model converted `area` into a numerical rank and fitted a one-variable linear regression. This portfolio revision:

- treats area and city as categorical variables using one-hot encoding;
- separates training and test data;
- uses a transparent extreme-value review;
- compares a baseline, Linear Regression, Ridge Regression and Random Forest;
- evaluates models with **R², RMSE and MAE**;
- compares location-only prediction with a unified feature model.

## Repository Structure

```text
KL-Selangor-Rental-Market-Analysis/
├── README.md
├── rental_market_portfolio.ipynb
├── requirements.txt
├── .gitignore
└── data/
    ├── README.md
    └── cleaned_dataset.csv
```

## How to Run

```bash
pip install -r requirements.txt
jupyter notebook rental_market_portfolio.ipynb
```

Run the notebook from the repository root so it can find `data/cleaned_dataset.csv`.

## Tools

Python, Pandas, NumPy, Matplotlib, scikit-learn, Jupyter Notebook

## Limitations

- The repository starts from the cleaned dataset exported by the original course project rather than the raw source file.
- The public source URL was not recorded in the original notebook.
- The 99th-percentile modelling filter may remove legitimate luxury listings together with anomalous records.
- The dataset has no suitable listing-date field for chronological validation.
- Model feature importance should not be interpreted as causality.
