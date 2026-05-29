---
name: managing-project-roadmap
description: Use when starting a new project, planning next work, asking project status, choosing what to build next, or completing work that may change project-level progress.
---

# Managing Project Roadmap

## Overview

Maintain a lightweight project-level roadmap so the agent does not rely on recent chat context to decide what matters next. The roadmap is the project map, not a log of every change.

Default file:

`docs/superpowers/roadmap.md`

## When to Use

Use this when:
- The human asks what to build next, what remains, project status, roadmap, backlog, milestones, or planning direction.
- A new project starts and no roadmap exists.
- `writing-plans` is about to plan a feature and a roadmap exists or should be created.
- Work is complete and roadmap status may need to be updated before completion is reported.
- A new feature request does not fit the current roadmap.

Do not use this to replace feature-level `brainstorming`, `analyzing-requirements`, or `writing-plans`.

## Roadmap Scope

Roadmap tracks project-level state:
- Product goal
- Current phase
- Milestones and their status
- Active work
- Next recommended steps
- Confirmed backlog
- Proposed inbox items
- Project-level risks and unknowns
- Important decisions
- Recent progress evidence

Roadmap should not track every small edit, local refactor, style change, copy tweak, or implementation detail.

## Initialization

If no roadmap exists and the project is new or the human asks for project planning, propose creating `docs/superpowers/roadmap.md`. Ask for confirmation before writing unless the human explicitly asked to create it.

Use this template:

```markdown
# Project Roadmap

## Product Goal

## Current Phase

## Milestones
| ID | Milestone | Goal | Status | Priority | Depends On | Evidence |
|----|-----------|------|--------|----------|------------|----------|

## Active Work
- None yet.

## Next Recommended Steps
1.

## Inbox
| Item | Source | Status | Triage |
|------|--------|--------|--------|

## Backlog
| Item | Priority | Reason | Status |
|------|----------|--------|--------|

## Risks and Unknowns
| Risk/Question | Impact | Status |
|---------------|--------|--------|

## Decision Log
| Date | Decision | Reason | Evidence |
|------|----------|--------|----------|

## Recent Progress
- None yet.
```

## Update Rules

Classify every proposed update before editing:

1. **Project-level change:** Update milestones, phase, backlog, risks, decisions, or next steps.
2. **Linked feature progress:** Update the linked milestone evidence or `Recent Progress`.
3. **Small/local change:** Do not update roadmap unless the human asks. State that no roadmap update is needed.
4. **New unconfirmed idea:** Put it in `Inbox` as `proposed`; do not promote to backlog or milestones without user confirmation.

Never mark a milestone complete without evidence such as commits, merged PRs, passing verification, shipped behavior, or explicit human confirmation.

Do not change milestone priority, product direction, or scope unless the human confirms it.

## Size Control

Keep roadmap readable:
- `Active Work`: maximum 3 items.
- `Next Recommended Steps`: maximum 3 items.
- `Recent Progress`: maximum 20 entries; summarize or archive older detail if needed.
- `Inbox`: if it grows beyond 20 items, ask the human to triage.
- Do not create a new milestone for every feature.
- Prefer updating existing milestone evidence over adding nested mini-milestones.
- Edit incrementally; do not rewrite the whole file unless the human asks for a roadmap cleanup.

## Feature Plan Link

When `writing-plans` creates a feature plan and roadmap exists, the plan should include:

```markdown
## Roadmap Link
- Milestone:
- Roadmap item:
- Completion update required: yes/no
```

If the feature does not fit the roadmap, ask whether to add it to `Inbox` before planning. If the human wants to proceed without roadmap changes, note the mismatch as a planning risk.

## Completion Update

Before reporting a feature complete:

1. Read `docs/superpowers/roadmap.md` if it exists.
2. Read the feature plan's `Roadmap Link` if present.
3. Decide whether the completed work changes project-level status.
4. Update only the relevant roadmap sections.
5. Keep the next recommendation list to 1-3 items.

If no roadmap exists, mention this once for substantial feature work:

```text
This project does not have `docs/superpowers/roadmap.md`. Adding one would let future AI sessions track project status and next steps beyond the current context.
```

Do not block completion just because no roadmap exists.

## Status Language

Use conservative statuses:
- `proposed`
- `planned`
- `active`
- `blocked`
- `done`
- `parked`
- `out-of-scope`

Use evidence-backed wording. Say "appears complete based on <evidence>" when verification is indirect.
