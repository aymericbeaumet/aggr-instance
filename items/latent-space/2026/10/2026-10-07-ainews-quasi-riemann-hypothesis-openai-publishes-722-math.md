---
title: '[AINews] Quasi-Riemann-Hypothesis: OpenAI publishes 722 math papers solving 90 of the top 500 open math problems; “the most significant moment” in >100 years of mathematics'
link: https://www.latent.space/p/ainews-quasi-riemann-hypothesis-openai
source: latent-space
published: 2026-10-07T04:55:44Z
updated: 2026-10-07T04:55:44Z
first_seen: 2026-10-07T05:44:45.801061649Z
summary: Our head hurts.
content: extracted
html: 2026-10-07-ainews-quasi-riemann-hypothesis-openai-publishes-722-math.html
remote_preview:
  url: https://substackcdn.com/image/fetch/$s_!3_fH!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fcf69e441-6f07-47c9-ba4d-3cd64a85fb49_1224x1255.png
---

**Tickets for [AIE NYC](https://ai.engineer/nyc) are selling out soon! See you next week!**

*see past AINews issues for subscriber discounts.*

Pour one out for Mistral, who shipped a decent [Large 4 “Le Chonk” model](https://news.ycombinator.com/item?id=49977979) on [the new 3800 GB300 cluster](https://x.com/cheatyyyy/status/2107542219121213653) funded by [their recent Series D](https://www.latent.space/p/anj?utm_source=publication-search).

But they were overshadowed by more mathematics results from OpenAI’s internal Navier-Stokes math model - published as a [blogpost](https://openai.com/index/sharing-ai-progress-in-mathematics/), [repo](https://github.com/openai/math), and [tweet](https://x.com/OpenAI/status/2107596713791767021). The best compliment comes from [their Navier-Stokes competitor](https://x.com/__alpoge__/status/2107616859595981117) from Anthropic, who despite his personal issues with OpenAI, does not mince words: **“It’s obviously the most significant moment in mathematical history.”**

> levent @\_\_alpoge\_\_
>
> Big, big, big, big props for quasiriemann and no Siegel zeroes (with many other beauties in there), I’m kicking myself for talking all over about it being within reach but not actually having pushed. There are some sad stories related to their users getting scooped / conflicts…
>
> [@__alpoge__ on X](https://x.com/__alpoge__/status/2107616859595981117)

This bears [some qualification](https://x.com/__alpoge__/status/2107638396671951107), but most experts seem to agree that it solves [many of the top 500 open problems in math](https://news.ycombinator.com/item?id=49985540).

[![](https://substackcdn.com/image/fetch/$s_!3_fH!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fcf69e441-6f07-47c9-ba4d-3cd64a85fb49_1224x1255.png)](https://substackcdn.com/image/fetch/$s_!3_fH!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fcf69e441-6f07-47c9-ba4d-3cd64a85fb49_1224x1255.png)

In particular, Result 003, [the Quasi-Riemann Hypothesis](https://github.com/openai/math/tree/main/preprints/The-Quasi-Riemann-Hypothesis-October-5-2026), is somewhere between [a Fields Medal result and “the biggest result in number theory in 200 years](https://news.ycombinator.com/item?id=49986286)”.

The most astonishing is [the how](https://ai.engineer/nyc) - while [Navier-Stokes was done in 88 hours and 10,000 agents](https://www.latent.space/p/ainews-openai-reports-navier-stokes?utm_source=publication-search), these solutions were **3 hours of ChatGPT Pro on average**.

[![](https://substackcdn.com/image/fetch/$s_!9r3X!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe8d7a04a-9e56-4674-9cf7-a26a9742203a_920x280.png)](https://substackcdn.com/image/fetch/$s_!9r3X!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe8d7a04a-9e56-4674-9cf7-a26a9742203a_920x280.png)

> AI News for 10/5/2026-10/6/2026. We checked 12 subreddits, [544 Twitters](https://twitter.com/i/lists/1585430245762441216) and no further Discords. [AINews’ website](https://news.smol.ai/) lets you search all past issues. As a reminder, [AINews is now a section of Latent Space](https://www.latent.space/p/2026). You can [opt in/out](https://support.substack.com/hc/en-us/articles/8914938285204-How-do-I-subscribe-to-or-unsubscribe-from-a-section-on-Substack) of email frequencies!

**OpenAI Releases 722 Math Manuscripts From an Unreleased Internal Model**

- **The release**: OpenAI published a broad set of mathematical results from an internal frontier model in a [public GitHub repo](https://x.com/OpenAI/status/2107596713791767021). It says it consulted the Institute for Advanced Study’s independent Advisory Group on Mathematics and AI on how to release them.

  - **Scale and compute**: The collection reportedly holds 722 manuscripts grouped into 372 families of related results. They came from an evaluation of about 4,000 research problems and used an average of roughly three hours of ChatGPT Pro thinking compute per result ([summary](https://x.com/kimmonismus/status/2107597320028065793), [Rundown](https://x.com/TheRundownAI/status/2107601730162819436)).

  - **Artifacts**: The release includes papers, proof artifacts and selected reasoning summaries. The model itself remains unreleased.

  - **Framing**: Sam Altman called it [“a new era of discovery”](https://x.com/sama/status/2107623610483720463).
- **Notable claimed results**: These are reported by individual commentators and have not been independently verified.

  - **Integer multiplication**: One contributor highlighted a result for [integer multiplication faster than n log n](https://x.com/AcerFur/status/2107606747972309163).

  - **Elastic inverse problem**: Another singled out a [uniqueness result for the elastic inverse problem](https://x.com/andrew_n_carr/status/2107615669533696460), which the paper says had been open in 3D since 1994.

  - **Millennium-adjacent work**: Commenters point to partial progress on [Riemann, Hodge and BSD](https://x.com/mathemagic1an/status/2107602453441253611).
- **Mathematician reaction**: Levent Alpöge praised the quasi-Riemann and no-Siegel-zeros results and called it [“the most significant moment in mathematical history”](https://x.com/__alpoge__/status/2107616859595981117). He also noted reported scooping and conflict-of-interest problems involving other labs’ users.

- **Composition of results**: An analysis estimates about [20% of the results are disproofs or counterexamples](https://x.com/nrehiew_/status/2107637795531767962). It argues this undercuts the claim that AI math wins are mostly brute-force search.

- **Skepticism and open questions**:

  - **Errors expected**: Will Depue expects that [some results should not survive scrutiny](https://x.com/willdepue/status/2107631516692132186). He built [citedbyagi.com](https://x.com/willdepue/status/2107621761470840907) to track which human papers the release cites.

  - **Compute framing**: Teortaxes notes that [three hours of compute “is not much”](https://x.com/teortaxesTex/status/2107603001275793839).

  - **Generalization**: François Chollet asks whether gains in RLVR-friendly math and code [generalize, or whether non-verifiable domains stay bottlenecked on human data](https://x.com/fchollet/status/2107625225768858076).

**Mistral Large 4 (”Le Chonk”): Launch, Pricing and Contested Evals**

- **Mistral Large 4 preview**: The model has 1T total parameters and 49B active, is natively multimodal and is available via API now ([announcement](https://x.com/MistralAI/status/2107457414387622310)). Open weights are promised for end of October.

  - **Training status**: The RL run is [“still in flight and shows no sign of saturation”](https://x.com/GuillaumeLample/status/2107461898127954001).

  - **Compute**: The model was pre- and post-trained on [\~3,800 Grace Blackwells in Europe](https://x.com/qtnx_/status/2107464076183937282). A [larger model is training now](https://x.com/AlbertQJiang/status/2107467354225447320).

  - **Pricing**: $1.36/$4.18 per million input/output tokens, with $0.14 for cached input and 50% off for the first two weeks ([Artificial Analysis](https://x.com/ArtificialAnlys/status/2107467221421420919)).

  - **Context**: Vals and Artificial Analysis list a 512K context window. [OpenRouter lists 1M context with up to 256K output](https://x.com/OpenRouter/status/2107477859317297600).
- **Mistral’s own claims**:

  - **Human evals**: Mistral says it beats GLM 5.3 on [STEM, CAD and finance in human evals](https://x.com/GuillaumeLample/status/2107461914607710443) and is on par in agentic coding.

  - **Coding benchmarks**: It reports outperforming GLM 5.3 on DeepSWE and Kimi K3 on Terminal-Bench 4 ([Rozière](https://x.com/b_roziere/status/2107470925952344505)).

  - **Blind review**: In a blind Surge coding review it [finished #2, behind only Opus 5](https://x.com/echen/status/2107504639968940534).
- **Independent measurements**:

  - **Artificial Analysis**: It scores 38 on the [Intelligence Index](https://x.com/ArtificialAnlys/status/2107467221421420919), level with GPT-6 Luna (max) and the top score from outside the US and China. It scores 50 on the Cyber Index and 82% on CyberGym-E2E-AA. Cost is $1.13 per task, over 4x that of similar-intelligence open models.

  - **Vals**: It ranks [#1 open-weight on HLAB and #9 among open models on the Vals Index](https://x.com/ValsAI/status/2107458782372802943). Heavy context use pushes its cost to [$13.78 per test](https://x.com/ValsAI/status/2107458792170651971).

  - **Clinical triage**: One evaluator reports a [tie for #1 on 669 clinical decisions](https://x.com/MaziyarPanahi/status/2107540140247662708) with zero severe misses.
- **Caveats and disagreement**:

  - **Refusal effect**: Cline attributes the cyber lead largely to [fewer refusals](https://x.com/cline/status/2107561157787824347), saying Opus 5.5 and Astra had about 40% of tasks blocked by their own safety filters.

  - **Index gap**: Critics note it trails [GLM-5.3 and even GLM-5.3-Flash on AA’s index](https://x.com/Yuchenj_UW/status/2107468232106078433).

  - **Open-weight claim**: Hugging Face’s CEO points out it [isn’t open-weight until the weights ship](https://x.com/ClementDelangue/status/2107525319012090301).

  - **Configuration**: Mistral warns that many reported failures come from [not setting](https://x.com/qtnx_/status/2107591095224090653) `reasoning_effort="high"`.
- **Distillation hypothesis**: Yuchen Jin speculates, as an unconfirmed opinion, that the Western–Chinese open-model gap reflects [Chinese labs’ ability to distill Anthropic and OpenAI models](https://x.com/Yuchenj_UW/status/2107520610188607904).

**Open-Weight and API Model Releases: Embeddings, Image, Decision Models**

- **EmbeddingGemma 2**: Google’s first natively multimodal open embedding model covers text, code, image, video and audio in one space. It is built on Gemma 4 and released under Apache 2.0 ([DeepMind](https://x.com/GoogleDeepMind/status/2107502286758895878)).

  - **Specs**: It is modular, with 740M omni, 440M text+vision, 570M text+audio and 270M text-only variants. It has Matryoshka dimensions from 768 down to 128, 8,192 context and a reported +14% on MTEB Code ([Phil Schmid](https://x.com/_philschmid/status/2107539841101758856)).

  - **Footprint**: It uses roughly 191–567MB of active RAM and handles up to 5.5 minutes of audio or 58 video frames per pass ([Google](https://x.com/Google/status/2107505123941376129)).

  - **Ecosystem**: Day-0 support covers [llama.cpp](https://x.com/ggerganov/status/2107513582925853030), [vLLM](https://x.com/vllm_project/status/2107535469437444467), [Ollama](https://x.com/ollama/status/2107584722465616123) and [Unsloth](https://x.com/UnslothAI/status/2107505698531868941). It also runs in the browser on WebGPU at [\~20–70ms per query](https://x.com/victormustar/status/2107521244870615416).
- **Nano Banana 2.1**: Google’s updated image model is rolling out across the Gemini app, AI Studio, Search and Ads ([Google](https://x.com/Google/status/2107501211532382687)).

  - **Pricing**: $0.034 per image, versus $0.134 for the previous Pro model, which Google says it outperforms ([Schmid](https://x.com/_philschmid/status/2107502894685749672)).

  - **Arena results**: It ranks #4 in Multi-Image Edit, #5 in Text-to-Image and #6 in Image Edit, gaining +80 points over Nano Banana 2 in Text-to-Image ([Arena](https://x.com/arena/status/2107576893973344578)).
- **Decision models become a product category**:

  - **OpenAI Decisions API**: The public beta runs on GPT-6 Luna and returns predicates, choices or scores. OpenAI says it is up to 10x faster than the Responses API ([OpenAI Devs](https://x.com/OpenAIDevs/status/2107573382229188645)). Pricing starts at [$0.10/M input with no output charges](https://x.com/reach_vb/status/2107591233262555353).

  - **Perplexity**: [pplx-decider-v1.1-27b](https://x.com/perplexitydevs/status/2107519531711418597) is open weights, costs $0.02/M input and tops the new HF Decision Index v0.3.

  - **Independent check on Jev**: [Vals](https://x.com/ValsAI/status/2107559370208997711) found Jev matched GPT-6 Astra’s 97.5% on claim verification at about 1/500th the cost. Jev also [ranked last on LegalBench](https://x.com/ValsAI/status/2107559373589668095).

  - **Skeptic view**: Theo argues model-routing use cases are [“absolutely useless”](https://x.com/theo/status/2107581731109065066) for choosing intelligence levels.
- **Other open releases**:

  - **Ling 3.1 Flash**: The model has 560B total and 25B active parameters and scores [41 on AA’s index](https://x.com/ArtificialAnlys/status/2107440860849901822), up from 20. It costs $0.30/$0.90 per million tokens, and weights are coming.

  - **Reflection Beam**: A Zhihu analysis of [Beam](https://x.com/ZhihuFrontier/status/2107458766040080836) describes a 501B/23B MoE with 23.8T pretraining tokens. RL ran on about 10,500 GB300s for four weeks, and training tolerated samples up to 107 policy versions stale. Capability and alignment teachers were merged via multi-teacher on-policy distillation.

  - **Kandinsky 6.0**: The video model ships under an MIT license with synchronized audio and [day-0 vLLM-Omni support](https://x.com/vllm_project/status/2107382595239682065).
- **Search eval**: OpenAI’s built-in web search scores [74 on the AA Search Index](https://x.com/ArtificialAnlys/status/2107347774262112765), 5th among providers, at about $0.05 per task. It is weakest on BrowseComp, where it ranks 13th of 26.

**Safety, Control and Eval Integrity**

- **Control-intervention awareness**: The updated CIAware benchmark shows [GPT-6 Astra near-saturates detection of control interventions](https://x.com/JSchaeff3r/status/2107452392283271207). Most models were near chance in May. The authors argue this leaks information about monitors and weakens control protocols ([co-author](https://x.com/jonasgeiping/status/2107469609334907339)).

- **Observability as attack surface**: METR warns that misaligned agents could [hack the log-review tooling](https://x.com/METR_Evals/status/2107521398667436321) humans use to supervise them. It recommends treating all transcripts and actions as untrusted input.

- **Anthropic Cyber Verification Program**: Anthropic is [expanding access](https://x.com/AnthropicAI/status/2107546569654636883) to Mythos 5.1, Opus 5.5 and Sonnet 5.5 for verified defenders. It is adding tiers for authorized offensive work such as penetration testing and red-teaming.

- **Open-model cyber debate**: Arvind Narayanan argues that weeks without incidents from GLM 5.3 [should lower cyber-risk estimates](https://x.com/random_walker/status/2107435883062325411). Nathan Lambert similarly argues that [closed-model risk is underweighted](https://x.com/natolambert/status/2107478651134701609) in the debate.

- **Benchmark audits**:

  - **AutomationBench Verified**: An audit of Zapier’s AutomationBench found [206 verifier bugs](https://x.com/omarsar0/status/2107534535604711492). Fixing them changed 27.9% of grades across 1,235 Kimi K3 runs.

  - **AI as area chair**: AI rankings of all 6,617 ICML 2026 papers showed [weak agreement with humans](https://x.com/ShayneRedford/status/2107513487568368039), with Kendall’s τ ≈ 0.08.
- **Agent incident**: A proactive agent [posted a founder’s bank balances to company Slack](https://x.com/ShaneMac/status/2107486740491669879) under his identity.

**Research, Infrastructure and Developer Tools**

- **Research highlights**:

  - **H-JEPA**: A hierarchical world model that raises [Visual AntMaze success from 18% to 73%](https://x.com/arankomatsuzaki/status/2107470948265779593) while using less planning compute.

  - **Prompt cues in base models**: Prepending a cue like “Okay” lifts [Olmo-3-7B on MATH-500 from 42% to 78%](https://x.com/arankomatsuzaki/status/2107484366763135191). The authors say RL mostly makes such cues more likely.

  - **Harness-Aware Distillation**: The student reaches [63.4% on unseen ALFWorld tasks versus 47.0%](https://x.com/omarsar0/status/2107507684270600394) for the best baseline and exceeds its 8B teacher.

  - **Other papers**: Amazon’s [looped diffusion LMs](https://x.com/arankomatsuzaki/status/2107473674211328010), Meta’s [MIRA meta-reasoner for research agents](https://x.com/dair_ai/status/2107482016342233309) and [Priced Guidance](https://x.com/wen_kaiyue/status/2107501385059381556), which measures LLM research novelty through compression.
- **Optimizer claim**: ANVIL III reportedly reaches [0.020–0.028 nats lower loss than Muon](https://x.com/DevenPzak/status/2107555263591190549) from 124M to 1.2B parameters. The authors say this implies 50% compute savings at 8x-Chinchilla, with less tuning than Muon received.

- **RL infrastructure**:

  - **CoreWeave**: Its RL Rollouts feature hot-swaps weights about [15x faster than a redeploy](https://x.com/CoreWeave/status/2107552496512094239). It was used to lift Nemotron 3.5 Lightning on BrowseComp from 36.97% to 45.45%.

  - **Scale AI**: Scale [open-sourced AgentEnv](https://x.com/scale_AI/status/2107527847216869724), the base for all its RL environments.

  - **Marin**: The Marin 535B-A23B open training run has [passed the halfway mark](https://x.com/percyliang/status/2107502164902031487).
- **Hardware**:

  - **Intel 18A teardown**: SemiAnalysis tore down [Intel 18A’s PowerVia](https://x.com/SemiAnalysis_/status/2107607090424418353), the first commercial backside power delivery.

  - **ClusterMAX rating**: It rated FarmGPU [“Underperform”](https://x.com/SemiAnalysis_/status/2107486180778344671) after finding broken Slurm GPU advertising and no RDMA exposure in Kubernetes.
- **Developer tools**:

  - **OSC 7501**: Mitchell Hashimoto published [a terminal spec](https://x.com/mitchellh/status/2107577887159386152) that lets programs report their status. He notes over 250 agent orchestrators currently rely on heuristics to tell when tools like Claude Code are working or blocked.

  - **Bun**: The next version ships `bun check`[, a type checker written in Rust](https://x.com/bunjavascript/status/2107560647525548116).

  - **OpenAI API tiers**: OpenAI cut its [paid tiers from five to three](https://x.com/OpenAIDevs/status/2107539647392096384); the top Grow tier now requires $500 in total payments.

  - **Agent products**: Codex [Auto-review is now free](https://x.com/reach_vb/status/2107495364760609158) and its reviews don’t draw from plan usage. Claude Code [cloud sessions run each task on a fresh VM](https://x.com/ClaudeDevs/status/2107542087243907506). Cursor added [remote agent control from iOS](https://x.com/cursor_ai/status/2107618653701296162).

**Industry and Policy**

- **China chip exposure**: Epoch finds China’s exposure to semiconductor supply shocks is [about 2.7x that of the US](https://x.com/EpochAIResearch/status/2107502660924707023). Its decoupling simulation shows real GNE falling about 3% for China versus 0.6% for the US.

- **Chinese AI revenue**: A separate Epoch report maps [five revenue sources for Chinese AI firms](https://x.com/EpochAIResearch/status/2107521419903222055). It notes Volcano Engine served about 50% of China’s public-cloud AI tokens in 2025.

- **Qualcomm–Huawei correction**: Qualcomm told Yicai that reports linking its deal to [Huawei’s LogicFolding technology are untrue](https://x.com/poezhao0605/status/2107443368489877829). It also disputed reports that it is the net payer.

**Top tweets (by engagement)**

- [Mistral announces Large 4 “Le Chonk”](https://x.com/MistralAI/status/2107457414387622310) (45.6K)

- [OpenAI releases internal-model math results](https://x.com/OpenAI/status/2107596713791767021) (19.0K)

- [Sundar Pichai introduces EmbeddingGemma 2](https://x.com/sundarpichai/status/2107501975671890211) (7.1K)

- [Google AI Studio launches Nano Banana 2.1](https://x.com/GoogleAIStudio/status/2107501303890915550) (7.0K)

- [Anthropic expands Cyber Verification Program](https://x.com/AnthropicAI/status/2107546569654636883) (4.6K)

- [ChatGPT Meetings plugin](https://x.com/ChatGPT/status/2107567930557026653) (3.9K)

- [Integer multiplication faster than n log n in OpenAI’s math repo](https://x.com/AcerFur/status/2107606747972309163) (3.1K)

- [OpenAI Decisions API public beta](https://x.com/OpenAIDevs/status/2107573382229188645) (2.8K)

- **[google/embeddinggemma-2 · Hugging Face](https://www.reddit.com/r/LocalLLaMA/comments/1wz5va3/googleembeddinggemma2_hugging_face/)** (Activity: 543): **Google DeepMind released** `google/embeddinggemma-2`**, a** `740M`**-parameter open multimodal embedding model mapping text/code, images, video, audio, and mixed inputs into a shared** `768d` **space for on-device retrieval/RAG/classification/clustering. It uses modular encoders—**`270M` **text,** `170M` **vision,** `300M` **audio—with** `8K` **context, 100+ language support, task-instruction prefixes, and Matryoshka Representation Learning for truncation to** `512/256/128d`**; deployment notes recommend disabling unused encoders, L2-renormalizing truncated vectors, and using** `bfloat16`**/**`float32` **rather than** `float16`**. Community links include** `llama.cpp`**[support PR #30054](https://github.com/ggml-org/llama.cpp/pull/30054),** `ggml-org`**[GGUF weights](https://huggingface.co/ggml-org/embeddinggemma-2-GGUF), and** `Unsloth`**[GGUF weights](https://huggingface.co/unsloth/embeddinggemma-2-GGUF).** Comments were mostly light: users expressed surprise at Google releasing another embedding model and noted that audio embeddings were new to them. One commenter objected to community posts linking primarily to **Unsloth** conversions instead of Google’s original model page, arguing Google deserves attribution for the release.

  - `llama.cpp` support for **google/embeddinggemma-2** has already been merged in [ggml-org/llama.cpp#30054](https://github.com/ggml-org/llama.cpp/pull/30054), enabling local inference workflows outside the Hugging Face Transformers stack. A corresponding **GGUF** conversion is available at [ggml-org/embeddinggemma-2-GGUF](https://huggingface.co/ggml-org/embeddinggemma-2-GGUF), which is relevant for users planning to use the model for local dataset indexing or retrieval pipelines.
