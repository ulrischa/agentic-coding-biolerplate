---
name: project-code-review
description: Review existing code or a diff for correctness, regressions, maintainability, and test gaps. Use when asked for a code review or change review; do not implement fixes unless requested.
---

# Code review

## Goal

Find actionable issues in the requested change or code area without editing files.

## Procedure

1. Identify the review scope (diff, commit, files or feature). If none is specified, inspect the working tree and state the assumed scope.
2. Read `AGENTS.md`, relevant architecture guidance and surrounding code paths.
3. Trace changed behavior, callers, inputs, outputs, error paths and compatibility constraints.
4. Look for correctness defects, regressions, security consequences, data loss, performance problems and insufficient tests.
5. Run existing, relevant read-only checks only when available and permitted. Do not silently modify source or dependencies.
6. Return findings **first**, ordered by severity. For each: explain the concrete scenario, impact and precise file location.
7. Then list verification performed, missing coverage and important assumptions. If no issues were found, say so, without implying the code is proven safe.

## Guardrails

- Report evidence, not speculative style preferences.
- Clearly label unverified concerns.
- Do not fix code, commit or push as part of the review unless explicitly requested.
