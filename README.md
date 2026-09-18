# Sustainable Investment Behavior

Econometric analysis of the determinants of individual investment in green financial instruments, with a focus on the difference between stated investment intentions and actual investment behavior.

The project uses survey data from 97 Master's students and applies Firth bias-reduced logistic regression to address small-sample and separation issues.

## Project Overview

The objective of the project is to investigate which individual characteristics are associated with investment in green financial instruments.

Two different outcomes are analyzed:

- `y.ipo` — stated intention to invest in green financial instruments;
- `y.real` — revealed behavior, indicating whether the respondent already invests in green financial instruments.

This distinction allows the analysis to compare:

- what individuals say they would do;
- what individuals actually do.

The project therefore combines sustainable finance, behavioral finance and small-sample econometrics.

## Research Question

The main question is:

> Which financial and environmental characteristics are associated with the probability of investing in green financial instruments?

The analysis also investigates whether the determinants of stated preferences differ from those of actual investment behavior.

## Dataset

The empirical analysis is based on survey data collected from 97 Master's students in Quantitative Finance at the University of Pavia.

The questionnaire includes information on:

- current investment behavior;
- willingness to invest additional income;
- environmental sensitivity;
- knowledge of green initiatives;
- knowledge of the Paris Agreement;
- perceived impact of green financial instruments;
- perceived safety of green investments;
- willingness to sacrifice financial returns for sustainability;
- stated willingness to invest in green instruments;
- actual investment in green instruments.

The dataset is observational and cross-sectional.

## Behavioral Dimensions

The explanatory variables can be conceptually grouped into four broad dimensions.

### Capital Availability

Variables related to the amount of personal capital already invested in financial instruments.

### General Investment Propensity

Variables measuring how much of an unexpected additional amount of money, such as a gift, the respondent would be willing to invest in financial instruments.

### Environmental Sensitivity and Information

Variables related to:

- environmental concern;
- knowledge of green initiatives;
- knowledge of the Paris Agreement;
- willingness to sacrifice returns for sustainability.

### Trust in Green Financial Products

Variables related to:

- perceived real-world impact of green investments;
- perceived safety of green instruments compared with traditional financial products.

## Data Preprocessing

The raw survey data are processed in Python before the econometric analysis.

The main preprocessing steps include:

- removal of technical columns;
- inspection of duplicate variables;
- treatment of missing values;
- transformation of categorical responses into binary dummy variables;
- conversion of the dependent variables into binary 0/1 format;
- removal of redundant or perfectly collinear predictors;
- removal of variables with no variation;
- aggregation of selected categories when necessary.

One category from each group of dummy variables is used as the baseline in order to avoid perfect multicollinearity.

The final cleaned dataset retains all 97 respondents.

## Multicollinearity and Redundancy

The preprocessing stage includes checks for highly correlated and linearly dependent regressors.

Variables with extremely high correlation or exact linear relationships are removed or merged.

For example, adjacent categories can be combined when they provide very similar information or when one category contains too few observations.

The resulting dataset is designed to reduce redundancy before estimation.

## Why Firth Logistic Regression?

Both dependent variables are binary, so logistic regression is the natural starting point.

However, the dataset creates two important statistical problems:

- small sample size;
- rare observations in some categories.

Initial logistic regression estimates based on standard Maximum Likelihood showed instability, including very large coefficients, inflated standard errors and separation problems.

### Separation

Separation occurs when one or more predictors almost perfectly distinguish observations with `Y = 1` from observations with `Y = 0`.

For example, suppose a rare category contains only respondents who invest in green products:

```text
env.very = 1  -> all observations have y.real = 1
env.very = 0  -> most observations have y.real = 0
```

A standard logistic regression can respond by pushing the corresponding coefficient toward extremely large values.

This makes Maximum Likelihood estimation unstable.

### Firth Bias Correction

Firth logistic regression introduces a bias-reduction correction that is particularly useful in:

- small samples;
- rare-event settings;
- separation problems.

In this project, Firth logistic regression successfully addresses the separation problem and prevents coefficients from diverging.

However, this does not solve every problem in the data.

## Separation vs Sparsity

Separation and sparsity are related but different issues.

### Separation

Separation occurs when a variable or combination of variables almost perfectly separates the two outcomes.

This can cause standard logistic regression coefficients to become extremely large or theoretically diverge.

Firth logistic regression is used to address this problem.

### Sparsity

Sparsity means that some categories contain very few observations.

For example:

```text
env.very = 1  -> 5 respondents
env.very = 0  -> 92 respondents
```

