---
title: Laya (OS Jev) on Mac M4 CoreML Offline (45 decisions per second)
link: https://gist.github.com/fordnox/e592d0f68b543fd044be8e6d040863a0
source: hnrss-org-frontpage
published: 2026-09-20T15:58:52Z
updated: 2026-09-20T15:58:52Z
first_seen: 2026-09-21T00:29:33.148458702Z
authors:
- putna
summary: 'Article URL: https://gist.github.com/fordnox/e592d0f68b543fd044be8e6d040863a0 Comments URL: https://news.ycombinator.com/item?id=49777106 Points: 125 # Comments: 22'
content: extracted
html: 2026-09-20-laya-os-jev-on-mac-m4-coreml-offline-45-decisions-per-second.html
preview:
  file: 2026-09-20-laya-os-jev-on-mac-m4-coreml-offline-45-decisions-per-second.preview-385d83fea357.webp
  width: 256
  height: 128
  alt: Laya on Mac m4 CoreML Offline https://github.com/mizorewww/laya-coreml - laya.sh
  color: '#25282c'
images:
- source: https://github.githubassets.com/assets/gist-og-image-54fd7dc0713e.png
  original:
    file: 2026-09-20-laya-os-jev-on-mac-m4-coreml-offline-45-decisions-per-second.image-0c7d1f3af096.png
    width: 1280
    height: 640
  color: '#1c1f23'
---

Nice! I just wanted to see what Laya does and put it behind Cloudflare's `typesafe/jev` API:

```
curl https://laya.inference.zaitlabs.com/ai/run \
  -H "Content-Type: application/json" \
  -d '{
    "state": "Help! My payouts have been failing for 3 days.",
    "questions": {
      "is_urgent": {"type": "noul", "instructions": "Does this convey urgency?"}
    }
  }'
```

```
{"answers":{"is_urgent":{"type":"noul","noul":0.7894}}}
```

[https://laya.inference.zaitlabs.com/terms](https://laya.inference.zaitlabs.com/terms)
