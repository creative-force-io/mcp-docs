# Setup guide

This guide connects an MCP-compatible client to the Creative Force MCP server and verifies the
connection. Setting up takes two parts: a **role permission** step done once by an administrator,
and a **connection** step done by each user in their chosen AI tool.

> 🧪 **Open beta.** The Creative Force MCP server is currently in open beta. Supported AI tools and
> available capabilities change regularly. To report an issue or request a capability, ask your AI
> assistant to use the **Send Feedback** tool — see [Send feedback](#send-feedback).

Supported clients: **Claude** (web, desktop, Claude Code), **ChatGPT** (Developer Mode),
**Microsoft Copilot** (via Copilot Studio), and **Notion** (Notion AI). Any other MCP-compatible
client that supports remote HTTP servers with OAuth should work using the
[OAuth settings reference](#oauth-settings-reference).

## Prerequisites

- A **Creative Force account** you can sign in to with a username and password.
- Your role has the relevant **MCP tool permissions** granted (see
  [Grant MCP tool permissions](#grant-mcp-tool-permissions)). The tools you see also depend on your
  **screen permissions** in Creative Force — you will only see tools for the areas your account can
  already access.

You do **not** need to create or paste an API key by hand — authentication happens through a
standard OAuth login with your Creative Force account. Use of the MCP server is free of charge and
no subscription add-on is required.

## Server endpoint

```
https://mcp.creativeforce.io/mcp
```

## Grant MCP tool permissions

MCP tools are off by default for every role. A Studio Admin (or any role with access to
**Settings → User Roles**) grants access once per role; customers with Admin access can do this
themselves.

1. In Creative Force, navigate to **Settings → User Roles**.
2. Select a role, then click **Edit**.
3. Go to the **MCP Server** tab.
4. For each tool, select **None** or **Access** — or set a whole group at once with the group
   dropdown (**Allow All (incl. new tools)**, **Allow All**, **Block All**, **Custom**).
5. Click **Save**.

Tools are organised into two groups on that tab:

| Group | What's in it |
|-------|--------------|
| **Read-Only** | Tools that read data without making any changes. |
| **Write/Delete** | Tools that change data — the planning-session tools, gated by the single **Write Planning** permission. |

**Query Planning and Write Planning are separate permissions.** A role with *Query Planning* but not
*Write Planning* can ask questions about sessions but cannot create, change, or delete them. Session
management changes live data and has no undo — see
[Manage Planning Sessions](https://help.creativeforce.io/en/articles/16072867-mcp-server-manage-planning-sessions).

> ⚠️ **All MCP tool permissions default to None for every standard role.** You must explicitly grant
> access. Connecting with an account that has no access permissions will fail.

Each tool you grant adds a corresponding capability in the connected AI tool. Granting more tools
surfaces more capabilities; we recommend enabling all of them for the fullest experience. Only
**Allow All (incl. new tools)** turns on tools added in future releases automatically.

A tool needs **both** its MCP Server permission *and* the Creative Force permission for the data
screen it reads — see **[permissions.md](permissions.md)** for the full tool-by-tool map.

Permission changes can take up to ten minutes to take effect, or re-authenticate to apply them
immediately.

## Connect Claude

How you connect depends on which Claude product you use.

### Claude.ai / Claude Desktop

#### Admin steps

The initial connection is configured once by an administrator of the Claude workspace, as a custom
connector.

1. In Claude, navigate to **Customize → Connectors** (in some versions, **Settings → Connectors**).
2. Click the **+** icon and choose **Add custom connector**.
3. Enter the following values exactly as shown:

   | Field | Value |
   |-------|-------|
   | Name | `Creative Force` |
   | URL | `https://mcp.creativeforce.io/mcp` |
   | OAuth Client ID | `mcp` |

4. Click **Add** to save the connector.

The **Creative Force** connector now appears in the connectors list for everyone in your
organisation.

> Custom connectors must be enabled for your account. On corporate Claude accounts this may be
> controlled by your Claude administrator; personal Claude accounts have them on by default.
> A native, certified **Creative Force** listing in Claude's connector directory is coming — until
> then, use the custom-connector flow above.

#### User steps

1. From [claude.ai](https://claude.ai) or the
   [Claude desktop app](https://support.claude.com/en/articles/10065433-install-claude-desktop), go
   to **Settings → Connectors**.
2. Click **+** to add a new connector, search for **Creative Force**, and click **Connect**.
3. A browser window opens — enter your Creative Force credentials and click **Log In**.
4. Click **Allow** to authorise the MCP server to access your Creative Force account.

> Your login token is cached locally. You won't need to log in again until it expires.

#### Enable tool permissions

Back in Claude's settings, review the available tool permissions for the Creative Force connector.
We recommend enabling all of them.

### Claude Code (CLI)

**Option A — CLI command:**

```
claude mcp add --transport http "CreativeForce" https://mcp.creativeforce.io/mcp --client-id mcp --callback-port 9008
```

**Option B — manual configuration.** Add the following to `.claude.json` in your project root, or
`~/.claude.json` for a global configuration:

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

On first use, Claude Code opens the Creative Force OAuth login in your browser.

## Connect ChatGPT

ChatGPT connects via a custom connector in **Developer Mode**, which is available on **Plus, Pro,
Team, Enterprise, and Edu** plans. The free plan does not support custom connectors and cannot
connect to the Creative Force MCP server.

1. In ChatGPT, open your account menu and go to **Settings → Apps → Advanced Settings**, then
   toggle on **Developer Mode**.
2. Click **Create App** and fill in:

   | Field | Value |
   |-------|-------|
   | Name | `Creative Force` |
   | MCP Server URL | `https://mcp.creativeforce.io/mcp` |
   | Authentication | OAuth |

3. Expand **Advanced OAuth Settings** and **uncheck OIDC Enabled**.
4. Set the **OAuth Client ID** to `mcp`.
5. Tick **I understand and want to continue**, then click **Create**.
6. In the pop-up, click **Sign in with Creative Force**, enter your credentials, and click
   **Allow** to authorise the connection.

To use Creative Force in a chat, click the **+** icon in the message input, hover over **More**, and
select **Creative Force**.

> 📝 Developer Mode carries an elevated risk warning from OpenAI. This is expected for custom MCP
> connectors that are not yet marketplace-certified; it does not indicate a problem with the
> Creative Force connector.

## Connect Microsoft Copilot

An administrator creates a custom agent in **Microsoft Copilot Studio**, connects it to the Creative
Force MCP server, and publishes it inside your Microsoft tenant. Once live, everyone in your
organisation with a Creative Force account can use it in Microsoft 365 and Teams — no installs and
no extra setup on their side.

> ⚙️ This setup must be performed by a **Microsoft account admin** and requires a
> [publishing licence for Copilot agents](https://learn.microsoft.com/en-us/microsoft-copilot-studio/requirements-licensing?tabs=web).

### Step 1 — Create a new agent

1. Open Copilot Studio.
2. Go to **Agents** and click **New Agent**.
3. Add the Creative Force logo, and set the model — **Claude Opus** is recommended.
4. Paste the recommended agent instructions below.

<details>
<summary><b>Recommended Copilot agent instructions</b> (click to expand)</summary>

```text
You are the Creative Force agent. You answer questions about the user's live Creative Force studio data using the Creative Force MCP tools, and you explain how Creative Force works.

SCOPE AND SOURCING
- For any question about jobs, productions, products, samples, tasks, planning sessions, talent and crew, assets, workflows, style guides, presets, vendors, locations, workspaces or studio settings, always call the Creative Force tools. Never answer from memory or from data earlier in the conversation — production data changes constantly.
- If a tool returns nothing, say so plainly. Never fill a gap with plausible-sounding records, codes, names or numbers.
- Quote the real identifiers (job code, product code, sample code, deliverable name) so the user can verify the answer in Creative Force.
- Do not expose tool names, GUIDs, internal field names or raw JSON. Use the Creative Force terms the user sees on screen.

CREATIVE FORCE VOCABULARY (these words overlap — get this right)
- When a user says "production" loosely they usually mean a job: the container that groups products for production. Use job queries for "which productions are in progress", "this production's deadline".
- A production work unit is one product moving through the workflow. Use it for step, status, assignee and vendor questions about individual products.
- A product request is the catalogue entry — code, name, brand, category, colour, style code.
- A task is a single workflow step: photography, retouch, photo review, post review, QC, final selection, copywriting.
- A sample is the physical item. "Has it arrived / where is it / has it been returned" are sample questions; "has it been shot / what step is it on" are production questions.
- E-commerce jobs contain products. Editorial projects contain deliverables. Pick the right family before querying.

BEFORE YOU QUERY
- Resolve the client/workspace and the date range first. If the studio has several workspaces and the question names none, ask rather than silently querying all of them or picking one.
- Convert relative dates ("this week", "tomorrow") to explicit dates and state the range you used.
- For "how many" or "which has the most", aggregate in the query instead of listing everything and counting by hand.
- For image requests, fetch previews rather than describing filenames.
- For "who changed this and when", use the event log for that one item.

ANSWERING
- Lead with the answer, then the detail. Use a short table for lists of more than three records.
- Call out risk the data reveals: samples not checked in for an imminent shoot, someone double-booked, items past due, deliverables due soon but still early in the workflow.
- If a result looks surprising, state what the query covered, so the user can tell "nothing there" apart from "wrong filter".

PERMISSIONS
- Access follows the user's own Creative Force permissions. On an authorisation or permission error, explain that their Creative Force role needs the relevant data-screen and MCP Server permissions enabled, and that the MCP Server feature must be enabled on the studio's subscription — their Creative Force administrator or Creative Force support can arrange both. Do not retry the same call repeatedly.

WHEN YOU HIT A LIMIT — DO THIS EVERY TIME
- Creative Force MCP is in open beta and capabilities grow with every release. When something is not available, do not simply decline: offer to send it to Creative Force with the send feedback tool.
- Compose the feedback yourself from the conversation — the use case the user was trying to complete, the limitation that stopped them, and their original wording. The user should not have to write it.
- Use the same tool when the user says an answer was wrong or unhelpful.
```

</details>

> The instructions above are reproduced from the Help Centre article. One line in them — that the
> MCP Server feature must be enabled on the studio's subscription — is no longer accurate; access is
> decided solely by the MCP tool permissions granted on a role. Trim that clause when you paste, or
> leave it: it only affects how the agent words a permission error.

### Step 2 — Add the MCP server

1. Click **Tools** on the right panel.
2. Click **Add** and select **Model Context Protocol (MCP)**.
3. Fill in the server details:

   | Property | Value |
   |----------|-------|
   | Server Name | `Creative Force MCP` |
   | Server Description | `Bring the full depth of Creative Force into any AI-powered workflow. With a real-time understanding of jobs, tasks, assets, planning sessions, and more, your AI can reason across your production data to surface bottlenecks, answer questions, and run multi-step workflows. Your creative production operating system, now wired directly into your AI.` |
   | URL | `https://mcp.creativeforce.io/mcp` |

### Step 3 — Authentication

Fill in the authentication details:

| Property | Value |
|----------|-------|
| Type | `Manual` |
| Authentication | `OAuth 2.0` |
| Client ID | `mcp` |
| Client Secret | *(not used)* |
| Authorization URL | `https://accounts.creativeforce.io/connect/authorize` |
| Token URL Template | `https://accounts.creativeforce.io/connect/token` |
| Refresh URL | `https://accounts.creativeforce.io/connect/token` |
| Scopes | `webapp_scope offline_access` |

Click **Add**.

### Step 4 — Connect Creative Force

1. Click **Not Connected** and select **Create new connection**. A Creative Force pop-up opens.
2. Log in to your Creative Force account.
3. Click **Submit**, wait for the connection to complete, then click **Add**.

### Step 5 — Request a publish licence

To use your agent in Copilot chats you need permission to publish. Request a publishing licence from
your Microsoft tenant administrator before continuing.

### Step 6 — Enable for Copilot and Teams

Click the **Publish** dropdown in the top-right corner, select **Teams + Microsoft 365**, and click
**Edit Details**.

### Step 7 — Edit agent details

Choose your **Team Settings**, then set:

| Field | Value |
|-------|-------|
| Short Description | `Live answers from your Creative Force studio production data` |
| Long Description | See below |

<details>
<summary><b>Recommended long description</b> (click to expand)</summary>

```text
Creative Force brings your studio's live production data into conversation, so you can ask for what you need instead of opening the app and building filters. Ask in plain language and get an answer grounded in your own workspaces.

WHAT IT'S FOR
Anything you would normally open Creative Force to find out or follow up on — how a shoot is planned and staffed, where physical samples are, how products and deliverables are moving through the workflow, what is assigned to whom, which assets are ready, and how your studio is configured. Rather than navigating screens and filters, you describe the question and the agent goes and gets the answer.

EXAMPLES
- "What's booked in the studio tomorrow, and is anyone double-booked?"
- "Which samples for Friday's shoot still haven't arrived?"
- "What's holding up job SS26-Denim?"
- "Which deliverables are due this week but are still in photography?"
- "What's assigned to me today?"
- "Show me the approved images for this style code."
- "What steps are in our on-model workflow, and what does the style guide require?"
- "Who changed this, and when?"

These are illustrations, not a menu. Ask the way you would ask a colleague — and if something isn't supported, the agent will tell you instead of guessing.

A GROWING SET OF CAPABILITIES
Creative Force adds capabilities continuously, so what the agent can reach expands over time without anything for you to install or update: more areas of the platform, more detail, and a growing set of actions it can take on your behalf rather than only reporting back. If something you need isn't there yet, say so in the conversation — you can rate an answer, flag a question it couldn't solve, or request a new capability, and it reaches the Creative Force team directly.

GOOD TO KNOW
- You need a Creative Force account. Access follows the permissions you already have, so you only see the workspaces and data you are entitled to.
- Answers reflect live data at the moment you ask.
- Naming the workspace, job or date range keeps broad questions fast and precise.
- Ask which records an answer is based on if you want to check it against the app.

Best used for the questions that would otherwise mean opening several screens: status checks, pre-shoot readiness, chasing blockers, and understanding how your studio is set up.
```

</details>

Scroll to the bottom, click **Save**, then click **Publish**.

### Use the agent

- **Microsoft 365** — click **See agent in Microsoft 365**. This opens the agent on **m365.cloud**,
  where you can add it and make it available in your chats. Share that link with colleagues so they
  can add it too.
- **Teams** — click **See agent in Teams**, then **Open**. The agent is then available in your chats.

## Connect Notion

Notion connects as a custom MCP server inside **Notion AI**. A workspace admin enables custom MCP
servers once, then each user adds the Creative Force connection.

> Requires a Notion account with **Notion AI** enabled.

### Admin step — enable custom MCP servers

1. Go to **Account → Settings → Connections**, then open the **Manage** tab.
2. Turn on **Enable custom MCP servers**.

### User steps — add Creative Force

1. In **Connections**, open the **Discover** tab and select the **MCP** sub-tab.
2. Click **Custom MCP**.
3. Fill in the connection form:

   | Field | Value |
   |-------|-------|
   | URL | `https://mcp.creativeforce.io/mcp` |
   | Name | `Creative Force` |
   | OAuth Client ID | `mcp` |

4. Click **Connect**. A browser window opens — sign in with your Creative Force account and click
   **Allow**.

Notion handles the rest. Creative Force is then available to your **Notion AI** agents.

## OAuth settings reference

For any client that asks for OAuth details manually:

| Setting | Value |
|---------|-------|
| MCP Server URL | `https://mcp.creativeforce.io/mcp` |
| Auth Server | `https://accounts.creativeforce.io` |
| Client ID | `mcp` |
| Client Secret | *not used — `mcp` is a public client* |
| Grant Type | Authorization Code (with PKCE) |
| Authorization Endpoint | `https://accounts.creativeforce.io/connect/authorize` |
| Token Endpoint | `https://accounts.creativeforce.io/connect/token` |
| Refresh Endpoint | `https://accounts.creativeforce.io/connect/token` |
| Scopes | `webapp_scope offline_access` |

These values are what `https://mcp.creativeforce.io/.well-known/oauth-protected-resource` and
`https://accounts.creativeforce.io/.well-known/openid-configuration` advertise, so most clients
discover them automatically and only need the server URL and client ID.

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
| No tools visible after connecting | Your role lacks MCP tool permissions | Ask your Studio Admin to grant access under **Settings → User Roles → MCP Server**. |
| A tool you expect is missing | Your account lacks the screen permission for that area | Request the relevant Creative Force permission; tools are filtered per user. |
| Tools visible but returning "Access denied" | Permission cache is stale (up to 10 min) | Wait and retry, or re-authenticate. |
| `401 Unauthorized` on every request | Token missing or expired | Re-run the OAuth login; restart the MCP client to re-authenticate, or clear the client's cached auth (e.g. delete `~/.mcp-auth`). |
| The browser never opens for login | The client did not discover the OAuth metadata, or the auth server is unreachable | Confirm the URL is exactly `https://mcp.creativeforce.io/mcp`, and that your network can reach `https://accounts.creativeforce.io`. |
| No results returned | Your account has no access to a studio with production data | Verify your Creative Force account is attached to a studio that holds data for the area you asked about. |
| Connection timeout | Server URL unreachable from your network | Confirm `https://mcp.creativeforce.io/mcp` is reachable from your network. |

Still stuck? See **[support.md](support.md)**.

## See also

- **[tool-reference.md](tool-reference.md)** — every tool and parameter.
- **[permissions.md](permissions.md)** — how tool access is granted per user role.
- **[use-cases.md](use-cases.md)** — realistic example prompts.
- Creative Force Help Centre:
  [MCP Server: Overview](https://help.creativeforce.io/en/articles/14743712-mcp-server-overview) ·
  [Setup & Configuration](https://help.creativeforce.io/en/articles/15570880-mcp-server-setup-configuration) ·
  [Copilot Studio](https://help.creativeforce.io/en/articles/16237883-mcp-server-copilot-studio) ·
  [Data Queries](https://help.creativeforce.io/en/articles/15566092-mcp-server-data-queries) ·
  [Manage Planning Sessions](https://help.creativeforce.io/en/articles/16072867-mcp-server-manage-planning-sessions)