Even if Firth produces finite coefficients, the estimate for `env.very` is still based on very limited information.

This can lead to:

- large confidence intervals;
- unstable coefficient estimates;
- high p-values;
- limited statistical power.

Therefore:

```text
Standard Logistic Regression
        ↓
Separation and unstable coefficients
        ↓
Firth Logistic Regression
        ↓
Separation is addressed
        ↓
Sparsity and limited information remain
        ↓
Rare categories are aggregated
        ↓
More stable estimation
```

## Non-Aggregated and Aggregated Models

The econometric analysis is performed in two stages.

### Stage 1 — Non-Aggregated Dataset

Firth logistic regression is first applied to the dataset produced after the initial preprocessing.

Although separation is addressed, several variables remain based on very small groups of respondents.

As a result:

- confidence intervals remain very wide;
- many p-values are large;
- coefficient estimates remain unstable.

### Stage 2 — Aggregated Dataset

To reduce sparsity, rare dummy categories are merged.

Categories with fewer than approximately 10% of observations in one of the two states are candidates for aggregation.

This creates larger groups and provides more information for coefficient estimation.

Firth logistic regression is then estimated again on the aggregated dataset.

The objective of the aggregation is therefore not to replace Firth regression.

Instead:

- Firth addresses separation;
- aggregation reduces sparsity.

The two approaches solve different but related problems.

## Econometric Models

Two separate Firth logistic models are estimated.

### Stated Preferences

```text
y.ipo = f(X)
```

This model investigates which characteristics are associated with a respondent stating that they would invest in green financial instruments.

### Revealed Preferences

```text
y.real = f(X)
```

This model investigates which characteristics are associated with already investing in green financial instruments.

The comparison between the two models is central to the project.

## Interpretation of Coefficients

The analysis uses two main measures to interpret the relationship between each explanatory variable and the probability of `Y = 1`:

- Odds Ratios;
- Average Marginal Effects.

These measures are used to understand which variables are more strongly associated with the dependent variable.

They do not identify causal effects.

## Odds Ratios

For a logistic regression coefficient `beta`, the Odds Ratio is:

`OR = exp(beta)`

Interpretation:

- `OR > 1` indicates a positive association with the probability of `Y = 1`;
- `OR < 1` indicates a negative association;
- `OR = 1` indicates no change in the odds.

For dummy variables, the Odds Ratio compares the category represented by the dummy with its baseline category.

Odds Ratios help identify which variables are more strongly associated with green investment behavior, but they should always be interpreted together with confidence intervals and p-values.

A very large Odds Ratio with a very wide confidence interval can indicate estimation uncertainty rather than strong evidence.

## Average Marginal Effects

Average Marginal Effects provide a more intuitive interpretation in terms of predicted probability.

For a dummy variable, the AME measures the average change in predicted probability when the variable changes from `0` to `1`.

For example:

`AME = +0.20`

means that the predicted probability of `Y = 1` increases on average by approximately 20 percentage points when moving from the baseline category to that category, according to the estimated model.

AMEs therefore help evaluate the practical magnitude of the association between each variable and green investment behavior.

## Association, Not Causality

The project uses observational survey data.

Therefore, Odds Ratios and Average Marginal Effects should be interpreted as measures of association.

They answer questions such as:

> Which characteristics are more strongly associated with the probability of investing in green financial instruments?

They do not answer:

> Which characteristics causally produce green investment behavior?

For example, a positive association between environmental concern and actual green investment does not imply that increasing environmental concern would necessarily cause an individual to invest.

Causal interpretation would require a different empirical design.

## Model Evaluation

Model performance is evaluated using both in-sample and out-of-sample measures.

### AUC

The Area Under the ROC Curve measures discriminatory ability.

Higher values indicate a stronger ability to distinguish between observations with `Y = 1` and `Y = 0`.

### Brier Score

The Brier score measures the accuracy of predicted probabilities.

Lower values indicate better probability calibration.

### Tjur's R²

Tjur's coefficient of discrimination measures the difference between the average predicted probability for observations with `Y = 1` and observations with `Y = 0`.

### Cross-Validation

Stratified five-fold cross-validation is used to evaluate out-of-sample predictive robustness.

## Apparent Perfect Performance

In the non-aggregated models, performance metrics are extremely high:

- AUC approximately equal to `1`;
- very low Brier scores;
- high Tjur's R².

These results should not be interpreted as evidence of a perfect predictive model.

Given the small sample and the limited number of positive outcomes, they are more plausibly explained by quasi-perfect separation and overfitting.

The model can effectively memorize patterns in the small sample.

