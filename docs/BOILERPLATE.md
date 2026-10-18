# Boilerplate guide

This guide explains how to adapt and maintain the starter. For its purpose,
contents and quick start, see the [repository README](../README.md).

## Separate repository documentation from your project

The root README describes this GitHub repository. When starting your own project,
replace it with the contents of `templates/README.md`, then fill in your project's
purpose, runtime versions, setup and verified commands. The template contains no
links that depend on its original directory, so it can be copied to the root.

Keep application documentation in your README, working conventions in `AGENTS.md`
and explanations of the starter in this guide. Maintain executable commands in
one place: your application's README. Other documents should refer to it.

`TODO` means an unanswered question, not an established requirement. The example
in `docs/DECISIONS.md` is not an accepted architecture decision. Complete these
skeletons only as actual decisions emerge. Remove unneeded documents along with
their references. The optional `src/` and `tests/` directories can be renamed or
removed; their `.gitkeep` files are unnecessary once they contain real files.

## How the assistants discover instructions and skills

An agent here is a coding assistant with repository and tool access. Selecting a
different model inside Copilot is not necessarily the same as starting a separate
Codex or Claude Code integration. Discovery depends on the actual assistant,
installed version and settings.

| Assistant | Project instructions | Skill entrypoints |
| --- | --- | --- |
| GitHub Copilot in VS Code | `AGENTS.md` | `.agents/skills/` and `.claude/skills/` |
| OpenAI Codex, CLI or extension | `AGENTS.md` | `.agents/skills/` |
| Claude Code, CLI or extension | `CLAUDE.md` imports `AGENTS.md` | `.claude/skills/` points to `.agents/skills/` |

Some newer Claude Code versions also load `AGENTS.md` directly under certain
conditions. Keeping the small import supports sessions that still need it. These
instructions target coding tools with repository access, not ordinary chat
interfaces without a connected project.

The Claude skill entrypoints contain instructions to read the canonical workflow.
Unlike the `@AGENTS.md` import in `CLAUDE.md`, these links are reading instructions,
not automatic imports. Check that the canonical file is actually opened on first
use. No symlinks are required, which keeps Windows checkouts straightforward.

Copilot may discover both entrypoints for the same workflow. If duplicate entries
appear, choose the one under `.agents/skills/`; both lead to the same instructions.
No discovery order is assumed. The `project-` name prefix avoids collisions with
bundled commands.

## Verify discovery

Start a new session at the repository root and ask:

> Read the applicable project instructions. List unanswered project questions
> and the two review skills with their file paths. Do not modify any files.

The answer alone does not prove automatic discovery. Also check the tool's UI:

- **Copilot:** Run `Chat: Open Customizations` from the Command Palette and inspect
  instructions and skills for the selected assistant. In Local harness sessions,
  `chat.useAgentsMdFile` can disable `AGENTS.md` discovery.
- **Claude Code:** Use `/context` to inspect project memory files and look for
  `project-` skills in the `/` menu.
- **Codex:** Look for the project skills in the skill menu or through `/skills`,
  where supported by the interface.

If discovery fails, check the working directory, selected assistant, file-name
capitalization and skill headers. Restart the session after changes. Personal or
organization settings can restrict project customizations. Full interactive
sessions across all three products were not run as part of this starter's review.

## Use the skills

Project instructions apply according to the assistant's discovery rules. Skills
are task-specific workflows loaded when needed. Automatic selection based on a
skill's description is possible, but not guaranteed.

| Workflow | Copilot | Codex | Claude Code |
| --- | --- | --- | --- |
| Code review | Ask to use `project-code-review` | `$project-code-review` | `/project-code-review` |
| Security review | Ask to use `project-security-review` | `$project-security-review` | `/project-security-review` |

Specify the scope, for example uncommitted changes or the last commit. Both
workflows report findings and suggest fixes. Implementing fixes requires a task
that authorizes that work. A review does not prove correctness or replace tests.

A useful implementation request states the goal, constraints and expected result:

> Add identifier validation to the existing form. Preserve its error display and
> submitted values. Respect the documented PHP version. Check valid and invalid
> inputs and explain the change briefly.

The project instructions call for focused edits, preservation of existing
behavior and honest verification reports. Inspect the diff and check results
before committing. Commits, pushes and deployments must be authorized by the
task. Record lasting discoveries in the existing project documents so another
session can continue from them.

## Add a skill

1. Create `.agents/skills/project-my-workflow/SKILL.md`:

   ```markdown
   ---
   name: project-my-workflow
   description: Explain what the workflow does and when to use it.
   ---

   # My workflow

   1. Identify the requested scope.
   2. Perform the documented steps and verify the result.
   ```

2. Create `.claude/skills/project-my-workflow/SKILL.md` with the same header.
   Add this reading instruction below it:

   ```markdown
   Read and follow [the canonical workflow](../../../.agents/skills/project-my-workflow/SKILL.md).
   Resolve this link relative to this file, not the terminal working directory.
   ```

3. Keep names, descriptions and links consistent. Maintain the actual procedure
   only in `.agents/skills/`.

The relative link goes up **three** levels from the Claude skill directory to the
repository root. Add supporting scripts and references only when needed. Do not
assume that optional fields for tool permissions or invocation controls behave
the same across assistants.

## What to configure next

Before application development, provide real project requirements and working
verification commands. Additional agent configuration cannot replace them. The
owner should choose a suitable license if reuse is to be governed by explicit
terms; this starter does not make that choice on their behalf.

Add further configuration only for a concrete need:

- CI once real tests or other reproducible checks exist.
- VS Code tasks and debug configuration once the commands are known.
- File-specific rules when different parts need different conventions.
- MCP servers, hooks or specialized agents for a workflow that benefits from them.

Extra prompt files or copies of shared rules are unnecessary for getting started.
`.editorconfig` requires editor or extension support; its presence alone does not
format source code.

`.gitignore`, search exclusions and Markdown instructions are **not access
controls**. Keep secrets outside the workspace where practical. Tool permissions
and operating-system permissions determine actual access. The `.env.example`
exception is intended for placeholders only.

## Review outcome

The original structure and Claude relative links were valid. Skill names were
made more specific to avoid bundled-command collisions. The repository README
now explains the starter's contents and usage; a separate README skeleton serves
new projects. Working rules address existing changes, open requirements and
missing test infrastructure. No runtime dependencies or automatic command
execution were needed.

## Official references

Support can change. Consult the documentation for the installed version when
behavior differs.

- [VS Code: project instructions](https://code.visualstudio.com/docs/agent-customization/custom-instructions)
- [VS Code: Agent Skills](https://code.visualstudio.com/docs/agent-customization/agent-skills)
- [Codex: skills](https://developers.openai.com/codex/skills/)
- [Claude Code: skills](https://code.claude.com/docs/en/skills)
- [Claude Code: project instructions and imports](https://code.claude.com/docs/en/memory)
- [Agent Skills: open specification](https://agentskills.io/specification)
