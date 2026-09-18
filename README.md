# Green Investment Behavior

Econometric analysis of stated and revealed preferences for green financial instruments using survey data and Firth bias-reduced logistic regression.

## Project Overview

This project investigates which financial and environmental characteristics are associated with individual investment in green financial instruments.

The analysis uses survey data from 97 Master's students and distinguishes between two binary outcomes:

- `y.ipo` — stated intention to invest in green financial instruments
- `y.real` — actual investment in green financial instruments

The main objective is to compare the factors associated with what individuals say they would do and what they actually do.

## Workflow

### 1. Data Preprocessing

The raw survey data are processed in Python.

The main steps include:

- transforming categorical responses into binary dummy variables;
- mapping the two target variables into 0/1 format;
- removing redundant and perfectly collinear variables;
- dropping variables with no variation;
- checking correlations and exact linear dependencies;
- creating the final dataset used for econometric estimation.

The final dataset retains all 97 respondents.

### 2. Initial Logistic Regression

Both outcomes are binary, so standard logistic regression was initially considered.

However, the small sample and rare categories produced separation problems, leading to unstable Maximum Likelihood estimates and extremely large coefficients.

### 3. Firth Logistic Regression

Firth bias-reduced logistic regression was introduced to address separation and small-sample bias.

This produced finite and more stable coefficient estimates.

However, another issue remained: **sparsity**.

Some dummy categories contained very few observations, meaning that even with Firth correction the corresponding coefficients were estimated with little information and therefore had very wide confidence intervals.

### 4. Aggregation of Rare Categories

To reduce sparsity, rare dummy categories were merged.

The final modeling strategy therefore follows:

```text
Standard Logistic Regression
        ↓
Separation problems
        ↓
Firth Logistic Regression
        ↓
Separation addressed
        ↓
Sparsity remains
        ↓
Aggregation of rare categories
        ↓
More stable estimates
```

Firth regression and aggregation therefore address two related but different problems:

- Firth correction addresses separation;
- aggregation reduces sparsity.

### 5. Two Separate Models

Firth logistic regression is estimated separately for:

```text
y.ipo = f(X)
```

and

```text
y.real = f(X)
```

This allows the determinants of stated investment intentions to be compared with those of actual investment behavior.

### 6. Interpretation

The estimated relationships are interpreted using:

- Odds Ratios (OR);
- Average Marginal Effects (AME);
- confidence intervals;
- p-values.

Odds Ratios show the direction and magnitude of the association in terms of odds.

Average Marginal Effects provide a more intuitive measure of the change in predicted probability associated with a regressor.

ORs and AMEs are used to identify which variables are more strongly **associated** with the probability of green investment.

They should not be interpreted as causal effects because the project is based on observational cross-sectional survey data.

### 7. Model Evaluation

Model performance is evaluated using:

- AUC;
- Brier Score;
- Tjur's R²;
- stratified five-fold cross-validation.

The extremely high performance of the non-aggregated models is interpreted cautiously because it is likely driven by quasi-perfect separation and overfitting rather than genuine predictive power.

## Main Results

### Stated Preferences

For `y.ipo`, the strongest result is related to general investment readiness.

Respondents willing to allocate a substantial share of an unexpected windfall to financial instruments are also more likely to state that they would invest in green financial products.

In the aggregated specification, the 26–50% investment category remains statistically significant.

This suggests that stated interest in green finance is strongly associated with broader willingness to participate in financial markets.

### Revealed Preferences

For `y.real`, none of the regressors reaches conventional 5% statistical significance.

However, very high environmental concern (`env.very`) shows the strongest suggestive positive association with actual green investment behavior.

The estimated effect is large, but the confidence interval is wide and the result remains statistically inconclusive.

## Main Takeaways

- Standard logistic regression was affected by separation.
- Firth logistic regression addressed the separation problem.
- Sparse categories still produced unstable inference.
- Aggregating rare categories improved estimation stability.
- Stated green investment intentions are mainly associated with general investment readiness.
- Actual green investment behavior is associated with environmental attitudes instead.
- Odds Ratios and Average Marginal Effects measure associations, not causal effects.
- The project should be viewed as an exploratory study due to the limited sample size.

## Repository Structure

```text
green-investment-behavior/
│
├── README.md
├── survey_data.csv
├── model_data.csv
├── data_preprocessing.ipynb
├── firth_logistic_analysis.ipynb
└── sustainable_investment_behavior_report.pdf
```

## Main Files

- `survey_data.csv` — original survey dataset
- `model_data.csv` — processed dataset used for econometric estimation
- `data_preprocessing.ipynb` — Python preprocessing workflow
- `firth_logistic_analysis.ipynb` — Firth logistic regression and model evaluation
- `sustainable_investment_behavior_report.pdf` — complete research report

## Technologies

- Python
- R
- pandas
- Logistic Regression
- Firth Logistic Regression
- Cross-Validation
- Bootstrap
- Econometrics
- Sustainable Finance
- Behavioral Finance

## Limitations

The analysis is based on a small sample of 97 respondents and includes relatively few positive observations for actual green investment.

The results should therefore be interpreted as exploratory and associative rather than causal.
