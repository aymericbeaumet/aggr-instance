---
title: Redis Renamed to Redict
link: https://andrewkelley.me/post/redis-renamed-to-redict.html
source: andrewkelley-me
published: 2024-03-22T20:32:03Z
updated: 2024-03-22T20:32:03Z
first_seen: 2026-09-19T21:30:23.395188495Z
summary: 'Redis Renamed to Redict Redict was originally created by Salvatore Sanfilippo under the name "Redis". Around 2018 he started losing interest in the project to pursue a science fiction career and gave stewardship of the project to Redis Labs. I think that was an unfortunate move because their goal is mainly to extract profit from the software project rather than to uphold the ideals of Free and Open Source Software. On March 20, 2024, they changed the license to be proprietary, a widely unpopular action. You know someone is up to no good when they write "Live long and prosper 🖖" directly above a meme of Darth Vader. What are the actual problems with these licenses? In short summary, the licenses limit the freedoms of what one can do with the software in order for Redis Labs to be solely enriched, while asking for volunteer labor, and having already benefitted from volunteer labor, that is and was generally offered only because it enriches everybody. SSPL is BAD Are they allowed to do this? All the code before the license change is available under the previous license (BSD-3), however it is perfectly legal to make further changes to the project under a different license. This means that it is also legal to fork the project from before the license change, and continue maintaining the project without the proprietary license change. The only problem there would be, the project would be missing out on all those juicy future contributions from Redis Labs... wait a minute, isn''t the project already done? Redict is a Finished Product Redict already works great. Lots of companies already use it in production and have been doing so for many years. In Why We Can''t Have Nice Software, I point out this pattern of needless software churn in the mindless quest for profit. This is a perfect example occurring right now. Redict has already reached its peak; it does not need any more serious software development to occur. It does not need to pivot to AI. It can be maintained for decades to come with minimal effort. It can continue to provide a high amount of value for a low amount of labor. That''s the entire point of software! Redict does not have any profit left to offer. It no longer needs a fund-raising entity behind it anymore. It just needs a good project steward. Drew DeVault is a Good Steward Drew is a controversial person, I think for two reasons. One, is that he has a record of being rude to many people in the past - including myself. However, in a similar manner as Linus Torvalds, Drew has expressed what I can only interpret as sincere regret for such interactions, as well as a pattern of improved behavior. I was poking through his blog to try to find example posts of what I mean, and it''s difficult to pick them out because he''s such a prolific writer, but perhaps this one or maybe this one. I''m a strong believer in applying the best game theory strategy to society: people should have consequences for the harm that they do, but then they should get a chance to start cooperating again. I can certainly think of "cancelable" things I have done in the past, that I am thankful are not public, and I cringe every time I remember them. Secondly, and I think this is actually the more important point, Drew has been an uncompromising advocate of Free and Open Source Software his entire life, walking the walk more than anyone else I can think of. It''s crystal clear that this is the driving force of his core ideology that determines all of his decision making. He doesn''t budge on any of these principles and it creates conflicts with people who are trying to exploit FOSS for their own gains. For example, when you call out SourceGraph you basically piss off everyone who has SourceGraph stock. Do enough of these callouts, and you''ve pissed off enough people that there''s an entire meme subculture around hating you. Meanwhile, Drew created and maintained wlroots and Sway, successfully appointing a successor maintainer to carry the torch, and runs a sustainable business on top of Free and Open Source Software. SourceHut has a dependency on Redict, so it naturally follows that Drew wants to keep his supply chain FOSS. Redis is the Fork The only thing Redis has going for it, as a software project, is the brand name. Salvatore is long gone. The active contributors who are working on it are, like I said, pivoting to AI. Seriously, here''s a quote from The Future of Redis: Making Redis the Go-To for Generative AI we’re staying at the forefront of the GenAI wave Meanwhile, Redict has an actual Free and Open Source Software movement behind it, spearheaded by Drew DeVault, who has a track record of effective open source project management. In other words, Redict is the true spiritual successor to what was once Redis. The title of this blog post is not spicy or edgy; it reflects reality.'
content: extracted
html: 2024-03-22-redis-renamed-to-redict.html
preview:
  file: 2024-03-22-redis-renamed-to-redict.preview-ad8c26547ce5.webp
  width: 256
  height: 254
  alt: screenshot of the PR merging with many downvotes and darth vader
  color: '#dadbdc'
images:
- source: https://andrewkelley.me/img/redis-screenshot-light.png
  original:
    file: 2024-03-22-redis-renamed-to-redict.image-f348b6938361.png
    width: 915
    height: 909
  variants:
  - file: 2024-03-22-redis-renamed-to-redict.image-4bc50d9fc158.webp
    width: 320
    height: 318
  - file: 2024-03-22-redis-renamed-to-redict.image-c60438cbdd36.webp
    width: 640
    height: 636
  - file: 2024-03-22-redis-renamed-to-redict.image-8278cf9115a9.webp
    width: 915
    height: 909
  color: '#fcfcfc'
- source: https://andrewkelley.me/img/redis-screenshot-dark.png
  original:
    file: 2024-03-22-redis-renamed-to-redict.image-76bb956404b0.png
    width: 915
    height: 909
  color: '#0d1117'
---

