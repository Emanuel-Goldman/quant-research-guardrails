---
paths:
  - "src/**/*.py"
  - "*.py"
---

# Experiment Logging Rule

- Every experiment should save clear outputs.
- Log the model name, feature set, target, train years, validation years, test years, gap years or days, seed, and metric values.
- Save selected features when feature selection is used.
- Save predictions when possible, not only aggregate metrics.
- Save baseline and extended results in comparable formats.
- Use descriptive output filenames that include the target, model, period, and feature set.
- Include a stable run identifier that links outputs and metadata.
- Avoid overwriting old results unless explicitly requested.
