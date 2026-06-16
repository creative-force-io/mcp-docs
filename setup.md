# Setup guide

This guide connects an MCP-compatible client (Claude or Gamma) to the Creative Force MCP server
and verifies the connection. It takes about five minutes.

## Prerequisites

- A **Creative Force account** you can sign in to with a username and password.
- Your studio's Creative Force subscription includes the **MCP Server** feature. If it does not,
  the connection is refused with a *"connection denied"* message — contact your Creative Force
  account manager to enable it.
- The tools you see depend on your **screen permissions** in Creative Force. You will only see
  tools for the areas your account can already access (read permission or higher).

You do **not** need to create or paste an API key by hand — authentication happens through a
standard OAuth login with your Creative Force account.

## Server endpoint

```
https://mcp.creativeforce.io/mcp
```

## Connect Claude

### Claude.ai / Claude Desktop (custom connector)

1. Open **Settings → Connectors**.
2. Choose **Add custom connector**.
3. Enter the server URL: `https://mcp.creativeforce.io/mcp`
4. Claude discovers the authentication settings automatically and opens a Creative Force login
   window. Sign in with your Creative Force account and approve access.
5. The connector appears as **Creative Force** with its available tools.

> Menu labels vary slightly by Claude version. The flow is always the same: add a custom
> connector by URL → sign in with Creative Force → approve.

### Claude Code / config-file clients

The hosted server only needs a URL. Add it to your MCP client configuration:

```json
{
  "mcpServers": {
    "creative-force": {
      "url": "https://mcp.creativeforce.io/mcp"
    }
  }
}
```

- **Claude Code:** `claude mcp add --transport http creative-force https://mcp.creativeforce.io/mcp`
- **Cursor / VS Code (MCP):** add the block above to the client's `mcp.json` / MCP settings.

On first use, the client opens the Creative Force OAuth login in your browser.

## Connect Gamma

Gamma connects to the same endpoint. In Gamma's connector settings, add a custom MCP server with
the URL `https://mcp.creativeforce.io/mcp` and complete the Creative Force sign-in when prompted.

## Test the connection

**Option A — a prompt (any client).** Ask:

> "List my Creative Force workspaces."

This calls `query_workspace`, the lightest tool, and confirms both the connection and your
permissions. You should get back your workspace names.

**Option B — MCP Inspector (developers).** Run the official inspector and point it at the
endpoint to list and call tools directly:

```
npx @modelcontextprotocol/inspector
```

Enter `https://mcp.creativeforce.io/mcp`, complete the OAuth login, and confirm the tool list
loads.

## Troubleshooting

| Symptom | Likely cause | Fix |
|---------|--------------|-----|
| `401 Unauthorized` | Token missing or expired | Re-run the OAuth login; clear the client's cached auth (e.g. delete `~/.mcp-auth`) and reconnect. |
| "Connection denied" on connect | Studio lacks the **MCP Server** subscription feature | Ask your Creative Force account manager to enable it. |
| A tool you expect is missing | Your account lacks the screen permission for that area | Request the relevant Creative Force permission; tools are filtered per user. |
| Login window never appears | The client did not discover the OAuth metadata | Confirm the URL is exactly `https://mcp.creativeforce.io/mcp` and your network allows it. |

Still stuck? See **[support.md](support.md)**.
