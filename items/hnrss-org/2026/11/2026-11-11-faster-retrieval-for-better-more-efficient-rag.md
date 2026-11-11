---
title: Faster Retrieval for Better, More Efficient RAG
link: https://machinelearning.apple.com/research/faster-retrieval
source: hnrss-org
published: 2026-11-11T16:57:45Z
updated: 2026-11-11T16:57:45Z
first_seen: 2026-11-11T21:51:32.649893059Z
authors:
- zerop
content: extracted
html: 2026-11-11-faster-retrieval-for-better-more-efficient-rag.html
remote_preview:
  url: https://mlr.cdn-apple.com/media/Home_1200x630_48225d82e9.png
  alt: Apple Machine Learning Research
---

Maximum inner product search (MIPS) sits at the core of recommendation systems, search, and retrieval-augmented generation (RAG). Given a query vector, MIPS systems search within a database of millions of candidate key vectors for the one that best matches the query. The original data point associated with that best-matching key is then used to handle the query: in RAG, the document that is best suited to answer the question is typically prefixed to the query.

As the number of keys grows to millions and vector dimensions reach thousands, the computational cost of running an exhaustive comparison of the query to all keys grows dramatically. Numerous approximate methods have been proposed to trade off accuracy for speed using clustering and quantization. While these baseline methods focus on the properties of the set of keys to build up clever indexing structures, their geometric arguments loosely assume that the set of queries itself will resemble that found in keys, or bypass any considerations about what the queries will look like. This can be problematic on two fronts: In the crucial RAG case, queries arise from short user-generated questions, whereas keys represent long, informative, and curated pieces of text, making keys and queries substantially different. Additionally, most MIPS use cases deployed in production target an existing base of users with a stable, well-characterized query distribution — a signal that generic, query-agnostic methods leave untapped. Crafting methods that work best for a likely source of inputs — rather than aiming for good performance on all possible inputs — is a central tenet of modern machine learning that helps redirect compute resources more efficiently.

