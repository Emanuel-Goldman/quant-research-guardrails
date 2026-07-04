---
paths:
  - "src/**/*.py"
  - "*.py"
---

# Modeling Simplicity Rule

- Start with simple baselines before adding complex models.
- Do not add model complexity without a clear experimental purpose.
- Keep baseline and extended pipelines as similar as possible, differing only in the feature set unless intentionally testing another change.
- Avoid changing preprocessing, sample construction, and hyperparameters simultaneously.
- Make each experiment test one main change.
- Prefer a transparent model when added complexity does not produce a robust, comparable improvement.
