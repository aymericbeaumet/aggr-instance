---
title: How Hugging Face Inference Endpoints, Jobs, and Buckets Power Search on Papers with Code
link: https://huggingface.co/blog/pwc-search
source: huggingface-co-blog
published: 2026-08-21T00:00:00Z
updated: 2026-08-21T00:00:00Z
first_seen: 2026-09-19T21:30:23.395188495Z
content: extracted
html: 2026-08-21-how-hugging-face-inference-endpoints-jobs-and-buckets-power.html
preview:
  file: 2026-08-21-how-hugging-face-inference-endpoints-jobs-and-buckets-power.preview-9ed3f86304a4.webp
  width: 256
  height: 128
  color: '#f1efed'
images:
- source: https://huggingface.co/blog/assets/pwc-search/thumbnail.png
  original:
    file: 2026-08-21-how-hugging-face-inference-endpoints-jobs-and-buckets-power.image-9cdc9e6e6a46.png
    width: 1300
    height: 650
  color: '#f9f7f5'
- source: https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/pwc-search/search-results.png
  original:
    file: 2026-08-21-how-hugging-face-inference-endpoints-jobs-and-buckets-power.image-49a0c5fea5cf.png
    width: 1314
    height: 1128
  color: '#f9f7f4'
- source: https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/pwc-search/hybrid-search.png
  original:
    file: 2026-08-21-how-hugging-face-inference-endpoints-jobs-and-buckets-power.image-408e7fc222f5.png
    width: 1494
    height: 828
  color: '#fcfcfd'
- source: https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/pwc-search/architecture.png
  original:
    file: 2026-08-21-how-hugging-face-inference-endpoints-jobs-and-buckets-power.image-38e7bf26cdbf.png
    width: 1012
    height: 828
  color: '#fbfbfa'
- source: https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/pwc-search/inference-endpoint.png
  original:
    file: 2026-08-21-how-hugging-face-inference-endpoints-jobs-and-buckets-power.image-ea90cd32a3d5.png
    width: 3448
    height: 1412
  color: '#fafbfb'
- source: https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/pwc-search/endpoint-analytics.png
  original:
    file: 2026-08-21-how-hugging-face-inference-endpoints-jobs-and-buckets-power.image-2863ab295843.png
    width: 3456
    height: 1978
  color: '#fbfbfb'
- source: https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/pwc-search/related_papers.png
  original:
    file: 2026-08-21-how-hugging-face-inference-endpoints-jobs-and-buckets-power.image-47925cafe0c2.png
    width: 682
    height: 878
  color: '#fcfcfb'
---

