---
paths:
  - "src/**/*.py"
  - "*.py"
---

# Feature and Target Discipline Rule

- Keep feature construction and target construction separate.
- Feature columns must not depend on target columns.
- Name target columns with their forecast horizon, for example `IVOL_fwd_252`.
- Give backward-looking features names that identify their window, for example `bf_IVOL_252` or `sf_UTIL_Z252`.
- Do not mix baseline features and securities-lending features accidentally.
- Preserve the distinction between `bf_*` baseline features and `sf_*` securities-lending features.
- When adding a feature, specify whether it is baseline, securities-lending, market-level, or target-related.
- Document the timestamp at which each feature becomes observable when it differs from the source event date.
