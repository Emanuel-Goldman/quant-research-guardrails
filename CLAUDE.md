# Repository Guidance

## Purpose

This repository is a reusable collection of Claude Code rules for data-science and quantitative-research projects. Its central goal is to prevent code that runs successfully while producing invalid, irreproducible, or misleading results.

## Repository layout

- `rules/` contains the rule files distributed by this project.
- `CLAUDE-RULES.md` contains the same rules consolidated into a single file; its section content must match the corresponding `rules/` file exactly.
- `README.md` documents the collection for users and contributors.
- `LICENSE` contains the MIT license.

## Working in this repository

- Treat every rule as user-facing documentation.
- Keep wording direct, testable, and specific enough to guide an implementation decision.
- Prefer one coherent concern per rule file.
- Preserve YAML frontmatter and use repository-relative glob patterns.
- Avoid duplicating guidance across files; cross-reference another rule when useful.
- Keep examples generic unless a domain-specific example materially clarifies the rule.
- Do not weaken leakage, split-integrity, or reproducibility safeguards for convenience.

## Validation checklist

When changing a rule:

1. Confirm its frontmatter is valid and its path scope is intentional.
2. Check that instructions do not conflict with another rule.
3. Verify that the rule describes observable behavior rather than vague preferences.
4. Update the rule table in `README.md` if files are added, removed, or renamed.
5. Apply the same change to the matching section in `CLAUDE-RULES.md` so it stays in sync with `rules/`.
6. Check Markdown formatting and links before committing.
