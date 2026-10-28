---
title: Artificial Analysis v5 and the benchmarker's dilemma
link: https://www.interconnects.ai/p/artificial-analysis-v5-and-the-benchmarkers
source: interconnects-ai
published: 2026-10-28T12:58:39Z
updated: 2026-10-28T12:58:39Z
first_seen: 2026-10-28T17:21:45.580541782Z
authors:
- Nathan Lambert
summary: A sort of review of the benchmarking industry and what's missing.
content: extracted
html: 2026-10-28-artificial-analysis-v5-and-the-benchmarker-s-dilemma.html
remote_preview:
  url: https://substack-post-media.s3.amazonaws.com/public/images/6a847075-78be-45e9-b554-ae9abc6f9296_3182x1790.png
---

A sort of review of the benchmarking industry and what's missing.

##### Housekeeping: I’m on the road so the voiceover will be late.

Hill-climbing is easy, knowing what to hill-climb is hard. Making scores go up is *expensive*, but we have a much clearer playbook for it than we do for creating meaningful benchmarks at the true frontier of AI today.

Independent evaluation of language models is under-appreciated in how hard of a task it is. These groups are not just taking current models and certifying their scores, which is a technically involved task. This is their job if AI progress is slow. When AI progress is as fast as it is, the technical details of the job are the routine work part of trying to put a single number on what makes for a good AI model today. The goal is to have a single number that is representative of current models, matching vibe checks, community sentiment, and scientific analysis, while being forward looking enough so that it’s not outdated in 3 months.

Everyone building a benchmark faces a sort of paradox. You need your benchmark to reduce to one single number, so that it is easy to discuss, use in reports, and compare models, but every method of aggregating scores comes with meaningful trade-offs. The best scientists need to do this, everyone does. It’s a matter of creating aggregating metrics that you think are most fair, knowing there will always be imperfections.

With the amount of money in the AI ecosystem today, the weight of independent evaluation is even higher – you dictate the winners and losers of a trillion dollar market. You also influence some of the models themselves, steering the shape of progress. This is a lot of pressure!

