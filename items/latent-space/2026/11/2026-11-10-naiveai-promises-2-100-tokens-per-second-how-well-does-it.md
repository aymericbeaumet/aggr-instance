---
title: NaiveAI Promises 2,100 Tokens per Second. How Well Does It Code?
link: https://latent.space/p/naiveai/
source: latent-space
published: 2026-11-10T18:56:32Z
updated: 2026-11-10T18:56:32Z
first_seen: 2026-11-10T19:38:28.320395710Z
authors:
- Gabriele Beltramo
summary: NaiveAI could potentially top Artificial Analysis’s speed rankings. Our hands-on test suggests its coding quality holds up against DeepSeek.
content: extracted
html: 2026-11-10-naiveai-promises-2-100-tokens-per-second-how-well-does-it.html
remote_preview:
  url: https://beehiiv-images-production.s3.amazonaws.com/uploads/asset/file/f3b4fd9f-a330-4090-a922-a912d25dcd7c/naiveai-feature2.png?t=1793721927
  alt: NaiveAI Promises 2,100 Tokens per Second. How Well Does It Code?
extra:
  thumbnail: https://beehiiv-images-production.s3.amazonaws.com/uploads/asset/file/f3b4fd9f-a330-4090-a922-a912d25dcd7c/naiveai-feature2.png?t=1793721927
---

NaiveAI could potentially top Artificial Analysis’s speed rankings. Our hands-on test suggests its coding quality holds up against DeepSeek.

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/f3b4fd9f-a330-4090-a922-a912d25dcd7c/naiveai-feature2.png?t=1793721796)

***Editor’s note:*** *With this post, we are launching a new series focused on trying new models and products with unbiased reviews. Reach out with suggestions to tips@latent.space.*

