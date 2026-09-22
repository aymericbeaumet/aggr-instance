---
title: Apple Copland D11E4 Booting in the Browser
link: https://www.pagetable.com/300
source: hnrss-org-frontpage
published: 2026-09-21T18:15:32Z
updated: 2026-09-21T18:15:32Z
first_seen: 2026-09-22T04:43:15.388238719Z
authors:
- luu
summary: 'Article URL: https://www.pagetable.com/300 Comments URL: https://news.ycombinator.com/item?id=49791125 Points: 112 # Comments: 32'
content: extracted
html: 2026-09-21-apple-copland-d11e4-booting-in-the-browser.html
---

Apple’s ill-fated [Copland](https://www.pagetable.com/66) operating system[^1] is notoriously hard to run on real hardware, and has not previously been available in emulation. Here is the last build D11E4 in an improved DingusPPC.

- Click the screen to give the machine the keyboard and the mouse; Escape gives them back.
- On real hardware, booting should take about 30s. A modern machine can match real-time in wasm.
- If any code hits an assertion, it drops into the debugger: click “Continue” to make it go again.
- Try running Copland HD→Applications→GXSlidemaster or Eric’s Solitaire.

The 11 patches necessary for unlocking Copland are on [this branch of my fork](https://github.com/mist64/dingusppc/tree/copland-boot). DingusPPC does not take patches written with the help of AI, so maybe someone wants to re-do these fixes based on the explanations in the commit messages. (The patches for this wasm version are on [this branch](https://github.com/mist64/dingusppc/tree/wasm-port).)

[^1]: Copland was supposed to replace the old System 7 with a modern microkernel-based system, but was cancelled. Apple instead bought the NeXTSTEP/OpenStep OS, which became Mac OS X.
