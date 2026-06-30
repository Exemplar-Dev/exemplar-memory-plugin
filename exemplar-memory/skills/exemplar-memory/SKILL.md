---
name: exemplar-memory
description: >-
  Use when the user wants to remember, recall, list, update, or forget personal facts,
  preferences, standing instructions, or agent-confirmed outcomes across sessions —
  e.g. "remember that I prefer dark mode", "what do you know about me?", "forget my
  dietary restrictions". Calls memory_tool via Exemplar MCP. Not for org documentation
  or runbooks.
---

# Exemplar long-term memory (`memory_tool`)

You have access to **`memory_tool`** on the Exemplar MCP server. Use it for **persistent,
scoped memory** about this user or session — not for general web search or static docs.

## When to use

- User says **remember**, **note**, **don't forget**, or confirms a preference → `add`
- User asks **what do you know about me**, **recall**, **my preferences** → `search` or `list`
- User wants to **correct** a stored fact → `update` (or `search` then `update`)
- User asks to **forget** or **delete** a memory → `delete` or `bulk_delete` (confirm first)

## Scopes (required on add)

At least one scope is required when storing memories:

| Param | Use |
|-------|-----|
| `user_id` | Per-user facts and preferences (most common) |
| `session_id` | Session-scoped context |
| `agent_id` | Agent-specific memory |
| `app_id` | Application-scoped memory |

If the user does not specify a user id, use a stable default such as `default` or derive
from context (e.g. git user email) and mention which scope you used.

## Actions

| Action | Purpose | Key params |
|--------|---------|------------|
| `add` | Store a fact | `content`, `user_id`, `memory_type`, optional scope fields |
| `search` | Semantic recall | `query`, `user_id`, `top_k`, `threshold` |
| `list` | List by scope | `user_id`, `limit` |
| `get` | One record | `memory_id` |
| `update` | Change a record | `memory_id`, `content`, optional scope/type |
| `delete` | Remove one | `memory_id` |
| `bulk_delete` | Remove by scope | at least one scope field |

## Memory types

- `fact` — general durable information (default)
- `preference` — likes/dislikes, settings, UI choices
- `tool_outcome` — confirmed result of an action
- `episodic` — short-lived turn summaries

## Tool call shape

```json
{
  "action": "search",
  "params": {
    "query": "UI theme preferences",
    "user_id": "alice",
    "top_k": 5
  }
}
```

## Guidelines

1. **Search before assuming** when the user refers to past preferences.
2. **Store concise third-person facts** — e.g. "User prefers dark mode and bullet-point answers."
3. **Confirm writes** briefly after `add`.
4. **Confirm deletes** before `delete` or `bulk_delete` unless the user was explicit.
5. **Do not invent memories** — only store what the user stated or you confirmed in conversation.
6. Org context is resolved from the API key — do not ask for organization id.

## Examples

- *Remember I'm vegetarian* → `add` with `memory_type: "fact"`, `user_id` set
- *What editor do I prefer?* → `search` with natural-language `query`
- *List everything you know about me* → `list` or `search` with broad query
- *Forget my dietary restrictions* → `search`, confirm matches, then `delete`
