---
paths:
  - "src/**/*.py"
  - "*.py"
---

# Feature and Target Discipline Rule

- Keep feature construction and target construction separate.
- Feature columns must not depend on target columns.
- Name target columns with their forecast horizon, for example `TARGET_fwd_252`.
- Give backward-looking features names that identify their window, for example `bf_FEATURE_252` or `alt_FEATURE_Z252`.
- Do not mix distinct feature families (for example baseline and alternative-data features) accidentally.
- Preserve prefix-based distinctions between feature families, for example `bf_*` baseline features versus `alt_*` alternative-data features.
- When adding a feature, specify which feature family it belongs to (for example baseline, alternative-data, market-level) and whether it is target-related.
- Document the timestamp at which each feature becomes observable when it differs from the source event date.
- If any of the rules above are violated in code being read, reviewed, or touched — not only in code being newly written — notify the user in chat with the specific violation before proceeding, even if fixing it is outside the current task's scope.
