---
name: writing-plans
description: Use when you have a spec or requirements for a multi-step task, before touching code
---

# Writing Plans

## Overview

Write an adaptive engineering plan that preserves developer judgment while making the work safe to start. The plan fixes the goal, boundaries, risks, and verification strategy. It does **not** pre-write implementation code, lock in new directory structures, or choose commit messages before the code exists.

The default output is an **Adaptive Engineering Plan**. Use a **Mechanical Handoff Plan** only when the human explicitly asks for a low-context agent handoff, a step-by-step implementation script, or complete task-by-task code.

**Announce at start:** "I'm using the writing-plans skill to create an adaptive engineering plan."

**Context:** If working in an isolated worktree, it should have been created via the `superpowers:using-git-worktrees` skill at execution time.

**Save plans to:** `docs/superpowers/plans/YYYY-MM-DD-<feature-name>.md`
- (User preferences for plan location override this default)

## AI Guidance Check

Before writing an implementation plan, check whether the repository has project-level AI coding guidance:
- `AGENTS.md`
- `CLAUDE.md`
- `GEMINI.md`
- `.cursor/rules/*.mdc`
- `.github/copilot-instructions.md`
- other project-level AI instruction files for the active coding tool

If no guidance file exists, do not write one automatically. Tell the human that the project has no AI coding guidance file and that adding one before implementation is recommended for AI coding best practice. Keep this advisory short and non-blocking:

```text
I don't see a project-level AI coding guidance file for this repository. For AI-assisted development, adding one before implementation usually improves consistency around verification commands, tests, commit messages, and AI pre-commit review.

I can add concise guidance first for the active AI coding tool, such as `AGENTS.md`, `CLAUDE.md`, `GEMINI.md`, Cursor rules, or Copilot instructions. It would cover toolchain checks, optional formatting strategy, test requirements, Conventional Commits, and staged-diff review. Confirm if you want me to add it before I write the implementation plan.
```

If the human confirms, use `superpowers:creating-agents-guidance` before writing the plan. If the human declines or wants to continue, proceed with the plan and note the absence as a planning risk only when relevant.

## Scope Check

If the spec covers multiple independent subsystems, it should have been broken into sub-project specs during brainstorming. If it wasn't, suggest breaking this into separate plans, one per subsystem. Each plan should produce working, testable software on its own.

## Planning Mode

### Default: Adaptive Engineering Plan

Use this for normal software work, especially existing codebases, architecture still being discovered, integration-heavy tasks, and work led by a human developer.

The plan should answer:
- What are we trying to change?
- What is explicitly out of scope?
- What code facts do we already know?
- Where are the likely touchpoints?
- What risks could make this fail?
- How will we verify the result?
- When should the implementer stop and revisit assumptions?

### Opt-in: Mechanical Handoff Plan

Use this only when the human explicitly asks for mechanical delegation to low-context workers. In that mode, the plan may include exact paths, detailed steps, and example code, but it must still avoid pretending that unknown code context is known.

## Adaptive Plan Header

Every adaptive plan MUST start with this header:

```markdown
# [Feature Name] Engineering Plan

> **For agentic workers:** This is an adaptive plan. First inspect the live codebase, then choose final paths, abstractions, and commit messages from the actual diff. Do not treat likely touchpoints as locked implementation instructions.

**Goal:** [One sentence describing the intended behavior or outcome]

**Non-Goals:** [What this plan deliberately does not change]

**Planning Mode:** Adaptive Engineering Plan

---
```

## Required Sections

### Goal and Boundaries

State the user-visible outcome, the system boundary, and the constraints that matter. Boundaries are more important than implementation guesses.

Include:
- In scope
- Out of scope
- Compatibility or migration constraints
- Assumptions that need confirmation during implementation

### Known Context

List facts already verified from the repository, docs, tests, runtime behavior, or user input. Do not list guesses as facts.

Good:
- "`src/routes/chat.ts` currently owns the SSE endpoint."
- "Existing tests use Vitest and MSW."

Bad:
- "Create `src/services/newService.ts`" when that structure has not been validated.
- "Use Redis key `foo:bar`" before the storage model is confirmed.

### Likely Touchpoints

Identify probable files, modules, APIs, or directories to inspect first. These are starting points, not locked paths.

Rules:
- Existing files may be listed as confirmed touchpoints when already inspected.
- New files must be described as candidate locations or naming constraints, not fixed paths.
- Do not design a new directory tree before reading the surrounding code patterns.
- If path choice depends on code discovered during implementation, say that explicitly.

