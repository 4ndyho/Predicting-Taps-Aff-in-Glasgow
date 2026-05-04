# Predicting-Taps-Aff-in-Glasgow
This project explores whether Glasgow’s iconic **“taps aff”** behaviour can be predicted **without using any weather data**.

Instead of relying on temperature, rain, or forecast feeds, the model uses **non-weather proxy signals** that may reflect how people plan and behave on warmer days, including:

- **Google Trends** search interest (e.g. *bbq*, *suncream*)
- **Glasgow mobility data** (cycling and pedestrian counts)
- **GB electricity demand**
- **Calendar features** such as weekends, holidays, and time of year

The goal was to test whether these low-cost, public signals could provide a useful **day-ahead prediction** of whether a day would be a “taps aff” day.

---

## Project Objective

The project investigates four main questions:

1. Can non-weather proxies predict “taps aff” with useful accuracy on unseen data?
2. Which signals contribute most to prediction performance?
3. Is performance stable across different years and calendar periods?
4. What decision threshold gives the best precision–recall trade-off for planning use?

---

## Approach

The problem was framed as a **daily binary classification task**.

### Data sources
- Google Trends data for terms such as **“bbq”** and **“suncream”**
- **Glasgow City Council / uSmart** cycling and pedestrian counts
- **NESO** Great Britain electricity demand data
- Calendar variables including:
  - weekends
  - UK holidays
  - month / seasonal effects

### Target label
The target variable (“taps aff” or not) was generated programmatically using a published rule based on the original *Taps Aff* logic, but **weather data was used only to create the label**, not as a model input.

### Feature engineering
The feature set focused on **recent movement and momentum**, rather than raw levels. This included:

- lagged features
- week-over-week differences
- rolling statistics
- exponential moving averages
- calendar and seasonality features

This was designed to capture behavioural shifts while avoiding data leakage.

### Models tested
- Majority class baseline
- Logistic Regression
- Random Forest
- Histogram-based Gradient Boosting (HGB)

Models were tuned using **expanding time-series splits** on **2018–2023**, with **2024+ held out** as a true out-of-time test set.

---

## Key Result

The strongest model was **Histogram-based Gradient Boosting**, which achieved strong hold-out performance using only proxy signals.

At the selected operating threshold on the 2024+ hold-out set, the model achieved approximately:

- **Accuracy:** 0.856
- **Precision:** 0.746
- **Recall:** 0.707
- **F1 Score:** 0.726

This suggests that non-weather behavioural proxies can provide meaningful predictive value, even without direct meteorological inputs.

---

## Why this project is interesting

This project is a fun local case study, but it also demonstrates a broader analytical idea:

> Useful predictions can sometimes be made from indirect, public, behavioural signals when direct measurements are unavailable, delayed, or intentionally excluded.

The work also highlights important data science principles, including:

- proxy variable design
- leakage-safe evaluation
- time-series validation
- calibration and threshold selection
- interpretability using feature importance / SHAP

---

## Tech Stack

- **Python**
- pandas
- NumPy
- scikit-learn
- pytrends
- meteostat
- matplotlib / visualisation tools
