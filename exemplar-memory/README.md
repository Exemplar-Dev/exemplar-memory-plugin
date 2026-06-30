# Exemplar Memory plugin

Bundled MCP + skill for **Claude Code** and **Codex**.

Full setup for all clients: [repository README](../README.md).

## Claude Code

```bash
export EXEMPLAR_API_KEY="eis_your_org_api_key"

/plugin marketplace add Exemplar-Dev/exemplar-memory-plugin
/plugin install exemplar-memory@exemplar-plugins
/reload-plugins
/mcp
```

## Codex

**Plugin (MCP + skill):**

```bash
export EXEMPLAR_API_KEY="eis_your_org_api_key"
codex plugin marketplace add Exemplar-Dev/exemplar-memory-plugin
# Restart Codex → /plugins → Exemplar Plugins → install exemplar-memory
```

**Direct MCP only:** see [Codex Option A](../README.md#codex) in the root README.

## Use in chat

```
Remember that I prefer metric units.
What do you know about my editor setup?
Forget my old office address.
```

## Actions

`add` · `search` · `list` · `get` · `update` · `delete` · `bulk_delete`
