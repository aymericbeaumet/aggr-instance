---
title: I Quit My Cushy Job at OkCupid to Live on Donations to Zig
link: https://andrewkelley.me/post/full-time-zig.html
source: andrewkelley-me
published: 2018-06-07T14:20:30Z
updated: 2018-06-07T14:20:30Z
first_seen: 2026-09-19T21:30:23.395188495Z
summary: 'I Quit My Cushy Job at OkCupid to Live on Donations to Zig I am fortunate to be one of those people who started tinkering with code in their teens, and then found themselves in the position where their hobby could not only pay rent, but afford a comfortable standard of living, even in New York City. For a little over a year now, I worked on the backend team at OkCupid, maintaining a large C++ codebase that has not been reworked since 2005 when Maxwell Krohn published the original OKWS paper on which OkCupid is still based. As satisfying as it is to delete dead code, rework abstractions to remove footguns, and migrate a monolithic codebase to Service Oriented Architecture, my coworkers were well aware that my real passion was spent on nights and weekends, where I poured all the energy I could muster into Zig - if not from my incessant comparisons between snippets of C++ code and how it could be better expressed in Zig, then from the tea mug that my lovely girlfriend Alee got me as a celebratory gift for reaching release 0.1.0: Zig is Picking Up Steam Ever since I gave this Software Should be Perfect talk, it seems that the Zig community has seen a steady influx of new members. The Zig community grew in size so much, that some weekends I found myself with only enough time to merge pull requests and respond to issues, and no time leftover to make progress on big roadmap changes. I found this both exciting, and frustrating. Here I was working on legacy code 40 hours per week, while multiple Zig issues needed my attention. And so I took the plunge. I gave up my senior backend software engineer salary, and will attempt to live on monthly donations. Roadmap Here are some of the bigger items that are coming up now that I have more time: remove more sigils from the language add tuples and remove var args self-hosted compiler http server and http client based on async/await decentralized package manager generate html documentation hot code swapping I can''t wait to make significant progress on these items. That said, I will also dedicate time each day for bug fixes and writing documentation. Every weekday except Friday, this will be my wakeup process: Make tea Fix a bug Write some documentation Deal with all open pull requests, whether that is by merging, rejecting, requesting changes, or solving the use case a different way. Proceed with working on a big feature item. On Fridays, I will spend the day working on a project other than Zig, but one that is implemented in Zig. For example: Rewrite Groove Basin in Zig. Work on my game that the raspberry pi boots directly into (this explores Zig''s ability to be used for embedded and Operating System development) Start reworking my digital audio workstation in Zig. I''m beyond excited to get Zig to a place where it can reasonably be used for high quality and practical software projects. My goal is to make Zig so useful and practical that people will find themselves using it without intending to. And so it comes down to this - will you fund my efforts? Why Donate to Zig? Consider the basic premise of a for-profit business: Customers pay currency in exchange for goods or services. If all goes well, both parties benefit from the exchange. But for-profit businesses are motivated, at the end of the day, not by customer satisfaction, but by profit. It''s fine, this is how the world works. But it''s not how Zig works. Zig is created by the open source community, for the open source community. One of our main tenets is Together we serve end users. The Zig project is a public service, entirely motivated by improving the technical landscape of open source tools. There is no board to please, no stock shareholders demanding higher quarterly earnings, no office politics or career progressions on the line. 100% of donations I receive go towards paying rent, buying food, and generally attempting to live a modest, but healthy life. Will you pledge $5/month?'
content: extracted
html: 2018-06-07-i-quit-my-cushy-job-at-okcupid-to-live-on-donations-to-zig.html
preview:
  file: 2018-06-07-i-quit-my-cushy-job-at-okcupid-to-live-on-donations-to-zig.preview-849e01c0eb85.webp
  width: 256
  height: 206
  color: '#b99e82'
images:
- source: https://andrewkelley.me/img/zig-mug.jpg
  original:
    file: 2018-06-07-i-quit-my-cushy-job-at-okcupid-to-live-on-donations-to-zig.image-8ad13b830388.jpg
    width: 533
    height: 428
  color: '#e4c8a6'
---

