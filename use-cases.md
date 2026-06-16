# Use cases & example prompts

Realistic prompts for working with your Creative Force data through an MCP client (Claude, Gamma,
etc.). Each example notes the **tool** it triggers and **what you get back**.

> These are illustrative. Actual results depend on **your studio's data** and **your permissions** —
> you only ever see what your Creative Force account already has access to. Everything here is
> **read-only** (the one exception is `send_feedback`).

## Jobs & production

> "How many production jobs are still open for the Spring 2026 catalog?"
- **Tool:** `query_ecomm_job`
- **You get:** a count/list of jobs filtered by name and status.

> "Show me the work units stuck in QC for vendor team Acme this week."
- **Tool:** `query_ecomm_production`
- **You get:** production work units filtered by status/step/vendor, with their current step.

> "Break down the workflow tasks assigned to Jane by status."
- **Tool:** `query_task`
- **You get:** workflow steps for that assignee, aggregated by status.

## Products & samples

> "List footwear products from brand Nike that aren't finished yet."
- **Tool:** `query_ecomm_product_request`
- **You get:** catalog products filtered by category, brand, and status.

> "Which physical samples for job J123 are still waiting to be checked in?"
- **Tool:** `query_sample`
- **You get:** samples for that job filtered by check-in status, with location and return date.

## Assets

> "How many approved assets do we have for the summer shoot, grouped by step?"
- **Tool:** `query_asset`
- **You get:** asset metadata counts aggregated by step (no images — metadata only).

> "Show me the hero images for product P456."
- **Tool:** `get_asset_preview` (often chained after `query_asset`)
- **You get:** an inline image gallery for the matching asset IDs.

## Planning & talent

> "What shoots are scheduled at the London studio next week, and who's on set?"
- **Tool:** `query_planning`
- **You get:** planning sessions filtered by location/date, with the production team.

> "Find photographers available to shoot next Tuesday."
- **Tool:** `query_talent_crew_schedule`
- **You get:** availability and bookings grouped by talent, with status and on-set role.

> "Who are our short-listed models with a fitness background?"
- **Tool:** `query_talent_crew`
- **You get:** talent/crew records filtered by skill and short-list, with contact details (subject
  to permissions).

> "Show me photos of the crew booked for the Nike session."
- **Tool:** `get_talent_crew_preview`
- **You get:** an inline avatar gallery (use for purely visual prompts).

## Editorial

> "List the editorial projects launching this quarter."
- **Tool:** `query_editorial_project`
- **You get:** editorial projects matching the date range.

> "Which editorial deliverables are overdue for the March issue?"
- **Tool:** `query_editorial_deliverable`
- **You get:** deliverables filtered by due date and status, per output unit.

> "Show the editorial productions still in progress for project Vogue-SS26."
- **Tool:** `query_editorial_production`
- **You get:** editorial work units for that project filtered by status.

## Workspaces & feedback

> "List my Creative Force workspaces."
- **Tool:** `query_workspace`
- **You get:** your workspaces (clients) with timezone/calendar settings — also the lightest way to
  test the connection.

> "That job-list answer was off — send feedback to the Creative Force team."
- **Tool:** `send_feedback`
- **You get:** your feedback delivered to Creative Force (the only tool that writes anything, and it
  writes nothing to your studio's data).

---

See the full parameter-level detail for every tool in the **[tool reference](tool-reference.md)**,
and how to connect in the **[setup guide](setup.md)**.
