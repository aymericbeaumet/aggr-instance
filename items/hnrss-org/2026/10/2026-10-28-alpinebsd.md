---
title: AlpineBSD
link: https://wiki.alpinebsd.org/
source: hnrss-org
published: 2026-10-28T14:30:26Z
updated: 2026-10-28T14:30:26Z
first_seen: 2026-10-28T21:53:36.421640500Z
authors:
- bradley_taunt
content: extracted
html: 2026-10-28-alpinebsd.html
remote_preview:
  url: https://wiki.alpinebsd.org/.static/AlpineBSD_1_cropped.svg
---

## Welcome to AlpineBSD!

AlpineBSD is an operating system based on the [NetBSD kernel](https://netbsd.org) with a userspace inspired by [Alpine Linux](https://alpinelinux.org) and the objective of making it possible to use **as little AI-generated software as possible**. There is no LLM output included in the base system or main package repository ([AlpineBSD AI policy](https://wiki.alpinebsd.org/ai-policy)).

**AlpineBSD is a work in progress**. There is currently no installer available. For the adventurous, a very high-level installation guide [is available here](https://wiki.alpinebsd.org/installation).

## Compared to NetBSD

AlpineBSD ships the NetBSD kernel, libc, and essential platform-specific tools like `mount`.

**NOTICE**: The NetBSD kernel has spotty hardware support, much less so than Linux. You cannot necessarily expect AlpineBSD to work on any random computer you have lying around. Consult [the NetBSD supported hardware list](https://wiki.netbsd.org/laptops/) for more information, particularly for laptops.

## Compared to Alpine Linux

AlpineBSD ships with the apk-tools package manager and busybox, and a slightly more GNU-oriented userspace, preferring e.g. GNU make over BSD make. AlpineBSD is not a fork of Alpine Linux, its package collection is original (though many packages are ultimately based on aports).

Alpine Linux uses the OpenRC init system. Currently, AlpineBSD uses [s6](https://skarnet.org/software/s6-frontend/overview.html) and we intend to add OpenRC as an alternative.
