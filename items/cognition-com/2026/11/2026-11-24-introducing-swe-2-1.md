---
title: Introducing SWE-2.1
link: https://cognition.com/blog/swe-2-1
source: cognition-com
published: 2026-11-24T16:00:00Z
first_seen: 2026-11-25T01:32:55.949141972Z
content: extracted
html: 2026-11-24-introducing-swe-2-1.html
remote_preview:
  url: https://cognition.com/images/swe-2-1/cover.jpg
  alt: Introducing SWE-2.1
---

We’re releasing SWE-2.1, a major upgrade over SWE-2. Like its predecessor, it is trained with pure reinforcement learning on Kimi K3. It delivers large gains across a wide range of benchmarks including FrontierCode 2.0 and Terminal-Bench 4.1. We achieve much of our gains in coding performance by targeting general intelligence and long-horizon workflows. This also leads to improvements across a variety of other domains, especially cybersecurity.

## [Cybersecurity](https://cognition.com/blog/swe-2-1#cybersecurity)

One notable improvement through our training is significantly increased capability on cybersecurity, vulnerability finding and exploitation tasks. SWE-2.1 achieves 80% on ExploitBench and 27.3% flags captured via intended exploits on ExploitGym. The model thus shows strong performance not only in finding vulnerabilities, but also carrying them out through the full exploitation chain, often over several hours of autonomous work.

### [Securing training infrastructure](https://cognition.com/blog/swe-2-1#securing-training-infrastructure)

Training a model with advanced capabilities in cybersecurity requires additional safeguards. Therefore, we built several security measures into training:

- We red-teamed our training setup, particularly around sandboxes.

- We implemented continuous safety monitoring. We created a canary eval where the model tries to escape the sandbox. Upon success, our infrastructure alerts the researchers.

The canary eval has been a successful intervention. Even after extensive red-teaming, the model discovered new vulnerabilities in our sandbox four times during training. Researchers were alerted and responded immediately each time.

### [Safeguards and access for defenders](https://cognition.com/blog/swe-2-1#safeguards-and-access-for-defenders)

Because advanced capabilities, particularly in cybersecurity, carry a risk of misuse, we deployed SWE-2.1 with safeguards that detect and prevent the most dangerous kinds of behavior. These safeguards target a narrow set of high-harm activities, and are calibrated to leave most legitimate work, including work in cyber defense, unaffected.

At the same time, some advanced defensive work requires capabilities these safeguards restrict. AI is raising the floor for attackers: exploits that once required rare expertise are now within reach of anyone with a capable model. We want to give defenders the same leverage, so we are rolling out SWE-2.1 Cyber, a version of SWE-2.1 with more permissive safeguards, to Devin Security.

Devin Security already offers deep scans powered by frontier cyber models like Mythos. With SWE-2.1 Cyber powering normal scans, we hope to bring increased cyber capabilities for defense to all.

## [Test-Time Scaling](https://cognition.com/blog/swe-2-1#test-time-scaling)

As models become more capable, the ability to operate autonomously over long time horizons, for hours or even days, becomes increasingly important.

Real-world engineering and security problems rarely finish in a few minutes. To see how SWE-2.1 performs over sustained autonomous runs, we plot its cumulative solve rate on ExploitGym against normalized rollout time (computed at a fixed TPS, 1s TTFT, plus real tool execution time).

#### ExploitGymFlags captured vs Time

As the model works for longer, it is able to solve more complex tasks. On our evals, we see that performance improves log-linearly as the time horizon extends, meaning that the model continues to find exploits as it works for more hours. Furthermore, the slope of the test-time scaling curve has increased significantly compared to the base model, demonstrating a significantly increased capability to autonomously manage and complete complex tasks.

## [Data and Recipe](https://cognition.com/blog/swe-2-1#data-and-recipe)

To train SWE-2.1, we expanded our tasks and environments across both short- and long-horizon workflows, spanning areas from science and performance engineering to cybersecurity. That breadth made the model more versatile while pushing its software engineering performance even further.

We also improved our post-training algorithms to control rollout length during training. Previously, length oscillations often destabilized training dynamics. Reducing those oscillations let us train reliably on tasks ranging from a few minutes to many hours.

Together, our data and recipe improvements allowed us to train stably for much longer, with performance on held-out eval tasks improving steadily throughout many days of training. This new recipe also contributed to SWE-2.1’s improved test-time scaling, helping SWE-2.1 make better use of additional compute on difficult tasks.

## [SWE-2.1 Ultrafast](https://cognition.com/blog/swe-2-1#swe-21-ultrafast)

