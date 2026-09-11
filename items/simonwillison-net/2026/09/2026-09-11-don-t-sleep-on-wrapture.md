---
title: Don't sleep on wrapture
link: https://simonwillison.net/2026/Sep/11/wrapture/
source: simonwillison-net
published: 2026-09-11T13:51:32Z
updated: 2026-09-11T13:51:32Z
first_seen: 2026-09-11T17:43:44.917817170Z
labels:
- graham-dumpleton
- monkey-patching
- observability
- open-source
- python
- testing
summary: 'Graham Dumpleton''s new monkey patching package wrapture is shaping up to be an indispensable tool for Python developers. I''m not sure why I''ve seen so little buzz about it! Graham has been posting new tutorials for it almost daily since the initial release on August 31st. Here''s everything he''s published so far: Introducing wrapture - a new monkey patching library that serves both testing and observability (think New Relic style tracing) at the same time. Unit testing with wrapture - how to use it for the same kinds of thing as unittest.mock. Recording calls with wrapture - recording method calls as timelines and processing and displaying them as trees. Phased behaviour in wrapture - arranging patched methods to change behavior across multiple calls. Beyond callables in wrapture - monkey patching attributes, dictionaries, generators. Live tracing with wrapture - tracing a live application to see exactly how it works. Zero-code tracing with wrapture - configuring tracing in a separate TOML file without modifying Python code at all. Tracing Flask with wrapture - using the separate wrapture-instrumenation package to instrument a Flask application. That package also provides instrumentation for aiohttp.client, aiohttp.web, django, fastapi, flask, grpc, http.client, httpx, jinja2, requests, sqlalchemy, sqlite3, starlette, urllib.request, urllib3, uvicorn, werkzeug.serving, wsgiref.simple_server, xmlrpc.client, xmlrpc.server. Finding slow code with wrapture - wrapture''s tools for recording timing information, both individually and aggregated across multiple calls. OpenTelemetry export in wrapture - exporting traces to OpenTelemetry. Graham also has a set of interactive workshops for wrapture, implemented as JupyterLab notebooks. Wrapture is still alpha software but it''s already very usable - especially given you can configure and try it out with a TOML file without modifying any Python code at all. This feels like one of those Swiss Army Knife packages that, once mastered, will provide value against all sorts of problems for years to come. Tags: graham-dumpleton, open-source, testing, python, observability, monkey-patching'
content: extracted
html: 2026-09-11-don-t-sleep-on-wrapture.html
---

Graham Dumpleton's new monkey patching package [wrapture](https://wrapture.readthedocs.io/) is shaping up to be an indispensable tool for Python developers. I'm not sure why I've seen so little buzz about it!

Graham has been posting new tutorials for it almost daily since [the initial release](https://simonwillison.net/2026/Aug/31/introducing-wrapture/) on August 31st. Here's everything he's published so far:

- [Introducing wrapture](https://grahamdumpleton.me/posts/2026/08/introducing-wrapture/) - a new monkey patching library that serves both testing and observability (think New Relic style tracing) at the same time.
- [Unit testing with wrapture](https://grahamdumpleton.me/posts/2026/09/unit-testing-with-wrapture/) - how to use it for the same kinds of thing as `unittest.mock`.
- [Recording calls with wrapture](https://grahamdumpleton.me/posts/2026/09/recording-calls-with-wrapture/) - recording method calls as timelines and processing and displaying them as trees.
- [Phased behaviour in wrapture](https://grahamdumpleton.me/posts/2026/09/phased-behaviour-in-wrapture/) - arranging patched methods to change behavior across multiple calls.
- [Beyond callables in wrapture](https://grahamdumpleton.me/posts/2026/09/beyond-callables-in-wrapture/) - monkey patching attributes, dictionaries, generators.
- [Live tracing with wrapture](https://grahamdumpleton.me/posts/2026/09/live-tracing-with-wrapture/) - tracing a live application to see exactly how it works.
- [Zero-code tracing with wrapture](https://grahamdumpleton.me/posts/2026/09/zero-code-tracing-with-wrapture/) - configuring tracing in a separate TOML file without modifying Python code at all.
- [Tracing Flask with wrapture](https://grahamdumpleton.me/posts/2026/09/tracing-flask-with-wrapture/) - using the separate [wrapture-instrumenation](https://github.com/GrahamDumpleton/wrapture-instrumentation) package to instrument a Flask application. That package also provides instrumentation for `aiohttp.client`, `aiohttp.web`, `django`, `fastapi`, `flask`, `grpc`, `http.client`, `httpx`, `jinja2`, `requests`, `sqlalchemy`, `sqlite3`, `starlette`, `urllib.request`, `urllib3`, `uvicorn`, `werkzeug.serving`, `wsgiref.simple_server`, `xmlrpc.client`, `xmlrpc.server`.
- [Finding slow code with wrapture](https://grahamdumpleton.me/posts/2026/09/finding-slow-code-with-wrapture/) - wrapture's tools for recording timing information, both individually and aggregated across multiple calls.
- [OpenTelemetry export in wrapture](https://grahamdumpleton.me/posts/2026/09/opentelemetry-export-in-wrapture/) - exporting traces to OpenTelemetry.

Graham also has a [set of interactive workshops](https://github.com/GrahamDumpleton/wrapture-workshops) for wrapture, implemented as JupyterLab notebooks.

Wrapture is still alpha software but it's already very usable - especially given you can configure and try it out with a TOML file without modifying any Python code at all.

This feels like one of those Swiss Army Knife packages that, once mastered, will provide value against all sorts of problems for years to come.
