---
title: 'Intelligence per Watt: Measuring Intelligence Efficiency of Local AI'
link: https://arxiv.org/abs/2511.07885
source: hnrss-org-frontpage
published: 2026-09-14T09:16:39Z
updated: 2026-09-14T09:16:39Z
first_seen: 2026-09-16T17:16:50.864462523Z
authors:
- pythonic_hell
summary: 'Article URL: https://arxiv.org/abs/2511.07885 Comments URL: https://news.ycombinator.com/item?id=49694035 Points: 126 # Comments: 42'
content: extracted
html: 2026-09-14-intelligence-per-watt-measuring-intelligence-efficiency-of.html
preview:
  file: 2026-09-14-intelligence-per-watt-measuring-intelligence-efficiency-of.preview-41145fe12e79.webp
  width: 256
  height: 149
  alt: arXiv logo
  color: '#ece6e5'
images:
- source: https://arxiv.org/static/browse/0.3.4/images/arxiv-logo-fb.png
  original:
    file: 2026-09-14-intelligence-per-watt-measuring-intelligence-efficiency-of.image-88f5088404c9.png
    width: 1200
    height: 700
  variants:
  - file: 2026-09-14-intelligence-per-watt-measuring-intelligence-efficiency-of.image-fc6386ce1e2e.webp
    width: 320
    height: 187
  - file: 2026-09-14-intelligence-per-watt-measuring-intelligence-efficiency-of.image-49eabc2e18a4.webp
    width: 640
    height: 373
  - file: 2026-09-14-intelligence-per-watt-measuring-intelligence-efficiency-of.image-f275dcf4bd8a.webp
    width: 1200
    height: 700
  color: '#fefefe'
---

## Title:Intelligence per Watt: Measuring Intelligence Efficiency of Local AI

Authors: [Jon Saad-Falcon](https://arxiv.org/search/cs?searchtype=author&query=Saad-Falcon,+J), [Avanika Narayan](https://arxiv.org/search/cs?searchtype=author&query=Narayan,+A), [Hakki Orhun Akengin](https://arxiv.org/search/cs?searchtype=author&query=Akengin,+H+O), [J. Wes Griffin](https://arxiv.org/search/cs?searchtype=author&query=Griffin,+J+W), [Herumb Shandilya](https://arxiv.org/search/cs?searchtype=author&query=Shandilya,+H), [Adrian Gamarra Lafuente](https://arxiv.org/search/cs?searchtype=author&query=Lafuente,+A+G), [Medhya Goel](https://arxiv.org/search/cs?searchtype=author&query=Goel,+M), [Rebecca Joseph](https://arxiv.org/search/cs?searchtype=author&query=Joseph,+R), [Shlok Natarajan](https://arxiv.org/search/cs?searchtype=author&query=Natarajan,+S), [Etash Kumar Guha](https://arxiv.org/search/cs?searchtype=author&query=Guha,+E+K), [Shang Zhu](https://arxiv.org/search/cs?searchtype=author&query=Zhu,+S), [Ben Athiwaratkun](https://arxiv.org/search/cs?searchtype=author&query=Athiwaratkun,+B), [John Hennessy](https://arxiv.org/search/cs?searchtype=author&query=Hennessy,+J), [Azalia Mirhoseini](https://arxiv.org/search/cs?searchtype=author&query=Mirhoseini,+A), [Christopher Ré](https://arxiv.org/search/cs?searchtype=author&query=R%C3%A9,+C)

[View PDF](https://arxiv.org/pdf/2511.07885) [HTML (experimental)](https://arxiv.org/html/2511.07885v6)

> Abstract:Large language model (LLM) queries are predominantly processed by frontier models in centralized cloud infrastructure. Demand growth strains this paradigm faster than providers can scale. Two advances create an opportunity to rethink it: small, local LMs (<=20B active parameters) now achieve competitive performance to frontier models on many tasks, and local accelerators (e.g., Apple M4 Max) can host these models at interactive latencies. This raises the question: can local inference viably redistribute demand from centralized infrastructure? This requires measuring both whether local LMs can accurately answer real-world queries and whether they can do so efficiently on power-constrained devices (e.g., laptops). We propose intelligence per watt (IPW), task accuracy per unit of power, as a unified metric for the capability and efficiency of local inference across model-accelerator configurations. We evaluate 20+ state-of-the-art local LMs, 8 hardware accelerators (local and cloud), and 1M real-world single-turn chat and reasoning queries. For each query, we measure accuracy (local LM win rate against frontier models), energy, latency, and power. We find three key results. First, local LMs successfully answer 88.7% of these queries, with accuracy varying by domain. Second, longitudinal analysis from 2023-2025 shows IPW improved 5.3x, driven by both algorithmic and accelerator advances, with locally-serviceable query coverage rising from 23.2% to 71.3%. Third, local accelerators achieve at least 1.4x lower IPW than cloud accelerators running identical models, revealing significant headroom for local accelerator optimization. These findings demonstrate that local inference can meaningfully redistribute demand from centralized infrastructure for a substantial subset of queries, with IPW serving as the critical metric for tracking this transition.

Subjects:

Distributed, Parallel, and Cluster Computing (cs.DC); Artificial Intelligence (cs.AI); Computation and Language (cs.CL); Machine Learning (cs.LG)

Cite as:

[arXiv:2511.07885](https://arxiv.org/abs/2511.07885) \[cs.DC\]

(or [arXiv:2511.07885v6](https://arxiv.org/abs/2511.07885v6) \[cs.DC\] for this version)

[https://doi.org/10.48550/arXiv.2511.07885](https://doi.org/10.48550/arXiv.2511.07885)

arXiv-issued DOI via DataCite

## Submission history

From: Jon Saad-Falcon \[[view email](https://arxiv.org/show-email/c69de5f7/2511.07885)\] \
 **[\[v1\]](https://arxiv.org/abs/2511.07885v1)** Tue, 11 Nov 2025 06:33:30 UTC (5,373 KB)\
 **[\[v2\]](https://arxiv.org/abs/2511.07885v2)** Fri, 14 Nov 2025 00:53:12 UTC (5,538 KB)\
 **[\[v3\]](https://arxiv.org/abs/2511.07885v3)** Thu, 26 Feb 2026 17:09:14 UTC (5,538 KB)\
 **[\[v4\]](https://arxiv.org/abs/2511.07885v4)** Thu, 21 May 2026 03:40:21 UTC (5,134 KB)\
 **[\[v5\]](https://arxiv.org/abs/2511.07885v5)** Fri, 7 Aug 2026 02:40:27 UTC (5,621 KB)\
 **\[v6\]** Sun, 6 Sep 2026 05:29:37 UTC (5,608 KB)
