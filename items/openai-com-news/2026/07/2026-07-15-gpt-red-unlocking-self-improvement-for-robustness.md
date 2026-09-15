---
title: 'GPT-Red: Unlocking Self-Improvement for Robustness'
link: https://openai.com/index/unlocking-self-improvement-gpt-red
source: openai-com-news
published: 2026-07-15T10:00:00Z
updated: 2026-07-15T10:00:00Z
first_seen: 2026-09-07T17:03:44.343711062Z
replicated_at: 2026-09-15T01:23:10.737129797Z
labels:
- safety
summary: Explore GPT-Red, OpenAI’s automated red teaming system that uses self-play to improve AI safety, alignment, and prompt injection robustness.
content: extracted
html: 2026-07-15-gpt-red-unlocking-self-improvement-for-robustness.html
preview:
  file: 2026-07-15-gpt-red-unlocking-self-improvement-for-robustness.preview-896ff1ed448a.webp
  width: 256
  height: 144
  alt: SEO card for GPT-Red article
  color: '#0e0e0e'
images:
- source: https://images.ctfassets.net/kftzwdyauwt9/3ACfFRKDhuNzU4isOGxHRv/26f7f9bded23de64ea183a9ec19904c5/SEO_Card.png?w=1600&h=900&fit=fill
  original:
    file: 2026-07-15-gpt-red-unlocking-self-improvement-for-robustness.image-ec551ac73486.png
    width: 1600
    height: 900
  variants:
  - file: 2026-07-15-gpt-red-unlocking-self-improvement-for-robustness.image-f88ac2707117.webp
    width: 320
    height: 180
  - file: 2026-07-15-gpt-red-unlocking-self-improvement-for-robustness.image-dbab8baa512a.webp
    width: 640
    height: 360
  - file: 2026-07-15-gpt-red-unlocking-self-improvement-for-robustness.image-08b5ce7851da.webp
    width: 960
    height: 540
  - file: 2026-07-15-gpt-red-unlocking-self-improvement-for-robustness.image-06560cced2be.webp
    width: 1280
    height: 720
  - file: 2026-07-15-gpt-red-unlocking-self-improvement-for-robustness.image-8fb9807a23eb.webp
    width: 1600
    height: 900
  color: '#000000'
---

