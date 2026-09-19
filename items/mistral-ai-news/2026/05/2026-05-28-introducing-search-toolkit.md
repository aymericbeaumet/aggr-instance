---
title: Introducing Search Toolkit
link: https://mistral.ai/news/search-toolkit/
source: mistral-ai-news
published: 2026-05-28T11:42:21Z
updated: 2026-05-28T11:42:21Z
first_seen: 2026-09-19T21:30:23.395188495Z
summary: Search Toolkit is a composable framework for building production search pipelines for AI applications.
content: extracted
html: 2026-05-28-introducing-search-toolkit.html
preview:
  file: 2026-05-28-introducing-search-toolkit.preview-f13b786ff79e.webp
  width: 256
  height: 134
  color: '#ef997d'
images:
- source: https://mistral.ai/cms-media/api/media/file/OG-mistral-main_1x.jpg
  original:
    file: 2026-05-28-introducing-search-toolkit.image-28135271f04d.jpg
    width: 1200
    height: 630
  color: '#fefefe'
- source: https://mistral.ai/_astro/search-api-1_ZWCkTP.webp?dpl=6aad049eaf4c2d00095b91e5
  original:
    file: 2026-05-28-introducing-search-toolkit.image-703b1adc7a05.webp
    width: 1920
    height: 722
  variants:
  - file: 2026-05-28-introducing-search-toolkit.image-168acbf8e1c1.webp
    width: 320
    height: 120
  - file: 2026-05-28-introducing-search-toolkit.image-34cb8705c6a3.webp
    width: 640
    height: 241
  color: '#f7f5f2'
- source: https://mistral.ai/_astro/search-icon-2_LFJpr.webp?dpl=6aad049eaf4c2d00095b91e5
  original:
    file: 2026-05-28-introducing-search-toolkit.image-5247367963e0.webp
    width: 1920
    height: 565
  variants:
  - file: 2026-05-28-introducing-search-toolkit.image-4c3b1f8b8185.webp
    width: 320
    height: 94
  - file: 2026-05-28-introducing-search-toolkit.image-df9b341e9d61.webp
    width: 640
    height: 188
  - file: 2026-05-28-introducing-search-toolkit.image-683d1f19ce65.webp
    width: 960
    height: 283
  color: '#e98734'
---

Today, we're releasing Search Toolkit in public preview. Search Toolkit is a composable framework for building production search pipelines for AI applications. We built it because teams building search infrastructure still spend too much engineering time on plumbing. Most stitch together separate tools for ingestion, retrieval, and evaluation, each with its own interface and its own assumptions about data. Search Toolkit brings all three into a single framework with a shared interface, so teams spend their time improving search quality instead of maintaining integrations. Search Toolkit is open source and runs wherever your infrastructure does. Cloud, on-premises, edge.

## **Search infrastructure is still harder than it should be.**

Most teams building retrieval systems spend more time assembling infrastructure than improving search quality. Ingestion requires one set of tools. Retrieval requires another. Evaluation, if it happens at all, is bolted on with a separate framework and separate assumptions about data shape.

Teams report weeks of integration work before they can run a single query against their own data. Measuring whether the retriever is returning the right results often requires yet another toolchain. For organisations building RAG workflows or internal knowledge systems, that overhead multiplies at every layer.

## **Where it fits.**

**Enterprise search.** Most organisations don't have a search problem. They have a dozen search problems. Internal wikis, support ticket systems, document repositories, file storage, codebases. Each source has different structure, different metadata, and needs different processing to index well. Teams typically end up building a separate ingestion pipeline for each one, with its own parsing logic, its own chunking strategy, and its own assumptions about what a "document" looks like. The result is a set of isolated indexes that can't be searched together, or a brittle custom layer that tries to unify them and becomes its own maintenance burden. Search Toolkit provides consistent processing and indexing patterns across source types within a single framework, so teams add new sources without rebuilding the pipeline each time.

