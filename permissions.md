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

Each tool needs **two** separate permissions, both named exactly as they appear in the role popup:

1. its **MCP SERVER** permission — the tool's own row on the **MCP SERVER** tab (e.g. *Query
   Asset*); and
2. a **Creative Force permission** — the data area the tool reads (section → permission).

Every current tool is in the **Read-only** group, so allowing that group (any "Allow All…" option)
grants all the MCP SERVER permissions below at once — or grant each one individually under
**Custom**. The Creative Force permission must be set in addition, in its own section of the role.

| Tool | MCP SERVER permission | Creative Force permission (section → permission) |
|------|-----------------------|--------------------------------------------------|
| `query_asset` | Query Asset | Assets → Assets Hub |
| `get_asset_preview` | Query Asset | Assets → Assets Hub |
| `query_ecomm_job` | Query E-Comm Job | E-Comm → Jobs |
| `query_ecomm_production` | Query E-Comm Production | E-Comm → Production |
| `query_task` | Query Task | E-Comm → Production |
| `query_ecomm_product_request` | Query E-Comm Product Request | E-Comm → Products |
| `query_sample` | Query Sample | Samples → Samples |
| `query_planning` | Query Planning | Planning → Calendar |
| `query_talent_crew` | Query Talent &amp; Resource | Planning → Talent &amp; Crew |
| `get_talent_crew_preview` | Query Talent &amp; Resource | Planning → Talent &amp; Crew |
| `query_talent_crew_schedule` | Query Talent &amp; Resource | Planning → Resourcing |
| `query_editorial_project` | Query Editorial Project | Editorial → Editorial projects |
| `query_editorial_production` | Query Editorial Production | Editorial → Production |
| `query_editorial_deliverable` | Query Editorial Deliverable | Editorial → Editorial deliverables |
| `query_workspace` | Query Workspace | Studio Settings → Clients |

So a role using `query_asset`, for example, needs **both** *Query Asset* (MCP SERVER) **and**
*Assets Hub* (Assets) — granting one without the other leaves the tool unavailable.

Notes:

- A single MCP SERVER permission can cover several tools — e.g. *Query Asset* gates both
  `query_asset` and `get_asset_preview`, and *Query Talent &amp; Resource* gates all three talent
  tools. Each still needs its own Creative Force permission.
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
