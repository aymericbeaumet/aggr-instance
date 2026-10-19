---
title: 'Profiling web-edit with Python 3.15: flame graphs and source heatmaps'
link: https://simonwillison.net/2026/Oct/12/python-3-15-web-edit-profiling/
source: simonwillison-net
published: 2026-10-12T16:58:00Z
updated: 2026-10-12T16:58:00Z
first_seen: 2026-10-13T23:05:17.612288367Z
labels:
- codex
- profiling
- python
summary: 'Research: Profiling web-edit with Python 3.15: flame graphs and source heatmaps I put the new Python 3.15 profiler through its paces by having GPT-6 Astra Extra High in Codex run it against my new web-edit app: I need a Python 3.15 flame graph and heatmap demo of the "web-edit" app from PyPI - research first then make me those demos Here''s the resulting example flame graph and heatmap. Tags: profiling, python, codex'
content: extracted
html: 2026-10-12-profiling-web-edit-with-python-3-15-flame-graphs-and-source.html
---

[Profiling web-edit with Python 3.15: flame graphs and source heatmaps](https://github.com/simonw/research/tree/main/python-3-15-web-edit-profiling#readme) — Profiling the unmodified web-edit app with Python 3.15’s built-in \`profiling.sampling\` revealed that mixed line-ending preservation dominated a busy editing workload: \`preserve\_endings\` appeared in 64.8% of 15,296 recorded CPU stacks, with \`SequenceMatcher.find\_longest\_match\` contributing 38.3% of leaf samples. Sampling all threads was essential, since 95.1% of stacks came from AnyIO’s worker thread and would have been missed by a main-thread-only profile.

I put the [new Python 3.15 profiler](https://simonwillison.net/2026/Oct/12/python-3-15/#a-sampling-profiler) through its paces by having GPT-6 Astra Extra High in Codex run it against my new [web-edit app](https://simonwillison.net/2026/Oct/12/web-edit/):

> `I need a Python 3.15 flame graph and heatmap demo of the "web-edit" app from PyPI - research first then make me those demos`

Here's the resulting example [flame graph](https://simonw.github.io/research/python-3-15-web-edit-profiling/artifacts/flamegraph.html) and [heatmap](https://simonw.github.io/research/python-3-15-web-edit-profiling/artifacts/heatmap/index.html).
