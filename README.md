# Data Analysis Projects

A collection of end-to-end data analytics projects in Python, covering data cleaning, exploratory analysis, visualization, and machine learning across a range of real-world datasets — from public health and conflict data to real estate pricing, personal fitness tracking, and trading performance.

Each project lives in its own folder and is mostly self-contained, built around Jupyter notebooks with supporting Python scripts where the workflow calls for it.

## Projects

### [COVID19](./COVID19)
An exploratory analysis of the global COVID-19 pandemic, built as a small data pipeline rather than a single notebook. It downloads case data from the [CSSEGISandData/COVID-19](https://github.com/CSSEGISandData/COVID-19) repository, country/continent reference data, and socioeconomic indicators from the World Bank, then runs them through a feature-engineering layer (`features/`) to produce processed datasets for confirmed/recovered/death cases, daily case changes, mortality rates, and cases since outbreak day zero. A `CovidDataViz` class (`visualizations/covid_data_viz.py`) wraps the processed data for plotting, and a small `unittest` suite (`tests/`) checks that known data artifacts (e.g. cruise ship entries) are filtered out correctly. Notebooks cover data wrangling and exploratory analysis of case counts, mortality, and socioeconomic correlations.

**Structure:** `data/` (download script) → `features/` (cleaning & feature scripts) → `notebooks/` (analysis) → `visualizations/` (plotting class) → `tests/`

### [flats-in-cracow](./flats-in-cracow)
A price-prediction project built on scraped real estate listings for Kraków, Poland, structured as a three-stage pipeline:
1. **Data Wrangling** — cleans a scraped listings dataset, handles erroneous and outlier values (e.g. a property listed at 1 PLN), imputes missing numeric fields with `KNNImputer`, and filters/validates categorical and binary columns.
2. **Exploratory Analysis** — visualizes numeric, binary, and categorical features and checks their correlation with listing price.
3. **Model** — engineers features (e.g. total rooms, area-to-room ratio), trains a `GradientBoostingRegressor` and an `MLPRegressor` inside `scikit-learn` pipelines, combines them with a `VotingRegressor`, evaluates against a dummy baseline using RMSE/MAE/MSLE, and visualizes how predicted price varies by area, room count, and district.

Each notebook also has a rendered PDF version, and supporting charts are in `img/`.

### [global-terrorism](./global-terrorism)
An exploratory analysis of the [Global Terrorism Database](https://www.start.umd.edu/gtd/) (2007–2017), looking at trends in attacks and fatalities by attack type, weapon, target, and region. The notebook follows a template structure: data loading and cleaning (subsetting, renaming, filtering, handling missing values, deriving measures), followed by exploration of categorical, numerical, geospatial, and time-based variables. Selected visualizations — including a time series of deaths by attack type, attack counts by weapon, and a geo-coded map of attacks by the five most active groups — are written up in [`img/README.md`](./global-terrorism/img/README.md) along with the assumptions made about the data.

### [polar](./polar)
A personal fitness analysis using exported workout data from a Polar sports watch (raw data not included for privacy reasons). JSON exports are parsed into a single dataframe of workouts, cleaned, and enriched with derived fields such as workout duration, heart-rate quantiles, and binary strength/cardio flags. The analysis covers calories burned by sport, time, and intensity, workout timing patterns (by hour and day of week), and a sequence of regression models predicting calories burned from duration and heart rate — culminating in a linear mixed-effects model with random effects by workout type, validated with residual and Q-Q plots. A written summary of findings (totals, correlations, model performance) is included at the end of the notebook, and model output is saved in [`mdl_results.txt`](./polar/mdl_results.txt).

### [trading-results](./trading-results)
A walk-forward analysis of results from a personal algorithmic trading strategy (raw trade log not included). After cleaning and enriching the data — extracting trade direction, close reason, duration, and profit-per-lot — the analysis breaks down profitability by market, time of day, and day of week, examines trade duration and drawdown, and estimates the probability of consecutive losses. The notebook finishes with a Monte Carlo simulation that resamples historical per-trade results to estimate the probability of profit, and best/worst-case outcomes, over the next 100 trades.

## Tech stack

- **Data wrangling & analysis:** pandas, numpy
- **Visualization:** matplotlib, IPython display utilities
- **Modeling & statistics:** scikit-learn (regression, pipelines, imputation, model selection), statsmodels (mixed models, QQ plots, VIF), scipy (distributions, statistical tests)
- **Other:** joblib (model persistence), tabulate, wbdata / GitPython (COVID-19 data acquisition)

## Getting started

These projects don't share a single environment file — each notebook lists its imports at the top. To run one, open it in Jupyter and install whatever's missing, typically:

```bash
pip install pandas numpy matplotlib scikit-learn statsmodels scipy joblib tabulate
```

The COVID19 project additionally needs `wbdata` and `GitPython` to fetch raw data:

```bash
pip install wbdata gitpython requests
```

Then, from inside a project folder:

```bash
jupyter notebook
```

Note that the raw datasets for `polar` and `trading-results` are personal data and aren't included in the repo, so those notebooks are best read as a walkthrough of the analysis rather than run end-to-end.
