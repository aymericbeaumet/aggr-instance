---
title: Language Models Can Rewrite Their Own Weights
link: https://arxiv.org/abs/2611.21885
source: hnrss-org
published: 2026-11-20T16:29:06Z
updated: 2026-11-20T16:29:06Z
first_seen: 2026-11-20T22:37:07.598874728Z
authors:
- lwhsiao
content: extracted
html: 2026-11-20-language-models-can-rewrite-their-own-weights.html
remote_preview:
  url: https://arxiv.org/static/browse/0.3.4/images/arxiv-logo-fb.png
  alt: arXiv logo
---

Authors: [Jiajun Fan](https://arxiv.org/search/cs?searchtype=author&query=Fan,+J), [Neil He](https://arxiv.org/search/cs?searchtype=author&query=He,+N), [Changyi Yang](https://arxiv.org/search/cs?searchtype=author&query=Yang,+C), [Andy Mao](https://arxiv.org/search/cs?searchtype=author&query=Mao,+A), [Xi Zhu](https://arxiv.org/search/cs?searchtype=author&query=Zhu,+X), [Chaoran Cheng](https://arxiv.org/search/cs?searchtype=author&query=Cheng,+C), [Kaiqu Liang](https://arxiv.org/search/cs?searchtype=author&query=Liang,+K), [Shuaike Shen](https://arxiv.org/search/cs?searchtype=author&query=Shen,+S), [Zetian Li](https://arxiv.org/search/cs?searchtype=author&query=Li,+Z), [Ruihan Guo](https://arxiv.org/search/cs?searchtype=author&query=Guo,+R), [Ge Liu](https://arxiv.org/search/cs?searchtype=author&query=Liu,+G)

[View PDF](https://arxiv.org/pdf/2611.21885) [HTML (experimental)](https://arxiv.org/html/2611.21885v2)

> Abstract:A language model is trained once and frozen. Making it better takes another training run, or a prompt, a harness or extra computation at inference that is gone once the task ends. We show a third way: to our knowledge for the first time, a language model rewrites its own weights by its own reasoning and becomes better, with no gradients and no training. We call this self-rewriting: the model is its own optimizer. In earlier work, a frontier model edited a much smaller one; here an open 27B model, given its weights, its graded answers and the ability to run itself, diagnoses its own failure and rewrites the weights behind it. It surpasses itself: its own edit makes it 6.6 points more accurate on problems it never saw. It keeps getting better: it tests its edits on practice problems and submits only what works, each edit improving on the last, and over eight consecutive sessions its accuracy only rises. It repairs a damaged copy of itself, like a broken robot that fixes itself without being rebuilt: the copy solves almost nothing, yet with no retraining and without ever reading the original weights, the model brings it back to over 80% of the original accuracy on image tasks it never saw. And the ability emerges with scale: in one model family, the smaller models make no edit or break themselves, and only the largest improves and repairs itself. Unlike test-time scaling, self-rewriting writes its gain into the weights, at no cost per answer; unlike training, it runs only forward passes, as serving does. The wall between training and inference is a limit of our tools, not a property of intelligence. We believe self-rewriting is a necessary step toward recursive self-improvement: a model that, like a human, does not separate training from inference could keep upgrading itself for life, with no larger model, no harness or prompt engineering, and no pre- or post-training.

|           |                                                                                                                        |
| --------- | ---------------------------------------------------------------------------------------------------------------------- |
| Subjects: | Artificial Intelligence (cs.AI)                                                                                        |
| Cite as:  | [arXiv:2611.21885](https://arxiv.org/abs/2611.21885) \[cs.AI\]                                                        |
|           | (or [arXiv:2611.21885v2](https://arxiv.org/abs/2611.21885v2) \[cs.AI\] for this version)                              |
|           | [https://doi.org/10.48550/arXiv.2611.21885](https://doi.org/10.48550/arXiv.2611.21885)  arXiv-issued DOI via DataCite |

## Submission history

From: Jiajun Fan \[[view email](https://arxiv.org/show-email/5f6c9d60/2611.21885)\] \
 **[\[v1\]](https://arxiv.org/abs/2611.21885v1)** Mon, 16 Nov 2026 20:55:08 UTC (6,363 KB)\
 **\[v2\]** Wed, 18 Nov 2026 08:57:59 UTC (6,379 KB)
