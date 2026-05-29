---
name: executing-roadmap
description: Use when the human wants to execute work from a project roadmap, continue the next roadmap item, run one or more roadmap features, or resume roadmap-driven development.
---

# Executing Roadmap

## Overview

Execute project work from `docs/superpowers/roadmap.md` without losing the project-level map. This skill is a lightweight orchestrator: it decides the stage, routes to the right skill, and keeps roadmap progress closed-loop.

Do not preload every related skill. Invoke only the skill needed for the current stage.

## Modes

### Guided Mode

Default mode. Use when the human asks to continue, do the next item, or implement one roadmap item without asking for full automation.

- Execute one roadmap item at a time.
- Ask for confirmation at major decision points.
- Update roadmap and commit after the item is verified when the human wants commits.
- Ask whether to continue after each completed item.

### Autopilot Mode

Use only when the human explicitly asks to run multiple items, finish a milestone, do N features, or keep going until blocked.

- Loop through selected roadmap items in priority order.
- One feature equals one implementation plan and one commit by default.
- Stop on unclear requirements, test failures, roadmap conflicts, security/billing/data-shape decisions, or dirty state that cannot be safely attributed.
- Do not push unless the human explicitly asks.

## Startup State Check

Start by reporting the current execution state:

```text
Roadmap execution active.

Current state:
- Roadmap: found / missing
- Target: explicit item / next recommended step / milestone / multiple items / unclear
- Mode: guided / autopilot
- Requested scope: one item / N items / milestone / unknown
- Requirement analysis: found / missing / not needed yet
- Engineering plan: found / missing / stale
- Implementation state: not started / in progress / needs verification / done
- Git state: clean / dirty / unknown
- Commit policy: ask / commit each item / no commit / push requested
- Roadmap update: pending / not needed yet

Next action:
```

If there is no roadmap and the human wants roadmap-driven execution, use `superpowers:managing-project-roadmap` to create or initialize it before planning work.

## Intent Detection

Classify the user's request:

| User intent | Examples | Action |
|-------------|----------|--------|
| Explicit item | "Implement M2 refund flow" | Execute that item |
| Next item | "Continue", "do next", "推进下一个" | Use `Next Recommended Steps` |
| Milestone | "Finish M1" | Execute items under that milestone |
| Multiple items | "Do 3 features", "keep going" | Autopilot loop |
| Planning/status only | "What remains?", "review roadmap" | Use roadmap management only |
| Unclear | No target or mode | Ask one concise clarification |

## Single Item Loop

For each selected roadmap item:

1. **Read roadmap** with `superpowers:managing-project-roadmap`.
2. **Select item** and confirm it matches the requested target.
3. **Clarify feature** with `superpowers:brainstorming` when intent or boundaries are unclear.
4. **Analyze requirements** with `superpowers:analyzing-requirements` when input is a PRD, prototype, meeting note, long feature description, or lacks acceptance criteria.
5. **Write plan** with `superpowers:writing-plans`; the plan must include `Roadmap Link`.
6. **Implement plan** with `superpowers:executing-plans` or `superpowers:subagent-driven-development`, based on task shape and available subagents.
7. **Use TDD** with `superpowers:test-driven-development` when behavior, business rules, bugs, parsers, state machines, permissions, or data changes require test-first coverage.
8. **Debug systematically** with `superpowers:systematic-debugging` if tests fail or behavior is unexpected.
9. **Verify completion** with `superpowers:verification-before-completion`.
10. **Run QA flow** only when relevant: `qa-testing-workflow`, `qa-test-design`, `qa-api-test-design`, or `qa-risk-review`.
11. **Update roadmap** with `superpowers:managing-project-roadmap` after verification.
12. **Commit** if requested by the human or required by the selected mode. Generate the message from the actual diff.

## Multi Item Loop

Autopilot repeats the single item loop.

Before each next item:
- Confirm the previous item was verified.
- Confirm roadmap was updated or explicitly did not need an update.
- Confirm git state is clean or attributable to the current item.
- Re-read roadmap because priorities may have changed.
- Stop if the next item is unclear, blocked, or now lower priority than an unresolved risk.

## Commit Policy

- Guided mode: ask before committing unless the human requested commits.
- Autopilot mode: commit each completed feature by default.
- One roadmap item should usually produce one commit.
- Do not bundle unrelated roadmap items in one commit.
- Do not push unless the human explicitly asks.
- Never reuse a commit message from a plan; generate it from the staged diff.

## Roadmap Update Policy

After each verified item:
- Update linked milestone evidence or status when the item affects project progress.
- Add meaningful work to `Recent Progress`.
- Keep `Next Recommended Steps` to 1-3 items.
- Put new unconfirmed ideas in `Inbox` as `proposed`.
- Do not add tiny/local changes as new roadmap items.
- Do not mark milestones done without evidence.

## Skill Routing

Core routing:
- Project map and next work: `managing-project-roadmap`
- Feature intent: `brainstorming`
- Feature requirements: `analyzing-requirements`
- Feature plan: `writing-plans`
- Implementation: `executing-plans` or `subagent-driven-development`
- Behavior tests: `test-driven-development`
- Completion proof: `verification-before-completion`
- Branch completion: `finishing-a-development-branch`

Optional routing:
- QA workflows only when the item is business-facing, API-facing, or high-risk.
- `creating-agents-guidance` when project-level AI guidance is missing and the human confirms adding it.
- `personal-info-cloudmind` only when user-specific history or preferences are needed and CloudMind is available.

Do not invoke unrelated skills just because they exist.