AI systems commonly encounter third-party data through browsers, connected apps, local files, and other tools. These affordances are necessary for performing real-world tasks, but they also create more opportunities for malicious actors to influence model behavior. For example, a third party might embed a [carefully crafted instruction⁠](https://openai.com/index/prompt-injections/) —designed to trick the model into uploading sensitive data to an external server—in an email, webpage, tool response, or code repository.

Human red-teaming is a critical part of our safety work, helping us uncover these vulnerabilities before deployment and put the right safeguards in place. But human red-teaming alone is difficult to scale. Designing and running these exercises is time-intensive, limiting how quickly we can identify new failure modes and incorporate them into stronger safeguards. Further, while these exercises produce valuable examples of successful attacks, they cannot generate the volume and diversity of adversarial data needed to improve model robustness through training.

Keeping pace with increasingly capable models requires red-teaming to scale as well. To this end, we have been training automated, internal-only red-teaming models that uncover vulnerabilities before deployment and generate attacks during model training to improve robustness. We believe automated red-teaming unlocks a crucial form of self-improvement for safety: using today’s models to directly help make future models safer.

**GPT‑Red** is the culmination of these efforts and our current best automated safety red-teaming model. Similar to how human red-teamers craft attacks, the model works toward a goal by sending a prompt, observing how GPT models respond to it, and iterating. We trained GPT‑Red at the compute scale of some of our largest post-training runs at OpenAI—an unprecedented amount of compute dedicated purely for improving safety.

We directly incorporate GPT‑Red into the training process of our production models. As a result, GPT‑5.6 Sol is our most robust model to prompt injections to date, achieving 6x fewer failures on our hardest direct prompt injection benchmark compared to our best production model from just four months earlier. The scalability of our approach leaves us excited for even stronger results in the future as we continue to train stronger red-teamers.

## Sample prompt-injected conversations

## Training GPT‑Red through self-play

GPT‑Red is trained using self-play reinforcement learning, where the model and a collection of diverse defender LLMs are trained simultaneously on a broad set of red-teaming scenarios. GPT‑Red is rewarded for eliciting a valid failure, such as a successful prompt injection, while the defender models are rewarded for resisting the attack and completing their original tasks. As the defenders become more robust, GPT‑Red is forced to discover stronger and more diverse attacks.

To support self-play training, we build an expansive set of realistic scenarios where prompt injections might be inserted. Each environment has a threat model that specifies what GPT‑Red can control and what counts as a successful attack. For instance, GPT‑Red might control part of a local file, a webpage banner, an email body, or output of a tool.

At the end of its training, GPT‑Red is a very strong attacker: it can break nearly all models it is pitted against, both internal and production models up to and including GPT‑5.5. After GPT‑Red completed training, we used it to generate prompt injections for the training of GPT‑5.6, resulting in the model becoming highly resistant to GPT‑Red’s attacks.

We keep GPT‑Red separate from the models we deploy. This keeps the malicious capabilities we specifically train into GPT‑Red out of the hands of adversarial actors, while instilling robustness into our production models.

## How strong is GPT‑Red?

GPT‑Red is highly effective against the population of defender models and red-teaming scenarios that it was trained on. We also evaluate whether the model is useful as a general-purpose red-teaming agent to benefit safety broadly at OpenAI. To do so, we test GPT‑Red’s effectiveness on novel safety environments and target models.

We first evaluate GPT‑Red’s ability to generalize to novel red-teaming scenarios using a replicated version of [the indirect prompt injection arena from Dziemian et al. (2025)⁠](https://arxiv.org/abs/2603.15714) . In this challenge, both human red-teamers and GPT‑Red independently proposed attacks against GPT‑5.1 on a set of pre-specified environments. These red-teaming scenarios and goals are distinct from those used to train GPT‑Red. GPT‑Red achieves significantly higher attack success rates, finding success on 84% of scenarios compared to 13% for humans.

GPT-Red attacker performance — All attacker samples aggregated: reward vs. solution length (chart data)
| MeanAttackerRewardTooltip | Series            | MeanAttackerReward | MeanSolutionLength |
| ------------------------- | ----------------- | ------------------ | ------------------ |
| 0.51                      | GPT-Red           | 0.5081             | 8899.5756          |
| 0.68                      | GPT-Red           | 0.6779             | 25456.7385         |
| 0.82                      | GPT-Red           | 0.8187             | 63625.8931         |
| 0.84                      | GPT-Red           | 0.8373             | 84141.1452         |
| 0.13                      | Human Red-Teamers | 0.1331             | 7000               |
| 0.13                      | Human Red-Teamers | 0.1331             | 100000             |

GPT‑Red excels as an automated red-teamer. GPT‑Red is able to generate successful attacks against GPT‑5.1 in substantially more scenarios than human red-teamers on the indirect prompt injection arena from Dziemian et al. (2025) using an internal mirror.

## Realistic red-teaming case studies

The ultimate test of a red-teamer is the ability to achieve targeted malicious goals against real-world agentic systems with incomplete knowledge of the system’s underlying model and harness design. Our first experiment in this setting pitted GPT‑Red against an AI-powered vending machine in the OpenAI office (similar to [Project Vend⁠](https://www.anthropic.com/research/project-vend-1) ) produced by Andon Labs. We gave GPT‑Red a description of the system and the ability to send attacks and observe tool calls from the simulated agent that closely mirrors the real world deployment. After iterating on attacks, GPT‑Red deployed its attack against the production agent, achieving all three of its malicious objectives:

- Change the price of an expensive in-stock item to the minimum allowed price of $0.50;
- Order a new $100+ item and offer it for $0.50; and
- Cancel another customer’s order.

We disclosed these vulnerabilities and new safeguards are actively being tested.

GPT‑Red is able to break a live autonomous agent known as Vendy that manages a vending machine. GPT‑Red was able to cause the production agent to change expensive items to be cheap and cancel other customer orders, by first testing in simulation and then transferring attacks to the live agent.

We also use GPT‑Red to attack a Codex CLI agent (based on GPT‑5.4 mini) on a suite of 10 held-out data-exfiltration scenarios. We compare the model to a prompted GPT‑5.5 baseline to study the impact of our training procedure on held-out red-teaming performance. GPT‑Red is both more effective, in that it can successfully get the agent to exfiltrate sensitive data in more scenarios, and is more token efficient.

Red-teaming on Data Exfiltration Tasks (chart data)
| Attacker | MaxSolved | MeanSolved | MinSolved | Tokens    |
| -------- | --------- | ---------- | --------- | --------- |
| GPT-Red  | 0         | 0          | 0         | 1000000   |
| GPT-Red  | 1         | 0.333      | 0         | 2138951   |
| GPT-Red  | 2         | 0.667      | 0         | 3323901   |
| GPT-Red  | 3         | 1          | 0         | 7893538   |
| GPT-Red  | 4         | 1.333      | 0         | 8390304   |
| GPT-Red  | 4         | 1.667      | 0         | 8408125   |
| GPT-Red  | 4         | 2          | 1         | 8412479   |
| GPT-Red  | 4         | 2.333      | 1         | 14289377  |
| GPT-Red  | 4         | 2.667      | 2         | 14823962  |
| GPT-Red  | 4         | 3          | 2         | 16337196  |
| GPT-Red  | 4         | 3.333      | 3         | 17549179  |
| GPT-Red  | 5         | 3.667      | 3         | 22146952  |
| GPT-Red  | 5         | 4          | 3         | 24154466  |
| GPT-Red  | 5         | 4.333      | 4         | 24546939  |
| GPT-Red  | 6         | 4.667      | 4         | 25820133  |
| GPT-Red  | 6         | 5          | 4         | 27450499  |
| GPT-Red  | 6         | 5.333      | 5         | 27450965  |
| GPT-Red  | 6         | 5.667      | 5         | 34759473  |
| GPT-Red  | 7         | 6          | 5         | 39929087  |
| GPT-Red  | 7         | 6.333      | 5         | 54869478  |
| GPT-Red  | 8         | 6.667      | 5         | 55337169  |
| GPT-Red  | 9         | 7          | 5         | 59651834  |
| GPT-Red  | 9         | 7.333      | 6         | 59975769  |
| GPT-Red  | 9         | 7.667      | 7         | 69866489  |
| GPT-Red  | 9         | 8          | 7         | 70332282  |
| GPT-Red  | 9         | 8.333      | 8         | 112146916 |
| GPT-5.5  | 0         | 0          | 0         | 1000000   |
| GPT-5.5  | 1         | 0.333      | 0         | 4945747   |
| GPT-5.5  | 2         | 0.667      | 0         | 6774608   |
| GPT-5.5  | 2         | 1          | 0         | 6988904   |
| GPT-5.5  | 3         | 1.333      | 0         | 9865090   |
| GPT-5.5  | 3         | 1.667      | 0         | 9879678   |
| GPT-5.5  | 4         | 2          | 0         | 10550654  |
| GPT-5.5  | 4         | 2.333      | 0         | 10853103  |
| GPT-5.5  | 5         | 2.667      | 0         | 11268171  |
| GPT-5.5  | 6         | 3          | 0         | 19604416  |
| GPT-5.5  | 6         | 3.333      | 0         | 32890424  |
| GPT-5.5  | 6         | 3.667      | 0         | 55506499  |
| GPT-5.5  | 6         | 4          | 0         | 108075388 |

GPT‑Red is more effective and efficient at breaking live codex agents. We test against a codex agent backed by GPT‑5.4 Mini on a custom suite of 10 data exfiltration tasks.

## Improving robustness with GPT‑Red

The ultimate goal of GPT‑Red is to improve the robustness of our models. Over the last six months, we’ve trained progressively stronger red-teaming models (precursors to GPT‑Red) with increasing compute, and used these models in the training of each successive production model since GPT‑5.3. Over time, each subsequent GPT release has gotten more robust.

As one example, an early version of GPT‑Red found a novel class of direct prompt injection attacks known as “Fake Chain-of-Thought” attacks. These attacks achieved success rates of upwards of 95% on GPT‑5.1 but are now below 10% for GPT‑5.6 Sol. Similarly, several of our indirect prompt injection benchmarks that target attacks in developer tools and browsing have been saturated by our latest model (>97% accuracy).

Robustness to GPT‑Red itself has also improved substantially. On a broad set of robustness environments, GPT‑Red’s attack success rates have dropped monotonically over time. With our latest model release, GPT‑5.6 Sol fails on only 0.05% of GPT‑Red’s direct prompt injections.

Robustness to the stronger attacks (chart data)
| Axis  | Model   | RateLabel | ReleaseDate | ReleaseLabel | Series                           | Rate   |
| ----- | ------- | --------- | ----------- | ------------ | -------------------------------- | ------ |
| left  | GPT-5.3 | 6.87%     | 2026-02-05  | Feb 5 2026   | Prompt Injections                | 6.8736 |
| left  | GPT-5.4 | 6.05%     | 2026-03-05  | Mar 5 2026   | Prompt Injections                | 6.0456 |
| left  | GPT-5.5 | 4.03%     | 2026-04-23  | Apr 23 2026  | Prompt Injections                | 4.0297 |
| left  | GPT-5.6 | 3.77%     | 2026-06-25  | Jun 25 2026  | Prompt Injections                | 3.7729 |
| right | GPT-5.3 | 0.293%    | 2026-02-05  | Feb 5 2026   | Instruction Hierarchy Violations | 0.2926 |
| right | GPT-5.4 | 0.274%    | 2026-03-05  | Mar 5 2026   | Instruction Hierarchy Violations | 0.2739 |
| right | GPT-5.5 | 0.093%    | 2026-04-23  | Apr 23 2026  | Instruction Hierarchy Violations | 0.0931 |
| right | GPT-5.6 | 0.051%    | 2026-06-25  | Jun 25 2026  | Instruction Hierarchy Violations | 0.0509 |

As we have continued to scale up self-play training for prompt injections, we have found new threats that can break existing models. Yet, our scaling has helped substantially improve robustness to these attacks as well. Attack success rate is computed as the average attempt success across all attempts by GPT‑Red on held-out environments.

## Robust while still being highly capable

A model can appear safer by refusing more requests or becoming less capable. A model that does less is naturally harder to attack, but that is not useful robustness.

We thoroughly evaluate both general frontier capabilities along with targeted over refusal tasks that we design. We find that all normal capabilities remain unaffected while significantly improving robustness. This suggests that the robustness gains came from better resistance to malicious instructions rather than improper tool-usage or refusing legitimate requests by default.

## Next steps

AI agents are already being used to improve the capabilities of our next-generation models. We believe with GPT‑Red that we have started to unlock a similar flywheel for safety, where today’s models can be used to make tomorrow’s models more robust, aligned, and trustworthy. We will continue to scale compute and data while making algorithmic improvements, to train future versions of GPT‑Red that are stronger than today’s model. And in turn, these models will help make future GPT releases safer.
