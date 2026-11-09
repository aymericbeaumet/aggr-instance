---
title: 'Our Smallest, Most Efficient Model: Muse Ember'
link: https://research.meta.ai/blog/muse-ember
source: hnrss-org
published: 2026-11-09T17:58:46Z
updated: 2026-11-09T17:58:46Z
first_seen: 2026-11-09T21:00:57.230935818Z
authors:
- dvaplima
labels:
- language models
content: extracted
html: 2026-11-09-our-smallest-most-efficient-model-muse-ember.html
remote_preview:
  url: https://research.meta.ai/articles/muse-glint/hero/blog-header-light-v1.webp
  alt: Abstract illustration of blue orbital curves, arrows and points looping out from a single center.
---

Today, we're introducing Muse Ember, our smallest and most efficient model designed for complex reasoning execution and the work agents repeat all day: tool calls, coding subtasks, routing, extraction, and long document parsing.

Muse Ember delivers more intelligence per dollar than Claude Haiku 5.5 or GPT-6 Luna at any reasoning setting: it matches Haiku's best score at about a third of the cost and beats every Luna setting. It also sits on the efficiency frontier of the full Artificial Analysis leaderboard: no model scores higher for less per task.

### Intelligence vs. Cost Per Task

Artificial Analysis Intelligence Index against cost per task at each reasoning effort, from the raw Artificial Analysis results. Muse Ember costs are estimates at proposed pricing.

- xhigh
- max
- medium
- high
- xhigh
- max
- high
- xhigh
- max
- Muse Ember
- Claude Haiku 5.5
- GPT-6 Luna

Intelligence vs. Cost Per Task: Intelligence vs. Cost Per Task
| Series              | Measurement | Value |
| ------------------- | ----------- | ----- |
| Cost Per Task (USD) | $0.116      |       |
| Score               | 33.9        |       |
| Cost Per Task (USD) | $0.136      |       |
| Score               | 35.8        |       |
| Cost Per Task (USD) | $0.092      |       |
| Score               | 24.6        |       |
| Cost Per Task (USD) | $0.159      |       |
| Score               | 29.6        |       |
| Cost Per Task (USD) | $0.248      |       |
| Score               | 34.3        |       |
| Cost Per Task (USD) | $0.411      |       |
| Score               | 36.2        |       |
| Cost Per Task (USD) | $0.073      |       |
| Score               | 21.4        |       |
| Cost Per Task (USD) | $0.104      |       |
| Score               | 23.3        |       |
| Cost Per Task (USD) | $0.152      |       |
| Score               | 27.8        |       |

Artificial Analysis | November 9, 2026. Each point is one reasoning-effort setting. Muse Ember cost at list pricing.

### Muse Ember evaluation results

Muse Ember evaluation results: Evaluation results
| Evaluation results                        | Featured model. Muse Ember (max) | Claude Haiku 5.5 (max)       | GPT-6 Luna (max) |
| ----------------------------------------- | -------------------------------- | ---------------------------- | ---------------- |
| Best result. 37.8                         | 13.9                             | 20.2                         |                  |
| Best result. 61.6                         | 57.0                             | 57.1                         |                  |
| Best result. 46.8                         | 36.1                             | 37.0                         |                  |
| Best result. 40.9                         | 39.2                             | 16.4                         |                  |
| Best result. 46                           | 37                               | 31                           |                  |
| 27.2                                      | Best result. 34.9                | 25.2                         |                  |
| Best result. 22.8                         | 20.8                             | Best result. 22.8            |                  |
| Best result. 69.5 (partial) 33.8 (binary) | 68.9 (partial) 27.8 (binary)     | 50.5 (partial) 17.0 (binary) |                  |
| Best result. 80.6                         | 78.6                             | 62.8                         |                  |
| Best result. 36                           | Best result. 36                  | 28                           |                  |

