---
title: Tor Project Issues Rapid-Response DoS-Defense Fix Amid Sustained DDoS Attack
link: https://forum.torproject.org/t/security-release-0-4-9-17/22454
source: hnrss-org
published: 2026-11-16T00:21:54Z
updated: 2026-11-16T00:21:54Z
first_seen: 2026-11-16T07:11:22.159097445Z
authors:
- aillia
content: extracted
html: 2026-11-16-tor-project-issues-rapid-response-dos-defense-fix-amid.html
remote_preview:
  url: https://forum.torproject.org/uploads/default/original/2X/d/d3424bed34f4ec18b2e99178c2801ba6dfb655d6.png
---

## Where to Download

- [Tarballs](https://dist.torproject.org/)
- [Gitlab Repository](https://gitlab.torproject.org/tpo/core/tor)
- [Bug Report](https://gitlab.torproject.org/tpo/core/tor/-/issues/new)

## Changes

Below are the major changes of the released versions and links to more detailed release notes.

## Stable

Today, we are releasing **0.4.9.17**, which includes a series of security fixes. One fix in particular is especially important for relay DoS defenses related to IPv6 handling.

This release comes in rapid response to the ongoing DDoS attack on the network, which has affected most relay operators. This is not an easy problem to address, as relays across the network are being overwhelmed by a massive flood of client circuits. We are actively evaluating additional defenses and so expect further releases in the near future.

We want to extend a huge thank you to all relay operators who have been helping us deal with this ongoing attack. The `tor-relays@` mailing list has been very active and we are very grateful for your time, effort, and support. Please, continue doing so!

**If you run a relay, please upgrade as soon as possible.** No action item is required after upgrading.

Thank you to all… except our attacker(s).

#### Release Notes

- [0.4.9.x](https://gitlab.torproject.org/tpo/core/tor/-/raw/release-0.4.9/ReleaseNotes)

2 Likes
