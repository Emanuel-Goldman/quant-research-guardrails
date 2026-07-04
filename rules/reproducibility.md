---
paths:
  - "src/**/*.py"
  - "*.py"
---

# Reproducibility Rule

- Experiments must be reproducible.
- Use fixed random seeds for model training, sampling, feature selection, and cross-validation when applicable.
- Design experiments so the seed is a single, exposed parameter, allowing the full run to be repeated with a different seed to check stability.
- Do not rely on hidden notebook state.
- Make important experiment parameters explicit arguments or named configuration values, not buried literals.
- Save enough metadata to reproduce each run: model name, feature set, target, train period, validation period, test period, gap, seed, and metric definitions.
- Record relevant data and code versions when practical.
- Do not overwrite previous experiment outputs unless explicitly asked.
