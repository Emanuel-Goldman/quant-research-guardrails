---
paths:
  - "src/**/*.py"
  - "*.py"
---

# Evaluation Discipline Rule

- Keep train, validation, and test logic strictly separated.
- The test set is final held-out data and must not influence feature selection, hyperparameter tuning, transformations, or modeling choices.
- Compare baseline and extended models using identical observations.
- Before reporting improvement percentages, verify that both models were evaluated on the same rows and target values.
- Compute evaluation metrics consistently across models.
- Do not change metric definitions casually; make changes to MSE, MAE, QLIKE, or R-squared logic explicit.
- When a target admits multiple representations (for example level vs. log, or a transformed vs. untransformed scale), state which representation the metrics are computed on.
- Use time-aware validation and an appropriate gap or embargo when labels overlap or information arrives with delay.
- Report uncertainty or variation across periods when a single aggregate metric could conceal instability.
