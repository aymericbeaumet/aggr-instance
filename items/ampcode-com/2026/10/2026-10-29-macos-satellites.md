---
title: macOS Satellites
link: https://ampcode.com/news/macos-satellites
source: ampcode-com
published: 2026-10-29T00:00:00Z
updated: 2026-10-29T00:00:00Z
first_seen: 2026-10-29T08:21:53.512644349Z
summary: 'Amp now has a Mac: ask it to work on your iOS or Mac app and watch it build, test, and run the app. Port to iPhone Duo New Mac App Feature Penny runs on iPhone 17 Pro. Amp has it open in DeviceHub on a Mac next to the thread. Amp checks Penny on the iPhone Duo and fixes the layout for both screens. Then it opens the new build on the Duo, closed. Amp unfolds the Duo, and Penny switches to the side-by-side layout. It taps a transaction to open it. Amp''s report: what it checked on the Duo. Amp recorded the Mac while it checked: closed, unfolded, tapped. Amp runs the iPod app for the Mac as it is today. Amp adds a Model setting for the black and red U2 iPod. Then it drives the app: Settings, Model, U2. Music, Artists, scroll to The Avalanches. The Avalanches, “Since I Left You.” Now playing on the U2 iPod. Amp''s report: what it built and how it checked it. Amp recorded the Mac while it played the song itself. How It Works Your thread still runs on a Linux orb, next to your dev server and toolchain. The Mac is a second machine attached to that orb. Amp can automatically start a Mac, run commands on it, and read and write files, so it decides on its own when the work needs a Mac. When it does, our server boots a fresh Mac in the cloud with Xcode and the simulators installed, in about 30 seconds. Your orb connects to it and keeps your code in sync, so Amp still edits on the orb and only builds, runs, and tests on the Mac. Screenshots, logs, and builds come back to the thread, and the app on the Mac reaches your backend on the orb through portals. The thread''s Mac tab streams the Mac''s screen. Watch Amp''s work, or take control of the Mac and try the app yourself. Every Mac starts fresh, and nothing on it is saved. When Amp is done with a Mac, it keeps it running for 20 more minutes. If you send a follow-up in that time, Amp reuses the same Mac and its build cache instead of starting a new Mac and building from scratch. After that, the Mac is deleted. To delete it right away, archive the thread or ask Amp to stop the Mac. Morbs Are Dead, Long Live macOS Satellites Before shipping macOS Satellites, we tried out Morbs: orbs that ran the whole thread on a Mac. But we ran into issues that made the approach unfeasible for a wider rollout. Docker and your backend took minutes to start there,[^1] and keeping a Mac disk around for every thread got expensive. So we split the thread across two machines. Everything that runs well on Linux stays on the orb and starts fast, and a Mac only shows up for the work that needs macOS.[^2] We''re calling them macOS Satellites because they orbit your orb. Pricing Size CPUs Memory Price mac.medium 6 14 GB $2/hour mac.large 12 28 GB $4/hour macOS Satellite time comes out of the same orb usage your plan includes, just at a higher rate. Once that runs out, it comes out of your Amp credits, the same way orb time does. Every Mac starts as a mac.medium. If you need more, ask Amp for a mac.large, or turn off mac.medium under Mac Satellites in Settings → Experimental Features. Join the Waitlist macOS Satellites are in beta and opening up as we add Macs. Join the waitlist in Settings → Experimental Features. Each day you click Move Me Up moves you up the line. Go point Amp at that app you''ve been meaning to build! Find out more in the docs. New to Amp? Sign up and get started on your Apple apps.'
content: extracted
html: 2026-10-29-macos-satellites.html
remote_preview:
  url: https://ampcode.com/og?_template=news&_version=7&title=macOS+Satellites&date=October+29%2C+2026&tagline=Your+orb+can+now+start+a+Mac+to+build%2C+test%2C+and+run+your+iOS+and+Mac+apps.&screenshot=https%3A%2F%2Fstatic.ampcode.com%2Fnews%2Forbs-now-have-a-mac-ipod-u2-desktop.avif&sig=dd968ad4e29e2b43d8c99e66674fb49d211a7a918eb75ace73661ed76139983b
---

Amp now has a Mac: ask it to work on your iOS or Mac app and watch it build, test, and run the app.

## How It Works

Your thread still runs on a Linux orb, next to your dev server and toolchain. The Mac is a second machine attached to that orb. Amp can automatically start a Mac, run commands on it, and read and write files, so it decides on its own when the work needs a Mac.

When it does, our server boots a fresh Mac in the cloud with Xcode and the simulators installed, in about 30 seconds. Your orb connects to it and keeps your code in sync, so Amp still edits on the orb and only builds, runs, and tests on the Mac. Screenshots, logs, and builds come back to the thread, and the app on the Mac reaches your backend on the orb through portals.

The thread's Mac tab streams the Mac's screen. Watch Amp's work, or take control of the Mac and try the app yourself.

Every Mac starts fresh, and nothing on it is saved. When Amp is done with a Mac, it keeps it running for 20 more minutes. If you send a follow-up in that time, Amp reuses the same Mac and its build cache instead of starting a new Mac and building from scratch. After that, the Mac is deleted. To delete it right away, archive the thread or ask Amp to stop the Mac.

## Morbs Are Dead, Long Live macOS Satellites

Before shipping macOS Satellites, we tried out Morbs: orbs that ran the whole thread on a Mac. But we ran into issues that made the approach unfeasible for a wider rollout. Docker and your backend took minutes to start there, and keeping a Mac disk around for every thread got expensive.

So we split the thread across two machines. Everything that runs well on Linux stays on the orb and starts fast, and a Mac only shows up for the work that needs macOS. We're calling them macOS Satellites because they orbit your orb.

## Pricing

| Size         | CPUs | Memory | Price   |
| ------------ | ---- | ------ | ------- |
| `mac.medium` | 6    | 14 GB  | $2/hour |
| `mac.large`  | 12   | 28 GB  | $4/hour |

macOS Satellite time comes out of the same orb usage your plan includes, just at a higher rate. Once that runs out, it comes out of your Amp credits, the same way orb time does.

Every Mac starts as a `mac.medium`. If you need more, ask Amp for a `mac.large`, or turn off `mac.medium` under **Mac Satellites** in [Settings → Experimental Features](https://ampcode.com/settings/experimental-features).

## Join the Waitlist

macOS Satellites are in beta and opening up as we add Macs. Join the waitlist in [Settings → Experimental Features](https://ampcode.com/settings/experimental-features). Each day you click **Move Me Up** moves you up the line.

Go point Amp at that app you've been meaning to build! Find out more in [the docs](https://ampcode.com/docs/orbs/mac).

New to Amp? [Sign up](https://ampcode.com/signup) and get started on your Apple apps.
