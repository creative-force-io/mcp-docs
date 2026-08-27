# Use cases & example prompts

Realistic prompts for working with your Creative Force data through an MCP client (Claude, ChatGPT,
Copilot, Notion). Each section covers one **query category** — the unit MCP permissions are granted
in — and notes the tools it triggers.

> These are illustrative. Actual results depend on **your studio's data** and **your permissions** —
> you only ever see what your Creative Force account already has access to. Everything is read-only
> except [Managing planning sessions](#managing-planning-sessions), which is clearly marked.

## Query categories at a glance

| Category | What you can ask about | Tools |
|----------|------------------------|-------|
| [Production](#production) | Search productions by step, vendor, photographer, or location | `query_ecomm_production`, `query_editorial_production` |
| [Assets](#assets) | Search digital assets by job, product, step, or time range | `query_asset`, `get_asset_preview` |
| [Jobs](#jobs) | Search production jobs by code, status, or deadline | `query_ecomm_job` |
| [Products](#products) | Search the product catalogue by code, name, category, brand, or status | `query_ecomm_product_request` |
| [Tasks](#tasks) | Search workflow tasks by step, status, assignee, or vendor | `query_task` |
| [Samples](#samples) | Search physical samples and track check-in status and location | `query_sample` |
| [Planning](#planning) | Search planning sessions by team member or time slot | `query_planning`, `query_talent_crew`, `query_talent_crew_schedule` |
| [Editorial](#editorial) | Editorial projects, productions, and deliverables | `query_editorial_project`, `query_editorial_deliverable` |
| [Collections](#collections) | Review and approval collections — decisions, ratings, comments, reviewers | `query_collection`, `query_collection_detail` |
| [Short Lists](#short-lists) | Casting short lists — talent, votes, comments and feedback | `query_shortlist`, `query_shortlist_detail` |
| [Event Log](#event-log) | Find specific events and who performed them, for products, samples and more | `query_event_log` |
| [Style Guides](#style-guides) | Style guide configuration — shot positions, asset naming, colour rules | `query_styleguide`, `get_styleguide_detail` |
| [Workflows](#workflows) | Workflow configuration — steps, settings, rejection transitions | `query_workflow`, `get_workflow_detail` |
| [Studio Settings](#studio-settings) | How your studio is configured | nine `query_*` tools |
| [Managing planning sessions](#managing-planning-sessions) | **Create, change and delete** sessions | `create_planning_session`, `update_planning_session`, `delete_planning_session` |
| [Feedback](#feedback) | Send feedback, report issues, or request features | `send_feedback` |

## Production

Productions are the core unit of shoot activity in Creative Force — each represents a scheduled
session with an assigned photographer, vendor, and workflow step.

- *"Which photographers have the most productions in progress right now?"*
- *"What percentage of our jobs are currently in post-production?"*
- *"How many of each production type were completed this week?"*

## Assets

Assets are the digital output of your productions — the photos and files that move through
post-production after a shoot. This gives your assistant visibility into what has been delivered and
when.

- *"How many assets did we produce yesterday?"*
- *"Show me all assets from job JOB-2025-01"*
- *"How many assets have been delivered for Product PCSC187B482C_200?"*
- *"Show me the hero images for product P456."* — chains `get_asset_preview` after `query_asset` to
  return an inline image gallery rather than filenames.

## Jobs

Jobs group related products into a unit of work with a shared timeline, giving a live view of job
progress and deadline risk across your studio.

- *"List all jobs currently in progress"*
- *"Which jobs are due this week?"*
- *"Show me all jobs that are overdue"*
- *"What is the status of job JOB-2025-01?"*

## Products

Products (also called Content Requests) represent individual items that need creative content
produced.

- *"Show me all products with due dates this week"*
- *"What is the status of product PCSC187B482C_200?"*
- *"How many products are currently in progress versus done?"*
- *"Show me all products in the 'Tops' Category"*

## Tasks

Tasks are the individual steps assigned to team members or external vendors within a production —
photography, retouching, quality control, and so on.

- *"Are there any Final Selection tasks still in progress?"*
- *"Which tasks are assigned to John Doe?"*
- *"Show me all tasks that have been rejected"*
- *"How many Photo Review tasks were completed this week?"*

## Samples

Samples are the physical items sent to your studio for photography. Track where they are, which have
been returned, and which are at risk of delay.

- *"Which samples haven't been returned yet?"*
- *"Where is sample SMP-001 right now?"*
- *"How many samples are checked in at Studio A?"*
- *"Which samples are overdue for return?"*

## Planning

Planning sessions are the scheduled shoot slots that allocate studio time, photographer capacity,
and sample availability.

- *"Show me the planning sessions for next week"*
- *"Which sessions have alerts this week?"*
- *"Is John Doe scheduled to shoot on Thursday?"*
- *"Which sessions still have samples not checked in?"*
- *"Find photographers available to shoot next Tuesday."* — `query_talent_crew_schedule`
- *"Who are our short-listed models with a fitness background?"* — `query_talent_crew`

## Editorial

Editorial projects contain deliverables, where e-commerce jobs contain products — name the right
family and the assistant picks the right tools.

- *"List the editorial projects launching this quarter."*
- *"Which editorial deliverables are overdue for the March issue?"*
- *"Show the editorial productions still in progress for project Vogue-SS26."*

## Collections

Review and approval collections across all types — Photo Review, Post Review, Selection and Gallery.
See where a review has got to, what each reviewer decided, the ratings, colour flags and comments
left on individual assets, who was invited and whether they have opened it, and how the collection
was shared.

- *"Which collections are still waiting on reviewer decisions?"*
- *"Who hasn't opened the review I sent for the Liberty campaign?"*
- *"Where did reviewers disagree on this collection?"*
- *"Show me the comments and star ratings left on this gallery."*
- *"Which Post Review collections are waiting on a re-invite?"*
- *"Rank our open collections by how many assets still need a decision."*

## Short Lists

Casting short lists — the talent on each list, who liked and disliked each option by name, the
comments and @-mentions left on them, the invited reviewer roster, and full talent profiles
including job title, skills, agencies, rates and contact details.

- *"Show me the short list for the Spring campaign and who voted for whom."*
- *"Which models got the most likes on this short list?"*
- *"What feedback did the client leave on the casting options?"*
- *"Which short lists have been shared but not yet reviewed?"*

## Event Log

The event history for products, samples, workflows, style guides, editorial projects and editorial
deliverables. Ask what happened to a specific item, who acted on it, and when, and get back a
timestamped, actor-attributed timeline. Always scoped to one item — there is no studio-wide event
search.

- *"Who changed the due date on this deliverable, and when?"*
- *"Show me the check-in and check-out history for this sample."*

## Style Guides

List style guides, optionally filtered by name or product type, and retrieve the full detail of any
one of them including its rules and asset metadata.

- *"Are any Style Guides using 'Sequence Token - Letter' in their naming?"*
- *"Which Style Guides are currently marked as invalid?"*
- *"Do any of my Style Guides not have Color References enabled?"*

## Workflows

Look up your production workflows — filter by name or status, or ask about a specific workflow to
see its full structure including ordered steps, the transitions between them, and step
configuration.

- *"Which Workflows do not have a rejection bypass at Post PQ?"*
- *"How many Workflows currently use Kelvin for Final Selection?"*
- *"Do any of my Workflows still use Vendor X for External Post Production?"*

## Studio Settings

Ask about your studio's configuration without navigating to Studio Settings. Currently queryable:
Production Types, Locations, Containers, Presets, Data Sources, Team On Set Skills,
Post-Production Vendors, Product Vendors, and Print Configurations.

- *"How many Production Types exist for the 'Model' Category?"*
- *"Which Data Sources have Sync Rules, and what are they?"*
- *"Do any of my Presets specs use Color Profile 'ECI-RGB'?"*

Presets go further than the list: ask for a single preset and get its full output spec — variants,
metadata rules, and every style guide or editorial deliverable it is attached to.

- *"What does the Ecom JPG preset actually output?"*
- *"What's different between these two presets?"*
- *"Which style guides use this preset?"*
- *"Which of our presets aren't attached to anything?"*

## Managing planning sessions

> ⚠️ **These change live data.** Every change is written to Creative Force immediately and appears
> in the planning view straight away. **There is no undo** — to reverse something, ask your assistant
> to make the opposite change, or adjust it directly in Creative Force. Requires the **Write
> Planning** permission; see [permissions.md](permissions.md).

**Create a session** — appears in the planning view immediately, before any title, team, set or
products are added. A session can be empty and still valid.

- *"Create a new on-model session for Thursday's reshoot."*
- *"Set up a flat session for next Tuesday afternoon."*

**Update details** — title, description, date, time, duration, or location.

- *"Move the Thursday on-model session to Friday at 9 am, running three hours."*
- *"Rename Session SES137123CF to Spring Denim Day 2."*
- *"Set the location for tomorrow's Session to Studio B."*

**Manage the team on set** — add someone with the skill they fill, change a skill, swap one person
for another, or remove someone. Every person needs a skill; if you do not name one, your assistant
will ask. Adding someone puts them on the crew but does **not** confirm their availability or hold
status.

- *"Add Jane Doe to Thursday's session as the photographer."*
- *"Swap the photographer on the Friday session to John Smith."*
- *"Change Maria's role on this session from stylist to model."*
- *"Remove the second assistant from tomorrow's session."*

**Manage products** — attach existing product requests by filtering what is available, or remove
ones no longer being shot that day. Attaching a product does not change its status or unassign it
from anywhere else. Products must match the session's production type — a Flat work unit cannot be
added to an On-Model session, which is usually why a product does not appear as available.

- *"Attach the products from the Outerwear category that have their samples checked in to Thursday's session."*
- *"Remove the three handbags from tomorrow."*

**Delete a session** — removes it from the planning view entirely. Deleting is guarded: if the
session still has confirmed bookings or outfits attached, your assistant will warn you or stop
rather than leave those pointing at nothing.

- *"Remove the first Session booked tomorrow."*

**Not covered by session management:** Resourcing, outfits, the underlying product request or
production task (sample check-in, secondary products), and Talent &amp; Crew user properties — a
person's skills, agency, rate and personal details cannot be changed through MCP.

## Feedback

Submit feedback, report issues, or request features without leaving your assistant.

- *"Send feedback to Creative Force: [your message]."*
- *"Report an issue to Creative Force: [your message]."*
- *"Submit a feature request to Creative Force: [your message]."*
- *"Send this to Creative Force as feedback. Add my prompt and use case for them to understand."* —
  the most useful form when you hit a limitation.

Include the business use case and the specific limitation you hit.

## Getting better answers

- **Be specific and outcome-focused.** *"How are my products doing?"* ❌ →
  *"Which products produced in the last 3 months have fewer than two approved images?"* ✅
- **Put the whole question in one prompt.** If it involves a workspace, job and deadline, name all
  three: *"Which products assigned to Workspace X are missing their final delivery assets and due
  within the next 7 days?"*
- **Distinguish context from limitation.** If an answer looks incomplete, add context or split the
  question. When the server genuinely cannot do something, the assistant should say so plainly —
  that is a limitation, not a phrasing problem, and it is worth sending as feedback.

---

See the full parameter-level detail for every tool in the **[tool reference](tool-reference.md)**,
how to connect in the **[setup guide](setup.md)**, and how access is granted in
**[permissions.md](permissions.md)**.
