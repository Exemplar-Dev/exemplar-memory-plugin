# exemplar-memory-plugin

Persistent memory for AI coding agents — store and recall preferences and facts across sessions via **`memory_tool`**.

Works with **Claude Code** (plugin), **Codex** (`codex mcp add`), and **Cursor**, **Claude Desktop**, **VS Code**, **Windsurf**, **Cline** (manual MCP).

## Get started

1. Create an `eis_` API key at [Exemplar Console](https://console.exemplar.dev) → Account → Tokens
2. Choose your client below (plugin or manual MCP)
3. Ask in natural language — see [Usage examples](#usage-examples)

Production MCP endpoint: `https://production-api.exemplar.dev/mcp`

---

## Claude Code (plugin — recommended)

```bash
export EXEMPLAR_API_KEY="eis_your_org_api_key"

/plugin marketplace add Exemplar-Dev/exemplar-memory-plugin
/plugin install exemplar-memory@exemplar-plugins
/reload-plugins
/mcp
```

Add `export EXEMPLAR_API_KEY=...` to `~/.zshrc` so it persists.

**Verify:** `exemplar-mcp` connected, `memory_tool` listed. An OAuth 404 message is harmless if tools show connected.

---

## Codex

Export your key first (`export EXEMPLAR_API_KEY="eis_..."` in `~/.zshrc`).

### Option A — Direct MCP (fastest, MCP only)

Add to `~/.codex/config.toml`:

```toml
[mcp_servers.exemplar-mcp]
url = "https://production-api.exemplar.dev/mcp"
bearer_token_env_var = "EXEMPLAR_API_KEY"

[mcp_servers.exemplar-mcp.env_http_headers]
"x-api-key" = "EXEMPLAR_API_KEY"
```

Restart Codex and run **`/mcp`** — confirm `memory_tool` is listed.

Or use the CLI (then add `env_http_headers` in config.toml if needed):

```bash
codex mcp add exemplar-mcp \
  --url https://production-api.exemplar.dev/mcp \
  --bearer-token-env-var EXEMPLAR_API_KEY
```

### Option B — Plugin (recommended: MCP + skill)

Registers this repo’s Codex marketplace (`.agents/plugins/marketplace.json`), then install from the plugin browser:

```bash
export EXEMPLAR_API_KEY="eis_your_org_api_key"

codex plugin marketplace add Exemplar-Dev/exemplar-memory-plugin
```

Restart Codex, run **`/plugins`** → **Exemplar Plugins** tab → install **exemplar-memory**.

> **Do not combine A and B.** The plugin registers `exemplar-mcp` via `.codex-mcp.json` — a manual `[mcp_servers.exemplar-mcp]` block would duplicate it.

**Upgrade / remove:**

```bash
codex plugin marketplace upgrade
codex plugin marketplace remove exemplar-plugins
```

---

## Cursor (manual MCP)

Add to `~/.cursor/mcp.json` or `.cursor/mcp.json`:

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

Restart Cursor. Settings → MCP to confirm the server is connected.

---

## Claude Desktop (manual MCP)

Claude Desktop uses stdio — bridge with `mcp-remote`:

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

Config: `~/Library/Application Support/Claude/claude_desktop_config.json` (macOS). Restart Claude.

---

## VS Code Copilot (manual MCP)

`.vscode/mcp.json`:

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

See [exemplar-memory/README.md](./exemplar-memory/README.md) or the [Exemplar Console](https://console.exemplar.dev) Memory tab for config shapes (`serverUrl` on Windsurf).

---

## Usage examples

Talk in plain language — your agent calls `memory_tool` for you.

### Remember

```
Remember that I prefer bullet-point answers.
Remember for user alice that I prefer dark mode.
Note for future sessions: deploy window is Tuesday 2–4pm UTC.
```

### Recall

```
What do you know about my formatting preferences?
What did I ask you to remember about deploy windows?
Search memory for user alice: UI preferences?
```

### List / update / forget

```
List what you have stored about me.
Update my theme preference — I switched to light mode.
Forget my dietary restrictions.
```

---

## Tips

| Topic | Guidance |
|-------|----------|
| **Scopes** | Memories need at least one of `user_id`, `session_id`, `agent_id`, `app_id`. Say *"for user alice"* on shared machines. |
| **What to store** | Preferences, standing rules, explicit facts — not ephemeral chat context. |
| **Cross-session** | Install once; memories persist across sessions. |
| **Tool approval** | Approve `memory_tool` on first use if prompted. |

---

## Troubleshooting

| Problem | Fix |
|---------|-----|
| Plugin install fails | Set `EXEMPLAR_API_KEY` before install; update Claude Code |
| OAuth SDK 404 in `/mcp` | Ignore if status is **connected** and tools are listed |
| No tools | `/reload-plugins` or restart the client |
| Auth errors | `echo $EXEMPLAR_API_KEY` — recreate key if needed |

---

## What gets installed (plugin)

- **MCP** — `memory_tool` (`add`, `search`, `list`, `get`, `update`, `delete`, `bulk_delete`)
- **Skill** — when Claude/Codex should use memory automatically

No Python SDK required for plugin mode.
