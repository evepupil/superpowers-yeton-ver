# Superpowers

Superpowers is a complete software development methodology for your coding agents, built on top of a set of composable skills and some initial instructions that make sure your agent uses them.

## Quickstart

Give your agent Superpowers: [Claude Code](#claude-code), [Codex CLI](#codex-cli), [Codex App](#codex-app), [Factory Droid](#factory-droid), [Gemini CLI](#gemini-cli), [OpenCode](#opencode), [Cursor](#cursor), [GitHub Copilot CLI](#github-copilot-cli).

## How it works

It starts from the moment you fire up your coding agent. As soon as it sees that you're building something, it *doesn't* just jump into trying to write code. Instead, it steps back and asks you what you're really trying to do. 

Once it's teased a spec out of the conversation, it shows it to you in chunks short enough to actually read and digest. 

After you've signed off on the design, your agent puts together an adaptive engineering plan. The plan fixes the goal, boundaries, risks, likely touchpoints, and verification strategy without pretending the final file layout or implementation code is knowable before inspecting the live code. It emphasizes YAGNI (You Aren't Gonna Need It), DRY, and risk-driven verification.

Next up, once you say "go", it launches a *subagent-driven-development* process, having agents work through each engineering task, inspect the live code before choosing final paths and abstractions, review their work, and replan when assumptions turn out to be wrong. It's not uncommon for Claude to be able to work autonomously for a couple hours at a time while staying aligned with the plan's intent.

There's a bunch more to it, but that's the core of the system. And because the skills trigger automatically, you don't need to do anything special. Your coding agent just has Superpowers.


## Sponsorship

