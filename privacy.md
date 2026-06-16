# Privacy and data handling

> **Review status:** This page describes how the Creative Force MCP server accesses and handles
> data, based on the server's actual behaviour. It must be reviewed and approved by a Creative
> Force data/privacy stakeholder before publication.

## Overview

The Creative Force MCP server lets an AI assistant query your studio's Creative Force production
data on your behalf. It is **read-only** and **studio-scoped**: it answers questions using the data
your own Creative Force account can already see, and it does not change that data. The one exception
is `send_feedback`, which sends a message to the Creative Force team.

Access is authenticated with your own Creative Force account (OAuth 2.1) — there is no separate
login and no shared API key.

## What the server accesses

When you ask a question, the server reads — strictly within your studio and limited to what your
Creative Force permissions already allow — from these areas:

- Jobs, productions (work units), and workflow steps/tasks
- Product catalog and physical samples
- Asset metadata and image previews
- Planning sessions and schedules
- Talent and crew records, including avatars and availability
- Editorial projects, productions, and deliverables
- Your workspaces and their settings

It reads this data live from Creative Force each time you ask. The server does **not** maintain its
own separate database of your business records.

## What the server does not do

- It does **not** create, edit, or delete your production data. The only write operation is
  `send_feedback`, which goes to the Creative Force team — not to your studio's records.
- It does **not** provide access across studios. A session only ever sees the authenticated user's
  studio.
- It does **not** bypass your permissions. You only see the tools and data for areas your Creative
  Force account is already allowed to access.

## Authentication and access control

- **OAuth 2.1 with PKCE**, using your Creative Force account. Access tokens are validated on each
  request and cached only briefly (typically until the token expires) to avoid re-validating every
  call; they are not retained beyond that.
- **Subscription-gated** — the server is available only to studios whose subscription includes the
  MCP Server feature.
- **Permission-filtered** — tools and results are filtered by your Creative Force screen permissions
  on every request.

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