[Andrew Kelley](https://andrewkelley.me/) - I Quit My Cushy Job at OkCupid to Live on Donations to Zig (2018 Jun 07)

I am fortunate to be one of those people who started tinkering with code in their teens, and then found themselves in the position where their hobby could not only pay rent, but afford a comfortable standard of living, even in New York City.

For a little over a year now, I worked on the backend team at [OkCupid](https://www.okcupid.com/), maintaining a large C++ codebase that has not been reworked since 2005 when [Maxwell Krohn](https://twitter.com/maxtaco) published the original [OKWS paper](https://pdos.csail.mit.edu/papers/okws:krohn-ms/okws_krohn-ms.pdf) on which OkCupid is still based.

As satisfying as it is to delete dead code, rework abstractions to remove footguns, and migrate a monolithic codebase to [Service Oriented Architecture](https://en.wikipedia.org/wiki/Service-oriented_architecture), my coworkers were well aware that my real passion was spent on nights and weekends, where I poured all the energy I could muster into [Zig](https://ziglang.org/) - if not from my incessant comparisons between snippets of C++ code and how it could be better expressed in Zig, then from the tea mug that my lovely girlfriend Alee got me as a celebratory gift for reaching [release 0.1.0](https://ziglang.org/download/#release-0.1.1):

![](https://andrewkelley.me/img/zig-mug.jpg)

## Zig is Picking Up Steam

Ever since I gave this [Software Should be Perfect](https://www.recurse.com/events/localhost-andrew-kelley) talk, it seems that the Zig community has seen a steady influx of new members.

The Zig community grew in size so much, that some weekends I found myself with only enough time to merge pull requests and respond to issues, and no time leftover to make progress on big roadmap changes.

I found this both exciting, and frustrating. Here I was working on legacy code 40 hours per week, while multiple Zig issues needed my attention.

And so I took the plunge. I gave up my senior backend software engineer salary, and will attempt to live on [monthly donations](https://github.com/users/andrewrk/sponsorship).

## Roadmap

Here are some of the bigger items that are coming up now that I have more time:

- [remove more sigils from the language](https://github.com/ziglang/zig/issues/1023)
- [add tuples and remove var args](https://github.com/ziglang/zig/issues/208)
- [self-hosted compiler](https://github.com/ziglang/zig/issues/89)
- [http server and http client based on async/await](https://github.com/ziglang/zig/issues/910)
- [decentralized package manager](https://github.com/ziglang/zig/issues/943)
- [generate html documentation](https://github.com/ziglang/zig/issues/21)
- [hot code swapping](https://github.com/ziglang/zig/issues/68)

I can't wait to make significant progress on these items. That said, I will also dedicate time each day for bug fixes and writing documentation. Every weekday except Friday, this will be my wakeup process:

1. Make tea
2. Fix a bug
3. Write some documentation
4. Deal with all open pull requests, whether that is by merging, rejecting, requesting changes, or solving the use case a different way.
5. Proceed with working on a big feature item.

On Fridays, I will spend the day working on a project other than Zig, but one that is implemented in Zig. For example:

- Rewrite [Groove Basin](https://github.com/andrewrk/groovebasin) in Zig.
- Work on my [game that the raspberry pi boots directly into](https://github.com/andrewrk/clashos) (this explores Zig's ability to be used for embedded and Operating System development)
- Start reworking my [digital audio workstation](http://genesisdaw.org/) in Zig.

I'm beyond excited to get Zig to a place where it can reasonably be used for high quality and practical software projects. My goal is to make Zig so useful and practical that people will find themselves using it without intending to.

And so it comes down to this - will you fund my efforts?

## Why Donate to Zig?

Consider the basic premise of a for-profit business:

- Customers pay currency in exchange for goods or services.

If all goes well, both parties benefit from the exchange. But for-profit businesses are motivated, at the end of the day, not by customer satisfaction, but by profit.

It's fine, this is how the world works. But it's not how Zig works.

Zig is created by the open source community, for the open source community. One of our main [tenets](https://ziglang.org/documentation/master/#Zen) is

> Together we serve end users.

The Zig project is a public service, entirely motivated by improving the technical landscape of open source tools. There is no board to please, no stock shareholders demanding higher quarterly earnings, no office politics or career progressions on the line. 100% of donations I receive go towards paying rent, buying food, and generally attempting to live a modest, but healthy life.

[Will you pledge $5/month?](https://github.com/users/andrewrk/sponsorship)

Thanks for reading my blog post.
