---
title: datasette-arrows 0.1a0
link: https://simonwillison.net/2026/Nov/18/datasette-arrows/
source: simonwillison-net
published: 2026-11-18T17:09:59Z
updated: 2026-11-18T17:09:59Z
first_seen: 2026-11-19T07:50:29.493483202Z
labels:
- datasette
- projects
summary: 'Release: datasette-arrows 0.1a0 Datasette 1.0a43 added the ability for plugins to implement streaming export formats. Datasette core supports streaming CSV and TSV - you can export millions of lines of data from a table without needing to load it all into memory, because those two file formats support streaming data in chunks. The register_output_renderer() plugin hook can now be used to implement the same pattern for other formats. I''ve learned never to release a new plugin feature without accompanying it with a plugin that uses it. Claude Fable 5.2 suggested the Apache Arrow streaming format as a good option, and built this plugin in almost a single shot. Add the plugin to Datasette (datasette install datasette-arrows) and the export dialog gains this new Arrow option: This screenshot is from Datasette''s main branch - the most recent release doesn''t yet group streaming formats such as Arrow with the TSV and CSV in the "All rows in this table" section, but that''s coming in the next alpha release. Tags: datasette, projects'
content: extracted
html: 2026-11-18-datasette-arrows-0-1a0.html
remote_preview:
  url: https://static.simonwillison.net/static/2026/arrow-export-dialog.webp
  alt: Export data modal for the threads table. The TSV and CSV exports are now joined by Arrow, showing "All 927 rows" and the text Apache Arrow IPC stream, for DuckDB, pandas, Polars and other tools. There is a Download button and a Copy URL button.
---

[Datasette 1.0a43](https://docs.datasette.io/en/latest/changelog.html#a43-2026-11-15) added the ability for plugins to implement streaming export formats. Datasette core supports streaming CSV and TSV - you can export millions of lines of data from a table without needing to load it all into memory, because those two file formats support streaming data in chunks. The [register\_output\_renderer()](https://docs.datasette.io/en/latest/plugin_hooks.html#register-output-renderer-datasette) plugin hook can now be used to implement the same pattern for other formats.

I've learned never to release a new plugin feature without accompanying it with a plugin that uses it.

Claude Fable 5.2 suggested the [Apache Arrow streaming](https://arrow.apache.org/docs/python/ipc.html) format as a good option, and built this plugin in almost a single shot.

Add the plugin to Datasette (`datasette install datasette-arrows`) and the export dialog gains this new Arrow option:

![Export data modal for the threads table. The TSV and CSV exports are now joined by Arrow, showing \"All 927 rows\" and the text Apache Arrow IPC stream, for DuckDB, pandas, Polars and other tools. There is a Download button and a Copy URL button.](https://static.simonwillison.net/static/2026/arrow-export-dialog.webp)

This screenshot is from Datasette's `main` branch - the most recent release doesn't yet group streaming formats such as Arrow with the TSV and CSV in the "All rows in this table" section, but that's coming in the next alpha release.
