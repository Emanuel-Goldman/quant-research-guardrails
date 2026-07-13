# Quant Research Guardrails

This is quantitative-research code. Correctness, point-in-time integrity, reproducibility, and honest evaluation take priority over convenience or unnecessary complexity.

## Fail Fast

- Validate required columns, data types, date ordering, uniqueness assumptions, and non-empty inputs at boundaries.
- Raise a clear error when an invariant is violated; do not silently substitute a fallback that changes the experiment.
- Do not catch broad exceptions unless adding useful context and re-raising or handling a specific, expected failure.
- Treat impossible values, duplicate keys, and unexpected row-count changes as errors when they threaten correctness.
- Use assertions for internal invariants, not for validating untrusted external inputs.
- If an assumption cannot be verified, stop and explain it before producing results.

## Code Reuse

- Search for an existing helper, pipeline, or abstraction before adding duplicate logic.
- Put logic shared by experiments in importable Python modules rather than copied notebook cells.
- Keep one authoritative implementation of feature construction, target construction, splitting, and metric calculation.
- Parameterize meaningful experimental differences instead of cloning whole functions or scripts.
- Do not create a generic abstraction until at least two real use cases demonstrate the shared behavior.
- When consolidating duplicate code, preserve behavior and add checks around research-critical calculations.

## Function Readability

- Give functions one clear responsibility and names that describe their outcome.
- Avoid mixing data preparation, model training, evaluation, logging, plotting, and saving outputs in one function.
- Make research-critical inputs explicit parameters rather than hidden globals or notebook state.
- Prefer early validation and early returns over deeply nested control flow.
- Use descriptive names for dates, horizons, windows, features, targets, and datasets.
- Document non-obvious assumptions, units, date conventions, return shapes, and mutation behavior.
- Split a function when its stages cannot be understood or tested independently.
- Keep public functions as high-level orchestration when possible, and move detailed logic into private helpers.
- Do not over-split trivial code; extract helpers only when it improves readability or avoids repeated logic.
- Prefer readable helper names over comments explaining messy code.
- Keep clever vectorization only when its correctness remains easy to inspect.

## Function Docstrings

- Every function should start with a short one-line docstring explaining what the function is for.
- The docstring should describe the purpose of the function, not every implementation detail.
- Keep docstrings short and useful.
- Prefer one clear sentence.
- Do not write long parameter explanations unless the function is complex or public-facing.
- For private helper functions, still include a short one-line docstring.
- If the function name is unclear, improve the function name rather than writing a long docstring.

## Data Leakage Prevention

- Avoid look-ahead bias above all else.
- Never use future returns, future labels, or future availability information when constructing features.
- When editing target construction, rolling features, feature selection, train/validation/test splits, or evaluation logic, explicitly verify date boundaries.
- Rolling features must use only information available up to the prediction date.
- Forward targets must not overlap the training feature window unless explicitly intended and documented.
- Perform feature selection only within training and validation data, never on the final test period.
- Do not tune hyperparameters using test-period performance.
- Fit preprocessing and transformations only on the appropriate training data.
- If date or information-availability logic is unclear, stop and explain the possible leakage risk before editing.
- If any of the leakage patterns above are observed anywhere in code being read, reviewed, or touched — not only in code being newly written — stop and flag the specific risk before proceeding, even if fixing it is outside the current task's scope.

## Reproducibility

- Experiments must be reproducible.
- Use fixed random seeds for model training, sampling, feature selection, and cross-validation when applicable.
- Design experiments so the seed is a single, exposed parameter, allowing the full run to be repeated with a different seed to check stability.
- Do not rely on hidden notebook state.
- Make important experiment parameters explicit arguments or named configuration values, not buried literals.
- Save enough metadata to reproduce each run: model name, feature set, target, train period, validation period, test period, gap, seed, and metric definitions.
- Record relevant data and code versions when practical.
- Do not overwrite previous experiment outputs unless explicitly asked.

## Experiment Logging

