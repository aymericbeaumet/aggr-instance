---
title: Grok Collections API
link: https://x.ai/news/grok-collections-api
source: x-ai-news
published: 2025-12-22T00:00:00Z
first_seen: 2026-09-08T10:17:10.166315Z
summary: State-of-the-art RAG system built directly into our API.
content: extracted
html: 2025-12-22-grok-collections-api.html
preview:
  file: 2025-12-22-grok-collections-api.preview-a9675f32390c.webp
  width: 256
  height: 144
  color: '#4d414c'
images:
- source: https://media.x.ai/v1/website/collections-api-1d546e34.webp
  original:
    file: 2025-12-22-grok-collections-api.image-c1d1163a5009.webp
    width: 1200
    height: 673
  variants:
  - file: 2025-12-22-grok-collections-api.image-ca89a571d013.webp
    width: 48
    height: 27
  - file: 2025-12-22-grok-collections-api.image-a467cfaee6b6.webp
    width: 320
    height: 179
  color: '#020408'
---

Today, we're excited to announce Collections API. With Collections, you can upload and search through entire datasets. From PDFs and Excel sheets to entire codebases, you can upload your files into a knowledge base that supports precise and fast search. This allows developers to build RAG applications without the headache of managing indexing and retrieval infrastructure.

To help you get started, we're making file indexing and storage free for the first week\*, with retrieval priced at a flat rate of $2.50 per 1,000 searches.

## [Indexing](https://x.ai/news/grok-collections-api#indexing)

- **Powerful document understanding**: We use OCR and layout-aware parsing to extract text while preserving structure such as the layout of a PDF, hierarchy of an Excel table, or the syntax of code.
- **Smart file management**: Easily upload, update, and download files. And when a file changes, our system efficiently reindexes it to ensure your collection is never stale.
- **Broad format support**: Collections supports a wide range of file types. [(see full list)](https://docs.x.ai/developers/files/collections#supported-mime-types)

## [Retrieval](https://x.ai/news/grok-collections-api#retrieval)

Choose the retrieval method that best fits your use case:

- **Semantic search**: To search using the meaning and intent behind a query.
- **Keyword search**: For precise term matching.
- **Hybrid search**: For the highest accuracy, combine keyword and semantic search. We support both a dedicated reranker model and reciprocal rank fusion.

What is our financial forecast for Q1 2026?

Financial\_plan\_2026.txt

Our company's annual financial projections indicate a robust growth trajectory for the upcoming fiscal year , with expected revenue increases driven by expanded market share in emerging sectors. Analysts predict a 15% rise in Q1 2026 , bolstered by strategic investments in technology and supply chain optimization. Key metrics such as EBITDA and net profit margins are forecasted to improve.

## [Benchmark Results](https://x.ai/news/grok-collections-api#benchmark-results)

Our Collections API delivers state-of-the-art retrieval performance, matching or outperforming leading models in real-world RAG tasks across finance, legal, and coding domains.

These fields are especially challenging due to their long, dense documents. To avoid hallucinations and deliver reliable answers, models must retrieve the exact passages and reason over them accurately.

### Accuracy\*

(Higher is better)

| Task | xAI  Grok 4.1 Fast | Google  Gemini Pro 3 | OpenAI  GPT 5.1 |
| ---- | ------------------ | -------------------- | --------------- |
| 93.0 | 85.9               | 84.7                 |                 |
| 73.9 | 74.5               | 71.2                 |                 |
| 86   | 85                 | 81                   |                 |

\*Internal source.

### [Financial Analysis](https://x.ai/news/grok-collections-api#financial-analysis)

Extracting tabular and numerical data from files can be challenging with semantic search alone. Hybrid search enables you to accurately retrieve this data from documents such as SEC filings\*, allowing the model to precisely reference information.

Retrieval Score

xAI Collections & Grok 4.1

Google File Search & Gemini Pro 3\*\*

OpenAI VS & GPT 5.1

\*Based on an internal dataset.

\*\*Gemini does not expose the actual retrieved files so this metric measures the files cited by Gemini rather than the raw retrieved files. We set the default top k for Gemini to be 20 passages.

### [Legal Analysis (LegalBench)](https://x.ai/news/grok-collections-api#legal-analysis-legalbench)

The [LegalBench dataset](https://github.com/zeroentropy-ai/legalbenchrag) tests retrieval and reasoning over nuanced legal language and complex cross-references, consisting of 128 challenging question-answer pairs drawn from an extensive corpus of authentic commercial contracts across multiple datasets.

Retrieval Score

xAI Collections & Grok 4.1

Google File Search & Gemini Pro 3\*

OpenAI VS & GPT 5.1

\*Gemini does not expose the actual retrieved files so this metric measures the files cited by Gemini rather than the raw retrieved files. We set the default top k for Gemini to be 20 passages.

### [Codebase (DeepCodeBench)](https://x.ai/news/grok-collections-api#codebase-deepcodebench)

Code understanding is crucial for applications such as code summarization and generation. We use the [DeepCodeBench dataset](https://huggingface.co/datasets/Qodo/deep_code_bench) to comprehensively benchmark for this. It features a diverse set of tasks drawn from real-world open-source repositories, API usage, and complex algorithmic problems.

### End to End Answer Performance

Accuracy Score

Grok 4.1

Gemini Pro 3

GPT 5.1

\*We evaluated the code understanding capability of agentic search on 232 code Q&A datapoints from DeepCodeBench across 8 repositories containing 8,000 files.

## [Data Privacy](https://x.ai/news/grok-collections-api#data-privacy)

We do not use user data stored on Collections for model training purposes, unless the user has given consent.

## [Start Building](https://x.ai/news/grok-collections-api#start-building)

### [Creating and Searching Collections](https://x.ai/news/grok-collections-api#creating-and-searching-collections)

py

### [Using Collections in Chat](https://x.ai/news/grok-collections-api#using-collections-in-chat)

py

### [Direct API Usage](https://x.ai/news/grok-collections-api#direct-api-usage)

sh

\*You may be charged after the free trial period. We will follow up with more information.