[**Naive-N0.5-Flash**](https://github.com/NaiveAI-Labs/Naive-N0.5-Flash?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) is a new **open-weights model** built for **AI coding tasks** and tuned to generate **as many tokens per second as possible**. It was developed by a Chinese company called [NaiveAI](https://naive.ai/en/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv), with the aim of speeding up engineering and research workflows so that teams can experiment, iterate and solve complex problems faster.

The model is an evolution of [**Xiaomi’s MiMo-V2.5**](https://mimo.xiaomi.com/mimo-v2-5/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv), which NaiveAI has trained further for long-context reasoning and paired with its own [NaiveRT](https://github.com/NaiveAI-Labs/NaiveRT?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) inference engine. Naive-N0.5-Flash arrives with a bold pitch: **a model built largely by AI and trained to do AI research itself, with recursive self-improvement as the long-term goal.**

To see what changed, it helps to know where it started. [**MiMo-V2.5**](https://huggingface.co/XiaomiMiMo/MiMo-V2.5?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) is a 310B-parameter sparse Mixture-of-Experts model with 15B active parameters, trained on 48T tokens. Its language backbone inherits the hybrid design of MiMo-V2-Flash, which mixes sliding-window attention (SWA) with global self-attention.

NaiveAI's recipe starts from those weights. Through continued, multi-stage training, the team adapted the checkpoint to DeepSeek Sparse Attention layers. According to the [NaiveAI technical blog](https://naive.ai/en/research/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv), **this took roughly 3.25T additional tokens on top of MiMo's original 48T.** About 3T of those additional tokens were trained at a fixed 1M context length and with a constant learning rate.

## Benchmarking NaiveAI vs. DeepSeek and Z.ai

We benchmarked **the ultrafast mode of Naive-N0.5-Flash** against [**DeepSeek V4.1 Flash 1017**](https://huggingface.co/deepseek-ai/DeepSeek-V4.1-Flash-1017?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) and [**GLM 5.3 Flash**](https://huggingface.co/zai-org/GLM-5.3-Flash?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv), and then gave all three the same app-building prompts.

The trade-off, we discovered, is that **Naive-N0.5-Flash takes longer to reason through harder problems, yet it still delivers answers faster overall.** However, that comes at a slightly higher cost per question — although that may be an acceptable price for applications where speed comes first.

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/ef9d0c26-8f60-437d-9701-ee19566d165d/image.png?t=1793716278)

Most popular models on OpenRouter as of the end of October 2026.

[DeepSeek V4.1 Flash 1017](https://huggingface.co/deepseek-ai/DeepSeek-V4.1-Flash-1017?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) and [GLM 5.3 Flash](https://huggingface.co/zai-org/GLM-5.3-Flash?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) are two of the most popular models in [OpenRouter](https://openrouter.ai/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv). Both belong to the **Flash model class** and demonstrate excellent capabilities in **AI-assisted coding**.

As of this writing, performance statistics for NaiveAI models are not yet available on [Artificial Analysis](https://artificialanalysis.ai/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv). Therefore, we report the Intelligence Index scores of the models used in our comparison:

### Model Intelligence Index

- DeepSeek V4.1 Flash 1017 (Max): 45

- GLM-5.3-Flash: 43

- Naive-N0.5-Flash: not yet available

Published benchmark results offer some insight into the new model's capabilities. They place its coding ability between the two comparison models, and ahead of both on [NL2Repo-Bench](https://arxiv.org/abs/2512.12730?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv).

|                          |                   |                  |                             |
| ------------------------ | ----------------- | ---------------- | --------------------------- |
| **Model**                | **NL2Repo-Bench** | **DeepSWE v1.1** | **Agent's Last Exam (ALE)** |
| GLM-5.3-Flash            | 56.3              | 63.4             | 26.3                        |
| DeepSeek V4.1 Flash 1017 | 68.2              | **77.0**         | **35.4**                    |
| Naive-N0.5-Flash         | **71.9**          | 67.8             | 32.4                        |

Our own evaluations below look at the differences between these models in more detail.

## Speed and verbosity experiments

**Speed is the headline feature.**[^1] NaiveAI promises faster research iteration than comparable models, which matters to developers and AI researchers who need to test ideas quickly.[^2]

We evaluated each model on 10 randomly sampled, text-only questions at each of three difficulty levels: 10 easy questions from [MATH-500](https://huggingface.co/datasets/HuggingFaceH4/MATH-500?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv), 10 medium questions from [GPQA-Diamond](https://huggingface.co/datasets/Idavidrein/gpqa?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) and 10 hard questions from [HLE-Diamond](https://huggingface.co/datasets/cais/hle-diamond?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv). That makes 30 questions per model. All experiments used the same maximum completion-token limit of 256K.

For each request, we recorded response speed (tokens per second, TPS), the total number of completion tokens, and end-to-end response time. The code for the experiments is at [https://github.com/gbeltramo/llm-verbosity](https://github.com/gbeltramo/llm-verbosity?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv).

**The tables below report the mean and median across all 30 successful requests for each configuration.** Throughput figures come from different sources: OpenRouter for DeepSeek V4.1 Flash 1017 and GLM 5.3 Flash, and server-side measurements for Naive-N0.5-Flash. They should be compared with that in mind.

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/551a71b8-fce5-4259-b40e-fb938cdfc13e/image.png?t=1793716278)

Graphic by Gabriele Beltramo

### Key observations

**Naive-N0.5-Flash reports exceptionally high throughput**, approaching or passing 1,000 tokens per second in both thinking configurations. Because its figures are server-side and the other models' figures come from OpenRouter, we do not claim an exact speed multiple over DeepSeek V4.1 Flash 1017 or GLM 5.3 Flash.

**Thinking has a large effect on verbosity.** On the hard HLE questions, Naive-N0.5-Flash with thinking on averaged about 108,500 completion tokens, roughly nine times the GLM 5.3 Flash (high) average and 11 times the DeepSeek V4.1 Flash 1017 (high) average.

**The medians tell a different story from the means.** Across all 30 questions, the median completion for Naive-N0.5-Flash with thinking on was about 3,200 tokens, far below its average of more than 51,000. A small number of requests produced exceptionally long reasoning traces and pulled the average up. The same pattern appears, less sharply, in the other models.

The tables also include **end-to-end response time, which is the fairer basis for judging speed**. The median question took 3.1 seconds with Naive-N0.5-Flash (thinking), against 5.3 seconds for GLM 5.3 Flash (high) and 10.1 seconds for DeepSeek V4.1 Flash 1017 (high). The means are closer: 45.3 seconds for Naive-N0.5-Flash, 45.0 for DeepSeek V4.1 Flash 1017, and 86.9 for GLM 5.3 Flash. **On the hard questions, Naive-N0.5-Flash (thinking) was slower on average than DeepSeek V4.1 Flash 1017 (high)**, at 89.8 seconds against 79.7.

Two caveats apply. The sample is small, at 10 questions per difficulty level, and we did not grade answer accuracy. Short, fast replies are only useful if they are correct.

## App experiments

We also asked the models to build the same web apps from the same initial prompt. We tested six combinations of model and thinking effort, with the thinking effort in parentheses:

- Naive-N0.5-Flash (no thinking)

- Naive-N0.5-Flash (thinking)

- DeepSeek V4.1 Flash 1017 (low)

- DeepSeek V4.1 Flash 1017 (high)

- GLM 5.3 Flash (low)

- GLM 5.3 Flash (high)

The [gallery of apps](https://gbeltramo.github.io/naive-apps/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) is available online. In summary, the three prompts asked the six model variants to do the following:

1. Create an interactive 3D view of the Earth and the Solar System.

2. Grow a 3D tree from a seeded random generator, with branches that taper and fork, thousands of leaves, and bark and leaf textures.

3. Do a browser port of the book *Ray Tracing in One Weekend*.

Below we’ve embedded results for the **thinking variants of each model**.

### Naive-N0.5-Flash (thinking)

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/f5ed1324-ac2f-4c8c-98f6-51cbe444fff8/image12.gif?t=1793717566)

Earth and the Solar System; Naive-N0.5-Flash (thinking)

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/9ad0f09a-db6e-4a41-bd5d-2d6a4a87d13f/image9.gif?t=1793720902)

3D tree; Naive-N0.5-Flash (thinking)

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/b27b4c8d-ae10-4b8c-a3c1-ee4b9097a2f2/image3.gif?t=1793720955)

*Ray Tracing in One Weekend*; Naive-N0.5-Flash (thinking)

### DeepSeek V4.1 Flash 1017 (high)

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/0bdd8e5e-4a91-4829-969a-c67210747a8c/image8.gif?t=1793720992)

Earth and the Solar System; DeepSeek V4.1 Flash 1017 (high)

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/a6e7a2f8-0361-4a34-8847-b886494e5873/image5.gif?t=1793721024)

3D tree; DeepSeek V4.1 Flash 1017 (high)

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/30ac0933-1ae9-4738-b879-0f533363f66f/image10.gif?t=1793721070)

*Ray Tracing in One Weekend*; DeepSeek V4.1 Flash 1017 (high)

### GLM 5.3 Flash (high)

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/69c44ae2-4727-4363-9f06-57f54183c2c7/image11.gif?t=1793721097)

