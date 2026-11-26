---
title: YouTube Ad Punisher – Auto-reloads YouTube pages when an ad plays
link: https://github.com/antonpictures/youtube-punisher
source: hnrss-org
published: 2026-11-26T17:36:47Z
updated: 2026-11-26T17:36:47Z
first_seen: 2026-11-26T21:52:31.976721529Z
authors:
- iantongeorge
content: extracted
html: 2026-11-26-youtube-ad-punisher-auto-reloads-youtube-pages-when-an-ad.html
remote_preview:
  url: https://opengraph.githubassets.com/aeef3b2ccad899828855d1526fa0530385af23c405932e77dd91fa08586bedda/antonpictures/youtube-punisher
  alt: YouTube Ad Blocker - Chrome / Browser Extension. Contribute to antonpictures/youtube-punisher development by creating an account on GitHub.
---

> **Auto-reloads YouTube pages when an ad plays — preserving your playback position. Punish the ads.**

[![License: MIT](https://camo.githubusercontent.com/e2ec9f9b84cbfa4a84d3c943ea5dcaf295c8cee0c665120898bd30ca552b1e39/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f4c6963656e73652d4d49542d79656c6c6f772e737667)](https://opensource.org/licenses/MIT) [![Chrome Extension](https://camo.githubusercontent.com/2d1a9a68b35753ba29f778cdca1676d77957b365564d5a09e369be00d7603299/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f4368726f6d652d457874656e73696f6e2d626c75652e737667)](https://chrome.google.com/webstore) [![Manifest V3](https://camo.githubusercontent.com/e58111e712826acc9ada72f8c6025b57bf12cff8a9b3650c87fdd5059c70ca5d/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f4d616e69666573742d56332d677265656e2e737667)](https://developer.chrome.com/docs/extensions/mv3/)

* * *

## The Problem

Ad density on YouTube is channel-dependent and changes over time — some channels front-load several ads, others run none. Traditional ad blockers can get detected by YouTube's anti-adblock system ("Ad blockers are not allowed here"), and the skip button takes a few seconds to appear. You sit there, hostage.

## The Solution

**Reload the page.** That's it. Two reloads and the ad is gone. Every time.

🎯 YouTube Ad Punisher automates this: the moment an ad appears, it captures your exact playback timestamp, reloads the page, and YouTube auto-seeks back to where you were. No ads. No waiting. No skip button hunting.

## Tips

- **Auto escape:** after 3 reloads, the extension automatically switches to a helper tab for 4 seconds (George's discovery, now automated), then switches back. No manual tab switching needed.

- **Ads keep coming after reload?** Switch to another tab for a few seconds, then switch back. Chrome throttles background tabs, which lets the ad state settle — George discovered this works every time.

- **Incognito:** see the Incognito section above.

## How It Works

```
Ad detected → Capture timestamp → Reload page → Auto-seek to timestamp → Resume
                    ↓ (if 3 reloads fail)
              Redirect to yout-ube.com (ad-free mirror, no looping)
```

### Two strategies

1. **Reload** (primary) — reloads the page 1-3 times. YouTube re-evaluates ad serving on each reload; after 1-2 cycles the ad slot is empty and the video plays clean. Timestamp is preserved via `&t=Xs` URL param + JS seek.

2. **yout-ube.com redirect** (fallback) — if reloads don't clear the ad after 3 attempts, redirects to `yout-ube.com` (an ad-free YouTube mirror). The URL includes `&loop=0` to prevent the infinite replay issue that the mirror has by default.

3. **Detect** — watches for `.ad-showing` class, skip buttons, and ad overlays (every 500ms + MutationObserver)

4. **Capture** — grabs `video.currentTime` and saves it to sessionStorage

5. **Reload** — `location.replace()` with `&t=Xs` in the URL

6. **Restore** — YouTube auto-seeks to the timestamp; playback resumes

7. **Cooldown** — 3-second guard prevents reload loops

## Features

- ✅ **Zero ads** — reloads until the ad is gone (usually 1-2 cycles)
- ✅ **Timestamp preserved** — never lose your place in the video
- ✅ **Live streams** — plain reload (no timestamp to preserve)
- ✅ **Ad history database** — every ad logged with advertiser, duration, skip text, video, and timestamp
- ✅ **Export** — JSON or CSV download of all collected ad data
- ✅ **Popup viewer** — click the extension icon to see stats, top advertisers, and full history
- ✅ **No permissions abuse** — only `storage` and `tabs`, only on YouTube domains

## Install

### Option A — Chrome / Brave / Edge / Opera (Load unpacked)

1. Clone the repo:

   ```shell
   git clone https://github.com/antonpictures/youtube-punisher.git
   ```

2. Open `chrome://extensions`
3. Enable **Developer mode** (top right)
4. Click **Load unpacked** → select the `youtube-punisher` folder
5. Done — activates on all YouTube pages automatically. ✅ **Tested**

### Option B — Safari (experimental: AppleScript workaround)

Safari blocks ad-hoc signed extensions from auto-injecting into YouTube. As a workaround, use the JS overlay + AppleScript watcher (`yt-ad-watch.sh`):

1. In Safari, enable: Safari → Settings → Advanced → **Show Develop menu in menu bar**
2. In Safari (on a YouTube page), run `./yt-ad-watch.sh` in Terminal
3. When an ad appears, the watcher detects it via MutationObserver (event-driven in the page), reloads with `&t=Xs`, and the MutationObserver re-detects on the next page load

> **Note:** This is best-effort. Chrome is fully reliable; Safari works but may need manual testing.

### Option C — Download zip

1. Download the latest release from [Releases](https://github.com/antonpictures/youtube-punisher/releases)
2. Unzip and follow Option A steps 2–5

## The Ad History Database

Every ad YouTube serves you is recorded:

| Field       | What it captures                     |
| ----------- | ------------------------------------ |
| Timestamp   | When the ad was detected             |
| Advertiser  | Company name from the ad overlay     |
| Video       | Title of the video you were watching |
| Video time  | Your playback position before reload |
| Ad duration | Length of the ad                     |
| Skip text   | "Skip Ad" button text (if available) |
| Action      | Reloaded or skipped                  |

Export as **JSON** or **CSV** from the popup. Data stays local — nothing is sent anywhere.

## Why Reloads Work (and Why Nobody Thought of This)

YouTube's ad system serves ads as part of the page load. When you reload, YouTube re-evaluates whether to serve an ad based on your session state. After 1-2 reloads, the ad slot is typically empty and the video plays directly. This is a **client-side, zero-cost** workaround — no filter lists, no network blocking, no detection by YouTube's anti-adblock system.

### The Scale Question

If millions of users did this, each reload = a full page load = server request. YouTube would see a spike in page loads but a drop in ad impressions. It's not a DDoS (no amplification, no botnet), but it's a **passive protest**: every reload is a vote that says "I will not watch your ad." The ad revenue model breaks when reloads are cheaper than ad views.

> *"Every reload is a vote: I will not watch your ad."* — George Anton, creator

## Comparison with Other Tools

| Tool                      | How it works                              | Reloads? | Collects data? |
| ------------------------- | ----------------------------------------- | -------- | -------------- |
| **uBlock Origin**         | Blocks ad network requests                | No       | No             |
| **SponsorBlock**          | Skips sponsored segments in videos        | No       | No             |
| **Enhancer for YouTube**  | Auto-clicks "Skip Ad" button              | No       | No             |
| **🎯 YouTube Ad Punisher** | Detects ad → reloads → seeks to timestamp | Yes      | Yes            |
| **Popup blockers**        | Block `window.open()` popups              | No       | No             |

**Best combo:** uBlock Origin (blocks most ads) + 🎯 YouTube Ad Punisher (catches the ones that slip through and logs them) + SponsorBlock (skips sponsor reads). They don't conflict.

## Limitations

- YouTube changes DOM class names periodically — if ads aren't detected, check the console for `[YT-AdReloader]` logs
- Some ads (mid-roll on live streams) can't preserve position — plain reload
- YouTube's "Premium" upsell interstitials may need a second reload cycle (handled by cooldown)

## Files

| File                      | Purpose                                                      |
| ------------------------- | ------------------------------------------------------------ |
| `manifest.json`           | Chrome extension manifest v3                                 |
| `content.js`              | Ad detection + reload + timestamp restore + history tracking |
| `popup.html` / `popup.js` | Ad history viewer with export                                |
| `background.js`           | Minimal service worker                                       |

## License

MIT — do whatever you want with it.

* * *

**Built with [Alice (SIFTA)](https://github.com/antonpictures/ANTON-SIFTA) for George Anton.**

*"If millions installed this, YouTube would feel every reload."* 🐜