3 months ago, we started a [revival](https://www.reddit.com/r/MachineLearning/comments/1tgmwqr/reviving_paperswithcode_by_hugging_face_p/) of [Papers with Code](https://paperswithcode.co) (see also the [announcement tweet](https://x.com/NielsRogge/status/2056366395605078252)). Its goal is to make open AI research accessible and digestible, so that people can easily find the artifacts related to a paper, find state-of-the-art (SOTA) across the various domains of AI, share interesting research and build on top of each other's work. In other words, its goal is to power the wave of research that leads to the next [Transformer](https://paperswithcode.co/paper/1706.03762).

Of course, making AI research accessible requires a powerful search engine, so that humans and agents can quickly find relevant and related work, either through the website or the `pwc search` [CLI command](https://github.com/huggingface/pwc-cli), which agents can use via the [Skill](https://github.com/huggingface/pwc-cli/blob/main/standalone_cli/SKILL.md).

It's important to note that searching for research is not quite the same as searching for regular text. A useful paper search engine should find an exact title or arXiv identifier, but it should also understand a query such as “small language models for code generation” even when those words do not appear together in a paper. It needs to recognize that “the original BERT paper” is a navigational request, tolerate an incomplete title or typos, and still respond quickly when a model service is cold or temporarily unavailable.

![Papers with Code search results for the query 'DINO'](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/pwc-search/search-results.png)

Search results on Papers with Code for the query DINO.

For [Papers with Code](https://paperswithcode.co), we built this as a **hybrid search** system. This is also based on our prior experience at [ML6](http://ml6.eu/), where we developed [RAG](https://paperswithcode.co/paper/2005.11401)-based systems for clients. It turned out that hybrid search typically outperforms keyword- and vector-based search systems, as it combines the best of both worlds (see also [this blog](https://techcommunity.microsoft.com/blog/azure-ai-foundry-blog/azure-ai-search-outperforming-vector-search-with-hybrid-retrieval-and-reranking/3929167) for more info). Keyword search finds exact mentions, whereas vector search finds more fuzzy, semantically similar terms. Note that [rerankers](https://www.pinecone.io/learn/series/rag/rerankers/) (also called cross-encoders) can further improve the results, although they also come with additional overhead and latency.

![Chart showing hybrid retrieval outperforming vector-only and keyword search](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/pwc-search/hybrid-search.png)

Hybrid retrieval outperforms keyword- and vector-only search. Figure from Microsoft, [Azure AI Search: Outperforming vector search with hybrid retrieval and reranking](https://techcommunity.microsoft.com/blog/azure-ai-foundry-blog/azure-ai-search-outperforming-vector-search-with-hybrid-retrieval-and-reranking/3929167) (2023).

Papers with Code relies on a PostgreSQL database, hence its full-text search capabilities provide a fast lexical baseline. For dense embeddings, [pgvector](https://github.com/pgvector/pgvector) is used to add semantic recall, and the [reciprocal rank fusion (RRF)](https://www.elastic.co/docs/reference/elasticsearch/rest-apis/reciprocal-rank-fusion) algorithm combines the two. Three Hugging Face services are used for the dense embeddings:

- [Hugging Face Jobs](https://huggingface.co/docs/hub/jobs) gives us burstable GPU compute for embedding the paper corpus.
- [Hugging Face Storage Buckets](https://huggingface.co/docs/hub/storage-buckets) provides the durable handoff between our database, experiments, and Jobs.
- [Hugging Face Inference Endpoints](https://huggingface.co/docs/inference-endpoints/index) serves low-latency embeddings for live queries and incremental updates.

Today, the system maintains embeddings for more than 110,000 current papers sourced from [arXiv](https://arxiv.org/) and [Daily Papers](https://hf.co/papers). This post explains the architecture, the design decisions behind it, and the lessons we learned while taking it to production.

## [](https://huggingface.co/blog/pwc-search#tldr) TL;DR

We deliberately split search into an offline corpus build and an online search service:

![Architecture diagram of the offline corpus build and online hybrid search pipeline](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/pwc-search/architecture.png)

Architecture of the offline corpus build and online hybrid search pipeline.

The expensive, throughput-oriented work runs as Jobs. Durable artifacts live in a Bucket. Only the small query-embedding step sits on the request path, behind a protected Inference Endpoint, to power the online search. If that endpoint is cold, busy, or unhealthy, search immediately falls back to full-text retrieval. This separation makes the system both powerful and fast.

## [](https://huggingface.co/blog/pwc-search#start-with-a-strict-embedding-contract) Start with a strict embedding contract

Embedding pipelines often fail in subtle ways: a model revision changes, query and document prompts are mixed up, vectors are truncated differently, or an updated abstract no longer matches its stored vector.

We avoid this by treating the embedding format as a versioned API. Every paper is encoded as:

```
normalized title + "\n\n" + normalized abstract
```

For each vector generation, we record:

- the model repository and exact revision;
- the output dimension;
- the input-format version;
- whether the input is a query or a document;
- the normalization method;
- a content hash for the source title and abstract.

Our production generation uses [`Qwen/Qwen3-Embedding-0.6B`](https://huggingface.co/Qwen/Qwen3-Embedding-0.6B), pinned to an exact revision, with 256-dimensional L2-normalized vectors. We selected the model with help from the [MTEB leaderboard](https://huggingface.co/spaces/mteb/leaderboard), the go-to benchmark for comparing embedding models. Note that newer embedding models like Qwen3 allow for 2 new features:

- one can specify a **dynamic embedding size**, which allows to trade-off quality with speed/storage costs. Qwen models call this "MRL" which is short for [Matryoshka Representation Learning](https://paperswithcode.co/paper/2205.13147). You can learn all about it [here](https://huggingface.co/blog/matryoshka). We chose an embedding size of 256 to make the search fast.
- one can provide an **instruction prompt**. Qwen embedding models support a `document` prompt (which we use to embed the papers) and live searches use their `query` prompt (to embed the user query).

This contract follows an embedding from export, through GPU inference, into PostgreSQL, and finally into online retrieval.

## [](https://huggingface.co/blog/pwc-search#jobs-turn-a-database-snapshot-into-a-vector-corpus) Jobs turn a database snapshot into a vector corpus

Full-corpus embedding is a classic batch workload. It needs a GPU for a relatively short period, benefits from high throughput, and should not consume resources between runs. [Hugging Face Jobs](https://huggingface.co/docs/huggingface_hub/guides/jobs) fits that shape well: a Job is defined by a command, a [hardware flavor](https://huggingface.co/docs/hub/main/en/jobs-pricing#pricing), and optionally a Docker image, and can run [uv](https://docs.astral.sh/uv/) scripts with their dependencies declared inline.

Our corpus build starts by exporting the latest version of every paper from a repeatable-read PostgreSQL snapshot. The exporter streams rows rather than loading the catalog into memory, writes bounded JSONL shards, and creates a manifest containing row counts and SHA-256 checksums.

We sync that immutable run directory to a private [Storage Bucket](https://huggingface.co/docs/hub/storage-buckets) and mount the Bucket directly (using [hf-mount](https://github.com/huggingface/hf-mount)) into an `l4x1` Job (an NVIDIA L4 GPU, which has 24GB of VRAM). From the worker's perspective it is simply a filesystem:

```
hf jobs uv run \
  --flavor l4x1 \
  --timeout 6h \
  --volume hf://buckets/OWNER/pwc-paper-embeddings:/bucket \
  embed_papers_job.py \
  --input /bucket/runs/RUN_ID/input \
  --output /bucket/runs/RUN_ID/output \
  --model Qwen/Qwen3-Embedding-0.6B \
  --revision MODEL_REVISION \
  --dimensions 256 \
  --allow-matryoshka
```

The worker:

1. verifies the input manifest and every shard checksum;
2. loads the pinned model revision;
3. sorts texts by length to reduce padding;
4. calls `encode_document` in batches (as noted in the [model card](https://huggingface.co/Qwen/Qwen3-Embedding-0.6B));
5. reduces the batch size automatically if the GPU runs out of memory;
6. truncates the [Matryoshka representation](https://huggingface.co/blog/matryoshka) to 256 dimensions and normalizes it;
7. writes float16 Parquet shards atomically; and
8. records throughput, package versions, hardware, peak VRAM, row counts, and output checksums.

Each completed shard has its own marker, so a restarted Job can skip verified work. This is useful for a large corpus: retrying should just resume work rather than overwriting existing embeddings.

In our 5,000-paper pilot, the Qwen Job encoded about 75 papers per second at 1024 dimensions on an L4 GPU. The same pass could be deterministically materialized at 512 and 256 dimensions, so we could compare the storage and retrieval trade-offs without paying for more inference.

## [](https://huggingface.co/blog/pwc-search#buckets-are-the-connective-tissue) Buckets are the connective tissue

[Storage Buckets](https://huggingface.co/docs/hub/storage-buckets) are mutable, S3-like object storage on the Hub, optimized for AI workloads. They can be accessed through `hf://buckets/...` paths and [mounted](https://github.com/huggingface/hf-mount) read-write in Jobs without building a separate storage integration.

For us, the Bucket is more than a place to put vectors. It is the boundary between three systems with different lifecycles:

- the production database exports source records;
- ephemeral Jobs consume those records and produce vectors;
- the importer validates the results before touching the search index.

We organize artifacts under immutable run prefixes:

```
runs/<run-id>/
├── input/
│   ├── manifest.json
│   └── papers-*.jsonl
└── output/
    ├── manifest.json
    ├── embeddings-*.parquet
    └── embeddings-*.complete.json
```

Buckets themselves are intentionally mutable, so immutability is an application-level rule: a run ID is never overwritten, and every artifact is covered by a manifest and checksum.

This gives us several useful properties:

- **Reproducibility:** we can trace a database generation back to an exact corpus snapshot, model revision, and set of artifacts.
- **Safe retries:** Jobs can resume from completed shards in the same run prefix.
- **Cheap experiments:** several models or dimensions can reuse one verified input snapshot.
- **Controlled rollout:** importing a generation does not activate it. We first validate coverage and build its index.
- **Simple rollback:** the previous generation and its artifacts remain available until the new one is proven stable.

Only after the importer rechecks schemas, checksums, dimensions, normalization, unique paper IDs, and current content hashes do we load the vectors into PostgreSQL. We then build a separate [HNSW](https://en.wikipedia.org/wiki/Hierarchical_navigable_small_world) index for the new generation and atomically mark it active only when every eligible current paper is covered (HNSW is the graph-based algorithm that enables fast vector search).

## [](https://huggingface.co/blog/pwc-search#inference-endpoints-put-semantic-search-on-the-request-path) Inference Endpoints put semantic search on the request path

Batch embeddings solve the document side of retrieval. A user query still needs to be embedded at request time using the same model contract.

We deploy the pinned model as an authenticated [Inference Endpoint](https://huggingface.co/docs/inference-endpoints/index) backed by [Text Embeddings Inference (TEI)](https://github.com/huggingface/text-embeddings-inference). The endpoint accepts the query text and returns a normalized 256-dimensional vector using the model's `query` prompt. Note that one could also leverage [vLLM](https://github.com/vllm-project/vllm) or [SGLang](https://github.com/sgl-project/sglang) here.

![Hugging Face Inference Endpoint overview for the Papers with Code query embedding model](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/pwc-search/inference-endpoint.png)

Hugging Face Inference Endpoint for the Papers with Code query embedding model.

The API then performs a cosine-distance search over the active pgvector generation:

```
SELECT paper_id,
       embedding <=> CAST(:query_vector AS halfvec(256)) AS distance
FROM paper_embeddings
WHERE generation_id = :active_generation
ORDER BY embedding <=> CAST(:query_vector AS halfvec(256))
LIMIT 50;
```

The HNSW index keeps this lookup fast. On our 5,000-paper pilot, the 256-dimensional Qwen index achieved 0.9955 Recall@20 against exact search, with 1.31 ms p50 and 2.21 ms p95 HNSW lookup latency. Its table and index used about 27% of the storage of the 1024-dimensional version while retaining essentially the same ANN recall in that test.

The Endpoint is configured with a maximum of one replica and can [scale to zero](https://huggingface.co/docs/inference-endpoints/guides/autoscaling) when idle. That is a useful cost lever, as this means you're not paying when there's no usage. However, this also means cold starts must be part of the application design rather than treated as an exceptional event, as it takes some time for the endpoint to spin up and serve traffic.

Our query client therefore has deliberately strict behavior:

- a one-second production timeout;
- a non-blocking concurrency limit;
- response dimension, finiteness, and norm validation;
- a short cache keyed by the query and embedding generation;
- a circuit breaker after repeated failures; and
- no raw query text in logs, only a normalized fingerprint.

If the endpoint is scaling up, times out, returns a malformed vector, or has no concurrency available, we skip the semantic branch immediately. Users still receive lexical results instead of waiting for an unreliable dependency.

Inference Endpoints works really reliably, and includes a nice dashboard so you can quickly see key analytics.

![Hugging Face Inference Endpoint analytics dashboard showing request volume, errors, latency, and replica state](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/pwc-search/endpoint-analytics.png)

Inference Endpoint analytics showing request volume, errors, latency, and replica state.

## [](https://huggingface.co/blog/pwc-search#hybrid-retrieval-is-stronger-than-either-branch-alone) Hybrid retrieval is stronger than either branch alone

For every query, the lexical branch retrieves up to 50 candidates using weighted PostgreSQL full-text search. The semantic branch retrieves up to 50 candidates from pgvector.

We combine their ranks using weighted reciprocal rank fusion (RRF):

 \\text{score}(d) = \\sum\_{r \\,\\in\\, \\{\\text{lexical},\\, \\text{semantic}\\}} \\frac{w\_r}{k + \\text{rank}\_r(d)}

RRF is simple and robust, because it combines ranks rather than scores from two systems with different scales. Basically, if a paper is ranked high both by the lexical branch and the semantic branch, it has a higher chance of being ranked high by the hybrid search. We currently use equal branch weights and (k=60) (k is the "rank constant", a hyperparameter of the RRF algorithm).

Dense retrieval improves recall for conceptual queries. Full-text retrieval remains excellent for exact terminology, identifiers, and rare names. We also preserve deterministic identity behavior on top of the fused ranking:

- exact titles and arXiv IDs stay at the top;
- the [method taxonomy](https://paperswithcode.co/methods) recognizes navigational searches such as “the original BERT paper”;
- incomplete titles and bounded spelling mistakes use conservative trigram candidates; and
- ambiguous fuzzy matches abstain rather than forcing a bad result.

Note: hybrid search isn't always the best option, it is recommended to start with keyword search as a cheap and fast baseline, and only adding semantic and/or hybrid search when it turns out those give a reasonable boost in retrieval quality. One could further improve the search by adding a reranker after keyword/semantic/hybrid retrieval, using a model like [Qwen3-Reranker](https://huggingface.co/collections/Qwen/qwen3-reranker).

## [](https://huggingface.co/blog/pwc-search#one-endpoint-two-update-paths) One Endpoint, two update paths

The large initial corpus is embedded with Jobs, but Papers with Code changes continuously. New papers arrive, abstracts are corrected, and new arXiv versions become current.

Launching a GPU Job for a handful of changed rows would add unnecessary startup and orchestration overhead. Instead, an hourly incremental process selects missing or content-changed papers and sends a bounded delta to the same TEI Endpoint, this time with the `document` prompt.

Each run processes at most 500 papers in batches of 16. Before an embedding is written, the source row is locked and its content hash is checked again. If a paper changed during inference, that vector is discarded and picked up by the next run.

This gives us a useful division of labor:

- **Jobs** handle full rebuilds, new model generations, and large backfills.
- **Inference Endpoints** handle interactive query embeddings and small incremental document updates.
- **Buckets** preserve the large-build artifacts and make those builds resumable and auditable.

The hourly path keeps the active index close to the live catalog without turning an online endpoint into an unbounded batch processor.

## [](https://huggingface.co/blog/pwc-search#related-papers-become-almost-free-online) Related papers become almost free online

The same document embeddings also power related-paper recommendations on each paper page.

![Related papers feature](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/pwc-search/related_papers.png)

Related papers for [SenseNova-U1](https://paperswithcode.co/paper/2605.12500).

Because the source paper already has a stored vector, related-paper retrieval requires no model call at request time. It is a single nearest-neighbor query over the active generation. If a vector is temporarily missing, the application can use a previous arXiv version or fill results from the existing task- and citation-based fallback. We fetch citation data through the [Semantic Scholar API](https://www.semanticscholar.org/product/api) and also built [`s2-cli`](https://github.com/huggingface/s2-cli), a command-line interface for querying its citation graph. The latter is used by the agent at [https://paperswithcode.co/chat](https://paperswithcode.co/chat).

## [](https://huggingface.co/blog/pwc-search#what-we-learned) What we learned

### [](https://huggingface.co/blog/pwc-search#1-separate-throughput-work-from-latency-sensitive-work) 1. Separate throughput work from latency-sensitive work

Corpus embedding and query embedding use the same model, but they are different infrastructure problems. Jobs optimize for throughput and bounded cost; Inference Endpoints optimize for availability and request latency.

### [](https://huggingface.co/blog/pwc-search#2-make-storage-the-explicit-contract-between-compute-and-production) 2. Make storage the explicit contract between compute and production

Buckets provide an explicit handoff between compute and production. Checksummed artifacts create a reviewable boundary before data enters the production index.

### [](https://huggingface.co/blog/pwc-search#3-pin-more-than-the-model-name) 3. Pin more than the model name

The revision, dimension, prompt, normalization, and input formatter all affect retrieval. Store them together and validate them everywhere.

### [](https://huggingface.co/blog/pwc-search#4-design-for-cold-starts) 4. Design for cold starts

Scale-to-zero is valuable when traffic is intermittent, but only if the product has a fast fallback. Hybrid search gave us that fallback naturally: lexical search is always useful on its own.

### [](https://huggingface.co/blog/pwc-search#5-smaller-vectors-can-be-a-systems-feature) 5. Smaller vectors can be a systems feature

Matryoshka embeddings let us evaluate quality, memory, index size, and latency as one trade-off. In our pilot, 256 dimensions preserved ANN recall while materially shrinking storage compared with 1024 dimensions.

### [](https://huggingface.co/blog/pwc-search#6-activation-should-be-boring) 6. Activation should be boring

New generations are imported beside the current one, indexed independently, checked for complete and current coverage, and then activated atomically. Rollback is a configuration change, not an emergency recomputation.

Feel free to try out the search at [https://paperswithcode.co](https://paperswithcode.co) or the chat interface at [https://paperswithcode.co/chat](https://paperswithcode.co/chat), and let us know any feedback!
