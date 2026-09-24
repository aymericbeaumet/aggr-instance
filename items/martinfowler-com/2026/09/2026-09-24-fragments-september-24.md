---
title: 'Fragments: September 24'
link: https://martinfowler.com/fragments/2026-09-24.html
source: martinfowler-com
published: 2026-09-24T15:35:00Z
updated: 2026-09-24T15:35:00Z
first_seen: 2026-09-24T15:55:53.597467136Z
authors:
- Martin Fowler
content: extracted
html: 2026-09-24-fragments-september-24.html
preview:
  file: 2026-09-24-fragments-september-24.preview-fbde5f67ca0e.webp
  width: 256
  height: 128
  color: '#301b59'
images:
- source: https://martinfowler.com/fragments/fragments-thumb.png
  original:
    file: 2026-09-24-fragments-september-24.image-1f897b24c40a.png
    width: 600
    height: 300
  color: '#070d4a'
---

Rob Bowley is “flipping tables in his head” with [anger at the current media coverage of the danger of AI killing us all](https://blog.robbowley.net/2026/09/18/the-ai-threat-is-real-it-just-isnt-the-one-in-the-headlines/)

> The risk I’m worried about isn’t a future machine deciding to wipe us out. It’s today’s AI, being wired into everything, carelessly and fast.

Cyber attacks have already cost millions of dollars in lost economic output, often without AI being involved at all. Bowley feels the push to slow down AI is distracting us from the problems that are lurking with current technology. Too often agents are deployed in situations where they include the [Lethal Trifecta](https://martinfowler.com/articles/agentic-ai-security.html#lethal-trifecta), opening up a gaping security hole.

> What we really need to slow down on is wiring it all up to everything. Not because of what the models might become, but because nobody has worked out how to do this safely yet. We are building on something we don’t know how to contain, and shipping it to everyone while we work it out.

* * *

I ran into Nikita Prokopov’s post: [I am sorry, but everyone is getting syntax highlighting wrong](https://tonsky.me/blog/syntax-highlighting/). His core complaint is about color themes that give every different code element a unique color.

> if everything is highlighted, nothing stands out. Your eye adapts and considers it a new norm: everything is bright and shiny, and instead of getting separated, it all blends together.

He recommends using an absolute minimum of colors, in his case four: string, constants, comments, and top-level definitions. That’s not far off my approach, where I’m also careful to use muted colors for things that shouldn’t stand out, and bright colors for things that should (primarily function names when they are defined).

It’s common for color schemes to mute comments so they are easily skipped. He agrees that this good when there is excessive commenting, but when comments are used properly they are important so need bright highlighting. I also like his suggestion to use background colors for light mode work. I use light mode, and that’s a tip I should try out.

With agentic programming, lots of folks are reading more code than ever. Careful use of color can do much to make that easier.

* * *

Like many folks whose remaining hair is getting gray, I’ve been rolling my eyes about all this talk about Forward Deployed Engineers, as much of it involves breathlessly relating what so many of us have been advocating for decades. I did find this recent [post by Vinoo Ganesh](https://www.latent.space/p/forward-deployed-engineer-best-practices) interesting, as he’s deep in this trend, including a chunk of time at Palantir, who may be patient zero for FDEs.

If you know me at all, you’ll not be surprised by my lack of surprise at this observation:

> A few months ago, a16z launched the Forward Deployed Engineer Fellowship and I was nominated as one of the fellows, alongside a handful of people I used to work with. It’s a great program and I’ve enjoyed so many of the conversations. Last week I went to my first fellow dinner in SF.
>
> Around the table were FDEs from Snowflake, Anthropic, and a number of startups I’d been reading about, and over the course of the evening it became clear that we were all using the same two words (forward deployed) to describe jobs that had almost nothing in common. In one part of the conversation an FDE was a sales engineer who joined ‘the second call,’ somewhere else it was a quota-carrying rep who could write Python, and a few seats down it was closer to a consultant with a laptop and a statement of work, brought in to deliver something the product couldn’t.

Ganesh goes on to explain his view of what an FDE should do, and it’s all sensible stuff (albeit written with rather more LLM-voice than I would prefer). He says the FDEs job is to understand the business, to “collect nouns and verbs”, which mirrors what the [Domain-Driven Design](https://martinfowler.com/bliki/DomainDrivenDesign.html) folks have been doing since before Eric wrote the blue book.

Despite all my eyeball rolling at this, the FDE meme is pushing for something valuable. Yes, it’s easy for me to remark that it’s nothing more than the [Agile Manifesto’s principle](https://agilemanifesto.org/principles.html) that “Business people and developers must work together daily throughout the project”, or the desire to co-locate users and developers which my colleagues have been championing for all of this century. I’ve argued for decades that the biggest issue in software development is the communication between developers and the folks that benefit from software, and thus we need to focus on bridging the [yawning crevasse of doom](https://martinfowler.com/videos.html#y2007-crevasse). But despite all this, we haven’t had much success, so I think it’s important that a new generation of pundits try again, with some different framing, names, and slogans.

Ganesh’s perspective is not a custom software developer’s point of view, but rather a product - or more strictly - platform team’s. The FDE is a developer who “sits with” their users, applies customizations - but importantly - feeds these changes back to the core platform to decide whether the platform should be enhanced for everyone else.

> Keeping the customer happy is a real job and a good one. It belongs to solutions architects, who are rightly measured on it. The forward deployed engineer is there to turn what the field teaches into the thing every future customer gets. An FDE engagement that ends with one delighted account and nothing changed upstream has failed at the only thing the role exists for. You got the context and you spent it locally.
