---
title: datasette-apps 0.4a0
link: https://simonwillison.net/2026/Nov/15/datasette-apps/
source: simonwillison-net
published: 2026-11-15T04:02:45Z
updated: 2026-11-15T04:02:45Z
first_seen: 2026-11-16T07:11:22.159097445Z
labels:
- datasette
- datasette-agent
- datasette-apps
- urls
summary: 'Release: datasette-apps 0.4a0 Datasette Apps are sandboxed HTML and JavaScript applications that live inside Datasette - think Claude Artifacts, but these ones can run SQL queries against your data. 0.4a0 adds support for linking: you can link to an app with #fragments in the URL and the app can see and use those, plus supported URL schemes are reported back to Datasette Agent so it can help write apps that link to each other. This means you can have e.g. an app that shows details about a specific record, and a completely separate app that searches those records and links to their representations in the first app. Tags: datasette-agent, datasette-apps, datasette, urls'
content: extracted
html: 2026-11-15-datasette-apps-0-4a0.html
---

Datasette Apps are sandboxed HTML and JavaScript applications that live inside Datasette - think Claude Artifacts, but these ones can run SQL queries against your data.

0.4a0 adds support for linking: you can link to an app with `#fragments` in the URL and the app can see and use those, plus supported URL schemes are reported back to Datasette Agent so it can help write apps that link to each other.

This means you can have e.g. an app that shows details about a specific record, and a completely separate app that searches those records and links to their representations in the first app.
