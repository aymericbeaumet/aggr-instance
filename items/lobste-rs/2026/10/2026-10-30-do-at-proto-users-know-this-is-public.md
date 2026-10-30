---
title: Do At-Proto Users Know This Is Public?
link: https://melaniekat.com/blog/public-on-pds/
source: lobste-rs
published: 2026-10-30T11:22:43Z
updated: 2026-10-30T11:22:43Z
first_seen: 2026-10-30T22:55:36.634700404Z
authors:
- melaniekat.com via robalex
labels:
- privacy
summary: Comments
content: extracted
html: 2026-10-30-do-at-proto-users-know-this-is-public.html
remote_preview:
  url: https://melaniekat.com/blog/public-on-pds/4hDsfeQsQx-1642.png
  alt: A screenshot my PDS data on PDSls.dev featuring data stored by a variety of apps.
---

I’ve been exploring the AT Protocol “atmosphere” that Bluesky runs on to get a better understanding of what people are doing with it. What I’ve found is a bunch of private data I’m not sure people know is public. Not only public, it’s easy to find.

To start my exploration I used two websites that catalog AT Protocol apps: [atstore.fyi](https://atstore.fyi) and [atmosphereaccount.com](https://atmosphereaccount.com/apps). I’m a long time RSS user, so I got curious about feed readers first. I found this one called Skyreader and I noticed that their page on [atstore.fyi](https://atstore.fyi/products/skyreader) contained reviews and mentions of the app by people who have used it. We’ll come back to this in a minute.

Something to know about Bluesky is that you can self-host what they call a personal data server or “PDS.” It’s where your data on Bluesky and other apps are stored, and it’s mostly public. Using a website like [PDSls.dev](https://pdsls.dev) you can explore it yourself. So if you wanted to see my data it’s [here](https://pdsls.dev/at://melaniekat.com). I can’t control how or what you do with it, it just exists.

I didn’t want to authenticate my account with Skyreader without knowing what it would store publicly. So remember those reviews that people left for Skyreader on atstore.fyi? They’re all listed next to their account names, so I put their handles in to PDSls to get a sense of what would be public. From this I learned that each feed you subscribe to is published in the lexicon (what Bluesky calls an object of data with a schema) `app.skyreader.feed.subscription` which contains a key called `feedUrl` that contains the RSS feed. Cool, so now I know not to put feeds I don’t want my name associated with in there.

Without trying, I found someone’s private Club MacStories feed, which gave me full access to their content. With a little effort I found paywalled feeds for 404 Media’s full text feeds, Patreon backer exclusive feeds, and private RSS feeds for Gitlab repositories.

This led me to seeing what else someone could do with all this openly available data. So I investigated searching across a Bluesky relay (like `bsky.network`) to pull down every collection record for `app.skyreader.feed.subscription`. It’s not as simple as one API call, because what this returns is an array of DIDs (decentralized identifiers) and from there you can look up the PDS server to request those collection records with the actual data.

This gave me over 1,300 RSS feeds people had subscribed to on Skyreader. No authentication, no real restrictions. It’s all just publicly available data on the internet.

It’s not just a Skyreader thing, I’ve noticed a few Discord clones store every message openly. There’s a Twitch clone that is the same way. I can watch as people chat all across [Stream.place](https://stream.place) and even search the data. If you could easily search all of Twitch chat I think that would make some people sweat bullets.

I’m sure some people will say, “Sure, that’s the point,” but I’m not convinced the users understand the risk.
