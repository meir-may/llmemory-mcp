# LLMemory MCP Server — Agent Install Guide

This file guides Cline (and other MCP-capable agents) through installing the LLMemory MCP server.

## Server type: REMOTE (Streamable HTTP + OAuth 2.1 DCR)

**This is NOT a local stdio server.** Do not clone, npm install, or run anything locally. The server is already deployed at `https://mcp.llmemory.xyz/mcp`.

## What to do

Add this entry to the user's MCP servers config:

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

For Cline specifically, this goes in `cline_mcp_settings.json` under the `mcpServers` key. Merge it with any existing entries — do not overwrite.

## After adding the config

1. The user must restart the Cline extension so it picks up the new server.
2. On the first tool call to `llmemory`, Cline will open a browser window for OAuth.
3. The user signs in with Google and grants Drive access (the `drive.file` scope, limited to the `LLMemory/` folder).
4. After authorization, all five tools are available: `list_sources`, `list_recent`, `search_chats`, `get_conversation`, `get_context_pack`.

## No API key needed

Auth is OAuth 2.1 with Dynamic Client Registration. The MCP client and server negotiate credentials automatically. The user never pastes or manages an API key.

## Troubleshooting

- **401 / auth error on first call:** normal — the OAuth flow hasn't completed yet. Prompt the user to complete the browser authorization.
- **401 after working previously:** the user's token was revoked or expired. Have them re-authorize via the MCP panel.
- **404 on the URL:** the server is down. Check `https://llmemory.xyz` for status.
- **"No conversations imported":** the user's Google Drive `LLMemory/` folder is empty. They need to install the LLMemory browser extension or CLI and sync their chats first. See `list_sources`.

## Do NOT

- Do not clone this repository.
- Do not run `npm install` or `npm start`.
- Do not ask the user for an API key.
- Do not add a `command` or `args` field — this is a remote server, not a stdio process.
