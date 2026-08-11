# Changelog

All notable changes to the Creative Force MCP documentation are recorded here. The format is
based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and the documentation follows
[Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

Initial public documentation for the Creative Force MCP server. To be tagged **v1.0.0** at the
public release.

### Changed
- Connecting no longer requires the **MCP Server** subscription add-on. Access is decided only by
  the MCP tool permissions granted on a role, so the prerequisite, the "connection denied"
  troubleshooting row, and the note about a missing **MCP Server** tab have been removed from the
  setup guide, along with the subscription sentence in the README.

### Added
- README, setup guide, and support page.
- `SECURITY.md` responsible-disclosure policy and security posture.
- `server.json` Model Context Protocol registry manifest.
- Tool reference covering all **16** tools, generated from the server source
  (published via the documentation generator).
- Use-case and prompt guide, and the privacy and data-handling page.
- Permissions page explaining how MCP tool access is granted per user role (the two
  permissions, the MCP SERVER tab groups and options, and the tool → data-area map).
