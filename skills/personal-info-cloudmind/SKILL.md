---
name: personal-info-cloudmind
description: Use when answering requires user-specific personal information, preferences, history, project context, prior decisions, habits, notes, status, or "what do you know about me" details that are not already clear in the current conversation, especially when CloudMind MCP is available.
---

# Personal Info CloudMind

## Overview

Use CloudMind as the user's personal memory layer when a request depends on facts about the user that are not in the current thread. Retrieve narrowly, answer from evidence, and state when the lookup is insufficient.

This is an optional personal-context skill. If CloudMind MCP is not configured, do not block the user's task; say the lookup is unavailable and continue with current-thread information.

## Trigger

Use this skill before answering when the user asks about or would benefit from:

- The user's personal profile, preferences, habits, work style, priorities, or communication preferences.
- The user's past projects, previous decisions, prior requests, earlier troubleshooting, or "last time" context.
- Personal notes, journal-like records, status, plans, goals, invoices, finance metadata, health metadata, or other private library content.
- Ambiguous statements like "you should know", "based on my history", "what do you know about me", "remember my preference", or "use my context".

Do not use for general public facts, coding questions that only need the repo, or private information already provided in the current conversation.

## Workflow

1. Decide whether the answer depends on unknown user-specific context. If yes, query CloudMind before giving a substantive answer.
2. Prefer `mcp__cloudmind__search_assets_for_context` with `profile: "personal_review"`, `allowFallback: false`, and a small `topK` such as 5.
3. If the result is too sparse and broader context is still appropriate, run a second narrow query with `allowFallback: true` or use `mcp__cloudmind__ask_library_for_context` for a grounded summary.
4. Use `mcp__cloudmind__get_asset` only when the search result points to a specific asset whose details are needed.
5. Answer with the minimal personal detail needed for the user's task. Mention uncertainty, stale evidence, or lookup failure instead of guessing.

## Query Patterns

Use direct, privacy-scoped queries:

- "user preferences communication style work habits"
- "recent project decisions for <project/topic>"
- "personal notes status goals priorities"
- "what personal information is visible about the user"

For project-specific questions, include the project name or path. For personal-context audits, inspect metadata such as domain, document class, sensitivity, and AI visibility when available.

## Privacy Boundaries

- Do not overfetch. Start with small `topK` and focused terms.
- Do not expose sensitive raw snippets unless the user explicitly asks and it is necessary.
- Do not infer private facts from weak matches. Say the evidence is inconclusive.
- Do not treat CloudMind as always current. If the information could have changed, say the answer is based on retrieved memory and may be stale.
- If CloudMind MCP is unavailable, say the lookup could not be performed and continue only with current-thread information.

## Tool Preference

Prefer these CloudMind MCP tools:

- `search_assets_for_context`: retrieval-first, best default for evidence-rich context.
- `ask_library_for_context`: quick grounded synthesis when a summary is enough.
- `get_asset`: inspect a selected asset after retrieval.
- `search_assets_by_terms` or `search_terms`: metadata-driven lookup when content terms are unclear.

Do not use `list_mcp_resources` or `list_mcp_resource_templates` for CloudMind personal lookup; this MCP server may not expose resources/templates.
