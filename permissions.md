# Permissions

The Creative Force MCP server shows each user **only the tools their account is allowed to use**.
Access is set **per user role** in Creative Force, so a studio can decide exactly what an AI
assistant may see on each person's behalf. This page explains how that works.

> This is the page linked from **"Learn more"** on the **User Roles → MCP SERVER** tab.

## A tool needs two permissions

To use any tool, a role needs **both**:

1. **An MCP SERVER permission** — the tool's **group** is allowed (or the tool itself is turned on
   under **Custom**), on the **MCP SERVER** tab.
2. **A Creative Force area permission** — the role can already access the data the tool reads
   (for example **Assets**, **Products**, **Samples**), set in that area's own section of the role.

Both are required. Turning a tool on under MCP SERVER is **not enough on its own** — if the role
cannot see Assets in Creative Force, the Assets tools stay hidden even when the MCP tool is allowed.
The MCP layer never widens what a user can already access; it only decides which tools are exposed
to the assistant.

## The MCP SERVER tab

Tools are organised into two groups:

| Group | What's in it |
|-------|--------------|
| **Read-only** | Tools that read data without making any changes. |
| **Write/Delete** | Tools that can read data and make changes. |

Each group has one control with four options:

| Option | Effect |
|--------|--------|
| **Allow All (incl. new tools)** | Every tool in the group is allowed, **including tools added later** — new tools are turned on automatically for this role, with no further setup. |
| **Allow All** | Every tool that exists today is allowed. A tool added later is **not** turned on automatically. |
| **Block All** | Every tool in the group is off. |
| **Custom** | Turn each tool on or off individually. |

Only **Allow All (incl. new tools)** covers future tools — by design, so that adding a tool never
silently grants it everywhere. The setting is per role; the server applies it on each request.

## What each tool needs

Each row lists the **two** permissions required to use that tool: its **MCP SERVER** group (allow
the group, or turn the tool on under **Custom**) **and** the **Creative Force permission** the role
must also have. The permission is named exactly as it appears in the role popup, under its section.

| Tool | MCP SERVER group | Permission section | Permission |
|------|------------------|--------------------|------------|
| `query_ecomm_job` | Read-only | E-Comm | Jobs |
| `query_ecomm_production` | Read-only | E-Comm | Production |
| `query_task` | Read-only | E-Comm | Production |
| `query_ecomm_product_request` | Read-only | E-Comm | Products |
| `query_asset` | Read-only | Assets | Assets Hub |
| `get_asset_preview` | Read-only | Assets | Assets Hub |
| `query_sample` | Read-only | Samples | Samples |
| `query_planning` | Read-only | Planning | Calendar |
| `query_talent_crew` | Read-only | Planning | Talent &amp; Crew |
| `get_talent_crew_preview` | Read-only | Planning | Talent &amp; Crew |
| `query_talent_crew_schedule` | Read-only | Planning | Resourcing |
| `query_editorial_project` | Read-only | Editorial | Editorial projects |
| `query_editorial_production` | Read-only | Editorial | Production |
| `query_editorial_deliverable` | Read-only | Editorial | Editorial deliverables |
| `query_workspace` | Read-only | Studio Settings | Clients |

Notes:

- Every tool today is **read-only**, so the **Write/Delete** group is currently empty. It is in
  place for future tools that change data; those will need their Creative Force permission at
  **Edit** level, not just View.
- This list is generated from the server's source and grows as tools are added. The complete,
  always-current tool list — with parameters and examples — is in
  **[tool-reference.md](tool-reference.md)**.

## Always available

Two tools need **no permission** and are not shown on the MCP SERVER tab:

- **`send_feedback`** — sends a note to the Creative Force team. Available to every connected user.
- **`query_property`** — returns the available filter properties; it carries no studio data of its
  own.

---

<sub>See also: the [setup guide](setup.md) for connecting a client, and
[privacy.md](privacy.md) for what the server reads and stores.</sub>
