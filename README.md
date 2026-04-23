# MCP Server Manager

> **Unmaintained.**

I run MCP servers across Cursor, VSCode, and Windsurf. Each stores its config in a different JSON file, in a different location, with slightly different schema. Adding the same server to all three means editing three files. I wanted one interface to CRUD them all from a launcher. So I built this Raycast extension.

Tried to ship to the Raycast Store. Their official MCP extension handles discovery, mine's a cross-editor control panel. Raycast policy is to enhance existing extensions rather than ship similar new ones. I didn't port it over.

Raycast extension for managing MCP (Model Context Protocol) servers across Cursor, VS Code, and Windsurf.

![demo](metadata/demo.gif)

## Install

```bash
git clone https://github.com/ramonclaudio/raycast-mcp-server-manager.git
cd raycast-mcp-server-manager
npm install
npm run build
npm run dev
```

Requires Raycast ≥ 1.50.0, Node.js ≥ 18.0.0, and at least one supported editor.

## Commands

Type `MCP` in Raycast:

- `List MCP Servers` — view all servers across editors
- `Add MCP Server` — create a new server config
- `Search MCP Servers` — filter across editors
- `Remove MCP Server` — delete with protection for critical servers
- `View Raw Configs` — direct file editing

## Supported editors

| Editor | Transports | Global config | Workspace config |
| --- | --- | --- | --- |
| Cursor | `stdio`, `SSE` | `~/.cursor/mcp.json` | `.cursor/mcp.json` |
| VS Code | `stdio`, `SSE`, `HTTP` | `~/Library/Application Support/Code/User/settings.json` | `.vscode/mcp.json` or `.vscode/settings.json` |
| Windsurf | `stdio`, `/sse` | `~/.codeium/windsurf/mcp_config.json` | `.windsurf/mcp.json` |

### Editor notes

**Cursor**: 40-tool cap across all MCP servers. Remote-dev/SSH may misbehave. Tool usage requires approval by default. SSE servers need full URL (`http://localhost:8000/sse`).

**VS Code**: Requires 1.99+. Supports secure input prompts for secrets via `inputs` field. Toggle tools from the chat UI. Workspace config goes in `.vscode/mcp.json` for cleanliness.

**Windsurf**: 100-tool cap. Non-standard `/sse` transport (URL must end with `/sse`). Requires `serverUrl` parameter instead of `url`.

## Transport types

```json
// stdio — local process
{
  "command": "python",
  "args": ["-m", "my_mcp_server"],
  "env": {"API_KEY": "your-key"}
}

// SSE — remote with event streaming
{
  "transport": "sse",
  "url": "https://api.example.com/mcp"
}

// Windsurf /sse — non-standard
{
  "transport": "/sse",
  "serverUrl": "https://api.example.com/sse"
}

// HTTP — standard request/response
{
  "transport": "http",
  "url": "http://localhost:8000/mcp"
}
```

The extension handles the differences automatically based on the selected editor.

## Server protection

The UI protects critical servers (like `mcp-server-time`) from accidental deletion in the List/Search/Remove commands. UI-only — raw config editing bypasses it.

## Troubleshooting

- **Extension not loading**: Raycast must be ≥ 1.50.0. Restart Raycast.
- **Servers missing**: Verify the config file exists, has valid JSON, and you have read permissions.
- **Connection failures**: Test accessibility, verify commands work locally, check environment variables.

## License

MIT
