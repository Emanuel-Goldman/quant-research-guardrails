---
paths:
  - "src/**/*.py"
  - "*.py"
---

# Data Leakage Prevention Rule

- Avoid look-ahead bias above all else.
- Never use future returns, future volatility, future labels, or future availability information when constructing features.
- When editing target construction, rolling features, feature selection, train/validation/test splits, or evaluation logic, explicitly verify date boundaries.
- Rolling features must use only information available up to the prediction date.
- Forward targets must not overlap the training feature window unless explicitly intended and documented.
- Perform feature selection only within training and validation data, never on the final test period.
- Do not tune hyperparameters using test-period performance.
- Fit preprocessing and transformations only on the appropriate training data.
- If date or information-availability logic is unclear, stop and explain the possible leakage risk before editing.
