---
title: We know 15k websites are infected. how do we tell them?
link: https://scotthelme.co.uk/we-know-15-579-websites-are-infected-how-do-we-tell-them/
source: hnrss-org
published: 2026-11-06T18:13:35Z
updated: 2026-11-06T18:13:35Z
first_seen: 2026-11-06T23:27:52.101188226Z
authors:
- speckx
labels:
- javascript
- report uri
- threat intelligence
content: extracted
html: 2026-11-06-we-know-15k-websites-are-infected-how-do-we-tell-them.html
remote_preview:
  url: https://storage.ghost.io/c/ee/88/ee889f88-37ef-43e5-9180-f9b88ee6261d/content/images/size/w1200/2026/11/Responsible-Disclosure-at-Scale.png
---

We've identified 15,579 websites that are currently infected with malicious JavaScript and between them, we conservatively estimate that they receive more than 135 million visits every month.

We know they're infected, and we have the evidence to back that up, but there's a problem: how on Earth do we tell them all?

As our Threat Intelligence capabilities at Report URI have grown, identifying compromised websites has become increasingly efficient. Unfortunately, responsible disclosure hasn't kept pace, and we're now finding infections considerably faster than we could ever hope to notify the affected organisations.

I genuinely don't know how to solve this problem, so I'm asking for help.

## What attacks we're finding

At the core of it, we're finding websites that are running JavaScript that they should absolutely not be running! For some examples of how nasty this can be:

