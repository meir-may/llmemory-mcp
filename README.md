# LLMemory MCP Server

A remote MCP server that lets AI coding agents search and pull context from your imported AI chat history (ChatGPT, Claude, Codex, Claude Code) without copy-pasting. Reads your vault live from your own Google Drive. Zero server-side storage, fully private.

**Server URL:** `https://mcp.llmemory.xyz/mcp`
**Transport:** Streamable HTTP
**Auth:** OAuth 2.1 with Dynamic Client Registration (no API keys — browser flow on first use)

## Install in Cline

This is a **remote MCP server** — no cloning, no npm install, no local process. Cline just needs the URL.

**Option A — Cline Dashboard (recommended):**
1. Log in to the [Cline Dashboard](https://app.cline.bot) → Organization → MCP Servers
2. Add a new **Remote MCP server**
3. Name: `LLMemory` · URL: `https://mcp.llmemory.xyz/mcp`
4. Save. In VS Code/JetBrains, open the Cline panel → MCP Servers → connect `llmemory`.
5. On first tool call, a browser window opens — authorize with Google and grant Drive access.

**Option B — Cline MCP config JSON:**
Add this to your Cline MCP settings (`cline_mcp_settings.json`):

```json
{
  "mcpServers": {
    "llmemory": {
      "type": "remote",
      "url": "https://mcp.llmemory.xyz/mcp"
    }
  }
}
```

Restart the Cline extension. On first tool call, the OAuth browser flow opens automatically.

## Install in other MCP clients

Same URL works in any MCP client that supports Streamable HTTP + OAuth 2.1:

- **Cursor** — [LLMemory Cursor plugin](https://github.com/meir-may/llmemory-cursor-plugin)
- **VS Code / GitHub Copilot** — add to `.vscode/mcp.json`: `{ "servers": { "llmemory": { "type": "http", "url": "https://mcp.llmemory.xyz/mcp" } } }`
- **Claude Desktop / Claude Code** — `claude mcp add --transport http llmemory https://mcp.llmemory.xyz/mcp`
- **Open WebUI** — Admin Settings → External Tools → add Streamable HTTP MCP server with URL `https://mcp.llmemory.xyz/mcp`

## Tools

| Tool | Purpose |
|---|---|
| `list_sources` | Show which AI providers have data in your vault and counts |
| `list_recent` | Recently updated conversations (id, title, source, message count). Filter by `source` |
| `search_chats` | Full-text search across all imported conversations. Returns snippets + conversation IDs |
| `get_conversation` | Fetch one full transcript by ID (Markdown or JSON) |
| `get_context_pack` | Agent-ready briefing (goal, recent state, files, open tasks) for one conversation or a whole folder |

## What it reads

LLMemory reads the `LLMemory/` folder in your Google Drive — a `manifest.json` plus one JSON file per conversation, written by the [LLMemory browser extension](https://llmemory.xyz) or CLI. The server is **read-only** and **zero-storage**: it brokers Google OAuth, streams the result, then forgets everything. Your chats never leave your Drive on a server.

## Privacy

- **Zero server-side storage.** No chat content, no per-user database. Cloudflare Workers free tier.
- **Your Drive is the storage.** Chats live in your own Google Drive `LLMemory/` folder.
- **Read-only.** The connector never writes to your vault.
- **OAuth-scoped.** The server only ever sees your `LLMemory/` Drive folder (`drive.file` scope).

Full privacy posture: [STORE_PRIVACY.md](https://github.com/intrepid37/LLMemory/blob/main/STORE_PRIVACY.md)

## Examples

- *"What did I figure out about the OAuth worker last week?"* → searches your vault
- *"Pull my most recent Codex session"* → `list_recent` filtered to `codex`, then `get_conversation`
- *"Give me a context pack for the session where we built the Claude connector"* → `get_context_pack`
- *"Show me which chats have data imported"* → `list_sources`

## Links

- [LLMemory](https://llmemory.xyz)
- [MCP connector design doc](https://github.com/intrepid37/LLMemory/blob/main/MCP_CONNECTOR.md)
- [Main repo](https://github.com/intrepid37/LLMemory)

## License

MIT