In [Amortizing Maximum Inner Product Search with Learned Support Functions](https://machinelearning.apple.com/research/amortizing-inner-product-search), a paper presented at ICML 2026, researchers from Apple train small neural networks (NNs) — once, offline, for a fixed database of keys — that learn how to route queries toward their best match faster. Their approach trades off an upfront amortization cost to understand how typical queries tend to be matched in exchange for faster, more accurate retrieval at inference time through these small NNs.

A fundamental principle in MIPS is to leverage the geometry of the set of keys to speed up search: Typical MIPS systems split the set of keys into *k* clusters — *k* is typically much smaller than the total number of keys *n* — keeping track of a representative centroid for each cluster. When receiving a new query, the system compares it with these *k* centroids. The query is then routed toward the cluster of the representative centroid it is closest to, and subsequently compared exhaustively with all keys stored in that cluster. If the database has *n* keys and *k* evenly distributed clusters, the effective number of keys that will be considered in that second step is on the order of *n*/*k* — a factor-of- *k* speedup when compared to the *n* comparisons done in the naive exhaustive approach. Unfortunately, this routing mechanism can result in errors, since centroid routing is not an exact approach, as illustrated in [Figure 1](https://machinelearning.apple.com/research/faster-retrieval#figure1) below.

Figure 1: Geometric approaches to search can fail and miss nearest neighbors. This is due to their reliance on partitioning the space based on distances to centroids. In the animation, a nearest neighbor is missed as it is not associated with the nearest centroid.

What makes MIPS interesting from a machine learning perspective is that it is built on a mathematical structure that had been largely overlooked before this work: The function that maps a query to the largest inner product with any key in a set is the [support function](https://en.wikipedia.org/wiki/Support_function) of that set, a well-studied object in convex analysis. This function has two important properties: First, it is convex, so it can be approximated by a class of neural networks specifically designed for convex functions, [input convex neural networks (ICNNs)](https://proceedings.mlr.press/v70/amos17b.html). Second, the gradient of that support function at any query returns the best-matching key itself.

The broad idea behind this work is to *learn* a neural network that can mimic the ground-truth support function (or its gradient directly) for a much lower computational cost. This can be achieved through a classical ML technique known as regression: one simply needs to precompute a large number of training examples formed by sampling input vectors (user queries) and pair them with the relevant output signal produced by MIPS (either the optimal score or the optimal key vector itself). All that is then needed is to train a suitably parameterized neural network to replicate the output given the input.

If the regression problem were to be solved perfectly, according to the mathematical approach outlined above, the same neural network that is trained to mimic the support function could also output the optimal key by evaluating its gradient (through automatic differentiation). In practice, we observe that training separate, specialized networks for each of these two tasks works best:

The **SupportNet** neural network architecture can learn the support function directly, using a (loosely constrained) ICNN. The approach is mathematically principled and aligns with the convex structure of the problem. We find that this approach can shine when used to compute one support function per data cluster to improve routing quality.

**KeyNet** models learn to *directly* output the best-matching key vector when fed a query. A consistency loss derived from Euler’s theorem for homogeneous functions keeps their predictions geometrically grounded in the underlying support function, even though no convexity is explicitly enforced.

Both models are trained on examples drawn from the relevant query distribution: for each sampled query, the ground-truth best match is computed once via exhaustive search and stored, creating a supervised dataset that captures the MIPS solution landscape for the distribution of queries that actually matter.

As shown in [Figure 1](https://machinelearning.apple.com/research/faster-retrieval#figure1), the simplest way to handle larger scales is to split the keys into clusters, compare the query to each cluster’s centroid, and route the query to the most promising clusters based on these *k* scores. This provides a cheap yet coarse rule that can be prone to mistakes because a centroid alone cannot account for the spread of its cluster. Indeed, a cluster can hold a key that sits far from its own centroid yet close to the query, and when the query is near such an outlier, centroid routing scores the cluster low and skips it, missing the true best match.

If one were given access to the support function of each cluster, then one would instead be able to do exact routing, since each support function quantifies exactly to what extent any key within each cluster can match the query with a large score. This is illustrated in [Figure 2](https://machinelearning.apple.com/research/faster-retrieval#figure2), in which the support function of each of the four clusters is plotted for a query that varies in the space of embeddings.

Figure 2: Relying on distances to centroids can be misleading as those distances do not account for every key in each cluster. Given access to each cluster’s support function, assignment can be made based on how well a given query matches any key in a cluster.

Leaning on this theoretical insight, we propose to learn these *k* support functions jointly with a trained SupportNet that can output *k* scores at once. We can then use the largest of these *k* scores to route any new query instead. The SupportNet model can answer a more useful question than a simple comparison to the centroid can: “How good is the *best* key inside this cluster for this query?” SupportNets are trained in a classic machine learning fashion, also known as regression: we generate sample queries that are drawn from the distribution (typically embeddings for text queries), compute the ground-truth support functions at those queries, and train a neural network to replicate these values.

Figure 3: SupportNets approximate the support functions of each cluster and assign queries to clusters based on the best match with any key, not only centroids as typical geometric approaches do.

We trained SupportNets of increasing sizes (XS, S, M, and L → 1%, 5%, 10%, and 20% of the database size) on [Natural Questions](https://huggingface.co/datasets/BeIR/nq-generated-queries) partitioned into 128 clusters. Natural Questions is a retrieval benchmark that pairs general-knowledge questions with passages from a large document database, roughly 2.6M unique passages in our setup.

[Figure 4](https://machinelearning.apple.com/research/faster-retrieval#figure4) below shows the retrieval performance in terms of recall as a function of different notions of cost. Routing recall is the fraction of queries whose true partition, the one holding the exact best key, lands among those the router chooses to probe. For both plots, an exhaustive search is carried out in the top-*s* clusters matched with the query, where *s* grows as powers of two *(1, 2, 4, 8, …, 128)*. Naturally, all methods reach a recall of 1 when they survey all 128 clusters.

The first tab shows that for a fixed retrieval performance the learned routers reach it at lower latency than centroid routing. The small SupportNet model hits 95% routing recall about 1.9× faster than centroid routing (1.74 ms vs 3.36 ms), and gains are even greater with the large model, which hits 97% in 1.74 ms. This plot explicitly factors in the additional compute incurred by running our small NNs rather than simply comparing to centroids.

The plot on the second tab looks exclusively at routing quality, neglecting the overhead incurred by running NNs. At a fixed budget of explored clusters, they find the right partition more often: scanning just 5% of the database, the learned routers recover the true partition ~90–93% of the time — up to 92.6% for the mid-size (M) — versus 87.5% for the centroid baseline.

Figure 4: Routing performance (recall) due to SupportNets of various sizes versus latency or the fraction of the database we allow the router to scan. Routers, all with a depth of 16 layers, are trained on a 128-cluster Natural Questions dataset. SupportNets dominate the centroid router: given a latency target, one can get a higher recall with a SupportNet or conversely, that recall can be achieved at lower latency by a SupportNet than by a centroid router.

SupportNet routing replaces the simple centroid-based routing found in existing inverted file index (IVF) systems. With KeyNet, we can also keep the IVF system as is and just feed it a modified version of the query. In that scenario, KeyNet works naturally as a *“query rewriter”* in front of a standard approximate search index: predict where the best match is likely to be in embedding space, then ask your favorite IVF system to look there instead of starting from the original query. Because the IVF index is highly optimized, it can find the result faster when assisted by KeyNet. We also rely on regression to train KeyNets, as summarized in [Figure 5](https://machinelearning.apple.com/research/faster-retrieval#figure5): sample queries are matched with their ground-truth, best-matching key, and the KeyNet neural network is trained to reproduce that association.

Figure 5: KeyNets are trained to map a query to its best match in a database. Combined with an IVF index, pre-processing queries via such a mapping makes search much easier: the resulting transformed query is now much closer to the target it is intended to retrieve.

We evaluated KeyNets of increasing sizes (XS, S, M, and L → 1%, 5%, 10%, and 20% of the database size) and in [Figure 6](https://machinelearning.apple.com/research/faster-retrieval#figure6) below, we show recall–cost tradeoffs for these different sizes. On the first tab, the search budget is wall-clock latency: reaching 99% recall takes up to ~8× less time with the largest model (0.23 ms vs 1.8 ms for the raw query). As before, the second tab focuses exclusively on the boost in “quality” provided by replacing the original query with its KeyNet transformation. The x-axis is the fraction of the database the index actually scans: scanning just 0.001% of the database, KeyNet-mapped queries retrieve the true key 89–97% of the time — up to 96.7% for the largest model (L) — versus 86% for the raw query, and reaching 99% recall takes up to ~11× fewer keys (0.009% vs 0.10% raw). Better recall for the same cost, or the same recall for a fraction of it.

Figure 6: Search performance (recall) using an IVF index to search a database made from the Natural Questions dataset. The plots compare results due to querying the IVF index directly with raw queries against a search executed after mapping queries with KeyNets of various sizes, all with a fixed depth of 16 layers. Latency in the plots corresponds to the total time required to map a query and search, except for the raw-query case, which accounts only for search. Pre-processing queries with KeyNet is worth it no matter the size: the pre-processing overhead is compensated for by faster search downstream.

We show examples obtained from the same [Natural Questions](https://huggingface.co/datasets/BeIR/nq-generated-queries) dataset discussed previously, served through a [FAISS IVF](https://github.com/facebookresearch/faiss) index. It is one of several datasets we studied, and the trend is consistent across them. Querying the index with KeyNet’s predicted key consistently beats querying with the raw question, across the full search-budget sweep.

Namely, [Figure 7](https://machinelearning.apple.com/research/faster-retrieval#figure7) below shows five Natural Questions examples: top-3 hits for the raw query versus the KeyNet-mapped query.

- [Gallery item 1](https://machinelearning.apple.com/research/faster-retrieval#default-item-0)
- [Gallery item 2](https://machinelearning.apple.com/research/faster-retrieval#default-item-1)
- [Gallery item 3](https://machinelearning.apple.com/research/faster-retrieval#default-item-2)
- [Gallery item 4](https://machinelearning.apple.com/research/faster-retrieval#default-item-3)
- [Gallery item 5](https://machinelearning.apple.com/research/faster-retrieval#default-item-4)

![Figure 7:  Top-3 hits for the raw query versus the KeyNet-mapped query: 'What is Matchbox Twenty?'](https://mlr.cdn-apple.com/media/faster_retrieval_fig_7_1_70acc2c0ac.png)

Figure 7: Top-3 hits for the raw query versus the KeyNet-mapped query: 'What is Matchbox Twenty?'

![Figure 7:  Top-3 hits for the raw query versus the KeyNet-mapped query: 'Who wrote The Eye of the World?'](https://mlr.cdn-apple.com/media/faster_retrieval_fig_7_2_c2047149d1.png)

Figure 7: Top-3 hits for the raw query versus the KeyNet-mapped query: 'Who wrote The Eye of the World?'

![Figure 7:  Top-3 hits for the raw query versus the KeyNet-mapped query: 'What is a wind controller used for?'](https://mlr.cdn-apple.com/media/faster_retrieval_fig_7_3_e46c3743fb.png)

Figure 7: Top-3 hits for the raw query versus the KeyNet-mapped query: 'What is a wind controller used for?'

![Figure 7: Top-3 hits for the raw query versus the KeyNet-mapped query: 'When will the Summertime Ball be on?'](https://mlr.cdn-apple.com/media/faster_retrieval_fig_7_4_82a8dc0d4d.png)

Figure 7: Top-3 hits for the raw query versus the KeyNet-mapped query: 'When will the Summertime Ball be on?'

![Figure 7: Top-3 hits for the raw query versus the KeyNet-mapped query: 'When was the show Victorious on Nickelodeon?'](https://mlr.cdn-apple.com/media/faster_retrieval_fig_7_5_168d478540.png)

Figure 7: Top-3 hits for the raw query versus the KeyNet-mapped query: 'When was the show Victorious on Nickelodeon?'

The choice between the two ends up being a practical one. Use **KeyNet** when inference speed is the bottleneck and you just need to approximate the best key. Use **SupportNet** when you also want a calibrated score (for re-ranking, thresholding, or downstream use), or when compute is tight at small model sizes — a regime where SupportNet stays competitive. Both train similarly, integrate with existing approximate-search infrastructure, and are reasonably robust to mild distribution shift in the query patterns. Ultimately, a very large-scale MIPS system for which the set of keys is huge may require both: train a SupportNet router over all clusters, and then one KeyNet per cluster to help its regular indices zero in on the best key faster.

Amortized MIPS reflects a shift in how retrieval can be approached when query distributions are known in advance — a common condition in production ML systems. By encoding MIPS solutions directly into network weights, the approach trades a one-time training (so-called *amortization*) cost for fast, lightweight inference and opens a practical path toward learned retrieval systems explicitly tailored to the queries they serve.

## Related readings and updates.

Maximum inner product search (MIPS) is a crucial subroutine in machine learning, requiring the identification of a vector taken within a database (the keys) that best aligns with a given query. We propose amortized MIPS: a regression-based approach that trains neural networks to directly predict MIPS solutions, amortizing the cost of repeatedly solving MIPS for queries drawn from a known distribution over a fixed key database. Our key insight is…

[Read more](https://machinelearning.apple.com/research/amortizing-inner-product-search)

This paper presents Wally, a private search system that supports efficient semantic and keyword search queries against large databases. When sufficiently many clients are making queries, Wally’s performance is significantly better than previous systems. In previous private search systems, for each client query, the server must perform at least one expensive cryptographic operation per database entry. As a result, performance degraded…

[Read more](https://machinelearning.apple.com/research/wally-search)
