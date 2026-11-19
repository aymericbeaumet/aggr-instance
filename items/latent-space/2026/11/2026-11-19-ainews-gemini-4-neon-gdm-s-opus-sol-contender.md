---
title: '[AINews] Gemini 4 Neon: GDM''s Opus/Sol Contender'
link: https://latent.space/p/ainews-gemini-4-neon-gdm-s-opus-sol-contender/
source: latent-space
published: 2026-11-19T07:07:43Z
updated: 2026-11-19T07:07:43Z
first_seen: 2026-11-19T07:50:29.493483202Z
authors:
- swyx .ai
labels:
- 'ainews: weekday roundups'
summary: November 18, 2026 — Google’s middle tier scores 61 on the Intelligence Index. Plus, Anthropic prices its IPO at $83.
content: extracted
html: 2026-11-19-ainews-gemini-4-neon-gdm-s-opus-sol-contender.html
remote_preview:
  url: https://latent.space/og/card/post/ainews-gemini-4-neon-gdm-s-opus-sol-contender/0da9bd9da4547622b312ec1f44ab6ab42cc9e07bed6eb1d47b2fff64e4150bda.png
  alt: 'Gemini 4 Neon: GDM''s Opus/Sol Contender — Latent Space social card'
---

[← AI News](https://latent.space/news/) AI News

November 18, 2026 — Google’s middle tier scores 61 on the Intelligence Index. Plus, Anthropic prices its IPO at $83.

Google DeepMind shipped [Gemini 4 Neon](https://blog.google/innovation-and-ai/models-and-research/gemini-models/introducing-gemini-4-neon/), its fourth Gemini 4 model in about seven weeks. If you’re confused about the naming, you’re not alone; the [1.5 month old Argon](https://latent.space/p/ainews-gemini-4-argon-gdms-answer/) is now [retired](https://news.ycombinator.com/item?id=50555649), the old Pro and Flash from Gemini 1-3 are dead, and if you’re thinking the names are based on atomic weights, Neon (10), the midtier model is faster/cheaper than Carbon (6). This is a helpful guide to the current leading models and sizes:

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/6ba7e411-386b-45b6-b64e-592072c52abf/image.png?t=1795071522)

Because of how distillation, RL, and benchmarking works, Neon technically outperforms Carbon by some measures despite being a smaller model…

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/c8f029e3-5969-4262-a13c-47b74e946270/image.png?t=1795071964)

