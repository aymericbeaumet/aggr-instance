---
title: 'Fragments: August 18'
link: https://martinfowler.com/fragments/2026-08-18.html
source: martinfowler-com
published: 2026-08-18T15:04:00Z
updated: 2026-08-18T15:04:00Z
first_seen: 2026-09-04T12:14:41.802665207Z
authors:
- Martin Fowler
content: extracted
html: 2026-08-18-fragments-august-18.html
---

Part of the reason why I’m at Thoughtworks is because I’d like to see a software development organization founded on technical excellence as an example for the rest of the industry. The trouble is that I have little aptitude or inclination for the hard work of building such an organization. So I rely on working with people who are prepared to actually put the effort in. A key partner in all of this is [Rachel Laycock](https://www.linkedin.com/in/rachellaycock/), who is the global CTO of Thoughtworks.

Not just is she far better than me at running a technology organization, she’s also a keen observer and connector of ideas. I’ve been urging her to write these down, even if her busy schedule makes it difficult for her to compose them into something substantial.

Happily [she’s starting writing “Rachel’s Ramblings”](https://martinfowler.com/rachels-ramblings/)

> Fast, imperfect, thinking out loud. Naming ideas early rather than waiting until they’re fully formed. Because the reality is, most of what I do day to day isn’t answering known questions. It’s spotting patterns and asking questions we haven’t quite figured out yet.

 ❄                ❄                ❄                ❄                ❄

My colleagues in Europe are organizing [XConf Europe in London on September 11th](https://www.eventbrite.co.uk/e/xconf-europe-2026-london-tickets-1989895694589?aff=MF).

The sessions examine what happens when agentic systems meet compliance, how to run sovereign models, performance patterns in data migrations and how to safely navigate legacy codebases. Lu Wilson will give a keynote on ‘Jam-oriented programming’.

 ❄                ❄                ❄                ❄                ❄

Noah Smith recognizes the high usage of AI, and its impressive feats - but also that [there aren’t signs of massive productivity growth or job losses](https://www.noahpinion.blog/p/what-will-more-intelligence-actually). This may be the calm before the storm, but Smith thinks there may something else in play. He quotes a [metaphor from François Chollet](https://x.com/fchollet/status/2038069289643806957)

> One of the biggest misconceptions people have about intelligence is seeing it as some kind of unbounded scalar stat, like height. “Future AI will have 10,000 IQ”, that sort of thing. Intelligence is a conversion ratio, with an optimality bound. Increasing intelligence is not so much like “making the tower taller”, it’s more like “making the ball rounder”. At some point it’s already pretty damn spherical and any improvement is marginal.

The thought here is that intelligence in the sense that we know it, isn’t something where there’s a lot of room for massive improvement. That doesn’t mean AI won’t be “smarter” than us in other respects, after all even without AI my computer is better at me than remembering what I’ve agreed to do over the next six months.

But even if AI doesn’t get smarter than humans, it can gain by being more replicable. Not just does this make it cheaper to use, perhaps more importantly it makes it more responsive. While I might harrumph at how slowly The Genie responds to my queries, it’s still far faster than contacting a human.

Smith continues by surmising that AI may be able to make sense of phenomena that can’t be reduced to simple laws, but can only be understood by something able to comprehend a multitude of details:

> there may be laws of the universe that humans can’t understand but AI can. I call these “cloud laws” — causal regularities that can be exploited by technology, but which are too diffuse and complex for an individual human being to either intuit or communicate.

His thought is that even if there isn’t any space for AI to get more intelligent than humans along the lines we are used to, that they can open up new directions. As well as these cloud laws he also thinks that AI can understand human systems that rely on the kind of tacit, distributed knowledge that human organizations build up over time.

My take-away here is that AI won’t seem more intelligent in the way that we typically frame intelligent, but more intelligent in different ways. The converse of which is that the human value comes in artfully combining our human nature with these new spells that The Genie can cast.

 ❄                ❄                ❄                ❄                ❄

Especially in our profession, we’ve seen increasing emphasis on the importance of data. However I’ve observed that most people still struggle to understand the message data is telling us. One of the reasons I’m interested in election forecasting is in how they communicate their insights, especially since so many people have difficulty with probabilistic forecasts. (I often wonder how much being a board-gamer has helped me be comfortable with this, all that time interacting with Combat Results Tables in my youth must have benefited me somehow.)

50+1 (one of the successors of 538) have published a little explainer on [how they designed their 2026 election forecast page](https://blog.fiftyplusone.news/p/how-501-designed-our-2026-election?r=a9pj&utm_medium=ios&triedRedirect=true). There’s a good discussion of the logic behind their simulation histogram, I like how they use a text annotation to explain one point, giving the reader enough guidance to understand the rest of the graphic. They also tackle the knotty problem of visualizing geographical data on the house races. There’s a common visualization error in the U.S. using [choropleth maps](https://datavizcatalogue.com/methods/choropleth.html) that leads to large areas of the landmass shown red, implying dirt votes rather than humans. Their approach to this, using dots on the map, helps visualize both the politics and the population density. They also explain how to deal with this kind of data on small screens. Lastly they describe their approach to tabular data, and how this is the right place for lots of details, together with affordances to help both casual and power-users navigate those tables.

 ❄                ❄                ❄                ❄                ❄

I’ve kept an eye on Alex Stamos for a while now, as he’s a sensible voice on security and safety. He’s posted a newsletter on substack that casts an intelligent eye over [recent safety issues with AI](https://alexstamos.com/p/moving-forward-from-hot-fable-summer).

He makes a clear critique of recent US government actions around LLM models

> On a Friday afternoon at around 5pm PT, Anthropic was forced to shut down a system that had been plumbed into coding agents, SOCs, customer service bots, and countless products. \[…\]
>
> This had the immediate effect of injecting political risk into the US AI ecosystem for both American and non-American customers. It signaled that you cannot depend on American AI infrastructure because, at any moment, an unwritten, capricious, and legally dubious justification could be used to yank that infrastructure from underneath your feet.
>
> When Fable was turned back on, it was much dumber and less useful to cyber defenders
>
> \[…\]
>
> While Fable was down, Z.ai was taking advantage of the free market and permissionless innovation culture provided by the (checks notes) General Secretary, Politburo, and Communist Party of the People’s Republic of China, and released GLM 5.2. With 753B parameters, it falls a bit short of Opus 4.8 in most tasks but is extremely efficient and is small enough to be trained and hosted in many enterprise contexts. With an MIT license it can be fine-tuned with a wide range of techniques and used by any customer in any context. Since then, Kimi K3 has rocked the industry by providing Fable-like performance

As he highlights, one of the biggest dangers with the danger of shutting down a frontier model is that it can cripple an organization’s defenses:

> Hugging Face tried to use an Anthropic model to defend itself during an active incident, got blocked by the classifier, and moved to GLM 5.2 on an emergency basis. Their advice to everyone else was to keep an open-weight model on the shelf for defensive cyber.

On the whole, he sees it as a Good Thing that these model escapes have happened:

> The OpenAI attack against Hugging Face, and Hugging Face’s excellent write-up has given us a preview of what a standard AI-enabled attack might look like in a matter of months.
>
> It’s good that we got this warning shot. Nobody got hurt, the target was a sophisticated actor with the ability to defend themselves and the ability to give us a detailed write-up, and OpenAI turned the model off.

He follows up by saying that all of this is signal that we should “stop talking about AI finding bugs, focus on fixing them”. These modern LLMs can do much to fix bugs and improve security, and people need to work on that rapidly to fix holes before less reputable folks than OpenAI find them. Then figure out how to harness LLMs to introduce this kind of checking into the everyday build process, so that this kind of analysis just a step in the continuous delivery build pipeline.

I agree with him both that open-weight models should be legal, have their upsides, but will also be used for many bad things by bad actors. Both the industry and government agencies need put serious effort into figuring out how to mitigate these risks.

Where I would go further is to say the same is true of the closed-weight models too. Although closed weight models are subject to greater controls, the same fundamental issues apply. He rightly takes the foundation model companies to task:

> There is an old saying I pass down to my students when I give them career advice - if you are a jerk to people on your way up, don’t expect them to catch you when you are on your way down

There’s a lot of sound advice for model companies, the government, defenders, and venture capitalists.

We will go through some rough changes, I just hope that we will indeed come through it with a better society. On the whole, that’s happened with previous technological changes like this, but past performance does not guarantee future results.

 ❄                ❄                ❄                ❄                ❄

The Economist has a good article on the [impact of AI in China](https://www.economist.com/briefing/2026/08/06/chinas-ai-drive-threatens-the-worlds-largest-workforce).

> China has made an all-out push in ai, under the conviction that, in its competition with America and the rest of the world, dominance of the technology is an almost existential necessity. \[…\] But the party is increasingly concerned about how ai will displace workers.

Robots and AI are appearing in an economy that’s struggling after the recent property crisis. The Chinese government is opposing firms using AI to cut jobs. China will need robots: its population will shrink by 25% by 2050. But with less working people, there’s less financial support for pensions. Many countries have to deal with shrinking population, but China’s challenge is particularly acute.

 ❄                ❄                ❄                ❄                ❄

[Rob Bowley:](https://bsky.app/profile/robbowley.net/post/3mte4kkiaoc2z)

> I go on holiday for a few weeks and we’ve already moved on from Loop Engineering to Graph Engineering
>
> The half-life of a paradigm is getting shorter than my annual leave
>
> My prediction: neuro-symbolic engineering by the end of August, at which point we’ll have gone full circle and reinvented Prolog
