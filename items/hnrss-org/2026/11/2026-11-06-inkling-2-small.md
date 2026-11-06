---
title: Inkling 2 Small
link: https://thinkingmachines.ai/news/inkling-2-small/
source: hnrss-org
published: 2026-11-06T00:00:24Z
updated: 2026-11-06T00:00:24Z
first_seen: 2026-11-06T11:27:16.888185679Z
authors:
- Philpax
content: extracted
html: 2026-11-06-inkling-2-small.html
remote_preview:
  url: https://thinkingmachines.ai/news/inkling-2-small/images/cover-social.png
  alt: Introducing Inkling 2 Small
---

Today we are introducing our second model generation with Inkling 2 Small. It is a Mixture-of-Experts model with 594B total parameters, 17B active, and supports image inputs and variable effort.

Inkling 2 Small far surpasses our previous models’ scores on the Artificial Analysis Intelligence Index with a score of 31, reflecting gains in performance on agentic coding, vision, knowledge work, and reasoning. The increase in capabilities comes from improvements made at every step of our training process, including more efficient pre-training and a large, unified RL run.

Inkling 2 Small will initially be available through Tinker, with closed weights, following the approach we outlined in a [safe path to open weights](https://thinkingmachines.ai/blog/a-safe-path-to-open-weights/). We intend to make the full weights available under an Apache 2.0 license next week after the completion of external safety testing and red-teaming.

You can try Inkling 2 Small on Tinker Playground today. The model is also available for fine-tuning and sampling on Tinker; checkpoints will be downloadable with the release of Inkling 2 Small’s weights.

## Capabilities

Inkling 2 Small is more capable across the board than our first generation, particularly in long-horizon autonomous work.

Inkling 2 Small’s vision and agentic capabilities enable it to incorporate visual feedback to iteratively build and refine websites, games, and 3D designs with interactive elements.

Inkling 2 Small uses Three.js to build a full city simulator with no pre-made assets, generating every building, texture and car procedurally, allowing users to grow a living city from a single road while managing power, budgets and emergencies.

A from-scratch PC emulator written in Python. It boots a real Linux kernel and provides a web interface that boots a live instance in the browser.

A hand-drawn valley on plain paper, with origami cranes landing on Inkling. Generated with Inkling 2 Small in Blender; revised with improvements to the water and chimney.

A floor-plan walkthrough generated from a map.

### Agentic coding and autoresearch

Inkling 2 Small was trained with RL on hundreds of thousands of long-horizon coding tasks across multiple agent harnesses and context-compaction strategies. We increased the quality of our synthetic coding data pipeline and moved beyond unit-test rewards alone; many tasks included an agentic RL grader that evaluated how the model approached the problem, not just whether its code passed. We also tested early checkpoints of Inkling 2 Small on [OpenCode](https://opencode.ai/) under the name Fledge-Alpha to assess our model’s performance on coding tasks in the real world.

The result is a model that is both stronger at coding and easier to use inside real agent loops. Inkling 2 Small improves on usability compared to Inkling: it follows instructions better, considers changes carefully, and verifies its work without being asked.

| Coding and Autoresearch                           | Inkling 2 Small                            | GLM-5.3-Flash                              | DeepSeek-V4.1-Flash-1017 | Muse Spark Open                                   | Kimi K3 |
| ------------------------------------------------- | ------------------------------------------ | ------------------------------------------ | ------------------------ | ------------------------------------------------- | ------- |
| 25.4% (Second-highest reported score in this row) | 14.2%                                      | 28.5% (Highest reported score in this row) | 7.6%                     | 17.3%                                             |         |
| 72.1% (Second-highest reported score in this row) | 63.4%                                      | 77.0% (Highest reported score in this row) | 60.0%                    | 68.5%                                             |         |
| 86.3% (Second-highest reported score in this row) | –                                          | –                                          | 82.4%                    | 88.2% (Highest reported score in this row)        |         |
| 81.6%                                             | 30.8%                                      | 87.7% (Highest reported score in this row) | 76.2%                    | 85.0% (Second-highest reported score in this row) |         |
| 31.9% (Second-highest reported score in this row) | 36.8% (Highest reported score in this row) | 30.5%                                      | –                        | 31.5%                                             |         |
| 5.7%                                              | 4.8%                                       | 12.9% (Highest reported score in this row) | 1.9%                     | 8.1% (Second-highest reported score in this row)  |         |

Kernel optimization (PMPP-Hard)

69 tasks · Four independent attempts · 10-hour budget

View full results

PMPP-Hard results
| Model  | AUC (task-hours) | Mean solved / 69 | Pass@4 / 69 |
| ------ | ---------------- | ---------------- | ----------- |
| 616.39 | 63               | 63               |             |
| 583.86 | 60.5             | 63               |             |
| 499.65 | 60               | 64               |             |
| 497.36 | 55.25            | 62               |             |
| 494.27 | 54.25            | 64               |             |
| 478.39 | 50.25            | 61               |             |
| 453.58 | 55.75            | 66               |             |
| 430.96 | 52.5             | 61               |             |
| 389.12 | 42               | 61               |             |

Rows are ordered by AUC: the area under the mean curve over 0–10 hours, with a maximum of 690 task-hours.

Each model uses its maximum available reasoning setting, the PI harness and a shared compaction policy. We include fixes for task and grading issues identified during evaluation. Each submitted solution receives two independent LLM audits, with a third review resolving disagreements.

[PMPP-Hard](https://blog.sinatras.dev/PMPP-Hard) is an external GPU kernel-optimization benchmark, adapted here for consistent cross-model evaluation. Curves show the mean across four independent attempts; legend values are mean tasks solved at 10 hours.

Beyond conventional software tasks, Inkling 2 Small was also trained on long-horizon autoresearch tasks. This included agentic GPU kernel optimization and ML tasks such as post-training a model autonomously, all run across a range of GPU architectures.

Preliminary kernel autoresearch results

DeepSeek-V3.2 sparse attention, forward + backward

DeepSeek Sparse Attention lets each query attend only to the top-k earlier tokens chosen by a lightweight indexer, so long-context cost no longer grows with the square of the context. Starting from the public TileLang kernels, make the training kernels faster while keeping their error close to the original's (tight tolerances), with no time limit. The benchmark covers several model sizes in different regimes, from packed pretraining sequences to long-context training. The public TileLang baseline is not tuned for B300, which leaves ample headroom on these workloads to measure each model's optimization capability and perseverance. Speedups are for these kernels alone, not end-to-end training.

To demonstrate its end-to-end autoresearch performance, we tested Inkling 2 Small on a task [Proximal](https://www.proximal.so/) built for one of our internal benchmarks. Inkling 2 Small autonomously trained Qwen3.5-4B on Tinker to predict various time series such as weather and Wikipedia page views. It started by analyzing the tasks and writing its own evaluator, experimented with training datasets and recipes, and tried various approaches to SFT and RL, while debugging the process as it goes. After 4.5 hours of work and $426 in Tinker spend, the trained model has more than doubled its forecasting score.

We want Qwen/Qwen3.5-4B to forecast time series. The model gets a window, the recent history of one series as text, and writes the values of the next steps. It is sampled several times for each window. The median of its forecasts should be close to what happens, and their spread should match how uncertain it is. \[...\] Train a LoRA of Qwen/Qwen3.5-4B with Tinker. \[...\] You have 12 hours and a Tinker budget of 500 USD.

**Bash** ls /app /data && cat /data/README.md

/app: examples.jsonl forecast … (25 more lines)

**Bash** tinker-usage 2>&1 | head -20; echo ---; tinker-usage --prices 2>&1 | head -40

time 0 h 0 min of 12 h (11 h 59 min left)

Now let me look at how sampling works with Tinker:

\~/tinkerbench/time-series-forecasting/ 0h 00m into the run

**Forecasts on its evaluation examples** Qwen3.5-4B (base)

**electricity**

US Southwest grid · demand

**wind & solar**

New York grid · wind power

**air quality**

Winnipeg · PM2.5

**page views**

Wikipedia · “Valhalla”

observed model's forecast

"Less error" is compared with simply copying the last values forward.

**Evaluation scores (% less error than copying**)100 examples

$0.00Tinker spend, of $500

Data: U.S. Energy Information Administration (EIA-930); weather and air quality data by Open-Meteo.com (CC BY 4.0), containing modified Copernicus Climate Change Service and Copernicus Atmosphere Monitoring Service information 2026; Wikimedia pageviews (CC0).

Start. Inkling 2 Small reads the task and the given raw data, covering years of hourly and daily history from five sources: electricity demand, wind and solar power, weather, air quality and Wikipedia page views.

### Vision interpretation and creation

Inkling 2 Small processes image inputs using a 292M-parameter vision encoder based on Perception Encoder [Bolya et al., 2025](https://arxiv.org/pdf/2504.13181).. We use 20×20-pixel patches with 2×2 spatial pooling, and interleave image patches with text tokens. The model demonstrates strong visual capabilities across multiple categories, including OCR, grounding, infographic interpretation, and visual reasoning.

| Visual reasoning and creation                    | Inkling 2 Small                                        | GLM-5.3-Flash                                     | DeepSeek-V4.1-Flash-1017                               | Muse Spark Open                                        | Kimi K3 |
| ------------------------------------------------ | ------------------------------------------------------ | ------------------------------------------------- | ------------------------------------------------------ | ------------------------------------------------------ | ------- |
| 25.2% (Highest reported score in this row)       | 15.4%                                                  | 17.2%                                             | 18.6%                                                  | 22.0% (Second-highest reported score in this row)      |         |
| 81.6% (Highest reported score in this row)       | 79.0%                                                  | 80.6%                                             | 80.9% (Second-highest reported score in this row)      | 80.5%                                                  |         |
| 64.7% (Highest reported score in this row)       | –                                                      | 47.7%                                             | –                                                      | 58.5% (Second-highest reported score in this row)      |         |
| 33.3% (Highest reported score in this row)       | 26.8%                                                  | 30.1% (Second-highest reported score in this row) | 29.6%                                                  | 25.5%                                                  |         |
| 83.8/91.5% (Highest reported score in this row)  | –                                                      | –                                                 | –                                                      | 73.8/83.2% (Second-highest reported score in this row) |         |
| 68.6/93.7% (Highest reported score in this row)  | 53.4/82.6% (Second-highest reported score in this row) | –/92.5%                                           | 44.3/80.2%                                             | 53.4/85.7% (Second-highest reported score in this row) |         |
| 85.6/87.7% (Highest reported score in this row)  | 80.2/89.4%                                             | 77.0/85.6%                                        | 85.4/88.7% (Second-highest reported score in this row) | 84.8/91.3%                                             |         |
| 36.7/80.4% (Highest reported score in this row)  | 16.3/78.0%                                             | 17.9/83.0%                                        | 35.7/63.2% (Second-highest reported score in this row) | 26.6/68.1%                                             |         |
| 1653                                             | 1607                                                   | 1665 (Highest reported score in this row)         | –                                                      | 1656 (Second-highest reported score in this row)       |         |
| 1021 (Highest reported score in this row)        | 937                                                    | –                                                 | –                                                      | 982 (Second-highest reported score in this row)        |         |
| 1020 (Second-highest reported score in this row) | 975                                                    | –                                                 | –                                                      | 1041 (Highest reported score in this row)              |         |

The model uses vision in the loop to interpret and validate the visual coding output it generates. Pending the release of its weights, it is set to be the top open-weights model on [DesignArena](http://designarena.ai).

DesignArena Website score

DesignArena Website leaderboard. Scores from the November 5 leaderboard snapshot; the blue bar identifies Inkling 2 Small. The comparison includes the top ten models and three additional open-weights models. Bars start at 1,100, not zero. [View on DesignArena](https://www.designarena.ai/leaderboard?section=website).

We even tested Inkling 2 Small against some of our researchers in a wine tasting event. It outperformed them using vision alone, correctly identifying three wines from a single picture in 19 out of 20 trials.

![Three glasses of red wine on a restaurant table, in the left-to-right order used in the model response.](https://thinkingmachines.ai/_astro/wine-tasting-three-glasses.aWBtNGl4_Z22jTBE.webp)

**Prompt:** you’re in a blind wine tasting competition. glasses are labeled 1 to 3 from left to right. varietals: 99 cab sauv, 19 cab sauv, 23 nebbiolo

**Inkling 2 Small:** Here’s my read — with the big caveat up front that color from a phone photo under warm restaurant lighting is shaky evidence, so treat this as an educated guess, not a verdict.

**My guess:\
Glass 1 = 2019 Cabernet Sauvignon\
Glass 2 = 1999 Cabernet Sauvignon\
Glass 3 = 2023 Nebbiolo**

Continue reading Show less

Why:

- **Glass 3 (right) is by far the lightest and most translucent** — bright, cherry-red, you can see light through the bowl. Nebbiolo is famously pale for a red, even when young, so 2023 nebbiolo is the best fit. A 1999 cab would likely show more brick/orange/tawny at the rim, and this one reads more vivid ruby-garnet.
- **Glass 1 (left) is the darkest and most opaque,** nearly purple-black at its core. That’s the 2019 cab — seven years in, Cabernet still holds dense color with primary fruit.
- **Glass 2 (middle) sits between the two:** still dark, but a shade less impenetrable than glass 1 and a bit more muted/garnet. A 25+-year-old cab, assuming decent storage, should have dropped some of its purple and opacity but not turned fully brick, so it lands here.

The honest weak points in that guess: lighting is uneven (the left glass sits in the dimmer zone near the hearth, which exaggerates its darkness), and a well-stored 1999 can drink much darker than it looks on camera. Hue differences, not depth, would be the real tell — if glass 2 shows orange/brown at the rim versus glass 1’s purple-red edge, that confirms it.

Fast sensory tie-breakers when you actually taste:

- **1999 cab:** tertiary — leather, tobacco, dried cassis, forest floor, cedar; tannins resolved and silky; fruit fully softened, maybe a touch of faded fruit.
- **2019 cab:** blackcurrant/graphite/cedar, still firm structured tannins, fresher fruit, more oak presence.
- **2023 nebbiolo:** pale garnet but fierce acid and grippy, drying tannins; red cherry, rose/violet, dried herbs, a tarry note — the classic “white-wine color, marathon-runner mouthfeel” giveaway.

If the pour in glass 3 is pale but your mouth feels sanded down by tannins, that’s the slam-dunk nebbiolo confirmation; if it is soft and plush, flip glass 3 and glass 2. Good luck in the competition — report back with the reveal if you get it.

Wine-tasting photo, shown as the model saw it, and the model’s response.

### Agentic knowledge work

Inkling 2 Small accelerates knowledge work in realistic professional environments. It proficiently looks up information, calls tools over MCP, and produces high-quality deliverables. On AA-Briefcase, an end-to-end knowledge work benchmark, these capabilities come together for an ELO score of 1527.

| Knowledge Work and Enterprise Ops                | Inkling 2 Small                                   | GLM-5.3-Flash                                     | Muse Spark Open                                   | DeepSeek-V4.1-Flash-1017 | Kimi K3 |
| ------------------------------------------------ | ------------------------------------------------- | ------------------------------------------------- | ------------------------------------------------- | ------------------------ | ------- |
| 1527 (Second-highest reported score in this row) | 1451                                              | 1279                                              | 1570 (Highest reported score in this row)         | 1497                     |         |
| 1630                                             | 1649 (Second-highest reported score in this row)  | 1401                                              | 1746 (Highest reported score in this row)         | 1543                     |         |
| 70.5% (Highest reported score in this row)       | 60.7%                                             | 61.8% (Second-highest reported score in this row) | –                                                 | 54.3%                    |         |
| 73.9% (Highest reported score in this row)       | 71.1%                                             | 68.5%                                             | 71.9% (Second-highest reported score in this row) | 71.7%                    |         |
| 71.8% (Highest reported score in this row)       | 46.7% (Second-highest reported score in this row) | –                                                 | –                                                 | 43.1%                    |         |

Inkling 2 Small is especially strong in finance and quantitative reasoning over spreadsheets, filings and documents. Among open weight models, Inkling 2 Small is the best performing model across several finance benchmarks.

| Finance Work                                      | Inkling 2 Small | GLM-5.3-Flash                                     | Muse Spark Open                                   | DeepSeek-V4.1-Flash-1017 | Kimi K3 |
| ------------------------------------------------- | --------------- | ------------------------------------------------- | ------------------------------------------------- | ------------------------ | ------- |
| 61.5% (Highest reported score in this row)        | 55.8%           | 53.0%                                             | 60.9% (Second-highest reported score in this row) | 58.0%                    |         |
| 58.1% (Second-highest reported score in this row) | –               | 59.0% (Highest reported score in this row)        | 50.4%                                             | –                        |         |
| 14.3% (Highest reported score in this row)        | 7.3%            | –                                                 | 12.5% (Second-highest reported score in this row) | 5.5%                     |         |
| 14.5% (Highest reported score in this row)        | 12.3%           | 13.8% (Second-highest reported score in this row) | 12.5%                                             | 9.5%                     |         |

### Customizability

Some models respond better to fine-tuning, independent of their out-of-the-box performance. We wanted Inkling 2 Small to be a strong base model for customization.

Inkling 2 Small learns faster and improves more than our previous models when trained on the same data and algorithm across a variety of post-training scenarios, which we curated based on customer use cases. For example, starting from similarly low scores on a public financial-document summarization task, Inkling 2 Small adapts quickly, reaching the final performance of Inkling Small with just 75 training steps and finishing substantially ahead.

Evaluating Model Finetunability

Inkling 2 Small learns faster and finishes at higher performance than our previous models, and is competitive with other OSS models of similar size in an RL fine-tuning setting. We used public earnings call data to create a task of investment analysis with expert judgement, similar to the type of [financial analysis workloads our customers use tinker for](https://thinkingmachines.ai/news/learning-to-replicate-expert-judgment-in-financial-tasks/) training custom models. To ensure a fair comparison, we ran a learning rate sweep for each model; all models were trained on Tinker with [Tinker Cookbook](https://github.com/thinking-machines-lab/tinker-cookbook) abstractions using our [Async RL implementation](https://github.com/thinking-machines-lab/tinker-cookbook/blob/4a3a2829190b45999021e423bc90b7523108edf4/tinker_cookbook/rl/train.py#L421) with the [Stable Reinforce](https://tinker-docs.thinkingmachines.ai/tinker/losses/stable-reinforce/) loss, over 200 steps. Results for the Inkling models are averaged over 3 seeds; the other models use single runs. Costs are estimated by multiplying the number of sampling and training tokens with current [tinker per-token pricing](https://tinker-docs.thinkingmachines.ai/tinker/models/models_and_pricing/); for sampling, we assume prefix cache hits within rollouts among the same group.

### Reasoning, knowledge, and epistemics

Inkling 2 Small is trained on challenging STEM problems, instruction following, information seeking with and without web search, and forecasting tasks. During RL, we use proper scoring rules for forecasting tasks and uncertainty-sensitive rewards on some question-answering tasks. This trained the model to assess uncertainty, express appropriate confidence, and make well-calibrated predictions.

| Long Context Reasoning and Factuality                 | Inkling 2 Small | GLM-5.3-Flash                                          | Muse Spark Open | Mistral Large 4                                   | Kimi K3 |
| ----------------------------------------------------- | --------------- | ------------------------------------------------------ | --------------- | ------------------------------------------------- | ------- |
| 28.7% (Highest reported score in this row)            | 18.2%           | 26.3%                                                  | 17.3%           | 26.9% (Second-highest reported score in this row) |         |
| 55.7/35.0% (Highest reported score in this row)       | –/24.5%         | 52.6/29.5% (Second-highest reported score in this row) | 50.3/27.0%      | –/40.5%                                           |         |
| 83.3% (Second-highest reported score in this row)     | 80.0%           | 78.3%                                                  | –               | 88.7% (Highest reported score in this row)        |         |
| 70.6% (Highest reported score in this row)            | 68.2%           | 68.7%                                                  | 65.7%           | 69.0% (Second-highest reported score in this row) |         |
| 16.1/73.3 (Second-highest reported score in this row) | 0.7/–           | 25.8/72.0 (Highest reported score in this row)         | 5.1/71.8        | –                                                 |         |

### Safety and alignment

Our safety work for Inkling 2 Small centers on two questions: how the model performs for the general user, and how well it resists the removal of safeguards by a malicious actor. The release of Inkling’s weights is pending further safety evaluations by us and our partners.

To assess misuse risks, we trained an internal helpful-only variant. We ran both this model and the release checkpoint through a wider suite of evaluations, with more external testers, than any previous release.

On chemical, biological, radiological, and nuclear (CBRN) risk, Inkling 2 Small’s knowledge and capability scores sit below the open-weight frontier. On FORTRESS, Scale AI’s private benchmark that scores how much harmful assistance a model gives in response to expert-written adversarial prompts across CBRN, terrorism, and nonviolent crime, Inkling 2 Small has a lower Average Risk Score compared with Inkling and Inkling-Small, and also demonstrates a higher Overrefusal Score (see Limitations). The helpful-only variant of Inkling 2 Small exhibits marginal uplift on CBRN knowledge relative to the release checkpoint, but it does not significantly advance the frontier on any dimension we measured.

On offensive and dual-use agentic cyber benchmarks, Inkling 2 Small is a large step up from Inkling Small but remains behind the open-weight frontier. It matches the open-weight frontier on Vals CyberBench v1.1 and PatchEval, both of which measure the ability to find and patch security vulnerabilities.

| Safety                                           | Inkling 2 Small                                | GLM-5.3-Flash                           | DeepSeek V4.1 Flash 1017                | Muse Spark Open                                | GLM 5.3 |
| ------------------------------------------------ | ---------------------------------------------- | --------------------------------------- | --------------------------------------- | ---------------------------------------------- | ------- |
| 16.2 (Second-best reported result in this row)   | 26.7                                           | 27.2                                    | 7.6 (Best reported result in this row)  | 28.2                                           |         |
| 20.4                                             | 3.6 (Second-best reported result in this row)  | 6.9                                     | 16.0                                    | 3.2 (Best reported result in this row)         |         |
| 82.0\* (Second-best reported result in this row) | 72.7                                           | –                                       | 87.7 (Best reported result in this row) | 72.5                                           |         |
| 97.1                                             | 97.8 (Second-best reported result in this row) | 99.4 (Best reported result in this row) | 97.1                                    | 97.5                                           |         |
| 79.6 (Second-best reported result in this row)   | 74.8                                           | 80.4 (Best reported result in this row) | 77.8                                    | 69.6                                           |         |
| 72.7 (Best reported result in this row)          | 67.9                                           | –                                       | –                                       | 72.1 (Second-best reported result in this row) |         |

\* While we handle mental distress situations well, our model sometimes overreacts with disclaimers in normal emotional support queries as mentioned in the Limitations section.

As models take on unsupervised, longer-horizon autonomous work, the potential harm from their misbehavior increases. Without proper attention to alignment, capabilities-focused RL can push models to succeed at any cost. When tasks were difficult, early checkpoints of Inkling 2 Small were persistent in trying to succeed—in some cases fabricating results, ignoring instructions, and concealing what they had done.

We worked across the stack to improve model alignment. In midtraining, we filtered out traces containing misbehavior and included traces with honest, aligned behavior. In RL, we added a general alignment reward that penalizes misbehavior on every task. We also built environments targeting specific failure modes we observed in training, such as rewarding models for giving up on impossible tasks. Finally, we used on-policy self-distillation [Zhao et al., 2026](https://arxiv.org/abs/2601.18734). from a teacher model prompted with a conduct spec, so the model internalizes principles for good behavior without explicit prompting.

We measured progress with both internal and external evaluations, and found that Inkling 2 Small substantially improves over previous Inkling models on both internal and external evaluations, and outperforms a set of comparable open-weights models on our internal evals.

| Inkling + open weights                         | Closed weights |                                              |      |      |      |                                               |
| ---------------------------------------------- | -------------- | -------------------------------------------- | ---- | ---- | ---- | --------------------------------------------- |
| 14.9 (Second-best reported result in this row) | 66.8           | 75.2                                         | 82.4 | 57.1 | 68.0 | 0.8 (Best reported result in this row)        |
| 11.8 (Second-best reported result in this row) | 59.4           | 59.1                                         | 60.8 | 29.2 | 37.6 | 0.8 (Best reported result in this row)        |
| 4.7 (Best reported result in this row)         | 71.9           | 67.2                                         | 7.8  | 25.8 | 10.9 | 7.0 (Second-best reported result in this row) |
| 44 (Best reported result in this row)          | 70             | 69                                           | –    | –    | –    | 59 (Second-best reported result in this row)  |
| 8 (Best reported result in this row)           | 37             | 36 (Second-best reported result in this row) | –    | –    | –    | –                                             |
| 0 (Best reported result in this row)           | 7              | 4 (Second-best reported result in this row)  | –    | –    | –    | –                                             |

Inkling 2 Small still trails closed-weights models, such as GPT-6.1 Sol on our alignment evaluations. Reducing this gap is a priority for future models.

We also worked to secure training and evaluations with two layers of monitoring. The first is synchronous: a monitor inspects tool calls before they run. Each task declares what actions it authorizes. Deterministic rules check calls against the task’s policy and circumvention patterns, escalating uncertain cases to an LLM judge. The second layer is asynchronous: an LLM judge reviews completed rollouts from training and evaluation for misbehavior and actions against our security boundary, raising alerts that we triage and escalate into our incident process. Together, these form a defense-in-depth approach to safety and alignment that we consider critical to developing a safe model factory.

## Training Inkling 2 Small

Inkling 2 Small’s initial pretraining phase (H1) covered 8.4 trillion tokens of a broad mix of natural language, code, and image data. It ran for 68 hours on NVIDIA GB300 NVL72 racks, which enabled high expert parallelism. Training ran natively in NVFP4, the 4-bit floating-point format introduced with the NVIDIA Blackwell GPU architecture. We’ll share more details on our NVFP4 recipe in a follow-up post soon.

Internal Code

Cutoff 2026-06 · Bits per byte (lower is better)

Inkling 2 Small significantly outperforms other open-weight models on our primary held-out evaluation. Note that some models are labeled as “base-recovered”. For models where only the post-trained checkpoint was available, we fine-tuned on a dataset of pretrain-formatted code and documents before computing log loss. We validated that this procedure mostly recovers base model loss on other models where ground truth (the actual base model) is available.

We followed this initial phase with a second phase (H2) of 3.8 trillion tokens of higher-quality, newer data the model hadn’t seen, at 512K context. The third and fourth phases mixed in data in the deployment format. The third phase was about 320 billion tokens of reasoning and agent trajectories at 1M context, alongside replayed pretraining data. The fourth phase (H4) kept only the highest-quality data: about 34 billion tokens of agent trajectories. The H3 and H4 mixtures consisted mostly of trajectories sampled from earlier models including Kimi K3 and our own models, filtered for correctness and alignment.

We then carried out a joint RL run, consisting of 1,075 steps and over 17 million rollouts over 9 days. See table below for more details. This single joint run contrasts with the previous Inkling release, where we ran separate reasoning and agentic RL runs and combined them with multi-teacher distillation.

Scale of the main RL run
| Metric              | Scale |
| ------------------- | ----- |
| Over 4 × 10²³ FLOPs |       |
| 2,048               |       |
| 9 days              |       |
| 17M+                |       |
| 3T+                 |       |

The task mix included software engineering in real repositories, terminal tasks, document creation, STEM question answering, vision, web app building, tool use with simulated users, and safety. Each task family had its own grader:

- hidden unit tests and programmatic checks for most coding work;

- rubric-based LLM judges for documents and tool use;

- reference answers for STEM and vision question answering;

- pairwise reward models comparing screenshots of rendered web apps.

RL training task mix
| Category | Rollouts | Tokens |
| -------- | -------- | ------ |
| 38.5%    | 65.0%    |        |
| 9.1%     | 13.3%    |        |
| 7.0%     | 7.6%     |        |
| 4.7%     | 7.2%     |        |
| 13.9%    | 3.0%     |        |
| 17.1%    | 2.3%     |        |
| 2.0%     | 1.4%     |        |
| 7.8%     | 0.2%     |        |

We randomly sampled and conditioned on an effort level, and applied a length penalty in the reward function, which trained the model to trade off speed and thoroughness.

Our asynchronous RL uses a score centering variant—the StableReinforce objective in Tinker—which tolerates significant staleness. Our run combines short and long horizon tasks with p50/p99 total rollout tokens being 43k and 1.5M respectively and p50/p99 duration of 8 minutes and 4 hours respectively. We use 256k context length and enable compaction training for long-horizon tasks. As a result, both training reward and evaluation performance grow log-linearly with compute.

Training reward

Training reward versus cumulative learner and sampler FLOPs, on a logarithmic axis.

STEM evaluation performance

STEM evaluation performance versus cumulative learner and sampler FLOPs, on a logarithmic axis.

Vision evaluation performance

Vision evaluation performance versus cumulative learner and sampler FLOPs, on a logarithmic axis.

Stable training
| Metric                                               | Value |
| ---------------------------------------------------- | ----- |
| 17 updates                                           |       |
| 16K                                                  |       |
| Median: 43K · p99: 1.5M                              |       |
| Median: 8 minutes · p99: 4 hours                     |       |
| 256K tokens, with compaction for longer trajectories |       |

Making this scale practical required improvements throughout the systems stack. Our RL infrastructure deploys NVFP4 MoE training and sampling. During sampling, prefill, draft, and verify run fully on CUDA graphs. Trainers mirror the pretrain stack with high expert parallelism, EPLB, and CUDA graphs to improve GPU efficiency. To support long horizon agentic workloads, we use sticky routing and DRAM KV-cache offloading for maintaining 93%+ prefix cache hit rate, allowing us to maintain a 5:3 sampler:trainer ratio despite training at long contexts. Weight transfer is done via GPUDirectRDMA at 93%+ line rate with a parallelism-invariant checksum verifier. We also further lower deployment inference cost by offering a variant of NVFP4 KV cache, which we find to minimally lower evals.

Long-horizon tasks give the model hours of autonomy, increasing potential risks from misaligned behavior. Midway through the main RL run, our asynchronous monitor flagged an instance where the model, failing at a task, worked around a network block through a third-party service it could reach using its search tools. We paused the run, hardened our infrastructure, removed environments that required web access, and added synchronous tool-call monitoring. We then resumed the run, incorporating additional training phases specifically for alignment, as described above.

Improvements to our process allow us to train models at different sizes using similar recipes with predictable scaling and performance. Inkling 2 Small is part of a ladder of models; we look forward to sharing more rungs with you soon.

## Benchmarking Inkling 2 Small

Like previous Inkling releases, we benchmarked Inkling-Small on a broad range of capabilities. All evals run at effort 0.99 (xhigh) and temperature 1.0. All evals run with a 1M-token maximum trajectory limit, increased from the 256K-token limit used for Inkling Small.

| Open Weights Flash                                     | Open Weights Pro                                       | Closed Weights Flash                             |                                                   |                                                   |                                                   |                                                   |                                                   |            |                                                        |                                                        |                                                        |                                                        |
| ------------------------------------------------------ | ------------------------------------------------------ | ------------------------------------------------ | ------------------------------------------------- | ------------------------------------------------- | ------------------------------------------------- | ------------------------------------------------- | ------------------------------------------------- | ---------- | ------------------------------------------------------ | ------------------------------------------------------ | ------------------------------------------------------ | ------------------------------------------------------ |
| 17/594                                                 | 18/320                                                 | 15/309                                           | 23/428                                            | 16/552                                            | 42/816                                            | 40/753                                            | 42/1020                                           | 49/1050    | 104/2800                                               | –                                                      | –                                                      | –                                                      |
| 30.6                                                   | 27.8                                                   | 26.7                                             | 29.1                                              | 32.6 (Second-highest reported score in this row)  | 24.9                                              | 31.7                                              | 29.9                                              | 26.1       | –                                                      | 27.9                                                   | 30.8                                                   | 36.5 (Highest reported score in this row)              |
| 56.1%                                                  | 50.9%                                                  | 54.8%                                            | 53.8%                                             | 61.3% (Highest reported score in this row)        | 48.6%                                             | 59.7% (Second-highest reported score in this row) | 57.8%                                             | 56.5%      | 53.0%                                                  | 52.3%                                                  | 56.0%                                                  | 58.7%                                                  |
| 25.4%                                                  | 14.2%                                                  | 21.6%                                            | 20.8%                                             | 28.5% (Second-highest reported score in this row) | 7.6%                                              | 21.6%                                             | 24.0%                                             | 15.0%      | 17.3%                                                  | 25.2%                                                  | 18.7%                                                  | 34.9% (Highest reported score in this row)             |
| 72.1%                                                  | 63.4%                                                  | 67.9%                                            | 73.7%                                             | 77.0% (Highest reported score in this row)        | 60.0%                                             | 69.0%                                             | 71.9%                                             | 61.7%      | 68.5%                                                  | 66.6%                                                  | 73.8% (Second-highest reported score in this row)      | 59.9%                                                  |
| 5.7%                                                   | 4.8%                                                   | 4.3%                                             | 3.3%                                              | 12.9% (Second-highest reported score in this row) | 1.9%                                              | 9.5%                                              | 5.7%                                              | 4.3%       | 8.1%                                                   | 8.6%                                                   | 10.0%                                                  | 20.0% (Highest reported score in this row)             |
| 81.6%                                                  | 30.8%                                                  | 79.0%                                            | 73.9%                                             | 87.7% (Second-highest reported score in this row) | 76.2%                                             | 78.1%                                             | 85.2%                                             | 81.6%      | 85.0%                                                  | 81.7%                                                  | 78.7%                                                  | 90.4% (Highest reported score in this row)             |
| 86.3% (Second-highest reported score in this row)      | –                                                      | –                                                | 84.3%                                             | –                                                 | 82.4%                                             | 84.3%                                             | –                                                 | –          | 88.2% (Highest reported score in this row)             | –                                                      | 58.8%                                                  | 52.9%                                                  |
| 31.9%                                                  | 36.8% (Highest reported score in this row)             | –                                                | –                                                 | 30.5%                                             | –                                                 | 34.5% (Second-highest reported score in this row) | –                                                 | –          | 31.5%                                                  | –                                                      | –                                                      | –                                                      |
| 494.27 (Second-highest reported score in this row)     | –                                                      | –                                                | –                                                 | 497.36 (Highest reported score in this row)       | –                                                 | 430.96                                            | –                                                 | 478.39     | 453.58                                                 | –                                                      | –                                                      | –                                                      |
| 70.0%                                                  | –                                                      | 69.3%                                            | –                                                 | –                                                 | –                                                 | –                                                 | –                                                 | –          | 72.6% (Highest reported score in this row)             | 61.5%                                                  | 62.2%                                                  | 70.4% (Second-highest reported score in this row)      |
| 85.4/85.0% (Second-highest reported score in this row) | –/82.0%                                                | –/62.9%                                          | –/80.4%                                           | –/83.2%                                           | 89.2/87.5% (Highest reported score in this row)   | 84.2/83.6%                                        | –/84.4%                                           | –/84.5%    | 82.3/84.2%                                             | –/77.8%                                                | –/87.5%                                                | –/81.0%                                                |
| 1527                                                   | 1451                                                   | 1487                                             | 1476                                              | 1570 (Second-highest reported score in this row)  | 1279                                              | 1503                                              | 1512                                              | 1348       | 1497                                                   | 1337                                                   | 1203                                                   | 1574 (Highest reported score in this row)              |
| 1630                                                   | 1649                                                   | 1614                                             | 1465                                              | 1746 (Highest reported score in this row)         | 1401                                              | 1657                                              | 1689 (Second-highest reported score in this row)  | –          | 1543                                                   | 1444                                                   | 1445                                                   | 1631                                                   |
| 70.5% (Highest reported score in this row)             | 60.7%                                                  | 61.2%                                            | 67.5% (Second-highest reported score in this row) | –                                                 | 61.8%                                             | 61.4%                                             | 62.0%                                             | 55.2%      | 54.3%                                                  | 49.0%                                                  | –                                                      | 39.2%                                                  |
| 46.8% (Highest reported score in this row)             | 33.2%                                                  | –                                                | –                                                 | –                                                 | –                                                 | 36.4%                                             | –                                                 | –          | 45.3% (Second-highest reported score in this row)      | –                                                      | –                                                      | –                                                      |
| 71.8% (Highest reported score in this row)             | 46.7%                                                  | –                                                | –                                                 | –                                                 | –                                                 | 51.8%                                             | –                                                 | –          | 43.1%                                                  | –                                                      | 58.5% (Second-highest reported score in this row)      | –                                                      |
| 40.4% (Second-highest reported score in this row)      | 47.2% (Highest reported score in this row)             | –                                                | –                                                 | –                                                 | –                                                 | –                                                 | –                                                 | –          | 33.4%                                                  | –                                                      | –                                                      | –                                                      |
| 67.5% (Highest reported score in this row)             | 60.6%                                                  | 63.3%                                            | 58.8%                                             | 62.9%                                             | 60.7%                                             | 65.0%                                             | 64.3%                                             | 61.7%      | 60.0%                                                  | 56.8%                                                  | 65.8% (Second-highest reported score in this row)      | 65.4%                                                  |
| 69.5%                                                  | 66.3%                                                  | 68.9%                                            | 67.8%                                             | 69.2%                                             | 62.0%                                             | 69.1%                                             | 67.4%                                             | 68.7%      | 71.3% (Second-highest reported score in this row)      | 70.0%                                                  | 72.9% (Highest reported score in this row)             | 65.5%                                                  |
| 65.8%                                                  | 62.0%                                                  | 59.9%                                            | 62.7%                                             | 74.9% (Highest reported score in this row)        | 52.1%                                             | 73.1% (Second-highest reported score in this row) | 64.9%                                             | 67.5%      | 68.7%                                                  | 58.9%                                                  | 66.8%                                                  | 62.2%                                                  |
| 14.3% (Highest reported score in this row)             | 7.3%                                                   | –                                                | –                                                 | 12.5% (Second-highest reported score in this row) | –                                                 | 11.0%                                             | –                                                 | 4.8%       | 5.5%                                                   | 6.8%                                                   | 4.5%                                                   | –                                                      |
| 14.5%                                                  | 12.3%                                                  | –                                                | 13.5%                                             | 12.5%                                             | 13.8%                                             | 13.9%                                             | –                                                 | 12.0%      | 9.5%                                                   | 10.8%                                                  | 15.5% (Highest reported score in this row)             | 15.3% (Second-highest reported score in this row)      |
| 62.0% (Second-highest reported score in this row)      | 53.7%                                                  | –                                                | 60.0%                                             | 59.3%                                             | 58.0%                                             | 59.5%                                             | –                                                 | 56.5%      | 56.7%                                                  | 53.9%                                                  | 58.8%                                                  | 62.2% (Highest reported score in this row)             |
| 21.7% (Second-highest reported score in this row)      | 10.4%                                                  | 13.3%                                            | 7.5%                                              | 14.2%                                             | 25.0% (Highest reported score in this row)        | 14.6%                                             | 13.8%                                             | 15.8%      | 12.9%                                                  | 4.2%                                                   | 12.1%                                                  | 4.2%                                                   |
| 46.6%                                                  | 45.2%                                                  | 38.0%                                            | 34.1%                                             | 45.2%                                             | 38.5%                                             | 49.0% (Highest reported score in this row)        | 47.1% (Second-highest reported score in this row) | 35.6%      | 46.2%                                                  | 30.3%                                                  | 38.9%                                                  | 43.8%                                                  |
| 1296 (Highest reported score in this row)              | –                                                      | –                                                | –                                                 | –                                                 | –                                                 | –                                                 | 1275                                              | –          | 1283 (Second-highest reported score in this row)       | 1161                                                   | 1221                                                   | –                                                      |
| 1340 (Highest reported score in this row)              | 1270                                                   | –                                                | 1280                                              | 1297                                              | 1303                                              | 1290                                              | 1307                                              | 1256       | 1334 (Second-highest reported score in this row)       | 1268                                                   | 1299                                                   | 1261                                                   |
| 1428 (Highest reported score in this row)              | 1276                                                   | –                                                | 1312                                              | 1261                                              | 1270                                              | 1303                                              | 1315                                              | 1225       | 1351 (Second-highest reported score in this row)       | 1261                                                   | 1305                                                   | 1319                                                   |
| 1401 (Highest reported score in this row)              | 1314                                                   | –                                                | 1332                                              | 1361                                              | 1261                                              | 1324                                              | 1333                                              | 1249       | 1381 (Second-highest reported score in this row)       | 1213                                                   | 1291                                                   | 1334                                                   |
| 1020                                                   | 975                                                    | 1052 (Second-highest reported score in this row) | –                                                 | –                                                 | –                                                 | 1034                                              | 1052 (Second-highest reported score in this row)  | –          | 1041                                                   | 1030                                                   | 899                                                    | 1192 (Highest reported score in this row)              |
| 64.7% (Highest reported score in this row)             | –                                                      | –                                                | –                                                 | 47.7%                                             | –                                                 | –                                                 | –                                                 | –          | 58.5% (Second-highest reported score in this row)      | 56.2%                                                  | –                                                      | –                                                      |
| 83.8/91.5% (Highest reported score in this row)        | –                                                      | –                                                | –                                                 | –                                                 | –                                                 | –                                                 | 56.6%/–                                           | 42.3%/–    | 73.8/83.2%                                             | 81.1/87.6% (Second-highest reported score in this row) | –                                                      | –                                                      |
| 33.3% (Second-highest reported score in this row)      | 26.8%                                                  | 25.4%                                            | –                                                 | 30.1%                                             | 29.6%                                             | –                                                 | 25.2%                                             | –          | 25.5%                                                  | 21.0%                                                  | 41.6% (Highest reported score in this row)             | –                                                      |
| 68.6/93.7% (Highest reported score in this row)        | 53.4/82.6% (Second-highest reported score in this row) | –                                                | –/91.0%                                           | –/92.5%                                           | 44.3/80.2%                                        | –                                                 | –                                                 | 42.3/71.5% | 53.4/85.7% (Second-highest reported score in this row) | –                                                      | –                                                      | –                                                      |
| 25.2% (Highest reported score in this row)             | 15.4%                                                  | 9.0%                                             | 20.8%                                             | 17.2%                                             | 18.6%                                             | 11.2%                                             | 19.2%                                             | 17.6%      | 22.0%                                                  | 22.8% (Second-highest reported score in this row)      | 21.0%                                                  | 20.8%                                                  |
| 81.6% (Second-highest reported score in this row)      | 79.0%                                                  | –                                                | 79.1%                                             | 80.6%                                             | 80.9%                                             | –                                                 | 74.1%                                             | 78.0%      | 80.5%                                                  | 79.7%                                                  | 85.6% (Highest reported score in this row)             | 79.1%                                                  |
| 85.6/87.7% (Second-highest reported score in this row) | 80.2/89.4%                                             | 66.2%/–                                          | 82.5%/–                                           | 77.0/85.6%                                        | 85.4/88.7%                                        | –                                                 | 74.2%/–                                           | 83.2/87.9% | 84.8/91.3%                                             | 81.8/85.3%                                             | 86.2/92.1% (Highest reported score in this row)        | 84.4%/–                                                |
| 36.7/80.4%                                             | 16.3/78.0%                                             | 7.5%/–                                           | 12.6%/–                                           | 17.9/83.0%                                        | 35.7/63.2%                                        | –                                                 | 10.8%/–                                           | 21.2/56.6% | 26.6/68.1%                                             | 32.0%/–                                                | 40.9/82.0% (Second-highest reported score in this row) | 46.4%/– (Highest reported score in this row)           |
| 55.7/35.0%                                             | –/24.5%                                                | –/29.5%                                          | 51.4/32.5%                                        | 50.6/25.0%                                        | 52.6/29.5%                                        | 57.4/33.5%                                        | 56.9/34.0%                                        | 50.3/27.0% | –/40.5%                                                | 65.1/42.5% (Highest reported score in this row)        | 65.1/46.5% (Highest reported score in this row)        | 64.0/43.5% (Second-highest reported score in this row) |
| 28.7%                                                  | 18.2%                                                  | 13.7%                                            | 28.1%                                             | 25.3%                                             | 26.3%                                             | 17.9%                                             | 15.2%                                             | 17.3%      | 26.9%                                                  | 22.2%                                                  | 33.1% (Highest reported score in this row)             | 29.6% (Second-highest reported score in this row)      |
| 63.5% (Second-highest reported score in this row)      | –                                                      | –                                                | –                                                 | –                                                 | 32.3%                                             | –                                                 | –                                                 | –          | 65.6% (Highest reported score in this row)             | –                                                      | –                                                      | –                                                      |
| 83.3%                                                  | 80.0%                                                  | –                                                | –                                                 | –                                                 | 78.3%                                             | 79.7%                                             | 86.3% (Second-highest reported score in this row) | –          | 88.7% (Highest reported score in this row)             | –                                                      | 81.3%                                                  | 82.7%                                                  |
| 70.6% (Second-highest reported score in this row)      | 68.2%                                                  | 60.7%                                            | 60.9%                                             | 69.0%                                             | 68.7%                                             | 68.4%                                             | 68.0%                                             | 65.7%      | 69.0%                                                  | 65.0%                                                  | 73.7% (Highest reported score in this row)             | 65.3%                                                  |
| 16.1                                                   | 0.7                                                    | -10.8                                            | -6.4                                              | 0.9                                               | 25.8 (Highest reported score in this row)         | 6.0                                               | -4.5                                              | 5.1        | –                                                      | -8.3                                                   | 23.3 (Second-highest reported score in this row)       | 12.3                                                   |
| 73.3% (Highest reported score in this row)             | –                                                      | –                                                | 6.3%                                              | 35.9%                                             | 72.0% (Second-highest reported score in this row) | 61.1%                                             | 39.8%                                             | 71.8%      | –                                                      | 20.7%                                                  | 44.9%                                                  | 65.8%                                                  |
| 44.5%                                                  | 27.6%                                                  | 22.1%                                            | 23.2%                                             | 50.4% (Highest reported score in this row)        | 41.3%                                             | 30.5%                                             | 32.3%                                             | 36.5%      | 37.9%                                                  | 27.3%                                                  | 48.4% (Second-highest reported score in this row)      | 36.8%                                                  |
| 82.0% (Second-highest reported score in this row)      | 72.7%                                                  | –                                                | –                                                 | –                                                 | 87.7% (Highest reported score in this row)        | 72.5%                                             | –                                                 | 77.0%      | 73.5%                                                  | 66.7%                                                  | 70.7%                                                  | 71.7%                                                  |
| 83.8% (Second-highest reported score in this row)      | 73.3%                                                  | –                                                | –                                                 | 72.8%                                             | 92.4% (Highest reported score in this row)        | 71.8%                                             | –                                                 | 76.0%      | 72.8%                                                  | –                                                      | –                                                      | –                                                      |
| 79.7%                                                  | 96.4% (Second-highest reported score in this row)      | –                                                | –                                                 | 93.1%                                             | 84.0%                                             | 96.8% (Highest reported score in this row)        | –                                                 | 91.8%      | 96.0%                                                  | –                                                      | –                                                      | –                                                      |
| 97.1%                                                  | 97.8%                                                  | 97.8%                                            | –                                                 | 99.4% (Highest reported score in this row)        | 97.1%                                             | 97.5%                                             | –                                                 | 99.0%      | 99.0% (Second-highest reported score in this row)      | –                                                      | –                                                      | –                                                      |
| 72.7%                                                  | 67.9%                                                  | 75.4%                                            | –                                                 | –                                                 | –                                                 | 72.1%                                             | 72.9%                                             | –          | 75.2%                                                  | 76.3% (Highest reported score in this row)             | 43.8%                                                  | 75.5% (Second-highest reported score in this row)      |
| 79.6% (Second-highest reported score in this row)      | 74.8%                                                  | –                                                | –                                                 | 80.4% (Highest reported score in this row)        | 77.8%                                             | 69.6%                                             | –                                                 | 76.1%      | –                                                      | –                                                      | –                                                      | –                                                      |

Highest reported score in each row; ties included Second-highest reported score in each row; ties included

**Evaluation snapshot:** Missing results are shown as dashes.

## Limitations

While Inkling 2 Small improves on usability vs Inkling, it is not the most succinct communicator, and doesn’t always choose the best tools for the job—especially in codebases with a large number of skills.

Like previous Inkling models, Inkling 2 Small natively supports image input; however, this model does not support audio.

Inkling 2 Small is also less optimized for the chat experience. We notice issues such as worse organization and presentation of content, overuses of slashes, and overreaction to mental health and other safety related queries.

## Try Inkling 2 Small today

We are proud to make Inkling 2 Small available on Tinker for inference and fine tuning. Following the completion of safety testing, the model will be available on additional platforms in the coming weeks.

Inkling 2 Small was made in service of [our mission](https://thinkingmachines.ai/blog/the-future-worth-building-is-human/) to build AI that extends human will and judgment. We hope that it encourages your ambitions, and we look forward to seeing what you build with it.
