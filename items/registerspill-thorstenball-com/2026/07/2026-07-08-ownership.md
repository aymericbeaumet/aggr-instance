---
title: Ownership
link: https://registerspill.thorstenball.com/p/ownership
source: registerspill-thorstenball-com
published: 2026-07-08T15:06:42Z
updated: 2026-07-08T15:06:42Z
first_seen: 2026-09-19T21:30:23.395188495Z
authors:
- Thorsten Ball
summary: Thoughts on ownership I sent to the Amp team in internal note
content: extracted
html: 2026-07-08-ownership.html
preview:
  file: 2026-07-08-ownership.preview-6ee8efa2addb.webp
  width: 256
  height: 173
  color: '#e7e7e7'
images:
- source: https://substack-post-media.s3.amazonaws.com/public/images/3f40d736-acff-4169-9051-4d7c1d1b7ded_2026x1372.png
  original:
    file: 2026-07-08-ownership.image-5e01685d55b6.png
    width: 2026
    height: 1372
  color: '#e6e6e6'
- source: https://substackcdn.com/image/fetch/$s_!uim-!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F450d3253-cf06-47ac-ad2e-688eef131f68.tif
  original:
    file: 2026-07-08-ownership.image-a65a61cacd35.png
    width: 693
    height: 22
  variants:
  - file: 2026-07-08-ownership.image-4ab39da1c88d.webp
    width: 320
    height: 10
  - file: 2026-07-08-ownership.image-9f9ab3aa9064.webp
    width: 640
    height: 20
  - file: 2026-07-08-ownership.image-13c4e42c2f3b.webp
    width: 693
    height: 22
  color: '#000000'
- source: https://substackcdn.com/image/fetch/$s_!__gY!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F394a7c97-4378-4e3d-9072-16d31ec47f61.tif
  original:
    file: 2026-07-08-ownership.image-f7facc04885c.png
    width: 717
    height: 34
  variants:
  - file: 2026-07-08-ownership.image-d94125c17f00.webp
    width: 320
    height: 15
  - file: 2026-07-08-ownership.image-949c8622b79f.webp
    width: 640
    height: 30
  - file: 2026-07-08-ownership.image-bab5735e4726.webp
    width: 717
    height: 34
  color: '#000000'
- source: https://substackcdn.com/image/fetch/$s_!jAX5!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4b50d509-3c6c-4db1-aea2-8f22bc60117c.tif
  original:
    file: 2026-07-08-ownership.image-ca887316e618.png
    width: 780
    height: 26
  variants:
  - file: 2026-07-08-ownership.image-6b7c6f796880.webp
    width: 320
    height: 11
  - file: 2026-07-08-ownership.image-d5a27565b12f.webp
    width: 640
    height: 21
  - file: 2026-07-08-ownership.image-57dc5bdc66eb.webp
    width: 780
    height: 26
  color: '#000000'
---

*The following is an internal Slack message I sent to my Amp teammates after I had a conversation with one of them about ownership. It’s only lightly edited.*

