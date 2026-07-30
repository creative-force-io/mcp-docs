# Creative Force MCP Server

[![Tools](https://img.shields.io/badge/tools-16-blue)](tool-reference.md)
![Auth](https://img.shields.io/badge/auth-OAuth%202.1-green)
![Access](https://img.shields.io/badge/access-read--only-brightgreen)

> Connect Claude, Gamma, and other MCP-compatible clients to your Creative Force
> photo-production data — read-only, studio-scoped, and authenticated with your own
> Creative Force account.

## What this is

[Creative Force](https://creativeforce.io) is the production platform for high-volume
e-commerce and editorial content studios. The **Creative Force MCP server** exposes your
studio's live production data to AI assistants through the
[Model Context Protocol](https://modelcontextprotocol.io), so you can ask questions like
*"how many products are still in photography for the spring catalog?"* and get answers
grounded in your real data.

The server is **read-only** — it answers questions, it does not change your data. The single
exception is `send_feedback`, which sends a note to the Creative Force team.

## What you can ask about

The server exposes **16 tools** across the Creative Force data model:

| Area | What you can query |
|------|--------------------|
| Jobs | Production jobs — code, name, status, deadline |
| Production | Work units (the central production entity) — status, step, vendor, team |
| Products | Product catalog — code, name, category, brand, colour, style |
| Workflow | Workflow steps/tasks — status, step, assignee, vendor |
| Assets | Asset metadata + inline image previews |
| Samples | Physical samples — check-in status, location, return date |
| Planning | Scheduled shoots — session, date, location, team |
| Talent &amp; Crew | Talent/crew records, avatars, scheduling and availability |
| Editorial | Editorial projects, productions, and deliverables |
| Workspaces | Your workspaces (clients) and their settings |

The complete, always-current tool list — every parameter, type, and example — lives in
**[tool-reference.md](tool-reference.md)**, generated directly from the server's source code.

## Who it's for

- **Studio managers and producers** — self-serve answers about production status without
  building a report.
- **Technical evaluators** — assess whether Creative Force's AI integration fits your
  workflow before contacting sales.
- **Developers** — connect Creative Force to your own MCP-compatible agent.

## Get started

1. **[Setup guide →](setup.md)** — connect Claude or Gamma and run your first query.
2. **[Tool reference →](tool-reference.md)** — every tool and parameter.
3. **[Use cases and prompts →](use-cases.md)** — realistic example prompts.
4. **[Permissions →](permissions.md)** — how tool access is granted per user role.
5. **[Privacy and data handling →](privacy.md)** — what the server reads and does not store.
6. **[Support →](support.md)** — how to get help.

## Server endpoint

```
https://mcp.creativeforce.io/mcp
```

Authentication is OAuth 2.1 with your Creative Force account — see the [setup guide](setup.md).
You will only see the tools your account's permissions allow.

---

<sub>Documentation for the Creative Force MCP server. The tool reference is generated from the
server source — see [CHANGELOG.md](CHANGELOG.md). Security policy: [SECURITY.md](SECURITY.md).</sub>
