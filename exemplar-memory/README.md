# Exemplar Memory plugin

Bundled MCP + skill for Claude Code and Codex.

Full setup for all clients: [repository README](../README.md).

## Plugin install

```bash
export EXEMPLAR_API_KEY="eis_your_org_api_key"
```

**Claude Code**

```bash
/plugin marketplace add Exemplar-Dev/exemplar-memory-plugin
/plugin install exemplar-memory@exemplar-plugins
/reload-plugins
/mcp
```

**Codex**

```bash
codex plugin marketplace add Exemplar-Dev/exemplar-memory-plugin
codex plugin add exemplar-memory@exemplar-plugins
```

## Use in chat

```
Remember that I prefer metric units.
What do you know about my editor setup?
Forget my old office address.
```

## Actions

`add` · `search` · `list` · `get` · `update` · `delete` · `bulk_delete`
