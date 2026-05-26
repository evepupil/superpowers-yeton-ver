---
name: executing-plans
description: Use when you have a written implementation plan to execute in a separate session with review checkpoints
---

# Executing Plans

## Overview

Load plan, review critically, execute all tasks, report when complete. Plans may be adaptive: they can define intent, likely touchpoints, risks, and verification without fixed paths or complete code.

**Announce at start:** "I'm using the executing-plans skill to implement this plan."

**Note:** Tell your human partner that Superpowers works much better with access to subagents. The quality of its work will be significantly higher if run on a platform with subagent support (such as Claude Code or Codex). If subagents are available, use superpowers:subagent-driven-development instead of this skill.

## The Process

### Step 1: Load and Review Plan
1. Read plan file
2. Review critically - identify questions, concerns, assumptions, and stop conditions
3. If concerns: Raise them with your human partner before starting
4. If no concerns: Create TodoWrite and proceed

### Step 2: Execute Tasks

For each task:
1. Mark as in_progress
2. Inspect the live code around likely touchpoints before finalizing paths or abstractions
3. Implement the task intent within the plan's boundaries
4. Run verifications as specified or choose equivalent risk-driven checks when the plan is adaptive
5. Generate any commit message from the actual diff and project convention
6. Mark as completed

### Step 3: Complete Development

After all tasks complete and verified:
- Announce: "I'm using the finishing-a-development-branch skill to complete this work."
- **REQUIRED SUB-SKILL:** Use superpowers:finishing-a-development-branch
- Follow that skill to verify tests, present options, execute choice

## When to Stop and Ask for Help

**STOP executing immediately when:**
- Hit a blocker (missing dependency, test fails, instruction unclear)
- Plan has critical gaps preventing starting
- You don't understand an instruction
- Live code contradicts a plan assumption and adapting would change public behavior, data shape, security, billing, or compatibility
- Verification fails repeatedly

**Ask for clarification rather than guessing.**

## When to Revisit Earlier Steps

**Return to Review (Step 1) when:**
- Partner updates the plan based on your feedback
- Fundamental approach needs rethinking

**Don't force through blockers** - stop and ask.

## Remember
- Review plan critically first
- Follow confirmed mechanical steps exactly when the plan is mechanical
- Treat likely touchpoints in adaptive plans as starting points, not locked paths
- Don't skip verifications
- Reference skills when plan says to
- Stop when blocked, don't guess
- Don't use prewritten commit messages from the plan; generate them from the actual diff
- Never start implementation on main/master branch without explicit user consent

## Integration

**Required workflow skills:**
- **superpowers:using-git-worktrees** - Ensures isolated workspace (creates one or verifies existing)
- **superpowers:writing-plans** - Creates the adaptive or mechanical plan this skill executes
- **superpowers:finishing-a-development-branch** - Complete development after all tasks