Earth and the Solar System; GLM 5.3 Flash (high)

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/0376ab96-86d2-47fd-9f8d-578da0505027/image7.gif?t=1793721127)

3D tree; GLM 5.3 Flash (high)

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/0a9a708a-43eb-4a36-9ad9-458eb414f42f/image13.gif?t=1793721151)

*Ray Tracing in One Weekend*; GLM 5.3 Flash (high)

### Solar system

The first app is an interactive 3D view of the Earth and the Solar System. Viewers can orbit the camera around the scene and watch the planets circle the sun, with controls to speed up or slow down their motion. Every surface is drawn from generated textures, with no image files.

[Naive-N0.5-Flash (thinking)](https://gbeltramo.github.io/naive-apps/solar_system/naive_05_flash_thinking/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) **produced the best result of the three.** Its sun shows sunspots, and the dark side of its Earth glows with city lights at night.

[DeepSeek V4.1 Flash 1017 (high)](https://gbeltramo.github.io/naive-apps/solar_system/deepseek_v4_1_flash_thinking_high/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) came close behind. Its version lacks an asteroid belt, and its Earth has less textural detail.

[GLM 5.3 Flash (high)](https://gbeltramo.github.io/naive-apps/solar_system/glm_5_3_flash_thinking_high/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) stumbled on lighting, which makes the planets hard to tell apart. Its speed controls are also too sensitive, so a small nudge sends the planets racing or crawling.

### Tree modeling

The second app grows a 3D tree from a seeded random generator, with branches that taper and fork, thousands of leaves, and bark and leaf textures. Wind makes the branches sway and the leaves flutter, and the shadows on the grassy ground move with them. The tree stands under a sky gradient and light fog. A control panel adjusts the seed, branching depth, leaf count, wind strength and direction, and sun position. There’s also a switch to autumn colors and a frame-rate counter sits in the corner.

The good results still had small glitches in the wind animation. In the DeepSeek V4.1 Flash 1017 (high) app, thin branches can detach from their parents in strong wind and appear to float. The effect is subtle, but it is there.

The problem is easier to see in the [GLM 5.3 Flash (high)](https://gbeltramo.github.io/naive-apps/tree_model/glm_5_3_flash_thinking_high/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) app. When the wind intensity is turned up, the whole tree starts to deform and wobble like a jelly sculpture in an earthquake.

### Ray Tracing in One Weekend

The third app is a browser port of the book [Ray Tracing in One Weekend](https://raytracing.github.io/books/RayTracingInOneWeekend.html?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv). It renders the book's cover scene: a vast gray ground, a scatter of small matte, metallic and glass spheres, and three large spheres in the center. One is glass, one a brown matte and one a polished metal. Depth of field blurs the foreground and background. The image builds up progressively on screen, with a progress bar, elapsed time and a rays-per-second estimate. A menu offers the book's earlier milestone scenes. The user can pick a quality preset and download the result as a PNG.

In this more coding-focused scenario, **all three models with thinking on produced a working app.**

[DeepSeek V4.1 Flash 1017 (high)](https://gbeltramo.github.io/naive-apps/ray_tracing_one_weekend/deepseek_v4_1_flash_thinking_high/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) **wins** because its ray tracer is the most efficient of the three. It also uses the full width of the screen to display the rendered image.

[GLM 5.3 Flash (high)](https://gbeltramo.github.io/naive-apps/ray_tracing_one_weekend/glm_5_3_flash_thinking_high/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) is again the weakest. Its speed is similar to the Naive-N0.5-Flash app, but its interface is more basic.

## Pricing

Naive positions N0.5-Flash as a capable, low-cost model. The [target prices](https://github.com/NaiveAI-Labs/Naive-N0.5-Flash/blob/3551de41a35f1470bc37831f11388a821c99162f/README.md?plain=1&utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv#L24) are **$0.10 per million input tokens, $0.40 per million output tokens, and $0.01 per million cache-read tokens**.

That puts it in the $0.20 to $0.60 per million output tokens band occupied by several capable Flash models, including the two we compared it against.

The final bill will also depend on how many tokens the model uses. That in turn depends on the thinking budget requested and the difficulty of the questions.

## Takeaways

In ultrafast mode, Naive-N0.5-Flash can peak at around 2,000 tokens per second and sustain nearly 1,000 during thinking-heavy tasks.

In our tests, **its median response time was several times shorter than those of GLM 5.3 Flash and DeepSeek V4.1 Flash 1017 at their high thinking settings.** Mean times were similar to DeepSeek V4.1 Flash 1017.

On complex tasks, it spent far more thinking tokens than the other two models. On easy and medium questions, it consistently replied faster.

**In the apps, Naive-N0.5-Flash was much more consistent than GLM 5.3 Flash (high).** With thinking on, it also holds its own against DeepSeek V4.1 Flash 1017. It built the most detailed solar system app, tied with DeepSeek V4.1 Flash 1017 on the tree model, and came a close second on the efficiency of the ray tracer.

For developers who **value fast iteration over token economy**, that speed is the main reason to try Naive-N0.5-Flash.

[^1]: The speed of Naive-N0.5-Flash depends on an efficient speculative-decoding drafter and a custom inference engine. The reported decoding speeds were measured on a single eight-GPU Blackwell node running speculative decoding in [NaiveRT](https://github.com/NaiveAI-Labs/NaiveRT?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv). The drafter was [Naive-N0.5-Flash-FP8-Draft](https://huggingface.co/NaiveAI/Naive-N0.5-Flash-FP8-Draft?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv), and the setup is tuned for single-stream decode latency rather than batch throughput.

    Realized speed depends on how many draft tokens the target model accepts per round. That varies with the checkpoint pair, the prompt, context length, sampling settings, and the content being generated. The reported numbers show this clearly:

    \- **Acceptance rate:** 70.6% with thinking disabled, but only 43.4% with thinking enabled.\
    \- **Decode speed:** about 984 tokens/s in the thinking phase and 1,816 tokens/s in the answer phase.\
    \- **Peak rate:** roughly 2,100 tokens/s over a single second. Treat this as an upper bound, not a typical rate.

    The engine matters as much as the drafter. The developers report that a full speculative round (draft, verify, sampling and commit) takes **3.4 ms** in NaiveRT versus **12.3 ms** in SGLang, a 72.4% reduction. The thinking-disabled campaign measured about 3.45 ms per round. For the full measurements, see [NaiveRT: Performance](https://github.com/NaiveAI-Labs/NaiveRT?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv#performance).

[^2]: Implications for reinforcement learning: Generating autoregressive rollouts is the main bottleneck in large-scale reinforcement learning. Anything that cuts decode cost, such as the sparse attention (DSA) adopted here, makes rollouts cheaper. Adapting other models to DSA could make that loop considerably more efficient and speed up frontier research.

    This fits Naive AI's larger promise: a self-improving AI that can optimize its own inference CUDA kernels and adopt the most efficient architecture for large-scale reinforcement learning training.