*I [shared it before](https://x.com/thorstenball/status/2066907538499506349) , but not here, because I didn’t think too much of it. Then today, someone said [their CTO shared my post with them](https://x.com/thorstenball/status/2074775146049474785) and I thought: well, now I have to put it in the newsletter, don’t I? So here we are.*

*Below, after the Slack post, I added some thoughts on juniors on how I see this advice applying to them.*

[![](https://substackcdn.com/image/fetch/$s_!uim-!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F450d3253-cf06-47ac-ad2e-688eef131f68.tif)](https://substackcdn.com/image/fetch/$s_!uim-!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F450d3253-cf06-47ac-ad2e-688eef131f68.tif)

Just had a (great) conversation about ownership and engineering here and I realized that I often use the phrase “ownership” or allude to it, but haven’t explained what “ownership” means to me in a while.

So, ownership.

If I ask you “can you own this?” or “can you take care of this?” or “are you on it?” — what I’m doing is I’m asking you to *own* it, to own the solution of a problem from end to end. From “we have a problem” to “we don’t have to think about it again.”

That means, when you say that you’re owning something, the expectation is that you…

- Think about what the problem actually is. Maybe you already have a solution in mind, without having thought about what we’re actually trying to solve here. Maybe you think “the problem is that we need to migrate from using X to using Y”, but that’s not a problem, that’s a solution. The problem is likely something like “performance is bad”, “it’s not stable”, “it fails for customer x”. Maybe there’s other possible solutions to that? Think about those. What are the tradeoffs? What’s the best solution to go with considering these tradeoffs?

- Think about edge cases. What are they? Which ones are important? Which ones can we ignore?

- Think about failures. Network failures are a given, for example. How do we handle them? Retry? Well, how often? How long?

- Think about data flow. How much data is involved here? Does data need to be migrated? Cleaned up? How can I get my hands on data to properly test this? What invariants are in the data? What assumptions do I have about the shape of the data that I haven’t confirmed yet?

- Think about how you’d test this. How can I know that what I built is correct or not? Are tests enough? Do I need to manually poke at things? Is the difference visible on a screenshot or in a video?

- How would we announce this? How do we communicate it? Can you picture it? How does it fit into the larger picture of our roadmap? Questions or concerns in that area — push back! ask!

- Do the work, with precision, with care, with a sense of urgency, with calmness. Do not half-ass things. Before you merge, ask yourself: am I proud of this? would I show this to John Carmack and say “here’s what I built, under these constraints, with these tradeoffs?”

- Test it manually. Yes, there’s automated tests. But in 99% of cases you can manually test or confirm that what you built works: you can run it yourself, you can ask an agent to run through test scenarios, you can poke at the data before and after, you can take screenshots, you can make a demo. Are you sure that what you did actually solves the problem?

- Make sure it lands in production and *works in production*. Is it deployed? Did the deploy fail? Do you need to activate a feature flag? Does the feature flag work? Can you use it in production? Can you confirm it’s actually deployed?

- If you think your colleagues needs to know about this change, because it’s new feature they should all test, or it’s a new convention in codebase, or maybe it’s a tricky thing everybody needs to be aware of, or something else: let them know! Do not underestimate peripheral vision: knowing that person X yesterday changed the behavior of how Z works might save person Y three hours of debugging today when a bug report related to Z comes in.

- Do customers need to know? Who reported the bug? Who’s blocked? Let them know.

- Does the world need to know? Announce that it’s out.

- Are there follow-ups? Do you need to check on what you shipped in the logs? A week later maybe?

Yes, that’s a lot. And there’s actually more, because I’m sure I forgot some stuff.

But that’s how you build a product in a small team. We don’t have PMs, we don’t have a Q&A department. We’re small, but we’re *great*, we can do all of that.

And it’s always okay to ask for help, it’s okay to ask questions, it’s okay to redo things and triple-check. What’s not okay is to implicitly assume that someone else will do the things here that you haven’t thought about.

[![](https://substackcdn.com/image/fetch/$s_!__gY!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F394a7c97-4378-4e3d-9072-16d31ec47f61.tif)](https://substackcdn.com/image/fetch/$s_!__gY!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F394a7c97-4378-4e3d-9072-16d31ec47f61.tif)

*“How does this apply juniors? You can’t expect them to really do all of that?”*

I’ve been asked these questions, or variations of them, after I shared the thoughts above and here’s my answer.

I do *not* expect juniors to *do* all of these things right away. But I would expect them to read through the list and *aspire* to one day be able to do all of these things. Until then, they can and should ask for help.

In fact, I don’t expect *anybody* to *always* do *all of these things* for *everything*. It’s a mental checklist of things to consider — problem, edge cases, tradeoffs, deployment, customers, messaging, … — but for quite a few things there aren’t edge cases to consider. Or big tradeoffs to weigh. Or deployment is a solved problem. And maybe someone else does the messaging for you.

And I imagine that most of these things you shouldn’t even consider when you work in a company with, say, 5000 employees. I’ve never worked in a company that large, only startups, so I can’t speak to how to successfully ship a software feature at Apple, end to end.

But when you work in a small company in which there’s only a single department, when you want to build things you’re proud of, when you work with me and you say own something, I expect you to keep these things in mind and run through them before you declare something as done.

[![](https://substackcdn.com/image/fetch/$s_!jAX5!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4b50d509-3c6c-4db1-aea2-8f22bc60117c.tif)](https://substackcdn.com/image/fetch/$s_!jAX5!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4b50d509-3c6c-4db1-aea2-8f22bc60117c.tif)

No posts
