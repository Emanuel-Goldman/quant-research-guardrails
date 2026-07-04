# Quant Research Guardrails

Use these rules when working on data-science or quantitative-research code. Correctness, point-in-time integrity, reproducibility, and honest evaluation take priority over convenience or unnecessary complexity.

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
- Make research-critical inputs explicit parameters rather than hidden globals or notebook state.
- Prefer early validation and early returns over deeply nested control flow.
- Use descriptive names for dates, horizons, windows, features, targets, and datasets.
- Document non-obvious assumptions, units, date conventions, return shapes, and mutation behavior.
- Split a function when its stages cannot be understood or tested independently.
- Keep clever vectorization only when its correctness remains easy to inspect.

## Data Leakage Prevention

- Avoid look-ahead bias above all else.
- Never use future returns, future volatility, future labels, or future availability information when constructing features.
- When editing target construction, rolling features, feature selection, train/validation/test splits, or evaluation logic, explicitly verify date boundaries.
- Rolling features must use only information available up to the prediction date.
- Forward targets must not overlap the training feature window unless explicitly intended and documented.
- Perform feature selection only within training and validation data, never on the final test period.
- Do not tune hyperparameters using test-period performance.
- Fit preprocessing and transformations only on the appropriate training data.
- If date or information-availability logic is unclear, stop and explain the possible leakage risk before editing.

## Reproducibility

- Experiments must be reproducible.
- Use fixed random seeds for model training, sampling, feature selection, and cross-validation when applicable.
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
- Name target columns with their forecast horizon, for example `IVOL_fwd_252`.
- Give backward-looking features names that identify their window, for example `bf_IVOL_252` or `sf_UTIL_Z252`.
- Do not mix baseline features and securities-lending features accidentally.
- Preserve the distinction between `bf_*` baseline features and `sf_*` securities-lending features.
- When adding a feature, specify whether it is baseline, securities-lending, market-level, or target-related.
- Document the timestamp at which each feature becomes observable when it differs from the source event date.

## Evaluation Discipline

- Keep train, validation, and test logic strictly separated.
- The test set is final held-out data and must not influence feature selection, hyperparameter tuning, transformations, or modeling choices.
- Compare baseline and extended models using identical observations.
- Before reporting improvement percentages, verify that both models were evaluated on the same rows and target values.
- Compute evaluation metrics consistently across models.
- Do not change metric definitions casually; make changes to MSE, MAE, QLIKE, or R-squared logic explicit.
- For volatility targets, state whether metrics are computed on volatility or variance.
- Use time-aware validation and an appropriate gap or embargo when labels overlap or information arrives with delay.
- Report uncertainty or variation across periods when a single aggregate metric could conceal instability.

## Notebook and Script Discipline

- Use notebooks for exploration, plots, and interpretation, not hidden production logic.
- Keep important logic in `.py` files rather than only in notebook cells.
- Do not rely on variables created by running previous cells in an undocumented order.
- When moving code from notebooks to scripts, convert repeated cells into focused functions.
- Keep notebooks short and centered on one analysis.
- Restart the kernel and run all cells in order before treating notebook output as reproducible evidence.

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
- Check whether gains concentrate in particular years, volatility buckets, market-cap buckets, or short-stress buckets.
- Distinguish predictive performance from economic interpretation.
- Do not claim causality from forecasting improvements.
- Report negative and unstable results rather than selecting only favorable periods or specifications.
- State material limitations, assumptions, and known sources of uncertainty alongside conclusions.
