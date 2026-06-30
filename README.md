# Exemplar Memory Plugin

**Long-term memory for AI coding agents** — a [Claude Code](https://code.claude.com/) plugin and [Model Context Protocol (MCP)](https://modelcontextprotocol.io/) server that lets **Claude Code**, **OpenAI Codex**, **Cursor**, **Claude Desktop**, **VS Code Copilot**, **Windsurf**, and **Cline** remember user preferences and facts across sessions.

Store once, recall with semantic search. No local vector DB. No Python SDK required for plugin install.

[![Exemplar Console](https://img.shields.io/badge/Console-exemplar.dev-orange)](https://console.exemplar.dev)
[![MCP](https://img.shields.io/badge/MCP-memory__tool-blue)](https://github.com/Exemplar-Dev/exemplar-memory-plugin)
[![Claude Code Plugin](https://img.shields.io/badge/Claude_Code-plugin-7c3aed)](https://code.claude.com/docs/en/plugins)
[![Codex Plugin](https://img.shields.io/badge/Codex-plugin-10a37f)](https://developers.openai.com/codex/plugins)

---

## What is this?

**exemplar-memory-plugin** is the official Exemplar plugin for persistent agent memory. It connects your coding agent to Exemplar’s hosted **`memory_tool`** MCP server so the agent can:

- **Remember** preferences, conventions, and standing rules (`add`)
- **Recall** relevant context with semantic search (`search`, `list`, `get`)
- **Update** or **delete** stored facts when things change

Memories are **org-isolated**, **scope-aware** (`user_id`, `session_id`, `agent_id`, `app_id`), and survive across chat sessions.

**Production MCP endpoint:** `https://production-api.exemplar.dev/mcp`

---

## Quick start

1. **Get an API key** — [Exemplar Console](https://console.exemplar.dev) → Account → Tokens → create an `eis_` org key
2. **Pick your client** — plugin (Claude Code / Codex) or manual MCP (Cursor, etc.)
3. **Talk naturally** — e.g. *“Remember that I prefer TypeScript strict mode”*

```bash
export EXEMPLAR_API_KEY="eis_your_org_api_key"
# Add to ~/.zshrc for persistence
```

---

## Supported clients

| Client | Install method | MCP + skill |
|--------|----------------|-------------|
| [Claude Code](#claude-code-plugin-recommended) | Plugin marketplace | Yes |
| [Codex](#codex) | Plugin (`/plugins`) or direct `config.toml` | Plugin only |
| [Cursor](#cursor-manual-mcp) | Manual `mcp.json` | MCP only |
| [Claude Desktop](#claude-desktop-manual-mcp) | Manual + `mcp-remote` | MCP only |
| [VS Code Copilot](#vs-code-copilot-manual-mcp) | Manual `.vscode/mcp.json` | MCP only |
| [Windsurf / Cline](#windsurf--cline) | Manual MCP config | MCP only |

---

## Table of contents

- [Claude Code (plugin)](#claude-code-plugin-recommended)
- [Codex](#codex)
- [Cursor (manual MCP)](#cursor-manual-mcp)
- [Claude Desktop (manual MCP)](#claude-desktop-manual-mcp)
- [VS Code Copilot (manual MCP)](#vs-code-copilot-manual-mcp)
- [Windsurf / Cline](#windsurf--cline)
- [Usage examples](#usage-examples)
- [memory_tool actions](#memory_tool-actions)
- [FAQ](#faq)
- [Troubleshooting](#troubleshooting)

---

## Claude Code (plugin — recommended)

Install from the Claude Code plugin marketplace in one flow (MCP server + agent skill):

```bash
export EXEMPLAR_API_KEY="eis_your_org_api_key"

/plugin marketplace add Exemplar-Dev/exemplar-memory-plugin
/plugin install exemplar-memory@exemplar-plugins
/reload-plugins
/mcp
```

**Verify:** `exemplar-mcp` shows connected and `memory_tool` is listed. An OAuth 404 in `/mcp` is harmless if tools appear connected.

---

## Codex

Export `EXEMPLAR_API_KEY` in your shell (`~/.zshrc`) before either option below.

### Option B — Plugin (recommended: MCP + skill)

Uses this repo’s [Codex marketplace](.agents/plugins/marketplace.json) (`.agents/plugins/marketplace.json`):

```bash
export EXEMPLAR_API_KEY="eis_your_org_api_key"

codex plugin marketplace add Exemplar-Dev/exemplar-memory-plugin
```

Restart Codex → run **`/plugins`** → **Exemplar Plugins** → install **exemplar-memory**.

```bash
codex plugin marketplace upgrade          # pull latest
codex plugin marketplace remove exemplar-plugins
```

> **Do not combine with Option A.** The plugin registers `exemplar-mcp` via `.codex-mcp.json` — manual `config.toml` would duplicate it.

### Option A — Direct MCP (fastest, MCP only)

Add to `~/.codex/config.toml`:

```toml
[mcp_servers.exemplar-mcp]
url = "https://production-api.exemplar.dev/mcp"
bearer_token_env_var = "EXEMPLAR_API_KEY"

[mcp_servers.exemplar-mcp.env_http_headers]
"x-api-key" = "EXEMPLAR_API_KEY"
```

Restart Codex → **`/mcp`** → confirm `memory_tool`.

Optional CLI (add `env_http_headers` in config.toml if needed):

```bash
codex mcp add exemplar-mcp \
  --url https://production-api.exemplar.dev/mcp \
  --bearer-token-env-var EXEMPLAR_API_KEY
```

---

## Cursor (manual MCP)

Add to `~/.cursor/mcp.json` or project `.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "exemplar-mcp": {
      "transport": "http",
      "url": "https://production-api.exemplar.dev/mcp",
      "headers": {
        "x-api-key": "eis_your_org_api_key"
      }
    }
  }
}
```

Restart Cursor → **Settings → MCP** → confirm connected.

---

## Claude Desktop (manual MCP)

Claude Desktop uses stdio MCP — bridge HTTP with [`mcp-remote`](https://github.com/geelen/mcp-remote):

```json
{
  "mcpServers": {
    "exemplar-mcp": {
      "command": "npx",
      "args": [
        "-y",
        "mcp-remote",
        "https://production-api.exemplar.dev/mcp",
        "--transport",
        "http-only",
        "--header",
        "x-api-key:eis_your_org_api_key"
      ]
    }
  }
}
```

**macOS config path:** `~/Library/Application Support/Claude/claude_desktop_config.json`

---

## VS Code Copilot (manual MCP)

Project or user `.vscode/mcp.json`:

```json
{
  "servers": {
    "exemplar-mcp": {
      "type": "http",
      "url": "https://production-api.exemplar.dev/mcp",
      "headers": {
        "x-api-key": "eis_your_org_api_key"
      }
    }
  }
}
```

---

## Windsurf / Cline

- **Windsurf:** `~/.codeium/windsurf/mcp_config.json` — use `serverUrl` (not `url`)
- **Cline:** MCP Servers settings or `cline_mcp_settings.json`

See [exemplar-memory/README.md](./exemplar-memory/README.md) or the [Exemplar Console Memory tab](https://console.exemplar.dev) for copy-paste configs.

---

## Usage examples

Ask in plain language — your agent invokes `memory_tool` automatically.

**Remember**

```
Remember that I prefer bullet-point answers.
Remember for user alice that I prefer dark mode.
Note for future sessions: deploy window is Tuesday 2–4pm UTC.
```

**Recall**

```
What do you know about my formatting preferences?
Search memory for user alice: UI preferences?
What did I ask you to remember about deploy windows?
```

**List / update / forget**

```
List what you have stored about me.
Update my theme preference — I switched to light mode.
Forget my dietary restrictions.
```

---

## memory_tool actions

| Action | Description |
|--------|-------------|
| `add` | Store a preference, fact, or standing instruction |
| `search` | Semantic search over memories |
| `list` | List memories for a scope |
| `get` | Fetch one memory by ID |
| `update` | Change stored content |
| `delete` | Remove one memory |
| `bulk_delete` | Remove many memories in scope |

**Scopes:** at least one of `user_id`, `session_id`, `agent_id`, or `app_id` is required on `add`. Say *“for user alice”* on shared machines.

---

## FAQ

### What is Exemplar Memory?

Exemplar Memory is a hosted long-term memory layer for AI agents. It stores scoped facts and preferences in Exemplar’s cloud backend and retrieves them via semantic search when the agent needs context from past sessions.

### How is this different from mem0?

Both provide persistent memory for coding agents via MCP. Exemplar Memory is built into the [Exemplar](https://exemplar.dev) platform (incidents, monitors, agent harness) and uses org-scoped `eis_` API keys with `memory_tool` on Exemplar’s MCP server.

### Does this work with Claude Code?

Yes. Install via `/plugin marketplace add Exemplar-Dev/exemplar-memory-plugin` then `/plugin install exemplar-memory@exemplar-plugins`. Includes MCP + skill.

### Does this work with OpenAI Codex?

Yes. **Plugin:** `codex plugin marketplace add Exemplar-Dev/exemplar-memory-plugin` then `/plugins`. **Direct MCP:** add `[mcp_servers.exemplar-mcp]` to `~/.codex/config.toml`.

### Does this work with Cursor?

Yes. Add the HTTP MCP server to `.cursor/mcp.json` with your `eis_` API key in the `x-api-key` header.

### Do I need the Python SDK?

No for plugin or MCP install. The [exemplar-harness-sdk](https://pypi.org/project/exemplar-harness-sdk/) is optional for custom Python agent apps.

### Is memory shared across my team?

Memories are isolated per **organization** (from your API key). Use `user_id` scopes to separate users within an org.

### What should I store?

Preferences, coding conventions, deploy windows, standing rules — explicit facts worth recalling later. Not ephemeral chat or full file contents.

---

## Troubleshooting

| Problem | Fix |
|---------|-----|
| Plugin install fails | Set `EXEMPLAR_API_KEY` before install; update Claude Code / Codex |
| OAuth / SDK 404 in `/mcp` | Harmless if status is **connected** and tools are listed |
| No `memory_tool` | `/reload-plugins`, restart client, or re-run `/mcp` |
| Auth errors | `echo $EXEMPLAR_API_KEY` — key must start with `eis_` |
| Duplicate MCP on Codex | Use plugin **or** direct `config.toml`, not both |

---

## Repository layout

```
exemplar-memory-plugin/
├── .agents/plugins/marketplace.json   # Codex plugin catalog
├── .claude-plugin/marketplace.json    # Claude Code plugin catalog
└── exemplar-memory/                   # Plugin package
    ├── .codex-plugin/plugin.json
    ├── .codex-mcp.json                # Codex MCP auth (env vars)
    ├── .claude-plugin/plugin.json
    ├── .mcp.json                      # Claude MCP auth
    └── skills/exemplar-memory/SKILL.md
```

---

## Links

- [Exemplar Console](https://console.exemplar.dev) — API keys & Memory integration guide
- [GitHub: Exemplar-Dev/exemplar-memory-plugin](https://github.com/Exemplar-Dev/exemplar-memory-plugin)
- [Model Context Protocol](https://modelcontextprotocol.io/)

---

## License

See repository license file. Plugin installs connect to Exemplar’s hosted API under your org’s terms of use.
