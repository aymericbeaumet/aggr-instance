---
title: '[AINews] Reflection Beam - 501B-A23B American Open Model'
link: https://www.latent.space/p/ainews-reflection-beam-501b-a23b
source: latent-space
published: 2026-10-06T06:28:43Z
updated: 2026-10-06T06:28:43Z
first_seen: 2026-10-06T07:29:29.789711479Z
summary: A small win for US open source
content: extracted
html: 2026-10-06-ainews-reflection-beam-501b-a23b-american-open-model.html
remote_preview:
  url: https://substackcdn.com/image/youtube/w_728,c_limit/DIu7xA898go
---

It’s been over a year since **[Reflection launched with us](https://www.youtube.com/watch?v=DIu7xA898go)** with big goals on coding (and hinted about [their RL approach](https://www.youtube.com/watch?v=QluDzKVfp6A&t=953s)):

[www.youtube.com](https://www.youtube.com/watch?v=DIu7xA898go)

But they stayed “stealth” longer than Thinking Machines and it was not clear we would ever get a model launch out of them, as the broader open model ecosystem did not slow down one bigt for them. Well, [we did](https://reflection.ai/blog/introducing-beam):

[![](https://substackcdn.com/image/fetch/$s_!eDub!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F92c7acf0-990a-45f0-b3ee-b1d7702184bd_874x894.png)](https://substackcdn.com/image/fetch/$s_!eDub!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F92c7acf0-990a-45f0-b3ee-b1d7702184bd_874x894.png)

They compare themselves to Inkling, Nemotron, and GLM 5.2, but the SOTA GLM 5.3, Kimi K3, Qwen 3.8 Max, and DeepSeek V4.1 Flash are generally ahead. Still, since this is US-trained from scratch, there’s a segment of the market that has been eagerly waiting for more options here, and more importantly, Reflection has now announced its arrival as a functional neolab!

> AI News for 10/03/2026-10/5/2026. We checked 12 subreddits, [544 Twitters](https://twitter.com/i/lists/1585430245762441216) and no further Discords. [AINews’ website](https://news.smol.ai/) lets you search all past issues. As a reminder, [AINews is now a section of Latent Space](https://www.latent.space/p/2026). You can [opt in/out](https://support.substack.com/hc/en-us/articles/8914938285204-How-do-I-subscribe-to-or-unsubscribe-from-a-section-on-Substack) of email frequencies!

**Reflection’s Beam Leads a Wave of Open-Weight Releases**

- **Beam launch**: Reflection announced Beam, a text-only 501B-total / 23B-active MoE for coding, agentic and scientific work. It was trained from scratch, and full weights under Apache 2.0 are due this month ([announcement](https://x.com/reflection_ai/status/2107186849370247235), [Laskin](https://x.com/MishaLaskin/status/2107187101045502158)).

  - **Training scale**: Team posts cite 23.8T pretraining tokens, partly from an OCR pipeline over hundreds of millions of PDFs ([data lead](https://x.com/nayshins/status/2107195687574306905)). They also describe a stable RL/OPD run on 10K GB300s with more than 100M rollouts across ~1M tasks ([Damos](https://x.com/brandondamos/status/2107189151380521293)).

  - **Claimed results**: A summary of Reflection’s claims gives 80.9 on SWE-bench Verified, 3–4x the inference efficiency of GLM 5.2, and four weeks each of pretraining and RL on ~10,500 GB300s ([summary](https://x.com/kimmonismus/status/2107191500136157404)). A tech report and OSS integrations are promised ([Polozov](https://x.com/alexpolozov/status/2107190019903336812)).

  - **Context**: Axios reported the launch ahead of time. It said Reflection pays $150M/month for Colossus compute plus a $1B Nebius deal, and that other unnamed US labs will ship open models this month ([Curran](https://x.com/AndrewCurran_/status/2106846163534241912)).
- **Independent and critical reads**: Artificial Analysis has early access and expects Beam to be among the most token-efficient open models for its intelligence ([AA](https://x.com/ArtificialAnlys/status/2107219177132155233)).

  - **MFU and architecture**: Elie Bakouch estimates only ~12% BF16 MFU in pretraining. He reads the architecture as 3:1 interleaved global/sliding-window attention and notes better held-out code perplexity than DSv4 ([analysis](https://x.com/eliebakouch/status/2107197730942804463)).

  - **Compute comparison**: Teortaxes calls Beam an iso-FLOP replication of DeepSeek V3 ([post](https://x.com/teortaxesTex/status/2107280201906586048)). He infers ~1.3B RL sandboxes over 4 weeks, with up to 170K running at once ([sandboxes](https://x.com/teortaxesTex/status/2107292795610501463)).

  - **Positioning**: Observers place Beam around GLM-5.2 level ([iScienceLuvr](https://x.com/iScienceLuvr/status/2107190622109262179)) and below DSv4 Flash on some benchmarks ([critique](https://x.com/multiply_matrix/status/2107270079536919025)). Nathan Lambert groups it with Nvidia and Thinking Machines as strong US releases that still trail Chinese counterparts ([Lambert](https://x.com/natolambert/status/2107214330529980676)).
- **Other open and specialized models**:

  - **Aleph Alpha Kolibri**: 78B total / 3.46B active, Apache 2.0, built for German and English. Self-reported scores are 96.9% AIME 2025, 84.3% GPQA Diamond and 66.4% SWE-Bench Verified ([summary](https://x.com/kimmonismus/status/2106669429320761788)). The dataset is unreleased, and agentic evals sit well below Qwen ([Jitsev](https://x.com/JJitsev/status/2107077751454761210)).

  - **Reka Rho-1**: A 19B omni model that understands and generates text, images, video and robot actions, trained from scratch on 320 H100s in ~3 months ([announcement](https://x.com/RekaAILabs/status/2107118937490006363), [compute](https://x.com/RekaAILabs/status/2107118947690557636)).

  - **Decision models**: Command Code’s Agr (31B) and Agr-flash (360M) skip text generation and return typed values with per-option probabilities for tool calls and routing ([Agr](https://x.com/CommandCodeAI/status/2107179710925140468)). SemiAnalysis explains that TypeSafe’s Jev uses the same no-decode approach and displaces frontier models mainly in router roles ([explainer](https://x.com/SemiAnalysis_/status/2107189878530228512)).

  - **Smaller releases**: Upstage’s Solar Mini 4 (35B / 3B active, 512K context) is free on Nous Portal for two weeks ([Nous](https://x.com/NousResearch/status/2107138770088714678)). Eleven v4 Turbo tops AA’s Provider Voice TTS arena at half the price of v4 ([AA](https://x.com/ArtificialAnlys/status/2107267465604649164)).

**OpenAI vs Anthropic: Subscription Value, Speed and Evals**

- **SemiAnalysis limit testing**: SemiAnalysis tested plans from Anthropic, OpenAI, Meta, SpaceXAI, MiniMax, Moonshot, Cursor, Cognition and others. It found that Claude subscriptions deliver 5x+ more API-equivalent value than OpenAI plans ([report](https://x.com/SemiAnalysis_/status/2107204965710053510)).

  - **Methodology**: Value depends on the credit cost of each model and token type, not on list API prices ([thread](https://x.com/SemiAnalysis_/status/2107252022424531076)).

  - **Task-cost adjustment**: Adjusting for task cost narrows Claude’s edge to 1.3–2.9x ([scaling01](https://x.com/scaling01/status/2107308460157407267)).

  - **Unverified compute estimate**: One analyst claims Anthropic spends 42% of inference compute on subscriptions that earn ~10% of revenue ([chart](https://x.com/stalkermustang/status/2107210742231699903)).
- **OpenAI capacity squeeze**: Users report that new $200 sign-ups were paused and that usage limits were effectively halved across plans. GPT-6.1 Sol was positioned as the efficient alternative ([analysis](https://x.com/kimmonismus/status/2106726110868234553)). Theo describes a reversal in coding-model preference between July and September ([post](https://x.com/theo/status/2106847019319062819)).

  - **OpenAI response**: Codex lead Tibo pledged a meaningful improvement or a full reset every day for 28 days ([pledge](https://x.com/thsottiaux/status/2106845241357824205)).

  - **Day 1 speedup**: Default speed for GPT-6 Astra and GPT-6.1 Sol rose ~50%, from ~30 to ~50 TPS. The change covers all subscription surfaces and Sign in with ChatGPT partners such as OpenCode, Pi, Amp and Devin ([day 1](https://x.com/thsottiaux/status/2107158998495748264), [TPS](https://x.com/thsottiaux/status/2107159119107146237)).

  - **Friction**: Banked Codex resets expire without timezone adjustment ([report](https://x.com/eliebakouch/status/2106972770349535296)). The always-on dots agent is limited to $100+ Pro plans ([criticism](https://x.com/kimmonismus/status/2107101248683954394)).

  - **Enterprise demand (reported)**: The Information reports that Microsoft cut projected internal Anthropic spend by more than a third. It also reports Meta’s Claude Code users fell from ~60K to ~30K, largely because of a push to Meta’s own tools ([summary](https://x.com/kimmonismus/status/2107229090595877092)).
- **Leaderboards**:

  - **Agent Arena**: Anthropic holds #1 in Code, Work and Chat. Fable 5.1 leads Code and Work, while GPT-6 Astra places #2 in Code ([Arena](https://x.com/arena/status/2107184642944389402)).

  - **Design Arena**: GPT-6 Astra is #1 in 3D Design, Frontend, Full Stack and Image-to-HTML ([Design Arena](https://x.com/DesignArena/status/2106884950691868702)).

  - **Hallucination**: On AA-Omniscience, Gemini 4 Argon guesses wrong on 15% of questions it doesn’t know, versus 29% for the next best model. GPT-6 Astra has the highest accuracy at 61% ([data](https://x.com/merge_api/status/2107138610877104342)).

**Agent Harnesses, RL Environments and Developer Tooling**

- **Multi-harness RL (Hugging Face)**: A capture proxy speaks the OpenAI Chat, OpenAI Responses, Anthropic and Gemini formats. It forwards calls to vLLM and records exact token IDs and logprobs for TRL, so 10 unmodified harnesses become RL environments ([Delangue](https://x.com/ClementDelangue/status/2107120717980471638), [explainer](https://x.com/akshay_pachaar/status/2106723534429184017)).

  - **Results**: The same weights score 62% under Mini-SWE-Agent and 33% under Claude Code. Training LFM2.5-2.6B across 4 harnesses lifts first-attempt solves from 42% to 54%, and a tool-call bonus cuts calls by 31%. SFT on 3,189 rollouts plateaus at 47.5%.

  - **Caveats**: The run used one task family and one seed.

  - **Environment hosting**: RL environments are now hosted and versioned on the HF Hub like datasets ([blog](https://x.com/ben_burtenshaw/status/2107116097614799312)).
- **Pi Durable**: Earendil’s harness is built around a small task-based workflow engine, so long-running, multiplayer agents can suspend and resume anywhere ([Pi](https://x.com/pidotdev/status/2107033061905104941)).

  - **Design**: The core is ~15K lines of TypeScript with SQLite/JSONL storage and runs on Bun or Cloudflare Durable Objects. Control is separated from execution environments ([review](https://x.com/realchendahuang/status/2106739355754869217)).

  - **Effect.ts**: The authors explain they skipped Effect because it does not provide durability ([Zechner](https://x.com/badlogicgames/status/2107082425373446245)).
- **Agent memory**: Cognition launched Devin “Dreaming,” which prunes and links a memory graph overnight. It is open-sourcing the git- and markdown-backed format as Agent Memory Repo ([launch](https://x.com/cognition/status/2107165034463867001), [format](https://x.com/walden_yan/status/2107185315014144357)).

- **Cursor SDK**: The update adds mid-run steering, background subagents that report back to the parent, replaceable system prompts, and MCP `readOnlyHint` and `destructiveHint` annotations on custom tools ([steering](https://x.com/cursor_ai/status/2107141004482793827), [annotations](https://x.com/cursor_ai/status/2107141038427308473)).

- **DeepSeek Harness**: An experimental Claude Code Mods compatibility layer in v0.2.1-alpha.1 tests whether DSH’s “everything is a plugin” architecture is a superset of Claude Code’s extension points ([team post](https://x.com/ZhihuFrontier/status/2106672853567283237)).

- **Routing and access**:

  - **Cline**: Its Pareto 26.10 Preview routes across models and grades answers, claiming $0.24 versus $13.41 per task at equal DeepSWE score ([Cline](https://x.com/cline/status/2107202446812733546)). Cline also paused its free DeepSeek-V4.1-Flash promotion over abuse ([notice](https://x.com/cline/status/2106828852353974713)).

  - **ChatGPT**: Custom MCP servers no longer require developer mode ([post](https://x.com/mxstbr/status/2107166154242572454)).

**Agent and Training Research**

- **Verification over sampling**:

  - **NVIDIA mid-harness**: The method samples candidate shell commands and verifies them before running one. A GPT-5.6 Sol verifier choosing among 8 actions lifts TerminalBench-Lite Pass@1 from 50% to 68%, while weak verifiers add little ([summary](https://x.com/dair_ai/status/2106700907106943107)).

  - **Google VeriHarness**: The method challenges claims that all rollouts agree on and resolves disagreements against workspace evidence. It adds +6.2 points with Gemini 3.5 Flash and +6.4 with Opus 4.8, and ~26K rollouts are released ([summary](https://x.com/omarsar0/status/2106700905051746803)).
- **Context management**:

  - **UT Austin compression study**: Across ~35K runs, compression that uses a third of the tokens can be 20–80% slower than full context. Threshold triggers beat step triggers, and the best policy varies by model ([summary](https://x.com/omarsar0/status/2106927371366596692)).

  - **PAIR**: The method replays an agent from the same state to isolate harmful compressions. It then rewrites the compression prompt and comes close to no-compression performance ([paper](https://x.com/dair_ai/status/2107264582687584567)).

  - **CorpusMap**: Precomputed entity pages for document collections raise answer quality 6.4–11.7 points while cutting input tokens 34–57% ([paper](https://x.com/dair_ai/status/2107146326639358188)).
- **Self-improving harnesses**:

  - **SelfSearch**: The method reaches a claimed 82.0% on Terminal-Bench 2.1 with DeepSeek V4 Flash, matching Codex, for $4.03 in search cost ([paper](https://x.com/omarsar0/status/2107123966792052859)).

  - **EverMind Raven**: Its evolved research harness hits 69.3% on BrowseComp ([paper](https://x.com/dair_ai/status/2106902948173529447)).
- **Optimization and architecture**:

  - **Dust**: A zeroth-order method using activation-perturbation “virtual populations” approaches, and sometimes exceeds, backprop on transformer pretraining. It claims to be 1,000–10,000x more compute-efficient than EGGROLL ([thread](https://x.com/industriaalist/status/2107194534501433804)).

  - **LOOM**: Looped MoEs train stably at 9–12 loops, and a 700M model is best at 5 loops at iso-FLOP ([thread](https://x.com/Shiwei_Liu66/status/2106980239238901881)).

  - **Policy gradient on ImageNet**: Ian Osband shows exact policy gradient reaches 4% on ImageNet versus 62% for cross-entropy, arguing that RL-loss failures are not just exploration problems ([post](https://x.com/IanOsband/status/2107101510236844333)).

  - **RL dynamics**: Base Labs finds RL updates are less low-rank than claimed ([rollout](https://x.com/baselabs/status/2107155320569053592)). Datalab reports RL alone eliminates tool-call loops at temperature 0, versus a 92% loop rate for SFT ([writeup](https://x.com/VikParuchuri/status/2107198657132953661)).
- **AI for science**: Vals AI reports that 90+ Opus 5.5 agents ran DFT simulations over 3 days and flagged two room-temperature magnetic semiconductor candidates, one synthesized back in 1999. The results are predictions only, with a public ledger ([thread](https://x.com/ValsAI/status/2107204457738256749), [caveats](https://x.com/ValsAI/status/2107204461374648618)).

- **Agent spend**: Epoch estimates OpenAI researchers’ coding-agent spend, valued at API prices, has doubled roughly monthly. The median researcher was at ~$600/day by mid-August ([Epoch](https://x.com/EpochAIResearch/status/2107174397698289762)).

**Inference Systems and Hardware**

- **OpenRouter pricing distortion**: Horace He shows GLM 5.3 priced at $0.08/M input but $5.00/M output on inference.net. He attributes this to OpenRouter’s inverse-square price routing and apparent overweighting of input price ([thread](https://x.com/cHHillee/status/2106905219116503255), [routing](https://x.com/cHHillee/status/2106905222719385664)).

- **llama.cpp**:

  - **Speculative decoding on Metal**: New kernels make speculative decoding up to 3.4x faster than plain decoding on an M3 Ultra (110 vs 32.1 tok/s) ([qvac](https://x.com/qvac/status/2107043339799593421)).

  - **v0.6.0**: Adds Clef text and vision support, Qwen3.8-Flash-Next, and a new `llama_batch_ext` API ([Gerganov](https://x.com/ggerganov/status/2107190267887632462)).
- **Agentic kernel work**: Baseten reports an engine built in a week of mostly autonomous agent work, with 90% faster decoding and 57% lower TTFT than the open-source baseline ([blog](https://x.com/baseten/status/2107114828787593307)).

- **Communications**: NCCL and PyTorch symmetric memory speed up small-to-medium collectives ([Bekman](https://x.com/StasBekman/status/2106978810247917610)).

- **Chinese accelerators**: Alibaba T-Head’s Zhenwu V900 has 216 GB of memory and 1,200 GB/s interconnect, claims 3x the M890, and ships Q1 2027 ([SemiAnalysis](https://x.com/SemiAnalysis_/status/2106821919622279225)).

**Safety, Policy and Industry**

- **OpenAI text watermarking**: OpenAI will add invisible statistical watermarks to eligible ChatGPT and Codex text in the EU under the AI Act, with an opt-in API toggle worldwide ([announcement](https://x.com/OpenAI/status/2107164650249101695)).

  - **Limits**: Rewriting or translation removes the watermark, and only approved researchers get the detector ([limits](https://x.com/OpenAI/status/2107164653147340988)).

  - **Robustness figure**: One cited test shows 25% synonym replacement dropping detection from ~92% to 17% ([critique](https://x.com/kimmonismus/status/2107168462959186341)).
- **Agent incidents and safety governance**:

  - **Bengio op-ed**: In the FT, Bengio argues recent agent hacks are not merely sandbox problems ([op-ed](https://x.com/Yoshua_Bengio/status/2107137278199906340)). He also cites a Quinnipiac poll in which 86% back independent safety standards ([poll](https://x.com/Yoshua_Bengio/status/2107115683880259605)).

  - **HF incident**: Neel Nanda calls the OpenAI x Hugging Face incident the most striking alignment failure so far ([Nanda](https://x.com/NeelNanda5/status/2107235249570873374)).

  - **Systems safety**: Ryan Lowe calls for nuclear-style layered systems safety at the labs ([Lowe](https://x.com/ryan_t_lowe/status/2106771912680456292)).

  - **Shutdown resistance**: An OpenAI alignment post documents what one researcher calls the most realistic precursor shutdown-resistance behavior seen so far ([link](https://x.com/idavidrein/status/2107213145681080666)).
- **Policy voices**:

  - **Autonomous weapons**: Former OpenAI researcher Joshua Achiam called for bans on certain autonomous weapons akin to chemical weapons ([post](https://x.com/jachiam0/status/2106757427723120909)). He is joining IFP and FAI as a fellow ([announcement](https://x.com/jachiam0/status/2107146309740466642)).

  - **Expert survey**: The LEAP panel of 250+ experts most supports an international body with US and China membership and pre-release authorization power, and opposes federal preemption ([FRI](https://x.com/Research_FRI/status/2107150179204010267)).

  - **Altman on trade-offs**: Sam Altman told Politico that “the world should accept some bad things happening” for the technology’s benefits ([Politico](https://x.com/politico/status/2106851857423282648)).
- **Compute access in China**:

  - **Tencent lease (reported)**: Per the FT, Tencent leased ~100K advanced chips in Oracle’s Southeast Asian data centers for ~$7B over five years ([summary](https://x.com/kimmonismus/status/2107012584670904477)).

  - **Smuggling charge**: US prosecutors charged a California reseller with smuggling more than $300M of GPU servers to China ([report](https://x.com/kimmonismus/status/2107006843587330382)).
- **Consolidation**:

  - **AMD and World Labs (reported)**: AMD reportedly bought World Labs for $8.2B ([DL Weekly](https://x.com/dl_weekly/status/2106716269244256685)).

  - **NVIDIA neutrality**: SemiAnalysis questions NVIDIA’s hardware neutrality after its SchedMD/SLURM and Hugging Face acquisitions ([SemiAnalysis](https://x.com/SemiAnalysis_/status/2106942597532963194)).

**Top tweets (by engagement)**

- [Tibo: a Codex improvement or reset every day for 28 days](https://x.com/thsottiaux/status/2106845241357824205) — 30.9K

- [GPT-6 Astra and 6.1 Sol ~50% faster across subscriptions](https://x.com/thsottiaux/status/2107158998495748264) — 22.2K

- [Achiam: ban certain autonomous weapons](https://x.com/jachiam0/status/2106757427723120909) — 11.2K

- [OpenAI EU text watermarking](https://x.com/OpenAI/status/2107164650249101695) — 7.7K

- [Reflection introduces Beam](https://x.com/reflection_ai/status/2107186849370247235) — 7.4K

- [Vals AI: Opus 5.5 agents find magnetic semiconductor candidates](https://x.com/ValsAI/status/2107204457738256749) — 5.8K

- [Theo: Anthropic vs OpenAI for coding, July vs September](https://x.com/theo/status/2106847019319062819) — 5.2K

- [HF: coding harnesses as RL environments](https://x.com/ClementDelangue/status/2107120717980471638) — 2.5K
