---
title: The Counting House
link: https://steve-yegge.medium.com/the-counting-house-e11125c60ce1?source=rss-c1ec701babb7------2
source: steve-yegge-medium-com
published: 2026-10-27T15:15:01Z
updated: 2026-10-27T15:15:01.557Z
first_seen: 2026-10-27T16:00:47.207930047Z
authors:
- Steve Yegge
labels:
- ai-automation
- ai-customer-service
- customer-intent
- software-factories
content: feed
html: 2026-10-27-the-counting-house.html
remote_preview:
  url: https://cdn-images-1.medium.com/max/1024/1*ogwR3jwmNBc_nEdkNd37cg.jpeg
---

![](https://cdn-images-1.medium.com/max/1024/1*ogwR3jwmNBc_nEdkNd37cg.jpeg)\
Two very different ways of deploying AI: Wheelhouse and Counting House

I think a pattern is starting to emerge in enterprise AI-modernization, and I’ve been collecting the design ideas into an assembly I call the Counting House. This pattern adds AI to systems handling large volumes of human work, notably customer service departments, though the same pattern fits claims processing and other casework, with millions to tens of millions of contacts per year. This writeup is a postcard introduction to the Counting House, compared side by side with SDLC-side development, which it does not much resemble.

You can build one of these even if you don’t have your SDLC-side AI adoption sorted out yet. Once you have a Counting House, you can control and improve your cost, quality, and human ramp-up time, all using comparatively cheap (even open-weight) AI models.

In fact, chances are, you *don’t* have your SDLC-side AI adoption sorted out. The big companies I’m talking to (e.g. banks) tell me they are still struggling with AI in software development. They see promising but inconsistent results, non-replicability across teams, ballooning costs, internal cultural pushback, etc.

From where I stand, SDLC-side AI adoption has been an industry-wide struggle in 2026, and that will probably continue into 2027.

A Counting House uses classic data science and a little good old-fashioned engineering knowhow to put lower-tier models on hardened rails, supporting human workflows. And it lets you measure and report how well those models do against their human counterparts.

I will briefly review my own software factory, the Wheelhouse, which I run more or less like a shop of human employees. And then we’ll dive into the Counting House, which stands in stark contrast, putting models on the tightest track possible.

### Wheelhouse Recap

I’ve blogged about my [Wheelhouse](https://yegge.ai/essays/the-shape-of-things-to-come/) system, which is a successor to [Gas Town](https://steve-yegge.medium.com/welcome-to-gas-town-4f25ee16dd04), which was the very first software factory. Wheelhouse is far more sophisticated, and is now doubtless one of the top solo software factories on the planet, comfortably producing 2000–4000 commits a week on a light multi-machine setup. Its agents work around the clock: building, reviewing, deploying [game](https://ghosttrack.com/) features, negotiating on Slack with my game admins, interacting on Discord with players, exchanging emails with my accountant and my lawyers. It’s a whole startup in a box. I plan to open-source at least the skeleton of it.

Wheelhouse is aimed squarely at SDLC in the real world, where building meaningful new systems requires a fair amount of scope and freedom. It’s an exploratory process, and so Wheelhouse is by design an open system with [no sandboxing](https://yegge.ai/essays/fences-not-sandboxes/). There is structure: doctrine and policy, human gates, and just fourteen mechanical “fences” (programmatic denials). But my agents are first-class colleagues. They work under my own identity, signing their own names to the work, and they can see the whole system and act independently. It is YOLO mode scaled up to the size of a small company.

I’ve met with at least a dozen people running teams of agents in systems resembling factories, and every single one of them is completely different. I’m starting to see software factories as being like private armies, and soon, everyone will have one. How much you lock yours down is your call; I have the luxury of keeping mine fairly open. But this is yet another reason SDLC-side AI has been slow to take off. Software factories are hideously expensive, hard to learn, hard to maintain, and straight-up risky to hand that much scope inside a real enterprise. And the more you try to lock one down, the more you strike at the heart of what makes it productive in the first place: collaboration and exploration.

*Read the rest of the essay at* [*yegge.ai*](https://yegge.ai/essays/the-counting-house/)*.*

![](https://medium.com/_/stat?event=post.clientViewed&referrerSource=full_rss&postId=e11125c60ce1)