### Risks and Open Questions

Call out the places where static planning is weak:
- Async or concurrent behavior
- Streaming, middleware, lifecycle, or transaction boundaries
- Data compatibility and migrations
- Auth, billing, privacy, or permission decisions
- External service contracts
- Performance, caching, and invalidation
- Test blind spots caused by excessive mocking

Open questions should be real blockers or implementation checkpoints, not filler.

### Verification Strategy

Use risk-driven verification. Do not force the same TDD template onto every change.

Required guidance:
- Business rules, parsers, state machines, concurrency, protocols, permissions, billing, and migrations need targeted tests or high-signal integration coverage.
- Glue code, config changes, copy changes, and simple wiring may be verified with lint, typecheck, smoke tests, or manual checks.
- Prefer tests that exercise real behavior. Avoid mocking trivial helpers just to satisfy a process.
- Include the commands or probes that are already known. If exact commands depend on project discovery, say what to find first.

### Execution Notes

Tell the implementer how to use the plan:
- Inspect live code before finalizing paths or abstractions.
- Let existing project patterns choose file placement.
- Update the plan or explain the deviation when an assumption is wrong.
- Generate commit messages from the actual diff at commit time, using the project's standard format such as Conventional Commits when applicable.
- Stop and ask when a decision changes public behavior, data shape, security, billing, or compatibility.

## Adaptive Task Shape

Tasks should be outcome-oriented, not line-by-line scripts:

```markdown
### Task N: [Outcome]

**Intent:** [What should be true after this task]

**Likely touchpoints:** [Existing files/modules to inspect first; candidate locations only for new files]

**Constraints:** [Patterns, compatibility, or user decisions that must be preserved]

**Risks:** [Task-specific risks]

**Verification:** [Targeted tests, commands, probes, or manual checks]

**Stop if:** [Signals that the plan assumption is wrong]
```

Keep tasks small enough to review, but not so small that the plan becomes a checklist of mechanical keystrokes. A task may involve reading, design adjustment, implementation, and verification when those steps are tightly coupled.

## Mechanical Handoff Rules

If the human explicitly requested a mechanical handoff plan:
- Say that you are switching to Mechanical Handoff mode.
- Include exact paths only after verifying they exist or clearly marking them as new planned files.
- Include code only where it reduces ambiguity and is grounded in known project patterns.
- Do not prewrite commit messages. State the commit boundary and require the implementer to generate the final message from the actual diff.
- Include a "Replan Trigger" for each task so agents stop when the live code contradicts the plan.

## Plan Failures

These are failures in both modes:
- Treating guessed paths as facts
- Locking a new directory structure before inspecting local patterns
- Prewriting production code for unknown context
- Requiring low-value mock tests for trivial helpers
- Omitting verification for high-risk behavior
- Forcing a stale plan after implementation disproves an assumption
- Hardcoding a commit message before seeing the final diff
- Writing vague placeholders such as "TBD", "TODO", "handle edge cases", or "add appropriate tests"

## Self-Review

After writing the plan, review it with fresh eyes:

1. **Spec coverage:** Can every requirement be mapped to a goal, boundary, task, or verification item?
2. **Fact vs guess:** Are known facts separated from likely touchpoints and assumptions?
3. **Path rigidity:** Did you avoid locking new files/directories unless the structure is already confirmed?
4. **Risk coverage:** Are the riskiest behaviors tied to concrete verification?
5. **Commit discipline:** Did you avoid prewriting commit messages and instead define commit boundaries?
6. **Replan triggers:** Does the plan tell implementers when to stop and revise assumptions?

Fix issues inline before handing off.

## Execution Handoff

After saving the plan, offer execution choices:

**"Plan complete and saved to `docs/superpowers/plans/<filename>.md`. Two execution options:**

**1. Subagent-Driven** - I coordinate task execution with fresh subagents, review between tasks, and replan when live code contradicts the plan

**2. Inline Execution** - Execute in this session using executing-plans, with checkpoints when assumptions change

**Which approach?"**

**If Subagent-Driven chosen:**
- **REQUIRED SUB-SKILL:** Use superpowers:subagent-driven-development
- Fresh subagent per task + two-stage review

**If Inline Execution chosen:**
- **REQUIRED SUB-SKILL:** Use superpowers:executing-plans
- Batch execution with checkpoints for review
