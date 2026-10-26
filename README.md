# Green Investment Behavior: Firth Logistic Regression and the Pitfalls of Replicated Data

Research project, Durham University Business School (Collegio Ghislieri–Durham exchange, 2025) · R · Python

## Objective

The objective was to **identify factors associated with individual investment in green financial instruments**, which could then **motivate future causal analysis** to test whether any of those factors actually have a causal effect, comparing the **stated intention** to invest green (`y.ipo`) with **actual** green investment (`y.real`).

## Data

A pilot survey collected **15 responses** from Master's students (8 stating an intention to invest green, 2 actually investing). Since this is far too small for regression analysis, the 15 responses were **replicated to 97 observations** to develop and test the estimation pipeline. 

## Approach

| | |
|---|---|
| **Preprocessing** | Python: dummy encoding of survey answers (done beforehand; `survey_data.csv` is already encoded), merging of sparse categories, removal of perfectly collinear and constant dummies |
| **Estimation** | Standard logistic regression showed separation → Firth bias-reduced logistic regression (R, `logistf`) → aggregation of rare categories to reduce sparsity |
| **Interpretation** | Odds ratio, confidence intervals |
| **Evaluation** | AUC, stratified 5-fold cross-validation and leave-one-profile-out cross-validation |

## Key takeaways

- **Firth regression handles separation** where standard maximum likelihood fails, producing finite estimates; aggregating rare categories further reduces sparsity.
- **Replication creates spurious results:** with only 15 distinct profiles repeated across 97 rows, standard cross-validation reports an AUC of 1, because test folds contain exact copies of training observations. Holding out each profile entirely (leave-one-profile-out CV), the AUC drops to 0.67 for stated intention and to 0.79 for actual investment.
So the leave-one-profile-out cross-validation revealed that the replicated rows were causing data leakage, which invalidates all previous results.

In conclusion, the project would need to be repeated with a larger sample of real, independent responses.

## Repository

- `survey_data.csv` — 15 pilot responses, dummy-encoded and replicated to 97 observations
- `model_data.csv` — processed dataset
- `data_preprocessing.ipynb` — preprocessing (Python)
- `firth_logistic_analysis.ipynb` — Firth logistic regression, evaluation and leave-one-profile-out cross-validation (R)
- `sustainable_investment_behavior_report.pdf` — report

**Tools:** R (logistf, pROC, caret) · Python (pandas)
