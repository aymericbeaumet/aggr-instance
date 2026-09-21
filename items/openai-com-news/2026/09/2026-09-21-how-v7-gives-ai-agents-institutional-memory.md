---
title: How V7 gives AI agents institutional memory
link: https://openai.com/index/v7
source: openai-com-news
published: 2026-09-21T00:00:00Z
updated: 2026-09-21T00:00:00Z
first_seen: 2026-09-21T17:01:01.954977951Z
labels:
- startup
summary: Using GPT-5.6, V7 turns scattered company files into context agents can use to complete complex, source-linked work.
content: extracted
html: 2026-09-21-how-v7-gives-ai-agents-institutional-memory.html
preview:
  file: 2026-09-21-how-v7-gives-ai-agents-institutional-memory.preview-0d48854e65bd.webp
  width: 256
  height: 144
  alt: OpenAI and V7 logos framing a black graphite macro texture.
  color: '#c7c6c6'
images:
- source: https://images.ctfassets.net/kftzwdyauwt9/7GdeQ1K1dmN9lKhNw73MmP/e494c0e02e6370e338a3740aee65fbfd/V7-seo-16x9-option-a.png?w=1600&h=900&fit=fill
  original:
    file: 2026-09-21-how-v7-gives-ai-agents-institutional-memory.image-717d244241f1.png
    width: 1600
    height: 900
  variants:
  - file: 2026-09-21-how-v7-gives-ai-agents-institutional-memory.image-3df3a013c2b0.webp
    width: 320
    height: 180
  - file: 2026-09-21-how-v7-gives-ai-agents-institutional-memory.image-bccd4ee3a92e.webp
    width: 640
    height: 360
  - file: 2026-09-21-how-v7-gives-ai-agents-institutional-memory.image-dc078370c4e8.webp
    width: 960
    height: 540
  - file: 2026-09-21-how-v7-gives-ai-agents-institutional-memory.image-d4cbeca3fac0.webp
    width: 1280
    height: 720
  - file: 2026-09-21-how-v7-gives-ai-agents-institutional-memory.image-7884106c2dde.webp
    width: 1600
    height: 900
  color: '#fefefe'
---

Today’s models can reason through complex tasks, but they don’t automatically understand the underlying business context of those tasks. Which fund report is current? How is the same entity named across three systems?

That context lives in documents, data rooms, spreadsheets, emails, and internal tools: scattered, unresolved, and invisible to agents. For teams in finance, insurance, and real estate, retrieval accuracy within workflows is non-negotiable.