My prompt for this article is [Artificial Analysis’s recent update to their Intelligence Index, v5](https://artificialanalysis.ai/articles/artificial-analysis-intelligence-index-v5). The AA index is the most prominent independent intelligence metric for the latest frontier models, followed in a distant second by [Epoch AI’s Capabilities Index (ECI)](https://epoch.ai/eci). To date, all of these have been incredibly useful for the community and massively flawed!

Language models are capable of so much today, and this scope of capabilities is growing quickly. Our era of singular intelligence indices *should* be phased out soon, but it will take a long time.

A summary of AAv5 is below, from their blog post, where they describe it as “reflecting real-world usage today:”

> **Intelligence Index v5 weights:**
>
> **➤ Knowledge Work (25%):** AA-Briefcase (15%), AutomationBench-AA (5%), and GDP.pdf (5%)
>
> **➤ Coding (25%):** FrontierCode 2.0 (15%) and Terminal-Bench 4.0 (10%)
>
> **➤ Science (25%):** Terminal-Bench-Science (15%) and CritPt v2 hard subset (10%)
>
> **➤ General (25%):** AA-Omniscience v1.1 15% (10% accuracy, 5% non-hallucination) and HLE-Diamond (10%)

Here’s the new ranking of models:

[![Image](https://substackcdn.com/image/fetch/$s_!WPu_!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F25433598-9909-4aff-b43f-d380c3eb099b_1200x567.jpeg "Image")](https://substackcdn.com/image/fetch/$s_!WPu_!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F25433598-9909-4aff-b43f-d380c3eb099b_1200x567.jpeg)

The problem with trying to benchmark real-world usage today is that it is extremely jagged, just like the models. The most proven use-case is coding, where open models are far more competitive and likely to keep up. In knowledge work, I’m less confident in open models.

At first glance, in aggregate, this definitely looks reasonable. Still, it shows signs of strain and confusion, as any benchmark will, with GPT 6.1 Terra beating Claude Fable 5.1. Today, some benchmarks measure the absolute frontier of capabilities, playing nicely in open-ended tasks through your favorite harness, which often does take a large model. Some other benchmarks measure certain skills that will be needed of a model in repetitive agentic tasks. I get the feeling that AA is tuning their benchmark more towards the former, a general agentic collaborator, but all of these behaviors overlap.

This takes us to the plot going around that prompted me to write this, where they showed the balance of open vs. closed models on the new benchmark. This plot shows 8 closed models beating the best to-be open-weight model, Kimi K3.1 Pro (which isn’t colored blue). This benchmark is taking the opinion of trying to measure where frontier models are moving into, which grows the gap to open models. This is an okay decision to take, but over time as existing workflows of open models become key factors of the economy, it could become a benchmark that misses where the core economic engine of the industry is.

[![](https://substackcdn.com/image/fetch/$s_!cZSa!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6c1717e0-0cc3-4dec-b83a-40b32953a26e_1200x598.jpeg)](https://substackcdn.com/image/fetch/$s_!cZSa!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6c1717e0-0cc3-4dec-b83a-40b32953a26e_1200x598.jpeg)

Open models’ demise from this seems greatly exaggerated, but I do think the previous index was flattering the open weight models too strongly. Consider this plot from the Kimi K3.1 Pro launch — yes, the open models are strong and useful in some cases, but the best OpenAI and Anthropic models have been a major step ahead in their flexibility and reliability.

[![Image](https://substackcdn.com/image/fetch/$s_!2Ajo!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F31703238-e53d-44f6-abe9-4e4093df3274_2464x1032.jpeg "Image")](https://substackcdn.com/image/fetch/$s_!2Ajo!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F31703238-e53d-44f6-abe9-4e4093df3274_2464x1032.jpeg)

Some of the evaluations in AAv5 that open models are most behind on are “science” evals like [terminal-bench-science](https://artificialanalysis.ai/evaluations/terminal-bench-science) and [CritPT v2](https://artificialanalysis.ai/evaluations/critpt-v2?critpt-subset=hard). Open models are behind, but the value proposition was never to compete truly at the frontier. The open model economy will be about

1. Reducing domains with high market demand for inference to a commodity price of hardware.

2. Allowing ownership of intelligence, specializing it to internal data, and controlling internal agents across many companies in the economy.

Both of these take a very long time and are partially represented in benchmarks. The supply of compute is currently so low that open model providers also are raking in solid margins – when this shrinks, we will know the system is starting to work. The same goes for diffusion into enterprises, it’s not clear what the exact ceiling on it is because of the difficult ML expertise that is needed to build evaluations and do post-training today.

[Share](https://www.interconnects.ai/p/artificial-analysis-v5-and-the-benchmarkers?utm_source=substack&utm_medium=email&utm_content=share&action=share)

At the next major model cycle, say in 2-3 months with GLM 6 or Kimi 3.5, I expect the leading open model labs to make substantial steps on this index, again surprising many observers. This is a cycle we have been on for years now.

I’ve written [many times](https://www.interconnects.ai/p/glm-53-how-chinese-labs-keep-stride) about how open models keep up with the frontier, and a summary is that:

1. The best open models do benchmax a little. The worst open models benchmax a lot (but don’t get a ton of attention anyways).

2. The best open models are a bit narrower in which capabilities they cover, as seen above with their gap on scientific domains. The open model labs are working to address this, but I think it could be come a strategic error to not focus.

There’s a more nuanced version of explanations of the performance gap today, where so many of the gains in training are coming from having RL environments with distributions close to the evaluations (a longer analysis on this is [here](https://x.com/teortaxesTex/status/2115396773673943203)). Keeping up with rapid progress across multiple domains requires a sophisticated RL stack that not all the open model labs may possess. Some may be reliant on buying data that has moderate lead times as well.

Another part of this announcement is shifting to “private benchmarks”, where the prompts are not public, as a sign that open labs are benchmaxxing. This sort of analysis [has been done before](https://www.lesswrong.com/posts/rJcCrXyEsJKmmDpWG/how-far-behind-are-open-models), and fits with my above theory on how open labs benchmax. It is easier to create training data to support an evaluation when you know the shape of it, but the task isn’t impossible with hidden data. All in, this is a change that directionally I think is defensible, but it’s not unanimous. For example, you’re putting complete trust in the private benchmark builder, as you no longer get the benefit of the community finding issues in the benchmark and [pushing the benchmark to improve itself](https://x.com/EpochAIResearch/status/2113703991767105607) – which happens *a lot!* There is enough money on the line, where I’d rather see transparency in benchmarking and enabling many eyes to find bugs.

The final point I need to make, which does flatter the closed labs, is that the likes of Anthropic and OpenAI [moved on from caring about these indices](https://www.interconnects.ai/p/opus-46-vs-codex-53) months, or even years, ago. Their focus is to a large majority on supporting internal uses – like AI R&D automation – and the use-cases of customers who are spending billions of dollars. Yes, they have team members hill climbing on other capabilities to unlock new markets, but these behaviors are to a minority extent driven by public benchmarks. At the same time, it is existential for small to medium sized labs to appear solid on these intelligence indices, as their branding is fully built around catching up to the frontier. It’s a very silly cat and mouse game at times, but one that will be lasting.

The only solution to our current equilibrium with one or two indices dominating the discourse on “where the frontier is” is to have more intelligence indices with other biases, e.g. just for coding (like [CursorBench](https://cursor.com/cursorbench)). In some ways this is forming, with the likes of [Vals AI](https://www.vals.ai/benchmarks/vals_index) and [Intelligence.ai](https://www.intelligence.ai/) growing, but the mindshare is very lopsided towards Artificial Analysis.

Every benchmark creator has their worldview. The dilemma is that you can always shift the numbers to match your narrative. This has always been a challenge of doing good science. The most recurring version of this question has been “how much effort should you put in to tune your training baseline?” More tuning makes your results more trustworthy, but often reduces the apparent success of your method. This is doubly true in the era of agents, where much of the analysis work is done through the lens of LLMs. In evaluation, this comes down to which benchmarks you choose to include in your index.

The benchmarking industry comes down to trust. I have a solid amount of trust for Artificial Analysis, but not the most I do for anyone in the ecosystem. Artificial Analysis’s business model is to make money from the labs to verify their results. They make a lot of money doing this, which is fine. Other benchmarking companies have a similar problem, such as [Arena](https://arena.ai/blog/ai-evaluations), which does pre-release testing for labs to pay the bills.

The entities I trust the most to be *fair* on evaluations are non-profits, academics, and government institutions like CAISI. Today, the institutions I trust to be *accurate* on the frontier evaluation are startups like Artificial Analysis and Arena. So, we are all stuck with trade-offs and dilemmas. No number is perfect and there is a large amount of wiggle room in the numbers for people to take analysis in different directions. This is a second dilemma, one that is sort of my job, the dilemma of a benchmarker of benchmarkers. It is more sociological than scientific.

*Thanks for reading to the end! I wanted to add a note of appreciation to my loyal audience. Much of Interconnects’ growth relies on word of mouth from readers, so if you have a friend you think would like this, please share it.*

[Share](https://www.interconnects.ai/p/artificial-analysis-v5-and-the-benchmarkers?utm_source=substack&utm_medium=email&utm_content=share&action=share)

*I also just published a comprehensive introductory [lecture](https://www.youtube.com/watch?v=79Pnm5hANsA&t=353s) on open models on YouTube, which you may enjoy.*
