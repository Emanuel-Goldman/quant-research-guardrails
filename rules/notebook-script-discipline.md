---
paths:
  - "*.ipynb"
  - "**/*.ipynb"
  - "*.py"
  - "**/*.py"
---

# Notebook and Script Discipline Rule

- Use notebooks for exploration, plots, and interpretation, not hidden production logic.
- Keep important logic in `.py` files rather than only in notebook cells.
- Run every main research experiment from a notebook that the user can inspect; do not run an experiment only in a shell, REPL, temporary script, or agent-only tool session and then use its results.
- Keep the experiment's inputs, configuration, execution, outputs, and any result used in analysis, decisions, research notes, or chat visible in the saved notebook.
- Reusable logic may live in importable `.py` modules, but the notebook must call it explicitly and retain the outputs needed to audit each conclusion.
- Small private diagnostic checks are allowed only to understand the code or data and decide what test to propose next. Do not treat their outputs as experimental evidence, use them to justify a conclusion, or report them as results; rerun any relevant check in the notebook before using its result.
- When asked to run, compare, evaluate, or test a research experiment, add and execute it in the notebook and save the notebook with its outputs.
- If a main experiment cannot be executed and recorded in a notebook, stop and explain the limitation before using or reporting any result.
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
