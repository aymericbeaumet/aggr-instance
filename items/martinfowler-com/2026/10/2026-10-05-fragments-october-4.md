---
title: 'Fragments: October 4'
link: https://martinfowler.com/fragments/2026-10-04.html
source: martinfowler-com
published: 2026-10-05T04:28:00Z
updated: 2026-10-05T04:28:00Z
first_seen: 2026-10-05T09:35:15.036553018Z
authors:
- Martin Fowler
content: extracted
html: 2026-10-05-fragments-october-4.html
---

In response to my [last fragments](https://martinfowler.com/fragments/2026-09-29.html) (probably the bit about us worrying if LLMs have consciousness when we when we should be wondering why they don’t have a conscience) [“Metalanguage” replied](https://x.com/metalanguage_os/status/2104934457333707058):

> we shipped the id and forgot the superego. classic software lifecycle.

I don’t know what was on their mind, but their post immediately made me think of the classic 1956 movie Forbidden Planet. Plenty of sci-fi, and other literature, have explored humans creating technology with unintended behavior, going back at least to Mary Shelly. But that movie was particularly influential on sci-fi film-making and in the heart of its story is what happens when we nurture a thinking machine.

I use the term “nurture” here deliberately. We talk of building software, but building implies a degree of determinism. When we build a bridge, or a locomotive, we expect it to behave in a controlled and well-understood manner. That’s a difference in degree to how we cultivate plants in our garden, or nurture young children. One of the challenges of working with these systems is understanding what has changed in this shift from building a computational system to nurturing an inferential one, and how our processes need to change in response.

We get unintended behavior with deterministic building: some bridges have collapsed, and our computational systems often have bugs. But one difference is that when we find a bug in a computational system we can usually fix it. Even if we can’t, we can usually disable a component so the bug won’t do further harm. With inferential LLMs however, there is no such simple fix or disablement, which may lead us to the fate of the Krell.

(If you haven’t seen Forbidden Planet, it’s well worth watching. Yes, it shows it was made in the 1950s - with special effects, music, acting, and attitudes of that decade. But the story is solid, and its key theme is very relevant to the future we build with generative AI. Just don’t read about it in advance, it’s better to be immersed in the story without spoilers - although my memory of that experience is understandably hazy.)

* * *

Many people who follow me also know my friend Ola Bini, who was my colleague at Thoughtworks for many years, and was living in Ecuador working as an independent software security expert. Sadly his time in Ecuador was dogged by a [bogus prosecution](https://www.eff.org/deeplinks/2025/04/six-years-dangerous-misconceptions-targeting-ola-bini-and-digital-rights-ecuador) by the authorities there. But things seemed to have settled down, and although not allowed to leave Ecuador, Ola was able to get on with his life.

Sadly that’s no longer the case as he [was deported from Ecuador on Friday:](https://www.eff.org/deeplinks/2026/10/ola-bini-ordered-leave-ecuador-under-obscure-accusations)

> According to information released by his lawyer, Bini was intercepted by a car with four people who identified themselves as immigration agents. He was then taken to an immigration office without further information or a formal order from a competent authority. There, officials told Bini that his visa had been revoked but didn’t show any supporting document.
>
> Bini’s defense filed a habeas corpus to safeguard his freedom and prevent his deportation. Yet, Ecuadorian authorities affirmed that the developer represents a threat or risk to public security and the state structure, and must leave the country. The ground for deportation is a secret report which allegedly asserts that Bini committed acts against the security of Ecuador. The defense could not access its contents.

I was really worried for a while, since it wasn’t clear where he was going to be deported to. But [he tweeted from Sweden](https://x.com/olabini/status/2106671967944511885), so I’m thankful for that. But this is only a partial relief. Ola has spent thirteen years in Ecuador and made it his home. To be thrown out of your home for scant reason is a heavy thing to bear, and the officials who did that have committed a serious offense.

* * *

DDD Europe have released the video of [Gien Verschatse interviewing Eric Evans and myself](https://www.youtube.com/watch?v=fndU_B5rmPE) at the conference in June. We start by talking about how we bonded over conceptual modeling in the late 1990s. The conversation quickly moves to AI, we note that it’s impossible to predict how such a big change will work out. We do expect that it will cause us to think about our work in different ways, but the change may well be liberating, it’s reinvigorated Eric’s love of programming.

> people are probably going to feel very frustrated by \[the new way of thinking about software\]… but when you get through that, there is a kind of a wonderful feeling of my brain’s been loosened up.

Our background in agile planning helps with the uncertainty, as we are used to taking small steps and being attentive to feedback. We mull on the interplay of writing and thinking, in terms of both prose and code, and how its very much an iterative process of exploration and refinement - the same is true when we chat with our LLMs.

And don’t miss Eric’s important final tip.

* * *

> Paul Graham:
>
> [@paulg on X](https://x.com/paulg/status/2106018287117345070)

> There were a lot of things that only worked because there’s a limit to the rate at which humans can operate. We’re about to find out what all of them are, as they break.

* * *

The speculation continues about whether or not reading code will play a part in a software developer’s future. [Geoffrey Huntley says](https://ghuntley.com/readable/?ref=geoffrey-huntley-newsletter).

> People are still saying, very loudly, that code should be readable so that humans can understand it. I no longer think that’s the goal.

Interestingly his example has the LLM explain a haskell function definition… by translating it to Python. Which, to me, suggests there is a role for code - just that LLM need not store code in the same form that it presents it to a reader. This is essentially the same idea as [projectional editing](https://www.martinfowler.com/bliki/ProjectionalEditing.html), which posits that the editable representation of software need not be the same as its storage representation.

Sam Ruby touches on this as he [muses on a Rails World keynote](https://intertwingly.net/blog/2026/09/25/Pencils-Down-Notation-Up.html). He quotes DHH saying:

> Rust is a good prompt compilation target for the moment, but so is C++. And soon assembler. Then microcode. Myopic to think we’re going to stop the agentic drill bit until it reaches computing bedrock.

He responds with:

> The post leaves one question unasked, though: what sits at the top of the drill? What do we keep, edit and trust as the source of truth?

He carries out exercise of looking at some Rails software. Represented in Ruby/Rails and its about 60,000 tokens. Compiling it into C it turns into 4,000,000 tokens. That increase in token size will hamper the LLM, that still has to fit it into its context window, and even if it were to fit, figure out where to focus its attention. Sam points out reasons why, even absent a human reading it, it makes sense to represent the program in a higher-level language.

> what Rails becomes when agents write the code: the most compact, precise and conventional specification of a web application, whatever it ends up compiled to.
>
> Let the drill go as deep as it can. Just keep the notation at the top.

This all reminds me of [what Unmesh Joshi argued](https://martinfowler.com/articles/what-is-code.html#CognitiveDebt): that code serves “two distinct but intertwined purposes”: instructions to a machine, and a conceptual model of the problem domain. After exploring how those change with LLMs he concludes:

> The role of coding is not disappearing. But it is changing.
>
> As LLMs make code generation cheaper, the mechanical act of writing instructions becomes less central. What becomes more important is making the conceptual model explicit, discovering the right vocabulary, and refining that vocabulary through iteration, domain expertise, and feedback. This is also why programming languages continue to matter deeply. We are not meant to be passive reviewers of generated code. The act of writing code is itself part of our thinking.
>
> Code is still instructions for a machine. But it is also a model of understanding. In the LLM era, that second role becomes even more important. The future of coding is not just writing more code faster. It is building better conceptual models, better vocabularies, and better foundations on top of which both humans and LLMs can work.

* * *

In a later post, Sam [pondered on how people are talking about the capabilities of agents](https://intertwingly.net/blog/2026/09/27/Partly-in-the-Right.html) in a way that resembles the parable of the blind men and the elephant. We all only have only a partial view of this object and where it’s going. A theme for all of us:

> The practical question isn’t whether agents are good. It’s this: for the task in front of you this week, where will the information come from, and what will check the result?

* * *

The Economist’s pithy summation of [investors concerns about the dangers of AI companies’ products](https://www.economist.com/finance-and-economics/2026/09/22/you-cant-trade-your-way-through-the-ai-apocalypse):

> It’s hard to celebrate an initial public offering that leads to a terminal public offing.

* * *

The news about the latest model from Google [is interesting](https://x.com/aipulseda1ly/status/2105394296815779861).

> Gemini 4 Argon has an insanely low hallucination rate on Artificial Analysis. 15%.
>
> Grok 4.7 is at 29%. GPT-6 Astra 45%. Opus 5.5 59%. Fable 5.1 69%.
>
> The only models below it barely answer anything. None of them get more than 15% right.
>
> It gets fewer answers right than Opus 5.5 on max, 50% against 66%. But when it doesnt know, it says so instead of making something up.

Being clearer about what it doesn’t know, at a cost of getting less answers right, is definitely a trade-off I prefer.
