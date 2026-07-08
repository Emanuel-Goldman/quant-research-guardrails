---
paths:
  - "*.ipynb"
  - "**/*.ipynb"
---

# Notebook Setup Cells

- Start every research notebook with a setup section before any data loading, transformation, plotting, modeling, or analysis.
- Put all imports in the first code cell unless an import is intentionally local to a function or optional dependency path.
- Put pandas display settings immediately after the imports, using exactly:

  ```python
  pd.set_option("display.max_columns", 700)
  pd.set_option("display.max_rows", 700)
  pd.set_option("display.float_format", "{:.8f}".format)
  ```

- Do not scatter imports or notebook-wide display configuration across analysis cells; move them back to the setup section when editing existing notebooks.
- Do not change notebook-wide display settings later in the notebook unless the cell explains why the temporary override is needed and restores or scopes the change.
