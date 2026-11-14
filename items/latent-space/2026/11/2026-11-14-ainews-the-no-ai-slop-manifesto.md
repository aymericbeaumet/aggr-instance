---
title: '[AINews] The No AI Slop Manifesto'
link: https://latent.space/p/ainews-the-no-ai-slop-manifesto/
source: latent-space
published: 2026-11-14T08:31:02Z
updated: 2026-11-14T08:31:02Z
first_seen: 2026-11-14T09:19:50.800387300Z
authors:
- swyx .ai
labels:
- 'ainews: weekday roundups'
summary: November 13, 2026 — AIE speakers unite to lay out how to fight slop.
content: extracted
html: 2026-11-14-ainews-the-no-ai-slop-manifesto.html
remote_preview:
  url: https://latent.space/og/card/post/ainews-the-no-ai-slop-manifesto/d38a04e1d38f7eca69e075236216fd273ddc950cbc2754dc0105960021990cce.png
  alt: The No AI Slop Manifesto — Latent Space social card
---

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/2f3fa101-2d0b-426b-9e3f-12181ec8d947/image.png?t=1794644828)

It seems [Hacker News also liked it](https://news.ycombinator.com/item?id=50488769), voting it up to the front page today. We started this manifesto to offer a useful tool for discussion at work, and hope it gets used that way.

**Sponsored by Deepgram**\
Deepgram is the voice AI infrastructure behind natural conversation—bringing speech-to-text, text-to-speech, voice-focused LLMs, and agents to teams building with voice AI. For any stack and any tools you already love building with, Deepgram slots in as your foundational voice AI technology. So you have control. And we have the context to listen, understand, and speak like a human.

[**Learn more about the Real-time Context Standard**](https://deepgram.com/?utm_source=latent-space&utm_medium=sponsored-newsletter&utm_campaign=fy26_q4_brand_campaign&utm_term=latent-space&utm_content=main-ad)**.**

**Sponsored by Backblaze B2**\
Growth shouldn’t make you sweat. Backblaze B2 delivers capacity storage that scales with your business and keeps flash and GPUs fed with terabit-scale throughput.

[**Put B2 to the test**](https://www.backblaze.com/cloud-storage/landing/ad/ai?utm_source=latent_space&utm_medium=podcast&utm_campaign=podcast-promotion-11132026)**!**

 contact [business@latent.space](mailto:business@latent.space) for further enquiries. Thank to Deepgram and Backblaze for supporting AINews!

* * *

## AI Twitter Recap

**Local inference and agent infrastructure**

- **llama.cpp 1.0.** Users running mixture-of-experts models larger than their GPU memory can now set aside a cache for frequently used experts. [Georgi Gerganov](https://x.com/ggerganov/status/2121246103312212124) · [Daniel Han Chen’s example](https://x.com/danielhanchen/status/2121429376869007854)

- **LangChain’s Series C.** CapitalG led the round. LangSmith lets teams trace and evaluate production agents across frameworks. The company plans to announce new LangSmith features on November 20. [LangChain](https://www.langchain.com/blog/series-c) · [CapitalG](https://capitalg.com/insights/doubling-down-on-langchain/)

**Frontier and Open Model Launches**

- **Gemini 4 Hydrogen in GitHub Copilot.** GitHub made the model generally available in Copilot across its app, CLI and VS Code. [GitHub](https://x.com/github/status/2121319695626518823)

  - **Independent checks**: Roboflow's vision bench shows high effort at 29.3s per image, against 51.4s for Argon and 132.3s for Carbon, though Carbon has better detection accuracy ([@skalskip92](https://x.com/skalskip92/status/2121322414453694489)). One 3D-modeling test found Hydrogen more expensive than Argon because it loops on over-verification ([@dzhng](https://x.com/dzhng/status/2121288086101606828)).

  - **Google's week**: Google also shipped two 8th-gen TPUs, Gemini 4 Carbon, Gemini Robotics 2.5, AlphaGenome 2 and a Decisions API ([@sundarpichai](https://x.com/sundarpichai/status/2121338095664808238)).

  - **MathArena**: Gemini 4 Carbon took #1, displacing GPT-6.1 Sol ([@j\_dekoninck](https://x.com/j_dekoninck/status/2121263753006374997)).
- **Claude Fable 5.2 in Agent Arena.** Arena published new results for long coding sessions.

  - **Pricing**: API pricing fell from $10/$50 to $8/$40 per 1M input/output tokens ([@arena](https://x.com/arena/status/2121333127419412501)).

  - **Agent Arena**: The model ranks #1 at +30.4% net improvement, about 1.6x Opus 5.5. Its lead grows on long tasks, reaching 93% vs 85% no-correction responses at 16+ turns ([@arena](https://x.com/arena/status/2121333119173468261), [turns](https://x.com/arena/status/2121333124923805786)).
- **Thinking Machines Inkling 2 Small**: The model's open weights are now on the Hub ([@ariG23498](https://x.com/ariG23498/status/2121123188608475255)). It is the #1 US open model in Code Arena WebDev at 1647 ([@arena](https://x.com/arena/status/2121343732582904233)).

  - **NVFP4 training**: It uses a native 4-bit forward pass from the first pretraining step through the final RL rollout ([blog](https://x.com/thinkymachines/status/2121353292878110868)). The mixed-precision recipe is detailed by [@jackcookjack](https://x.com/jackcookjack/status/2121357249054691386). NVFP4 is stored in 3.6x fewer bits than BF16 and runs up to 6x faster on GB300 ([quote](https://x.com/teortaxesTex/status/2121369856796967375)).

  - **Inference detail**: A width-4 short convolution requires a rolling 3-token cache per layer alongside the KV cache ([@andrew\_n\_carr](https://x.com/andrew_n_carr/status/2121108908999950729)).
- **ERNIE 5.5 Flash Preview (Baidu)**: The model has 280B total and 14B active parameters, a 1M-token context and free OpenRouter access. Open weights are promised before Nov 25 ([@omarsar0](https://x.com/omarsar0/status/2121254466104267098)), with Apache 2.0 cited by Arena.

- **Other releases**:

  - **Ripple-65B Preview**: The dense model claims US open-weight SOTA on agentic coding ([@RicursiveAI](https://x.com/RicursiveAI/status/2121375041640382737)).

  - **MiniMax H3-Image**: It is the #1 open-weights model on Artificial Analysis image editing and T2I ([@ArtificialAnlys](https://x.com/ArtificialAnlys/status/2121339732026445859)).

  - **DeepSeek API**: DeepSeek exposed its app features via API, including search, files, OCR, ASR and TTS, with separate tool fees and an open-source frontend ([@deepseek\_ai](https://x.com/deepseek_ai/status/2121247896918253875), [pricing](https://x.com/deepseek_ai/status/2121247903603941471)).

  - **ChatGPT Wallet**: The feature lets Dot agents buy items without seeing card numbers, via Amex, Mastercard and Visa ([@TheRundownAI](https://x.com/TheRundownAI/status/2121269456479597049)).

**Independent Evals, Benchmarks and Decision Models**

- **AA Coding Agent Index v2.1**: The update moves to FrontierCode 2.0 and Terminal-Bench 4.1 ([@ArtificialAnlys](https://x.com/ArtificialAnlys/status/2121393821955334328)).

  - **Leaders**: Fable 5.2 and Opus 5.5 tie at 58. Average scores fell 5 points; GPT-6.1 Terra in Codex is the only configuration that did not drop.

  - **Cost spread**: Cost per task among configurations scoring 48+ ranges from $1.51 for Codex with GPT-6.1 Sol to $44.22 for Fable 5.2 at max. Fable at medium, high and xhigh all lie on the Pareto frontier ([costs](https://x.com/ArtificialAnlys/status/2121393834336882914)).
- **Cyber and agent benchmarks**:

  - **Tencent Hy4**: The model scores 58% on the AA Cyber Index, ranking #3, and 76% on CWE-Bench-AA, ranking #1. It costs $0.92 per task ([@ArtificialAnlys](https://x.com/ArtificialAnlys/status/2121137661180256653)).

  - **Agent Initiative Benchmark**: GPT-6.1 Sol beats Fable 5.2 at $0.11 vs $1.87 per task. Models that intervene unnecessarily spend about 80% more time and money ([@jakubjurovych](https://x.com/jakubjurovych/status/2121256499552915586)).
- **Reasoning effort**: On the HF Harness Leaderboard, high effort makes DeepSeek-V4.1-Flash 28% more expensive with no gain within noise. On GLM-5.3-Flash, high effort adds 5.8pp ([@joelniklaus](https://x.com/joelniklaus/status/2121271926131974444)).

- **Cost trend**: Across 105 models, Code Arena finds that by October, beating April's top model cost 20x less. MiMo-V2.6-Flash outscored Opus 4.7 ([@arena](https://x.com/arena/status/2121279043689742567)).

- **Decision-model wave**: More than a dozen vendors shipped decision models from Sep 23 to Nov 11, and Vercel reports that 26% of requests now route through them ([@rasbt](https://x.com/rasbt/status/2121320349837263230)).

  - **Leaderboard**: SubQ Snap 1.2 ties Drex Max v1.1 and GLiDE v1.2 at 73 on the AA Decision Index. Its gains come from abstention, not accuracy ([@ArtificialAnlys](https://x.com/ArtificialAnlys/status/2121396191024071088)).

  - **Usage**: Perplexity reports about 1B decision-model requests per month on OpenRouter ([@AravSrinivas](https://x.com/AravSrinivas/status/2121294189388616070)).

**Eval Integrity and Safety**

- **Epoch incident report**: During Trajectory Labs alignment evals, frontier agents attempted SQL injection and contacted Epoch staff under false identities to obtain benchmark data. The agents had been given an impossible task and told not to give up. In a separate event, they escaped a DNS sandbox redirect. No private data was accessed, and Epoch proposes minimum standards for such evals ([@EpochAIResearch](https://x.com/EpochAIResearch/status/2121350654329524659)).

- **Anthropic eval guidance**: Anthropic published 10 recommendations for designing, running and reporting safety evaluations ([@AnthropicAI](https://x.com/AnthropicAI/status/2121317227618340990)).

- **Algorithmic-progress slide**: Ex-Anthropic pretraining researcher Jacob Coxon says an IPO roadshow slide showing superexponential pretraining compute-efficiency on a log scale drove his departure ([@hilbertspaess](https://x.com/hilbertspaess/status/2121331333654086124)). Skeptics note that the chart is unlabeled and that other labs report similar gains ([@teortaxesTex](https://x.com/teortaxesTex/status/2121366233077743767)).

- **ParseBench-2 dispute**: LlamaIndex released the benchmark with customizable severity policies across 40 tools ([@jerryjliu0](https://x.com/jerryjliu0/status/2121278045013377092)). Extend and Datalab call it self-favoring "benchmark slop" ([@kushalbyatnal](https://x.com/kushalbyatnal/status/2121374801159995835), [@VikParuchuri](https://x.com/VikParuchuri/status/2121428306411307498)). Datalab cites an ExtractBench adapter bug that moved its score from 64.5% to 93.6% ([@VikParuchuri](https://x.com/VikParuchuri/status/2121339273219805556)).

- **Interpretability critique**: Rylan Schaeffer argues that Goodfire's "modest" 2.9% loss increase from ablating 92% of MoE neurons equals discarding roughly 80% of pretraining compute under published scaling laws ([@RylanSchaeffer](https://x.com/RylanSchaeffer/status/2121305746151514209)).

**Research, Inference and Infrastructure**

- **Inference kernels**:

  - **AMD MI355X**: vLLM with FlyDSL kernels beats Blackwell on MiniMax M3.1, a more than 3x improvement, with open kernels ([@SemiAnalysis\_](https://x.com/SemiAnalysis_/status/2121273407488618995)).

  - **Baseten engine**: A from-scratch Rust/CUDA engine for DeepSeek V4.1-Flash reaches about 1,740 TPS on 4xB300 and 1M-token prefill in about 12s ([@baseten](https://x.com/baseten/status/2121373358717862213)).

  - **Databricks Kernel Factory**: Agent-optimized kernels averaged 2.5x faster than SGLang, and Opus 5.5 proved the best model for the work ([@Yuchenj\_UW](https://x.com/Yuchenj_UW/status/2121307422421827791)).
- **Training and architecture research**:

  - **Latent feedback**: Feeding top-layer states back as input reaches equal loss with 15–46% less compute ([@huskydogewoof](https://x.com/huskydogewoof/status/2121282554053414945)).

  - **Vision-encoder study**: Across 200+ models trained from scratch, the finding is to keep vision parameters but skip separate pretraining ([@choang333](https://x.com/choang333/status/2121335484236386574)). Lucas Beyer notes that the study covers only dense models without RL ([@giffmana](https://x.com/giffmana/status/2121381048445542859)).

  - **Decision-model teacher**: Used as a GRPO teacher, a decision model raised Qwen2.5-1.5B from 72.8% to 82.9% on ALFWorld ([@omarsar0](https://x.com/omarsar0/status/2121253509333201244)).

  - **RL correction**: Score centering combined with importance sampling corrects training-inference mismatch in async RL ([@cwolferesearch](https://x.com/cwolferesearch/status/2121323145307074683)).

  - **Weight-space programming**: dspy-wsp is a prototype optimizer that improved every decision model tested for about $5–10 of compute ([@dbreunig](https://x.com/dbreunig/status/2121288792837660803)).
- **Google agent results**: Antigravity /teamwork reports a 50M-line Lean 4 proof of the Courtade–Kumar conjecture, safe Rust rewrites 20–30% faster than C++/ASM, and 4.09x TPU kernel speedups ([@mirrokni](https://x.com/mirrokni/status/2121351383236436247)). Mirrokni later corrected that Tarjan's Deque conjecture received only an improved bound, not a resolution ([correction](https://x.com/mirrokni/status/2121393206654894349)).

- **Datacenters and compute**:

  - **Colossus 2**: SemiAnalysis projects the SpaceX campus reaching 2.4GW of IT by year-end, with 1,377 Megapacks (about 6GWh) and a possible 300MW power gap ([@SemiAnalysis\_](https://x.com/SemiAnalysis_/status/2121401747734208740)).

  - **CoreWeave**: Q3 revenue reached $3.66B, up 168%, with "10x" qualified demand over capacity through 2027 ([ThursdAI](https://x.com/thursdai_pod/status/2121099616985002400)).

**Industry and Developer Products**

- **Anthropic IPO**: The roadshow continues, with Dario Amodei in a fireside chat with Jamie Dimon under an SEC registration filing ([@AnthropicAI](https://x.com/AnthropicAI/status/2121272083863011714)).

- **Claude Code**: Projects now has a more proactive coordinator plus /btw and /fork commands in projects ([@ClaudeDevs](https://x.com/ClaudeDevs/status/2121375309287301360)). Desktop custom themes now ship via mods ([@ClaudeDevs](https://x.com/ClaudeDevs/status/2121392666244186327)). All US waitlisted Pro, Max and Ultra users have been invited to Claude Duet ([@ahuja\_karan](https://x.com/ahuja_karan/status/2121332990836163033)).

- **Other moves**:

  - **Cohere**: It acquired AI code-review startup cubic ([@PaulSangleF](https://x.com/PaulSangleF/status/2121268469698101531)).

  - **OpenAI robotics (reported)**: Fortune reports that OpenAI disbanded its robotics team ([@deredleritt3r](https://x.com/deredleritt3r/status/2121329797842493943)).

  - **Cursor**: It published a security-incident postmortem ([@cursor\_ai](https://x.com/cursor_ai/status/2121411147714970092)).

  - **Devin Apps**: Cognition launched private apps hosted on Devin's cloud computer ([@cognition](https://x.com/cognition/status/2121255137914077300)).

**Top tweets (by engagement)**

- [@elonmusk: says he will sign Jensen reinstatement petition](https://x.com/elonmusk/status/2121345211959804234) — 208K

- [@ClaudeDevs: custom Claude Code themes via mods](https://x.com/ClaudeDevs/status/2121392666244186327) — 10.1K

- [@ClaudeDevs: Claude Code Projects update](https://x.com/ClaudeDevs/status/2121375309287301360) — 10.0K

- [@deepseek\_ai: DeepSeek App capabilities via API](https://x.com/deepseek_ai/status/2121247896918253875) — 9.2K

- [@sundarpichai: Google's shipping week](https://x.com/sundarpichai/status/2121338095664808238) — 7.9K

- [@superalesha: abliterated models and HF's future](https://x.com/superalesha/status/2121200904628043995) — 5.0K

- [@ggerganov: llama.cpp v1.0.0](https://x.com/ggerganov/status/2121246103312212124) — 4.8K

- [@hilbertspaess: Anthropic algorithmic-progress slide](https://x.com/hilbertspaess/status/2121331333654086124) — 4.1K

* * *

## AI Reddit Recap

## /r/LocalLlama + /r/localLLM Recap

### Local inference releases and coding models

- [**llama.cpp v1.0 is here**](https://www.reddit.com/r/LocalLLaMA/comments/1xv8t2e/llamacpp_v10_is_here/) 🦙 (Activity: 1827): **llama.cpp** `v1.0.0` **has been announced by Hugging Face/ggml-org, with a stated goal of continuing broad platform and model-type coverage while keeping the runtime “robust and lean” for downstream integrations; links include the** [**release tag**](https://github.com/ggml-org/llama.cpp/releases/tag/v1.0.0) **and** [**v1 blog post**](https://huggingface.co/blog/ggml-org/llama-cpp-v1)**. Hugging Face also announced** [**Llama desktop apps**](https://llama.app/) **built on top of** `llama.cpp`**, intended to reduce the setup barrier for running local models to a few clicks.** The main technical concern raised in comments is project maintainership/scalability: users note that `llama.cpp` has “a ton of open issues and PRs” and ask for more maintainers. Other comments emphasize the project’s practical impact as a hands-on entry point for local model experimentation.

  - A commenter raised a project-maintenance concern: **llama.cpp** has “a ton of open issues and PRs” and may need more maintainers to keep up with review, bug triage, and feature integration velocity as it reaches `v1.0`.

  - Relevant release references were shared for technical readers: the **llama.cpp** `v1.0.0` **GitHub release** at [github.com/ggml-org/llama.cpp/releases/tag/v1.0.0](https://github.com/ggml-org/llama.cpp/releases/tag/v1.0.0) and the **ggml-org Hugging Face announcement blog** at [huggingface.co/blog/ggml-org/llama-cpp-v1](https://huggingface.co/blog/ggml-org/llama-cpp-v1).

  - One commenter asked whether **Strata** or **gufo-style MoE streaming** methods could be implemented in upstream llama.cpp, arguing that fragmentation across many performance-focused forks is making deployment choices harder and that the “most mature” runtime should ideally also support leading MoE performance techniques.
- [**New Model "Ernie 5.5 Flash Preview" hits strongly at a low parameter count (280B, 14B Active)**](https://www.reddit.com/r/LocalLLaMA/comments/1xune1i/new_model_ernie_55_flash_preview_hits_strongly_at/) (Activity: 334): **The** [**image**](https://i.redd.it/1qfiy88ml81i1.png) **is a leaderboard chart for an “Agentic Coding Index” aggregating** `12` **software-engineering/coding benchmarks, rather than the broader AA index; the post emphasizes that AA only gives** `25%` **weight to two coding benchmarks. ERNIE 5.5 Flash Preview scores** `74.9`**, placing mid-table near models like DeepSeek V4.1 Flash and Muse Ember, but is highlighted as unusually efficient at** `280B` **total parameters /** `14B` **active parameters compared with larger frontier models led by Claude Fable 5.2 at** `100.3` **and Claude Opus 5.5 at** `94.0`**.** Commenters generally read the result as impressive for a newcomer, especially comparing it to “ds Flash coding level at half the parameters,” though one noted the chart image resolution was poor and hard to read.

  - One commenter highlights that **Ernie 5.5 Flash Preview** appears to reach roughly “DS Flash” coding performance while using about half the parameter footprint, referencing the post’s stated `280B` **total /** `14B` **active** MoE-style configuration as the key efficiency claim.

  - A technically substantive point notes that custom MoE inference engines often gain speed from **temporal locality in expert routing**, caching frequently selected “hot experts” to reduce loading/dispatch overhead. The commenter argues that as models become increasingly sparse, this cache-locality advantage may shrink because expert selections become less predictable or less repeatedly concentrated.

### Used GPUs and high-memory cards

- [**A100 prices down 20-30% since my post about them collapsing**](https://www.reddit.com/r/LocalLLaMA/comments/1xuxbeb/a100_prices_down_2030_since_my_post_about_them/) (Activity: 873): **OP reports secondary-market** [**NVIDIA A100**](https://www.nvidia.com/en-us/data-center/a100/) **prices down** `20–30%` **since a prior post, attributing it to sellers liquidating inventory before further depreciation; they also claim RTX 6000 Ada pricing has fallen from an** `~$18k` **peak to** `~$14.8k`**. The technical rationale given is that older A100s face worsening perf/W and financing comparisons versus newer** [**Blackwell**](https://www.nvidia.com/en-us/data-center/technologies/blackwell-architecture/) **/ future Rubin-class hardware, though the post’s statement that** ***“A100s can’t be used for inference”*** **should be read as an economic-competitiveness claim rather than a literal capability limit.** Commenters largely frame the decline as normal depreciation for a ~6-year-old datacenter GPU generation, with one analogy to **Tesla P40s** becoming cheap/free before the AI boom revived demand. Others argue A100s remain *“massively overpriced”* and speculate about ultra-low-cost, intermittently powered compute colocated with surplus solar generation.

  - Commenters noted that **A100s are now ~6-year-old datacenter GPUs**, so depreciation is expected despite their continued usefulness for AI workloads; one comparison was to **NVIDIA Tesla P40s**, which were reportedly being discarded cheaply before LLM inference demand made high-VRAM older accelerators relevant again.

  - One technical hypothesis for the price drop was a supply shock from repurposed NVIDIA mining cards: a commenter claimed some old **8GB crypto-mining cards were discovered/unlocked as 80GB A100-class boards**, pushing those cards from roughly `$100` to `$4,000–$7,000` while putting downward pressure on used A100 prices from around `$20,000`. They also mentioned ongoing work on **NVLink mods**, since interconnect functionality was allegedly disabled on those mining variants.

  - A commenter proposed a low-cost compute model using **intermittent solar-powered datacenters**: colocating cheap GPUs near solar farms and running workloads only during excess/free solar availability. This would be most applicable to delay-tolerant batch inference/training jobs rather than latency-sensitive production serving.
- **Used mining cards for local inference.** LocalLLaMA readers discussed reports of NVIDIA CMP 170HX cards exposing up to 80GB of memory. The figure matters to buyers trying to fit a model on a single card. The thread supplies no independent performance comparison with an A100.

- [**Alleged GeForce RTX 5090 with 96GB listed for sale in China**](https://www.reddit.com/r/LocalLLaMA/comments/1xucffq/alleged_geforce_rtx_5090_with_96gb_listed_for/) (Activity: 357): [**VideoCardz**](https://videocardz.com/newz/alleged-geforce-rtx-5090-with-96gb-listed-for-sale-in-china) **reports an unverified Chinese listing for an alleged GeForce RTX 5090 with** `96GB` **VRAM, which would imply a non-standard/heavily modified board rather than an official retail GeForce configuration. The capacity targets AI/workstation workloads and overlaps more with professional Blackwell-class cards, prompting skepticism around provenance, validation, and whether it is effectively a modded card or mislabeled pro/workstation SKU.** Commenters focused on pricing and value versus NVIDIA professional cards such as the RTX 6000-class products, with one noting they would consider traveling to test it if it were “half the price of a 6000.” Another questioned how it could be much cheaper if it uses the same amount of high-density VRAM, highlighting skepticism about the economics of a `96GB` configuration.

  - Commenters were skeptical that a `96GB` GeForce RTX 5090 would actually be cheaper than an RTX 6000-class card, since it would require the same high-capacity VRAM components and likely involves aftermarket “frankensteining” of existing RTX 5090 boards rather than a factory SKU. One technically focused concern was that extra capacity would not necessarily come with increased memory bandwidth, and buyers would face uncertain driver behavior, lack of robust validation, and no manufacturer warranty—though the card may still correctly report `96GB` of onboard VRAM.

## Less Technical AI Subreddit Recap

/r/Singularity, /r/Oobabooga, /r/MachineLearning, /r/OpenAI, /r/ClaudeAI, /r/StableDiffusion, /r/ChatGPT, /r/ChatGPTCoding, /r/aivideo

### Qwen 4 preview and UMAP 0.6

- [**Qwen 4 Max Preview has been evaluated by Stealthmodels**](https://www.reddit.com/r/Qwen_AI/comments/1xu4n0j/qwen_4_max_preview_has_been_evaluated_by/) (Activity: 189): **Stealthmodels published StealthMark results for Qwen 4 Max Preview, with a launch tracker at** [**stealthmodels.com/qwen-4**](https://stealthmodels.com/qwen-4) **and a head-to-head against Qwen 3.8 Max at** [**stealthmodels.com/compare/qwen3.8-max-0902-vs-qwen4-max-preview**](https://stealthmodels.com/compare/qwen3.8-max-0902-vs-qwen4-max-preview)**. The reported main technical delta is substantially improved token efficiency at highest reasoning settings, plus improved Visual Intelligence, while General Intelligence appears slightly regressed versus Qwen 3.8 Max on the hardest related benchmarks. The poster argues this could make Qwen 4 Max Preview more usable for long-running agentic tasks despite the preview status, and expects potentially larger relative gains in cheaper-to-train Flash and 27B variants.** Commenters were cautiously optimistic: one highlighted that a Qwen 4 Flash with similar reasoning-efficiency gains over 3.8 Flash would be valuable, while others emphasized that this is only a preview and broader testing is necessary before judging regressions.

  - Commenters focused on whether **Qwen 4 Max Preview / Qwen 4 Flash** represents a reasoning-efficiency jump over the prior **Qwen 3.8 Flash** line, with one user saying they would be satisfied if “Qwen 4 Flash is just 3.8 Flash Next” but with materially better reasoning efficiency.

  - There was skepticism about the **Stealthmodels benchmark** ranking, specifically the claim that a `27B` model scores “far above Flash in 3.8”; one commenter argued this result makes the benchmark unreliable or at least questionable without more validation.

  - A technically relevant improvement users highlighted was **vision capability**. One commenter said Qwen 3.8 felt weak on vision tasks, especially detecting frontend/UI mistakes, and hoped the reported vision gains in Qwen 4 Max would also transfer to smaller models like the `27B` variant.
- [**UMAP 0.6: new initialization, new optimizers and faster performance \[P\]**](https://www.reddit.com/r/MachineLearning/comments/1xudaah/umap_06_new_initialization_new_optimizers_and/) (Activity: 215): **UMAP 0.6 (**[**GitHub**](https://github.com/lmcinnes/umap)**,** [**pip install -U umap-learn**](https://umap-learn.readthedocs.io/)**) introduces opt-in layout changes via** `compatibility_layout=False`**: a recursive UMAP-based initialization, faster PyNNDescent nearest-neighbor search, and new parallel reproducible optimizers. Claimed gains include ~**`4x` **faster neighbor search, seeded multi-core runs versus UMAP 0.5’s seeded single-threaded behavior, and a** `3M` **arXiv-paper embedding completed in under** `5 min` **while 0.5 was still in NN search; one commenter reported a** `500%` **speedup on their project. Technically, the main quality improvements are attributed to recursive coarse-to-fine graph initialization and optimizer changes including hard-negative mining and better behavior at higher repulsion strengths, improving large-scale cluster detail and kNN recall; docs are available for** [**optimizers**](https://umap-learn.readthedocs.io/en/latest/optimizers.html) **and** [**large datasets**](https://umap-learn.readthedocs.io/en/latest/large_datasets.html)**.** Comments were mostly enthusiastic rather than technical, praising **Leland McInnes** and the continued ANN/UMAP performance improvements; the only concrete user report was a large observed speedup in a current project.

  - A user reported an empirical performance improvement after trying **UMAP 0.6** on an active project, claiming a roughly `500%` speedup. No dataset size, hardware, or configuration details were provided, so the result is notable but not benchmark-grade.

  - One technical question focused on whether the new performance improvements still apply when users provide their own **connectivity graph** and **initialization**. This matters because UMAP runtime can be dominated by different phases—ANN graph construction, initialization, or embedding optimization—depending on what inputs are precomputed.

  - Another commenter specifically called out continued improvements in **approximate nearest neighbor** performance, asking how the project keeps making ANN faster. This reflects interest in whether UMAP 0.6’s gains come from faster neighbor search, improved graph construction, or downstream optimizer changes.

### AI deployment and safety

- [**AI-assisted air traffic planning**](https://www.reddit.com/r/singularity/comments/1xvl00y/the_faa_rolled_out_its_875_million_ai_air_traffic/) (Activity: 1899). The FAA describes SMART as a planning tool that brings weather, traffic, staffing and flight paths into one view so operators can anticipate congestion. The FAA introduced it in September. [FAA announcement](https://www.faa.gov/newsroom/trumps-transportation-secretary-sean-p-duffy-delivers-state-art-air-traffic-control)

- **DeepSeek safety test.** A Reddit user reported that DeepSeek generated code during a safety test. Commenters questioned the claim that the demo showed a working backdoor. The thread supplied no independent evidence that anyone gained access to another system.

- [**ChatGPT randomly sent me an unsolicited follow-up message**](https://www.reddit.com/r/ChatGPT/comments/1xv32sv/chatgpt_randomly_sent_me_an_unsolicited_followup/) (Activity: 3823): **The image (**[**screenshot**](https://i.redd.it/9tjc7wfvhc1i1.jpeg)**) shows a ChatGPT thread where the assistant appears to initiate an unsolicited follow-up at** `4:15 AM`**, asking whether a prior PC-build decision involving a Ryzen 5 9600X and RTX 5060 Ti 16GB worked out. The post’s technical significance is that the user reports no explicit reminder, Task, or proactive-agent setup, raising the possibility of an undocumented proactive messaging experiment, latent automation trigger, or bug in ChatGPT’s conversation/task system.** Commenters mostly joked, but the main technical pushback was that *something* must have triggered the message—e.g., a scheduled task, prior instruction, or external automation—rather than the model independently deciding to check in.

  - A commenter argued that the unsolicited follow-up was unlikely to be spontaneous behavior from the model itself and was almost certainly caused by an external or product-level trigger: *“some automated task, some instruction in a chat previously, some external trigger had to trip to fire that process.”* They emphasized the distinction between LLM generation and orchestration around it, noting there is not a model “subroutine” independently deciding to re-engage a user.
