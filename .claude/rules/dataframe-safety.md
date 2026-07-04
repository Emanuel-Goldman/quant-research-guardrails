---
paths:
  - "src/**/*.py"
  - "*.py"
---

# DataFrame Safety Rule

- Avoid ambiguous chained assignment in pandas.
- Use `.copy()` intentionally when creating train, validation, and test subsets that will be modified.
- Do not mutate input DataFrames inside helpers unless the function name and documentation clearly indicate mutation.
- Prefer returning a new DataFrame over modifying in place.
- Before merging, check and document the merge keys and expected relationship.
- After important merges, check row counts, match rates, and duplicate keys when relevant.
- Do not silently drop rows with missing values; make the reason and affected columns clear.
- When filtering observations, make the condition explicit and readable.
- Preserve index meaning deliberately; reset or align indexes only for a stated reason.
