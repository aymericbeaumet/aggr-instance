---
title: Rate limits on GitLab.com are changing
link: https://about.gitlab.com/blog/rate-limit-change-2026/
source: hnrss-org-frontpage
published: 2026-09-17T15:33:54Z
updated: 2026-09-17T15:33:54Z
first_seen: 2026-09-17T20:33:50.623063168Z
authors:
- darkwater
summary: 'Article URL: https://about.gitlab.com/blog/rate-limit-change-2026/ Comments URL: https://news.ycombinator.com/item?id=49742353 Points: 118 # Comments: 95'
content: extracted
html: 2026-09-17-rate-limits-on-gitlab-com-are-changing.html
preview:
  file: 2026-09-17-rate-limits-on-gitlab-com-are-changing.preview-67f668639bad.webp
  width: 256
  height: 134
  color: '#5c4286'
images:
- source: https://res.cloudinary.com/about-gitlab-com/image/upload/v1785336708/ww4mlighwlvvzgnmbe6w.png
  original:
    file: 2026-09-17-rate-limits-on-gitlab-com-are-changing.image-549376371892.jpg
    width: 1800
    height: 945
  color: '#332c68'
---

GitLab.com hosts millions of projects for teams of every size that need a platform they can rely on. Demand is climbing quickly, and we expect platform load to grow several times over this year. Predictable limits are what keep GitLab.com fast for everyone on it, including the automation and agent workloads teams are building on the platform.

To hold that as we scale, we're updating how rate limits work. Starting October 19, 2026, rate limits on GitLab.com will align with your subscription tier. Free accounts and unauthenticated requests happen first, on October 19. Premium and Ultimate move in January 2027.

## [](https://about.gitlab.com/blog/rate-limit-change-2026/#what-is-changing)What is changing

**Limits align with your subscription.** Free, Premium, and Ultimate subscription plans get their own limits, applied per user and per top-level group. Free takes effect October 19; Premium and Ultimate in January 2027.

**Signing in gets you the full limit.** An authenticated request is governed by your subscription plan below. A request that arrives with no credentials gets 60 requests per hour per IP address.

The per-plan limits are published in the [rate limits documentation](https://docs.gitlab.com/user/gitlab_com/rate_limits/).

## [](https://about.gitlab.com/blog/rate-limit-change-2026/#what-happens-on-october-19)What happens on October 19

There will be two preview windows for Free and unauthenticated traffic, on October 7 and October 14 from 15:00 to 19:00 UTC. Signed-in Premium and Ultimate requests are not affected, since those limits do not change until January. Unauthenticated requests are capped no matter where they come from, including automation running against a paid account without credentials. A preview window (engineers call these brownouts) is a short, planned window where we switch the new limits on and then switch them back off. Nothing else about the service changes while it runs. The point is to give you a real look at how your own workloads behave under the new limits, weeks before they apply for good.

On **October 19** the new limits take effect.

We set these limits by looking at how GitLab.com is actually used. Almost all users are already inside the new limits and won't notice any change. We also looked at what similar platforms allow. The Free limit and the anonymous allowance match the industry norm, while Premium and Ultimate are more generous, at levels other platforms reserve for their enterprise tiers or don't publish at all.

## [](https://about.gitlab.com/blog/rate-limit-change-2026/#if-youre-close-to-a-limit)If you're close to a limit

If you find that you are nearing a limit, authenticate your requests. It's usually a small change: Invoking a [personal access token](https://docs.gitlab.com/api/rest/authentication/), an OAuth token, or the CI/CD job token all move a request off the anonymous 60 requests per hour and onto your plan's limits, which are much higher.

Next, look at how you're calling the API. Batching, caching, and pagination go a long way, and polling in a tight loop burns through your allowance fast. When you do cross a limit, you get an `HTTP 429` back with a `Retry-After` header saying how long to wait, so a client that reads its own response headers mostly fixes itself. Backing off exponentially recovers faster than retrying immediately.

[Upgrading to Premium or Ultimate](https://about.gitlab.com/pricing/) increases the limits, too, per user and per top-level group.

If you need a higher limit on an ongoing basis, we are working on a way to purchase capacity above the standard plan limits, with details coming later this year. If that sounds like you, reach out to your account team or email `limits@gitlab.com` and tell us what you need.

## [](https://about.gitlab.com/blog/rate-limit-change-2026/#what-changes-and-what-doesnt)What changes and what doesn't

These limits are set so no single workload can slow the platform for everyone else. Ordinary signed-in work isn't the target, and, for almost all users, a normal day looks identical. Browsing the UI, working in your editor, pushing and pulling with git, and running CI/CD within your plan all carry on exactly as they do today. Some heavy automation and a small number of Free-tier workloads will reach the new ceilings.

What doesn't change:

- You can always reach and export your own data and repositories.
- GitLab Self-Managed and GitLab Dedicated limits stay with your operator. This is only a GitLab.com change.
- We'll notify you before we make additional changes.

**A reminder:** Make sure to authenticate your requests to GitLab.com so your limits are higher.

## [](https://about.gitlab.com/blog/rate-limit-change-2026/#faq)FAQ

**How do I know whether this affects me?**\
 Compare your busiest minute against the [published limits for your plan](https://docs.gitlab.com/user/gitlab_com/rate_limits/#rate-limits-by-plan). Most customers are not close. The quickest signal in the meantime is the `RateLimit-Remaining` header on your API responses, which tells you how much of your current window is left, and we are building a view in the product for release later this year that shows your usage against your plan's limits.

**My project is public and busy. What are my options?**\
 Three things help. Ask the automation that calls your project to sign in, which moves it onto its own limits rather than the anonymous allowance. Make the project private if the traffic is not coming from the audience you built it for, which stops anonymous callers reaching it at all. Or [upgrade to Premium or Ultimate](https://about.gitlab.com/pricing/) for much higher limits.

**What if I am a member of several top-level groups?**\
 Your user limit will be the highest subscription tier available to you. If you are a member of an [Ultimate](https://about.gitlab.com/pricing/ultimate/) group, you will have access to the Ultimate limit.

**What happens when I hit a limit?**\
 You get `429 Too Many Requests` with `RateLimit-*` headers and a `Retry-After`. Wait the interval it gives you, then retry.

**My integration genuinely can't authenticate. What now?**\
 Reach out to us at `limits@gitlab.com`. There are legitimate anonymous patterns, a public status badge being the obvious one. If you are concerned that an integration you own may be affected, contact us.

**Does this apply to GitLab Self-Managed or GitLab Dedicated?**\
 No. This is a GitLab.com-only change.

## [](https://about.gitlab.com/blog/rate-limit-change-2026/#additional-resources)Additional resources

- [Rate limits by plan](https://docs.gitlab.com/user/gitlab_com/rate_limits/#rate-limits-by-plan)
- [Authenticating your API requests](https://docs.gitlab.com/api/rest/authentication/)
- Questions, or you've encountered a case we haven't thought of? Contact your account team or email `limits@gitlab.com`.
