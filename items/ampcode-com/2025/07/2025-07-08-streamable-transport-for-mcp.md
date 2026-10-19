---
title: Streamable Transport for MCP
link: https://ampcode.com/news/streamable-mcp
source: ampcode-com
published: 2025-07-08T00:00:00Z
updated: 2025-07-08T00:00:00Z
first_seen: 2026-10-04T12:50:30.804479567Z
summary: Amp now uses streamable HTTP transport for MCP servers by default with a fallback to Server-Sent Events. That allows you to connect to your own MCP servers hosted on Cloudflare or use other remote servers such as https://learn.microsoft.com/api/mcp.
content: extracted
html: 2025-07-08-streamable-transport-for-mcp.html
remote_preview:
  url: https://static.ampcode.com/news/streamable-mcp.png
  alt: Streamable HTTP transport in action
---

Amp now uses [streamable HTTP transport](https://modelcontextprotocol.io/specification/2025-06-18/basic/transports#streamable-http) for MCP servers by default with a fallback to Server-Sent Events.

That allows you to connect to your own MCP servers hosted on [Cloudflare](https://blog.cloudflare.com/streamable-http-mcp-servers-python/) or use other remote servers such as `https://learn.microsoft.com/api/mcp`.

![Streamable HTTP transport in action](https://static.ampcode.com/news/streamable-mcp.png)
