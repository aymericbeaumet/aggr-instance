---
title: Tool-Level Permissions
link: https://ampcode.com/news/tool-level-permissions
source: ampcode-com
published: 2025-08-07T00:00:00Z
updated: 2025-08-07T00:00:00Z
first_seen: 2026-10-04T12:50:30.804479567Z
summary: 'Instead of only allowing you to configure which Bash commands are allowed to run and which aren''t, Amp now gives you the ability to define which tool it''s allowed to run, and with which arguments. When Amp wants to call a tool, it first checks your list of permissions to find an entry that matches the tool and the tool call arguments. The matched entry then tells Amp to either allow the tool call, reject it, ask you for confirmation, or delegate the decision to another program. If no matching entry is found, Amp checks the built-in permission list that contains sensible defaults. You can define these granular tool-level permissions by adding entries to the amp.permissions list in your configuration file. Here''s an example: "amp.permissions": [ // Ask before running a command containing "git commit" { "tool": "Bash", "matches": { "cmd": "*git commit*" }, "action": "ask"}, // Reject command containing "python " or "python3 " { "tool": "Bash", "matches": { "cmd": ["*python *", "*python3 *"] }, "action": "reject"}, // Ask before running any MCP tool { "tool": "mcp__*", "action": "ask"}, // Delegate everything else to a permission helper (must be on $PATH) { "tool": "*", "action": "delegate", "to": "my-permission-helper"} ] You can read more about the new tool-level permissions in the manual.'
content: extracted
html: 2025-08-07-tool-level-permissions.html
---

Instead of only allowing you to configure which Bash commands are allowed to run and which aren't, Amp now gives you the ability to define which tool it's allowed to run, and with which arguments.

When Amp wants to call a tool, it first checks your list of permissions to find an entry that matches the tool and the tool call arguments. The matched entry then tells Amp to either *allow* the tool call, *reject* it, *ask* you for confirmation, or *delegate* the decision to another program. If no matching entry is found, Amp checks the built-in permission list that contains sensible defaults.

You can define these granular tool-level permissions by adding entries to the `amp.permissions` list in your [configuration file](https://ampcode.com/manual#permissions). Here's an example:

```json
"amp.permissions": [
  // Ask before running a command containing "git commit"
  { "tool": "Bash", "matches": { "cmd": "*git commit*" }, "action": "ask"},
  // Reject command containing "python " or "python3 "
  { "tool": "Bash", "matches": { "cmd": ["*python *", "*python3 *"] }, "action": "reject"},
  // Ask before running any MCP tool
  { "tool": "mcp__*", "action": "ask"},
  // Delegate everything else to a permission helper (must be on $PATH)
  { "tool": "*", "action": "delegate", "to": "my-permission-helper"}
]
```

You can read more about the new tool-level permissions [in the manual](https://ampcode.com/manual#permissions).
