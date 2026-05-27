---
name: creating-agents-guidance
description: Use when creating, updating, auditing, completing, initializing, or bootstrapping project-level AI coding guidance files for Codex, Claude Code, Gemini CLI, Cursor, GitHub Copilot, OpenCode, Factory Droid, or general coding agents.
---

# Creating Agents Guidance

## Overview

Create or complete a project-level AI coding guidance file for the current repository. The goal is to give coding agents repo-specific engineering constraints before they implement, test, review, or commit code.

This skill produces guidance, not a hard gate. It improves agent behavior, but it cannot block `git commit`. If the human wants hard enforcement, recommend adding a git hook or CI gate separately.

## When to Use

Use this skill when:
- The human asks to create or update `AGENTS.md`, `CLAUDE.md`, `GEMINI.md`, Cursor rules, Copilot instructions, or AI coding guidance.
- The human asks to initialize, init, bootstrap, or set up a new project with AI coding rules.
- The human wants repo-level rules for type checks, formatting, tests, commit messages, or AI pre-commit review.
- A new project is about to be created and no AI guidance file exists yet.
- `writing-plans` is about to create a plan and the repository has no AI coding guidance file.

Do not use this for ordinary code changes, ordinary code review, or general project summaries unless the human asks for repo-level AI guidance.

## Supported Tools and Guidance Files

This fork's README advertises support for Claude Code, Codex CLI, Codex App, Factory Droid, Gemini CLI, OpenCode, Cursor, and GitHub Copilot CLI. Cover those tools when choosing guidance targets.

Use this mapping:

| Tool | Preferred project guidance file |
|------|---------------------------------|
| General agents / Codex CLI / Codex App | `AGENTS.md` |
| Claude Code | `CLAUDE.md` |
| Gemini CLI | `GEMINI.md` |
| Cursor | `.cursor/rules/ai-engineering-guardrails.mdc` |
| GitHub Copilot | `.github/copilot-instructions.md` |
| OpenCode | `AGENTS.md` unless the project already has an OpenCode-specific convention |
| Factory Droid | `AGENTS.md` unless the project already has a Droid-specific convention |

Notes:
- Cursor also supports `AGENTS.md` as a simpler alternative, but project rules in `.cursor/rules/*.mdc` are the preferred Cursor-native format.
- GitHub Copilot repository-wide custom instructions use `.github/copilot-instructions.md`; Copilot CLI can also load `AGENTS.md`.
- Claude Code can create project memory with its native init flow; when `CLAUDE.md` already exists, only add or update the guardrails block.

Check for existing guidance in this order:
- `AGENTS.md`
- `CLAUDE.md`
- `GEMINI.md`
- `.cursor/rules/*.mdc`
- `.github/copilot-instructions.md`
- Other obvious project-level AI instruction files already present in the repo

If the human names a target tool or file, use that target. If no target is specified:
- Update existing guidance files when present.
- If multiple supported-tool files already exist, update the relevant ones and keep content consistent.
- Prefer `AGENTS.md` when no guidance file exists and the human does not request a specific tool.
- For a new project where the human asks to support multiple AI coding tools, propose creating one common `AGENTS.md` plus tool-specific files only when useful.

## Native Init Awareness

Many AI coding tools have native init or project-rules flows that create their preferred guidance file. When the human is initializing a new project:
- Prefer the active tool's native guidance file when it is obvious.
- If the tool can run its own init command, tell the human it can create the native file, then this skill can add the shared engineering guardrails.
- If no tool is clear, propose `AGENTS.md` as the common cross-agent baseline.
- Do not create every possible tool file by default. Create only the files that match the human's target tools.

## Consent Gate

Never write or modify a guidance file without first telling the human what will be added and getting explicit confirmation.

Use a short confirmation message and name the files you plan to create or update:

