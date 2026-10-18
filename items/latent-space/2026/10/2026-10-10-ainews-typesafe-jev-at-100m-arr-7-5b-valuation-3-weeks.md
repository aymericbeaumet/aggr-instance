---
title: '[AINews] TypeSafe/Jev at >$100M ARR, $7.5B valuation 3 weeks after launch'
link: https://www.latent.space/p/ainews-typesafejev-at-100m-arr-75b
source: latent-space
published: 2026-10-10T06:45:57Z
updated: 2026-10-10T06:45:57Z
first_seen: 2026-10-10T09:43:54.203598127Z
summary: Wow.
content: extracted
html: 2026-10-10-ainews-typesafe-jev-at-100m-arr-7-5b-valuation-3-weeks.html
remote_preview:
  url: https://substackcdn.com/image/fetch/$s_!JxMf!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F__ss-rehost__tw-video-preview-13_2108594586310631424.jpg
---

As you can see in the AINews X recap section below, everyone on earth has cloned the Jev API, but only one company can ever create the category. TypeSafe announced their “[Series AI](https://typesafe.ai/blog/series-ai)” and Sequoia “[leaked](https://doomers.ai/work/typesafe-ai-case-study)” that they crossed 100M ARR in their first week.

> Diogo Almeida @CompleteSkeptic
>
> launched a model accidentally served trillions of tokens a day 29.4% of the fortune 500 showed up raised a really big series A from @a16z it's been 3 weeks we would like to sleep now
>
> [@CompleteSkeptic on X](https://x.com/CompleteSkeptic/status/2108594987177021737)

Although there are [cynics and accusations of astroturfing](https://news.ycombinator.com/item?id=50023450), we hope it is evident that [our Jev pod](https://www.youtube.com/watch?v=cFx9Z3ZXca0&t=6155s) was 100% authentic.

[www.youtube.com](https://www.youtube.com/watch?v=cFx9Z3ZXca0)

> AI News for 10/8/2026-10/9/2026. We checked 12 subreddits, [544 Twitters](https://twitter.com/i/lists/1585430245762441216) and no further Discords. [AINews’ website](https://news.smol.ai/) lets you search all past issues. As a reminder, [AINews is now a section of Latent Space](https://www.latent.space/p/2026). You can [opt in/out](https://support.substack.com/hc/en-us/articles/8914938285204-How-do-I-subscribe-to-or-unsubscribe-from-a-section-on-Substack) of email frequencies!

**Decision Models Become a Product Category**

- **The pattern**: Several vendors shipped “decision” models on the same day. These return typed answers (probabilities, picks from a list, scores) in a single forward pass instead of free text. Jev is the reference point everyone benchmarks against, and [@scaling01](https://x.com/scaling01/status/2108631066961477646) remarked on how fast the format spread.

  - **OpenAI Decisions API**: Three request types: probability that a condition is true, pick from a list, or score against levels. It accepts text and images, runs on GPT-6 Luna, costs $0.10/M input tokens with no output charge, and is “up to 10x faster” by OpenAI’s own figure ([@LearnOpenCV](https://x.com/LearnOpenCV/status/2108618320618377654)).

  - **Microsoft-Decision-1**: Positioned for LLM judges and screening scientific hypotheses. An early evaluator says decision models still struggle on consistency and complex decisions ([@omarsar0](https://x.com/omarsar0/status/2108644675166888033)).

  - **Perplexity pplx-decider-v1.1-27b**: Claims top Decision Bench accuracy at 94.5% across 1,071 cases, at $0.017 per 1K decisions ([@perplexitydevs](https://x.com/perplexitydevs/status/2108588583158526461)).

  - **Cloudflare clef**: New clef-omni accepts audio, video, image and text. clef-flash is now cheaper than Jev, and clef overall is about 2x faster ([@michellechen](https://x.com/michellechen/status/2108625984585170988)). Weights are on [Hugging Face](https://x.com/julien_c/status/2108650927288918084).

  - **Liquid d1**: Now on Vercel AI Gateway, with vision support for classify, route and score tasks ([@vercel\_dev](https://x.com/vercel_dev/status/2108722332659589606)).
- **Serving and routing**: vLLM Semantic Router’s Decision 2.0 answers multiple questions about one input in one pass, with per-option probabilities ([@vllm\_project](https://x.com/vllm_project/status/2108441335044997468)). LangSmith uses Jev as a judge that returns separate typed answers for difficulty and correctness on every trace ([@hwchase17](https://x.com/hwchase17/status/2108453917277254058)).

- **Train your own**: Unsloth released a free notebook that turns Qwen3.5-4B into a decision model on 8GB of VRAM ([@UnslothAI](https://x.com/UnslothAI/status/2108568667449618930)). A walkthrough on Qwen3.5-0.8B reports accuracy rising from 37% to 65% in 60 steps, about 10 minutes on 4GB ([@akshay\_pachaar](https://x.com/akshay_pachaar/status/2108658332739485746)).

- **Why harnesses want this**: Many agent steps are yes/no calls rather than generation. LangChain says routing each task to the cheapest adequate model cut median Open SWE cost per task by 64% ([@hwchase17](https://x.com/hwchase17/status/2108777449622245793)).

- **Related research**: Apple/CMU’s Selection-based Structured Reasoning (SSR) applies the same idea inside agents ([@ZhihuFrontier](https://x.com/ZhihuFrontier/status/2108435752891850881)).

  - **Method**: Six natural-language strategies are scored by length-normalized log-likelihood in one batched forward pass that shares the KV cache.

  - **Results**: Per-turn reasoning latency falls by more than 90%, but end-to-end latency per question falls only 28–54%. On Qwen3-VL-4B with GRPO, average success is 61.37% versus 61.25% for a TAPO+GSPO baseline.

**Multi-Agent Orchestration and Coding Tools**

- **Claude Managed Agents dynamic workflows (public beta)**: A lead agent writes a phased plan, fans it out to up to 1,000 agents per run, then merges the results. It is enabled with `multiagent_20261001` ([@ClaudeDevs](https://x.com/ClaudeDevs/status/2108591328732856655), [config](https://x.com/ClaudeDevs/status/2108591331660468538)).

  - **Cost warning**: Anthropic advises starting with scoped tasks because token use can be high ([guidance](https://x.com/ClaudeDevs/status/2108591334449684643)).

  - **Claude Code Projects**: All waitlisted Pro and Max users were admitted. Each project runs tasks as parallel threads ([@ClaudeDevs](https://x.com/ClaudeDevs/status/2108621476538781878)), and sessions can now run locally ([@gem\_ray](https://x.com/gem_ray/status/2108625622889353414)).

  - **Opus 5.5 fast mode**: It has rolled out, but it bills against usage credits and is not included in subscriptions ([@theo](https://x.com/theo/status/2108730886191735218)).
- **Do agent teams pay off?**: Vals AI ran GPT-6 Sol and Opus 5.5 on Vibe Code Bench, alone and as teams ([@ValsAI](https://x.com/ValsAI/status/2108608719420600709)).

  - **Results**: Teams cost 1.8–5.1x more. Only Sol at medium effort improved significantly, by 7.3 points.

  - **Behavior**: Sol delegated in parallel along architectural lines. Opus ran sequential waves, reaching about 6.8 subagents and roughly 1,140 subagent tool calls per app at max effort, with no significant gain ([details](https://x.com/ValsAI/status/2108608723816206365)).
- **Prime Agent rewrites itself in Rust**: Over two weeks, a swarm of more than 2,000 agents used 10K+ sandboxes and 200B+ GLM-5.3 tokens. The result reaches usable input about 13x faster and uses 83% less startup memory ([@PrimeIntellect](https://x.com/PrimeIntellect/status/2108672479007047952)). An accompanying essay argues that context limits lead inevitably to swarms ([essay](https://x.com/PrimeIntellect/status/2108645812591092114)).

- **Codex updates**:

  - **Windows sandbox**: A new mode built on Microsoft Execution Containers (MXC) gives faster setup, network enforcement and granular file controls ([@OpenAIDevs](https://x.com/OpenAIDevs/status/2108573188703781190)).

  - **Composer predictions**: Codex now suggests your next message, in beta for Pro users only ([announcement](https://x.com/OpenAIDevs/status/2108624138369929725)). Some users criticize the Pro-only gating ([@Angaisb\_](https://x.com/Angaisb_/status/2108646791558172772)).

  - **Reliability**: There were complaints of daylong outages ([@dzhng](https://x.com/dzhng/status/2108446839620169806)).

  - **Sentiment**: DHH says GPT-6.1 Sol made Codex his primary tool over Claude ([@dhh](https://x.com/dhh/status/2108518418081054724)).
- **Devin and Grok Bot**: Devins can now spawn trees of managed Devins, so wall time tracks the slowest branch rather than the sum ([@devindevelopers](https://x.com/devindevelopers/status/2108587364926758936)). Devin also accepts personal ChatGPT plans for GPT usage ([@cognition](https://x.com/cognition/status/2108692010056188048)). Separately, Grok Bot gets its own email address for sign-ups and scheduling ([@bot](https://x.com/bot/status/2108609764766908772)).

**Model Releases and Independent Evals**

- **Qwen-Image-2.1-Turbo (open weights)**: An accelerated checkpoint of the 7B Qwen-Image-2.1. It does 8-step 2K generation and natural-language editing, loads through Diffusers `QwenImage21Pipeline`, and launches alongside Pro and Turbo APIs ([@Alibaba\_Qwen](https://x.com/Alibaba_Qwen/status/2108549075218120949)).

- **StepFun Step 5 Preview**: A 600B-total, 27B-active sparse MoE with 1M context and vision ([@omarsar0](https://x.com/omarsar0/status/2108609487338631207)).

  - **Results**: It scores 33.89 on the Hermes Index, matching GPT-6 Luna, and is free on Nous Portal for a week ([@NousResearch](https://x.com/NousResearch/status/2108638389045960958)).

  - **Availability**: It reached #1 on OpenRouter Trending, which measures usage, not quality ([@kimmonismus](https://x.com/kimmonismus/status/2108675280772677914)). Open weights are due October 15. Max output was corrected to 64K tokens ([correction](https://x.com/omarsar0/status/2108767737543634953)).
- **Upstage Solar Mini 4**: A 35B MoE with 3B active, 524K context and 208 tok/s. Its AAII score of 24 is the best at 3B active, within a point of Nemotron 3 Ultra. It is free in Cline ([@cline](https://x.com/cline/status/2108630302381994318)).

- **Gemini 4 Argon**: Reported at 77.9% on DeepSWE v1.1 versus Opus 5.5’s 74.2%. It ships first to 650+ Fairwind Program defenders at $2/$10 per M tokens ([@dl\_weekly](https://x.com/dl_weekly/status/2108648776164319667)).

  - **Signals**: Reasoning-effort selectors have appeared in Antigravity ([@testingcatalog](https://x.com/testingcatalog/status/2108696747874697354)), and Logan Kilpatrick says “Argon is coming” ([@OfficialLoganK](https://x.com/OfficialLoganK/status/2108613251349242028)).

  - **Unconfirmed**: Business Insider reports that an internal “Carbon” checkpoint approaches Opus 5.5 on coding.
- **Speech models**: HeyGen Voice tops the Artificial Analysis Controlled Voice TTS arena with an Elo of 1,201, at $30/1M characters and 40 chars/s ([@ArtificialAnlys](https://x.com/ArtificialAnlys/status/2108608255387935199)). Whistle is a 16.9MB on-device STT model said to rival Whisper base ([@victormustar](https://x.com/victormustar/status/2108526909457867026)).

- **Multi-turn image editing**: Artificial Analysis chained 30 consecutive edits ([@ArtificialAnlys](https://x.com/ArtificialAnlys/status/2108600224478572767)).

  - **Results**: Ideogram 4.5 and FLUX 3 edit locally, leaving 95%+ of the image untouched on small edits. GPT Image 2.5 Sunburst re-renders most of the frame each turn, keeping only about 20% unchanged, so it drifts. Nano Banana 2.1 gradually darkens.
- **OCR benchmarks**: Roboflow’s new benchmark covers 48 models, with GPT-6 Astra leading text localization ([@skalskip92](https://x.com/skalskip92/status/2108603073509925105)). Datalab’s OmniParseBench has 16K tests across 90 languages, and its own model does not rank first ([@VikParuchuri](https://x.com/VikParuchuri/status/2108668863231451578)).

- **Arena roundup**: Claude Haiku 5.5 ranks #30 on WebDev at $0.10/$0.50, matching GPT-6 Luna’s price while scoring 6 points higher. Mistral Large 4 sits at #43 on Agent Arena ([@arena](https://x.com/arena/status/2108568443037614515)). On ARC-AGI-3, a new high score of 59.17% ([@arcprize](https://x.com/arcprize/status/2108577347192676612)).

**Research, Training and Inference Systems**

- **vLLM and SGLang on Vera Rubin**: vLLM reports more than 7.8x GB200 throughput on MiniMax M3 at matched interactivity on AgentX. These are early results ([@vllm\_project](https://x.com/vllm_project/status/2108736734309896625)).

  - **Technique**: Locality-aware MoE uses CUDA 13.4 locality domains so each SM reads only local HBM, worth up to 1.2x faster MoE decode ([details](https://x.com/vllm_project/status/2108736793541800036)).

  - **SGLang**: Up to 20% faster FP8 MLA at 128K context, and a 5.9% end-to-end gain from MoE tail fusion that removes 276 launches per decode step ([@sgl\_project](https://x.com/sgl_project/status/2108699825005060496)).

  - **SemiAnalysis claims**: A preview InferenceX submission shows 3.2x profit per gigawatt and up to 10x performance per dollar versus GB300 ([@SemiAnalysis\_](https://x.com/SemiAnalysis_/status/2108604228440658369)).
- **TRL v1.15**: The fused LM head is now on by default and avoids materializing the full logits tensor ([@LysandreJik](https://x.com/LysandreJik/status/2108549658440241376)).

  - **Results**: On Gemma 3 1B, GRPO sequence length rises from 28K to 114K and DPO from 10K to 59K. Peak memory at 8K falls 52–82%, and training is up to about 11% faster.
- **Data and post-training services**:

  - **Datology Curation Studio**: Claims a 6x compute multiplier on 39 open datasets for a 30B MoE ([@pratyushmaini](https://x.com/pratyushmaini/status/2108587864644870540)). It also cites Thomson-1, trained for $450K, beating GPT-5.6 Sol head-to-head ([@arimorcos](https://x.com/arimorcos/status/2108578519920021944)).

  - **Tinker**: Price cuts of up to 70%, long-context priced the same as short, and GLM-5.3-Flash and DeepSeek-v4.1-Flash added ([@tinkerapi](https://x.com/tinkerapi/status/2108670587241697771)).
- **DeepSeek periodic weak spots**: ByteDance Seed finds that retrieval depends on where a token lands relative to the compression stride ([@ZhihuFrontier](https://x.com/ZhihuFrontier/status/2108475220298551728)).

  - **Evidence**: The pattern persists without RoPE or learned gates, and tracks stride length.

  - **Interpretation**: V4.1’s stride of 2 reduces but does not eliminate the effect.
- **Agent research**:

  - **Agent plasticity (Meta)**: Measures held-out gain per learning dollar. The best performers are not the most efficient learners ([@omarsar0](https://x.com/omarsar0/status/2108580251127439710)).

  - **MIMESIS**: A 9B user simulator that beats Opus 5 on behavioral fidelity by 13.4 points ([@dair\_ai](https://x.com/dair_ai/status/2108588052914598084)).

  - **Base-model selection (NVIDIA)**: Ranks checkpoints by whether the base model can reproduce the “decisive edit,” a signal that tracks post-trained SWE-bench Verified scores ([@dair\_ai](https://x.com/dair_ai/status/2108584781877522655)).