[Redict](https://redict.io/) was originally created by Salvatore Sanfilippo under the name "Redis". Around 2018 he started losing interest in the project to pursue a science fiction career and gave stewardship of the project to [Redis Labs](https://en.wikipedia.org/wiki/Redis_\(company\)).

I think that was an unfortunate move because their goal is mainly to extract profit from the software project rather than to uphold the ideals of Free and Open Source Software. On March 20, 2024, they [changed the license to be proprietary](https://github.com/redis/redis/pull/13157), a widely unpopular action. You know someone is up to no good when they write "Live long and prosper 🖖" directly above a meme of Darth Vader.

![screenshot of the PR merging with many downvotes and darth vader](https://andrewkelley.me/img/redis-screenshot-light.png) ![screenshot of the PR merging with many downvotes and darth vader](https://andrewkelley.me/img/redis-screenshot-dark.png)

## What are the actual problems with these licenses?

In short summary, the licenses limit the freedoms of what one can do with the software in order for Redis Labs to be solely enriched, while asking for volunteer labor, and having [already benefitted from volunteer labor](https://github.com/redis/redis/pull/13157#issuecomment-2014737480), that is and was [generally offered only because it enriches *everybody*](https://news.ycombinator.com/item?id=39775468).

[SSPL is BAD](https://ssplisbad.com/)

## Are they allowed to do this?

All the code before the license change is available under the previous license (BSD-3), however it is perfectly legal to make further changes to the project under a different license.

This means that it is also legal to *fork* the project from before the license change, and continue maintaining the project without the proprietary license change. The only problem there would be, the project would be missing out on all those juicy future contributions from Redis Labs... wait a minute, isn't the project already *done*?

## Redict is a Finished Product

Redict already works great. Lots of companies already use it in production and have been doing so for many years.

In [Why We Can't Have Nice Software](https://andrewkelley.me/post/why-we-cant-have-nice-software.html), I point out this pattern of needless software churn in the mindless quest for profit. This is a perfect example occurring right now. Redict has already reached its peak; it does not need any more serious software development to occur. It does not need to [pivot to AI](https://redis.com/blog/the-future-of-redis/). It can be maintained for decades to come with minimal effort. It can continue to provide a high amount of value for a low amount of labor. That's the entire point of software!

**Redict does not have any profit left to offer**. It no longer needs a fund-raising entity behind it anymore. It just needs a good project steward.

## Drew DeVault is a Good Steward

[Drew](https://drewdevault.com/) is a controversial person, I think for two reasons.

One, is that he has a record of being rude to many people in the past - including myself. However, in a [similar manner as Linus Torvalds](https://lore.kernel.org/lkml/CA+55aFy+Hv9O5citAawS+mVZO+ywCKd9NQ2wxUmGsz9ZJzqgJQ@mail.gmail.com/), Drew has expressed what I can only interpret as sincere regret for such interactions, as well as a pattern of improved behavior. I was poking through his blog to try to find example posts of what I mean, and it's difficult to pick them out because he's such a prolific writer, but perhaps [this one](https://drewdevault.com/2022/05/30/bleh.html) or maybe [this one](https://drewdevault.com/2023/05/01/2023-05-01-Burnout.html). I'm a strong believer in applying [the best game theory strategy](https://en.wikipedia.org/wiki/Tit_for_tat) to society: people should have consequences for the harm that they do, but then they should get a chance to start cooperating again. I can certainly think of "cancelable" things I have done in the past, that I am thankful are not public, and I cringe every time I remember them.

Secondly, and I think this is actually the more important point, Drew has been an uncompromising advocate of Free and Open Source Software his entire life, walking the walk more than anyone else I can think of. It's crystal clear that this is the driving force of his core ideology that determines all of his decision making. He doesn't budge on any of these principles and it creates conflicts with people who are trying to exploit FOSS for their own gains. For example, when you [call out SourceGraph](https://drewdevault.com/2023/07/04/Dont-sign-a-CLA-2.html) you basically piss off everyone who has SourceGraph stock. Do enough of these callouts, and you've pissed off enough people that there's an entire meme subculture around hating you.

Meanwhile, Drew created and maintained [wlroots](https://gitlab.freedesktop.org/wlroots/wlroots) and [Sway](https://swaywm.org/), successfully appointing a successor maintainer to carry the torch, and runs [a sustainable business](https://sourcehut.org/) on top of [Free and Open Source Software](https://sr.ht/~sircmpwn/sourcehut/). SourceHut has a dependency on Redict, so it naturally follows that Drew wants to keep his supply chain FOSS.

## Redis is the Fork

The only thing Redis has going for it, as a software project, is the brand name. Salvatore is long gone. The active contributors who are working on it are, like I said, pivoting to AI. Seriously, here's a quote from [The Future of Redis](https://redis.com/blog/the-future-of-redis/):

> Making Redis the Go-To for Generative AI
>
> we’re staying at the forefront of the GenAI wave

Meanwhile, Redict has an actual Free and Open Source Software movement behind it, spearheaded by Drew DeVault, who has a track record of effective open source project management.

In other words, Redict is the true spiritual successor to what was once Redis. The title of this blog post is not spicy or edgy; it reflects reality.

Thanks for reading my blog post.
