# Waste Demand Forecasting

An exploratory machine-learning project investigating whether historical monthly waste-collection data can be used to forecast the tonnage of different waste streams across boroughs and community districts.

The notebook focuses on forecasting three collection categories:

- Refuse
- Paper
- Mixed glass and plastic (MGP)

The work is intended to explore how historical collection patterns, geographic information and seasonality can support operational planning. The notebook compares several modelling approaches and evaluates them against a simple previous-month baseline.

## Project status

**Current stage: exploratory modelling and retrospective evaluation**

The current notebook is a development and evaluation notebook. It is not a production forecasting pipeline or a live operational tool. It expects a local input file named `data.csv`, which is not included in this repository folder.

## Data and scope

The notebook reads a local file:

```python
df = pd.read_csv("data.csv")
```

It parses the `MONTH` column using the format `YYYY / MM` and restricts the modelling data to January 2015–December 2019.

The columns selected for the modelling dataset are:

- `MONTH`
- `BOROUGH`
- `COMMUNITYDISTRICT`
- `REFUSETONSCOLLECTED`
- `PAPERTONSCOLLECTED`
- `MGPTONSCOLLECTED`
- `RESORGANICSTONS`

`RESORGANICSTONS` is retained in the initial data selection but excluded from the current feature-engineering and modelling workflow.

**Data provenance:** the notebook does not document the original publisher or download URL for `data.csv`. Add the source, licence and any required attribution here once confirmed. The raw data file is deliberately not bundled with this project.

## Feature engineering

The data is ordered chronologically within each borough and community district. For each of the three modelled waste streams, the notebook creates:

- A one-month lag (`lag1`).
- A twelve-month lag (`lag12`).
- A three-month rolling mean (`roll3`), calculated using prior months only.

It also derives the calendar month number, year and quarter. Rows without the required twelve-month lag history are removed before modelling.

The lagged and rolling features are intended to represent recent collection levels and annual seasonality without using the current month's target value as an input.

## Modelling approach

The notebook trains separate regression models for refuse, paper and MGP tonnage.

### XGBoost

The initial models use `XGBRegressor` with:

- `enable_categorical=True`
- `tree_method="hist"`
- `n_estimators=200`
- `max_depth=5`
- `learning_rate=0.05`
- `random_state=42`

The refuse model is also evaluated using a reduced feature set and time-aware hyperparameter tuning.

### Alternative models

The notebook compares XGBoost with:

- Linear regression, using one-hot encoding for borough and community-district categories.
- A multilayer perceptron (MLP) neural network, using a preprocessing pipeline with one-hot encoding and feature scaling.

Linear regression is also evaluated for paper and MGP. Additional experiments examine scaled linear-regression coefficients and a reduced feature set for MGP.

## Evaluation design

The notebook uses a chronological holdout:

- **Training period:** observations before January 2019.
- **Test period:** observations from January 2019 onwards within the selected 2015–2019 window.

Performance is measured using:

- Mean absolute error (MAE), in tonnes.
- Root mean squared error (RMSE), in tonnes.

The models are compared with a naive baseline that predicts the current month's collection using the previous month's observed collection (`lag1`).

The notebook also runs `TimeSeriesSplit` for a time-aware hyperparameter search on the refuse model. A standard cross-validation search is discussed in the model-selection findings as less appropriate for time-ordered data.

## Findings recorded in the notebook

The notebook's written findings report the following results for the refuse test set:

| Model | MAE (tonnes) | RMSE (tonnes) | Improvement vs naive |
|---|---:|---:|---:|
| Linear regression (all features) | 199.7 | 268.9 | 25.6% |
| XGBoost (feature-selected) | 199.7 | 282.5 | 25.6% |
| XGBoost (all features) | 204.0 | 278.7 | 24.0% |
| Linear regression (selected features) | 204.1 | 273.5 | 23.8% |
| Neural network (MLP) | 204.9 | 272.6 | 23.7% |

Based on these recorded results, linear regression with the full feature set is the recommended approach for the refuse data: it ties for the lowest MAE and has the lowest RMSE among the compared models.

Other observations recorded in the notebook:

- The twelve-month lag is reported as the dominant feature across the three waste streams, indicating a strong annual seasonal signal.
- Removing the MGP twelve-month lag increased MAE by less than 4% in the notebook's ablation experiment.
- Reducing the refuse XGBoost feature set slightly improved MAE but increased RMSE, illustrating a trade-off between typical error and larger misses.
- The MLP did not outperform the simpler approaches on this dataset.

These figures are the results documented in the notebook, not independently rerun or verified as part of this README. The written findings describe the data as approximately 2,800 rows and note that the available history constrains time-series cross-validation.

## Limitations and interpretation

- The modelling window is historical and ends in 2019; performance on this period does not establish performance on current collection patterns.
- The notebook depends on a local `data.csv` file whose source is not recorded in the notebook.
- The current modelling scope excludes organic waste.
- Hyperparameter tuning is explored for refuse; equivalent tuning for paper and MGP is not completed in this notebook.
- The findings suggest that historical seasonal patterns are useful, but this does not establish how well the model would perform under changed collection policies, service levels, population patterns or other operational conditions.
- The notebook is a retrospective modelling exercise, not evidence of a deployed system.

## Potential next steps

1. Document and verify the original data source, licence and collection methodology.
2. Update the dataset and test whether the relationships observed in 2015–2019 remain stable in more recent years.
3. Re-run the full evaluation with a clearly defined final holdout period and consistent baselines for all three waste streams.
4. Investigate whether the same model-selection conclusions hold for paper and MGP.
5. Refactor reusable data preparation, feature engineering and prediction code into functions or Python modules.
6. Develop a separate forecasting workflow that can generate future monthly predictions using only information available at the forecast date.
7. Consider operational factors and finer-grained data, where available, before treating forecasts as suitable for capacity or resource decisions.

## Repository structure (current)

```text
waste-demand-forecasting/
├── README.md
├── requirements.txt
└── waste_demand_model_development.ipynb
```

The input `data.csv` is expected locally and is not included. If it is appropriate to share the dataset, add it with source and licence information; otherwise keep it outside version control and document how to obtain it.

## Setup

Create and activate a virtual environment, then install the dependencies:

```bash
python -m venv .venv
pip install -r requirements.txt
```

Place the required `data.csv` file in the notebook's working directory, then run the notebook cells in order.

## Reproducibility and data handling

- Keep raw data, generated outputs and model artefacts out of version control unless they are small, permitted to share and necessary to reproduce the project.
- Do not commit credentials or private information.
- Record package versions and the dataset source when preparing a reproducible release.
