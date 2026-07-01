# Setup guide

This guide connects an MCP-compatible client (Claude and others) to the Creative
Force MCP server and verifies the connection. It takes about five minutes.

## Prerequisites

- A **Creative Force account** you can sign in to with a username and password.
- Your studio's Creative Force subscription includes the **MCP Server** feature. If it does not,
  the connection is refused with a *"connection denied"* message — contact your Creative Force
  account manager to enable it. Use of the MCP server is free of charge.
- Your role has the relevant **MCP tool permissions** granted (see
  [Grant MCP tool permissions](#grant-mcp-tool-permissions)). The tools you see depend on your
  **screen permissions** in Creative Force — you will only see tools for the areas your account
  can already access (read permission or higher).

You do **not** need to create or paste an API key by hand — authentication happens through a
standard OAuth login with your Creative Force account.

## Server endpoint

```
https://mcp.creativeforce.io/mcp
```

## Grant MCP tool permissions

MCP tools are off by default for every role. A Studio Admin (or any role with access to
**Settings → Roles**) grants access once per role; customers with Admin access can do this
themselves.

1. Navigate to **Settings → Roles** in the Creative Force web app.
2. Select a role (or create a new one).
3. Click **Edit Permissions**.
4. Go to the **MCP Server** tab.
5. For each tool, select **None** or **Access**.
6. Click **Save**.

Each tool you grant adds a corresponding capability in the connected AI tool. Granting more tools
surfaces more capabilities; we recommend enabling all of them for the fullest experience.

> If you don't see the **MCP Server** tab in role settings, the MCP Server feature is not enabled
> on your subscription. Contact your Creative Force account manager.

Permission changes can take up to ten minutes to take effect, or re-authenticate to apply them
immediately.

## Connect Claude

### Claude.ai / Claude Desktop (custom connector)

1. Open **Settings → Connectors** (in some versions, **Customize → Connectors**).
2. Choose **Add custom connector** (the **+** icon).
3. Enter the server URL: `https://mcp.creativeforce.io/mcp`
4. Optionally set the **Name** to `Creative Force` and **OAuth Client ID** to `mcp`.
5. Claude discovers the authentication settings automatically and opens a Creative Force login
   window. Sign in with your Creative Force account and click **Allow** to approve access.
6. The connector appears as **Creative Force** with its available tools. Enable the tools you want
   from Claude's connector settings.

> Menu labels vary slightly by Claude version. The flow is always the same: add a custom
> connector by URL → sign in with Creative Force → approve.
>
> Custom connectors must be enabled for your account. On corporate Claude accounts this may be
> controlled by your Claude administrator; personal Claude accounts have them on by default.
> A native, certified **Creative Force** listing in Claude's connector directory is coming — until
> then, use the custom-connector flow above.

### Claude Code (CLI)

**Option A — CLI command:**

```
claude mcp add --transport http "CreativeForce" https://mcp.creativeforce.io/mcp --client-id mcp --callback-port 9008
```

**Option B — manual configuration.** Edit `.claude.json` (project root) or `~/.claude.json`
(global):

```json
{
  "mcpServers": {
    "CreativeForce": {
      "type": "http",
      "url": "https://mcp.creativeforce.io/mcp",
      "oauth": {
        "clientId": "mcp",
        "callbackPort": 9008
      }
    }
  }
}
```

On first use, each client opens the Creative Force OAuth login in your browser.

## OAuth settings reference

For any client that asks for OAuth details manually:

| Setting | Value |
|---------|-------|
| MCP Server URL | `https://mcp.creativeforce.io/mcp` |
| Auth Server | `https://accounts.creativeforce.io` |
| Client ID | `mcp` |
| Grant Type | Authorization Code |
| Authorization Endpoint | `https://accounts.creativeforce.io/connect/authorize` |
| Token Endpoint | `https://accounts.creativeforce.io/connect/token` |
| Scopes | `webapp_scope offline_access` |

## Test the connection

Ask:

> "List my Creative Force workspaces."

This calls `query_workspace`, the lightest tool, and confirms both the connection and your
permissions. You should get back your workspace names. Other quick checks: *"List all jobs in
progress"* or *"Which samples haven't been returned yet?"*

## Send feedback

If you hit a limitation, ask your AI tool to send feedback — the MCP server forwards it to the
Creative Force team via the `send_feedback` tool. A useful report names the use case you wanted to
perform and the limitation you hit (e.g. "I wanted to do A but B isn't accessible / C can't be
changed yet"). You can also just say *"send this to Creative Force as feedback, include my prompt
and use case"* and the AI tool composes it for you.

## Troubleshooting

| Symptom | Likely cause | Fix |
|---------|--------------|-----|
| `401 Unauthorized` | Token missing or expired | Re-run the OAuth login; clear the client's cached auth (e.g. delete `~/.mcp-auth`) and reconnect. |
| "Connection denied" on connect | Studio lacks the **MCP Server** subscription feature | Ask your Creative Force account manager to enable it. |
| No tools visible after connecting | Your role lacks MCP tool permissions | Ask your admin to grant access under **Settings → Roles → MCP Server**. |
| A tool you expect is missing | Your account lacks the screen permission for that area | Request the relevant Creative Force permission; tools are filtered per user. |
| Tools visible but returning "Access denied" | Permission cache is stale (up to 10 min) | Wait and retry, or re-authenticate. |
| Login window never appears | The client did not discover the OAuth metadata | Confirm the URL is exactly `https://mcp.creativeforce.io/mcp` and your network allows it. |
| Connection timeout | Server URL unreachable from your network | Confirm `https://mcp.creativeforce.io` is reachable from your network. |

Still stuck? See **[support.md](support.md)**.
