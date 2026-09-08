---
title: 'Fragments: September 8'
link: https://martinfowler.com/fragments/2026-09-08.html
source: martinfowler-com
published: 2026-09-08T15:22:00Z
updated: 2026-09-08T15:22:00Z
first_seen: 2026-09-08T17:25:23.893129994Z
authors:
- Martin Fowler
content: extracted
html: 2026-09-08-fragments-september-8.html
preview:
  file: 2026-09-08-fragments-september-8.preview-fbde5f67ca0e.webp
  width: 256
  height: 128
  color: '#301b59'
images:
- source: https://martinfowler.com/fragments/fragments-thumb.png
  original:
    file: 2026-09-08-fragments-september-8.image-1f897b24c40a.png
    width: 600
    height: 300
  variants:
  - file: 2026-09-08-fragments-september-8.image-7ebca9929478.webp
    width: 48
    height: 24
  color: '#070d4a'
---

Christian Catalini says we’re in a situation where we are [vastly reducing the cost of generating things, but not the cost of verifying them:](https://catalini.com/ideas/economics-of-ai/).

> This explains why the first major AI products appeared in chat, image generation, and code assistance. Not because these were the hardest human problems, but because their outputs were relatively easy to inspect. A user can judge the tone of a message, look at an image, or run a test on a piece of code. \[…\] The old automation boundary was routine versus non-routine work. The new boundary is increasingly measurable versus non-measurable work.

The issue is then over how well you can measure something. In our profession, we know there’s a big difference between how many lines of code we write and how productive we are, and we’ve seen a regular [failure to understand how to measure productivity](https://martinfowler.com/bliki/CannotMeasureProductivity.html). Too much of what makes work effective is subject to either slow feedback loops or assessments that require subtle judgment. The danger is that people use lots AI automation while using incomplete measurements of its effectiveness, leading to short-term dashboards going up, but disaster in longer time-scales. He refers to these illusory short-term gains as **counterfeit utility**.

> Scale this across companies and institutions and the result is a **Hollow Economy**: extraordinary measured activity sitting on top of weakening human capability, hidden technical debt, correlated errors, and outcomes that nobody can confidently stand behind.

Another highlight in the article was his advice to “build a history of decisions, not a gallery of outputs”. The point is that with AI we can all build really impressive things, but our value lies in the judgment that we’ve formed. It reminds me of how math problems were marked at school. We weren’t just marked on getting the final answer, we were also marked based on our reasoning process.

He uses the OpenAI–Hugging Face incident as an illustration of this gap between generation and verification. He criticizes those who anthropomorphize the agents involved in the attack. By doing so we focus on the behavior of the AI agents, but instead we should focus on the financial incentives that created them and the environment they are operating in.

> Labs are locked in a race. The training run is where the money goes, and RL optimizes exactly what you score. The runs were scored on capability. They were not scored on “did not poison the Artifactory cache.”

I assert that the organizations that build and run agents are responsible for *everything* those agents do, whether that behavior is intended or emergent. If they reap counterfeit utility by neglecting verification, they must face consequences: legal, financial, and if necessary: criminal. To deal effectively with AI, we need to change the incentives involved to ensure people invest more in verification than they do in generation. Otherwise we are driving a car that has a powerful engine, but weak brakes.

 ❄                ❄                ❄                ❄                ❄

Brian Cantrill relates how [readers are exasperated with “writers” using LLMs](https://bcantrill.dtrace.org/2026/09/05/the-revolt-of-the-reader/).

> To those who read broadly, the hand of the LLM is so clear it’s as if the writer’s intellectual fly is open. In fact, it’s so jarring that I have to believe that those writing with LLMs are either not reading enough to see the LLM’s obvious structural tells — or (and?) they aren’t even reading their own content. (A confession: with particularly egregious pieces, I have fantasized about sentencing the author to read them aloud, certain that they themselves will be unable to endure the slop that they are foisting upon the rest of us.)

He points out that readers do care about this, a survey found 78% of readers stop immediately once they sense something is the work a stochastic parrot, and 71% go on to blacklist the writer. It’s not the polish, it’s the authenticity that counts. Readers will always prefer the clumsy voice of the author over the gloss of an LLM’s whispering.

Cantrill reports good success with using [Pangram](https://www.pangram.com/blog/pangram-4-technical) to detect AI writing. I confess I’m a bit wary, do I really trust anyone’s judgment to disentangle LLM-voice from changes in generation and context? Maybe people steeped in Silicon Valley culture authentically speak in LLM-voice these days. Sadly for them, to be misclassified by their readers as an LLM is just as bad as using the damn things.

 ❄                ❄                ❄                ❄                ❄

One of the dirty non-secrets about LLMs is that they were trained on a vast corpus of writing, without consulting the authors of that writing to see if they were cool with it. Individual authors like me can’t do a great deal about it, so are easy to ignore, but music companies aren’t exactly known for taking this kind of thing lying down. So they are [suing over the use song lyrics for LLM training](https://www.theguardian.com/business/2026/aug/31/aanthropic-sued-alleged-theft-songs-ai-train-claude).

> Sony Music Publishing and Warner Chappell, music publishers who manage the copyright of songs on behalf of songwriters and composers, are seeking damages for alleged misuse of “tens of thousands” of copyrighted works by Anthropic. \[…\] The plaintiffs claim they are victims of “one of the largest and most blatant ongoing thefts of intellectual property in history”.

Looking at it a broader societal point of view, there is an argument that the benefits of LLMs could be worth far more than any losses to us authors. But we should not forget that these tools are built on a foundation they used without our consent, and that should be taken into account as we regulate these tools and the fruits they provide.

 ❄                ❄                ❄                ❄                ❄

[Steve Yegge:](https://x.com/Steve_Yegge/status/2094947586373505122)

> All models, no matter how smart, will eventually build systems that they can no longer understand or maintain, if you let them. Fable 5 finally outbuilt itself, and flailed on me for a week. Fable 5.1 looks like it will fix it. For now. But you have to keep an iron grip on system size, or it’ll run away from you.

 ❄                ❄                ❄                ❄                ❄

I was going through some slightly-related work and discovered that the Creating Passionate Users blog had disappeared from the internet (and has been gone since maybe a year ago). For those who don’t know, **Creating Passionate Users** was one of the treasures of the Golden Age of internet blogging. It was the work of [Kathy Sierra](https://en.wikipedia.org/wiki/Kathy_Sierra), also known for co-creating the “Head First” series of computer books. It talked about user experience, and remains some of the best writing on the topic, full of sparkling insights that greatly influenced my thinking, as well as many folks more engaged on user-experience work.

Sadly not just was the blog ahead of time in its content, it was a harbinger of the darker side of the internet, as Kathy came under attack from a particularly virulent form of [Net Nastiness](https://martinfowler.com/bliki/NetNastiness.html). That led her to retreat from active participation on the web, and we’ve missed her ever since.

Fortunately the Wayback Machine did its great duty, and [we can still read its snapshot](https://web.archive.org/web/20250909191913/https://headrush.typepad.com/creating_passionate_users/). I’ve often thought that, if I had a clone to spare, I’d like to create a guided tour of Creating Passionate Users to help readers today read that excellent material. (And if you’re reading this Kathy, and want it still hosted on the web, I’d be delighted to.)

 ❄                ❄                ❄                ❄                ❄

[Simon Willison:](https://bsky.app/profile/simonwillison.net/post/3muwzopsoqs26)

> “I don’t know the answer myself, but I asked a blowhard I know and he took a wild guess, here’s what he said: “
>
> How I interpret pasted replies from an LLM in online conversations

 ❄                ❄                ❄                ❄                ❄

Jessica Kerr loves the feeling of being part of a team of people that learns from each other and from the codebase they are building as extensions of themselves - she incorporates the term [symmathesy](https://jessitron.com/2018/04/15/the-origins-of-opera-and-the-future-of-programming/) for this: a learning system composed of learning parts (both the people and the code).

[“But now agents!”](https://jessitron.com/2026/08/30/who-are-we-now/)

> There was a turning point last year where I noticed that not only are they useful, it is irresponsible not to use them, at least in conjunction with my own code. They’re more thorough, as well as faster. How am I supposed to be responsible for this system, when I don’t understand each line of code?

She has a habit of digging out old terms and ideas and applying them to our digital world. To frame what’s happening, she digs out two bits of latin

> - Verum Factum: I made it, so I get it
> - Vexationes Artium: Put it to the test \[i.e. experiments\]

Agents can’t have Verum Factum knowledge, since it’s gone once their context window clears. They can use Vexationes Artium, running tests to see if something is working.

> If we want agents to write working, reliable code for us, we have to double down, 10x down on our objective verification. We need to vexate that code in artful ways. And we have the agent help us with that, with its thoroughness.

This is, of course, true of those building these AI models - they certainly don’t have a Verum Factum knowledge of how they work, all they can do is come up with artful vexations to figure out what might be going on in there.

What does that mean for us humans? Kerr says The Enlightenment elevated the idea that reason was the special quality of mankind. But now we’ve built machines that can reason. We need to focus instead on human qualities that the machines don’t have. Imagination is more important to us now than reason. And the essence of our humanity is in our relationships with other people.

This material was put together for a conference talk, it’s available in [video](https://www.youtube.com/watch?v=Nvr0klVxw5c) and in [text](https://jessitron.com/2026/08/30/who-are-we-now/) form. Generally I far prefer text, but there’s so many delights in her talk that I found it worth watching. (Such as her current definition of legacy software, as code that wasn’t built with agents in mind - and is thus lacking in testability and consistency.)

 ❄                ❄                ❄                ❄                ❄

Jim Gumbly outlines the decision space for [three people who are making vital judgments](https://www.jimgumbley.com/blog/timing-tradeoff-skynet-timeline.html) about controls and regulations for AI: Sam Altman, Dario Amodei, and David Sacks (for US government).

> Sacks worries that binding oversight could weaken American leadership and the financing that sustains it. Sam and Dario face commercial incentives to keep producing more capable models. The value of those models increasingly comes from persistence, cooperation and permission to act. Those same properties increase the demands on containment and oversight. Some advances also make the evidence harder to interpret.
>
> Each decision can be understandable on its own. Their combined effect can still move deployment ahead of our ability to control it.

A complication for all this is the issue of observability of these models.

> OpenAI reports that Astra is better aligned on its evaluations while also being less monitorable. Its traces can be shorter and less informative. In adversarial tests, where it was instructed to evade oversight, it could conceal some behaviour from monitors.
>
> \[…\]
>
> That matters to the familiar software cycle of releasing something, observing what happens and improving it. The cycle depends on the quality of the observations. Fewer warning flags are reassuring only to the extent that the warning system remains capable of detecting the relevant failures.

 ❄                ❄                ❄                ❄                ❄

There’s an El Niño year coming up, and The Grauniad reports that climate scientists predict [this El Niño is going to be a spectacularly hot one.](https://www.theguardian.com/environment/2026/sep/03/el-nino-supersizing-before-our-eyes-united-nations?CMP=share_btn_url)

> The most recent data, from Monday, shows the temperature of the ocean at the heart of El Niño at 2.6C above the 30-year average. That is already close to the highest anomaly ever recorded in the satellite data era, 3.1C in 2015, with months to go before the peak is expected.
>
> That peak is forecast to reach about 4C in November, according to the average of 14 different models. Data from analysis of corals, tree rings and historical documents suggest no El Niño has reached this level in the last millennium, said Zeke Hausfather, a climate analyst.

If these forecasts end up being accurate, will this make a difference to how seriously people are taking the climate crisis?
