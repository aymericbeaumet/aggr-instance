---
title: 'Show HN: Flipflop – Iroh-powered persistent tunnels for filesharing'
link: https://github.com/andunieee/flipflop
source: hnrss-org
published: 2026-10-15T04:25:28Z
updated: 2026-10-15T04:25:28Z
first_seen: 2026-10-16T00:17:27.373653594Z
authors:
- andunie
summary: 'This is a project that spun from my frustrations of using LocalSend and DashBeam to transfer stuff between devices, and with the extra goal of being fast, trying out the Slint framework and, most importantly, establishing a communication channel with trusted external people that can be left always on. Comments URL: https://news.ycombinator.com/item?id=50099136 Points: 105 # Comments: 60'
content: extracted
html: 2026-10-15-show-hn-flipflop-iroh-powered-persistent-tunnels-for.html
---

Establish a trusted tunnel between two or more devices: from that point on one can send files to the other anytime.

It works through bluetooth, LAN or even through the wild internet. It uses [Iroh](https://iroh.computer).

Use it to share stuff between all your devices, and maybe devices of your family members, in the same household or afar, sharing links, plain text, anything.

[![flipflop on the desktop](https://github.com/andunieee/flipflop/raw/master/docs/screenshot.png)](https://github.com/andunieee/flipflop/blob/master/docs/screenshot.png)

## Download

Get the latest Linux, macOS and Windows builds and the Android APK from the [releases page](https://github.com/andunieee/flipflop/releases/latest).

On Windows, you can either run the executable direclty or use the `-setup.exe` installer. Because Microsoft hates the world, they will show a "Windows protected your PC" warning when you try.

Because Apple hates the world even more, I can't publish it for iOS.

## Build

```shell
cargo run --release          # desktop
just android-install         # Android (needs cargo-apk, Android SDK + NDK)
```
