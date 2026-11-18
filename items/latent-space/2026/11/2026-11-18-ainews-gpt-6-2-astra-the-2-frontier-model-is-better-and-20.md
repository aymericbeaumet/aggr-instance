---
title: '[AINews] GPT-6.2 Astra, the #2 Frontier Model is better and 20%-600% cheaper, but has mixed reviews; 6.2 Sol likely main workhorse'
link: https://latent.space/p/ainews-gpt-6-2-astra-the-2-frontier-model-is-better-and-20-600-cheaper-but-has-mixed-reviews-6-2-sol/
source: latent-space
published: 2026-11-18T10:19:04Z
updated: 2026-11-18T10:19:04Z
first_seen: 2026-11-18T17:57:41.978411823Z
authors:
- swyx .ai
labels:
- 'ainews: weekday roundups'
summary: November 17, 2026 — GPT-6.2 Astra and Sol lower benchmark costs. Early testers disagree on what Astra is like to work with.
content: extracted
html: 2026-11-18-ainews-gpt-6-2-astra-the-2-frontier-model-is-better-and-20.html
remote_preview:
  url: https://latent.space/og/card/post/ainews-gpt-6-2-astra-the-2-frontier-model-is-better-and-20-600-cheaper-but-has-mixed-reviews-6-2-sol/d54e18a56681771d00f9af81d7e4a5b70fa4952861990442511933f8ff963105.png
  alt: 'GPT-6.2 Astra, the #2 Frontier Model is better and 20%-600% cheaper, but has mixed reviews; 6.2 Sol likely main workhorse — Latent Space social card'
---

[← AI News](https://latent.space/news/) AI News

November 17, 2026 — GPT-6.2 Astra and Sol lower benchmark costs. Early testers disagree on what Astra is like to work with.

[OpenAI shipped GPT-6.2 Astra and Sol](https://openai.com/index/introducing-gpt-6-2-astra-sol/) today, two days before [Anthropic’s expected first trading day](https://research.ftserussell.com/products/index-notices/home/getnotice/?id=2622497). In [Artificial Analysis](https://artificialanalysis.ai/es/articles/openai-has-the-2-model-in-the-intelligence-index-with-the-release-of-gpt-6-2-astra-and-6-2-sol)’ tests, **Sol ties September’s Astra at about 15% of the cost per task**. New Astra scores 62 on the Intelligence Index, just behind Fable 5.2 at 65. Net net, Anthropic still owns the world’s best model, but OpenAI owns the world’s most efficient models in terms of intelligence per $.

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/38ecd72a-69fc-402f-ab81-15ac4b07bf8c/image.png?t=1794996495)

What’s more unusual about this launch is the STRONGLY negative influencer feedback…

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/80c83708-0ed3-4691-a7a9-041cf2062106/image.png?t=1794996785)