Also today, Anthropic priced its IPO at **$83 per share**, above its initial range, and Palo Alto Networks agreed to buy Applied Compute in a **$4.5B all-stock deal**. [Anthropic](https://www.anthropic.com/news/anthropic-prices-initial-public-offering) · [Palo Alto Networks](https://investors.paloaltonetworks.com/news-releases/news-release-details/palo-alto-networks-acquire-applied-compute-bringing-secure-open)

* * *

## AI Twitter Recap

**Gemini 4 Neon**

**Launch and availability**

- **Pricing and specs.** Neon costs $3 for input, $0.15 for cached input and $15 for output per million tokens. It has a 1M-token context window and four effort levels. Vals reports a maximum output limit of 1M tokens. [Google](https://blog.google/innovation-and-ai/models-and-research/gemini-models/introducing-gemini-4-neon/) · [Vals](https://x.com/ValsAI/status/2123168183746195620)

- **Rollout.** Neon is coming to the paid Gemini API in AI Studio and Android Studio, Gemini for business, and Antigravity for Pro and Ultra subscribers. In the Gemini app, Pro and Ultra users can select it as “Deeper analysis,” including for Tasks. [Google’s rollout](https://blog.google/innovation-and-ai/models-and-research/gemini-models/introducing-gemini-4-neon/) · [Gemini app](https://x.com/GeminiApp/status/2123169070988337603)

- **Day-one integrations.** [Cursor](https://x.com/cursor_ai/status/2123198346802450926), [GitHub Copilot](https://x.com/github/status/2123181574116233646) across its app, CLI and VS Code, and [LangChain Managed Deep Agents](https://x.com/caspar_br/status/2123192026745409844).

- **Release cadence.** Arena counted gaps of 28 days between Argon and Carbon, 15 between Carbon and Hydrogen, and six between Hydrogen and Neon. [Arena](https://x.com/arena/status/2123192239761596566)

**Independent results**

- **High versus xhigh.** At high effort, Neon matched Carbon’s 60 on the Intelligence Index for $3.74 per task. On GDP.pdf it scored 33.4% at high and 28.0% at xhigh. [Artificial Analysis](https://artificialanalysis.ai/articles/gemini-4-neon-intelligence-gain)

- **Agent Arena.** Neon debuted second with 1,336 points across about 4,200 sessions. It had a $2.82 median task cost and placed second on steerability. Carbon still leads the separate Text Arena. [Agent Arena](https://x.com/arena/status/2123169189397709174) · [Text Arena](https://x.com/arena/status/2123169199396946279)

- **Vals.** Neon placed second on the Vals Index at 70.9%, about 0.4 points behind Fable 5.2 at less than half the cost per task. On Code Migration it averaged 160 turns, fewer than Carbon’s 234 or Hydrogen’s 574, and scored below both. [Vals Index](https://x.com/ValsAI/status/2123168167879147542) · [Code Migration](https://x.com/ValsAI/status/2123168182051762433)

- **CursorBench 4.1.** Neon scored 63.7% at default effort, up from Hydrogen’s 57.3%. [Cursor](https://x.com/cursor_ai/status/2123198346802450926)

**Hands-on reports**

- **Steering and capacity.** In 13 early Antigravity sessions, Kimmonismus recorded 762 tool calls and a 93% input-cache hit rate. When told to avoid the terminal, Neon ran zero shell commands in those tests. The logs also recorded 57 capacity errors and four stalled runs. [Test logs](https://x.com/kimmonismus/status/2123168557228011759)

- **How teams use it.** Theo says he’s now using Neon for copy, Fable to implement changes, and Astra to playtest. Jeremy Howard praised Neon while saying he was reverting Solveit’s Astra and Sol options to their pre-6.2 versions. [Theo](https://x.com/theo/status/2123273253850628260) · [Jeremy Howard](https://x.com/jeremyphoward/status/2123280439314108762)

**New results for GPT-6.2**

- **Math and formal proof.** Astra and Sol took the top two spots on MathArena. Astra solved 42 of 53 ArXivLean problems. Vals separately reports what it believes is the first formalization in Lean of Smoktunowicz’s Gap Theorem, with the proof checked by an independent kernel replay. [Tim Gehr](https://x.com/TimGMath/status/2123067901280485621) · [Vals](https://x.com/ValsAI/status/2123129588066959380)

- **Agent Arena.** Astra debuted sixth. Sol placed eleventh at a $0.90 median task cost. On the separate Code Arena WebDev board, Astra placed third at a 42% lower median cost than its predecessor. [Agent Arena](https://x.com/arena/status/2123234410032214464) · [WebDev](https://x.com/arena/status/2123112692810764496)

- **Background discounts.** OpenAI opened a public beta for GPT-6.2 Responses API jobs that can wait. Two-hour and eight-hour targets for the first response come with 30% and 40% discounts per token, respectively. [OpenAI Developers](https://x.com/OpenAIDevs/status/2123188195202617408)

- **What changed after launch.** OpenAI’s Tibo Sottiaux says early reviewers used builds from before the final few days of changes. Artificial Analysis found Astra’s responses got shorter and it was less likely to admit uncertainty in its behavioral evaluation. [Sottiaux](https://x.com/thsottiaux/status/2122929634916679820) · [Artificial Analysis](https://x.com/ArtificialAnlys/status/2123236464003588136)

**Open weights and inference**

- **Correction to yesterday’s Pi adapter numbers.** [Yesterday’s issue](https://latent.space/p/ainews-gpt-6-2-astra-the-2-frontier-model-is-better-and-20-600-cheaper-but-has-mixed-reviews-6-2-sol/) cited 15 to 32 solves on the 51 hardest SWE-bench Pro tasks. Hugging Face reran affected tasks after finding a tunnel outage that hit nine base-model runs. The corrected result is **20 to 33 solves**, or 39.2% to 64.7%. It also released an OpenCode adapter that moved Qwen3.8-27B from 35.3% to 51.0% on the same subset with thinking off. [Corrected model card](https://huggingface.co/harness-hub/Qwen3.8-27B-pi-adapted) · [OpenCode release](https://huggingface.co/harness-hub/Qwen3.8-27B-opencode-adapted)

- **Tencent releases Hy4 Flash weights.** The 224B-parameter multimodal MoE activates 12B parameters per token, supports 1M context and includes a roughly 64B n-gram embedding table. Tencent released instruct, FP8 and base checkpoints. [Tencent repository](https://github.com/Tencent-Hunyuan/Hy4-flash) · [vLLM](https://x.com/vllm_project/status/2122917809948852559)

- **vLLM 0.34.0.** The extensible KV cache is on by default and Model Runner V2 is the sole GPU runner. The team reports up to 1.95× faster NVFP4 decoding on GB200. [vLLM](https://x.com/vllm_project/status/2123102778885276004)

- **Inference capacity grew faster than peak compute.** Epoch estimates that the capacity of chips sold for a fixed basket of agent workloads grew 4.6× from Q2 2025 to Q2 2026, while peak 8-bit compute grew 2.6×. [Epoch AI](https://epoch.ai/data-insights/agentic-inference-capacity-growth)

- **Runway cuts serving hardware in half.** It reports serving a 57B video model at 24 fps on four GPUs, down from eight. [Runway](https://x.com/runwayml/status/2123060689053036600)

- **Agent tooling.** Prime Intellect launched Model Factory for RL, supervised fine-tuning, serving and evals. Hugging Face released the message dataset from its 1,000-agent PyBot project. [Prime Intellect](https://x.com/PrimeIntellect/status/2123207653530972470) · [Hugging Face](https://x.com/lvwerra/status/2123089416352829725)

**Research and chip design**

- **OpenAI models help redesign AMD circuits.** OpenAI reports up to 14% improvement against optimized baselines on blocks of AMD’s flagship GPU design. The team used Synopsys tools to check that rewritten circuits preserved behavior and to measure power, performance and area. Some changes made it into a design sent for manufacturing. [OpenAI](https://openai.com/index/advancing-chip-design-with-our-models/)

- **Measuring research acceleration.** OpenAI published an update on internal agent use and research task completion, and proposed regular disclosure of indicators for AI research acceleration. [OpenAI](https://openai.com/index/research-acceleration-jagged-frontier/)

- **Smaller harness edits can save tokens.** In a 40-issue held-out test, a gated approach that edited one module at a time scored 40.0% on SWE-bench Lite at 46.7K tokens per task. A full rewrite scored 42.5% at 451.6K tokens. [DAIR research summary](https://x.com/omarsar0/status/2123077276350976396)

- **Agent teamwork.** MAC Bench finds most gains from eight agents came from exploring in parallel. Opus 5.5 was the sole tested model to show an added gain from coordination. [Bespoke Labs](https://x.com/bespokelabsai/status/2123099143996616741)

**Media and voice**

- **Genie 4 reaches Project Genie.** Google’s world model adds native audio and lets users change the world with text mid-session. Access is rolling out to eligible Google AI Ultra subscribers. [Google DeepMind](https://blog.google/innovation-and-ai/models-and-research/google-deepmind/genie-4-now-available-project-genie/)

- **Turn detection and transcription.** Inworld released weights for a voice-isolation model and its SAVA turn detector, which it says detects completed turns in a median 71 ms. AssemblyAI split its 3.7 release into models for voice agents, live transcription and recordings. [Inworld](https://x.com/inworld/status/2123120546908926300) · [AssemblyAI](https://x.com/AssemblyAI/status/2123080177508929849)

- **Kling Image 4.0.** It placed seventh in Artificial Analysis’s text-to-image rankings and sixth in editing. [Artificial Analysis](https://x.com/ArtificialAnlys/status/2123242530489966738)

**Industry**

- **Anthropic prices its IPO at $83.** The offering covers 980M shares, about $81.3B at the offer price. About $79.4B of that comes from shares sold by Anthropic before fees. Nasdaq trading under ANT is expected November 19. [Anthropic](https://www.anthropic.com/news/anthropic-prices-initial-public-offering)

- **Palo Alto Networks agrees to buy Applied Compute.** The $4.5B all-stock deal gives Palo Alto a platform for post-training and serving open-weight models. The companies expect to close in Palo Alto’s fiscal Q3 2027, subject to regulatory clearance and other conditions. [Palo Alto Networks](https://investors.paloaltonetworks.com/news-releases/news-release-details/palo-alto-networks-acquire-applied-compute-bringing-secure-open)

* * *

## AI Reddit Recap

**Local audio**

- **Blue TTS v3.** A developer says their MIT-licensed model generates audio at about 50× real time on CPU. The thread’s headline number is a throughput claim about that setup. [Code and demos](https://github.com/maxmelichov/BlueTTS) · [Discussion](https://www.reddit.com/r/LocalLLaMA/comments/1xytrz9/blue_tts_v3_opensource_tts_about_50x_realtime_on/)

- **Inworld voice isolation.** The release includes a 64M-parameter model for offline use and a 58M model for real-time use, both at 48 kHz. [Weights](https://huggingface.co/inworld/voice-isolator) · [Local tests and discussion](https://www.reddit.com/r/LocalLLaMA/comments/1xzqpzu/new_voice_isolation_model_from_inworld/)

- **AudioGhost.** A local interface for Meta’s SAM-Audio lets users describe a sound in text and separate it from the rest of a recording. [Project discussion](https://www.reddit.com/r/LocalLLaMA/comments/1xz6xl7/audioghost_v12_remove_any_sound_from_audio_or/)

**Local inference and coding**

- **Experts from an SSD.** TokenWing’s developer claims it can stream a majority of routed experts from SSD. A user reported 45–50 tok/s on a 120B MoE with 12 GB of VRAM and 32 GB of RAM. [LocalLLM thread](https://www.reddit.com/r/LocalLLM/comments/1xzmymn/ssd_moe_at_high_speed_the_wait_is_over/)

- **One job, two models.** A developer paired GPT-6.2 Astra for planning and review with local Qwen3.8-27B for implementation. The reported API bill fell from $7.40 to $1.62 on that run. [Workflow and discussion](https://www.reddit.com/r/Qwen_AI/comments/1xytns6/qwen_38_27b_did_the_heavy_lifting_for_gpt62_astra/)

- **Neon’s place in the line-up.** Readers discussed whether Neon becomes the everyday replacement for Gemini 3.1 Pro, now that Google has separate tiers for fast work and harder projects. [Launch discussion](https://www.reddit.com/r/singularity/comments/1xzrjzx/introducing_gemini_4_neon/) · [Tier discussion](https://www.reddit.com/r/GeminiAI/comments/1xza6pu/gemini_4_neon_coming_today_it_sits_right_between/)

Thanks for reading

## Stay a little ahead of the curve.

Fresh ideas, original reporting, and deep conversations in your inbox.

[Subscribe to Latent Space](https://latent.space/subscribe/)
