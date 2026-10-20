---
title: AI models can provably hide arbitrary capabilities
link: https://arxiv.org/abs/2610.13928
source: hnrss-org
published: 2026-10-13T02:07:08Z
updated: 2026-10-13T02:07:08Z
first_seen: 2026-10-13T11:00:50.474439149Z
authors:
- sbulaev
content: extracted
html: 2026-10-13-ai-models-can-provably-hide-arbitrary-capabilities.html
extra:
  comments_url: https://news.ycombinator.com/item?id=50065692
  num_comments: 112
  points: 214
---

[View PDF](https://arxiv.org/pdf/2610.13928) [HTML (experimental)](https://arxiv.org/html/2610.13928v2)

> Abstract:Capability evaluations aim to surface dangerous model behaviors before deployment, but their reliability depends on the assumption that hidden capabilities can be elicited. We show this assumption does not hold under adversarial conditions by constructing backdoor attacks that embed encrypted neural networks within host model weights, decrypting and executing them only upon receiving a secret trigger. Using digital lockers, these hidden circuits remain provably hard to elicit and interpret under cryptographic assumptions, even given full access to model weights, extending prior work from hiding fixed strings to arbitrary computations. We implement our proof of concept constructions in PyTorch and evaluate elicitation through fine-tuning, weight noising, gradient-based trigger search, and latent adversarial training, using a small chemical reaction prediction task as a proxy for CBRN-relevant capabilities. Our results demonstrate resistance to these methods and, together with our cryptographic guarantees, reveal a limit of elicitation-based capability audits. We release our models to support the improvement of defenses.

|           |                                                                                                                        |
| --------- | ---------------------------------------------------------------------------------------------------------------------- |
| Comments: | 22 pages, 6 figures, 3 tables. Accepted at NeurIPS 2026                                                                |
| Subjects: | Machine Learning (cs.LG); Cryptography and Security (cs.CR)                                                            |
| Cite as:  | [arXiv:2610.13928](https://arxiv.org/abs/2610.13928) \[cs.LG\]                                                        |
|           | (or [arXiv:2610.13928v2](https://arxiv.org/abs/2610.13928v2) \[cs.LG\] for this version)                              |
|           | [https://doi.org/10.48550/arXiv.2610.13928](https://doi.org/10.48550/arXiv.2610.13928)  arXiv-issued DOI via DataCite |

## Submission history

From: Andis Draguns \[[view email](https://arxiv.org/show-email/6518b944/2610.13928)\] \
 **[\[v1\]](https://arxiv.org/abs/2610.13928v1)** Fri, 9 Oct 2026 17:55:06 UTC (878 KB)\
 **\[v2\]** Wed, 14 Oct 2026 17:58:11 UTC (541 KB)
