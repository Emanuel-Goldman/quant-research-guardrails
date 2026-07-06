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
- When computing an error metric such as MSE or QLIKE for each company or group, isolate the group first and call the same metric function used elsewhere directly on that subset:

  ```python
  df_comp = df[df["PERMNO"] == permno]
  base_mse = mean_squared_error(df_comp[target_col], df_comp[pred_col])
  ```

  Do not hand-roll global per-row errors and aggregate them with `groupby(...).mean()`, even when the result is numerically equivalent. Prefer a small helper such as `company_mse(df, model, target_col, permno)` plus a loop over groups. This keeps each group calculation self-contained, reuses the pipeline's authoritative metric implementation, and is easier to audit. This is a sanctioned exception to the Performance and Memory preference for vectorized operations: it loops over groups, not rows.
