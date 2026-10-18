---
name: project-security-review
description: Perform a focused security review of application code, configuration, or a proposed change. Use for explicit security audits, threat reviews, or vulnerability assessment requests; do not run offensive tests against live systems.
---

# Security review

## Goal

Find exploitable weaknesses and realistic security risks in the requested scope without changing application behavior.

## Procedure

1. Establish scope, exposed interfaces, data sensitivity and the threat model. Do not invent deployment properties.
2. Inspect relevant code and configuration for input handling, output encoding, injection, authentication, authorization, CSRF, sessions, SSRF, file paths, secret management and dependency risks as applicable.
3. Examine the actual data and control flow. Distinguish exploitable defects from defense-in-depth improvements.
4. Inspect existing security tests and documented protections; do not assume a control exists unless evidence supports it.
5. Suggest the smallest safe mitigation and a regression test for each confirmed issue.
6. Produce a report ordered by severity with evidence, affected files, preconditions, impact and remediation.
7. State limits, including code you could not inspect and tests you could not run.

## Guardrails

- Do not execute attacks against live or third-party targets.
- Do not expose secrets or sensitive records in reports.
- Do not implement fixes or alter infrastructure unless explicitly requested.
