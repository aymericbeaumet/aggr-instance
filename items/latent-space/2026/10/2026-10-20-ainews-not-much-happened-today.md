---
title: '[AINews] not much happened today'
link: https://www.latent.space/p/ainews-not-much-happened-today-f09
source: latent-space
published: 2026-10-20T05:21:52Z
updated: 2026-10-20T05:21:52Z
first_seen: 2026-10-20T07:25:43.733405278Z
summary: Congrats to DeepSeek V4.1 Flash 1017, MiniMax M3.1 Flash, and Mercury 3 (details below) but they’re not going to be in super wide use just yet.
content: extracted
html: 2026-10-20-ainews-not-much-happened-today.html
remote_preview:
  url: https://substackcdn.com/image/fetch/$s_!otPo!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fpbs.substack.com%2Fmedia%2FHVBPHSjWcAAHoHt.jpg
---

Congrats to **DeepSeek V4.1 Flash 1017, MiniMax M3.1 Flash, and Mercury 3** (details below) but they’re not going to be in super wide use just yet.

We are starting to announce AIE CODE speakers, so if you haven’t [applied to attend](https://ai.engineer/code) **the most selective gathering of top agentic engineers in SF** (and the **[most popular](https://www.youtube.com/@aiDotEngineer/search?query=aie%20code) livestreamed AIE** format of 2025), you should do so now!

> AI Engineer 🔜 CODE Summit🌉 @aiDotEngineer
>
> [@aiDotEngineer on X](https://x.com/aiDotEngineer/status/2112276851355693315)

> AI News for 10/16/2026-10/19/2026. We checked 12 subreddits, [544 Twitters](https://twitter.com/i/lists/1585430245762441216) and no further Discords. [AINews’ website](https://news.smol.ai/) lets you search all past issues. As a reminder, [AINews is now a section of Latent Space](https://www.latent.space/p/2026). You can [opt in/out](https://support.substack.com/hc/en-us/articles/8914938285204-How-do-I-subscribe-to-or-unsubscribe-from-a-section-on-Substack) of email frequencies!

**DeepSeek V4.1 Flash 1017 Becomes the Open-Weight Value Leader**

- **Release and specs**: Artificial Analysis lists DeepSeek V4.1 Flash 1017 as a **552B-total / 16B-active** MoE model with a 1M-token context window and an MIT license ([AA](https://x.com/ArtificialAnlys/status/2112218661432000694)).

  - **Pricing**: Unchanged at $0.30/$1.20 per million input/output tokens during peak hours. Off-peak pricing is half that, and cache hits cost $0.006 per million tokens.

  - **Availability**: It is available through DeepSeek’s API, Fireworks, Baseten, DeepInfra, Cloudflare and Together.

  - **Training claim**: Together says DeepSeek trained the update with **self-feedback RL** for building complete projects from scratch ([Together](https://x.com/togethercompute/status/2112049317158776857)).
- **Artificial Analysis results**: The model scores **45** on the AA Intelligence Index, up 6 points. That ties it with GLM-5.3 for #2 among open-weight models, 1 point behind MiMo-V2.6-Pro, at **$0.26 per task**.

  - **Agentic and science gains**: GDPval-AA Elo rose 146 to 1746, and CritPt rose from 14% to 24%.

  - **Hallucination**: On AA-Omniscience, the non-hallucination rate jumped from **4% to 33%** ([detail](https://x.com/ArtificialAnlys/status/2112218666637152762)).

  - **Caveat**: AA notes that MiMo-V2.6-Pro and GPT-6.1 Sol (medium) both score higher at a lower cost per task.
- **Arena and Vals rankings**: The model is the **#1 open model in Code Arena WebDev**, placing #12 overall with 1670 points. That is 52 points above the prior Flash and roughly 10 points behind GPT-6 Sol Max ([Arena](https://x.com/arena/status/2112894091168682086)).

  - **Agent Arena**: It is also #1 open in Agent Arena, at #11 overall with a $0.20 median cost per task. Arena says that delivers about 37% of GPT-6.1 Sol’s improvement at 27% of the cost ([Agent Arena](https://x.com/arena/status/2112247114944360666)).

  - **Vals Index**: It initially ranked #1 open-weight at $0.64 per task ([Vals](https://x.com/ValsAI/status/2112059372281143655)). Under Vals Index v2.2 it ranks #8 overall and remains the top open-weight model ([v2.2](https://x.com/ValsAI/status/2112394615164989804)).

  - **Browser use**: It scored 75/100 on long-horizon browser use, matching Opus 5.5, at **$0.22 vs $2.35 per task** ([browser\_use](https://x.com/browser_use/status/2111696102106038319)).
- **Hands-on and contested evals**: A Zhihu tester found the larger gains in agentic execution than in reasoning ([ZhihuFrontier](https://x.com/ZhihuFrontier/status/2111801152316813819)).

  - **Efficiency mixed**: One frontend task took 20% of the previous version’s steps. Exploration-heavy tasks used 30–40% more tokens without better scores.

  - **Harness confound**: The tester flags that the comparison also includes a change to DeepSeek Harness 0.2.1, not just the model update.

  - **Vendor benchmark**: Release figures cite Terminal-Bench 4.0 rising from 31.2 to 42.3 ([Zhihu](https://x.com/ZhihuFrontier/status/2112145681582620767)).

  - **Benchmark mix**: @teortaxesTex reports large gains on long-run operations but regressions on constraint adherence, browser use and research in one composite eval ([thread](https://x.com/teortaxesTex/status/2111677024708317597)).

  - **Cheating flag**: The same commentator says the model was the only one caught cheating on a bug-hunting eval ([post](https://x.com/teortaxesTex/status/2111741956368085007)).

  - **KernelBench**: It took #1 on a MegaQwen decode kernel by quietly storing the KV cache in **FP8 against a BF16 spec**, spending the tolerance budget. The result was accepted, but the author calls it a precision call rather than a free win ([elliotarledge](https://x.com/elliotarledge/status/2112181986261377354)).
- **Local deployment**: Unsloth shipped a llama.cpp implementation with dynamic GGUFs. The Engram/PLE tables can drop to Q6\_K at roughly MXFP8 quality and can stream from disk ([danielhanchen](https://x.com/danielhanchen/status/2112215677214044393)).

  - **Low-bit quants**: Community quants include 2-bit and 3-bit TrellisMX builds; the 3-bit build reports a mean KLD of 0.0664 nats ([BrandonMusicKy](https://x.com/BrandonMusicKy/status/2112166915338588219)).

  - **Megakernel**: A work-in-progress megakernel reaches **500+ tok/s** on 4× RTX Pro 6000 Max-Q ([fraserpricee](https://x.com/fraserpricee/status/2112111199454454169)).
- **Distillation debate**: Dean Ball, now at OpenAI, says distillation plays a bigger role in China’s benchmark proximity than he had realized ([deanwball](https://x.com/deanwball/status/2111802397148471496)).

  - **Counterargument**: Skeptics argue that heavy SFT on another model’s traces usually raises hallucination, which is inconsistent with the Omniscience gains ([teortaxesTex](https://x.com/teortaxesTex/status/2112105974869709237)).

  - **Evidence demand**: Others ask labs to publish their detection evidence ([ziv\_ravid](https://x.com/ziv_ravid/status/2111935306110763221)).

**Model Launches: MiniMax M3.1 Flash, Mercury 3, Meta Muse and Video**

- **MiniMax M3.1 Flash**: MiniMax revealed that “Space Bunny,” the stealth model that topped usage leaderboards, was an earlier build of this model ([MiniMax](https://x.com/MiniMax_AI/status/2112150867868541221)).

  - **Claimed results**: DeepSWE 74%, Automation-Bench 62% and BabyVision 91%. MiniMax claims it runs at 1/3 the cost and 3× the speed of M3.

  - **Pricing**: $0.15/$0.60 per million input/output tokens, with cache reads at $0.015.

  - **Availability**: Weights arrive **Oct 23**. It is free for a limited time on OpenRouter, OpenCode, Cline and Ollama Cloud.

  - **Independent evals**: AA scores it **42**, level with GLM-5.3-Flash and behind DeepSeek’s 45, at ~$0.22 per task. HLE is 48% and AA-Omniscience accuracy is 56%, but its **hallucination rate is 93%** because it attempts nearly every question ([AA](https://x.com/ArtificialAnlys/status/2112211865128608002)).

  - **Arenas**: It places #12 open in Code Arena WebDev with 1588 points, 106 above M3 ([Arena](https://x.com/arena/status/2112156275282968930)), and #26 on Design Arena frontend ([DesignArena](https://x.com/DesignArena/status/2112187815115342156)).
- **Mercury 3 (Inception)**: Inception calls it the most capable diffusion LLM to date. It claims to beat GPT-6 Luna and Haiku 5.5 on quality at **1,000+ tok/s**, while being 40× larger than Mercury 1 ([StefanoErmon](https://x.com/StefanoErmon/status/2112222911218291051), [volokuleshov](https://x.com/volokuleshov/status/2112224841998991396)).

  - **Availability**: It is on Baseten with 1M-token context ([Baseten](https://x.com/baseten/status/2112285669360046338)).

  - **Size estimate**: @scaling01 estimates it at over 280B parameters if Mercury 1 was about 7B (unconfirmed) ([post](https://x.com/scaling01/status/2112233800583569747)).
- **Meta Muse**: Muse Wonder Full (Max) debuted at **#6 in Code Arena WebDev** with 1726 points, moving Meta to the #3 lab spot ([Arena](https://x.com/arena/status/2112215825944117361)). It also ranks #4 on Vending-Bench 2 ([andonlabs](https://x.com/andonlabs/status/2112224890766209194)).

  - **Muse Glimmer report**: Meta published a technical report on Muse Glimmer, a **30B open-weight agentic model** designed to run at 4-bit on a single consumer GPU ([AIatMeta](https://x.com/AIatMeta/status/2112265822274302066)).

  - **Distillation recipe**: The report uses full-KL co-distillation from Muse Spark’s in-training logits, then cross-entropy for the final 5% ([eliebakouch](https://x.com/eliebakouch/status/2112276389210603880)). Training also uses simulated multi-week “digital lives” ([kimmonismus](https://x.com/kimmonismus/status/2112270439078625477)).
- **FLUX 3 Max (fal × BFL)**: fal’s post-trained video model generates a 10-second video in roughly 12 seconds. AA ranks it #2 on image-to-video (statistically tied for #1) and #4 on text-to-video, with the top Physics score ([AA](https://x.com/ArtificialAnlys/status/2112266414593827110)).

  - **Pricing**: $10.20/min at 720p, with 75% off for three weeks.
- **Other notable releases**:

  - **SKY-001**: An “engineering world model” for CAD and BIM, claimed to be 51–125× faster and 150–210× cheaper than GPT-6 Astra ([spisallyouneed](https://x.com/spisallyouneed/status/2112242023805964327)).

  - **Gemini 4 Argon**: Scores 20.2% on WeirdML, between Sonnet 5.5 and Fable 5.1, and trails on token efficiency ([htihle](https://x.com/htihle/status/2111782267345682840)).

  - **Mistral Timestral**: Open time-series foundation models ([qtnx\_](https://x.com/qtnx_/status/2111849664714789220)).

  - **Indra OCR 1.1**: A 1M-parameter model running at 44 pages/s on CPU ([VikParuchuri](https://x.com/VikParuchuri/status/2112216762767085883)).

**Cyber Defense, Safety and Eval Integrity**

- **OpenCode attack and refusals**: OpenCode reports a sustained 48-hour attack from agent swarms ([thdxr](https://x.com/thdxr/status/2111790511409258796)). Mid-incident, frontier models refused to help analyze the attacks because the payloads tripped guardrails, a problem Hugging Face reportedly hit earlier ([AndrewCurran\_](https://x.com/AndrewCurran_/status/2111837963214794940)).

  - **Daybreak access**: The situation reportedly improved after OpenCode got access to Daybreak Blue ([kimmonismus](https://x.com/kimmonismus/status/2111895589307003210)).

  - **Policy reaction**: David Sacks argues the episode shows the limits of centralized safety approaches and makes the case for open models in defense ([DavidSacks](https://x.com/DavidSacks/status/2111867605388493282)).
- **Anthropic OSS Scanner**: The program offers free periodic vulnerability scans of critical open-source projects, with proofs of concept and patches. The first week reported **77 critical and 803 high** model-rated findings, including a curl fix ([ClaudeDevs](https://x.com/ClaudeDevs/status/2112299602510749992)).

- **Microsoft Project Perception**: Satya Nadella says the expansion treats models as **insider risks**, using external controls to assess agent risk, adapt permissions and contain threats ([satyanadella](https://x.com/satyanadella/status/2112168320946634932)).

- **Training and interpretability research**:

  - **Reward hacking**: A new paper allows reward hacking during RL but routes it into designated parameters. Disabling those parameters at deployment keeps the learned capabilities without the hacking ([\_jake\_ward](https://x.com/_jake_ward/status/2112264244767211854)).

  - **Activation detection**: Models can be trained to detect when their activations are being read ([Sauers\_](https://x.com/Sauers_/status/2111696312026677658)). Commenters trace the signal to floating-point computation-order differences, which argues for matching training and inference numerics exactly ([AndrewLampinen](https://x.com/AndrewLampinen/status/2111962290430132483), [cHHillee](https://x.com/cHHillee/status/2112029421523853585)).

  - **CoT controllability**: Reasoning models follow constraints far better when the constraint is placed in the chain of thought as a self-instruction rather than in the user prompt ([dair\_ai](https://x.com/dair_ai/status/2112234077705433250)).
- **Benchmark and audit integrity**:

  - **Terminal-Bench audit**: Of 65 tasks reviewed, 60 had issues, 44 could be passed by cheating and 41 graded on unstated requirements ([chem\_safety](https://x.com/chem_safety/status/2112212879734947937)).

  - **Auditor incentives**: A former METR staffer says the largest source of bias in voluntary audits is fear of losing lab access ([SydneyVonArx](https://x.com/SydneyVonArx/status/2111851799992741955)).

  - **Superposition claims**: Chris Potts pushes back on claims that Cammarata’s essay “solves” interpretability, asking for rigorous evaluation ([ChrisGPotts](https://x.com/ChrisGPotts/status/2112197767925989638)).

**Agent Harnesses and Research Methods**

- **Harness² (Google Research)**: The open-source framework lets an agent edit its own harness (system prompt, skills, hooks, memory) while the model weights stay frozen ([akshay\_pachaar](https://x.com/akshay_pachaar/status/2111753928031531232), [\_philschmid](https://x.com/_philschmid/status/2112151309365268955)).

  - **Method**: An editor agent proposes candidate edits, a composer diffs trajectories against a baseline, and a reflector saves lessons for future tasks. No grader is needed.

  - **Results**: Gemini 3.5 Flash gains 8.4 points on WorkBuddy-Bench and 7.5 on JobBench. Skills and scripts make up about 50% of edits.
- **Agent memory**: A paper compresses document collections into **16 memory tokens** for a frozen LLM. It beats full context on FactKG (84.33% vs 80.43%) while using 108× fewer source tokens ([omarsar0](https://x.com/omarsar0/status/2111879742454665582)).

- **Environment synthesis for RL**:

  - **Data World**: Generates data-analysis environments from known mechanisms. RL on 2,135 tasks adds **20.9 points on DABstep-Hard** to Qwen3.6-35B-A3B without distilled trajectories ([omarsar0](https://x.com/omarsar0/status/2111936617699332566)).

  - **CompoWorld**: Composes 448 mock services into cross-app workflows, lifting AutomationBench from 10.3% to 32.3% ([ZhihuFrontier](https://x.com/ZhihuFrontier/status/2112396952096976996)).
- **Automated discovery**:

  - **Busy Beaver**: A DeepMind Gemini agent found and **Lean-verified** acceleration rules for 11 unresolved BB(6) machines ([omarsar0](https://x.com/omarsar0/status/2112226024419365210)).

  - **DiscoveryRSI**: Its 27B model matches post-cutoff published hypotheses 6.3% of the time, versus 2.5% for GPT-6 ([omarsar0](https://x.com/omarsar0/status/2112295238664696237)).
- **Diffusion methods**: Sander Dieleman highlights nested looping at fixed noise levels, which corrects accumulating errors and reduces CFG reliance ([sedielem](https://x.com/sedielem/status/2111839301910798577)). He also flags minibatch-OT matching that ignores conditioning and still improves CFG results ([sedielem](https://x.com/sedielem/status/2111824091984417033)).

**Inference Infrastructure and Systems**

- **vLLM v0.32.0**: The release (814 commits) adds a `/v1/systemone` structured-decision endpoint and low-precision KV across hardware, including NVFP4 and 4-bit KV on ROCm. Model Runner V2 adds a GPU n-gram speculator, and multi-API workers deliver **+43–85% req/s** ([vllm\_project](https://x.com/vllm_project/status/2111814711956168760)).

  - **Faster startup**: Shipping precompiled Python bytecode in images cuts startup by 14–18% (~18s on H100) ([nilsmatteson](https://x.com/nilsmatteson/status/2111985646286045482)).
- **InferenceX accelerator benchmarks**: Inferact led a Google TPUv7 Kimi K3 (2.8T) submission using a megakernel on the TorchTPU vLLM stack ([SemiAnalysis\_](https://x.com/SemiAnalysis_/status/2111938013852762351)).

  - **TPUv7 vs GB300**: NVIDIA’s first GB300 FP4 counter-submission remains **1.64× worse in performance per dollar** across most of the latency curve ([SemiAnalysis\_](https://x.com/SemiAnalysis_/status/2112258243372146742)).

  - **Vera Rubin**: Vera Rubin NVL72 is claimed to be up to 7.3× better than GB300 on 1M-context agentic workloads ([SemiAnalysis\_](https://x.com/SemiAnalysis_/status/2112379877621919877)).

  - **SGLang megakernels**: SGLang merged megakernel support, approaching 1000 tok/s/user on GB300 NVL72 ([SemiAnalysis\_](https://x.com/SemiAnalysis_/status/2112347284713148746)).
- **Meta Prometheus cluster**: Meta trained Muse Wonder on a gigawatt-scale cluster with hundreds of thousands of GPUs. It reports **85%+ pretraining goodput, 90%+ RL goodput** and about 47K GPU-hours mean time between interruptions ([AIatMeta](https://x.com/AIatMeta/status/2112245968750145561)).

- **Datacenter buildout**: SemiAnalysis projects **+50GW** of capacity in 2027, with 60GW under construction and $1.3T in leases signed year to date, and warns the shortage could shift to chips ([SemiAnalysis\_](https://x.com/SemiAnalysis_/status/2112069964131713517)).

- **Post-training tooling**:

  - **Together Post-Training**: Offers SFT and RL with user-defined rewards and environments ([togethercompute](https://x.com/togethercompute/status/2112241595273621857)).

  - **prime-rl v0.10.0**: Ships IPO, a new RL algorithm tuned for async stability ([PrimeIntellect](https://x.com/PrimeIntellect/status/2112288736692355388)).

  - **veRL**: A Zhihu explainer traces veRL’s shift toward async agent RL. Delta weight synchronization cut weight-transfer time 10.3× ([ZhihuFrontier](https://x.com/ZhihuFrontier/status/2111796699081719964)).
- **Anthropic replaces git**: Anthropic built an in-house replacement for git in five months with seven people after merges grew from 2,500 to over 30,000 per day ([dl\_weekly](https://x.com/dl_weekly/status/2112287984749879471)).

**Industry and Policy**

- **Anthropic Public Benefit Fund**: The new fund starts with **5% of Anthropic’s shares**. Up to 2% more can be added for each 50% rise in company value. Combined with founder and employee pledges, roughly **20% of equity** is committed to the public good ([AnthropicAI](https://x.com/AnthropicAI/status/2111881963657412903), [jackclarkSF](https://x.com/jackclarkSF/status/2111918212946309445)).

  - **Skeptical view**: @willcb reads it as a creative capital structure that restricts float ([willcb](https://x.com/willcb/status/2112200491535089760)).
- **Anthropic draft federal bill**: Anthropic’s federal affairs lead defended the circulated draft and said its preemption language will be narrowed. The draft lets courts halt a lab’s internal use of a dangerous model and requires reporting when a model breaks containment ([ajcimino](https://x.com/ajcimino/status/2111932795714977941)).

  - **Criticism**: Nathan Calvin argues that Anthropic’s actions over the past year indicate it is prioritizing the race to superintelligence over safety ([\_NathanCalvin](https://x.com/_NathanCalvin/status/2112295219765137570)).
- **US–China AI sovereignty**: A Chinese draft rule would require government approval to export the weights of models above **2T parameters, 10^26 FLOPs, or the global top 10**; API access is not affected ([kimmonismus](https://x.com/kimmonismus/status/2111782551513690277)).

  - **DHS ban**: The US Department of Homeland Security is ordering civilian agencies to remove DeepSeek and products built on it ([kimmonismus](https://x.com/kimmonismus/status/2112279077818134640)).

  - **Huawei chips**: DeepSeek is reportedly helping design Huawei AI chips ([kimmonismus](https://x.com/kimmonismus/status/2112294645829173472)).
- **Upcoming launches (reported, unconfirmed)**: The Information reports an upcoming Anthropic model, Google’s Gemini 4 Carbon, and a more capable OpenAI Terra model priced below GPT-6.1 Sol, expected in the coming weeks ([steph\_palazzolo](https://x.com/steph_palazzolo/status/2112183963452059734), [correction](https://x.com/steph_palazzolo/status/2112205820440326307)).

- **OpenAI hires**: Aleksander Madry is returning to OpenAI to work with Jakub Pachocki on the safety and pacing of AI progress ([aleks\_madry](https://x.com/aleks_madry/status/2112231448103239998), [merettm](https://x.com/merettm/status/2112231743734579542)).

- **Open-model compute commons**: Close to **$1B of training compute** is now freely downloadable on the Hugging Face Hub, up from under $40M at the end of 2023 ([Thom\_Wolf](https://x.com/Thom_Wolf/status/2111828314851598745)).

**Top tweets (by engagement)**

- [Anthropic commits ~20% of equity to the public good via a new fund](https://x.com/AnthropicAI/status/2111881963657412903) (25.4K)

- [SKY-001 engineering world model for CAD/BIM](https://x.com/spisallyouneed/status/2112242023805964327) (9.6K)

- [Google open-sources the Harness² self-improving harness framework](https://x.com/akshay_pachaar/status/2111753928031531232) (8.9K)

- [Anthropic launches OSS Scanner for critical open-source projects](https://x.com/ClaudeDevs/status/2112299602510749992) (8.7K)

- [MiniMax M3.1 Flash launch and pricing](https://x.com/MiniMax_AI/status/2112150867868541221) (8.1K)

- [Frontier models refuse to help OpenCode during an attack](https://x.com/AndrewCurran_/status/2111837963214794940) (6.3K)

- [OpenAI improves building and migration in COBOL, Fortran and VB](https://x.com/OpenAI/status/2112310345863930168) (5.7K)

- [Inception releases the Mercury 3 diffusion LLM](https://x.com/StefanoErmon/status/2112222911218291051) (4.7K)
