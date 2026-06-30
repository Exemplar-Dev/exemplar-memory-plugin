# Exemplar Memory

Persistent memory for Claude Code via **`memory_tool`** — scoped facts and preferences with semantic recall.

## Install

```bash
/plugin marketplace add exemplar-dev/exemplar-memory-plugin
/plugin install exemplar-memory@exemplar-plugins
```

When prompted, enter your org-scoped `eis_` API key from
[Exemplar Console](https://console.exemplar.dev) → Account → Tokens.

Restart the session or run `/reload-plugins`, then check `/mcp`.

## `memory_tool` actions

`add`, `search`, `list`, `get`, `update`, `delete`, `bulk_delete`
