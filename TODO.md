# TODO

## Future Data Sources

### Google Drive

The official Anthropic MCP server (`@modelcontextprotocol/server-gdrive`) was archived May 2025 and is no longer maintained. Community alternatives exist but none are production-stable:

- [felores/gdrive-mcp-server](https://github.com/felores/gdrive-mcp-server) — converts Docs to Markdown, Sheets to CSV
- [piotr-agier/google-drive-mcp](https://github.com/piotr-agier/google-drive-mcp) — covers Drive, Docs, Sheets, Slides, Calendar
- [isaacphi/mcp-gdrive](https://github.com/isaacphi/mcp-gdrive) — read files, search, read/write Sheets

**Next step:** Vet community servers, pick the most maintained one, and add setup docs.

### Sybill

No MCP server exists (official or community). Sybill has webhook automations (push-based via Svix) and Zapier/Make integrations, but no public REST API suitable for wrapping in an MCP server.

**Next step:** Request API access from Sybill, or build a bridge MCP server that ingests webhook payloads into a queryable local store.
