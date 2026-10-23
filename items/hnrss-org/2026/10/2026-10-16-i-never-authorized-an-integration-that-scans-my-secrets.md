---
title: I never authorized an integration that scans my secrets
link: https://github.com/home-assistant/home-assistant.io/issues/49314
source: hnrss-org
published: 2026-10-16T20:33:41Z
updated: 2026-10-16T20:33:41Z
first_seen: 2026-10-17T00:21:44.539394313Z
authors:
- trvrprkr
content: extracted
html: 2026-10-16-i-never-authorized-an-integration-that-scans-my-secrets.html
extra:
  comments_url: https://news.ycombinator.com/item?id=50124251
  num_comments: 117
  points: 164
---

Leaving this closed since this repository is for our documentation, but I want to follow up, because my earlier replies were more dismissive than this deserved. My apologies.

As it turns out, this check can already be turned off after all:

```
ha security options --pwned=false
```

To be transparent: this check isn't new, it has been part of the Supervisor for years. It checks app options that are marked as passwords. A recent release fixed the warning that surfaces it, which is why you're seeing it now.

Privacy was a design requirement for this feature from the start, which is why your secrets never leave your system. The Supervisor hashes them locally and sends only the first 5 characters of the SHA-1 hash to Have I Been Pwned. HIBP returns every hash starting with that prefix, and the matching happens on your own instance. You can see exactly what it does here: [https://github.com/home-assistant/supervisor/blob/main/supervisor/utils/pwned.py](https://github.com/home-assistant/supervisor/blob/main/supervisor/utils/pwned.py)

You are right that our documentation should have explained this, including how to turn it off. We'll get that page updated.

../Frenck

[![](https://github.com/frenck/frenck/raw/main/images/linkedin.svg)](https://www.linkedin.com/in/frenck/)    [![](https://github.com/frenck/frenck/raw/main/images/youtube.svg)](https://youtube.com/@frenck)    [![](https://github.com/frenck/frenck/raw/main/images/bluesky.svg)](https://bsky.app/profile/frenck.social)    [![](https://github.com/frenck/frenck/raw/main/images/mastodon.svg)](https://fosstodon.org/@frenck)    [![](https://github.com/frenck/frenck/raw/main/images/instagram.svg)](https://www.instagram.com/frenck/)    [![](https://github.com/frenck/frenck/raw/main/images/threads.svg)](https://www.threads.net/@frenck)    [![](https://github.com/frenck/frenck/raw/main/images/facebook.svg)](https://www.facebook.com/frenck.dev/)    [![](https://github.com/frenck/frenck/raw/main/images/x.svg)](https://x.com/frenck)    [![](https://github.com/frenck/frenck/raw/main/images/tiktok.svg)](https://www.tiktok.com/@frenck.nl)

Blogging my personal ramblings at [frenck.dev](https://frenck.dev)