- Credit card skimmers - often known as '[Magecart](https://blog.report-uri.com/tag/magecart/?utm_source=scotthelme.co.uk)', they target payment pages and skim full card details to send them to the attacker.
- [ClickFix](https://blog.report-uri.com/tag/clickfix/?utm_source=scotthelme.co.uk) attacks - designed to trick users into completing a "prove you're human" challenge and instead installing malware on their device.
- Device exploitation - loading malware to compromise specific versions of iOS and install spyware, like [DarkSword](https://blog.report-uri.com/tag/darksword/?utm_source=scotthelme.co.uk) or [Coruna](https://blog.report-uri.com/tag/coruna/?utm_source=scotthelme.co.uk).
- Redirects - these can send the visitor anywhere, including fake login pages to phish details, device security 'warnings', scams, and much more.

All of these are bad scenarios for the innocent visitor, and attackers change their payloads depending on their objective.

## How these sites are compromised

There are many ways an attacker can get malicious JS onto a site, but here are the main categories that we group the initial vectors into from what we're seeing:

- Direct compromise - typically a WordPress vulnerability or similar, the site's own JS files are modified to include the malicious JS.
- Supply chain - a third-party script file that a site loads is infected with malware, and is then loaded on to the site, exposing all visitors.
- XSS - good old traditional XSS, a vulnerability in the site itself that allows JS to be injected.
- [DeadTag](https://blog.report-uri.com/deadtag-how-forgotten-website-tags-became-a-delivery-network-for-iphone-spyware/?utm_source=scotthelme.co.uk) - a threat group we named recently, buying expired domains that sites still load JS from, reviving them, and serving malware.

Unfortunately, there are a lot of different ways that this can go wrong, as we're seeing.

## The scale of the problem

Right now, we have **15,579 unique sites** that are infected with one of the malicious JS attacks listed above. Those are the sites where there is no doubt in our mind, and the list of sites where we have some suspicion but haven't got to 100% confidence is thousands more. Take September as an example, because we haven't quite finished processing everything from October yet, and this is the tally for new sites that we identified as being infected:

![](https://storage.ghost.io/c/ee/88/ee889f88-37ef-43e5-9180-f9b88ee6261d/content/images/2026/11/image.png)

These are the sites that, as of today, are still infected. We remove sites once they clear their own infection as we have no concern about trying to notify them, so there were more than this.

In terms of traffic volumes, it's quite surprising how quickly this adds up. None of the sites in the list are global leaders, but we do have a huge presence of regional leaders. For example, if we look at `.fr`, `.es` and `.de` sites, some of them are really highly ranked sites in those countries, but they barely crack the Top 1 Million globally. Being a top site in France still gets you quite a hefty amount of traffic it turns out. Using various sources of data, and being ultra-conservative with our estimates, our list of currently infected sites is comfortably receiving a combined 135 million visits a month. That's a lot of exposure.

## So, what do we do?

We continue to reach out directly to those sites that we identify as particularly high impact and try to notify them, though this is happening on an admittedly small scale. The process of responsible disclosure consumes a lot of resources, use of [security.txt](https://scotthelme.co.uk/say-hello-to-security-txt/?utm_source=scotthelme.co.uk) is low, and the vast majority of our emails and messages go unanswered. It's simply not possible for us to scale this up.

As these are sites we're crawling and identifying ourselves, we have no contact information like we would for a customer. So, as things currently stand, that's where we are, and I'd like to hear suggestions on what we can do.

If you're from an NCSC/CERT, especially in Europe where we seem to be finding a lot of impacted sites, please reach out to `intel@report-uri.com` and we'd be happy to provide a list of infected sites in your region. Hopefully you would be able to have more success in notifying the impacted sites than we would.

If you're an organisation and believe having commercial access to our threat intelligence feeds would help you protect your own customers, you can also reach out to `intel@report-uri.com` for details.

Back to the original problem, though, and it pains me to say it but I don't have a clear idea on how we can try to have more of a positive impact. If you think there's something you can help with, an idea you'd like to put forward, or a suggestion on what we can do with all of this data, please drop by the comments below and I'd be happy to hear from you.

We have intelligence that could help protect millions of website visits, and we need help to turn it into action.

### Update: 6th Nov 2026 21:36 UTC

A lot of questions have been raised across various social channels and especially the [HN thread](https://news.ycombinator.com/item?id=50396064&ref=scotthelme.co.uk), so I thought I'd answer them in one central place:

**Why hasn't the list of sites been published?**\
There are a few reasons:

- I didn't want this to be a 'name and shame' as the sites on that list are, after all, victims of attacks. Someone broke into their site, and the owners probably have no idea.
- If the list is widely disseminated and absorbed into blocklists, trying to get your domain of those lists afterwards is close to impossible. For a small business, that could be devastating.
- Many of these websites were likely infected because of an existing vulnerability that was exploited in something like WordPress, Magento or similar. Handing out a list of viable targets does not seem like a good idea.

**Why not use the WHOIS contacts?**\
Since GDPR back in 2018, I've found that data to be mostly useless. What you get is either nothing, a registrar web form, or a relay address such as the Namecheap or GoDaddy privacy mailboxes.

**Why not contact the hosting providers?**\
Because so many of these sites are behind a CDN provider or other proxy service, so the IP resolution doesn't give any indication of who the hosting provider is to contact them.

**Why not just send an email to every website?**\
I could probably try to automate that, but to what email address? I'd have to hit a selection of common ones, info@, support@, abuse@, maybe others? There are also close to 16,000 of them, have you ever tried to send 16,000 emails? The manually curated emails have almost no response rate so a spammy mailshot seems unlikely to help, and could also trash my email reputation!

Have you enjoyed this post or found it helpful? \
☕️ Consider [buying me a coffee](https://donate.stripe.com/4gMeVd3xm0sOcRzexF93y00) to say thanks! \
🔔 Subscribe for [free notifications](https://scotthelme.ghost.io/#/portal/signup) when I publish! \
🤩 Become a [member](https://scotthelme.ghost.io/#/portal/signup) and support my content!

Tags: [Threat Intelligence](https://scotthelme.co.uk/tag/threat-intelligence/), [Report URI](https://scotthelme.co.uk/tag/report-uri/), [javascript](https://scotthelme.co.uk/tag/javascript/)
