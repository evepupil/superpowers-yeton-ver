---
name: analyzing-requirements
description: Use when development work is based on product requirements, PRDs, prototypes, meeting notes, user stories, or long feature descriptions that need clarification before implementation planning.
---

# Analyzing Requirements

## Overview

Turn product or business requirements into a development-ready requirement analysis before implementation planning. The output clarifies what must be built, what is out of scope, what is unknown, and what engineering risks matter.

This skill is for development planning. It does not produce test cases; use `qa-requirement-analysis` and `qa-test-design` for QA outputs.

## When to Use

Use this when:
- The human provides a PRD, prototype, meeting note, user story, or long feature description.
- The request says to build from requirements, analyze requirements, or write a plan from product material.
- Requirements are scattered across chat, files, screenshots, or links.
- Acceptance criteria, permissions, states, or business rules are missing or ambiguous before `writing-plans`.

Skip this for tiny changes where the requirement fits in a few clear sentences. In that case, write a short requirement summary inline and proceed.

## Core Rules

- Do not invent requirements.
- Treat uncertain details as open questions.
- Do not lock implementation paths, directory structures, class names, or API names.
- Do not treat current code behavior as the business requirement unless the human says it is authoritative.
- Use business language for requirements and implementation language only for engineering implications.

## Process

1. Inspect the provided requirement material and any relevant repository context.
2. Separate confirmed facts from assumptions and questions.
3. Identify users, permissions, flows, states, business rules, acceptance criteria, and engineering implications.
4. Ask only for missing information that blocks scope, safety, compatibility, billing, permissions, data shape, or acceptance.
5. Produce either a lightweight inline analysis or a saved requirement analysis document.

## Output

For non-trivial work, save to:

`docs/superpowers/requirements/YYYY-MM-DD-<feature-name>.md`

Use this structure:

```markdown
# [Feature] Requirement Analysis

## Goal
[Business outcome, not implementation.]

## Scope
### In Scope
### Out of Scope

## Users and Permissions
[Actors, visibility, authority, and permission differences.]

## User Flow
[Main flow, branches, failures, empty/loading/error states when relevant.]

## Business Rules
[Fields, states, limits, calculations, timing, idempotency, compatibility.]

## Acceptance Criteria
[Observable outcomes the human can verify.]

## Engineering Implications
[Likely data, API, module, migration, security, billing, permission, compatibility, or operational impact.]

## Open Questions
| Question | Impact if unresolved | Suggested owner |
|----------|----------------------|-----------------|
```

For simple work, use a short inline version:

```markdown
Requirement summary:
- Goal:
- In scope:
- Out of scope:
- Acceptance:
- Open questions:
```

## Handoff to Planning

Before invoking `writing-plans`, confirm the requirement analysis is usable:
- Scope is explicit.
- Acceptance criteria exist.
- High-risk unknowns are listed.
- Implementation paths remain flexible.

If blockers remain, ask the human whether to resolve them now or carry them into the plan as risks.
