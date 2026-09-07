---
title: GrapheneOS Overhauled Default Apps and Secure Clipboard
link: https://grapheneos.social/@GrapheneOS/117225539756835649
source: hnrss-org-frontpage
published: 2026-09-06T20:24:00Z
updated: 2026-09-06T20:24:00Z
first_seen: 2026-09-07T17:03:44.343711062Z
authors:
- Cider9986
summary: 'Article URL: https://grapheneos.social/@GrapheneOS/117225539756835649 Comments URL: https://news.ycombinator.com/item?id=49590512 Points: 367 # Comments: 232'
content: extracted
html: 2026-09-06-grapheneos-overhauled-default-apps-and-secure-clipboard.html
---

We're well into the process of converting the Messaging app included in GrapheneOS into a modern app. We'll be making a new release later today with a completely overhauled user interface written in Android Compose. We've made a massive amount of other improvements and bug fixes beyond that too.

* * *

In the longer term, we plan to add support for RCS including support for the standard end-to-end encryption (E2EE) via Messaging Layer Security (MLS). Currently, RCS including E2EE is available on GrapheneOS via Google Messages. We want to avoid the need to use Google Messages for RCS eventually.

* * *

RCS on Android is currently implemented with code across the OS, Google Messages and Google Play services. Our plan is to start by making an equivalent to the portion in Google Messages. It would initially require sandboxed Google Play for RCS activation, etc. but we plan to implement that too.

* * *

RCS isn't an open platform in practice. It isn't even as open as SMS/MMS. It heavily depends on proprietary Google and carrier infrastructure in practice. We can start by replicating Google's approach and then we can work on only using carrier services for carriers where it's actually supported.

* * *

We're also going to be overhauling or fully replacing the rest of the AOSP apps in the near future. AOSP Gallery is incredibly outdated and is being entirely replaced. AOSP Keyboard may be similar. We recently hired a bunch of new people and will be hiring more so our progress will be accelerating.

* * *

In the past, our focus was nearly entirely on the base OS rather than bundled apps with alternatives available. We've heavily ramped up the resources we're investing into the bundled apps. We'll also be scaling up our work on the base OS too. More donations will enable us to hire even more people.
