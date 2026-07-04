---
paths:
  - "src/**/*.py"
  - "*.py"
---

# Function Readability Rule

- Give functions one clear responsibility and names that describe their outcome.
- Make research-critical inputs explicit parameters rather than hidden globals or notebook state.
- Prefer early validation and early returns over deeply nested control flow.
- Use descriptive names for dates, horizons, windows, features, targets, and datasets.
- Document non-obvious assumptions, units, date conventions, return shapes, and mutation behavior.
- Split a function when its stages cannot be understood or tested independently.
- Keep clever vectorization only when its correctness remains easy to inspect.
