---
title: Thoughts on the Future of Web Browsers
link: https://sarahjamielewis.com/log/2026/future-of-web-browsers.html
source: lobste-rs-top-1w
published: 2026-09-19T22:27:02Z
updated: 2026-09-19T22:27:02Z
first_seen: 2026-09-21T11:04:21.981356257Z
authors:
- sarahjamielewis.com via orib
labels:
- browsers
summary: Comments
content: extracted
html: 2026-09-19-thoughts-on-the-future-of-web-browsers.html
---

Back in July, inspired by yet another AI integration into firefox, I wrote a [small thread on mastodon](https://mastodon.social/@sarahjamielewis/116959309031485270):

> If the independent, non-slop web has any future at all, then now must be the time for every firefox fork to commit to working together to maintain a hardfork isolated from Mozilla.
>
> It's a project that is too large to be handled by any small project alone (and maybe even all of them combined), but one that is too important to left under the guidance of an organization like Mozilla.
>
> Without such bold co-operation I fear we have already lost.
>
> Mozilla and, by extension, Firefox have laid out the direction they want to go - and have consistently moved in that direction over the last decade. There is no redemption arc there, they are not going to turn the ship around - and every week and month that goes by, Firefox gains more slop and drifts further from the visions that founded it.
>
> Projects like Tor Browser and Waterfox are painstakingly disabling / patching out the worst - but every release becomes more expensive, and things do slip through.

In the rest of the thread I laid out a rough vision for a ridiculous optimisitc plan, involving many forks coming together to maintain a base separate from Mozilla, perhaps utilizing the work already being done by Tor Project, or some other fork.

That thread, and subsequent conversations spawned [the base browser project](https://codeberg.org/basebrowserproject/organization). An attempt at a place people could gather and discuss ideas / direction.

Inspired by some of the momentum I set out to create a patch that removed all of the AI-integration code from Firefox, which resulted in a [ridiculous patch impacting 1605 files changed with 852297 deletions, totalling 37 megabytes](https://mastodon.social/@sarahjamielewis/116994970688711387).

Over the next few weeks, myself and a small team of volunteers worked on a few more patches, made changes to the giant AI patch, and worked out some scripts to compress the 37Mb down to a reasonable size, just under a megabyte (we did this by using gits existing irreverable-delete flag and some custom python to allow repatching the file). Big thanks to cliffmccarthy and gellge specifically, and to everyone else who contributed to testing/discussing the patches and the project.

## Current Status

Today, [base browser](https://codeberg.org/basebrowserproject/organization) features a set of patches, based around the current Firefox 153 ESR, that strip user hostile features completely out of the code base (as opposed to the common soft-fork approach of disabling these.)

I believe that deleting these features entirely is the correct approach for exactly the reasons that make it a pain - these features are large, and increasingly tightly integrated into the core browser. They also account for an increasingly percentage of the total firefox code base and quite frankly:

**I do not believe that these features should be anywhere near the core base of a web browser**

Now, I am well prepared to lose this battle. I do not believe that there is enough funding, or enough developer effort to maintain something like this long term. Unless we all pool our efforts into making a base like this possible.

This effort has a foundation of shifting sands, Firefox is already moving far beyond simple AI integrations, [imagining a future where the entire browser context is a "smartwindow" built as much for an third-party AI agent](https://mastodon.social/@sarahjamielewis/117282061166655756) as it is for people.

I don't want the web to go in that direction and, frankly, I cannot follow.

As I have also said, [I am not the right person to do something like this](https://mastodon.social/@sarahjamielewis/116966194548439681), but I at least needed to do something to try and make it happen.

If all that comes out of this is a few people learning how to build Firefox from source I'd consider it a win. If any project ends up using or adopting these patches I'd be ecstatic.

## Temporary Actions

Outside of attempting to patch away the most egregious integrations, I've also been exploring other avenues for reducing my reliance on firefox:

1. **Move as much offline as possible** - I've been playing with [openzim](https://wiki.openzim.org/wiki/OpenZIM) readers as a way to move some basic web browsing tasks away from the core web browser (openzim files tend to trend towards a minimal subset of HTML/CSS/JS that permits simpler browsers) and there already exsits a [growing collection of sites](https://browse.library.kiwix.org/) that I have moved to reading offline.
2. **Move to standlone applications where possible** - as someone who spend most of my time on a desktop computer, I much prefer standalone applications to web applications. In the last few months I've been experimenting more with apps for some of the services I cannot replace with offline readers e.g. mastodon. I've not yet found solutions for everything.
3. **Move to feed readers where possible** - In the same vein as above, feeds provide another way to interact with online systems without needing an entire browser.

## Doing More than Running Away

But even with all of that, there still exists the problem that these strategies exist to counter the prevailing narrative of slop-dominance, rather than as an inspirational act in-and-of themselves.

I don't want to spent my efforts, my time, my life, simply fighting to remaing in place. The reason I spent my youth with my head buried in programming books and my mind swimming in code was because I wanted to build things that matter, I wanted to understand the world better.

More so, I am convinced that future cannot arrive by taking the past, grinding it down and serving it, reheated, as visionless slop.

### Finding that vision, is not without challenge:

Over the last decade-plus web standards have become over-saturated to the priority of commercial interests.

To maintain a web browser to any kind of quality assurance and security you need a well funded team - or rely on one in your dependency tree.

Money rarely comes without strings attached and, on every page load, you can feel that tension between the pure philosophical vision and expected returns.

### But we've been somwhere like this before...

There was a time in the early 2000s when Firefox triggered a browser renascence and there was a lot of excitement about what a "browser" could be...feeds, blogging integration, collective tagging, open comments....

The original spirit that the web should be as writable as it was readable, extended to shareable.

And in some way, shaped by economics and technology, we got an approximation of that vision..shrinkwraped and sanitized.

I often think about the visions put forth by browsers like Amaya and, much later Flock. That a browser should be a tool for creation as much as consumption.

I still hold that vision in my heart, over the few years I have [experimented with building little browsers](https://mastodon.social/@sarahjamielewis/112820652601249456) that support gemini (the [protocol](https://geminiprotocol.net/), not the llm \*sigh\*) and rss and other web technologies unpolluted by what has become of modern standards.

I'm unconvinced that is \*the\* future, but maybe it could be *a* future.

To borrow an old call to arms that I have found some inspiration in in recent times...**we need new noise**.

I'm still trying to find it.
