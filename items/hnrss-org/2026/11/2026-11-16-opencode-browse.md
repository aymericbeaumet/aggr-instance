---
title: OpenCode Browse
link: https://opencode.ai/product/browse
source: hnrss-org
published: 2026-11-16T21:20:37Z
updated: 2026-11-16T21:20:37Z
first_seen: 2026-11-17T09:02:46.426369911Z
authors:
- mfiguiere
content: extracted
html: 2026-11-16-opencode-browse.html
remote_preview:
  url: https://opencode.ai/og/product-browse-1528ffd6f6.png
  alt: 'OpenCode link card: “Browse” and “Let your agents use your real browser, with your tabs and logins.” beside the desktop app setting up the Browse extension, at an angle.'
---

An MCP server and a Chromium extension that let your agents use your real browser, with your tabs, your logins and your extensions. They open tabs in the background, grouped out of your way, and read pages, click, type, take screenshots and record tabs. When it's your turn, for a login, a 2FA prompt or a payment, they hand the tab back to you and continue once you click Continue.

- **Your real browser** Chrome, Edge, Brave, Arc, Helium, Vivaldi, Opera and Chromium.
- **Out of your way** Agent tabs open in the background, in collapsed groups.
- **Handoffs** Do the login or 2FA step yourself, then click Continue.
- **Lend a tab** Click the toolbar button to hand an agent a tab you have open.
- **Record and capture** Screenshots, tab recordings and network traffic.
- **Any MCP client** OpenCode, Claude Code, Cursor, Codex, Zed and many more.
- **All your logins** Agents use the sites you're already signed into. No keys, no setup.
- **Allowed sites** Keep an agent's tab on the sites you name.

Install the [Chrome extension](https://chromewebstore.google.com/detail/mfnicocicmmlkpjnaffgihfjhdgjkdjg) first. It walks you through the rest of the steps. To install the CLI directly (you should not need to do this), use any of these.

- curl curl -fsSL https://opencode.ai/browse/install | bash
- pwsh irm https://opencode.ai/browse/install.ps1 | iex
- npm npm install -g @opencode/browse && browse install
- bun bun add -g @opencode/browse && browse install

- Hex On-device voice dictation for any app. [Learn more](https://anoma.ly/hex)
- OpenTunnel Public HTTPS URLs for local apps. [Learn more](https://opentunnel.xyz)
- OpenTUI A library for terminal user interfaces. [Learn more](https://opentui.com)