- Every experiment should save clear outputs.
- Log the model name, feature set, target, train years, validation years, test years, gap years or days, seed, and metric values.
- Save selected features when feature selection is used.
- Save predictions when possible, not only aggregate metrics.
- Save baseline and extended results in comparable formats.
- Use descriptive output filenames that include the target, model, period, and feature set.
- Include a stable run identifier that links outputs and metadata.
- Avoid overwriting old results unless explicitly requested.

## Research Notes

- Maintain a project-level `RESEARCH_NOTES.md` as the durable record of the research process; create it when research work begins if it does not exist.
- Update the notes throughout the work, not only when the analysis or coding task is finished.
- Record every material research conclusion and every decision that affects the experiment, including data selection, sample construction, features, targets, splits, metrics, models, robustness checks, and interpretation.
- Record design decisions made while writing or reviewing code even when the current task is about another part of the project.
- Record research or experiment decisions supplied by the user in chat as soon as they become applicable to the project.
- For each entry, state the decision or conclusion, the reason for it, and the evidence, constraint, or tradeoff that supports it; include rejected alternatives when they materially explain the choice.
- Use dated, concise entries and identify the affected experiment, file, or component when useful.
- Distinguish confirmed conclusions and adopted decisions from hypotheses, open questions, and tentative ideas.
- Preserve prior entries as research history; correct or supersede them with a new entry instead of silently rewriting the record.
- Do not put secrets, credentials, or unnecessary sensitive data in the notes.

## Findings Report

- Write findings reports as concise, decision-oriented quantitative reports.
- Start each report with the verdict and a data audit before detailed analysis.
- Structure every analysis section as `Why we ran it -> What we see -> What we can conclude -> What we cannot conclude`.
- Support claims with exact metrics and referenced tables, figures, notebooks, or result files.
- Clearly separate in-sample evidence from out-of-sample evidence.
- Disclose material limitations, assumptions, instability, data-quality concerns, and leakage risks.
- Finish with an explicit advance or do-not-advance decision.
- Include the next point-in-time-safe experiment that should be run.
- Never claim more than the evidence supports.

## DataFrame Safety

- Avoid ambiguous chained assignment in pandas.
- Use `.copy()` intentionally when creating train, validation, and test subsets that will be modified.
- Do not mutate input DataFrames inside helpers unless the function name and documentation clearly indicate mutation.
- Prefer returning a new DataFrame over modifying in place.
- Before merging, check and document the merge keys and expected relationship.
- After important merges, check row counts, match rates, and duplicate keys when relevant.
- Do not silently drop rows with missing values; make the reason and affected columns clear.
- When filtering observations, make the condition explicit and readable.
- Preserve index meaning deliberately; reset or align indexes only for a stated reason.

## Feature and Target Discipline

- Keep feature construction and target construction separate.
- Feature columns must not depend on target columns.
- Name target columns with their forecast horizon, for example `TARGET_fwd_252`.
- Give backward-looking features names that identify their window, for example `bf_FEATURE_252` or `alt_FEATURE_Z252`.
- Do not mix distinct feature families (for example baseline and alternative-data features) accidentally.
- Preserve prefix-based distinctions between feature families, for example `bf_*` baseline features versus `alt_*` alternative-data features.
- When adding a feature, specify which feature family it belongs to (for example baseline, alternative-data, market-level) and whether it is target-related.
- Document the timestamp at which each feature becomes observable when it differs from the source event date.
- If any of the rules above are violated in code being read, reviewed, or touched — not only in code being newly written — notify the user in chat with the specific violation before proceeding, even if fixing it is outside the current task's scope.

## Evaluation Discipline

