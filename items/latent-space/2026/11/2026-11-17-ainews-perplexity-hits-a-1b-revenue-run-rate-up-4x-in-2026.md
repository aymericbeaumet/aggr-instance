---
title: '[AINews] Perplexity hits a $1B revenue run rate, up 4x in 2026'
link: https://latent.space/p/ainews-perplexity-hits-a-1b-revenue-run-rate-up-4x-in-2026/
source: latent-space
published: 2026-11-17T09:24:56Z
updated: 2026-11-17T09:24:56Z
first_seen: 2026-11-17T18:26:16.162530996Z
authors:
- swyx .ai
labels:
- 'ainews: weekday roundups'
summary: November 16, 2026 — Computer gets Teammates, Perplexity ships Monitor API, and a new research lab launches.
content: extracted
html: 2026-11-17-ainews-perplexity-hits-a-1b-revenue-run-rate-up-4x-in-2026.html
remote_preview:
  url: https://latent.space/og/card/post/ainews-perplexity-hits-a-1b-revenue-run-rate-up-4x-in-2026/2627049380666217b18fbe35046e68da8e58cd8f8b30f47fa7c60b73ece9e2fd.png
  alt: Perplexity hits a $1B revenue run rate, up 4x in 2026 — Latent Space social card
---

[← AI News](https://latent.space/news/) AI News

November 16, 2026 — Computer gets Teammates, Perplexity ships Monitor API, and a new research lab launches.

**Perplexity crossed a $1B annual revenue run rate today**, CEO Aravind Srinivas [told Semafor](https://x.com/semafor/status/2122456557602459903) at a live event. [The Information](https://www.theinformation.com/articles/nvidia-discusses-perplexity-investment-30-billion-plus-valuation-considered-tech-licensing-deal) reported previously under $250M at the start of the year and over $750M by August, with Computer driving some of the growth. At usual [agent lab](https://latent.space/p/agent-labs) multiples (eg [Cognition also crossed $1B](https://cognition.com/blog/1b-run-rate), with [Cursor crossing $4.6B](https://x.com/swyx/status/2119429305512165838)), this would roughly mean that Perplexity is now worth $30-50B — if the unit economics hold up. This is a nice bump on the [$20B round in September 2025](https://www.theinformation.com/briefings/perplexity-finalizes-20-billion-valuation-round), and qualifies for our “>$10B fundraise” rule for title stories.

Gathering up some recent Perplexity launches for those catching up:

- **Computer.** September’s [Automations](https://www.perplexity.ai/en-GB/hub/blog/computer-adds-automations-for-ongoing-work) let it pick up recurring tasks where it left off. November’s [Teammates](https://www.perplexity.ai/changelog/teammates-fully-local-computer-on-mac-and-voice-mode) take on ongoing jobs. Computer also runs fully locally on supported Macs.

- **Monitor API.** [Watch a topic or page](https://www.perplexity.ai/en-GB/hub/blog/introducing-perplexitys-monitor-api) and get a webhook when something relevant changes.

- **Frontier Research.** The new [research lab](https://www.perplexity.ai/sv/hub/blog/introducing-perplexity-frontier-research) has access to 10,000+ GPUs and plans to publish papers, data, models, and code.

**Sponsored by Deepgram**\
Deepgram is the voice AI infrastructure behind natural conversation—bringing speech-to-text, text-to-speech, voice-focused LLMs, and agents to teams building with voice AI. For any stack and any tools you already love building with, Deepgram slots in as your foundational voice AI technology. So you have control. And we have the context to listen, understand, and speak like a human.

[**Learn more about the Real-time Context Standard**](https://deepgram.com/?utm_source=latent-space&utm_medium=sponsored-newsletter&utm_campaign=fy26_q4_brand_campaign&utm_term=latent-space&utm_content=main-ad)**.**

* * *

## AI Twitter Recap

**Qwen4-Max in the wild**

Friday’s [Qwen4-Max preview](https://latent.space/p/ainews-the-no-ai-slop-manifesto/) is now a shipping model. At the highest reasoning setting, Qwen4-Max scored 43 on the Intelligence Index, up from 34. Average cost fell from $10.02 to $3.27 per task. Alibaba cut output pricing from $6 to $4.50 per million tokens, and the model generated about 39% fewer of them. Kimi K3.1 Pro cost $3.68 per index task and scored 67% on Terminal-Bench 4.0 to Qwen’s 48%. [Artificial Analysis](https://x.com/ArtificialAnlys/status/2122161629592953032)

- **Inside the model.** Alibaba kept the total parameter count at 2.4T and reduced the number active per token by 25% to 71B. The model takes images and text, with a 1M-token context window. Smaller open-weight Qwen4 models are still to come. [Alibaba](https://www.alibabacloud.com/en/press-room/alibaba-unveils-qwen4-max)

- **Arena and Vals**: Arena ranks it #14 overall on Agent Arena, the top Chinese model, with +7.5% net over 14.7k sessions ([Arena](https://x.com/arena/status/2122153116959482051)).

  - **WebDev**: It placed #6 in Code Arena WebDev at 1750, +74 over its predecessor ([WebDev](https://x.com/arena/status/2122153582384583156)). It also placed #6 in Image-to-WebDev, just above Claude Opus 5 and GPT-6 Sol ([I2WebDev](https://x.com/arena/status/2122153586704687570)).

  - **Vals Index**: It is #11 overall, with large gains in coding and finance ([Vals](https://x.com/ValsAI/status/2122152965293457822)).

  - **Legal-benchmark caveat**: On HLAB it gets 95.7% of checklist items right but passes only 21.3% of tasks, and its output tokens rise 3.6x ([HLAB](https://x.com/ValsAI/status/2122152974843822150)).
- **Vision results (independent)**: Roboflow calls it underwhelming on its VLM benchmark, with Google, OpenAI and Anthropic now far ahead. At high reasoning effort, however, it is ~4.5x faster and ~68% cheaper per image than Qwen3.8-Max ([Roboflow](https://x.com/skalskip92/status/2122387961081729246)).

**Safety, alignment and eval integrity**

- **OpenAI embedded assessments**: OpenAI is giving Apollo, METR, Transluce and Goodfire internal access to its models and systems ([OpenAI](https://x.com/OpenAI/status/2122359930628063248)).

  - **Scope by group**: Apollo places six evaluators onsite for four months ([Rundown](https://x.com/TheRundownAI/status/2122379979694707134)). METR reviews OpenAI's largest RL run, with access to transcripts, monitor records and checkpoint snapshots ([METR](https://x.com/METR_Evals/status/2122361049114657203)). Transluce runs a six-week audit of post-training alignment evals, with a fuller follow-up in early 2027 ([Transluce](https://x.com/TransluceAI/status/2122367515045339588)). Goodfire embeds interpretability researchers to monitor training runs ([Goodfire](https://x.com/GoodfireAI/status/2122362693730574665)).

  - **Limits**: METR says its scope excludes the misalignment incidents OpenAI has disclosed and is narrower than METR proposed ([scope](https://x.com/METR_Evals/status/2122361050456801698)).
- **Criticism of the access terms**: Critics say the deals fall short of Sam Altman's promise of "employee-like access."

  - **Reported gaps**: Assessors get no Slack access, OpenAI selects which staff they interview, and they cannot access weights or run training-based elicitation ([Neel Nanda](https://x.com/NeelNanda5/status/2122394159197950006), [Alex Mallen](https://x.com/alextmallen/status/2122410729978626291)).

  - **Former-employee account**: Mikita Balesni says assessors cannot quote employees, even anonymously, and some may be barred from publishing on internal-only models ([balesni](https://x.com/balesni/status/2122479636668101087)).
- **Goodfire acquisition talks**: The Information reports OpenAI explored buying the interpretability startup for more than $5B. Goodfire reportedly wanted to stay independent so it could sell to multiple labs ([report](https://x.com/steph_palazzolo/status/2122331129265135929)).

- **Other integrity signals**:

  - **Benchmark contamination**: Claude appears to have memorized details of the LinuxArena control eval ([Karvonen](https://x.com/adam_karvonen/status/2122492345656734020)).

  - **Incident taxonomy**: OpenAI published a v0.1 framework for classifying AI control incidents ([framework](https://x.com/dkaushik96/status/2122409812591813071)).

  - **Disclosure policy**: Anthropic is cutting its coordinated disclosure window from 90 to 30 days for high- and critical-severity vulnerabilities ([policy](https://x.com/WeldPond/status/2122352314056855751)).

**Inference systems and agent infrastructure**

- **Together moves on from hand-tuned megakernels.** Its first design made it into production for one customer. Agents now search for smaller groups of GPU operations to fuse. Together reports 28% higher throughput on Llama 3.2 1B on B200. [Engineering writeup](https://www.together.ai/blog/road-to-together-compiler)

- **DeepSeek V4.1 Flash on Vera Rubin**: vLLM with Dynamo on SemiAnalysis's AgentX benchmark shows 63.4x per-chip throughput over GB300 NVL72 at 400 tok/s/user, and 93.3x at 600 ([vLLM](https://x.com/vllm_project/status/2122374424704024701)).

  - **Batch-1 latency**: SGLang-adjacent work reaches ~2,500 tok/s at batch size 1 on TP4, 1.7x its GB300 result ([saltyph](https://x.com/saltyph/status/2122476616593047645)). Baseten reports a similar 1.7x over its 4xB300 numbers ([Baseten](https://x.com/baseten/status/2122481834147877291)).

  - **Agent-driven optimization**: An agent shared Engram tables across HBM via NVLink symmetric memory to recover prefix-cache capacity ([Liu](https://x.com/liu_juntian/status/2122388250061103582)). Another team says agents found optimizations that made its B300 step time 10% faster ([Halex](https://x.com/Halex623/status/2122391758193807439)).
- **Agentic serving research**: MIT's Evolving Infra replaces fixed prefill/decode disaggregation with online adaptation. Against NVIDIA Dynamo on up to 256 GB300s, it reports up to 4.1x lower latency and 14x lower P95 TTFT ([Evolving Infra](https://x.com/Mlsys_HaoKang/status/2122374238023925845)).

  - **Pipeline-parallel prefill**: Inferact finds PP2 on MiniMax-M3.1-Flash sustains 23% more requests per GPU than DEP2 at a 2s TTFT target ([Inferact](https://x.com/inferact/status/2122479013897896315)).

  - **Prefix-cache pitfalls**: Ollama found that a single system-prompt timestamp or a change in reasoning effort can wipe the cache ([Ollama](https://x.com/ollama/status/2122531964154712541)).
- **Sandboxes and training**:

  - **External sandboxes**: Simon Willison describes running agents on a Mac while executing their code in WebAssembly, QEMU or Apple Container sandboxes. The pattern includes allow-list proxies and credential replacement ([post](https://x.com/simonw/status/2122500307557265651)).

  - **MLPerf**: In MLPerf Training v6.1, a GB300 NVL72 trained GPT-OSS-20B in 19.7 minutes, 2.86x faster than H100 ([Lambda](https://x.com/LambdaAPI/status/2122419500339576870)).
- **One endpoint for open models.** Hugging Face Inference Providers now runs on OpenRouter using the same endpoint and token. [Hugging Face](https://x.com/huggingface/status/2122393775314264519)

**Research, benchmarks and evals**

- **PyBot**: A swarm of 1,000 DeepSeek-V4.1-Flash agents spent a week writing a rule-based chatbot with no neural network ([lewtun](https://x.com/_lewtun/status/2122333622946373889)).

  - **Scale**: The agents exchanged ~175k messages and merged ~8k PRs.

  - **Result**: The bot is 630k lines of Python with zero dependencies and is reported on par with SmolLM-360M on everyday questions.
- **Self-improvement and training**:

  - **AutoRL**: Agents supervising a live RL run lifted Qwen3 on AIME 2024 from 46.67% to 66.67% ([omarsar0](https://x.com/omarsar0/status/2122429507756069008)).

  - **Benchmark-gaming caveat**: A roundup warns that many self-improving agent gains come from learning the benchmark rather than the task ([TuringPost](https://x.com/TheTuringPost/status/2122333725786620408)).

  - **Weight-space programming**: Opus 5.5 hand-wrote transformer weights that solved algorithmic tasks with a median of 166 parameters ([dair\_ai](https://x.com/dair_ai/status/2122370369097277634)).

  - **Data curation**: DatologyAI published the tech report behind its claimed 6x pretraining compute multiplier ([Datology](https://x.com/datologyai/status/2122366590201307615)).

  - **Distillation reproduction**: A small-scale replication confirms perturbed-input distillation helps when the student trains from scratch, but finds it hurts when distilling into a pretrained student ([Maini](https://x.com/pratyushmaini/status/2122496448319357245)).
- **New benchmarks**:

  - **GradeBench**: The best frontier model agrees with instructors only 57% of the time when grading handwritten physics answers ([GradeBench](https://x.com/yoonseok_yang/status/2122375477537198582)).

  - **Time to First Audio**: AA's new TTS latency benchmark finds a median of 376 ms for streaming models versus 2.0 s for non-streaming ones ([AA](https://x.com/ArtificialAnlys/status/2122374288653373870)).

  - **SpeedRunBench**: Inkling-2-Small cleared SuperTux 1-1 in 1,988 frames, ahead of Opus 5 and GPT-5.6 Sol ([SpeedRunBench](https://x.com/VarunGangal/status/2122176290757476441)).

**Science models and developer tools**

- **Gemini 4 Carbon**: DeepMind says Carbon powered AlphaProof Code agents that verified Signal's cryptographic code. It also spotted wet-lab errors and found 40x more related protein sequences ([details](https://x.com/GoogleDeepMind/status/2122366994708124098)).

  - **Science access**: Google expanded free Carbon access for 10,000 scientists through Antigravity ([program](https://x.com/GoogleDeepMind/status/2122366997501780327)).

  - **Context window**: Antigravity doubled the context window for Argon, Carbon and Hydrogen, cutting compaction 3.5x per user turn ([Mohan](https://x.com/_mohansolo/status/2122119826575646918)).

  - **User reports (anecdotal)**: One researcher says Carbon solved a problem that waves of Astra and Fable attempts had not ([anecdote](https://x.com/scottnarmstrong/status/2122089351081275657)).
- **OpenAI for science and health**: OpenAI is opening free frontier access to up to 200,000 scientists, with applications from Nov 23 ([program](https://x.com/OpenAI/status/2122377585523126615)).

  - **Productivity study**: OpenAI reports scientists roughly doubled their productivity in a study of 1,800 participants ([study](https://x.com/OpenAI/status/2122380010287960095)).

  - **Physics showcase**: Physicist Nathaniel Craig used GPT-6 Astra to develop a candidate lattice definition of the Standard Model ([blog](https://x.com/OpenAI/status/2122123437141553630)).

  - **Healthcare**: ChatGPT for Healthcare adds computer-use agents and more than 20 plugins ([healthcare](https://x.com/OpenAI/status/2122465625109090646)).
- **Claude Code**: The desktop app now runs iPhone Duo in the iOS simulator and can fold and unfold the device to test layouts ([ClaudeDevs](https://x.com/ClaudeDevs/status/2122439098740674839)).

  - **Verification**: The team published how it uses cheap tests, benchmarks and end-to-end runs to let Claude verify its own work ([blog](https://x.com/ClaudeDevs/status/2122457608963395586)).

  - **Unreleased model (unconfirmed)**: A "Silicon 2" entry spotted in Devin is suspected to be an Anthropic model ([rumor](https://x.com/kimmonismus/status/2122325062531604670)).

  - **Liquid d1.2-ultra**: It ranks #1 on AA's Decision Index at $0.042 per M input tokens ([AA](https://x.com/ArtificialAnlys/status/2122350577766338883)).

  - **Sage S2.1 Flash**: The open-weight science model reports leading MLE-bench Lite and OfficeQA Pro results ([Sage](https://x.com/sageai_official/status/2122539305826681084)).

  - **FLUX-mimic-x**: A 7B video-action robotics model ranks #1 on RoboLab among real-time models ([mimic](https://x.com/mimicrobotics/status/2122343940653224018)).

  - **Unsloth computer use**: Local models can now drive desktop apps and game engines through Unsloth ([Unsloth](https://x.com/danielhanchen/status/2122365870995612059)).

**Industry and policy**

- **Nvidia.** The US Commerce and Justice departments issued a joint statement on the Megaspeed investigation. [Commerce](https://x.com/CommerceGov/status/2122533142137614375)

- **Financing and M&A**:

  - **Noeri**: Yejin Choi and Carlos Guestrin's continual-learning lab raised $110M ([Choi](https://x.com/YejinChoinka/status/2122379749372907852)).

  - **Corma**: The cyber-defense model company raised a $125M Series A at a $1B valuation ([Corma](https://x.com/AlonPluda/status/2122344413951111663)).

  - **World Labs**: The team is joining AMD ([Fei-Fei Li](https://x.com/drfeifei/status/2122377911873491397)).
- **Market data**: Anthropic's S-1 shows $16.3B revenue in H1 2026 and compute costs falling from 251% to 48% of revenue ([The Batch](https://x.com/DeepLearningAI/status/2122363904881819677)).

  - **Enterprise adoption**: Databricks reports open-model token share rose from 7.3% to 30.9% in 3.5 months ([summary](https://x.com/Yuchenj_UW/status/2122392225908924604)).

* * *

## AI Reddit Recap

**Local models and hardware**

- **What fits in 16 GB?** Unsloth expects a smaller Qwen4-27B to run in 16 GB. Local users want to see the quantization and how much memory is left for the context window. One developer already splits coding jobs between Opus for specs and a local Qwen3.8-27B for implementation. [Memory discussion](https://www.reddit.com/r/LocalLLM/comments/1xxkdzg/qwen427b_reported_to_run_on_16_gb_of_ram/) · [Local workflows](https://www.reddit.com/r/LocalLLM/comments/1xxe89d/qwen_officially_announces_qwen4max_and_qwen4/)

- **Hy4 Flash.** Tencent’s multimodal model has 224B parameters and activates 12B per token. At $0.070 per million input tokens and $0.281 per million output tokens, it gives developers a cheaper way to try Hy4’s 1M context window. [Tencent’s release](https://hy.tencent.ai/research/hy4-flash) · [Discussion](https://www.reddit.com/r/LocalLLaMA/comments/1xxlgpb/introducing_hy4_flash/)

- **EUROPA.** A reported test run on the JUPITER supercomputer trained a roughly 952B-parameter mixture of experts with 32.8B parameters active per token. The local-model community’s question is why the team chose that scale for its first model. [Training run and discussion](https://www.reddit.com/r/LocalLLaMA/comments/1xwr917/europa_the_eus_open_ai_model_is_aiming_for_nearly/)

- **Where do two quantizations diverge?** inside-llm loads two GGUFs with the same tokenizer and lets you step through their predictions side by side. Watch the first token where they disagree, then compare the probabilities they assigned to the alternatives. [Project](https://github.com/manuelgscom/inside-llm) · [Demo and discussion](https://www.reddit.com/r/LocalLLaMA/comments/1xx7qbf/i_have_created_a_tool_to_observe_what_happens/)

- **Basalt’s 770 tokens/sec headline depends on what you generate.** The developer reports up to 776 tokens/sec for structured output from Qwen3.8-27B, 262 in chat and 194 for prose. Readers asked for longer quality tests of the 4-bit prefill. [Basalt](https://github.com/jesdga95/basalt) · [Benchmark thread](https://www.reddit.com/r/LocalLLaMA/comments/1xx1cu9/basalt_040_more_models_still_as_fast_qwen_27b_up/)

- **An FPGA running Qwen 27B.** Valley Semiconductor reports 7.9 tokens/sec on short prompts and about 5.5 in live chat. Its 50 tokens/sec target at 20 W is a projection for a future chip. [Live demo](https://valleysemiconductor.com/) · [Hardware discussion](https://www.reddit.com/r/LocalLLM/comments/1xy3dgz/we_built_a_chip_prototype_that_runs_qwen_27b_you/)

**Agents in use**

- **Claude Meetings.** A CTO describes inviting Claude into a 30-minute Google Meet. It updated project records in Notion and called the team’s own tools to update bids during the meeting. [Field report](https://www.reddit.com/r/ClaudeAI/comments/1xy3iuo/started_using_claude_meetings_today/)

- **A Minecraft test for Hydrogen.** A developer asked Gemini 4 Hydrogen to recover missing source and build a datapack for 11 Minecraft versions. The agent reportedly launched two graphical clients to test fixes for dropped items and raycasts. [Developer’s account](https://www.reddit.com/r/Bard/comments/1xxed6e/gemini_4_hydrogen_a_skeptics_review/)

- **What teams hire a harness developer to do.** One example from the discussion: a coding agent can propose a change, while the harness decides which repository it may write to, which CI test counts as passing and when a person needs to approve the next step. [Claude Code discussion](https://www.reddit.com/r/ClaudeCode/comments/1xxk5ak/what_actually_is_an_ai_harness_and_why_are/)

**Vision and video**

- **Testing a video model’s physics.** After a second MiniMax H3 demo, commenters proposed a harder test: describe the scene, leave the outcome out of the prompt and see whether the model predicts what the objects will do. [Demo and discussion](https://www.reddit.com/r/StableDiffusion/comments/1xwulg7/physics_understanding_of_minimax_h3_part_2/)

Thanks for reading

## Stay a little ahead of the curve.

Fresh ideas, original reporting, and deep conversations in your inbox.

[Subscribe to Latent Space](https://latent.space/subscribe/)
