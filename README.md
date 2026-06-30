# exemplar-memory-plugin

Claude Code plugin for **Exemplar long-term memory** — store and recall preferences and facts across sessions.

## Setup (first time)

### 1. Get an API key

1. Open [Exemplar Console](https://console.exemplar.dev) → **Account** → **Tokens**
2. Create or copy an org-scoped key that starts with `eis_`

No base URL or org id to configure — the key carries your org context.

### 2. Install the plugin

In Claude Code:

```bash
/plugin marketplace add Exemplar-Dev/exemplar-memory-plugin
/plugin install exemplar-memory@exemplar-plugins
```

When prompted, paste your **`eis_` API key**.

### 3. Confirm it is connected

```bash
/reload-plugins
/mcp
```

You should see **`exemplar-mcp`** connected and **`memory_tool`** in the tool list.

MCP endpoint: `https://production-api.exemplar.dev/mcp`

---

## How to use it (end user)

You do **not** call `memory_tool` yourself. Talk to Claude in normal language; it uses the tool when memory is relevant.

### Remember something

```
Remember that I prefer dark mode and bullet-point answers.
```

```
Note for future sessions: our deploy window is Tuesday 2–4pm UTC.
```

Claude stores a scoped memory (usually under your `user_id`).

### Recall later (same or new session)

```
What do you know about my UI preferences?
```

```
What did I ask you to remember about deploy windows?
```

Claude searches memory before answering.

### List or update

```
List what you have stored about me.
```

```
Update my theme preference — I switched to light mode.
```

### Forget

```
Forget what you know about my dietary restrictions.
```

Claude should confirm before deleting.

---

## Tips

| Topic | Guidance |
|-------|----------|
| **Who is the memory for?** | Memories are scoped by `user_id` (and optionally `session_id`, `agent_id`, `app_id`). If you work alone, defaults are fine. For a shared machine, say *"remember this for user alice"* so recall stays scoped. |
| **What to store** | Preferences, standing rules, facts you explicitly asked to remember — not one-off chat context. |
| **New session** | Install once; memories persist across Claude Code sessions. |
| **Approval** | First `memory_tool` use may ask you to approve the tool — allow it to enable recall and storage. |

---

## Troubleshooting

| Problem | What to try |
|---------|-------------|
| No `exemplar-mcp` in `/mcp` | Run `/reload-plugins` or restart Claude Code |
| Connection / auth errors | Check the `eis_` key in plugin settings; create a new token if needed |
| Memories not found | Use the same `user_id` scope when recalling; try *"search memory for …"* explicitly |
| Plugin not listed | Re-run `/plugin marketplace add Exemplar-Dev/exemplar-memory-plugin` |

---

## What gets installed

- **MCP** — `memory_tool` with actions: `add`, `search`, `list`, `get`, `update`, `delete`, `bulk_delete`
- **Skill** — guides Claude on when and how to use memory (invoked automatically)

See [exemplar-memory/README.md](./exemplar-memory/README.md) for a short reference.
