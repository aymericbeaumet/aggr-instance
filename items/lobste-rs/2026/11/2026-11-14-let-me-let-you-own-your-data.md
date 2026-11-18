---
title: Let Me Let You Own Your Data
link: https://www.willett.io/posts/data/
source: lobste-rs
published: 2026-11-14T16:08:27Z
updated: 2026-11-14T16:08:27Z
first_seen: 2026-11-18T17:57:41.978411823Z
authors:
- willett.io via thomas0
labels:
- databases
- privacy
summary: Comments
content: extracted
html: 2026-11-14-let-me-let-you-own-your-data.html
---

Why do I have to choose between my web app respecting people’s privacy and it, like… having basic functionality?

When I was working on [Cheapskate](https://www.willett.io/posts/cheapskate), I realized after a few weeks that there was a fundamental issue. The app relies on Supabase for the unsexy parts of its architecture: authentication and a SQL datastore. All of it is bog-standard and all of it I happily handed off to be someone else’s problem. To be clear, I love Supabase – if I had to run all that boilerplate myself, I never would have made Cheapskate.

But… the *shape* of it is wrong. I don’t want to own this data. This is a person’s financial details! Shouldn’t that live with them, and not with me? Is it even possible to make a web app like this, with anything close to the same convenience and functionality (durable, works across devices, shareable with friends) as Supabase allows?

Well, I really wanted to. I spent a lot of time trying. Here are all the things that don’t work:

- [localStorage](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage): the easiest one to try, and doesn’t require user accounts, but the inability to access data across devices or share with someone else makes it a non-starter
- [showDirectoryPicker](https://developer.mozilla.org/en-US/docs/Web/API/Window/showDirectoryPicker): cool, and durable (unlike localStorage), but not yet supported by all browsers, and sharing (while possible) requires an extra service, e.g. Dropbox or iCloud
- [CloudKit Private Database](https://developer.apple.com/documentation/cloudkit/ckcontainer/privateclouddatabase): Apple’s obscure BaaS! Locks us into Apple SSO, requires a $99 Apple Developer subscription, and the JS library doesn’t implement sharing anyway
- [SQLite in Google Drive](https://developers.google.com/workspace/drive/picker/guides/web-picker): cute, but it similarly locks us into Google SSO, and I actually tried this, it was miserable to set up both as a user and a developer
- [B-Y-O-DynamoDB](https://aws.amazon.com/dynamodb/): this is actually how I made [Cardi](https://github.com/pickledish/cardi) a few years ago! It works great… other than how it is insane to ask people to “log in” using AWS keys lol
- [remoteStorage](https://remotestorage.io/rs.js/docs/): really cool, and the Platonic ideal of this concept, but support is thin, and the docs explicitly call out collaboration as [unsupported](https://remotestorage.io/rs.js/docs/getting-started/share-public-data.html)
- [CRDT / “local-first”](https://www.inkandswitch.com/essay/local-first/): again cool, but I haven’t seen a mature “Firebase for CRDTs” like the one proposed, and it’s unclear if it would even solve the privacy aspect

Besides, all of these options compromise on the data model anyway, being KV or blob storage when all I really want is an API to run SQL queries. You know, like any normal app?

Argh!
