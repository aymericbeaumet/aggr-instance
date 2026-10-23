---
title: 'Remove pgrust: the entry is cheating'
link: https://github.com/ClickHouse/ClickBench/pull/2165
source: hnrss-org
published: 2026-10-23T08:05:33Z
updated: 2026-10-23T08:05:33Z
first_seen: 2026-10-23T14:22:19.033652984Z
authors:
- f311a
content: extracted
html: 2026-10-23-remove-pgrust-the-entry-is-cheating.html
remote_preview:
  url: https://opengraph.githubassets.com/4870be30cb291ef6f3ef20ef4fd4192b92a854ec1ef7d9bed8b58b200e3d9a47/ClickHouse/ClickBench/pull/2165
  alt: 'This removes the pgrust entry (added in #1163, updated to v0.4-preview in #2106) from the benchmark. The results it reports are produced by machinery written specifically for the 43 ClickBench quer...'
---

[![@alexey-milovidov](https://avatars.githubusercontent.com/u/18581488?s=40&v=4)](https://github.com/alexey-milovidov)

`Remove pgrust: the entry is tuned for the benchmark queries and cache…`

`d807078`

```
…s their results

The v0.4-preview binary embeds one hand-written kernel per ClickBench query
(crates/gravity/backend/irprobe/src/q00.rs ... q36.rs), plan matchers named
after the benchmark queries, and a cross-query cache of filter verdicts that
makes the hot runs replay the answer of the previous run without touching data.
Trivially perturbed queries such as SELECT MIN(URL) FROM hits are refused with
an error. See the pull request description for the details.
```