```text
I can add project-level AI coding guidance before implementation. I will only add engineering constraints and will not rewrite existing business background, roadmap, or requirement notes.

Detected/target tools and files:
- General/Codex/OpenCode/Droid: AGENTS.md
- Claude Code: CLAUDE.md
- Gemini CLI: GEMINI.md
- Cursor: .cursor/rules/ai-engineering-guardrails.mdc
- GitHub Copilot: .github/copilot-instructions.md

I will create or update only the files that match this project and your target tools.

Planned content:
- detected project toolchain and real verification commands
- type/static/compile checks
- formatting strategy, without forcing whole-repo formatting
- test and regression-test requirements
- minimum verification before AI reports completion
- AI staged-diff review before AI-assisted commits
- Conventional Commits message rules
- rule to avoid unrelated broad refactors

Confirm and I will write it.
```

If the human declines, continue the original task without writing guidance.

## Repository Inspection

Before writing, inspect the live repository. Prefer existing evidence over assumptions:
- Package/config files: `package.json`, `pyproject.toml`, `setup.cfg`, `tox.ini`, `pom.xml`, `build.gradle`, `go.mod`, `Cargo.toml`, `.csproj`, `Makefile`
- CI workflows: `.github/workflows/*`, `.gitlab-ci.yml`, Azure or Jenkins files
- Existing commands in README or scripts
- Test directories and known test runners
- Formatting/lint/typecheck config files

Do not invent commands. If a command is not discoverable, write the intent and mark the command as project-specific to confirm.

## Additive Update Rules

If a guidance file already exists:
- Preserve existing user content, including business background, roadmap, architecture notes, and team-specific process.
- Do not rewrite, reorganize, or delete sections outside this skill's owned block.
- Add a new block named `## AI Engineering Guardrails` if missing.
- If that block already exists, update only that block.
- For Cursor `.mdc` files, preserve frontmatter and place the guardrails in the Markdown body.

If creating a new file:
- Keep it concise and repo-specific.
- Include only engineering guidance useful to coding agents.
- Do not add company-local or machine-local rules unless the human explicitly asks.
- For Cursor `.mdc`, include minimal frontmatter:

```yaml
---
description: AI engineering guardrails for this repository
globs: ["**/*"]
alwaysApply: true
---
```

## Guardrails Block

Use this structure for `## AI Engineering Guardrails`:

```markdown
## AI Engineering Guardrails

### Scope
- These rules apply to AI-assisted code changes in this repository.
- Keep changes focused on the current task. Do not perform unrelated broad refactors.
- If historical code does not meet these rules, improve only the touched area unless the task explicitly asks for cleanup.

### Project Toolchain
- Language/runtime:
- Package/build tool:
- Test framework:
- Existing verification commands:

### Type, Static, and Build Checks
- Run the repository's real type/static/compile checks before reporting completion when code changes affect behavior or public contracts.
- If no type/static check is configured, do not invent one; document the gap.

### Formatting and Linting
- Prefer project-configured lint/format commands.
- Do not run whole-repo formatting when historical formatting drift would create unrelated churn.
- At minimum, keep touched files consistent with nearby code and avoid introducing new lint noise.

### Tests
- Bug fixes should include a regression test when practical.
- New behavior should include unit, integration, or smoke coverage appropriate to the risk.
- If tests cannot be added or run, explain the reason and provide a manual verification path.

### AI Pre-commit Review
- Before AI-assisted commits, review the staged diff with `git diff --cached`.
- Check for missing tests, boundary cases, permission/auth risks, data consistency issues, accidental broad changes, and secrets.
- Fix critical or major findings before committing, or explicitly report why they remain.

### Commit Messages
- Use Conventional Commits: `<type>(<scope>): <summary>`.
- Prefer `feat`, `fix`, `refactor`, `test`, `docs`, `chore`, `build`, `ci`, `perf`, or `revert`.
- Generate the final message from the actual diff, not from an earlier plan.
```

Fill in `Project Toolchain` with detected facts. If something is unknown, write `Not detected` or `To confirm`, not a guess.

## Writing Style

- Be concrete and short.
- Prefer commands that were found in the repo.
- Keep formatting optional unless the repo already enforces it.
- Use "should" for guidance and "must" only for rules that protect correctness or safety.
- Make clear that AI pre-commit review applies when the AI is helping commit code.

## Completion Check

After writing or updating:
- Report the file path.
- Summarize the added or updated guardrails.
- Mention any commands or tooling that could not be detected.
- Do not claim hard enforcement unless a real hook or CI gate was added.
