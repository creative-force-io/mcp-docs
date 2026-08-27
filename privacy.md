# Privacy and data handling

> **Review status:** This page describes how the Creative Force MCP server accesses and handles
> data, based on the server's actual behaviour. It must be reviewed and approved by a Creative
> Force data/privacy stakeholder before publication.

## Overview

The Creative Force MCP server lets an AI assistant query your studio's Creative Force production
data on your behalf. It is **read-first** and **studio-scoped**: it answers questions using the data
your own Creative Force account can already see. 35 of its 39 tools are read-only. The four that
write are `create_planning_session`, `update_planning_session` and `delete_planning_session` — which
change planning sessions only — and `send_feedback`, which sends a message to the Creative Force
team. Write tools are off by default and must be granted per role.

Access is authenticated with your own Creative Force account (OAuth 2.0 authorization code flow with
PKCE) — there is no separate login and no shared API key.

## What the server accesses

When you ask a question, the server reads — strictly within your studio and limited to what your
Creative Force permissions already allow — from these areas:

- Jobs, productions (work units), and workflow definitions, steps and tasks
- Product catalog and physical samples
- Asset metadata and image previews
- Style guides and their capture/output requirements
- Planning sessions and schedules
- Talent and crew records, including avatars, availability and — where your role has the **Rates**
  screen permission — their rate information
- Editorial projects, productions, and deliverables
- Review and approval collections: per-asset decisions, ratings, colour flags and markings, comment
  text with its author and timestamp, the invited-reviewer roster (name, email, role, and whether
  they have opened the review), and the collection's sharing settings including its share link and
  any domain allow-list
- Casting short lists: the talent on each list, likes and dislikes attributed by name, comments, and
  full talent profiles including rates where your role has the **Rates** permission
- The event log for a record: who changed it and when
- Studio settings: locations, presets, print configurations, production types, product and
  post-production vendors, containers, data sources, and on-set skills
- Your workspaces and their settings

It reads this data live from Creative Force each time you ask. The server does **not** maintain its
own separate database of your business records.

## What the server can change

The server can create, update and delete **planning sessions**, and nothing else. Those three tools
are off by default: they require the **Write Planning** permission on your role, which is separate
from *Query Planning*, plus the underlying Creative Force permission at Edit level. Deleting a
session is annotated as destructive, so MCP clients prompt for confirmation before it runs.

Changes are written to Creative Force immediately and appear in the planning view straight away.
**There is no undo** — to reverse a change, make the opposite change or adjust it directly in
Creative Force. Deletion is guarded: a session with confirmed bookings or outfits attached will warn
or stop rather than leave those pointing at nothing.

Session management covers the session and the schedule, set, team and products on it. It cannot
change Resourcing, outfits, the underlying product request or production task, or Talent &amp; Crew
user properties — a person's skills, agency, rate and personal details are not modifiable via MCP.

## What the server does not do

- It does **not** create, edit, or delete any production data outside planning sessions. Jobs,
  products, samples, assets, tasks, editorial records and studio settings are read-only through
  every tool.
- It does **not** provide access across studios. A session only ever sees the authenticated user's
  studio.
- It does **not** bypass your permissions. You only see the tools and data for areas your Creative
  Force account is already allowed to access, and a write tool additionally needs Edit rights.

## Authentication and access control

- **OAuth 2.0 authorization code flow with PKCE**, using your Creative Force account. Access tokens
  are validated on each request and cached only briefly (typically until the token expires) to avoid
  re-validating every call; they are not retained beyond that.
- **Permission-filtered** — tools and results are filtered by your Creative Force screen permissions
  on every request. Permission changes take effect within about ten minutes, or immediately on
  re-authentication.

No subscription add-on is required; access is decided solely by the MCP tool permissions granted on
your role.

## Logging and analytics

To keep the service reliable and to understand how it is used, Creative Force records information
about each tool call:

- **The tool you used and the parameters you provided.** Parameters can include the search terms,
  names, or codes you put into a question. These are recorded in operational logs and monitoring,
  and a tool-usage event is also sent to a **third-party product-analytics provider**.
- **A truncated snippet of the result** (up to the first 1,024 characters) is recorded in
  operational logs and monitoring to help diagnose problems. This snippet is **not** sent to the
  analytics provider.
- Each record is associated with your user identifier, your studio identifier, and a session
  identifier.

This information is used for operational monitoring, troubleshooting, and product improvement, and
is handled under Creative Force's main privacy policy — which describes retention periods, data
location, and the full list of sub-processors.

## Data retention and location

Retention periods and the data-hosting region are governed by Creative Force's main privacy policy.

## Your rights and contact

Requests regarding your personal data are handled in line with Creative Force's main privacy policy:
[Creative Force privacy policy](https://creativeforce.io).

To report a security or privacy concern about the MCP server specifically, see
[SECURITY.md](SECURITY.md).

---

<sub>Part of the Creative Force MCP documentation. This page reflects the server's behaviour at the
time of writing.</sub>
