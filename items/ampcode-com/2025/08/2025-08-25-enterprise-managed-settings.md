---
title: Enterprise Managed Settings
link: https://ampcode.com/news/enterprise-managed-settings
source: ampcode-com
published: 2025-08-25T00:00:00Z
updated: 2025-08-25T00:00:00Z
first_seen: 2026-10-04T12:50:30.804479567Z
summary: 'Amp now allows system administrators to configure organization-wide settings that override individual settings for Enterprise Amp customers. These settings can help ensure security, standardization, and best practices across an organization. Managed settings can be used, for example, to do the following: Set MCP servers to those that work well in your organization (amp.mcpServers) Allow or block MCP servers (amp.mcpPermissions) Allow or block Bash commands that match specified patterns (amp.permissions) Override any other Amp setting To use managed settings, system administrators should ensure the settings are defined in the following files: macOS: /Library/Application Support/ampcode/managed-settings.json Linux: /etc/ampcode/managed-settings.json Windows: %ProgramData%\ampcode\managed-settings.json Settings in these files use the same schema as individual settings. You can read more about managed settings in the manual.'
content: extracted
html: 2025-08-25-enterprise-managed-settings.html
---

Amp now allows system administrators to configure organization-wide settings that override individual settings for Enterprise Amp customers. These settings can help ensure security, standardization, and best practices across an organization. Managed settings can be used, for example, to do the following:

- Set MCP servers to those that work well in your organization (`amp.mcpServers`)
- Allow or block MCP servers (`amp.mcpPermissions`)
- Allow or block Bash commands that match specified patterns (`amp.permissions`)
- Override any other Amp setting

To use managed settings, system administrators should ensure the settings are defined in the following files:

- **macOS**: `/Library/Application Support/ampcode/managed-settings.json`
- **Linux**: `/etc/ampcode/managed-settings.json`
- **Windows**: `%ProgramData%\ampcode\managed-settings.json`

Settings in these files use the same schema as individual settings.

You can read more about managed settings [in the manual.](https://ampcode.com/manual?internal#enterprise-managed-policy-settings)