![](https://mistral.ai/_astro/search-api-1_ZWCkTP.webp?dpl=6aad049eaf4c2d00095b91e5)

**RAG and retrieval quality.** When a RAG system returns poor results, the first question is whether the problem is retrieval or generation. In practice, most teams have no clean way to answer that. They tweak prompts, adjust chunking strategies, and swap models without knowing whether the retriever is surfacing the right context in the first place. And even teams that do focus on retrieval often lack the tooling to compare strategies rigorously, on their own data, with their own relevance judgments. The alternative is writing custom evaluation scripts for each experiment. Search Toolkit includes built-in evaluation that measures retriever performance independently, so you can isolate retrieval quality from generation quality and compare configurations as your corpus evolves.

**Domain-specific retrieval.** Legal filings, medical records, codebases, financial disclosures. Off-the-shelf retrievers are trained on general-purpose text and tend to struggle with specialised terminology, document structures, and relevance criteria that differ from web search. Teams that need domain-tuned retrieval often end up building custom retrieval infrastructure from scratch, which is expensive to maintain and hard to evaluate.

## **Search in an agentic world**

Agents working on enterprise tasks need access to enterprise context. They make retrieval decisions autonomously and at high volume, so the quality of the search infrastructure underneath them directly affects every downstream step. For searching across large document corpora, agents perform semantic search on an index, which gives them precise results at low latency.

Agents also need live data. With [Connectors](https://docs.mistral.ai/studio-api/knowledge-rag/connectors), they pull directly from source systems like CRMs, code repositories, and productivity tools through MCP integrations. An agent can query an indexed corpus when it needs to search across a large body of content, and pull live data from a source system when it needs the latest state. Search Toolkit gives your agents a high-quality indexed search path to call on alongside live retrieval.

![](https://mistral.ai/_astro/search-icon-2_LFJpr.webp?dpl=6aad049eaf4c2d00095b91e5)

## **What's inside.**

**Ingestion.** Index and process data from multiple sources with configurable pipelines. Search Toolkit handles document parsing, chunking, and embedding generation. Custom document formats and preprocessing steps plug in through a standard adapter interface.

**Retrieval.** Search Toolkit ships with BM25 sparse retrieval, dense embedding-based retrieval, and hybrid configurations that combine both. Each is configurable to your data and use case.

**Evaluation.** Measure search quality with built-in metrics: recall, precision, MRR, and NDCG. Run evaluations against your own test sets, compare retriever configurations side by side, and track quality across releases.

All modules share a common configuration interface. Replace your indexer, swap your retriever, add an evaluator. The rest of the pipeline adapts.

Search Toolkit has been designed for advanced use cases for the enterprise, and battle tested across financial services, manufacturing, public sector, and media & entertainment verticals. CMA CGM uses Search Toolkit alongside Voxtral to help journalists detect fake news. The pipeline processes audio from three distinct data sources and returns alerts within 15 seconds end to end.

**Watch the demo**

## **Get started.**

The fastest way to try Search Toolkit is with our [**starter app template**](https://github.com/mistralai/search-starter-app).

**Prerequisites**

Install

[Docker](https://docs.docker.com/get-docker/)

. You also need

[uv](https://docs.astral.sh/uv/)

in the generated project.

**Scaffold a new project**

```bash
uvx copier copy gh:mistralai/search-starter-app my-search-projectcd my-search-project
```

**Run it**

```bash
# Start Vespa locally with Dockermake setup-vespa# Index sample datamake ingest path=sample_data/hello.txt# Run a querymake search query="hello world"
```

The template includes:

- Pre-configured Vespa indexing

- Hybrid retrieval (BM25 + vector)

- Sample data and ingestion pipeline

For full details, see the [starter app README](https://github.com/mistralai/search-starter-app).

## What’s next

Once you’ve tried the starter app, dive deeper:

- [**Tune your ingestion pipeline**](https://docs.mistral.ai/studio-api/knowledge-rag/search-toolkit/ingestion) – Configure parsers, chunking strategies, embedding models, and extractors for specific file types to handle your data sources.

- [**Manage Vespa schema & relevance**](https://docs.mistral.ai/studio-api/knowledge-rag/search-toolkit/vespa)– Optimize indexing and ranking profiles for your use case.

- [**Build your dream retrieval**](https://docs.mistral.ai/studio-api/knowledge-rag/search-toolkit/retrieval) – Leverage advanced features like LLM query rewriting, reranking, and hybrid retrieval.

For the full reference, see the [Search Toolkit documentation](https://docs.mistral.ai/studio-api/search-toolkit).
