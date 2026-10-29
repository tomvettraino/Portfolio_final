# Football Score Prediction Model

An independent football analytics project exploring whether historical match data and team-performance features can be used to predict the number of goals scored by each team, and from these estimates derive likely match scorelines.

The project is being developed as a quantitative football modelling portfolio piece. The current notebook is an exploratory development notebook: it contains data collection, feature engineering, model training, prediction functions and backtesting experiments. It is not yet a polished, one-click live prediction tool.

## Project status

**Current stage: model development and evaluation**

The current notebook builds and evaluates a score-prediction approach using historical results and match statistics. The next stage is to separate experimentation from the process used to generate predictions for upcoming fixtures.

Some sections are commented out because they represent approaches that were explored, did not produce a sufficiently convincing result, or need further development. They have been retained as research notes rather than presented as functioning parts of the current model.

## Data

The notebook currently retrieves match-level CSV data from [Football-Data.co.uk](https://www.football-data.co.uk/).

The data-loading function is configured for:

- English Premier League (`E0`)
- English Championship (`E1`)
- English League One (`E2`)

The season list in the notebook currently runs from 2023/24 to 2026/27. The latest season may contain only the fixtures and results available when the notebook is run.

The notebook selects match dates, teams, full-time and half-time results, shots, shots on target, corners, fouls, cards and selected bookmaker odds fields. It removes fixtures without full-time results before model development.

The notebook also writes a local `football_data.csv` file when the data-loading cell is run. This generated data file is not included in this project folder.

## Current modelling approach

### 1. Feature engineering

The notebook sorts matches chronologically and constructs team-level features using previous matches. These include:

- Exponentially weighted rolling averages of goals scored and conceded.
- Rolling averages of shots.
- Rolling standard deviations of goals scored and conceded.
- Longer-window rolling averages, intended to capture a slower-moving measure of team performance.
- League points and goal-difference features.
- Goal-scoring and goal-conceding overperformance features.
- Head-to-head summaries, with an indicator for whether relevant history is available.

The rolling statistics use shifted match data so that a team's current match result is not included in the historical features used for that match.

### 2. Goal prediction

Two separate `XGBRegressor` models are trained:

- A home-goals model.
- An away-goals model.

Both use the Poisson count objective. The model outputs expected home and away goals, which are then used to construct a scoreline probability grid using Poisson probabilities.

### 3. Scoreline and match-outcome evaluation

The notebook includes functions to:

- Calculate probabilities for possible scorelines.
- Select a representative scoreline from the probability grid, taking match-outcome points into account.
- Evaluate exact-score predictions and correct match outcomes.
- Calculate points per match using a scoring system of three points for an exact score, one point for the correct outcome, and zero otherwise.

### 4. Model evaluation and backtesting

The notebook evaluates goal predictions using mean absolute error (MAE) for home and away goals.

It also includes chronological cross-validation using `TimeSeriesSplit` over unique match dates. Each fold trains fresh home- and away-goal models on earlier dates and evaluates them on a later period.

A simple baseline is included for comparison. It selects a constant scoreline using the training period's average points under the same scoring system, then applies that scoreline to the test period.

The intention is to assess whether the model adds value beyond a simple benchmark, rather than relying on training performance alone. Results should be interpreted using the printed fold-level and overall backtest outputs from the notebook; no performance claim is made here.

## Exploratory work and approaches to revisit

The notebook retains commented-out experiments and notes, including:

- **ClubElo ratings:** attempts to retrieve historical club ratings as an additional measure of team strength. The current notebook does not use ClubElo ratings in its active model.
- **Team-style clustering:** an attempt to group teams based on team-level performance statistics. The exploratory features included goals, shots, shots on target, corners, fouls and cards.
- **Additional style information:** possession and passing statistics were considered as potential additions to the clustering features. The relevant WhoScored scraping and feature-merging code is currently commented out and is not part of the active pipeline.
- **Cluster features in the prediction model:** commented-out references show where cluster labels were considered, but clustering is not currently used by the active prediction functions.

These experiments are retained to document the development process. They should be revisited only if they can be implemented reliably and shown to add value through chronological evaluation.

## Planned development

The next steps are:

1. **Separate development and prediction workflows.** Keep an exploratory development notebook for feature experiments, model comparisons, tuning and backtesting. Create a separate live-prediction notebook that loads the chosen model and produces predictions for upcoming fixtures.
2. **Make the live workflow reproducible.** Separate data collection, feature engineering, model training and prediction into clearly defined steps. Ensure the live notebook can construct the same features expected by the trained models using information available before kick-off.
3. **Improve evaluation.** Continue testing against simple baselines with chronological splits. Review performance across seasons and competitions, and keep model selection separate from the final held-out evaluation.
4. **Investigate NLP-derived availability features.** Explore football news and reporting as a possible source of proxies for injuries, suspensions and player availability. Potential approaches include extracting relevant text, identifying teams and players, classifying availability-related statements, and aggregating information into pre-match team-level features.
5. **Test whether new features help.** Evaluate any NLP or other additional features against the existing model using the same chronological backtesting framework. News must be timestamped so that information published after kick-off cannot leak into pre-match predictions.

The NLP work is a planned extension, not an implemented feature of the current notebook.

## Repository structure (planned)

The project is intended to develop towards a structure similar to:

```text
football-score-prediction/
├── README.md
├── requirements.txt
├── notebooks/
│   ├── development.ipynb
│   └── live_predictions.ipynb
├── src/
│   ├── data_collection.py
│   ├── feature_engineering.py
│   ├── modelling.py
│   └── prediction.py
├── models/
│   └── (saved model files; generated locally)
└── data/
    └── (locally generated data; not committed)
```

This is a target structure, not a description of files that already exist. The current development notebook is included at the project root for now and can be moved into `notebooks/` when the structure is refactored.

## Setup

The notebook was developed with a Google Colab-style workflow. To install the Python dependencies in a local environment:

```bash
python -m venv .venv
```

Activate the environment, then run:

```bash
pip install -r requirements.txt
```

In Google Colab, dependencies can instead be installed in the notebook runtime. The current notebook contains a Selenium/Chromium setup cell from earlier data-collection experiments. Selenium is not required by the active Football-Data.co.uk CSV-loading or modelling workflow; if those unused imports and setup cells are removed during cleanup, Selenium and `webdriver-manager` can also be removed from `requirements.txt`.

Run the development notebook from top to bottom to recreate the data and modelling workflow. Internet access is required to retrieve the CSV files. The notebook writes generated data locally.

