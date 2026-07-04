# Quant Research Guardrails

Reusable Claude Code rules for data-science and quantitative-research projects where silent errors can be more dangerous than obvious failures.

The rule set emphasizes point-in-time correctness, leakage prevention, reproducibility, disciplined evaluation, safe DataFrame operations, and honest interpretation. It also includes practical software-engineering guidance for readable, maintainable research code.

## One-file quick start

Open [`CLAUDE-RULES.md`](CLAUDE-RULES.md), copy the entire file, and paste it into your project's `CLAUDE.md`. It contains all 13 rules in one ready-to-use document.

Use the individual files under `.claude/rules/` when you prefer path-scoped, modular rules.

## Included rules

| Rule | Purpose |
| --- | --- |
| `fail-fast.md` | Surface invalid assumptions and data problems immediately. |
| `code-reuse.md` | Keep shared research logic in reusable functions and modules. |
| `function-readability.md` | Make functions focused, explicit, and easy to verify. |
| `data-leakage.md` | Prevent look-ahead bias and test-set contamination. |
| `reproducibility.md` | Make experiments repeatable from recorded inputs and settings. |
| `experiment-logging.md` | Preserve run metadata, predictions, and comparable results. |
| `dataframe-safety.md` | Avoid quiet pandas mutation, merge, and filtering errors. |
| `feature-target-discipline.md` | Keep features, targets, horizons, and feature families distinct. |
| `evaluation-discipline.md` | Enforce clean splits and apples-to-apples model comparisons. |
| `notebook-script-discipline.md` | Keep production logic independent of notebook state. |
| `modeling-simplicity.md` | Test one modeling change at a time against simple baselines. |
| `performance-memory.md` | Improve efficiency without obscuring correctness. |
| `research-interpretation.md` | Separate robust predictive evidence from causal claims. |

## Use in a project

Copy the `.claude/rules` directory into the root of your project. Claude Code will discover the scoped Markdown rules there.

PowerShell:

```powershell
Copy-Item -Recurse .claude\rules C:\path\to\your-project\.claude\rules
```

Bash:

```bash
cp -R .claude/rules /path/to/your-project/.claude/rules
```

Review and adapt the path scopes and domain-specific naming examples before using the rules in a different research setting.

## Design principles

- Correctness before cleverness.
- Point-in-time data availability is explicit.
- Train, validation, and test responsibilities never blur.
- Experiments leave enough evidence to be reproduced and audited.
- Research code stays readable enough to challenge its assumptions.
- Reported gains use comparable samples and stable metric definitions.

## Repository structure

```text
.
|-- .claude/
|   `-- rules/
|       `-- *.md
|-- CLAUDE.md
|-- CLAUDE-RULES.md
|-- LICENSE
`-- README.md
```

## Contributing

Contributions are welcome. Keep new rules concise, actionable, and focused on preventing a concrete class of research or engineering failure.

## License

MIT
