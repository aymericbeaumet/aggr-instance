---
title: Using Parseable with Datasette for OpenTelemetry traces
link: https://simonwillison.net/2026/Oct/6/datasette-parseable-opentelemetry/
source: simonwillison-net
published: 2026-10-06T19:07:31Z
updated: 2026-10-06T19:07:31Z
first_seen: 2026-10-06T19:48:54.930461426Z
labels:
- alex-garcia
- datasette
- observability
- opentelemetry
summary: 'TIL: Using Parseable with Datasette for OpenTelemetry traces I saw Parseable in a Show HN today - it''s a new observability platform with both an open source (AGPL) Rust implementation (a single ~180MB binary), an "Enterprise" version with extra features and a cloud hosted option. Since Datasette 1.0a41 added OpenTelemetry support (thanks, Alex Garcia), I decided to fire up Codex and have it figure out how to run Parseable and feed it traces from Datasette. Here''s my (human-written) TIL showing the patterns that worked, and here''s a screenshot of a Datasette trace displayed within the Parseable localhost web application: Tags: datasette, observability, alex-garcia, opentelemetry'
content: extracted
html: 2026-10-06-using-parseable-with-datasette-for-opentelemetry-traces.html
remote_preview:
  url: https://raw.githubusercontent.com/simonw/til/refs/heads/main/datasette/datasette-parsable.webp
  alt: 'Screenshot of a trace detail view in an observability web app, with a span waterfall overlaid on a dimmed navigation sidebar and filter column. Dimmed sidebar: breadcrumb "Community > Traces > datas" (cut off), search box "Search... ⌘K", nav items "Home", "Ingest telemetry", section "ANALYZE": "Keys'
---

[Using Parseable with Datasette for OpenTelemetry traces](https://til.simonwillison.net/datasette/datasette-parseable-opentelemetry) — \[Parseable\](https://www.parseable.com) is a brand new observability tool, compatible with \[OpenTelemetry\](https://opentelemetry.io), for storing and querying observability data.

I [saw Parseable in a Show HN](https://news.ycombinator.com/item?id=49978171) today - it's a new observability platform with both an [open source (AGPL)](https://github.com/parseablehq/parseable) Rust implementation (a single ~180MB binary), an "Enterprise" version with extra features and a cloud hosted option.

Since [Datasette 1.0a41 added OpenTelemetry support](https://docs.datasette.io/en/latest/changelog.html#a41-2026-09-24) (thanks, Alex Garcia), I decided to fire up Codex and have it figure out how to run Parseable and feed it traces from Datasette.

Here's my (human-written) TIL showing the patterns that worked, and here's a screenshot of a Datasette trace displayed within the Parseable localhost web application:

![Screenshot of a trace detail view in an observability web app, with a span waterfall overlaid on a dimmed navigation sidebar and filter column. Dimmed sidebar: breadcrumb \"Community > Traces > datas\" (cut off), search box \"Search... ⌘K\", nav items \"Home\", \"Ingest telemetry\", section \"ANALYZE\": \"Keystone\", \"Dashboards\", \"SQL Editor\", section \"OBSERVE\": \"Logs\", \"Metrics\", \"Traces\" (selected), \"APM\", \"Agents\", section \"MONITOR\": \"Alerts\", \"Errors\", section \"DATA\": \"Datasets\", and at the bottom \"Settings\", \"Book a call\", \"Support\". Dimmed filter column, cut off at the right edge: \"Search fi\", \"Core\", \"Log format\", \"User agent\", \"Source IPs\", \"Error\", \"Service\", \"service.ins\", \"service.na\", \"datasett\" (checked), \"Span\", \"Database\", \"HTTP\", \"http.reque\", \"NULL\" (unchecked), \"GET\" (checked), \"http.respo\", \"http.route\", \"Server\", \"server.add\", \"Telemetry\", \"URL\", \"All fields\". Trace panel header: \"Trace detail > 6f819a2170bcd1e91c6ea3ae236ec60b\" with a copy icon, a \"Related logs\" button and a close X. Summary: \"Start time 6:56 PM, Oct 6, 2026 UTC\", \"Duration 40.9 ms\", \"Spans 247\". A minimap with axis \"0ns 10.2ms 20.5ms 30.7ms 40.9ms\" shows many short span bars cascading diagonally from top left toward the lower middle, with a few longer bars. Below is a span table with a \"Span name\" header, a \"Search spans...\" box, collapse and expand buttons, and a timeline axis \"0ns 10.2ms 20.5ms 30.7ms 40.9ms\". Rows (name, service, duration): root span with collapse toggle \"123\", \"GET /...\" \"datasette...\" 40.9ms spanning the full timeline; then alternating rows where each \"db.query\" has a collapse toggle \"1\": db.query datasette-local 679µs, db.query.execute datasette-local 278µs, db.query datasette-local 341µs, db.query.execute datasette-local 55µs, db.query datasette-local 357µs, db.query.execute datasette-local 197µs, db.query datasette-local 330µs, db.query.execute datasette-local 73µs, db.query datasette-local 2.26ms, db.query.execute datasette-local 2.02ms, db.query datasette-local 232µs, db.query.execute datasette-local 64µs, db.query datasette-local 207µs, db.query.execute datasette-local 71µs, db.query datasette-local 6.11ms. The child span bars start progressively later across the early part of the timeline.](https://raw.githubusercontent.com/simonw/til/refs/heads/main/datasette/datasette-parsable.webp)
