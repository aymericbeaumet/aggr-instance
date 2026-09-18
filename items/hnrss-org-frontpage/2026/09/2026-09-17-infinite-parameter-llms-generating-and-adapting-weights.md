---
title: 'Infinite-Parameter LLMs: Generating and Adapting Weights from Live Data'
link: https://arxiv.org/abs/2609.18842
source: hnrss-org-frontpage
published: 2026-09-17T16:55:14Z
updated: 2026-09-17T16:55:14Z
first_seen: 2026-09-18T01:14:43.309993603Z
authors:
- Betelbuddy
summary: 'Article URL: https://arxiv.org/abs/2609.18842 Comments URL: https://news.ycombinator.com/item?id=49743483 Points: 107 # Comments: 30'
content: extracted
html: 2026-09-17-infinite-parameter-llms-generating-and-adapting-weights.html
preview:
  file: 2026-09-17-infinite-parameter-llms-generating-and-adapting-weights.preview-41145fe12e79.webp
  width: 256
  height: 149
  alt: arXiv logo
  color: '#ece6e5'
images:
- source: https://arxiv.org/static/browse/0.3.4/images/arxiv-logo-fb.png
  original:
    file: 2026-09-17-infinite-parameter-llms-generating-and-adapting-weights.image-88f5088404c9.png
    width: 1200
    height: 700
  variants:
  - file: 2026-09-17-infinite-parameter-llms-generating-and-adapting-weights.image-fc6386ce1e2e.webp
    width: 320
    height: 187
  - file: 2026-09-17-infinite-parameter-llms-generating-and-adapting-weights.image-49eabc2e18a4.webp
    width: 640
    height: 373
  - file: 2026-09-17-infinite-parameter-llms-generating-and-adapting-weights.image-f275dcf4bd8a.webp
    width: 1200
    height: 700
  color: '#fefefe'
---

[View PDF](https://arxiv.org/pdf/2609.18842) [HTML (experimental)](https://arxiv.org/html/2609.18842v1)

> Abstract:The scaling laws hold that a language model grows more capable with more parameters and more training data, and Mixture-of-Experts (MoE) architectures have ridden these laws to remarkable results, activating only a fraction of an enormous stored parameter bank for each token. That success is built on static pretraining data. A deployed model faces a different world, where much of the data that would make it more useful is not in its training set but in the live interaction it is currently handling, such as the facts a user supplies or the corrections they give. A conventional model cannot learn from this data, because its weights are frozen after training. Instead, the knowledge and behaviour supplied at run time are placed in the prompt, by retrieval or instruction, and re-read on every request only to be discarded once the request ends. We ask how an architecture could learn from live interaction by writing it into its weights. Taking inspiration from MoE, we propose the \\textbf{Infinite-Parameter LLM}. A compact hypernetwork turns the data given at run time into a low-rank modulation of a shared base network, so the feed-forward weights are generated from live data rather than stored in a fixed bank. Where prior weight generators read the context once and freeze, we carry a Bayesian belief over the generator's latent code and update it online, so the effective weight is re-derived from that evolving belief as the session proceeds rather than fixed after one read. The stored footprint stays fixed, yet the weights the model can compile are effectively infinite. For the knowledge and behaviour supplied at run time, carrying them in the weights rather than the prompt is amortized in compute, frees the context window, persists across turns, and can generalise better than in-context use. We specify an evaluation protocol that tests exactly this against in-context learning and retrieval.

Subjects:

Artificial Intelligence (cs.AI); Machine Learning (cs.LG)

Cite as:

[arXiv:2609.18842](https://arxiv.org/abs/2609.18842) \[cs.AI\]

(or [arXiv:2609.18842v1](https://arxiv.org/abs/2609.18842v1) \[cs.AI\] for this version)

[https://doi.org/10.48550/arXiv.2609.18842](https://doi.org/10.48550/arXiv.2609.18842)

arXiv-issued DOI via DataCite (pending registration)

## Submission history

From: Jinli Hu Dr \[[view email](https://arxiv.org/show-email/6e1de2cd/2609.18842)\] \
 **\[v1\]** Wed, 16 Sep 2026 15:49:34 UTC (50 KB)
