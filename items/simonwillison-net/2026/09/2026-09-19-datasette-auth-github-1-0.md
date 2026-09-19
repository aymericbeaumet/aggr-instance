---
title: datasette-auth-github 1.0
link: https://simonwillison.net/2026/Sep/19/datasette-auth-github/
source: simonwillison-net
published: 2026-09-19T19:52:02Z
updated: 2026-09-19T19:52:02Z
first_seen: 2026-09-19T23:45:30.112448451Z
labels:
- datasette
- github
- plugins
summary: 'Release: datasette-auth-github 1.0 I run this GitHub login plugin on the agent.datasette.io demo site and I noticed that my authenticated sessions weren''t lasting very long. It turned out that the plugin was setting cookies without a Max-Age parameter, so they were expiring at the end of a browser session (which in Mobile Safari seems to happen pretty often, independently of how you are using the app.) I fixed that in #80 and, since this plugin has been around for quite a while and is tested against both Datasette 0.65.x and Datasette 1.0ax, I decided to bump it up to a 1.0 release. I''m trying to get better at promoting stable plugins to 1.0. Tags: github, plugins, datasette'
content: extracted
html: 2026-09-19-datasette-auth-github-1-0.html
---

I run this GitHub login plugin on the [agent.datasette.io](https://agent.datasette.io/) demo site and I noticed that my authenticated sessions weren't lasting very long. It turned out that the plugin was setting cookies without a `Max-Age` parameter, so they were expiring at the end of a browser session (which in Mobile Safari seems to happen pretty often, independently of how you are using the app.)

I fixed that in [#80](https://github.com/simonw/datasette-auth-github/issues/80) and, since this plugin has been around for quite a while and is tested against both Datasette 0.65.x and Datasette 1.0ax, I decided to bump it up to a 1.0 release. I'm trying to get better at promoting stable plugins to 1.0.
