# Security policy

## Reporting a vulnerability

If you discover a security or privacy vulnerability in the Creative Force MCP server or its
documentation, please report it privately. **Do not open a public GitHub issue.**

- Use GitHub's **private vulnerability reporting** on this repository (the **Security** tab →
  *Report a vulnerability*), or contact Creative Force via <https://creativeforce.io>.
- Please include a description, reproduction steps, and the potential impact.

We will acknowledge your report, investigate, and keep you informed of the resolution.

## Security posture

The Creative Force MCP server is designed to minimise risk:

- **Read-first.** 38 of the 42 tools are queries that cannot change your data. Writes are confined
  to a single area — planning sessions (`create_planning_session`, `update_planning_session`,
  `delete_planning_session`) — plus `send_feedback`, which sends a message to the Creative Force
  team and writes nothing to your studio's data. No tool can modify jobs, products, samples,
  assets, tasks, editorial records, collections, short lists or studio settings.
- **Writes are opt-in and separately gated.** Write tools sit in their own **Write/Delete**
  permission group on the role's MCP Server tab, behind a **Write Planning** permission that is
  distinct from *Query Planning*, off by default for every role, and additionally requiring the
  underlying Creative Force permission at Edit level. A studio that grants only the Read-Only group
  has a strictly read-only server.
- **Destructive operations are annotated and guarded.** `delete_planning_session` carries
  `destructiveHint`, so MCP clients prompt the user for confirmation before it runs, and the server
  refuses or warns when the session still has confirmed bookings or outfits attached.
- **Writes are immediate and have no undo.** Changes land in Creative Force straight away. Reversing
  one means making the opposite change. Treat *Write Planning* as you would edit rights in the web
  app.
- **Studio-scoped.** A session can only ever see data for the authenticated user's studio.
  Access is resolved per request from the user's own Creative Force identity.
- **Permission-filtered.** Tools are filtered by the user's Creative Force screen permissions —
  a user only sees the tools for areas they are already allowed to access.
- **No shared credentials.** Connections authenticate with the user's own Creative Force account
  using the OAuth 2.0 authorization code flow with PKCE. There are no shared or long-lived API
  keys to leak.

## A note for agent operators (prompt injection)

Because the server returns your studio's content to an AI assistant, treat tool results as
untrusted input to the model, exactly as you would any external data. Text retrieved from
production records (names, notes, descriptions) could attempt to influence the assistant. Keep a
human in the loop for any consequential action the assistant suggests based on retrieved data.

This matters more now that the server has write tools. If your studio grants the **Write/Delete**
group, retrieved text is reaching a model that can create, change and delete planning sessions.
Keep the client's own confirmation prompts enabled, and grant the Write/Delete group only to roles
that would be entitled to make those changes in the web app.

See also **[privacy.md](privacy.md)** for data-handling details.
