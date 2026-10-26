---
title: Attention Is Not What You Need (2025)
link: https://arxiv.org/abs/2512.19428
source: hnrss-org
published: 2026-10-25T23:52:05Z
updated: 2026-10-25T23:52:05Z
first_seen: 2026-10-26T14:07:38.108821132Z
authors:
- westurner
content: extracted
html: 2026-10-25-attention-is-not-what-you-need-2025.html
remote_preview:
  url: https://arxiv.org/static/browse/0.3.4/images/arxiv-logo-fb.png
  alt: arXiv logo
---

[View PDF](https://arxiv.org/pdf/2512.19428) [HTML (experimental)](https://arxiv.org/html/2512.19428v1)

> Abstract:We revisit a basic question in sequence modeling: is explicit self-attention actually necessary for strong performance and reasoning? We argue that standard multi-head attention is best seen as a form of tensor lifting: hidden vectors are mapped into a high-dimensional space of pairwise interactions, and learning proceeds by constraining this lifted tensor through gradient descent. This mechanism is extremely expressive but mathematically opaque, because after many layers it becomes very hard to describe the model with a small family of explicit invariants. \
> To explore an alternative, we propose an attention-free architecture based on Grassmann flows. Instead of forming an L by L attention matrix, our Causal Grassmann layer (i) linearly reduces token states, (ii) encodes local token pairs as two-dimensional subspaces on a Grassmann manifold via Plucker coordinates, and (iii) fuses these geometric features back into the hidden states through gated mixing. Information therefore propagates by controlled deformations of low-rank subspaces over multi-scale local windows, so the core computation lives on a finite-dimensional manifold rather than in an unstructured tensor space. \
> On the Wikitext-2 language modeling benchmark, purely Grassmann-based models with 13 to 18 million parameters achieve validation perplexities within about 10 to 15 percent of size-matched Transformers. On the SNLI natural language inference task, a Grassmann-Plucker head on top of DistilBERT slightly outperforms a Transformer head, with best validation and test accuracies of 0.8550 and 0.8538 compared to 0.8545 and 0.8511. We analyze the complexity of Grassmann mixing, show linear scaling in sequence length for fixed rank, and argue that such manifold-based designs offer a more structured route toward geometric and invariant-based interpretations of neural reasoning.

|           |                                                                                                                        |
| --------- | ---------------------------------------------------------------------------------------------------------------------- |
| Subjects: | Machine Learning (cs.LG); Artificial Intelligence (cs.AI); Algebraic Geometry (math.AG)                                |
| Cite as:  | [arXiv:2512.19428](https://arxiv.org/abs/2512.19428) \[cs.LG\]                                                        |
|           | (or [arXiv:2512.19428v1](https://arxiv.org/abs/2512.19428v1) \[cs.LG\] for this version)                              |
|           | [https://doi.org/10.48550/arXiv.2512.19428](https://doi.org/10.48550/arXiv.2512.19428)  arXiv-issued DOI via DataCite |

## Submission history

From: Chong Zhang \[[view email](https://arxiv.org/show-email/92c1f38b/2512.19428)\] \
 **\[v1\]** Mon, 22 Dec 2025 14:29:18 UTC (16 KB)