For more details about our evaluations, see our [report](https://research.meta.ai/static/muse-ember-evaluation-report "Muse Ember Evaluation Report").

Muse Ember is available today on the Meta Model API, at $0.10 per million input tokens and $0.50 per million output tokens.

In early testing, our partners put Muse Ember to work on the kinds of tasks agents repeat all day, from reviewing contracts to writing code.

![Box logo](https://research.meta.ai/articles/muse-glint/partner-quotes/box-logo-light-v1.svg?dpl=dpl_ANtafPPBTB3QMKUEjE5giciZF6j1)

> “On the Box AI Complex Work Eval, Muse Ember delivers impressive accuracy, speed and token efficiency. It can score a contract against a company's negotiation playbook clause by clause and land on the right overall risk rating, or screen candidates against a detailed brief and flag the borderline ones who still qualify as alternates. Muse Ember is a fast, efficient foundation for agents doing real work on enterprise content.”

Author

Yashodha Bhavnani

Role

VP of AI Products

Company

Box

## Agents

Agents spend most of their tokens on many small, structured steps, and Muse Ember is built for exactly that. It can carry a business workflow end to end, pulling records, calling tools in sequence, and updating systems across apps.

With a 1M-token context window and up to 512K output tokens, it can take in a full codebase or document set in a single call.

### AutomationBench-AA

AutomationBench-AA results at max reasoning, reproduced from the doc's current chart.. Score

AutomationBench-AA: AutomationBench-AA
| Series | Measurement            | Value |
| ------ | ---------------------- | ----- |
| Model  | Muse Ember (max)       |       |
| Score  | 37.8%                  |       |
| Model  | GPT-6 Luna (max)       |       |
| Score  | 20.2%                  |       |
| Model  | Claude Haiku 5.5 (max) |       |
| Score  | 13.9%                  |       |

Artificial Analysis | November 9, 2026. Strict pass@1.

### Agents' Last Exam

Agents' Last Exam results at max reasoning, reproduced from the doc's current chart.. Score

Agents' Last Exam: Agents' Last Exam
| Series | Measurement            | Value |
| ------ | ---------------------- | ----- |
| Model  | Muse Ember (max)       |       |
| Score  | 61.6%                  |       |
| Model  | GPT-6 Luna (max)       |       |
| Score  | 57.1%                  |       |
| Model  | Claude Haiku 5.5 (max) |       |
| Score  | 57.0%                  |       |

Meta internal evaluation. Linux subset.

### Personal Superintelligence Index

Personal Superintelligence Index results at max reasoning, reproduced from the doc's current chart.. Score

Personal Superintelligence Index: Personal Superintelligence Index
| Series | Measurement            | Value |
| ------ | ---------------------- | ----- |
| Model  | Muse Ember (max)       |       |
| Score  | 46.8%                  |       |
| Model  | GPT-6 Luna (max)       |       |
| Score  | 37.0%                  |       |
| Model  | Claude Haiku 5.5 (max) |       |
| Score  | 36.1%                  |       |

Internal composite index of personal and knowledge work.

## Coding

Muse Ember is built for the coding subtasks an agent sends off hundreds of times a session: reading files, running tests, making targeted fixes.

### Terminal-Bench 4.0

Terminal-Bench 4.0 results at max reasoning, reproduced from the doc's current chart.. Score

Terminal-Bench 4.0: Terminal-Bench 4.0
| Series | Measurement            | Value |
| ------ | ---------------------- | ----- |
| Model  | Muse Ember (max)       |       |
| Score  | 40.9%                  |       |
| Model  | Claude Haiku 5.5 (max) |       |
| Score  | 39.2%                  |       |
| Model  | GPT-6 Luna (max)       |       |
| Score  | 16.4%                  |       |

Each model in its best published harness. See footnote 1.

### Coding Agent Index-AA

Coding Agent Index-AA results at max reasoning, reproduced from the doc's current chart.. Score

Coding Agent Index-AA: Coding Agent Index-AA
| Series | Measurement            | Value |
| ------ | ---------------------- | ----- |
| Model  | Muse Ember (max)       |       |
| Score  | 46                     |       |
| Model  | Claude Haiku 5.5 (max) |       |
| Score  | 37                     |       |
| Model  | GPT-6 Luna (max)       |       |
| Score  | 31                     |       |

Artificial Analysis | November 9, 2026

### FrontierCode 2.0-AA

FrontierCode 2.0-AA results at max reasoning, reproduced from the doc's current chart.. Score

FrontierCode 2.0-AA: FrontierCode 2.0-AA
| Series | Measurement            | Value |
| ------ | ---------------------- | ----- |
| Model  | Claude Haiku 5.5 (max) |       |
| Score  | 34.9%                  |       |
| Model  | Muse Ember (max)       |       |
| Score  | 27.2%                  |       |
| Model  | GPT-6 Luna (max)       |       |
| Score  | 25.2%                  |       |

Artificial Analysis | November 9, 2026

For the hardest problems, pair it with Muse Wonder: Muse Wonder plans, Muse Ember handles the volume.

## Multimodal

Muse Ember natively understands text, images, audio, and video, so agents can read screenshots, charts, and scanned documents, or work from voice memos, meeting recordings, and video without a separate model. It can work through long real-world documents, operate a computer directly from screenshots, and pick out fine visual detail.

### GDP.pdf-AA

GDP.pdf-AA results at max reasoning, reproduced from the doc's current chart.. Score

GDP.pdf-AA: GDP.pdf-AA
| Series | Measurement            | Value |
| ------ | ---------------------- | ----- |
| Model  | Muse Ember (max)       |       |
| Score  | 22.8%                  |       |
| Model  | GPT-6 Luna (max)       |       |
| Score  | 22.8%                  |       |
| Model  | Claude Haiku 5.5 (max) |       |
| Score  | 20.8%                  |       |

Artificial Analysis | November 9, 2026

### OSWorld 2.1

OSWorld 2.1 results at max reasoning, reproduced from the doc's current chart.. Score

OSWorld 2.1: OSWorld 2.1
| Series | Measurement            | Value |
| ------ | ---------------------- | ----- |
| Model  | Muse Ember (max)       |       |
| Score  | 69.5%                  |       |
| Model  | Claude Haiku 5.5 (max) |       |
| Score  | 68.9%                  |       |
| Model  | GPT-6 Luna (max)       |       |
| Score  | 50.5%                  |       |

Meta internal evaluation. Partial-credit reward.

### PerceptionBench

PerceptionBench results at max reasoning, reproduced from the doc's current chart.. Score

PerceptionBench: PerceptionBench
| Series | Measurement            | Value |
| ------ | ---------------------- | ----- |
| Model  | Muse Ember (max)       |       |
| Score  | 80.6%                  |       |
| Model  | Claude Haiku 5.5 (max) |       |
| Score  | 78.6%                  |       |
| Model  | GPT-6 Luna (max)       |       |
| Score  | 62.8%                  |       |

Meta internal evaluation. With tools.

## Safety

Before release, we evaluated Muse Ember under our Meta Superintelligence Scaling Framework, including chemical and biological, cybersecurity, and loss-of-control risks. Across the categories assessed, our evaluations found that Muse Ember remained below the framework’s high-risk thresholds. Though it’s a small model, it received the same core safety training and testing. We also evaluated how Muse Ember behaves when acting on users' behalf. That covers whether it respects user intent, handles sensitive information appropriately, resists instructions hidden in web pages, documents, or tool outputs that try to redirect it, and seeks human input before high-impact actions.

## Availability

Muse Ember is available today on Muse Code and Meta Model API as `muse-ember-1.0`. It costs $0.10 per million input tokens, $0.004 per million cached input tokens and $0.50 per million output tokens, with a 1M-token context window and up to 512K output tokens. Muse Ember supports the Responses API.

When we introduced Muse Wonder and Muse Wonder Full, we said we would use them to develop specialized models we can bring to everyone. Muse Ember is the first trained using Muse Wonder Full. It rounds out our portfolio of models: Muse Wonder for the hardest problems, Muse Spark for coding and agentic workflows, and Muse Ember for repeatable coding and agentic tasks at scale at about $0.14 per Artificial Analysis task, a cost low enough to run in every product, for everyone.

### Muse Ember and Muse Spark 1.3 comparison

Muse Ember and Muse Spark 1.3 comparison: Artificial Analysis comparison
| Artificial Analysis                       | Featured model. Muse Ember (max) | Featured model. Muse Spark 1.3 (max) |
| ----------------------------------------- | -------------------------------- | ------------------------------------ |
| 36                                        | Best result. 37                  |                                      |
| Best result. $0.14                        | $2.39                            |                                      |
| Best result. $0.10 (input) $0.50 (output) | $1.25 (input) $4.25 (output)     |                                      |

Our goal is personal superintelligence: models that help you pursue your goals, create what you imagine, deepen your relationships, and take action on what you value most. Bringing it to everyone takes more than frontier capability. It takes frontier capability at a price that scales. We have even more efficient models in training and look forward to sharing what's next.

[Try Muse Ember today](https://dev.meta.ai)
