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
the group, or turn the tool on under **Custom**) **and** the **Creative Force area permission** for
the data it reads.

| Tool | MCP SERVER group | Creative Force area permission |
|------|------------------|--------------------------------|
| `query_ecomm_job` | Read-only | Jobs |
| `query_ecomm_production` | Read-only | E-comm Production |
| `query_task` | Read-only | E-comm Production |
| `query_ecomm_product_request` | Read-only | Products |
| `query_asset` | Read-only | Resources Assets |
| `get_asset_preview` | Read-only | Resources Assets |
| `query_sample` | Read-only | Samples |
| `query_planning` | Read-only | Planning View |
| `query_workflow` | Read-only | Workflow |
| `get_workflow_detail` | Read-only | Workflow |
| `query_styleguide` | Read-only | Style Guides |
| `get_styleguide_detail` | Read-only | Style Guides |
| `query_talent_crew` | Read-only | Talent &amp; Crew |
| `get_talent_crew_preview` | Read-only | Talent &amp; Crew |
| `query_talent_crew_schedule` | Read-only | Resourcing Calendar |
| `query_editorial_project` | Read-only | Editorial Projects |
| `query_editorial_production` | Read-only | Editorial Productions |
| `query_editorial_deliverable` | Read-only | Editorial Deliverables |
| `query_event_log` | Read-only | Event Log Access |
| `query_workspace` | Read-only | Clients |
| `query_containers` | Read-only | Container |
| `query_data_sources` | Read-only | Data Source |
| `query_locations` | Read-only | Locations &amp; Sets |
| `query_post_production_vendors` | Read-only | Post Production Vendors |
| `query_presets` | Read-only | Preset |
| `query_print_configurations` | Read-only | Print Configurations |
| `query_product_vendors` | Read-only | Product Vendors |
| `query_production_types` | Read-only | Production Types |
| `query_team_on_set_skills` | Read-only | *(no extra area)* |
| `create_planning_session` | Write/Delete | Planning View *(write)* |
| `update_planning_session` | Write/Delete | Planning View *(write)* |
| `delete_planning_session` | Write/Delete | Planning View *(write)* |

Notes:

- The **Studio Settings** tools share one MCP SERVER entry, but each still needs **its own** area
  permission (Containers, Data Sources, and so on) for that tool to appear.
- The **Write/Delete** tools are present only where write tools are enabled; on a standard
  read-only deployment that group is empty. They also need their Creative Force area at **write**
  level, not just read.
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
