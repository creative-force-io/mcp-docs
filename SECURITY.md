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

- **Read-only.** Every tool is a query. The server does not create, update, or delete your
  production data. The single exception is `send_feedback`, which sends a message to the
  Creative Force team and writes nothing to your studio's data.
- **Studio-scoped.** A session can only ever see data for the authenticated user's studio.
  Access is resolved per request from the user's own Creative Force identity.
- **Permission-filtered.** Tools are filtered by the user's Creative Force screen permissions —
  a user only sees the tools for areas they are already allowed to access.
- **OAuth 2.1 authentication.** Connections authenticate with the user's Creative Force account
  using OAuth 2.1 with PKCE. There are no shared or long-lived API keys to leak.
- **Subscription-gated.** The server is only available to studios whose subscription includes the
  MCP Server feature.

## A note for agent operators (prompt injection)

Because the server returns your studio's content to an AI assistant, treat tool results as
untrusted input to the model, exactly as you would any external data. Text retrieved from
production records (names, notes, descriptions) could attempt to influence the assistant. Keep a
human in the loop for any consequential action the assistant suggests based on retrieved data.

See also **[privacy.md](privacy.md)** for data-handling details.
