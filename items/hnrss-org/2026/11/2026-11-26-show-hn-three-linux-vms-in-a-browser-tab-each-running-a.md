---
title: 'Show HN: Three Linux VMs in a browser tab, each running a different browser'
link: https://michidk.github.io/sidebyside/
source: hnrss-org
published: 2026-11-26T15:00:06Z
updated: 2026-11-26T15:00:06Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- michidk
content: extracted
html: 2026-11-26-show-hn-three-linux-vms-in-a-browser-tab-each-running-a.html
---

Faster: run the relay on your computerThe browsers in this page reach the internet through a relay, because a web page can't open network connections itself. By default that's a free public relay: shared, throttled, and websites see its IP address. Running the relay on your own computer takes that detour out, and websites see your own connection. It listens only on your computer, and the browsers can't reach your home network through it.

1. Install [Node.js](https://nodejs.org/) (version 18 or newer) if you don't have it.
2. Open a terminal and run this. Leave it running while you use this page; stop it with Ctrl+C.It prints `HTTP server listening on 127.0.0.1:5001` when it's ready.
3. Switch this page to it:

   Use local relay

   If your browser asks to allow access to devices on your local network, allow it.
4. Press **Open**. To go back to the public relays, clear the Wisp relay field in Settings.

The page itself still runs the browsers emulated, so expect faster page loads, not native speed.
