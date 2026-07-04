---
paths:
  - "src/**/*.py"
  - "*.py"
---

# Data Leakage Prevention Rule

- Avoid look-ahead bias above all else.
- Never use future returns, future labels, or future availability information when constructing features.
- When editing target construction, rolling features, feature selection, train/validation/test splits, or evaluation logic, explicitly verify date boundaries.
- Rolling features must use only information available up to the prediction date.
- Forward targets must not overlap the training feature window unless explicitly intended and documented.
- Perform feature selection only within training and validation data, never on the final test period.
- Do not tune hyperparameters using test-period performance.
- Fit preprocessing and transformations only on the appropriate training data.
- If date or information-availability logic is unclear, stop and explain the possible leakage risk before editing.
- If any of the leakage patterns above are observed anywhere in code being read, reviewed, or touched — not only in code being newly written — stop and flag the specific risk before proceeding, even if fixing it is outside the current task's scope.
