---
title: Ringg’s AI agents resolve up to 65% of customer calls with OpenAI
link: https://openai.com/index/ringg
source: openai-com
published: 2026-09-23T12:00:00Z
updated: 2026-09-23T12:00:00Z
first_seen: 2026-10-19T01:26:53.998049939Z
summary: Using GPT-5.6, Ringg powers multilingual agents across voice, chat, WhatsApp, and web for 90% less cost vs. GPT-4.1.
content: extracted
html: 2026-09-23-ringg-s-ai-agents-resolve-up-to-65-of-customer-calls-with.html
---

When call volume rises, customer service operations typically scale by adding people, but this increases the cost and complexity of every interaction. [Ringg](https://www.ringg.ai/), a voice and chat agent platform, saw this problem firsthand while working with large consumer businesses in India.

These companies were not only struggling to answer more calls, their customer service agents were also battling fragmented, manual systems to help customers complete tasks, from buying insurance to booking appointments.

That experience led Ringg to build an enterprise agent platform with high-efficiency models like GPT‑5.6 at its core, spanning voice, chat, WhatsApp, and the web. Migrating suitable real-time workloads from GPT‑4.1 to GPT‑5.6 reduced model costs by approximately 90% while delivering the required quality and latency.

> “Model quality is only part of the equation. We also need low latency, reliable tool use, strong instruction following, and economics that work at scale. OpenAI gave us the balance we needed.”

—Siddharth Tripathi, Co-founder, Ringg

Ringg’s agents now handle more than 7 million connected calls each month, and its customers have an average customer satisfaction (CSAT) score of 4.8.

## Building an agent platform around customer outcomes

For agents to complete a customer request, it may require checking a policy, retrieving an account record, scheduling an appointment, updating a CRM, or transferring the conversation to a specialist with the relevant context intact.

For real-time interactions, Ringg uses GPT‑5.6 Luna and other models to interpret customer requests, select tools, and guide customers through multi-step workflows. Ringg’s orchestration layer executes actions across CRMs, ticketing platforms, payment systems, scheduling tools, and internal APIs, escalating cases to a human with a conversation summary when needed.

Ringg’s knowledge system combines structured filtering with semantic retrieval across datasets, PDFs, CSVs, and business documents, helping agents answer questions using relevant enterprise information.

Ringg can also divide work among specialized subagents for qualification, support, verification, scheduling, and escalation. The platform coordinates those steps while maintaining a consistent conversation with the customer across voice, chat, WhatsApp, and the web.

## Routing each task to the right OpenAI model

After evaluating OpenAI alongside alternatives across conversational quality, latency, instruction following, tool calling, multilingual performance, reliability, and cost, Ringg found that OpenAI delivered the strongest overall balance for production workloads.

GPT‑5.6 and other OpenAI models also performed more consistently in following complex instructions, executing tool calls, maintaining conversational context, and operating reliably at enterprise scale.

Ringg routes work to a specific OpenAI model based on the needs of the task. GPT‑4.1 handles most real-time voice and chat traffic. GPT‑5.6 Luna remains in the production stack and is used when its performance, latency, or price-performance profile better suits a request. GPT‑5.6 Terra handles post-call analysis, including summaries and sentiment classification. And GPT‑5.6 Sol supports evaluation, prompt improvement, and model-as-judge workflows.

When a customer speaks during a voice call or sends a chat, Ringg’s orchestration system combines that input with the agent’s instructions, conversation history, customer-specific data, relevant knowledge-base context, and available tools. Ringg’s routing layer then selects the appropriate model and configuration. The model’s output is passed through the orchestration system and delivered through the customer’s chosen channel.

For longer interactions, the system creates a structured summary when the context approaches approximately 80,000 tokens. The conversation can then continue with the important information preserved, without repeatedly sending the entire history.

## Using production evals to improve quality and cost

Ringg tests models using historical conversations and simulated customer flows before production deployment. Its evaluation platform uses those results to identify weaknesses and recommend prompt improvements, creating a continuous improvement loop for its agents and configurations.

One evaluation involved Ringg’s post-call analysis workflow. The company tested GPT‑5.6 Terra against alternatives like Gemini 2.5 Flash, and Terra came out on top. Ringg moved summaries and sentiment classification to GPT‑5.6 Terra. Terra maintained high accuracy for summaries and sentiment analysis while materially improving unit economics.

The company also tests models across language and regional variations, including conversations that switch between languages or combine English with local-language phrases, common behavior in the markets it serves. GPT‑5.6 Terra outperformed Gemini 2.5 Flash, with up to 97% accuracy on common regional languages, making GPT‑5.6 a better fit for agents serving Ringg’s customers.

Models that pass offline testing are introduced to a small share of production traffic before rollout expands. In production, Ringg’s router monitors latency and endpoint health across regions and shifts traffic when an endpoint becomes unavailable or crosses a latency threshold. Specialized nodes, alerts, and versioned deployments help isolate problems and limit their impact.

This approach also helps Ringg improve unit economics. Migrating certain real-time workloads from GPT‑4.1 to GPT‑5.6 Luna reduced model costs by approximately 90%.

> “OpenAI has been highly responsive when we’ve needed support with model migrations. The dedicated Slack support and access to the core engineering team help us move quickly with less engineering uncertainty, ship faster, and expand what our agents can accomplish.”

—Siddharth Tripathi, Co-founder, Ringg

## Delivering customer results across industries

For its customers, Ringg’s agents resolve up to 65% of routine customer inquiries without any involvement from a human agent.

Policybazaar, one of India’s largest online insurance platforms, uses Ringg to connect more than 57,000 customer requests, and 67% of calls are handled without human intervention. Policybazaar’s average response time fell from 8–12 minutes to under 60 seconds, an improvement of approximately 88%.

At Practo, a global healthcare platform, Ringg’s agents help customers book healthcare appointments and complete service requests. Following deployment, Practo achieved an 85% first-call resolution rate and response times below three seconds. Operating costs declined by 70% compared with its previous human-led workflow, while Ringg now completes more than 1,000 appointment bookings each day.

Online investment platform Groww uses Ringg to resolve 72% of inbound queries related to IPOs, futures, and options entirely through self-service, with an average handling time of two minutes.

## Extending agents into the browser

Using OpenAI’s computer-use capabilities, Ringg is developing browser agents for platform onboarding, Know Your Customer (KYC) processes, IT troubleshooting, on-call incident support, and claims processing.

The company is also developing a context layer that can preserve information across channels and interactions. A customer might begin a request over voice, continue it on WhatsApp, and finish it in a browser without having to repeat the same details each time.

> “OpenAI’s computer-use capabilities accelerated our browser-agent roadmap. They let us combine what is happening on a user’s screen with conversational context, so agents can guide people through complex workflows in real time.”

—Siddharth Tripathi, Co-founder, Ringg

For Ringg, the next generation of customer operations will be measured by completed business outcomes and automation depth, rather than call volume or headcount.
