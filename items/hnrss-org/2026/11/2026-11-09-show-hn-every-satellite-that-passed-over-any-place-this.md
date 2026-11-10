---
title: 'Show HN: Every satellite that passed over any place this week, and its photos'
link: https://www.watched.earth/
source: hnrss-org
published: 2026-11-09T17:51:31Z
updated: 2026-11-09T17:51:31Z
first_seen: 2026-11-10T03:25:21.974411074Z
authors:
- a585gupt
summary: 'hey HN, i built this because i wanted to know what had actually photographed my street. type in any place and it finds every satellite that passed over it in the last 7 days (SGP4 on the CelesTrak catalog, runs in your browser in about half a second), then pulls the real Sentinel-2 / Landsat / NASA images from those passes. some fun bits: - the pass search can''t skip a pass. checked it against a brute force on poles, the antimeridian and tiny swaths: 0 missed - all ~17k objects in orbit are drawn on the GPU, solving Kepler''s equation every frame - Sentinel-2 timestamps are often 5-10 min off from when the sat actually crosses your spot, which was a fun one to figure out it only says "watched" if a real image exists, and it doesn''t store your location. code: https://github.com/dev-Armaan/satellites would love feedback, especially on what data sources to add Comments URL: https://news.ycombinator.com/item?id=50428907 Points: 243 # Comments: 45'
content: extracted
html: 2026-11-09-show-hn-every-satellite-that-passed-over-any-place-this.html
remote_preview:
  url: https://www.watched.earth/og-default.png
  alt: 'A real Sentinel-2 satellite photograph of Palm Jumeirah, Dubai, beside the line: Something watched you this week. Enter any place on Earth.'
---

Slow connection, still loading

Watched needs JavaScript and WebGL to compute satellite passes in your browser.
