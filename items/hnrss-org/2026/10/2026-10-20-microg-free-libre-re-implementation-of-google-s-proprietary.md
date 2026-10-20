---
title: 'MicroG: Free-libre re-implementation of Google''s proprietary Android user space'
link: https://github.com/microg/GmsCore/wiki
source: hnrss-org
published: 2026-10-20T06:37:07Z
updated: 2026-10-20T06:37:07Z
first_seen: 2026-10-20T12:56:01.525437407Z
authors:
- theanonymousone
content: extracted
html: 2026-10-20-microg-free-libre-re-implementation-of-google-s-proprietary.html
remote_preview:
  url: https://opengraph.githubassets.com/bb9c42654290d5d1665fc4a69aa0f666fe21012a5932f2f41237b59dd500e740/microg/GmsCore
  alt: Free implementation of Play Services. Contribute to microg/GmsCore development by creating an account on GitHub.
---

## ![microg logo](https://camo.githubusercontent.com/fe0dd09b8fc41bda067ec9f93da8c0dad9308538b788f752a1db621562315ed3/687474703a2f2f692e696d6775722e636f6d2f685859346c63432e706e67) GmsCore

### Overview

microG GmsCore is a free software reimplementation of Google's Play Services. It allows applications calling proprietary Google APIs to run on AOSP-based ROMs like LineageOS, acting as a free replacement for the non-free, proprietary [Google Play Services](https://en.wikipedia.org/wiki/Google_Play_Services) (sometimes referred to as the more generic term "GApps"). It is a powerful tool to reclaim your privacy and freedom while enjoying Android core features (although apps you use that take advantage of it may still be using proprietary libraries to communicate with microG, just as they do when communicating with the actual Google Play Services).

### Features

- Opt-in to Google Services and extend application support
- On-/Offline location service
- Easy on battery, memory and CPU
- No bloatware
- Works on real devices, test emulators and virtual mobile infrastructure
- Free (open source) (Apache 2.0 licensed)

### System Requirements

Your Android system needs to support [signature spoofing](https://github.com/microg/android_packages_apps_GmsCore/wiki/Signature-Spoofing) so GmsCore can pretend the existence of the official Play Services to applications calling Google APIs. See the linked page to know about ROM providing out-of-the-box support and what you can do about the other ones.

### Modules

GmsCore includes the Unified Network Location Provider module (UnifiedNlp) which handles application calls to Google's network location provider.

For a full-working microG setup, you must also install a Play Store replacement application (like *microG Companion*). See [installation](https://github.com/microg/GmsCore/wiki/Installation).

### Project status

The current status of implemented APIs is documented [on this wiki page](https://github.com/microg/android_packages_apps_GmsCore/wiki/Implementation-Status).

### Contributions welcome!

Please report bugs and include logs, screenshots, version numbers and device information depending on what issue you observed. Also, do not bother to contact the author of third-party apps when it might be related to microG services. If you created a logcat, but fear or know it contains sensitive data, you can send a private message.

Thanks for continously supporting this project with feedback, pull requests and donations!