After building a widely used computer vision accessibility app together, Rizzoli and Edwardsson started [V7](https://www.v7labs.com/) in 2018 to help companies teach AI systems how their businesses work. V7 Go is an agentic platform to build mission critical workflows, and organize buried context into memory that agents can query and act on.

V7 Go uses GPT‑5.6 Luna to extract information from millions of files and organize it in the Context Graph, which connects entities, relationships, and cited evidence, powering MCP search and repeatable workflows that can span hundreds of steps. For Workflows, V7 Go uses GPT‑5.6 Terra and Sol for reasoning and tool use across complex, multi-step instructions that take humans dozens of hours to complete. V7 is also starting to use GPT‑6 Astra on the most demanding Context Graph queries, including financial analysis across thousands of documents.

With context, models, and tools working together, V7 says agents complete 50–100 step workflows in minutes, reaching 99.9% accuracy, while maintaining an auditable trail of every decision made.

> “To solve hard enterprise use cases across finance and insurance, AI needs to learn how your business operates just as well as it learned from the Internet.”

—Alberto Rizzoli, Co-Founder and CEO at V7

## Giving agents the context to understand the whole business

The Context Graph solves a specific problem. Agents have to rediscover context on every request, leading to dozens of searches costing time and tokens, and often missing key information buried in relationships.

When data arrives, V7 Go connects to repositories such as SharePoint and Google Drive, scans them for entities, relationships, facts, attributes, and metrics, and populates a graph that’s an order of magnitude cheaper and faster to traverse than long-context approaches.

The Context Graph gives agents a structured, up-to-date record they can query directly. When a new file arrives, V7 Go identifies the companies, funds, people, or any entity in an ontology, then connects each fact to a new or existing record, and preserves cited evidence to the original source. If the graph does not contain enough information, V7 Go can still search the underlying documents with RAG.

V7 has also tested how much the structure of that context matters. On HERB, a benchmark for finding and connecting information spread across enterprise systems, V7’s retrieval-only system outperformed the official baseline by 69% and reduced hallucinations on un-answerable queries by 38%. V7 Go uses that source-linked context to keep complex workflows grounded in each company’s own information.

V7 Go uses that organized context in workflows such as private equity deal screening and insurance underwriting. In the demo below, a workflow extracts information from a deal document called a Confidential Information Memorandum (CIM) feeds key financials, deal terms, management details, and cites risk fields before V7 Go produces a screening note.

The Context Graph makes it so that a model can work with a firm’s history without relearning it each time. For long-running agents, V7 Go keeps recent exchanges in the model’s active context and stores older material in the graph to be retrieved when needed.

That shared context is already speeding up document-heavy work across V7’s customers:

- Asset managers can screen deals 21x faster than before, reducing a full-day process to just 15 minutes

- A financial services team cut review time from more than 100 hours to under 10, saving $12,000 in expert costs per task

- Insurance teams reduced errors in claims processing by 13.5% compared to a manual baseline, after granting their agents historical knowledge of all previous claims and existing policies

> “With GPT-5.6 Terra, we have been able to remove many intermediate workflow stages that previously existed only to simplify the task for the model. It’s saved us days of delivery work and often gets things right on the first build of a workflow, thanks to a stronger model and access to more context.”

—Simon Edwardsson, Co-Founder and CTO at V7

## Choosing OpenAI to keep complex workflows on track

Complex, multi-step workflows depend on a model reliably following lengthy instructions, running tools, interpreting results, and navigating a long series of steps. A single upstream error can lead to expensive consequences and broken trust in AI systems. V7 Go guides models across long horizon tasks spanning deterministic code, handovers to smaller models, file generation steps, and integrations, with an auditable trace of every run.

In AI-generated workflows, V7 Go maps each step to fast, medium, and smart tiers. GPT‑5.6 Luna handles structured extraction and other high-volume work, and GPT‑5.6 Terra or Sol power chat, the Go Agent path, and steps that require more reasoning or tool use.

V7 tests new models against a continuously maintained benchmark suite covering citation accuracy, extraction quality across hundreds of document types, answer correctness, instruction following, latency, cost, and real-world enterprise workflows. OpenAI outperforms most models on the behaviors V7 cares about most. OpenAI also approved and implemented V7’s capacity increase needs within hours, compared with weeks for other providers V7 uses.

> “We chose OpenAI as our default because it performs best on the multi-step tool workflows V7 Go depends on. In our Context Graph benchmark, GPT-5.6 Sol reduced the tool-call error rate from 2.7% with GPT-5.5, to 0.2%”

—Simon Edwardsson, Co-Founder and CTO at V7

In V7’s latest harness, key workflows with several external calls now finish up to 50% faster. V7 measured another efficiency gain with GPT‑5.6 Luna: a 78% lower cost per document than with GPT‑5.4 mini. V7 also moved its document-heavy V7 Go workloads from the Chat Completions API to the Responses API. In its testing, the change reduced token use by roughly 5% for some PDF-heavy workflows and improved caching reliability.

V7 also tested GPT‑6 Astra on its most difficult queries. GPT‑5.6 Sol had saturated many of V7’s existing benchmarks, so the company created a more challenging set of graph-query questions using messier, real-world data across thousands of documents. The test’s dataset spans four difficulty levels. On the very-hard level, V7 reports that GPT‑5.6 Sol scored 78%, while GPT‑6 Astra scored 89% accuracy. Both models scored close to 100% on the easy, medium, and hard levels.

## Bringing what the business knows into ChatGPT and Codex

V7 Go already exposes Context Graph querying and ingestion through its MCP server, so customers can use it from ChatGPT and other compatible clients. They can also create V7 Go workflows through MCP in Codex. Together with simpler workflow design, this has reduced the time required to create a medium-length workflow from around one hour to about 20 minutes.

V7’s longer-term goal is to make that shared memory more proactive. The team is working toward workflows that start when facts in the Context Graph change, flag inconsistencies, and show people which analyses need another look. A restated fund report, for example, could prompt V7 Go to flag work that still relies on the old figures.

“Our goal is to help enterprises re-tool for the age of AI, with workflows that solve mission critical tasks, and memory that outperforms us humans” says Rizzoli. “Finance firms getting real value from AI will not be the ones with the most agents. They will be the ones with the best context.”
