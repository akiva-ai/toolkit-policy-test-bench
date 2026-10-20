# toolkit-policy-test-bench: agent guide

## Workspace policy

When working under `augment-projects`, including repo worktrees, read
`C:/Users/ccana/Documents/augment-projects/akiva-enterprise-products/docs/agentic-workflows/workspace-agent-policy.md`
before work. It owns current review-model routing and browser/acceptance
workflows. Outside that workspace, do not assume sibling repos or local tools
exist; repository instructions remain applicable.

Guidance for AI coding assistants working in this repository.

## Commands

| Purpose | Command |
| ------- | ------- |
| Install (dev) | `pip install -e ".[dev]"` |
| Tests | `pytest -q` (CI: `pytest --cov=toolkit_policy_test_bench --cov-fail-under=70`) |
| Lint / format | `ruff check .` and `ruff format --check .` |
| Type-check | `pyright src/` |

## Layout

- `src/toolkit_policy_test_bench/`: the package. `cli.py` (entry point
  `toolkit-policy`), `runner.py` (applies suite checks), `detectors.py` (built-in
  PII/secret rules and the regex safety helpers), `plugins.py` (custom detectors),
  `pack.py` + `signing.py` (suite zips, hashes, Ed25519).
- `tests/`: pytest suite. `docs/CODEBASE_MAP.md`: module map.

## Conventions

- Fail closed. A check that cannot be completed (missing prediction, regex timeout,
  pack that does not verify) must fail the case or the command, never pass it.
- Run every regex through `detectors.compile_pattern` plus `safe_search` /
  `safe_findall`, never `re` directly, so the timeout and size cap apply.
- Runtime dependencies: `regex` only; `cryptography` stays optional (`[signing]`).
- Write the failing test first. Security fixes come before features.
- Test fixtures for secret formats are assembled from string parts so that no
  complete token-shaped literal is committed.
- Keep README status labels (Working / Partial / Planned) in step with the code, and
  add a CHANGELOG entry for user-visible changes.