- Keep train, validation, and test logic strictly separated.
- The test set is final held-out data and must not influence feature selection, hyperparameter tuning, transformations, or modeling choices.
- Compare baseline and extended models using identical observations.
- Before reporting improvement percentages, verify that both models were evaluated on the same rows and target values.
- Compute evaluation metrics consistently across models.
- Do not change metric definitions casually; make changes to MSE, MAE, QLIKE, or R-squared logic explicit.
- When a target admits multiple representations (for example level vs. log, or a transformed vs. untransformed scale), state which representation the metrics are computed on.
- Use time-aware validation and an appropriate gap or embargo when labels overlap or information arrives with delay.
- Report uncertainty or variation across periods when a single aggregate metric could conceal instability.
- When computing an error metric such as MSE or QLIKE for each company or group, isolate the group first and call the same metric function used elsewhere directly on that subset:

  ```python
  df_comp = df[df["PERMNO"] == permno]
  base_mse = mean_squared_error(df_comp[target_col], df_comp[pred_col])
  ```

  Do not hand-roll global per-row errors and aggregate them with `groupby(...).mean()`, even when the result is numerically equivalent. Prefer a small helper such as `company_mse(df, model, target_col, permno)` plus a loop over groups. This keeps each group calculation self-contained, reuses the pipeline's authoritative metric implementation, and is easier to audit. This is a sanctioned exception to the Performance and Memory preference for vectorized operations: it loops over groups, not rows.

## Notebook Setup Cells

- Start every research notebook with a setup section before any data loading, transformation, plotting, modeling, or analysis.
- Put all imports in the first code cell unless an import is intentionally local to a function or optional dependency path.
- Put pandas display settings immediately after the imports, using exactly:

  ```python
  pd.set_option("display.max_columns", None)   # show all columns
  pd.set_option("display.width", None)         # don't wrap to terminal width
  pd.set_option("display.max_colwidth", None)  # don't truncate cell contents
  ```

- Do not scatter imports or notebook-wide display configuration across analysis cells; move them back to the setup section when editing existing notebooks.
- Do not change notebook-wide display settings later in the notebook unless the cell explains why the temporary override is needed and restores or scopes the change.

## Notebook and Script Discipline

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

## LaTeX PDF Compilation

- Build LaTeX PDFs with `./build_summary_pdf.sh <file.tex>` from the `Cross-Sectional-Return-Prediction` folder.
- With no argument, the script builds `summary/literature-review.tex`.
- Use `tectonic` as the LaTeX engine; it runs BibTeX and repeats the passes needed for citations and `\ref` cross-references in one command.
- If no LaTeX engine is present, install `tectonic` with `conda install -y -n base -c conda-forge tectonic` before compiling.
- Compile a fragment, meaning a `.tex` file with no `\documentclass`, by wrapping it in a standalone document that loads its packages and `\bibliography`; use the wrapper generated by the script rather than editing the fragment into a full document.
- Write the PDF next to the source `.tex` file.
- After compilation, delete generated wrappers and auxiliary files such as `.aux`, `.log`, `.bbl`, `.blg`, and `.out`; keep only the `.pdf`.

## Modeling Simplicity

- Start with simple baselines before adding complex models.
- Do not add model complexity without a clear experimental purpose.
- Keep baseline and extended pipelines as similar as possible, differing only in the feature set unless intentionally testing another change.
- Avoid changing preprocessing, sample construction, and hyperparameters simultaneously.
- Make each experiment test one main change.
- Prefer a transparent model when added complexity does not produce a robust, comparable improvement.

## Performance and Memory

- Avoid unnecessary copies of very large DataFrames.
- Avoid repeated groupby or rolling operations inside loops when a result can be computed once.
- Avoid row-by-row pandas loops for large datasets.
- Prefer readable vectorized pandas or NumPy operations.
- Cache expensive intermediate results only when doing so cannot introduce stale data or leakage.
- Do not optimize prematurely when code is already fast enough and readability would suffer.
- Measure the actual bottleneck before making a performance-driven rewrite.

## Research Interpretation

- Do not treat small metric improvements as meaningful without checking robustness.
- When reporting improvements, include absolute metric values and percentage improvement.
- Check whether gains concentrate in particular years, market regimes, market-cap buckets, or stress periods.
- Distinguish predictive performance from economic interpretation.
- Do not claim causality from forecasting improvements.
- Report negative and unstable results rather than selecting only favorable periods or specifications.
- State material limitations, assumptions, and known sources of uncertainty alongside conclusions.