For this reason, the project places greater emphasis on coefficient stability, uncertainty and the aggregated specification rather than on apparently perfect predictive performance.

## Main Results — Stated Preferences

For `y.ipo`, the strongest result is related to general willingness to invest additional income.

Respondents who report that they would allocate a meaningful share of an unexpected windfall to financial instruments are substantially more likely to state that they would also invest in green financial products.

In the aggregated specification, the category corresponding to investing approximately 26–50% of additional income remains statistically significant.

The estimated association is large, with a positive Odds Ratio and a substantial Average Marginal Effect.

The result suggests that stated willingness to invest in green products is strongly associated with broader investment readiness.

Environmental variables are less statistically robust in the stated-preference model.

## Main Results — Revealed Preferences

The results for `y.real` are less precise.

None of the regressors reaches conventional 5% statistical significance in the aggregated model.

However, very high environmental concern, represented by `env.very`, emerges as the most suggestive predictor.

The estimated association is positive and the Average Marginal Effect is large.

However:

- the confidence interval is extremely wide;
- the p-value remains above conventional significance levels;
- the sample contains relatively few positive observations.

Therefore, this result should be interpreted as suggestive rather than conclusive.

## Stated vs Revealed Preferences

The main behavioral finding of the project is the difference between stated and revealed preferences.

### Stated Preferences

Stated willingness to invest in green instruments appears to be primarily associated with:

- general investment propensity;
- willingness to allocate additional income to financial assets;
- broader investment readiness.

### Revealed Preferences

Actual investment behavior shows weaker statistical evidence.

However, the results suggest that environmental concern may play a relatively more important role in actual green investment decisions.

This creates a potential divergence between:

```text
What individuals say they would do
        ↓
General investment readiness

What individuals actually do
        ↓
Possible stronger role of environmental values
```

Because of the small sample, this difference should be interpreted as exploratory rather than definitive.

## Sample Size Sensitivity

A bootstrap-based sensitivity analysis is also performed to investigate whether increasing the sample size would improve statistical significance for selected regressors in the revealed-preference model.

The results suggest that simply doubling or tripling the sample may not be sufficient to consistently produce statistically significant coefficients.

This indicates that the problem is not only sample size.

Redundancy among regressors and sparsity in some categories also contribute to estimation instability.

## Main Findings

- Firth logistic regression successfully addresses separation problems present in standard logistic regression.
- Sparsity remains an important issue even after applying Firth correction.
- Aggregating rare categories improves the stability of coefficient estimates.
- Stated willingness to invest in green instruments is strongly associated with general investment readiness.
- Environmental variables provide limited evidence for stated preferences.
- Very high environmental concern shows suggestive positive association with actual green investment behavior.
- The revealed-preference model remains statistically fragile because of the small sample and rare positive outcomes.
- Odds Ratios and Average Marginal Effects help identify which variables are more strongly associated with green investment behavior.
- These associations should not be interpreted as causal effects.
- Apparently perfect predictive metrics are likely driven by overfitting and quasi-perfect separation rather than genuine out-of-sample predictive power.

## Limitations

The main limitations of the project are:

- small sample size (`N = 97`);
- few positive outcomes for revealed preferences;
- sparse dummy categories;
- wide confidence intervals;
- redundancy among regressors;
- limited statistical power;
- potential overfitting;
- observational and cross-sectional survey design.

The project should therefore be interpreted as an exploratory analysis rather than definitive evidence on the determinants of sustainable investment behavior.

## Repository Structure

```text
sustainable-investment-behavior/
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
- `data_preprocessing.ipynb` — Python notebook for data cleaning, dummy creation, redundancy checks and preparation of the modeling dataset
- `firth_logistic_analysis.ipynb` — econometric analysis using Firth logistic regression, model evaluation, Odds Ratios and Average Marginal Effects
- `sustainable_investment_behavior_report.pdf` — complete research report

## Technologies

- Python
- R
- pandas
- Logistic Regression
- Firth Logistic Regression
- Cross-Validation
- Bootstrap
- Sustainable Finance
- Behavioral Finance
- Econometrics

## Conclusion

The project shows that sustainable investment behavior cannot be interpreted only through environmental preferences.

Stated willingness to invest in green financial products appears strongly related to general investment propensity, while actual investment behavior provides tentative evidence of a stronger role for environmental concern.

Methodologically, the project also illustrates the challenges of estimating binary-choice models in small and sparse datasets.

Firth bias correction successfully handles separation, while aggregation of rare categories is required to reduce sparsity and improve estimation stability.

The findings should therefore be interpreted as exploratory associations rather than causal effects.
