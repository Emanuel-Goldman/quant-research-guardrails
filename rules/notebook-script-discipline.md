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
- Do not place multiple bare display expressions in one notebook cell and expect every result to render; only the final expression is displayed automatically:

  ```python
  companies_imp.head(20)  # Not displayed
  companies_imp.tail(20)  # Displayed
  ```

  Put one bare display expression in each cell, or call `print(...)` or `IPython.display.display(...)` explicitly. Prefer `display(...)` when rich DataFrame formatting should be preserved:

  ```python
  from IPython.display import display

  display(companies_imp.head(20))
  display(companies_imp.tail(20))
  ```
