---
title: AI is dramatically accelerating AI research
link: https://www.planned-obsolescence.org/p/ai-is-dramatically-accelerating-ai
source: hnrss-org
published: 2026-11-06T20:27:23Z
updated: 2026-11-06T20:27:23Z
first_seen: 2026-11-06T23:27:52.101188226Z
authors:
- yurivish
content: extracted
html: 2026-11-06-ai-is-dramatically-accelerating-ai-research.html
remote_preview:
  url: https://substackcdn.com/image/fetch/$s_!u_X_!,w_1200,h_675,c_fill,f_jpg,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fdd74ff70-df26-4e1a-be31-4fc3543bada8_2048x1072.png
---

If acceleration compounds, the naive extrapolation implies superintelligence within a year

*This post discusses a public METR report on AI R&D acceleration at Anthropic. I did not work on this project or see the unredacted report.* [1](https://www.planned-obsolescence.org/p/ai-is-dramatically-accelerating-ai#footnote-1) All opinions are my own and do not represent METR or the report authors.

Thanks to Ryan Greenblatt for discussion, feedback, and help with modeling, and Daniel Kokotajlo, Eli Lifland, and Hjalmar Wjik for comments.

Earlier this week, [Anthropic stated in their S1](https://x.com/theobearman/status/2117277210570604945) that they “may imminently meet” the threshold of “dramatically accelerated progress due to AI” as defined in their Responsible Scaling Policy. In [their latest RSP (effective July 2026)](https://www-cdn.anthropic.com/files/4zrzovbb/website/0bacdc8440ea96e62a8766d99ebe1d4eea6d5f3a.pdf), say this threshold will be met when:

1. They observe or expect overall capabilities progress to be going twice as fast as the fastest sustained rate before AI was contributing much to AI R&D. [2](https://www.planned-obsolescence.org/p/ai-is-dramatically-accelerating-ai#footnote-2)

2. This is plausibly attributable to AI automating AI R&D, rather than acceleration in compute or other inputs. [3](https://www.planned-obsolescence.org/p/ai-is-dramatically-accelerating-ai#footnote-3)

For the last few months, some of my colleagues at METR have been working on a deep-access investigation to understand how much Anthropic is being accelerated by its AI systems. **From my reading of [their report](https://metr.org/blog/2026-11-05-anthropic-ai-rd-acceleration-assessment/) and Anthropic’s RSP, it looks like the “dramatic acceleration” threshold has already been crossed.** [4](https://www.planned-obsolescence.org/p/ai-is-dramatically-accelerating-ai#footnote-4)

[![](https://substackcdn.com/image/fetch/$s_!hcJn!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F40ef3251-b0ed-4ed8-bb5f-649bfccc7563_1880x1561.png)](https://substackcdn.com/image/fetch/$s_!hcJn!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F40ef3251-b0ed-4ed8-bb5f-649bfccc7563_1880x1561.png)

The report authors found that:

1. Between October 2025 and October 2026, capabilities progress at Anthropic has been going about twice as fast as it had been over the previous twelve months. The measurement they report is an internal version of the [Epoch Capabilities Index](https://epoch.ai/eci?view=graph&tab=release-date) called AECI; a wide range of other supporting evidence for acceleration has been redacted. They said that this redacted evidence was "a majority of the evidence \[they\] found most informative."

2. While the pace may have initially started picking up for a mix of reasons, AI automation is probably the large majority of the reason why capabilities are *still* improving at this faster pace today. Their reasons for believing this have been redacted.

This is a much bigger deal than it might sound at first. I think this report is the most important update we have gotten about AI risk in the last couple of years — much bigger than [the Hugging Face attack](https://www.planned-obsolescence.org/p/the-hugging-face-attack-surprised). Naively, it points toward much shorter timelines than most external researchers (including [the AI 2027 team](https://ai-2027.com/)) have projected.

Importantly, **if AI****is roughly doubling capabilities progress overall, it must be speeding up human researchers by** ***much*** **more than that.**

[![](https://substackcdn.com/image/fetch/$s_!AN_L!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7fbd709e-dadd-451e-b6b1-1018da098568_2048x751.png)](https://substackcdn.com/image/fetch/$s_!AN_L!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7fbd709e-dadd-451e-b6b1-1018da098568_2048x751.png)

Researchers work on improving algorithms and architectures; Anthropic has sometimes referred to these innovations as “compute multipliers.” [5](https://www.planned-obsolescence.org/p/ai-is-dramatically-accelerating-ai#footnote-5) Overall AI capabilities (“effective compute”) is the product of compute multipliers and training compute. Because the compute multipliers are only one input into capabilities, this means that if AI capabilities are advancing twice as fast overall because of software progress, the rate of growth in compute multipliers would need to be *more* than twice as fast: perhaps ~2.5-3x faster based on reasonable assumptions. [6](https://www.planned-obsolescence.org/p/ai-is-dramatically-accelerating-ai#footnote-6) Since there’s uncertainty about the size of speedup and how much of it comes from AI, I’ll go with ~2-3x.

Similarly, researcher *labor* is only one input to discovering these compute multipliers: they need to test their ideas, which requires compute for experiments. If we assume a very simple Cobb-Douglas model where experiment compute and labor are similarly important, that means the uplift to labor roughly needs to be the *square* of the acceleration in compute multipliers. [7](https://www.planned-obsolescence.org/p/ai-is-dramatically-accelerating-ai#footnote-7) This would imply that we would need a (~2-3x)^2 = ~4-9x speed up in labor to make overall AI capabilities go twice as fast. [8](https://www.planned-obsolescence.org/p/ai-is-dramatically-accelerating-ai#footnote-8)

However, once capabilities *start* speeding up significantly, this also means that low-hanging fruit is plucked at a correspondingly faster rate. Each new compute multiplier takes more and more work to discover and implement (i.e., “ideas are getting harder to find”). Before there was much acceleration from AI, labor roughly grew at some roughly fixed exponential rate from hiring more humans. Combined with growth in compute, this compensated for diminishing returns enough to maintain the baseline rate of capabilities progress. But to maintain a new faster rate of capabilities progress without scaling compute faster, we would need a new faster rate of growth in overall labor.

[![](https://substackcdn.com/image/fetch/$s_!plX6!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1c28af7c-b5ee-409a-8151-438b756ee57f_2048x1248.png)](https://substackcdn.com/image/fetch/$s_!plX6!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1c28af7c-b5ee-409a-8151-438b756ee57f_2048x1248.png)

Since the human labor force is stuck growing at more or less the old rate, the uplift from AI would need to get larger and larger over time just to sustain capabilities progress at its new rate. That means the uplift *today*, after several months of accelerated progress from AI, [9](https://www.planned-obsolescence.org/p/ai-is-dramatically-accelerating-ai#footnote-9) needs to be even higher than ~4-9x, the estimate for what it would take to double progress at the start: **maybe the necessary labor uplift is ~10x**. This is very uncertain, and substantially more or less is plausible. But it lines up surprisingly well with informal self-reports from Anthropic staff, who say they are now getting a ~12x uplift from the use of internal models.

![](https://substackcdn.com/image/fetch/$s_!1KlI!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e1f8a00-b6d6-4060-8341-6e9c298ba4a3_1880x1175.png)

When you’re picturing a 10x uplift, you should *not* be imagining that every researcher is now a team of ten researchers — as anyone who has ever done anything knows, a team of ten does not get ten times as much valuable work done per hour as one person. Instead, it’s **as if every researcher at Anthropic is in a time warp bubble where they experience ten days for every day that passes in the outside world**. As if AI lets them think, read, speak, type, and do *everything else* ten times faster. [10](https://www.planned-obsolescence.org/p/ai-is-dramatically-accelerating-ai#footnote-10) Anthropic probably wouldn’t have been able to speed up its research this much by hiring any number of people. [11](https://www.planned-obsolescence.org/p/ai-is-dramatically-accelerating-ai#footnote-11)

A reasonable default expectation is that as AI capabilities continue to improve, AI researchers will be sped up more and more from automation: if today it takes them a month to get as much work done as they used to in a year, soon it will take a week, and then perhaps a day. If this increasing labor uplift is strong enough to overcome diminishing returns, then in the absence of other bottlenecks it would translate into even *larger* speed ups in the rate of AI progress: progress will start going 4x faster, and then 8x, and then 16x, and so on.

![](https://substackcdn.com/image/fetch/$s_!u_X_!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fdd74ff70-df26-4e1a-be31-4fc3543bada8_2048x1072.png)

The report authors were very uncertain about this question, and say that their picture changed several times with new information. But ultimately, **they did not find evidence of bottlenecks or steep diminishing returns that would get in the way of compounding acceleration.** [12](https://www.planned-obsolescence.org/p/ai-is-dramatically-accelerating-ai#footnote-12) They also report seeing some (confusing) redacted evidence indicating that acceleration could already be above 2x and increasing over time.

**If we take it at face value that AI is now nearly doubling the rate of AI capabilities progress and assume acceleration from AI is smooth, this naively implies that we would reach extreme superintelligence within a year**. It seems that around a year’s worth of AI progress got us from modest acceleration to the point where AIs now make AI progress roughly twice as fast. If acceleration from AI is smoothly increasing, then at this new rate, we should expect that we will double the rate of progress again within six months. At that point, it would take three months to double it again, then six weeks. [13](https://www.planned-obsolescence.org/p/ai-is-dramatically-accelerating-ai#footnote-13)

In theory, these doubling times would add up to infinite capabilities within one year. In reality, at some point we would [reach the effective ceiling on software efficiency](https://www.forethought.org/research/how-quick-and-big-would-a-software-intelligence-explosion-be#highlight--intelligence%20explosion). But I expect these limits to be very far from where we are now.

If we go through this feedback loop, I expect that the AI systems that come out the other side will be able to very quickly develop novel weapons and very easily overpower human civilization in a conflict. If these systems were seriously misaligned, I think they would likely [disempower humanity](https://www.lesswrong.com/posts/pRkFkzwKZ2zfa3R6H/without-specific-countermeasures-the-easiest-path-to), and this could lead to human extinction.

The “dramatically accelerated progress due to AI” is such an important and extreme milestone because it is a strong concrete indicator of extreme capabilities in the very near term. Indeed, [in an earlier version of Anthropic’s RSP (May 2025)](https://www-cdn.anthropic.com/872c653b2d0501d6ab44cf87f43e1dc4853e4d37.pdf), the highest named risk threshold for AI research automation (AI R&D-5) corresponded to a somewhat *weaker* milestone. [14](https://www.planned-obsolescence.org/p/ai-is-dramatically-accelerating-ai#footnote-14) According to the old RSP, Anthropic would be required to have, at a minimum, security good enough to “protect against model-weight theft by state-level adversaries” [15](https://www.planned-obsolescence.org/p/ai-is-dramatically-accelerating-ai#footnote-15) by the time it reached AI R&D-5.

I agree that very strong security, both for model weights and algorithmic secrets themselves, is a minimum prerequisite for undergoing an intelligence explosion next year without running very large risks of killing billions of people. The AI industry is nowhere near this bar: both [OpenAI](https://www.wsj.com/tech/ai/hackers-used-anthropics-claude-to-break-into-openai-b40ba883) and [Anthropic](https://www.washingtonpost.com/technology/2026/10/26/researchers-cybersecurity-start-up-cathedral-got-inside-anthropic/) recently had major security breaches by amateur groups. Beyond strong model weight security, I also think we would need robust logging and [monitoring](https://metr.org/blog/2026-10-22-red-teaming-openai-agent-monitoring/) with universal coverage, reliable [measurements](https://metr.org/blog/2026-01-19-early-work-on-monitorability-evaluations/) of [monitorability](https://www.redwoodresearch.org/blog/proposal-for-tracking-architecture-on-monitorability), effective countermeasures against [monitor collusion](https://blog.redwoodresearch.org/p/how-to-prevent-collusion-when-using), principled alignment training techniques with very convincing [model organisms](https://www.lesswrong.com/posts/ChDH335ckdvpxXaXX/model-organisms-of-misalignment-the-case-for-a-new-pillar-of-1) or [interpretability](https://arxiv.org/abs/2501.16496) to understand how well they’re working, governance infrastructure for [pacing progress](https://www.pacingthefrontier.com/) if we notice these alignment and control interventions start to break down…and many other measures if we’re going to approach an intelligence explosion in anything like a responsible way.

Unfortunately, most of the important evidence that fed into these conclusions has been redacted from this report. Analytical choices will inevitably be fraught, and the report authors say that the evidence was often confusing.

I trust that my colleagues did a careful and rigorous job navigating making sense of what they saw, and I made a big update based on this report. But I wish [much more of the underlying evidence was transparent](https://www.planned-obsolescence.org/p/evidence-about-risk-should-be-transparent) so readers don’t have to rely on trust. We may be running out of time to get on the same page about timelines and [takeoff speeds](https://www.planned-obsolescence.org/p/takeoff-speeds-rule-everything-around), and it is much harder to do that when external scientists cannot examine the data and test out their own models.

[1](https://www.planned-obsolescence.org/p/ai-is-dramatically-accelerating-ai#footnote-anchor-1)

Because I may have seen other confidential information that might be relevant to timelines and takeoff speeds, I will only discuss implications of public evidence rather than sharing precise bottom line forecasts.

[2](https://www.planned-obsolescence.org/p/ai-is-dramatically-accelerating-ai#footnote-anchor-2)

The precise condition is: “*We observe or expect double the rate of progress in AI aggregate capabilities compared to both the rate we’d expect and the fastest rate of extended progress we’ve observed in the absence of significant AI contributions to AI R&D.”* My understanding is the reason to have both of these conditions (double the rate of progress *both* compared to the counterfactual *and* compared to the fastest sustained rate without much AI assistance) is to exclude cases where the pace of capabilities *would* have slowed down a lot without AI automation, but we manage to keep the old trends going with dramatic uplift from AI.

[3](https://www.planned-obsolescence.org/p/ai-is-dramatically-accelerating-ai#footnote-anchor-3)

The precise condition is: *“It is plausible that this doubling is significantly attributable to the automation of research and/or engineering (as opposed to other factors, such as increased headcount, compute, or general productivity), such that continuation of the trend in AI progress seems likely to lead to even greater acceleration.”*

[4](https://www.planned-obsolescence.org/p/ai-is-dramatically-accelerating-ai#footnote-anchor-4)

The definition of the threshold is a bit finicky, partly because two speedup conditions must be met (speedup relative to the counterfactual and speedup relative to the past). But on a plain reading, my best guess is that it has been met.

The authors of the METR report estimated roughly a doubling in the rate of progress relative to the *past*, but they only attribute roughly 80% of the current speed-up to AI, which would mean the central estimate for observed speedup relative to no AI was roughly ~1.8x as of the time of writing their report, in October 2026.

But they also found that the contribution of AI has been growing over time, and I would guess that now (as of November 2026) it is appreciably higher. Even if the rate of progress is not *fully* double the counterfactual right this second, the threshold incorporates reasonable expectations (*“We observe **or expect** double the rate of progress…”*).

[5](https://www.planned-obsolescence.org/p/ai-is-dramatically-accelerating-ai#footnote-anchor-5)

Anthropic CEO Dario Amodei explains the idea of compute multipliers in [this essay](https://darioamodei.com/post/on-deepseek-and-export-controls): *“The field is constantly coming up with ideas, large and small, that make things more effective or efficient: it could be an improvement to the architecture of the model (a tweak to the basic Transformer architecture that all of today’s models use) or simply a way of running the model more efficiently on the underlying hardware. New generations of hardware also have the same effect. What this typically does is shift the curve: if the innovation is a 2x “compute multiplier” (CM), then it allows you to get 40% on a coding task for $5M instead of $10M; or 60% for $50M instead of $100M, etc. Every frontier AI company regularly discovers many of these CM’s: frequently small ones (~1.2x), sometimes medium-sized ones (~2x), and every once in a while very large ones (~10x).”*

[6](https://www.planned-obsolescence.org/p/ai-is-dramatically-accelerating-ai#footnote-anchor-6)

This is a simplified version of the modeling that my colleague Ryan Greenblatt goes through [here](https://x.com/RyanGreenblatt/status/2118447235289817503). His numbers are based on his [prior](https://www.lesswrong.com/posts/FG54euEAesRkSZuJN/ryan_greenblatt-s-shortform?commentId=rAFxcETtNtsuzw6aG) public [modeling](https://www.lesswrong.com/posts/Jga7PHMzfZf4fbdyo/if-mythos-actually-made-anthropic-employees-4x-more) of the factors like the importance of software vs training compute and the importance of labor vs experimental compute.

[7](https://www.planned-obsolescence.org/p/ai-is-dramatically-accelerating-ai#footnote-anchor-7)

Common formulations of Cobb-Douglas often measure labor as the *number* of humans. In that formulation, there needs to be a parallelization penalty on top of labor and the coefficients don’t add to 1. As I discuss below, I’m choosing to represent labor in terms of *serial labor acceleration* rather than the *number* of humans; this formulation was suggested by Ryan Greenblatt in [this post](https://www.lesswrong.com/posts/LjgcRbptarrRfJWtR/a-breakdown-of-ai-capability-levels-focused-on-ai-r-and-d).

[8](https://www.planned-obsolescence.org/p/ai-is-dramatically-accelerating-ai#footnote-anchor-8)

To make the math simple, I just assumed that labor and experiment compute contributed equally (i.e. compute multipliers = (labor)^0.5 \* (compute)^0.5. This means you square the acceleration in compute multipliers to back out the acceleration in labor. The range I gave omits uncertainty on the size of the contributions: for example if labor contributed ⅔, you would instead need to raise it to the power of 3/2.

[9](https://www.planned-obsolescence.org/p/ai-is-dramatically-accelerating-ai#footnote-anchor-9)

I say several months rather than a full year because the report authors say that the *initial* faster progress in late 2025 was in significant part due to other factors, though the *current* speedup is mainly due to AI.

[10](https://www.planned-obsolescence.org/p/ai-is-dramatically-accelerating-ai#footnote-anchor-10)

In practice, the 10x uplift is unlikely to be uniform: some tasks (e.g. more [“hill-climbable” tasks](https://metr.org/blog/2026-05-19-frontier-risk-report/#coding-agents-did-real-projects-that-would-take-humans-hours-or-days)) will be sped up much more than others, for a net productivity improvement as big as a 10x uniform acceleration.

[11](https://www.planned-obsolescence.org/p/ai-is-dramatically-accelerating-ai#footnote-anchor-11)

That is, any number of people that actually exist. I’d guess it would be sufficient if you could clone all the existing researchers several hundred times, but there aren’t that many strong AI researchers and engineers in the world.

[12](https://www.planned-obsolescence.org/p/ai-is-dramatically-accelerating-ai#footnote-anchor-12)

 From [the report](https://metr.org/blog/2026-11-05-anthropic-ai-rd-acceleration-assessment/): *“A key concern is that acceleration from the use of AI could start to compound, as the development of more capable models leads to further acceleration. As we describe below, we did not identify any clear bottlenecks (limited inputs that would prevent further growth) or steep diminishing returns to further scaling that would get in the way of such compounding acceleration, though our understanding evolved several times over the course of this assessment, and we have significant uncertainty. Because of this, we believe there is a significant risk that further capabilities progress could lead to even faster acceleration over the coming year.”*

[13](https://www.planned-obsolescence.org/p/ai-is-dramatically-accelerating-ai#footnote-anchor-13)

This heuristic argument is consistent with simple but plausible models where recursive self-improvement makes the rate of AI progress exponential in cumulative AI progress (e.g., log effective compute); [here is a derivation of this heuristic argument](https://claude.ai/share/5ebb5437-f514-410c-b52a-014f3f62cc15) for one parametrization of this class of models.

[14](https://www.planned-obsolescence.org/p/ai-is-dramatically-accelerating-ai#footnote-anchor-14)

AI R&D-5 was defined as: *“The ability to cause dramatic acceleration in the rate of effective scaling. Specifically, this would be the case if we observed or projected an increase in the effective training compute of the world’s most capable model that, over the course of a year, was equivalent to two years of the average rate of progress during the period of early 2018 to early 2024. We roughly estimate that the 2018-2024 average scaleup was around 35x per year, so this would imply an actual or projected one-year scaleup of 35^2 = ~1000x*.” It is weaker because it compares to the fixed 2018-2024 period rather than the fastest observed pace without significant AI acceleration, and it doesn’t technically require that you do any attribution to AI to meet the threshold.

[15](https://www.planned-obsolescence.org/p/ai-is-dramatically-accelerating-ai#footnote-anchor-15)

The minimum commitment was to reach [RAND’s ASL-4 security level](https://www.rand.org/pubs/research_reports/RRA2849-1.html); my understanding is that this is a level of security that would protect model weights from a routine operation by top state actors (e.g., China), but not from a top-priority operation. This is how Anthropic describes it: *“At minimum, the ASL-4 Security Standard (which would protect against model-weight theft by state-level adversaries) is required, although we expect a higher security standard may be required.”*
