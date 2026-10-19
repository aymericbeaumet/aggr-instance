---
title: MCP Permissions
link: https://ampcode.com/news/mcp-permissions
source: ampcode-com
published: 2025-08-25T00:00:00Z
updated: 2025-08-25T00:00:00Z
first_seen: 2026-10-04T12:50:30.804479567Z
summary: 'The setting amp.mcpPermissions defines rules that block or allow MCP servers. MCP permissions are evaluated using a rule-based system with the same pattern matching syntax that is used for tool permissions. The first matching rule determines the action. If no rules match, the MCP server is allowed by default. The following configuration would block all MCP servers except locally-executed servers from the @modelcontextprotocol npm organization and remote servers from trusted-service.com: { "amp.mcpPermissions": [ { "matches": { "command": "npx @modelcontextprotocol/server-*" }, "action": "allow" }, { "matches": { "url": "https://trusted-service.com/mcp/*" }, "action": "allow" }, { "matches": { "url": "*" }, "action": "reject" } { "matches": { "command": "*" }, "action": "reject" } ] } Read more about amp.mcpPermissions in the manual.'
content: extracted
html: 2025-08-25-mcp-permissions.html
---

The setting `amp.mcpPermissions` defines rules that block or allow MCP servers.

MCP permissions are evaluated using a rule-based system with the same pattern matching syntax that is used for [tool permissions](https://ampcode.com/manual#permissions). The first matching rule determines the action. If no rules match, the MCP server is allowed by default.

The following configuration would block all MCP servers except locally-executed servers from the `@modelcontextprotocol` npm organization and remote servers from `trusted-service.com`:

```json
{
	"amp.mcpPermissions": [
		{
			"matches": { "command": "npx @modelcontextprotocol/server-*" },
			"action": "allow"
		},
		{
			"matches": { "url": "https://trusted-service.com/mcp/*" },
			"action": "allow"
		},
		{
			"matches": { "url": "*" },
			"action": "reject"
		}
		{
			"matches": { "command": "*" },
			"action": "reject"
		}
	]
}
```

Read more about `amp.mcpPermissions` in [the manual.](https://ampcode.com/manual?internal#core-settings)
