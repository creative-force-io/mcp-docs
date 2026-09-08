# Permissions

The Creative Force MCP server shows each user **only the tools their account is allowed to use**.
Access is set **per user role** in Creative Force, so a studio can decide exactly what an AI
assistant may see — and change — on each person's behalf. This page explains how that works.

> This is the page linked from **"Learn more"** on the **User Roles → MCP SERVER** tab.

## A tool needs two permissions

To use any tool, a role needs **both**:

1. **An MCP SERVER permission** — the tool's **group** is allowed (or the tool itself is turned on
   under **Custom**), on the **MCP SERVER** tab.
2. **A Creative Force area permission** — the role can already access the data the tool reads
   (for example **Assets Hub**, **Products**, **Samples**), set in that area's own section of the role.

Both are required. Turning a tool on under MCP SERVER is **not enough on its own** — if the role
cannot see the **Assets Hub** in Creative Force, the Assets tools stay hidden even when the MCP tool is allowed.
The MCP layer never widens what a user can already access; it only decides which tools are exposed
to the assistant.

## The MCP SERVER tab

Tools are organised into two groups:

| Group | What's in it |
|-------|--------------|
| **Read-Only** | Tools that read data without making any changes — 35 of the 39 tools. |
| **Write/Delete** | Tools that change data — the three planning-session tools, gated by the single **Write Planning** permission. |

Each group has one control with four options:

| Option | Effect |
|--------|--------|
| **Allow All (incl. new tools)** | Every tool in the group is allowed, **including tools added later** — new tools are turned on automatically for this role, with no further setup. |
| **Allow All** | Every tool that exists today is allowed. A tool added later is **not** turned on automatically. |
| **Block All** | Every tool in the group is off. |
| **Custom** | Turn each tool on or off individually. |

Only **Allow All (incl. new tools)** covers future tools — by design, so that adding a tool never
silently grants it everywhere. The setting is per role; the server applies it on each request.

Individual permissions have two settings, **None** and **Access**:

| Control | Behaviour |
|---------|-----------|
| **None** | When a connected AI assistant tries to use that capability, the request fails. |
| **Access** | When a connected AI assistant tries to use that capability, the request succeeds. |

A role granted only the **Read-Only** group gets a strictly read-only server.

## What each tool needs

Each tool needs **two** separate permissions, both named exactly as they appear in the role popup:

1. its **MCP SERVER** permission — the tool's own row on the **MCP SERVER** tab (e.g. *Query
   Asset*); and
2. a **Creative Force permission** — the data area the tool reads (section → permission).

One MCP SERVER permission usually covers a whole **category** of tools rather than a single tool.
The Creative Force permission must be set in addition, in its own section of the role.

### Read-Only group

| MCP SERVER permission | Tools it covers | Creative Force permission (section → permission) |
|-----------------------|-----------------|--------------------------------------------------|
| Query Asset | `query_asset`, `get_asset_preview` | Assets → Assets Hub (View) |
| Query E-Comm Job | `query_ecomm_job` | E-COMM → Jobs (View) |
| Query E-Comm Production | `query_ecomm_production` | E-COMM → Production (View) |
| Query E-Comm Product Request | `query_ecomm_product_request` | E-COMM → Products (View) |
| Query Editorial Production | `query_editorial_production` | Editorial → Production (View) |
| Query Editorial Project | `query_editorial_project` | Editorial → Editorial projects (View) |
| Query Editorial Deliverable | `query_editorial_deliverable` | Editorial → Editorial deliverables (View) |
| Query Task | `query_task` | Task Management → Photography Management, Internal Post Management, Digital Processing Management (View) |
| Query Sample | `query_sample` | Samples → Samples (View) |
| Query Planning | `query_planning` | Planning → Calendar, Set, Talent &amp; Crew (View) |
| Query Talent &amp; Resource | `query_talent_crew`, `get_talent_crew_preview`, `query_talent_crew_schedule` | Planning → Talent &amp; Crew (View); Resourcing for the schedule tool |
| Query Collection | `query_collection`, `query_collection_detail`, `query_shortlist`, `query_shortlist_detail` | Collaboration → Gallery Collections, Review Collections, Selection Collections, Short Lists (View) |
| Query Event Log | `query_event_log` | Studio Settings → Event Log (View) |
| Query Style Guide | `query_styleguide`, `get_styleguide_detail` | Studio Settings → Style Guides (View) |
| Query Workflow | `query_workflow`, `get_workflow_detail` | Studio Settings → Workflow (View) |
| Query Presets | `query_presets`, `query_preset_detail` | Studio Settings → Presets Settings (View) |
| Query Studio Setting | `query_containers`, `query_data_sources`, `query_locations`, `query_post_production_vendors`, `query_print_configurations`, `query_product_vendors`, `query_production_types`, `query_team_on_set_skills` | Studio Settings → the matching sub-setting (e.g. Production Types) (View) |
| Query Workspace | `query_workspace` | Studio Settings → Clients (View) |

