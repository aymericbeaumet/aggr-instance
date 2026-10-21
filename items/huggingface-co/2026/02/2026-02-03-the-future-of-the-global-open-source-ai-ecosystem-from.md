---
title: 'The Future of the Global Open-Source AI Ecosystem: From DeepSeek to AI+'
link: https://huggingface.co/blog/huggingface/one-year-since-the-deepseek-moment-blog-3
source: huggingface-co
published: 2026-02-03T15:03:19Z
updated: 2026-02-03T15:03:19Z
first_seen: 2026-10-04T12:50:30.804479567Z
content: extracted
html: 2026-02-03-the-future-of-the-global-open-source-ai-ecosystem-from.html
---

* * *

This is the third and final blog in a three-part series on China's open source community's historical advancements since January 2025's "DeepSeek Moment." The first blog on strategic changes and open artifact growth is available [here](https://huggingface.co/blog/huggingface/one-year-since-the-deepseek-moment), and the second blog on architectural and hardware shifts is available [here](https://huggingface.co/blog/huggingface/one-year-since-the-deepseek-moment-blog-2).

In this third article, we examine paths and trajectories of prominent Chinese AI organizations, and posit future directions for open source.

For AI researchers and developers contributing to and relying on the open source ecosystem and for policymakers understanding the rapidly changing environment, **due to intraorganizational and global community gains, open source is the dominant and popular approach for Chinese AI organizations for the near future. Openly sharing artifacts from models to papers to deployment infrastructure maps to a strategy with the goal of large-scale deployment and integration.**

### China's Organic Open Source AI Ecosystem

Having examined strategic and architectural changes since DeepSeek's R1, we get a glimpse for the first time at how **an organic open source AI ecosystem is taking shape in China.** A culmination of powerful players, some established in open source, some new players, and some changing course entirely to contribute to the new open culture, signal that the open collaborative approach is mutually beneficial.

This collaboration is reaching beyond national boundaries; the most followed organization on Hugging Face is DeepSeek, and the fourth most followed is Qwen.

