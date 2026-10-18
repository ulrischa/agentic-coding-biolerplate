# Agentic Coding Boilerplate

A lightweight, language-neutral project starter for **GitHub Copilot in VS Code**,
**OpenAI Codex** and **Claude Code**. It provides shared project instructions,
reusable review workflows and documentation templates without imposing a framework
or adding runtime dependencies.

## Contents

| File / directory | Purpose |
| --- | --- |
| `AGENTS.md` | Shared working conventions, safety rules and verification expectations |
| `CLAUDE.md` | Imports the shared instructions for Claude Code |
| `.agents/skills/` | Canonical code-review and security-review workflows |
| `.claude/skills/` | Claude Code entrypoints pointing to the canonical workflows |
| `.editorconfig` | Shared text formatting conventions |
| `.gitignore` | Common exclusions for local configuration and generated files |
| `.vscode/extensions.json` | GitHub Copilot extension recommendations |
| `templates/README.md` | README skeleton for the project you build from this starter |
| `docs/ARCHITECTURE.md` | Architecture documentation skeleton |
| `docs/DECISIONS.md` | Lightweight template for significant technical decisions |
| `docs/BOILERPLATE.md` | Detailed setup and usage guide |
| `src/`, `tests/` | Optional source and test directory placeholders |

## Getting started

1. Clone or download this repository, or use GitHub's **Use this template** option
   if available. When adding it to an existing project, reconcile existing files
   and instructions before copying anything over them.
2. Open the repository root in VS Code and use your chosen coding assistant.
   The starter itself requires no WSL, administrator privileges or package install.
   Install and authenticate the assistant separately; its access requirements apply.
3. Replace this README with the contents of [templates/README.md](templates/README.md).
   Fill in your project purpose, supported runtime versions, setup instructions and
   real verification commands. The separate template keeps this repository's
   documentation out of your application's README.
4. Adapt `AGENTS.md`, `.gitignore` and `.editorconfig` to the project. Complete
   architecture and decision records only where useful; adjust or remove the
   placeholder directories as needed.
5. Start a new agent session at the repository root. Check that the shared
   instructions and review skills are available before assigning a small task.

The [guide](docs/BOILERPLATE.md) explains file responsibilities,
assistant discovery, troubleshooting and how to add a skill. Keep it as a reference
while replacing this README with your own project documentation.

## Using the review skills

| Workflow | GitHub Copilot | Codex | Claude Code |
| --- | --- | --- | --- |
| Code review | Ask to use `project-code-review` | `$project-code-review` | `/project-code-review` |
| Security review | Ask to use `project-security-review` | `$project-security-review` | `/project-security-review` |

Specify the scope, such as uncommitted changes or the last commit. Both workflows
report findings and suggested fixes; applying fixes requires a request that covers
that work. Automatic skill selection depends on the assistant and is not guaranteed.

The `project-` prefix avoids collisions with bundled commands. Claude entrypoints
reuse the canonical files without symlinks, keeping ordinary Windows checkouts
simple. Copilot may discover both entrypoints; they lead to the same workflow.

## Scope and verification

This repository contains configuration and documentation, not an application.
Application test, lint, build and run commands are **not configured**. Add commands
appropriate to your project rather than treating placeholders as successful checks.

Local file links, skill metadata and entrypoint paths, JSON syntax, ignore patterns
and text formatting were checked. Full interactive sessions across all three
assistants were not executed. Discovery and optional features depend on the installed
versions and settings; see the official references in the guide.

Custom agents, hooks, MCP servers, CI pipelines and additional prompt files are
optional extensions for concrete needs. Markdown instructions and ignore patterns
are not access controls; actual access depends on tool permissions and the environment.
