---
paths:
  - "src/**/*.py"
  - "*.py"
  - "notebooks/**/*.ipynb"
---

# Code Reuse Rule

- Search for an existing helper, pipeline, or abstraction before adding duplicate logic.
- Put logic shared by experiments in importable Python modules rather than copied notebook cells.
- Keep one authoritative implementation of feature construction, target construction, splitting, and metric calculation.
- Parameterize meaningful experimental differences instead of cloning whole functions or scripts.
- Do not create a generic abstraction until at least two real use cases demonstrate the shared behavior.
- When consolidating duplicate code, preserve behavior and add checks around research-critical calculations.
