---
title: datasette-mcp 0.2
link: https://simonwillison.net/2026/Sep/1/datasette-mcp/
source: simonwillison-net
published: 2026-09-01T15:30:12Z
updated: 2026-09-01T15:30:12Z
first_seen: 2026-09-04T12:14:41.802665207Z
labels:
- datasette
- model-context-protocol
summary: 'Release: datasette-mcp 0.2 "rows" from execute_sql is now an array of objects. Previously it was an array of arrays. This should help weaker models avoid losing track of which positional array element maps to which column. #1 Now depends on mcp>=2.1.1. This is the first non-alpha release of the plugin. I''m confident it''s ready as I''ve been using it quite a bit myself. Tags: datasette, model-context-protocol'
content: extracted
html: 2026-09-01-datasette-mcp-0-2.html
---

> - `"rows"` from `execute_sql` is now an array of objects. Previously it was an array of arrays. This should help weaker models avoid losing track of which positional array element maps to which column. [#1](https://github.com/datasette/datasette-mcp/issues/1)
> - Now depends on `mcp>=2.1.1`.

This is the first non-alpha release of the plugin. I'm confident it's ready as I've been using it quite a bit myself.
