# Exemplar Memory plugin

Bundled MCP + skill for Claude Code. Codex users should prefer **`codex mcp add`** — see [repository README](../README.md#codex-mcp--recommended).

## Claude Code plugin install

```bash
export EXEMPLAR_API_KEY="eis_your_org_api_key"

/plugin marketplace add Exemplar-Dev/exemplar-memory-plugin
/plugin install exemplar-memory@exemplar-plugins
/reload-plugins
/mcp
```

## Use in chat

```
Remember that I prefer metric units.
What do you know about my editor setup?
Forget my old office address.
```

## Actions

`add` · `search` · `list` · `get` · `update` · `delete` · `bulk_delete`
