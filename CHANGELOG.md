# Changelog

All notable changes to the Creative Force MCP documentation are recorded here. The format is
based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and the documentation follows
[Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

Initial public documentation for the Creative Force MCP server. To be tagged **v1.0.0** at the
public release.

### Changed
- **Documentation re-synced with the live server (34 tools).** The generated
  `tool-reference.md` and `tools.json` were already current; every hand-written page still
  described a 16-tool, read-only server. Corrected across the README, security policy, privacy
  page, permissions page, use-case guide and registry manifest:
  - Tool count 16 → **34** (30 read-only, 4 write). The README tool badge now reads from
    `tools.json` via the shields endpoint API, so it cannot drift again.
  - "Read-only server" → **read-first**: writes exist but are confined to planning sessions
    (`create_planning_session`, `update_planning_session`, `delete_planning_session`), sit in the
    **Write/Delete** permission group that is off by default, and require the underlying Creative
    Force permission at Edit level. `delete_planning_session` is annotated destructive.
  - "OAuth 2.1" → **OAuth 2.0 authorization code flow with PKCE**, which is what the authorization
    server actually implements. See the note in the setup guide's OAuth reference.
  - Documented the areas added since the last pass: style guides, workflow configuration, the event
    log, and studio settings (locations, presets, print configurations, production types, product
    and post-production vendors, containers, data sources, on-set skills).
- **Setup guide rewritten** to match the published Help Centre articles, adding the three clients it
  did not cover: **ChatGPT** (Developer Mode custom connector), **Microsoft Copilot** (the full
  Copilot Studio agent flow, including the recommended agent instructions and store listing text),
  and **Notion** (custom MCP server inside Notion AI). The Claude section is now split into admin
  and user steps, the role-permission path is corrected to **Settings → User Roles**, and the
  troubleshooting table absorbs the Help Centre's rows.
- **Permissions page completed from the Help Centre.** The tool table covered 15 of 34 tools and
  described the Write/Delete group as empty. It now maps every tool to its MCP SERVER permission and
  its Creative Force screen permission, sourced from the Data Queries and Manage Planning Sessions
  articles. Corrections that came out of that pass:
  - `send_feedback` has a **Send Feedback** permission on the MCP SERVER tab. The page said it
    needed none and was not shown there.
  - `query_task` maps to **Task Management** (Photography / Internal Post / Digital Processing
    Management), not E-Comm → Production.
  - `query_planning` needs Planning → Calendar, **Set** and **Talent & Crew**, not Calendar alone.
  - Writes are gated by a single **Write Planning** permission, separate from *Query Planning* — a
    role can query sessions without being able to change them.
  - New section on gating that is not a role permission: the **Rates** screen permission on
    `query_talent_crew` (`ratesAccess: denied` means hidden, not absent) and the **Advanced Style
    Guides** / **Localization** plan features that gate tabs in `get_styleguide_detail`.
- **Use-case guide restructured** around the same query categories permissions are granted in, using
  the Help Centre's own example prompts, plus a section on managing planning sessions (no undo,
  guarded delete, what session management does not cover) and the prompt-writing guidance from the
  Overview article.
- **Write semantics documented** in the security and privacy pages: changes are immediate with no
  undo, deletion is guarded against sessions with confirmed bookings or outfits, and session
  management cannot touch Resourcing, outfits, the underlying product request, or Talent & Crew user
  properties.
- **Removed the two remaining "subscription-gated" statements** in `SECURITY.md` and `privacy.md`.
  Dropping the add-on prerequisite was supposed to remove four such statements; the README and setup
  guide were fixed at the time, these two were missed.
- Connecting no longer requires the **MCP Server** subscription add-on. Access is decided only by
  the MCP tool permissions granted on a role, so the prerequisite, the "connection denied"
  troubleshooting row, and the note about a missing **MCP Server** tab have been removed from the
  setup guide, along with the subscription sentence in the README.

### Added
- README, setup guide, and support page.
- `SECURITY.md` responsible-disclosure policy and security posture.
- `server.json` Model Context Protocol registry manifest.
- Tool reference covering all tools, generated from the server source
  (published via the documentation generator).
- Use-case and prompt guide, and the privacy and data-handling page.
- Permissions page explaining how MCP tool access is granted per user role (the two
  permissions, the MCP SERVER tab groups and options, and the tool → data-area map).
