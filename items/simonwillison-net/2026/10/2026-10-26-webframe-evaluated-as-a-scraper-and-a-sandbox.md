---
title: 'webframe: evaluated as a scraper and a sandbox'
link: https://simonwillison.net/2026/Oct/26/webframe-evaluation/
source: simonwillison-net
published: 2026-10-26T16:15:00Z
updated: 2026-10-26T16:15:00Z
first_seen: 2026-10-27T04:50:20.607730595Z
labels:
- browsers
- go
- javascript
- jeff-lindsay
- shot-scraper
summary: 'Research: webframe: evaluated as a scraper and a sandbox webframe is a neat little project from Jeff Lindsay which lets you "run a web page like a program" - you point it at a URL or file and it executes it in a hidden headless web view and outputs console.log() to standard output and any exceptions to standard error. It''s written in Go on top of the glaze library, which provides access to a system browser ("WKWebView on macOS, WebKitGTK on Linux, WebView2 on Windows") so it doesn''t need to bundle its own browser engine. It reminds me of my own shot-scraper javascript tool, albeit in a much smaller package that doesn''t depend on Playwright. I had Claude Opus 5.5 put it through its paces and prepare a research report for me. Here''s the prompt I used: Explore https://github.com/tractordev/webframe try running it and put it though its paces, build a few exploratory prototypes, then evaluate it as a potential sandboxing tool and as a tool for scraping web pages using the same patterns encouraged by the "shot-scraper JavaScript" command (see simonw/shot-scraper on GitHub, docs directory) Tags: browsers, go, javascript, jeff-lindsay, shot-scraper'
content: extracted
html: 2026-10-26-webframe-evaluated-as-a-scraper-and-a-sandbox.html
---

[webframe: evaluated as a scraper and a sandbox](https://github.com/simonw/research/tree/main/webframe-evaluation#readme) — An evaluation of webframe found it to be a fast, lightweight browser-JavaScript runner suitable for scraping, with performance comparable to shot-scraper, but with important rough edges including CSP failures, a 0×0 viewport, hanging loads, missing HTTP metadata, and limited browser controls.

[webframe](https://github.com/tractordev/webframe) is a neat little project from Jeff Lindsay which lets you "run a web page like a program" - you point it at a URL or file and it executes it in a hidden headless web view and outputs `console.log()` to standard output and any exceptions to standard error.

It's written in Go on top of the [glaze](https://github.com/crgimenes/glaze) library, which provides access to a system browser ("WKWebView on macOS, WebKitGTK on Linux, WebView2 on Windows") so it doesn't need to bundle its own browser engine.

It reminds me of my own [shot-scraper javascript](https://shot-scraper.datasette.io/en/stable/javascript.html) tool, albeit in a much smaller package that doesn't depend on Playwright.

I had Claude Opus 5.5 [put it through its paces](https://claude.ai/code/cshk_01MX3aJH8vDGmGSj1xH9BYhj2HviVvLDaJbiELHyDoReYT) and prepare a research report for me. Here's the prompt I used:

> `Explore https://github.com/tractordev/webframe try running it and put it though its paces, build a few exploratory prototypes, then evaluate it as a potential sandboxing tool and as a tool for scraping web pages using the same patterns encouraged by the "shot-scraper JavaScript" command (see simonw/shot-scraper on GitHub, docs directory)`
