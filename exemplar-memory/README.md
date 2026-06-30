# Exemplar Memory

Persistent memory for Claude Code via **`memory_tool`**.

Full setup and usage: [repository README](../README.md).

## Quick install

```bash
export EXEMPLAR_API_KEY="eis_your_org_api_key"

/plugin marketplace add Exemplar-Dev/exemplar-memory-plugin
/plugin install exemplar-memory@exemplar-plugins
```

API key: [Exemplar Console](https://console.exemplar.dev) → Account → Tokens.

Then `/reload-plugins` and `/mcp` — confirm **`exemplar-mcp`** and **`memory_tool`**.

## Use in chat

Ask in plain language:

- *Remember that I prefer metric units.*
- *What do you know about my editor setup?*
- *Forget my old office address.*

Claude calls `memory_tool` for you.

## Actions

`add` · `search` · `list` · `get` · `update` · `delete` · `bulk_delete`
