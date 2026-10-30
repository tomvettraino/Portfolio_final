# Data Science Portfolio

Welcome to my data science portfolio. This repository contains projects exploring how data analysis, statistical modelling and machine learning can be applied to practical problems.

I am currently studying for an MSc in Data Science at the University of Sussex, having previously graduated with First-Class Honours in Psychology with Finance. My interests include predictive modelling, machine learning and using data to support evidence-based decision-making.

## Projects

### 1. UK Bike-Share Market Feasibility & Multi-Criteria Selection Engine

**Technologies:** Python, pandas, Matplotlib

A business-focused data analysis project exploring the commercial viability and expansion potential of a sustainable bike-share operator across 16 major UK metropolitan areas.

The project integrates six public datasets covering demographics, transport behaviour, cycling infrastructure, household income and weather. I developed a weighted Multi-Criteria Decision Analysis (MCDA) framework to normalise indicators and produce comparable market-attractiveness scores.

**Key areas:**
- Data cleaning, integration and geographic aggregation across different administrative boundaries.
- Feature engineering, including cycling infrastructure density and household car ownership.
- Min-max normalisation and weighted scoring.
- Ranking and comparing potential markets to support business expansion decisions.

[View project](./ReCycle%20Internship/)

### 2. Football Match Prediction Model

**Technologies:** Python, pandas, NumPy, scikit-learn, XGBoost, Matplotlib

An independent machine learning project developed to predict football match outcomes and scorelines across the English Premier League, Championship and League One.

The model uses historical match data and engineered team-performance features to train separate XGBoost regression models for home and away goals. I have explored rolling performance statistics, exponentially weighted averages and other features to capture changes in team performance over time.

Predictions are evaluated using chronological backtesting, with a scoreline probability grid used to translate predicted goals into likely match outcomes.

**Key areas:**
- Data preparation and feature engineering using historical football data.
- Separate regression models for home and away goals.
- Time-aware model validation and performance evaluation.
- Scoreline probability estimation and comparison against baseline approaches.
- Experimentation with alternative features and modelling approaches.

[View project](./Score%20predictions%20model/)

### 3. Waste Demand Forecasting

**Technologies:** Python, pandas, NumPy, scikit-learn, XGBoost, Matplotlib

A time-series forecasting project exploring whether historical waste collection data can be used to predict future demand across different waste streams.

The analysis uses monthly data and engineered temporal features, including lagged values, seasonal indicators and rolling averages. I compared Linear Regression, XGBoost and Multi-Layer Perceptron models, using chronological training and testing periods to evaluate their forecasting performance.

**Key areas:**
- Time-series data preparation and temporal feature engineering.
- Comparison of linear and machine learning models.
- Chronological model evaluation using MAE and RMSE.
- Assessment against a simple lag-based baseline.
- Hyperparameter experimentation and model comparison.

[View project](./Wastedemandmodel/)

---

## Skills & Tools

| Area | Tools and techniques |
|---|---|
| Programming | Python, R, SQL |
| Data analysis | pandas, NumPy, data cleaning, exploratory data analysis |
| Visualisation | Matplotlib, ggplot2 |
| Machine learning | scikit-learn, XGBoost, regression, model evaluation |
| Feature engineering | Lagged variables, rolling statistics, normalisation, engineered ratios |
| Evaluation | MAE, RMSE, chronological validation, baseline comparisons |
| Decision analysis | Weighted scoring, Multi-Criteria Decision Analysis |

## Approach

Across these projects, I aim to follow a structured approach to working with data:

1. **Understand the problem:** Identify the question the analysis or model is intended to address.
2. **Prepare the data:** Clean, integrate and explore the available data before modelling.
3. **Develop features:** Identify and construct relevant variables to improve analysis or predictive performance.
4. **Build and evaluate:** Compare appropriate methods and assess performance using suitable evaluation techniques.
5. **Interpret results:** Consider what the findings mean in the context of the original problem and where the analysis could be improved.

I use these projects to develop my practical skills in data science, explore different modelling approaches and understand how analytical methods can inform real-world decisions.