We think that some of this is overreacting and normal influencer tendency to either have everything be 🤯🤯🤯 or the literal WORST.MODEL.EVER, BUT also that some of the criticisms are valid, and the RL artefacts are noticeable and [downright bizarre](https://x.com/emollick/status/2122783985134682207) (don’t even get us started about [the terrible launch video](https://x.com/swyx/status/2122862646265520150)). However, we still use it to automate our work and that’s all that matters in the end.

Great ship, but Anthropic’s IPO is unaffected and that will be probably the headline of the week/year/decade (until OpenAI IPO’s….)

**Today’s AI News is brought to you by SerpApi!**

[Retrieve public web pages by URL](https://serpapi.com/latentspace-ainews-extract-link) as LLM-ready Markdown or structured JSON. Map sites and run bounded crawls with one API. Pair with 100+ SerpApi search APIs for search-to-content workflows.

* * *

## AI Twitter Recap

**GPT-6.2 Astra and Sol**

- **Availability.** Today’s models are rolling out to ChatGPT Work and Codex for Plus, Pro, Enterprise and Edu, and to developers through the API as `gpt-6.2-astra` and `gpt-6.2-sol`. [OpenAI](https://openai.com/index/introducing-gpt-6-2-astra-sol/)

- **API prices per million tokens.** Astra is $8 input, $0.40 cached input and $40 output. Sol keeps its predecessor’s pricing at $2, $0.10 and $10. Both have 1M context and a 128K output limit. [Vals](https://x.com/ValsAI/status/2122750584084426852) · [Ultrafast pricing and rollout](https://x.com/OpenAIDevs/status/2122752500877144461)

- **Astra for Law.** The Enterprise configuration adds a legal search index. Artificial Analysis measured 20.8% on Harvey LAB-AA, versus 15.0% for base Astra. [Artificial Analysis](https://x.com/ArtificialAnlys/status/2122752733497463210)

- **Preparedness.** OpenAI rated Astra High for AI Self-Improvement, a first for one of its externally released models. On a separate measure, Vals’ RSI-Index rose from 27.07% to 29.01%, with 50% denoting its human reference. [System card](https://deploymentsafety.openai.com/gpt-6-2-astra-sol/gpt-6-2-astra-sol.pdf) · [Vals](https://x.com/ValsAI/status/2122751897434309101)

- **FrontierMath Erdős.** Epoch verified seven solutions in Lean out of 68 problems. The previous best was three. [Epoch](https://x.com/EpochAIResearch/status/2122762423501467663)

**What the benchmarks measure**

- **Agents’ Last Exam 1.1.** OpenAI reports Astra at xhigh gained 5.2 points over GPT-6 Astra at max, using less than a third of the cost per task. It reports a 5.5-point lead over Fable 5.2 at about 5% of the cost on that test. On GDPval v2, it says Astra used 22% fewer output tokens than its predecessor. [OpenAI’s results](https://openai.com/index/introducing-gpt-6-2-astra-sol/)

- **Long engineering runs.** Mercor’s OpenWorldEngineer puts Astra at 67.2%, Fable 5.2 at 54.6%, and GPT-6 Astra at 44.2%. On Vals’ multi-turn Vibe Code Bench, Astra was first and Sol was third. [Mercor](https://x.com/mercor/status/2122754439094354101) · [Vals](https://x.com/ValsAI/status/2122859493495320692)

- **3D and robotics.** Astra led AA-3D with 1,318, ahead of Fable at 1,271 and Sol at 1,259. On Artificial Analysis’s robotics preview, Astra scored 79 and Sol 78, ahead of Fable at 75. [Artificial Analysis](https://artificialanalysis.ai/es/articles/openai-has-the-2-model-in-the-intelligence-index-with-the-release-of-gpt-6-2-astra-and-6-2-sol)

- **Research agents.** On Perplexity’s WANDR, Astra scored 0.783 at about $12.54 per task. Sol reached 0.746 at about $2.65. [Perplexity](https://x.com/perplexity_ai/status/2122814545639223348)

**Early testers disagreed**

- **Matt Shumer** called the models “essentially unusable” after weeks of testing and said his Opus and Fable agents rejected much of Astra’s subagent work. [Review](https://x.com/mattshumer_/status/2122752192226406421) · [Workflow details](https://x.com/mattshumer_/status/2122826912406126786)

- **Pietro Schirano** attributed many of his early issues to the harness. Arena’s Peter Gostev tested Astra against Fable on visual coding and singled out Astra’s SVG peacock. [Schirano](https://x.com/skirano/status/2122770640604111133) · [Gostev](https://x.com/petergostev/status/2122816629402910859)

- **The demos.** Gabriel Chua built a walkable Singapore using more than 100 Codex threads. Simon Willison ran the same three.js mech prompt through several models, including Astra, Sol, Carbon and Fable. [Singapore build](https://x.com/gabrielchua/status/2122762679760883967) · [Same-prompt comparison](https://x.com/simonw/status/2122822310428279136)

**Microsoft Ignite and Gemini**

- **Microsoft ships its own coding models.** MAI Code 2 is for multi-step coding, and Code 2 Flash is for quicker edits. Both are available in GitHub Copilot’s model picker, Copilot Fusion and Foundry. Microsoft also announced a multimodal upgrade to its Decision model. [Microsoft](https://blogs.microsoft.com/blog/2026/11/17/ignite-2026-building-the-frontier-ecosystem/)

- **Copilot Fusion.** Microsoft reports 71.2% on Terminal-Bench 4.1 with its multi-model harness, at 88% lower cost than Claude Fable 5.2 in that comparison. [Jay Parikh](https://x.com/jayparikh/status/2122782972994838781)

- **Image editing.** Artificial Analysis ranks MAI Image 2.7 first on its editing benchmark, at $34.82 per 1,000 images. [Artificial Analysis](https://x.com/ArtificialAnlys/status/2122754535206846610)

- **Gemini Tasks.** Hydrogen now powers free Tasks in six countries, including tasks that need a phone call or a text. Google also showed a new Antigravity robotics plugin with MuJoCo simulation. [Sundar Pichai](https://x.com/sundarpichai/status/2122741728247738688) · [Google DeepMind](https://x.com/GoogleDeepMind/status/2122767525419364630)

**Agent harnesses and inference**

**Hugging Face’s Pi adapter.** A small adapter trained inside **pi** lifted Qwen3.8-27B from 15 to 32 solves on the 51 hardest SWE-bench Pro tasks. Across that subset it made 55% fewer tool calls per task. [Model card and paired results](https://huggingface.co/harness-hub/Qwen3.8-27B-pi-adapted)

- **Harness costs vary sharply.** Baseten measured up to a 4.4× spread in cost per passing task across its benchmark of 22 harnesses and five open models. [Baseten](https://x.com/baseten/status/2122768212177592359)

- **The harness may specialize to its model.** Google’s SIM keeps the rationale and outcome of prior harness edits. A harness it evolved on Opus scored 68.41 on Gemini, versus 73.89 for the starting harness. [Research summary](https://x.com/omarsar0/status/2122701047118635485)

- **More multi-agent products.** Cognition launched persistent Devin Bots. Perplexity reported that its mixed-model teams beat four instances of the lead model on Terminal-Bench and WANDR. Replit reported diminishing returns from 16 to 32 agents on NumPy tasks. [Cognition](https://x.com/cognition/status/2122737511684587551) · [Perplexity](https://x.com/perplexity_ai/status/2122728654400815244) · [Replit](https://x.com/pirroh/status/2122810974579425680)

- **Coordination dominated a 1,000-agent run.** Hugging Face says only 40–60 agents were calling the model at any moment. They read roughly 130 tokens for every token they wrote, with merges and tests contributing to the bottleneck. [Thomas Wolf](https://x.com/Thom_Wolf/status/2122673947409858730)

- **A small fraction of cache misses can dominate the bill.** SemiAnalysis reports that 1.8% of uncached requests accounted for roughly 15% of spend in its sample. [SemiAnalysis](https://x.com/SemiAnalysis_/status/2122853999506260021)

- **Marin and K2 Horizon opened up their training runs.** Marin released 57 intermediate Mochi checkpoints from its 535B-A23B run. The K2 report compares four model sizes trained on the same 22T tokens and describes a decoding adapter that the team says reaches 2.2× speedup. [Marin](https://x.com/dlwh/status/2122799840057004296) · [K2 Horizon](https://x.com/IFM_AI/status/2122721434262786095)

- **Zyphra’s long-context attention.** The team reports 9–60× speedups at 10M tokens after converting several models to its subquadratic attention, with Qwen losing under two points on RULER at 1M. [Zyphra](https://x.com/ZyphraAI/status/2122775085308354754)

- **Speculative decoding.** vLLM reports up to 30% higher acceptance length from a sliding-window draft for Kimi K3.1 Pro. NVIDIA also has a 4B draft for Qwen3.8-27B. [vLLM](https://x.com/vllm_project/status/2122736816809992294) · [Draft-model research](https://x.com/HuggingPapers/status/2122737583361057024)

**Industry and policy**

- **NVIDIA’s first earnings after the CEO transition.** Under interim CEO Colette Kress, NVIDIA posted $115.8B in quarterly revenue, up 103% year over year, and said Vera Rubin NVL72 had shipped. The board said it did not intend to reinstate Jensen Huang. [NVIDIA](https://nvidianews.nvidia.com/news/nvidia-announces-financial-results-for-third-quarter-fiscal-2027) · [AP](https://apnews.com/article/3026bcbe04ab57b844884382e583422a)

- **Isomorphic Labs raises $5B.** Greenoaks and Sequoia co-led the Series C. The company says it will scale its drug-design platform and develop its therapeutic pipeline. [Isomorphic Labs](https://www.isomorphiclabs.com/articles/isomorphic-labs-announces-series-c-investment-round)

- **Genesis AI raises $500M.** The Series A values the robotics company at $3.5B post-money. It plans to use the funding to deploy its Eno robot and expand training and data collection. [Zhou Xian](https://x.com/zhou_xian_/status/2122752003197833357)

* * *

## AI Reddit Recap

**Local models and hardware**

- **A 9B model with layers that run twice.** SenseNova-Loop-9B runs 24 of its 32 layers a second time. The model’s authors report GAIA rising from 37.86 in the preview to 69.90 in the release. Local users asked how much memory remains for the KV cache at long context. [Weights](https://huggingface.co/sensenova/SenseNova-Loop-9B) · [Discussion](https://www.reddit.com/r/LocalLLaMA/comments/1xymz9z/sensenovaloop9b_vs_its_own_preview/)

- **Maxwell V1 Flash.** The 35B/3B-active MoE claims a 6.7× attention speedup over dense GQA at 256K. Its sparse attention still retains the full-history KV cache, which is what local users want measured before they commit hardware. [Model card](https://huggingface.co/maxwell-model/Maxwell-V1-Flash) · [Discussion](https://www.reddit.com/r/LocalLLaMA/comments/1xyamk6/maxwellmodelmaxwellv1flash_hugging_face/)

- **Olala mini 7A1B.** OVHcloud previewed a small MoE with 1B active parameters and a 32K context window. The thread is waiting for weights and reproducible benchmarks. [OVHcloud](https://corporate.ovhcloud.com/en/newsroom/news/ovh-olala-mini/) · [Discussion](https://www.reddit.com/r/LocalLLaMA/comments/1xygtj4/ovh_announces_olala_mini_7a1b/)

- **Intel’s Crescent Island numbers.** The early brief’s eye-catching throughput comparison uses FP64-heavy workloads. Intel’s separate LLM comparison claims 1.12× throughput and 29% lower latency versus RTX PRO 6000 Blackwell Server Edition. [Hardware discussion](https://www.reddit.com/r/LocalLLaMA/comments/1xyioi8/intel_shares_first_benchmarks_of_its_nextgen/)

- **Gemma on a printer.** A builder reports running Gemma 3 270M via a PostScript print job. [Demo](https://www.reddit.com/r/LocalLLaMA/comments/1xyjkts/gemma_on_a_printer/)

**Agents in use**

- **A video-game agent built for real-time input.** miHoYo’s Gamer samples the screen at 5 Hz and emits keyboard and mouse activity at 30 Hz. Its RL version solved about half of the held-out Honkai puzzle tasks in the authors’ evaluation. [Project](https://mihoyo-gamer.github.io/) · [Discussion](https://www.reddit.com/r/LocalLLaMA/comments/1xxy7eb/mihoyo_gamer_a_generalist_game_agent/)

- **A fictional league runs a nightly broadcast.** The builder says Fock Ball simulates the games in a physics engine and uses Claude for surrounding work such as commentary and the newsroom. [Builder’s post](https://www.reddit.com/r/ClaudeAI/comments/1xyklpy/i_invented_a_sports_league_that_runs_itself_fock/)

- **ICLR reviews and AI detectors.** Reviewers report that the program chairs flagged likely AI-generated reviews. The thread asks how a detector can distinguish editing help from whole-review generation. [Peer review discussion](https://www.reddit.com/r/MachineLearning/comments/1xyf1tq/iclr_2027_pcs_ran_an_ai_detector_on_reviews_peer/)

Thanks for reading

## Stay a little ahead of the curve.

Fresh ideas, original reporting, and deep conversations in your inbox.

[Subscribe to Latent Space](https://latent.space/subscribe/)
