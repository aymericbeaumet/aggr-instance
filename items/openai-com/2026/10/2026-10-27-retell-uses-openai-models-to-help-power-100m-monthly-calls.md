---
title: Retell uses OpenAI models to help power 100M monthly calls
link: https://openai.com/index/retell-ai
source: openai-com
published: 2026-10-27T15:00:00Z
updated: 2026-10-27T15:00:00Z
first_seen: 2026-10-27T23:58:53.000547404Z
summary: GPT-6 Astra tops Retell’s voice agent benchmark, while Codex helps it reach a $1.6M annual revenue run rate per employee.
content: extracted
html: 2026-10-27-retell-uses-openai-models-to-help-power-100m-monthly-calls.html
---

[Retell AI](https://www.retellai.com/) is a voice AI platform for contact center operations, helping businesses build agents that can answer questions, take action, and connect a caller with a person when needed. Its platform now serves more than 4,000 businesses, with monthly call volume growing from 30 million in early 2026 to more than 100 million by September.

As Retell serves more enterprises with complex workflows, it uses OpenAI models to keep call quality and reliability high. Today, OpenAI models generate responses for 80% of calls on Retell, with customers achieving resolution rates above 80%. Retell has also surpassed 1 trillion tokens consumed across OpenAI models, reflecting the scale of that adoption.

Retell’s engineers use Codex throughout development, from researching unfamiliar code to building features and debugging issues, and GPT‑6 Astra to test front-end interfaces.

> “We choose models the way our customers choose vendors: on real calls, not demos. OpenAI keeps earning its spot.”

—Bing Wu, Co-founder and CEO, Retell AI

## Choosing OpenAI models for the demands of a live call

Enterprise voice agents need to follow company policies and stay within their assigned roles, even as conversations change. Retell built its vCX benchmark to compare dozens of models using difficult, multi-turn conversations drawn from real production scenarios.

It tests how well models follow instructions, stay grounded in the information they are given, call the right tools, and use natural conversational language. In Retell’s October 13, 2026 benchmark, GPT‑6 Astra ranked first for instruction following at 89.7%.

OpenAI models lead Retell’s voice agent benchmark (chart data)
| Highlight | Model           | Score |
| --------- | --------------- | ----- |
| true      | GPT-6 Astra     | 0.897 |
| false     | GPT-6.1 Sol     | 0.885 |
| false     | GPT-6 Sol       | 0.862 |
| false     | GPT-5.5         | 0.856 |
| false     | Claude-5.5 Opus | 0.838 |

Source: bench.retellai.com · October 13, 2026 · All providers · “None/low” reasoning · Fast variants excluded

Retell also shared evaluations conducted before the launch of the GPT‑6 Astra model that showed GPT‑5.5 ranking highest among models on both non-hallucination rate (88.0%) and tool-call correctness (88.3%) with reasoning enabled, outperforming Gemini‑3.1‑Pro and Claude‑4.6 on both measures. It also led on non-hallucination rate with reasoning off.

Retell says new customers who choose OpenAI models continue using them. Customers also have access to OpenAI Realtime models through the Realtime API and OpenAI text-to-speech as a voice engine. Retell makes new OpenAI models available the day they launch and reruns its benchmark as models improve.

## Resolving more than 80% of customer requests with agents

Contact center teams use Retell to define their agents’ business context, goals, and rules. Developers can further configure workflows and integrations.

During a call, Retell’s harness runs the audio models, tracks the conversation, and manages when the agent speaks, listens, or responds to interruptions. OpenAI models, including GPT‑6.1 Sol, GPT‑6 Astra, and GPT‑5.5, use the customer’s instructions and conversation context to generate responses and request tool calls. Retell executes those calls through integrations with the customer’s systems and can write outcomes back to a customer record.

When human judgment is needed, the voice agent briefs the representative with context, answers their follow-up questions, and confirms they are ready before connecting the customer. After a call, Retell uses GPT‑5.6 Terra to analyze the transcript and extract outcomes and other data.

With this approach, Retell reports customer resolution rates above 80%. At electronics company Anker, agents handle Tier‑1 post-sales support in the US and UK, including order tracking, warranty checks, and troubleshooting. Anker reported an 80.4% case resolution rate and a Net Promoter Score (NPS) of 63. Its team also said the agents’ tone, pacing, and warmth met the standard they expected of a human agent.

At Pine Park Health, Retell agents handle appointment reminders and proactive scheduling and record the outcomes in patients’ medical charts. The team recovered medical-assistant time equivalent to 2.1 full-time roles and redirected it to patient care. Its scheduling agent achieved a 55.7% booking rate when it reached a patient.

## Keeping calls responsive as volume grows

Daytime traffic on Retell’s platform can reach more than 50 times its overnight low. To help keep calls responsive even at peak demand, Retell uses dedicated servers with OpenAI, shared across its customers. In Retell’s October 13, 2026 benchmark, GPT‑6 Astra and GPT‑6 Sol each recorded median first-sentence latency of 883 milliseconds, with GPT‑5.6 Luna ranking highest at 497 milliseconds.

OpenAI’s regional API endpoints allow developers to route traffic to specific geographic regions, which helps Retell reduce network travel time and meet customers’ data residency requirements in industries such as healthcare and financial services.

Retell also builds redundancy into each layer of the service. It runs its own telephony stack with a second stack ready to take over, along with backups for voice and model services, to help keep calls running when a service fails.

## Cutting latency with OpenAI models on Amazon Bedrock

Retell reaches OpenAI models through Amazon Bedrock, OpenAI’s API, and Azure. Retell built its platform to answer callers quickly, so it routes requests among the three providers based on latency. Bedrock has the lowest latency and currently receives the most traffic.

As of November 2026, OpenAI models responded through Bedrock in about half the time they took through the other two providers. Retell can route every OpenAI model on its platform through Bedrock, in every agent workflow.

> “For our use case, we value latency a lot. More traffic naturally goes to Bedrock because it is the best option for latency.”

—Zexia Zhang, Co-founder and CTO, Retell AI

## Using Codex to research, build, and debug

As Retell’s enterprise growth brings more requests for custom workflows and integrations, its engineers often need to work in unfamiliar parts of the codebase. Before writing code, they give Codex a high-level requirement. Codex surveys the code, checks it against actual data, and explains what needs to change. One engineer said Codex did this research better and much faster than he could.

During implementation, engineers say Codex often catches details they had not considered. For debugging, Codex cross-references logs and database records with the code to investigate what is happening. Retell’s engineers also use Codex in their work with customers building on OpenAI models to speed up tasks such as writing the instructions that guide how a model responds in a customer’s application.

To test changes to its web interface, Retell relies on GPT‑6 Astra’s computer use. When an engineer opens a front-end pull request, GPT‑6 Astra opens the interface, clicks through it, tests flows, tries to break them, and reports the issues it finds. Retell says earlier models could not perform this work reliably.

Retell continues to hire, but its engineering team hasn’t needed to grow at the same pace as the business. Retell reports $1.6 million in annual revenue run rate per employee.

> “By using Codex, the engineering team has a virtual teammate that can pair with them on any kind of difficult task.”

—Bing Wu, Co-founder and CEO, Retell AI

## Putting customer expertise to work

Every business knows how it wants to serve its customers. Retell’s goal is to let the people closest to that work apply their expertise directly, using advanced voice AI and a platform that lets them build, test, and improve their own agents. Next, Retell plans to expand quality assurance and simulation, support more languages, and develop knowledge base auto-learning.

As OpenAI models continue to advance, Retell expects its customers to deliver world-class service to millions of callers, on their own terms.

“No one knows a company’s customers better than the company itself. We give them the power to act on that expertise and improve AI agents directly. The goal is for them to build an in-house team of internally deployed engineers who own and run it all,” says Bing.