[![Screenshot 2026-02-03 at 09.50.11](https://cdn-uploads.huggingface.co/production/uploads/63a369d98c0c89dcae3b8329/HIqQbjFfaiO2W-9cQ7GPE.png)](https://cdn-uploads.huggingface.co/production/uploads/63a369d98c0c89dcae3b8329/HIqQbjFfaiO2W-9cQ7GPE.png)

In addition to models, openly sharing science and techniques has not only informed other AI organizations, but also the entire open source community. The most popular papers on Hugging Face largely come from Chinese organizations, namely ByteDance, DeepSeek, Tencent, and Qwen.

[![Screenshot 2026-02-02 at 09.10.35](https://cdn-uploads.huggingface.co/production/uploads/63a369d98c0c89dcae3b8329/CZfhMjHHhR2nVGs20F0le.png)](https://cdn-uploads.huggingface.co/production/uploads/63a369d98c0c89dcae3b8329/CZfhMjHHhR2nVGs20F0le.png) *source: [https://huggingface.co/spaces/evijit/PaperVerse](https://huggingface.co/spaces/evijit/PaperVerse)*

#### The Established

**Alibaba** positioned open source as an ecosystem and infrastructure strategy. [Qwen](https://huggingface.co/Qwen) was not shaped as a single flagship model, but continuously expanded into a family covering multiple sizes, tasks, and modalities, with frequent updates on Hugging Face and their own platform ModelScope. Its influence did not concentrate on any single version. Instead, it was repeatedly reused as a component across different scenarios, gradually taking on the role of a general AI foundation. **By mid-2025, Qwen became the model with most derivatives on Hugging Face, with over 113k models using Qwen as a base, and over 200k model repositories tagging Qwen,** far exceeding Meta's Llama's 27k or DeepSeek's 6k. Organization-wide, Alibaba boasts the most derivatives almost as much as both Google and Meta combined.

At the same time, Alibaba aligned model development with cloud and hardware infrastructure, integrating models, chips, platforms, and applications into a single engineering stack.

**[Tencent](https://huggingface.co/tencent)** also made a significant move from borrowing to building. As one of the first major companies to integrate DeepSeek into core consumer-facing products after R1's release, Tencent did not initially frame open source as a public narrative. Instead, it brought mature models in through plug-in style integration, ran large-scale internal validation, and only later began to release its own capabilities. From May 2025 onward, Tencent accelerated open releases in areas where it already had strengths, such as vision, video, and 3D with its own brand named Tencent Hunyuan (it's now Tencent HY), and these models quickly gained adoption in the community.

**[ByteDance](https://huggingface.co/ByteDance)**, by following its "AI application factory" approach, starts selectively open sourcing high value components while keeping its competitive focus on product entry points and large scale usage. In this context, the ByteDance Seed team has contributed several notable open-source artifacts, including **[UI-TARS-1.5](https://huggingface.co/ByteDance-Seed/UI-TARS-1.5-7B)** for multimodal UI understanding, \*\*[Seed-Coder](https://huggingface.co/collections/ByteDance-Seed/seed-coder) \*\*for data-centric code modeling, and the **[SuperGPQA](https://huggingface.co/datasets/m-a-p/SuperGPQA)** dataset for systematic reasoning evaluation. Despite a relatively low-profile open-source presence, ByteDance has achieved significant scale in China's AI market, with its AI application **Doubao** surpassing **100 million DAU** in December 2025.

The most notable change within **[Baidu](https://huggingface.co/baidu)**, whose CEO openly calls short on open source, also started its shift: after years of prioritizing closed models, it re-entered the ecosystem through free access and open release, such as the **[Ernie 4.5 series](https://huggingface.co/collections/baidu/ernie-45)**. This shift was accompanied by renewed investment in its open-source framework, **[PaddlePaddle](https://huggingface.co/PaddlePaddle)**, as well as its own AI chip **Kunlunxin**, which announced an IPO on January 1, 2026. By connecting models, chips, and PaddlePaddle within a more open system, Baidu can lower costs, attract developers, and influence standards, while maintaining strategic control under shared constraints of compute, cost, and regulation.

#### The Normalcy of "DeepSeek Moments"

Among startups, **[Moonshot](https://huggingface.co/moonshotai)**, **[Z.ai](https://huggingface.co/zai-org)**, and **[MiniMax](https://huggingface.co/MiniMaxAI)** adjusted rapidly and brought new momentum to the open-source community within months after R1. Models such as **[Kimi K2](https://huggingface.co/moonshotai/Kimi-K2-Thinking)**, **[GLM-4.5](https://huggingface.co/zai-org/GLM-4.5)**, and **[MiniMax M2](https://huggingface.co/MiniMaxAI/MiniMax-M2)** all earned places on AI-World's open-model milestone rankings.At the end of 2025, Z.ai and MiniMax released their most advanced open-source models to date and subsequently announced their IPO plans in close succession.

The open-sourcing of Kimi K2 was widely described as a "Another DeepSeek moment" for the community. Although Moonshot has not announced an IPO, market reports indicate that the company raised approximately $500M in funding by the end of 2025, with AGI and agent-based systems positioned as its primary commercialization objectives.

Those application-first companies such as **[Xiaohongshu](https://huggingface.co/rednote-hilab)**, **[Bilibili](https://huggingface.co/IndexTeam)**, **[Xiaomi](https://huggingface.co/XiaomiMiMo), and [Meituan](https://huggingface.co/meituan-longcat)**, previously focused only on the application layer, began training and releasing their own models. With their native advantage in real usage scenarios and domain data, once strong reasoning became available at low cost through open source, building in-house models became practical. It tunes AI around their specific businesses, rather than being constrained by the cost structures or limits of external providers.

If the business world seized the ROI positive opportunity for growth, research institutions and the broader community welcomed the shift even more willingly. Organizations such as **[BAAI](https://huggingface.co/BAAI)** and **[Shanghai AI Lab](https://huggingface.co/internlm)** redirected more effort toward toolchains, evaluation systems, data platforms, and deployment infrastructure, projects like FlagOpen, OpenDataLab, and OpenCompass. These efforts did not chase single-model performance, but instead strengthened the long-term foundations of the ecosystem.

### Foundations for the Future

The defining feature of the new ecosystem is not that there are more models, but that an entire chain has formed. Models can be open-sourced and extended; deployments can be reused and scaled; software and hardware can be coordinated and swapped; and governance capabilities can be embedded and audited. This is a shift from isolated breakthroughs to a system that can actually run in the real world.

This ecosystem did not appear overnight. It is built on years of accumulated infrastructure "tailwind" since 2017. **Over the past several years, China has periodically invested in data centers and compute centers, gradually forming a nationwide, integrated compute layout centered on the "East Data, West Compute" strategy**. The national plan established 8 major compute hubs and 10 data center clusters, guiding compute demand from the east toward the central and western regions.

**Public information suggests China intends to invest in continual growth in energy capacity**. China's total compute capacity is around [1590 EFLOPS](http://english.scio.gov.cn/pressroom/2026-01/22/content_118293753.html) as of the year 2025, ranking among the top globally. Sources in China assert that intelligent compute capacity, tailored for AI training and deployment, is expected to [grow by roughly 43% year over year](https://www.scmp.com/economy/china-economy/article/3323046/china-bets-ai-pump-trillions-economy-amid-national-computing-push), far outpacing general-purpose compute. At the same time,the average data center power usage effectiveness (PUE) fell to around 1.46, indicating better effectiveness and providing a solid hardware foundation for AI at scale. Energy is a clear key focus.

If the 2017 "New Generation AI Development Plan" was mainly about setting direction and building foundations, then **the August 2025 "AI+" action plan clearly shifted focus toward large-scale deployment and deep integration. This marks a directionally different pursuit from AGI**. The emergence of R1 provided the missing "lift" at the engineering and ecosystem level. It was the catalyst that systematically activated compute, energy, and data infrastructure that had already been built.

As a result, in the year following R1's release, China's AI development accelerated along two main paths. First, AI became more deeply embedded in industrial processes, moving beyond chatbots toward agents and workflows. Second, **greater emphasis was placed on autonomous and controllable AI systems**, reflected in more flexible training pathways and increasingly localized deployment strategies.

Looking back, the real turning point was not the growth in the number of models, but a **fundamental change in how open-source models are used**. Open source moved from an optional choice to a **default assumption in system design**. Models became reusable and composable components within larger engineering systems.

### Looking Back to Look Forward

From DeepSeek to "AI+", China's path in 2025 was not about chasing peak performance. It was about building a **practical path organized around open source, engineering efficiency, and scalable delivery**, a path that has already begun to run on its own.

Resource constraints did not limit China's AI development. In some respects, they reshaped its trajectory. The release of DeepSeek R1 acted as a catalytic event, triggering a chain of responses across the domestic industry and accelerating the formation of a more organically structured ecosystem. At the same time, this shift created a critical window for continued domestic research and development. As this ecosystem matures, its longer-term impact---and how the global AI community may engage with an increasingly self-sustaining AI ecosystem in China---will become important questions for future discussion.
