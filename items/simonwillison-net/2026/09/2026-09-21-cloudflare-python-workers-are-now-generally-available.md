---
title: Cloudflare Python Workers are now generally available
link: https://simonwillison.net/2026/Sep/21/cloudflare-python-worker/
source: simonwillison-net
published: 2026-09-21T22:25:44Z
updated: 2026-09-21T22:25:44Z
first_seen: 2026-09-22T00:03:48.971012939Z
labels:
- cloudflare
- pyodide
- python
- webassembly
summary: 'Cloudflare Python Workers are now generally available After a two year preview, Cloudflare''s support for running Python code in their server-side Workers platform is now stable: "Python is now a first-class, fully supported language on the Cloudflare Developer Platform". A neat thing about this is how it works. Cloudflare are running Python compiled to WebAssembly via Pyodide in their V8-based workerd runtime. This comes with some limitations, documented here - most notably both multiprocessing and threading are non-functional in the WebAssembly VM. One particularly interesting detail of this is the local development environment story - their pywrangler development tool (confusingly packaged as workers-py on PyPI) runs a full local simulation of their stack, including executing code with Pyodide in WebAssembly in V8 in a 123MB workerd binary, which for me ended up in node_modules/@cloudflare/workerd-darwin-arm64/bin/workerd. Python Workers represent a significant investment in the wider Python ecosystem by Cloudflare. The release announcement is credited to Gyeongjae Choi, Dominik Picheta, and Hood Chatham - Gyeongjae and Hood are both Pyodide core maintainers. Via Hacker News Tags: python, cloudflare, webassembly, pyodide'
content: extracted
html: 2026-09-21-cloudflare-python-workers-are-now-generally-available.html
---

**[Cloudflare Python Workers are now generally available](https://blog.cloudflare.com/python-workers-ga/)** ([via](https://news.ycombinator.com/item?id=49787142 "Hacker News")) After a two year preview, Cloudflare's support for running Python code in their server-side Workers platform is now stable: "Python is now a first-class, fully supported language on the Cloudflare Developer Platform".

A neat thing about this is how it works. Cloudflare are running Python compiled to WebAssembly via Pyodide in their V8-based [workerd](https://github.com/cloudflare/workerd) runtime.

This comes with some limitations, [documented here](https://developers.cloudflare.com/workers/languages/python/stdlib/) - most notably both `multiprocessing` and `threading` are non-functional in the WebAssembly VM.

One particularly interesting detail of this is the local development environment story - their [pywrangler](https://developers.cloudflare.com/workers/languages/python/#the-pywrangler-cli-tool) development tool (confusingly packaged as [workers-py](https://pypi.org/project/workers-py/) on PyPI) runs a full local simulation of their stack, including executing code with Pyodide in WebAssembly in V8 in a 123MB `workerd` binary, which for me ended up in `node_modules/@cloudflare/workerd-darwin-arm64/bin/workerd`.

Python Workers represent a significant investment in the wider Python ecosystem by Cloudflare. The release announcement is credited to Gyeongjae Choi, Dominik Picheta, and Hood Chatham - Gyeongjae and Hood are both Pyodide core maintainers.