So a role using `query_asset`, for example, needs **both** *Query Asset* (MCP SERVER) **and**
*Assets Hub* (Assets) — granting one without the other leaves the tool unavailable.

### Write/Delete group

| MCP SERVER permission | Tools it covers | Creative Force permission |
|-----------------------|-----------------|---------------------------|
| Write Planning | `create_planning_session`, `update_planning_session`, `delete_planning_session` | Planning, at **Edit** level |

**Query Planning and Write Planning are separate.** A role with *Query Planning* but not *Write
Planning* can ask questions about sessions but cannot create, change, or delete them. Granting
*Write Planning* is what turns session management on.

Notes:

- A single MCP SERVER permission can cover several tools — e.g. *Query Asset* gates both
  `query_asset` and `get_asset_preview`, and *Query Studio Setting* gates all nine studio-settings
  tools. Each still needs its own Creative Force permission. A tool's permission name cannot be
  inferred from its tool name, so use this table or the role popup itself.
- This list is generated from the server's source and grows as tools are added. The complete,
  always-current tool list — with parameters and examples — is in
  **[tool-reference.md](tool-reference.md)**.

## Gating that is not a role permission

Two things narrow what a tool returns even when the role permissions above are fully granted.
Neither is set on the MCP SERVER tab:

- **Field-level permission.** `query_talent_crew` returns a talent's **rates** only when the role has
  the **Rates** screen permission. When rates are withheld the response says so explicitly
  (`ratesAccess: denied`) — that means hidden by permission, not absent. Do not read a missing
  `rates` array as "this talent has no rates configured".
- **Plan features.** Parts of `get_styleguide_detail` depend on the studio's subscription rather than
  the user's role: the Delivery tab needs the **Advanced Style Guides** plan feature (or pre-existing
  saved routing), and the Localization tab needs the **Localization** plan feature. A tab the user
  cannot see on the Style Guide screen is not surfaced through the tool either.
- **Subscription add-ons.** Some tools read Creative Force features that are sold as add-ons. Each
  collection type (Photo Review, Post Review, Selection, Gallery) requires its own add-on;
  `query_shortlist` requires the **Short List (Resources)** add-on; editorial presets under
  `query_preset_detail` require **Editorial Projects**. Querying something the studio has not
  purchased returns an explanatory error rather than an empty list, so an assistant can tell "you
  don't have this" apart from "there is nothing here". This is about the underlying Creative Force
  features — **the MCP server itself needs no add-on**, and access to it is decided solely by the
  role permissions above.

## Always available

**`query_property`** needs no permission and is not shown on the MCP SERVER tab — it returns the
available filter properties and carries no studio data of its own.

`send_feedback` **does** have its own **Send Feedback** permission on the MCP SERVER tab. It is not a
read-only tool, but it is not in the Write/Delete group either: it writes only to Creative Force's
own feedback inbox, never to your studio's data.

## Applying changes

Permission changes take effect within about ten minutes, or immediately if the user
re-authenticates. All MCP tool permissions default to **None** for every standard role, so access is
always an explicit grant.

---

<sub>See also: the [setup guide](setup.md) for connecting a client, and
[privacy.md](privacy.md) for what the server reads and stores. Permission names and screen mappings
on this page follow the Creative Force Help Centre articles
<a href="https://help.creativeforce.io/en/articles/15566092-mcp-server-data-queries">Data Queries</a>
and <a href="https://help.creativeforce.io/en/articles/16072867-mcp-server-manage-planning-sessions">Manage Planning Sessions</a>.</sub>
