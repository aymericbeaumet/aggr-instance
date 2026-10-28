---
title: Gemini 4 Carbon
link: https://blog.google/innovation-and-ai/models-and-research/gemini-models/introducing-gemini-4-carbon/
source: hnrss-org
published: 2026-10-28T17:01:23Z
updated: 2026-10-28T17:01:23Z
first_seen: 2026-10-28T21:53:36.421640500Z
authors:
- acossta
content: extracted
html: 2026-10-28-gemini-4-carbon.html
remote_preview:
  url: https://storage.googleapis.com/gweb-uniblog-publish-prod/images/gemini_carbon_102726_social_share.width-1300.png
---

Gemini 4 Carbon is our strongest model with state-of-the-art cybersecurity and long horizon capabilities.

* * *

![Faceted iridescent crystal surrounded by floating white number 4s.](https://storage.googleapis.com/gweb-uniblog-publish-prod/images/gemini_carbon_102726_header.width-2200.format-webp.webp)

Today, we’re announcing the next frontier model: Gemini 4 Carbon, our strongest model with state-of-the-art cybersecurity and long horizon capabilities.

Carbon ranks #1 on the recently updated v5.0 of [Artificial Analysis Intelligence Index](https://artificialanalysis.ai/evaluations/artificial-analysis-intelligence-index) at 59. It shows significant gains across coding and agentic tasks as well as across key capabilities critical for cybersecurity defense, landing a firm lead in SEC-bench Pro (88.2%) for finding vulnerabilities, ExploitGym (54.8%) for exploitation, and CWE-bench (71.0%) for patching. On internal benchmarks, Carbon drives a strong uplift in generating secure code, and in live coding environments with our own teams it shows reduced misalignment and fabrication rates.

Building on [Gemini 4 Argon](https://blog.google/innovation-and-ai/models-and-research/gemini-models/introducing-gemini-4-argon/), which was launched two weeks ago, Carbon illustrates the pace at which we’re pushing the frontier forward. Our researchers and models are working hand in hand to advance capabilities faster, while strengthening our safeguards in tandem.

We're taking a deliberate approach to rollout and are actively engaged in the U.S. government’s voluntary process for pre-release model testing. Starting this week, we will make Carbon available to a subset of cyber defenders and will gradually phase access to additional trusted partners through our [Fairwind Program](https://deepmind.google/fairwind-program/). We’re continuing to gather feedback and iterate on guardrails before making Carbon available to developers, businesses, and consumers via our product surfaces as soon as possible.

Gemini 4 Carbon will launch at the same introductory price [1](https://blog.google/innovation-and-ai/models-and-research/gemini-models/introducing-gemini-4-carbon/#footnote-1) as Argon at $2 per million input tokens and $10 per million output tokens, with cached input tokens priced at 95% off input token price.

## Improving coding and reasoning for challenging, long-horizon tasks

Designed to take on much harder, long-horizon engineering problems, Carbon is built for sustained agentic workflows. Internally, our teams have seen Carbon run for days to persist through dead ends to tackle complex, multi-step engineering tasks like root-causing and patching subtle bugs across large codebases and low-level systems.

Building on user feedback, we focused on improving model performance across a variety of domains, including coding, agentic, and scientific tasks, as well as full-stack web development and multimodal understanding. With improved quality of output and artifacts in web development, Carbon jumps +89 elo points to land at #4 (1768) on [WebDev Arena](https://arena.ai/leaderboard/code/webdev).

![Gemini 4 Carbon benchmark table](https://storage.googleapis.com/gweb-uniblog-publish-prod/images/gemini_4_carbon__evals-table.width-2000.format-webp.webp)

## State-of-the-art cybersecurity performance to help build secure systems

Gemini 4 Carbon delivers a significant leap in cybersecurity capabilities. It achieves state-of-the-art performance across the cybersecurity defense lifecycle — vulnerability detection, validation, and remediation.

Our teams are already seeing strong gains in cyberdefense. On internal benchmarks, Carbon achieves more than 2x uplift in success rate for generating secure code over Argon. As another example, the Chrome team has reported that Carbon performed more than 10X better than another lab's large frontier model when generating high quality patches. Wiz, our leading Cloud security platform, also observes Carbon leading on benchmarks for web penetration testing, a core capability for [Wiz's Scan for Good](https://www.wiz.io/blog/scan-for-good-critical-ai-exposures) initiative.

Carbon excels across key external benchmarks (evaluated without guardrails):

- **Finding real vulnerabilities:** Core to cybersecurity defense is finding real vulnerabilities. Carbon achieves a state-of-the-art 88.2% on [SEC-bench Pro](https://sec-bench.github.io/), autonomously uncovering complex flaws buried in real-world software.

![SEC bench Pro chart](https://storage.googleapis.com/gweb-uniblog-publish-prod/images/gemini_4_carbon_secbench-pro.width-2000.format-webp.webp)

- **Validating risk through exploitation:** To prioritize which vulnerabilities to remediate first or remediate at all, cyber defenders increasingly rely on models to check which vulnerabilities are exploitable. Carbon sets a new state-of-the-art on the popular exploitation benchmark [ExploitGym](https://github.com/sunblaze-ucb/exploitgym) (54.8%).

![chart showing ExploitGym benchmark](https://storage.googleapis.com/gweb-uniblog-publish-prod/images/gemini_4_carbon_exploitgym.width-2000.format-webp.webp)

- **Patching at the root cause:** Ultimately after identifying the vulnerabilities, fixing the root cause is a critical step in the cybersecurity defense lifecycle. Carbon reaches 71.0% on Collinear’s [CWE-bench](https://cwe-bench.com/#leaderboard), turning validated findings directly into working code fixes.

![chart showing CWE-bench](https://storage.googleapis.com/gweb-uniblog-publish-prod/images/gemini_4_carbon_cwe-bench.width-2000.format-webp.webp)

## Advancing frontier model safety & security

Gemini 4 Carbon builds on the same multi-layered safety and security stack of Argon — combining model training, real-time misuse defenses, prompt-injection resilience, and chain-of-thought monitoring. In particular, we made significant strides in model alignment with Argon, and reported its propensities to behave in an aligned way in our [Safety and Security report](https://storage.googleapis.com/deepmind-media/gemini/gemini_4_safety_report.pdf). With Carbon, we challenged ourselves to go a step further on alignment, and trained the model to respond to complex or high-pressure tasks in the right way.

- **Reducing shortcuts and loophole exploitation:** With Carbon, we further mitigated shortcut attempts in training while ensuring high-quality coding performance. Carbon reduced successful cheating on Terminal-Bench 4.0 to 0.0%, with attempts to cheat dropping to 0.4% — all while improving overall accuracy. In live coding environments with our internal teams, we observed that high-severity misalignment alerts dropped by almost 80%.
- **Greater transparency and honesty:** In our internal testing, Carbon cuts unsupported claims by approximately 2x over Argon. It puts methodological caveats directly into the deliverable or withholds blocked actions with a clear list of what remains required, rather than fabricating or hedging around missing evidence.
- **Improved collaboration with the user in the face of ambiguity:** Rather than guessing or forcing a path forward, Carbon retrieves the relevant sources and surfaces discrepancies to the user before acting — as a result, users should observe a much better collaborative experience with Carbon.
- **Following through without overstepping:** Carbon simultaneously reduced overreach by 26% and under-service by 29% resulting in much more reliable execution of multi-step plans end-to-end while respecting project constraints and staying strictly within scope.
- **Resisting prompt injection:** While Argon previously led the Gray Swan’s Indirect Prompt Injection (IPI) Benchmark in prompt injection resistance with a very low 0.7% attack success rate, Carbon achieves 0.0% — resisting every evaluated attack attempt.

![chart showing Gray Swan IPI benchmarks](https://storage.googleapis.com/gweb-uniblog-publish-prod/images/gemini_4_carbon_grey_swan.width-2000.format-webp.webp)

## Coming soon to developers, businesses, and consumers

Pushing the frontier forward requires moving fast while remaining deliberate about deployment. With Gemini 4 Carbon, our goal is to deliver meaningful leaps in software engineering and cybersecurity defense without compromising on safety or reliability. We’re eager to see how the initial cohort of cyber defenders use this model and we look forward to bringing Carbon to developers, businesses, and consumers soon.
