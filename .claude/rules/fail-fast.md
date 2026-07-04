---
paths:
  - "src/**/*.py"
  - "*.py"
---

# Fail-Fast Rule

- Validate required columns, data types, date ordering, uniqueness assumptions, and non-empty inputs at boundaries.
- Raise a clear error when an invariant is violated; do not silently substitute a fallback that changes the experiment.
- Do not catch broad exceptions unless adding useful context and re-raising or handling a specific, expected failure.
- Treat impossible values, duplicate keys, and unexpected row-count changes as errors when they threaten correctness.
- Use assertions for internal invariants, not for validating untrusted external inputs.
- If an assumption cannot be verified, stop and explain it before producing results.