If Superpowers has helped you do stuff that makes money and you are so inclined, I'd greatly appreciate it if you'd consider [sponsoring my opensource work](https://github.com/sponsors/obra).

Thanks! 

- Jesse


## Installation

Installation differs by harness. If you use more than one, install Superpowers separately for each one.

### Claude Code

Install this fork directly from GitHub:

```bash
/plugin install github:evepupil/superpowers-yeton-ver
```

### Codex CLI

Install this fork from its GitHub repository.

- Open the plugin search interface:

  ```bash
  /plugins
  ```

- Add the GitHub repository:

  ```bash
  https://github.com/evepupil/superpowers-yeton-ver
  ```

- Select `Install Plugin`.

### Codex App

Install this fork from its GitHub repository.

- In the Codex app, click on Plugins in the sidebar.
- Add or install from `https://github.com/evepupil/superpowers-yeton-ver`.
- Follow the prompts.

### Factory Droid

- Register the marketplace:

  ```bash
  droid plugin marketplace add https://github.com/evepupil/superpowers-yeton-ver
  ```

- Install the plugin:

  ```bash
  droid plugin install superpowers@superpowers
  ```

### Gemini CLI

- Install the extension:

  ```bash
  gemini extensions install https://github.com/evepupil/superpowers-yeton-ver
  ```

- Update later:

  ```bash
  gemini extensions update superpowers
  ```

### OpenCode

OpenCode uses its own plugin install; install Superpowers separately even if you
already use it in another harness.

- Tell OpenCode:

  ```
  Fetch and follow instructions from https://raw.githubusercontent.com/evepupil/superpowers-yeton-ver/refs/heads/main/.opencode/INSTALL.md
  ```

- Detailed docs: [docs/README.opencode.md](docs/README.opencode.md)

### Cursor

- In Cursor Agent chat, install this fork directly from GitHub:

  ```text
  /add-plugin https://github.com/evepupil/superpowers-yeton-ver
  ```

### GitHub Copilot CLI

Install this fork directly from GitHub:

```bash
copilot plugin install github:evepupil/superpowers-yeton-ver
```

## The Basic Workflow

1. **brainstorming** - Activates before writing code. Refines rough ideas through questions, explores alternatives, presents design in sections for validation. Saves design document.

2. **using-git-worktrees** - Activates after design approval. Creates isolated workspace on new branch, runs project setup, verifies clean test baseline.

3. **writing-plans** - Activates with approved design. Creates an adaptive engineering plan with goals, boundaries, known context, likely touchpoints, risks, and verification strategy. Mechanical step-by-step handoff plans are opt-in.

4. **subagent-driven-development** or **executing-plans** - Activates with plan. Dispatches fresh subagent per task with two-stage review (spec compliance, then code quality), or executes in batches with human checkpoints.

5. **test-driven-development** - Activates during implementation when behavior changes need test-first development. It enforces RED-GREEN-REFACTOR for those changes; plans still choose verification by risk rather than forcing low-value mock tests everywhere.

6. **requesting-code-review** - Activates between tasks. Reviews against plan, reports issues by severity. Critical issues block progress.

7. **finishing-a-development-branch** - Activates when tasks complete. Verifies tests, presents options (merge/PR/keep/discard), cleans up worktree.

**The agent checks for relevant skills before any task.** Mandatory workflows, not suggestions.

## What's Inside

### Skills Library

**Testing**
- **test-driven-development** - RED-GREEN-REFACTOR cycle (includes testing anti-patterns reference)

**QA Testing**
- **qa-testing-workflow** - Staged QA workflow coordination with confirmation gates
- **qa-requirement-analysis** - Convert requirements and prototypes into testable QA materials
- **qa-test-design** - Business-oriented test point design
- **qa-api-test-design** - Optional API test design when code or API docs are available
- **qa-risk-review** - QA-focused review of code changes and test risk

**Debugging**
- **systematic-debugging** - 4-phase root cause process (includes root-cause-tracing, defense-in-depth, condition-based-waiting techniques)
- **verification-before-completion** - Ensure it's actually fixed

**Collaboration** 
- **brainstorming** - Socratic design refinement
- **writing-plans** - Adaptive engineering plans
- **executing-plans** - Batch execution with checkpoints
- **dispatching-parallel-agents** - Concurrent subagent workflows
- **requesting-code-review** - Pre-review checklist
- **receiving-code-review** - Responding to feedback
- **using-git-worktrees** - Parallel development branches
- **finishing-a-development-branch** - Merge/PR decision workflow
- **subagent-driven-development** - Fast iteration with two-stage review (spec compliance, then code quality)

**Meta**
- **writing-skills** - Create new skills following best practices (includes testing methodology)
- **using-superpowers** - Introduction to the skills system

## Philosophy

- **Risk-Driven Verification** - Match tests, smoke checks, and manual probes to the real failure modes
- **Systematic over ad-hoc** - Process over guessing
- **Complexity reduction** - Simplicity as primary goal
- **Evidence over claims** - Verify before declaring success

Read [the original release announcement](https://blog.fsck.com/2025/10/09/superpowers/).

## Contributing

The general contribution process for Superpowers is below. Keep in mind that we don't generally accept contributions of new skills and that any updates to skills must work across all of the coding agents we support.

1. Fork the repository
2. Switch to the 'dev' branch
3. Create a branch for your work
4. Follow the `writing-skills` skill for creating and testing new and modified skills
5. Submit a PR, being sure to fill in the pull request template.

See `skills/writing-skills/SKILL.md` for the complete guide.

## Updating

Superpowers updates are somewhat coding-agent dependent, but are often automatic.

## License

MIT License - see LICENSE file for details

## Community

Superpowers is built by [Jesse Vincent](https://blog.fsck.com) and the rest of the folks at [Prime Radiant](https://primeradiant.com).

- **Discord**: [Join us](https://discord.gg/35wsABTejz) for community support, questions, and sharing what you're building with Superpowers
- **Issues**: https://github.com/evepupil/superpowers-yeton-ver/issues
- **Release announcements**: [Sign up](https://primeradiant.com/superpowers/) to get notified about new versions
