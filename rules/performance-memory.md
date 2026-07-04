---
paths:
  - "src/**/*.py"
  - "*.py"
---

# Performance and Memory Rule

- Avoid unnecessary copies of very large DataFrames.
- Avoid repeated groupby or rolling operations inside loops when a result can be computed once.
- Avoid row-by-row pandas loops for large datasets.
- Prefer readable vectorized pandas or NumPy operations.
- Cache expensive intermediate results only when doing so cannot introduce stale data or leakage.
- Do not optimize prematurely when code is already fast enough and readability would suffer.
- Measure the actual bottleneck before making a performance-driven rewrite.
