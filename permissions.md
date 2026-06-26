# Permissions

The Creative Force MCP server shows each user **only the tools their account is allowed to use**.
Access is set **per user role** in Creative Force, so a studio can decide exactly what an AI
assistant may see on each person's behalf. This page explains how that works.

> This is the page linked from **"Learn more"** on the **User Roles → MCP SERVER** tab.

## A tool needs two permissions

A tool is available to a user only when **both** of these are true:

1. **MCP-tool permission** — the tool (or its group) is turned on for the role, on the
   **MCP SERVER** tab.
2. **Data-area permission** — the role also has permission for the Creative Force area the tool
   reads (for example **Assets**, **Products**, **Samples**), set in that area's own section of
   the role.

Turning a tool on under MCP SERVER is **not enough on its own** — if the role cannot see Assets in
Creative Force, the Assets tools stay hidden for that user even when the MCP tool is allowed. The
MCP layer never widens what a user can already access; it only decides which tools are exposed to
the assistant.

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

## Which data-area permission each tool needs

Every tool reads one Creative Force area and needs that area's permission in addition to the MCP
tool being allowed. The current tools:

| Creative Force area | Data-area permission | Tools |
|---------------------|----------------------|-------|
| Jobs | Jobs | `query_ecomm_job` |
| Production | Production | `query_ecomm_production`, `query_task` |
| Product Hub | Products | `query_ecomm_product_request` |
| Assets | Assets | `query_asset`, `get_asset_preview` |
| Samples | Samples | `query_sample` |
| Planning | Planning | `query_planning` |
| Workflow | Workflow | `query_workflow`, `get_workflow_detail` |
| Style Guides | Style Guides | `query_styleguide`, `get_styleguide_detail` |
| Talent &amp; Crew | Talent &amp; Crew | `query_talent_crew`, `get_talent_crew_preview` |
| Talent scheduling | Resourcing Calendar | `query_talent_crew_schedule` |
| Editorial | Editorial Projects | `query_editorial_project` |
| Editorial | Editorial Production | `query_editorial_production` |
| Editorial | Editorial Deliverables | `query_editorial_deliverable` |
| Event Log | Event Log | `query_event_log` |
| Workspaces | Clients | `query_workspace` |
| Studio Settings | Containers | `query_containers` |
| Studio Settings | Data Sources | `query_data_sources` |
| Studio Settings | Locations &amp; Sets | `query_locations` |
| Studio Settings | Post-Production Vendors | `query_post_production_vendors` |
| Studio Settings | Presets | `query_presets` |
| Studio Settings | Print Configurations | `query_print_configurations` |
| Studio Settings | Product Vendors | `query_product_vendors` |
| Studio Settings | Production Types | `query_production_types` |
| Studio Settings | *(no extra area)* | `query_team_on_set_skills` |

The **Studio Settings** tools share one MCP-tool permission, but each still needs its own data-area
permission (Containers, Data Sources, and so on) for the matching tool to appear.

> This list is generated from the server's source and grows as tools are added. The complete,
> always-current tool list — with parameters and examples — is in
> **[tool-reference.md](tool-reference.md)**.

## Always available

Two tools need no permission and are not shown on the MCP SERVER tab:

- **`send_feedback`** — sends a note to the Creative Force team. Available to every connected user.
- **`query_property`** — returns the available filter properties; carries no studio data of its own.

## Write/Delete tools

The **Write/Delete** group is in place for tools that change data. On a standard read-only
deployment it is empty. Where write tools are enabled (for example the planning write tools), they
follow the same model: the role needs the MCP-tool permission **and** the underlying data-area
permission at the right level — so allowing the group covers the MCP side, while each tool still
respects the data-area permission it writes to.

---

<sub>See also: the [setup guide](setup.md) for connecting a client, and
[privacy.md](privacy.md) for what the server reads and stores.</sub>