We are also releasing SWE-2.1 Ultrafast. Through a partnership with Cerebras, we are able to provide the same model at 10× the speed. Because SWE-2.1 continues to improve with more test-time compute, inference speed determines how much work it can finish under a tight deadline. During an active incident or urgent fix, 10× throughput turns hours of investigation and iteration into just minutes.

#### Token generation speed

0.0s 3.0s

SWE-2.1 Ultrafast

lore m ipsu m dolo r sit amet cons ecte tur adip isci ng elit sed do eius mod temp or inci didu nt ut labo re et dolo re magn a aliq ua ut enim ad mini m veni am quis nost rud exer cita tion ulla mco labo ris nisi ut aliq uip ex ea comm odo cons equa t duis aute irur e dolo r in repr ehen deri t in volu ptat e veli t esse cill um dolo re eu fugi at null a pari atur exce pteu r sint occa ecat cupi data t non proi dent sunt in culp a qui offi cia dese runt moll it anim id est labo rum lore m ipsu m dolo r sit amet cons ecte tur adip isci ng elit sed do eius mod temp or inci didu nt ut labo re et dolo re magn a aliq ua

0 tokens 600 tok/s

1,800 tokens 600 tok/s

SWE-2 Flash

lore m ipsu m dolo r sit amet cons ecte tur adip isci ng elit sed do eius mod temp or inci didu nt ut labo re et dolo re magn a aliq ua ut enim ad mini m veni am quis nost rud exer cita tion ulla mco labo ris nisi ut aliq uip ex ea comm odo cons equa t duis aute irur e dolo r in repr ehen deri t in volu ptat e veli t esse cill um dolo re eu fugi at null a pari atur exce pteu r sint occa ecat cupi data t non proi dent sunt in culp a qui offi cia dese runt moll it anim id est labo rum lore m ipsu m dolo r sit amet cons ecte tur adip isci ng elit sed do eius mod temp or inci didu nt ut labo re et dolo re magn a aliq ua

0 tokens 200 tok/s

600 tokens 200 tok/s

SWE-2.1

lore m ipsu m dolo r sit amet cons ecte tur adip isci ng elit sed do eius mod temp or inci didu nt ut labo re et dolo re magn a aliq ua ut enim ad mini m veni am quis nost rud exer cita tion ulla mco labo ris nisi ut aliq uip ex ea comm odo cons equa t duis aute irur e dolo r in repr ehen deri t in volu ptat e veli t esse cill um dolo re eu fugi at null a pari atur exce pteu r sint occa ecat cupi data t non proi dent sunt in culp a qui offi cia dese runt moll it anim id est labo rum lore m ipsu m dolo r sit amet cons ecte tur adip isci ng elit sed do eius mod temp or inci didu nt ut labo re et dolo re magn a aliq ua

0 tokens 60 tok/s

180 tokens 60 tok/s

[Free through 12/8 on Desktop and CLI Available in Devin Desktop, Cloud, and CLI at a discounted price until 1/30 Get SWE-2.1](https://devin.ai/desktop)

* * *

## [Benchmark Methodology Notes](https://cognition.com/blog/swe-2-1#benchmark-methodology-notes)

**Harness:** We report upstream or self-reported scores if they're available. Otherwise, we evaluate models on our internal evaluation framework. For closed models, we use the model developer's preferred harness. For open and SWE models, we use the Devin CLI harness.

**Time budgets:** To keep benchmark scores independent of inference speed, we enforce time limits on a simulated clock that charges each model call 1 s of time-to-first-token plus a constant c seconds per output token (a fixed TPS), matching our production serving speeds: $\text{time in seconds} = \text{turns} + c \times \text{tokens} + \text{tools}$.

**ProgramBench:** We report the percentage of tasks where the model passes at least 95% of tests. SWE-2.1 and SWE-2 are run in our harness with a 250k-token context with up to 3 compactions (1M tokens total). Other models are reported in mini-swe-agent with each model's native context window (up to ~1M tokens).

**ExploitBench:** We ran each task five times with a 3-hour simulated clock and took the union of the flags captured for each task. Taking the union of flags is standard practice for this benchmark (see e.g. the [GPT-5.6 System Card](https://deploymentsafety.openai.com/gpt-5-6/gpt-5-6.pdf)).

**ExploitGym:** We report pass@1 over the set of 869 tasks within a 6-hour timeout. We evaluate with mitigations disabled, following Opus 5.5 and Gemini 4 Neon. We report two metrics - flags captured and success, following the ExploitGym paper. “Flags captured” includes successful, real exploits unrelated to the intended vulnerability specified in the initial prompt. To compute “success”, we adopted the upstream agent-as-a-judge implementation.
