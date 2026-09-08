---
title: Creepy crawlies
link: https://simonwillison.net/2026/Sep/7/creepy-crawlies/
source: simonwillison-net
published: 2026-09-07T23:08:58Z
updated: 2026-09-07T23:08:58Z
first_seen: 2026-09-08T10:17:10.166315Z
labels:
- ai-ethics
- crawling
- datasette
- git
- linux
summary: 'Creepy crawlies Konstantin Ryabitsev discusses how bad the "background radiation" of abusive crawlers has become from the perspective of git.kernel.org, the official Git repository for the Linux kernel: TL;DR: we spend more CPU cycles rendering commits for scrapers than we spend on all other kinds of legitimate access, including git clones. At any one time, across 5 geo-distributed nodes, there are 14 CPU cores doing nothing but rendering git commits as html. I worry about this a lot from the perspective of Datasette, which serves a huge number of crawlable web pages. Via Hacker News Tags: crawling, git, linux, datasette, ai-ethics'
content: extracted
html: 2026-09-07-creepy-crawlies.html
---

**[Creepy crawlies](https://people.kernel.org/monsieuricon/creepy-crawlies)** ([via](https://news.ycombinator.com/item?id=49491791 "Hacker News")) Konstantin Ryabitsev discusses how bad the "background radiation" of abusive crawlers has become from the perspective of [git.kernel.org](https://git.kernel.org/), the official Git repository for the Linux kernel:

> TL;DR: we spend more CPU cycles rendering commits for scrapers than we spend on all other kinds of legitimate access, including git clones. At any one time, across 5 geo-distributed nodes, there are 14 CPU cores doing nothing but rendering git commits as html.

I worry about this a lot from the perspective of Datasette, which serves a huge number of crawlable web pages.
