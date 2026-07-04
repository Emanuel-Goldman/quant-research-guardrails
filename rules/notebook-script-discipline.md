---
paths:
  - "*.ipynb"
  - "notebooks/**/*.ipynb"
  - "src/**/*.py"
---

# Notebook and Script Discipline Rule

- Use notebooks for exploration, plots, and interpretation, not hidden production logic.
- Keep important logic in `.py` files rather than only in notebook cells.
- Do not rely on variables created by running previous cells in an undocumented order.
- When moving code from notebooks to scripts, convert repeated cells into focused functions.
- Keep notebooks short and centered on one analysis.
- Restart the kernel and run all cells in order before treating notebook output as reproducible evidence.
