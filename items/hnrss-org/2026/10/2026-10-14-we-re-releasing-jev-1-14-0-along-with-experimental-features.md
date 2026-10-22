---
title: We’re releasing Jev 1.14.0 along with experimental features for dreamers
link: https://typesafe.ai/blog/jev-1-14
source: hnrss-org
published: 2026-10-14T21:11:03Z
updated: 2026-10-14T21:11:03Z
first_seen: 2026-10-15T00:11:15.830933315Z
authors:
- emschwartz
content: extracted
html: 2026-10-14-we-re-releasing-jev-1-14-0-along-with-experimental-features.html
extra:
  comments_url: https://news.ycombinator.com/item?id=50095353
  num_comments: 52
  points: 143
---

*TL;DR: We're releasing an improved Jev 1.14.0 along with experimental features for our dreamers: images, a new type, and longer context. Enter the* `danger zone` *at your own risk. 🔥*

## Our philosophy

A huge debate we’ve been having is on the balance between reliability and creativity.

To really care about the prod builders, we should be shipping models that meet our quality bar for intelligence and have been battle-tested to the limits of what we can do internally.

On the other hand, there’s no way that our team has a full understanding of what is needed to spark delight in the world and build sick sci-fi. We don’t want to impose our values of extreme reliability on those dreamers.

Our solution: share those dreamer-ready features under “danger zone” flags in our API headers. These are features that will be here to stay for future models, are (as far as we can tell) better than what exists out there, and will continue to get better by leaps and bounds. But as a warning, we think there is still way too much jaggedness for us to vouch for these for production use cases!

See these new capabilities as a continuation of what we’ve built over 2 years of stealth. They’re less ready for automation, but very fun to play with. They’re also an early glimpse into what we’re focused on making reliable enough to meet TypeSafe standards.

### Composable Images

Not only are we enabling multimodality for future Jev models, we’re enabling it for the current Jev that you all know and love. Surprise: the Jev you all know and love (`jev-1.13.0`) was multimodal all along!

Image support is fully composable via a find-and-replace in `assets`. You can put them anywhere that our JSON interface supports: in the `state`, `instructions`, or even `criteria`! Play around with it, push the boundaries of technology, and let us know where Jev can improve.

As a warning, all models (Jev included) perform worse on multimodality over pure text. Breaking down questions into the simplest possible decompositions helps a lot.

`TypeSafe-Danger-Zone-Images: true`

Check out docs: [docs.typesafe.ai/go/images](http://docs.typesafe.ai/go/images)

### Types Beyond Decisions

We’re introducing another new type as well: `box`. It returns a single bounding box - no type errors allowed. It’s our first type that’s not purely meant for smart-if statements, and is a preview for our explorations beyond simple decisions into what else System One Models can support.

We made huge leaps and bounds on this one recently, so we’re enabling this only for `jev-1.14.0` (spoilers).

Check out docs: [docs.typesafe.ai/go/box](http://docs.typesafe.ai/go/box)

### Size *does* matter, apparently 💅

We’ve heard you loud and clear on context lengths. Jev actually supports a much longer context length than we’ve enabled in production. We chose to limit it intentionally because Jev, like all models, is less reliable on longer contexts, and this constraint encourages more effective usage patterns in prod.

We aren’t your parents though, so while we strongly recommend limiting context length, you no longer have to:

`TypeSafe-Danger-Zone-Long-Context: true`

Check out docs: [docs.typesafe.ai/models#context-length](https://docs.typesafe.ai/models#context-length)

### Opt-in to help us

We love the community’s desire to help make models better! We’ve added an opt-in to allow you to help find model jaggedness with your queries. We will still not train on your queries, but this does make it easier for us to find issues and help debug your use cases.

![](https://framerusercontent.com/images/9xWPxly4HZ7S43IpAEGS2VOFM.png?width=1528&height=1078)

## jev-1.14.0

We’re also releasing a new version of Jev under `jev-preview`! Not meant to be flashy but instead be small, subtle improvements to *everything*. Most things should be a tiny bit better, but we expect the largest gain in multi-lingual support.

Model releases should generally also be strict improvements, but we’re well aware of how much developers hate updating their dependencies. `jev-latest` will be updated to `jev-1.14.0` in a week. `jev-1.13.0` will be available for a while.

We intend to give 60 days warning before `jev-1.13.0` is formally deprecated, and 30 days for `jev-1.14.0`. We’ll release a deprecation policy prior to this.

### Measuring Intelligence

As you all know, we [dislike public benchmarks](https://typesafe.ai/blog/antibenchmaxxing). However, we have our own internal best guesses on how to measure the form factor of intelligence that matters most for developers.

We’re trying something new out here: we’re going to treat you guys like adults and share this with you all.

##### Some nuance

- This measurement is our best guess and will continue to evolve. If ever we show you our internal numbers again, don’t expect them to be consistent.

- Don’t believe it entirely. Always favor your own experimentation and evals with your use cases.

- We’re including only open weight models because we guard our validation set very closely and are spooked out about the data policies of our model providers.

- This is likely biased towards our models. Feel free to ignore our model, and instead focus on the relative ranking of open source models.

![](https://framerusercontent.com/images/IMAdGJMIKQYl8JkqnebGaIB3g.png?width=2136&height=1372)![](https://framerusercontent.com/images/TmIsBzc45HeG5x98MZM5oHM.png?width=2136&height=1372)![](https://framerusercontent.com/images/UHjjLTnHvk90WzLkGbrDdbSHkM.png?width=2136&height=1372)

## What’s next

Lots of people love to ask what kind of sci-fi we’re releasing next. While we are working on some mindblowing stuff, expect most releases to be beautifully boring (like we think AI should be): small improvements across the board, better stability, and features for real deployment.
