# Creative Force MCP Server

[![Tools](https://img.shields.io/endpoint?url=https%3A%2F%2Fraw.githubusercontent.com%2Fcreative-force-io%2Fmcp-docs%2Fmaster%2Ftools.json)](tool-reference.md)
![Auth](https://img.shields.io/badge/auth-OAuth%202.0%20%2B%20PKCE-green)
![Access](https://img.shields.io/badge/access-read--first-brightgreen)

> Connect Claude, ChatGPT, Microsoft Copilot, Notion, and other MCP-compatible clients to your
> Creative Force photo-production data — studio-scoped, permission-filtered, and authenticated with
> your own Creative Force account.

## What this is

[Creative Force](https://creativeforce.io) is the production platform for high-volume
e-commerce and editorial content studios. The **Creative Force MCP server** exposes your
studio's live production data to AI assistants through the
[Model Context Protocol](https://modelcontextprotocol.io), so you can ask questions like
*"how many products are still in photography for the spring catalog?"* and get answers
grounded in your real data.

The server is **read-first**: of its 34 tools, **30 are read-only** queries that never change your
data. The four that write are limited and explicit — `create_planning_session`,
`update_planning_session`, `delete_planning_session` (flagged destructive, so clients prompt before
it runs), and `send_feedback`, which sends a note to the Creative Force team and writes nothing to
your studio's data. Write tools live in their own **Write/Delete** permission group and are off by
default for every role; see [permissions.md](permissions.md).

## What you can ask about

The server exposes **34 tools** across the Creative Force data model:

| Category | What you can query |
|----------|--------------------|
| Production | Search productions by step, vendor, photographer, or location |
| Assets | Search digital assets by job, product, step, or time range — with inline image previews |
| Jobs | Search production jobs by code, status, or deadline |
| Products | Search the product catalogue by code, name, category, brand, or status |
| Tasks | Search workflow tasks by step, status, assignee, or vendor |
| Samples | Search physical samples and track check-in status and location |
| Planning | Search planning sessions by team member or time slot — **and create, change or delete them** |
| Talent &amp; Crew | Talent and crew records, avatars, scheduling and availability |
| Editorial | Editorial projects, productions, and deliverables |
| Event Log | Find specific events and who performed them, for products, samples and more |
| Style Guides | Style guide configuration — shot positions, asset naming, colour rules |
| Workflows | Workflow configuration — steps, settings, rejection transitions |
| Studio Settings | Production types, locations, containers, presets, data sources, on-set skills, product and post-production vendors, print configurations |
| Workspaces | Your workspaces (clients) and their settings |
| Feedback | Send feedback, report issues, or request features |

The complete, always-current tool list — every parameter, type, and example — lives in
**[tool-reference.md](tool-reference.md)**, generated directly from the server's source code.

## Who it's for

- **Studio managers and producers** — self-serve answers about production status without
  building a report.
- **Technical evaluators** — assess whether Creative Force's AI integration fits your
  workflow before contacting sales.
- **Developers** — connect Creative Force to your own MCP-compatible agent.

## Get started

1. **[Setup guide →](setup.md)** — connect Claude, ChatGPT, Copilot or Notion and run your first
   query.
2. **[Tool reference →](tool-reference.md)** — every tool and parameter.
3. **[Use cases and prompts →](use-cases.md)** — realistic example prompts.
4. **[Permissions →](permissions.md)** — how tool access is granted per user role.
5. **[Privacy and data handling →](privacy.md)** — what the server reads and does not store.
6. **[Support →](support.md)** — how to get help.

## Server endpoint

```
https://mcp.creativeforce.io/mcp
```

Authentication is the OAuth 2.0 authorization code flow with PKCE, using your Creative Force
account — see the [setup guide](setup.md). You will only see the tools your account's permissions
allow.

---

<sub>Documentation for the Creative Force MCP server. The tool reference is generated from the
server source — see [CHANGELOG.md](CHANGELOG.md). Security policy: [SECURITY.md](SECURITY.md).</sub>
