---
title: Introducing GPT-6.1 Terra
link: https://openai.com/index/introducing-gpt-6-1-terra
source: openai-com
published: 2026-10-26T00:00:00Z
updated: 2026-10-26T00:00:00Z
first_seen: 2026-10-26T19:53:35.807667070Z
labels:
- product
summary: Bringing fast and capable computer use to everyone
content: extracted
html: 2026-10-26-introducing-gpt-6-1-terra.html
---

We’re introducing **GPT-6.1 Terra**, a fast, efficient model that combines detailed visual reasoning and precise computer use alongside clearer, more natural writing. A significant step up from GPT‑5.6 Terra, it excels at the communication and computer-based tasks that fill the workday, from drafting proposals and interpreting technical drawings to filling out forms and editing videos.

Terra continues to advance the cost-intelligence frontier across our model families, providing a new balance of speed, capability, and cost. Its standard API prices are 50% lower than those of GPT-5.6 Terra and GPT-6.1 Sol, making it practical for developers to automate high-volume work, like reading incoming invoices and entering their details into accounting software. In ChatGPT Work and Codex, your plan goes further with GPT-6.1 Terra—the same usage allowance covers more tasks than with GPT‑5.6 Terra and substantially more than with GPT‑6.1 Sol or GPT‑6 Astra.

## From visual understanding to action

We measured GPT-6.1 Terra’s computer-use capabilities on **Agents’ Last Exam,** a benchmark of professional tasks that includes turning architectural drawings into a 3D model and reviewing an animated film shot by shot to create a time-coded log for a restoration team. At medium reasoning effort, GPT-6.1 Terra scores 15 percentage points higher than GPT-5.6 Terra at about one-third the cost. It even performs on par with GPT-6 Astra at only one-tenth its price per token. At max effort, Terra is over three times as token efficient as Claude Opus 5.5, and one-fourth its price per token, resulting in a ~9x lower cost per task. Terra is also much quicker, completing tasks **over three times faster** than both GPT-6 Astra and GPT-6.1 Sol.

On **MMMU-Pro**, a benchmark of visual reasoning, GPT-6.1 Terra outperforms GPT-5.6 Terra at every reasoning setting. It even nearly matches the scores of our most capable frontier model, GPT-6 Astra, and GPT-6.1 Sol at much lower cost.

**OSWorld 2.1** tests how well AI agents use desktop apps to complete multistep tasks, such as building a 3D model from a technical drawing. At medium reasoning effort, GPT-6.1 Terra surpasses GPT-5.6 Terra’s best score at a fraction of the cost per task.

## A significant step up on factuality and coding

This year, we’ve invested heavily in reducing hallucinations and factual errors produced by our models. On our internal benchmark of error-inducing prompts, the error rate at low reasoning effort fell from 53.6% with GPT-5.6 Terra to 16.7% with GPT-6.1 Terra, a drop of nearly 37 percentage points. At maximum effort, the error rate fell by 18 percentage points to 4.3%, nearly matching the rates for GPT-6 Astra and GPT-6.1 Sol.

GPT-6.1 Terra marks a substantial improvement over GPT-5.6 Terra for software engineering. **Terminal-Bench 4.0** tests how well coding agents solve problems from the command line, such as fixing a webpage whose layout shifts as it loads. At maximum reasoning effort, GPT‑6.1 Terra solves 50.3% of tasks, more than double GPT‑5.6 Terra’s published score of 21.5%. For the most complex coding tasks, GPT-6.1 Sol and GPT-6 Astra remain the better choice.

**Terminal-Bench-Science 0.1** tests how well agents use code to carry out scientific research tasks, such as tracking cell divisions in a time-lapse video. At max reasoning, GPT-6.1 Terra surpasses GPT-6.1 Sol at lower cost, coming within five points of Opus 5.5 at less than one-fifth the estimated cost.

## Safety and alignment

As we make powerful AI more efficient and accessible, we continue training, testing, and refining our safeguards. GPT‑6.1 Terra meets the Critical threshold in cybersecurity under our Preparedness Framework, and maintains the protections developed for GPT‑6 Astra.

GPT-6.1 Terra brings with it the alignment improvements of the GPT-6 series. According to our evaluations, GPT-6.1 Terra is more likely to stay within scope and honestly disclose limitations and potential blockers to the user compared to GPT-5.6 Terra. In our prompt-injection tests, GPT-6.1 Terra also showed strong resistance to malicious instructions designed to divert it from the user’s task. More detail is available in the [system card](https://deploymentsafety.openai.com/gpt-6-1-terra).

## Pricing and availability

Starting today, GPT-6.1 Terra is available to all Plus, Pro, Enterprise, and Edu users in ChatGPT Work and Codex. Developers can also access it through the OpenAI API as `gpt-6.1-terra`.

Standard API pricing is $1 per million input tokens, $0.05 per million cached input tokens, and $5 per million output tokens.
