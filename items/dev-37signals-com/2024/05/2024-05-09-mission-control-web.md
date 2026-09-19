---
title: Mission Control — Web
link: https://dev.37signals.com/mission-control-web/
source: dev-37signals-com
published: 2024-05-09T17:00:00Z
updated: 2024-05-09T17:00:00Z
first_seen: 2026-09-19T21:30:23.395188495Z
authors:
- Lewis Buckley
summary: Deny requests to your Rails app.
content: extracted
html: 2024-05-09-mission-control-web.html
preview:
  file: 2024-05-09-mission-control-web.preview-b960208d28a7.webp
  width: 256
  height: 134
  alt: 37signals Dev
  color: '#0a0a0a'
images:
- source: https://dev.37signals.com/assets/images/opengraph/mission-control-web.png
  original:
    file: 2024-05-09-mission-control-web.image-8d9ac61e9c46.png
    width: 2400
    height: 1260
  color: '#000000'
extra:
  thumbnail: https://dev.37signals.com/assets/images/opengraph/mission-control-web.png
---

You might have noticed that [Mission Control — Jobs](https://github.com/rails/mission_control-jobs), isn’t simply titled “Mission Control”. That’s because it was developed alongside another useful tool, Mission Control — Web.

Today I’m pleased to announce that we are open sourcing Mission Control — Web and you can [find it on GitHub](https://github.com/basecamp/mission_control-web).

Here’s how it looks:

* * *

## What’s it for?

Mission Control — Web allows immediate control of web requests during incidents, by denying access to parts of the application, defined by specific paths.

My colleague [Jorge](https://dev.37signals.com/author/jorge/) proposed Mission Control — Web in 2022. He identified the need for such a tool during previous incidents, where a whole application became unavailable due to specific features being unperformant.

This is admittedly a simple tool, but it’s a nice lever to be able to pull in case of emergency.

* * *

## What does it do?

It has two parts: an admin dashboard and a middleware. These can be configured in the same app, or ideally, two separate apps. They share a Redis database.

The Admin dashboard is used to set up a list of paths, defined as regex patterns. Requests to these paths will be denied in the application configured with the middleware.

[Version 0.2.0](https://rubygems.org/gems/mission_control-web) is available now. We hope you find it useful! If you are interested in contributing, we have prepared [a list of improvements and new features](https://github.com/basecamp/mission_control-web/issues) we’d like to see in the future, some of them great for first-time contributors.

Sign up to get posts via email,\
 or [grab the RSS feed](https://dev.37signals.com/feed/posts.xml).
