---
title: 'Fragments: July 6'
link: https://martinfowler.com/fragments/2026-07-06.html
source: martinfowler-com
published: 2026-07-06T12:53:00Z
updated: 2026-07-06T12:53:00Z
first_seen: 2026-09-04T12:14:41.802665207Z
authors:
- Martin Fowler
content: extracted
html: 2026-07-06-fragments-july-6.html
---

Last week, Thoughtworks ran a second [Future of Software Development Retreat](https://martinfowler.com/bliki/FutureOfSoftwareDevelopment.html), this time in Europe. As with the previous event, I’ll be sharing some fragmentary thoughts on this. There were five parallel streams, so I could, at best, only attend ⅕ of sessions. This isn’t an event that forms conclusions, rather one that allows those exploring to share what they’ve found, and their visions for the future. The [bliki post](https://martinfowler.com/bliki/FutureOfSoftwareDevelopment.html) lists all the writing I’ve run into on this, by myself and others. I’ll be updating it as more posts appear.

Giles Edwards-Alexander [“noticed a real difference between the retreats”](https://overwatering.org/blog/2026/07/notes-from-fose-europe/):

> Where Deer Valley had hesitancy and a belief that there was something here even if we weren’t yet sure what it was, Engelberg had confidence: the value is here. As I explained to a colleague today, this was not a conference for true believers: the evidence is in.
>
> What does the evidence say? Well, that was less clear. Some patterns and practices are emerging (one attendee had catalogued dozens of agentic engineering pattern libraries) but they are emerging. There is more work to do to truly establish what is effective, and when.

Greg Herlein [felt similarly](https://blog.herlein.com/post/retreat-observations/):

> Reading the reports of the February event, when a lot of these same folks last got together, the conversation was about what agentic development might look like. Aspirational. More about what was coming.
>
> This time? Everybody in the room was doing it. Shipping it. Not slides - production. The whole debate about whether this changes software engineering is over. People have stopped arguing about whether a while ago. They’re arguing about how, and the how is getting real.

On a more micro level, I noted two other things. Firstly, there was much talk now about [harness engineering](https://martinfowler.com/articles/harness-engineering.html), when that wasn’t even a term in Utah - an example of how rapidly things are moving. Secondly people are now worrying about the cost of tokens, where before folks were wanting to do almost anything to incentivize people to talk to [The Genie](https://martinfowler.com/articles/who-is-llm.html).

 ❄                ❄

A question that continued from Utah was whether architecture and design are still important. There seems to be two landmark hypotheses here, one is that The Genie has such a Galaxy Brain that we no longer need to care about such matters, it will handle as much spaghetti as we can throw at it. The other is, in Laura Tacho’s [memorable phrase](https://martinfowler.com/fragments/2026-02-13.html): “the Venn Diagram of Developer Experience and Agent Experience is a circle”. The point being that The Genie uses the same constructs to understand a code base that humans do, so things like good modularity and naming help it as much as it helps humans. Adam Tornhill’s writing is a [good example](https://adamtornhill.substack.com/p/welcome-to-code-for-humans-and-machines) of this viewpoint.

Tidbits from our session on this:

- to evaluate the value of architecture we need to focus on desirable outcomes. Internal design quality boils down to [ease of change](https://martinfowler.com/articles/is-quality-worth-cost.html). The question is whether the lessons we’ve learned so far will continue for agents.
- a way to measure design quality is to look at token costs. If the same change requires less tokens that indicates a better architecture.
- a good architecture only shows its quality over time, we can’t easily measure it in the short term
- why did 3GL languages continue when things like 4GLs, UML etc not take hold? It’s because these programming languages hit a sweet spot of human comprehension of computation
- we’re at the first time ever where the computers care about code quality
- will future models write machine code directly? If so what will humans review or specify?
- we should beware of speculating about what LLMs may do in the future. Instead we need [mechanical sympathy](https://martinfowler.com/bliki/MechanicalSympathy.html) for our LLMs, so we can gain a sense of how they work and how best to use them.
- One workflow:
  - take story from backlog
  - talk it over with an agent
  - once get an agreement, make an ADR for persistent record of spec
  - generate a task list
  - get agent to complete it
- we need abstractions to communicate with agents *(echoing Unmesh Joshi’s thoughts on [building conceptual models](https://martinfowler.com/articles/what-is-code.html))*
- we often find duplication in LLM generated code, together with mixing of concerns (eg intermingled domain and display logic) - even with a good harness
- get agents to generate explanatory documentation at the end of a session
- overnight quality checks with a report for humans to act on in the morning
- LLMs look at existing code, so if that code has problems, the LLM will amplify them
- we should be wary of drawing too many conclusions comparing LLM code with human code - human code varies enormously from team to team.

 ❄                ❄

In his account of the retreat, Mathias Verraes goes into the details of [his perspective of these issues of software design](https://verraes.net/2026/07/software-design-in-the-agentic-age/). He adds another concern: we need good design as a hedge against the risk of dependence on AI. After all, we don’t know how high the costs may rise to. We see governments blocking access to models. We see popular opposition to AI campaigning against data centers and calling for regulation. How much can we rely on AI tools being available to maintain and extend our software in the future?

 ❄                ❄                ❄                ❄                ❄

Charity Majors has a post on [the ethics of working with AI](https://charitydotwtf.substack.com/p/make-ai-boring-again) and does an excellent job of articulating how I feel about this topic. She outlines the harms inherent in AI, both in the creation of its models (training on stolen data) and in inference (slop, lack of accountability, skill atrophy). Her conclusion however, like mine, is that there’s no ethical gain from renouncing the use of AI and castigating those who use it. Such purity provides little practical help with a technology that is so powerful and so useful.

> The way you show care is by showing up. The way you make the world a better place is by getting down in the muck and building it, using whatever skills and resources you have on hand. The way you drive change is you engage.
>
> Yes, we are all complicit. Yes, we are all compromised. No argument. But what are you going to do with that feeling of conviction? Will you channel your discomfort into solidarity and action, or try to ease your conscience by removing yourself from the system? Which does more to help those being harmed?

Her suggestions on how to engage aren’t striking, but that’s hardly unusual. At the [Future of Software Development Retreat](https://martinfowler.com/bliki/FutureOfSoftwareDevelopment.html) I convened a session on this question, and nothing striking turned up there either. That said, I’ve never been much of an activist, so my imagination may be limited.

 ❄                ❄                ❄                ❄                ❄

Gergely Orosz has run into a case where an article of his was [erased from Google search](https://blog.pragmaticengineer.com/pollen-tried-to-remove-my-article-about-callum-negus-fancey-and-google-is-assisting-to-it/) by a clearly fraudulent DMCA claim.

> It seems that anyone can file a bogus copyright claim to get an article they don’t like removed from Google’s search index. This happened in this case. I have no information on who filed the copyright claim. Even less so on who claims to be the copyright owner? Because I am the only possible copyright owner!

He was able to find the DMCA complaint, it was made by “Ellie Piee” whose profile listed them as living on Bouvet Island, an uninhabited Norwegian dependent territory near Antarctica. It claimed Gergely’s article copied a New York Post article entitled “Band Leader Hits Winning Chord”. But Gergely’s article is “Inside Pollen’s Collapse: “$200M Raised” but Staff Unpaid”, and the two do not share a single sentence. There’s an obvious motivation for folks connected with Pollen to have done this, and I hope the resulting [Streisand effect](https://en.wikipedia.org/wiki/Streisand_effect?ref=blog.pragmaticengineer.com) bites them where it hurts.

 ❄                ❄                ❄                ❄                ❄

404 media have a bunch of (paywalled) reports on the impact of companies realizing that **token costs are getting out of control**. They’ve acquired [leaked Slack chats, internal dashboards, emails and other material](https://www.404media.co/companies-are-throttling-employees-ai-use-because-its-too-expensive/) from companies including Citi and Amazon.

Companies are urging staff to use less powerful models, or cutting off frontier models entirely. A dashboard indicates that one company has seen its token bill rise from $5 million in August 2025 to $15 million in May 2026, on track to spend over $120 million in the fiscal year.

404 earlier reported about [Accenture taking steps to reduce token usage](https://www.404media.co/the-tokenpocalypse-is-here-companies-are-scrambling-to-stop-spending-so-much-on-ai/). The biggest problem wasn’t software engineering using [agentic programming](https://martinfowler.com/bliki/AgenticProgramming.html), but rather staff “chewing tokens” by using AI to do things like turning PDFs into presentation slides. They saw themselves, and their clients, grappling with exponential increases in token costs. Inevitably, after consulting firms spent time urging their clients to use AI heavily, they are now offering services to control these costs.

Another post says it appears that one way to reduce token costs is to get AI tools to [speak like cavemen](https://www.404media.co/companies-are-making-claude-and-codex-talk-like-cavemen-to-stop-ais-soaring-costs/), using a [skill/plugin](https://github.com/JuliusBrussee/caveman?ref=404media.co).

There’s a good summary of all this on 404’s freely available podcast: [The AI Tokenpocalypse Is Here](https://www.youtube.com/watch?v=Sia4LZGNkVs).

 ❄                ❄                ❄                ❄                ❄

I share these thoughts just after the July 4th weekend here in America, indeed the Semiquincentennial. Historian Bret Devereaux celebrated this event with [a careful reading of the Declaration of Independence](https://acoup.blog/2026/07/04/collections-on-the-declaration-of-independence/), a document often talked about more than it’s read. Which is a shame since it is hardly very long, and its impact was remarkable, and not just in what is now the United States.

> The Declaration of Independence was recognized as a radical, potentially explosive document at the time of its issuance, as we’ll see. And it was explosive: the world of 1775 was one dominated by monarchies with just a tiny handful of traditional republics (which we should not ignore!). It took a long time for the seeds of the declaration to spread, but the world it helped create is one where liberal democracies, while hardly universal (more people have always lived in unfree societies than free ones) represent the most economically and culturally dominant bloc in world affairs – something that had never happened before. The Declaration, in its way, remade not just the Thirteen Colonies, but slowly, surely, as water seeps through the cracks of rocks (or my floorboards, alas), it remade the whole world.

Devereaux shines a light onto the world of this text, illuminating its historic context, a world that is very different to the one anyone reading this grew up in. It’s assertions of a natural law that there is equality of rights among men and that governments ought to derive their powers from the consent of the governed would seem hardly worth stating now, yet were deeply radical in 1776. I’ve found that reading history like this has helped me understand how the world is, and gives me a broader perspective on the drama of current affairs.
