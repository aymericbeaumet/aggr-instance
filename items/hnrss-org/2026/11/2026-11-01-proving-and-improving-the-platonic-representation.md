---
title: Proving and improving the Platonic Representation Hypothesis with JEPAs
link: https://arxiv.org/abs/2610.39302
source: hnrss-org
published: 2026-11-01T13:24:49Z
updated: 2026-11-01T13:24:49Z
first_seen: 2026-11-02T00:33:20.199752031Z
authors:
- Anon84
content: extracted
html: 2026-11-01-proving-and-improving-the-platonic-representation.html
remote_preview:
  url: https://arxiv.org/static/browse/0.3.4/images/arxiv-logo-fb.png
  alt: arXiv logo
---

[View PDF](https://arxiv.org/pdf/2610.39302) [HTML (experimental)](https://arxiv.org/html/2610.39302v1)

> Abstract:Whether models trained on different modalities converge to the same representation of the world is central to AI. The Platonic Representation Hypothesis (PRH) conjectures that they do as they scale, but without a proof it offers neither a guarantee nor a recipe. We turn the hypothesis into a theorem. First, we prove a \\emph{Platonic Representation Theorem}: Joint-Embedding Predictive Architectures (JEPAs) trained separately on any number of modalities learn the same representation up to one rotation whenever their positive pairs are two observations of one scene--\\emph{good views}--and their misalignment is bounded otherwise. Second, we show that next-token prediction is a JEPA with \\emph{bad views}, parts of a single document, which explains why language models align with vision only through scale. Third, we validate the theory from 145k captions to the texts of 940M web images, and uncover a scaling law of alignment that predicts unseen runs within seed noise. Good views need 42 times fewer training samples than next-token prediction on the same corpus to reach the same alignment, and a 109M-parameter text JEPA aligns with DINOv2 and CLIP's image encoders better than every language model we evaluated, up to 72B parameters, and than CLIP's text encoder: next-token prediction is a remarkably inefficient route to platonic representations. As a showcase, one rotation turns separately trained encoders into an image captioner (CIDEr .838 on COCO). Even that rotation needs no pair: on the latest pair-free benchmark, our text JEPA beats every zero-shot accuracy reported with language models up to 8B parameters, e.g., 64\\% against at most 56\\% top-5 on ImageNet-100. Positive views, and not scale alone, are thus the lever to build platonic representations.

|           |                                                                                                                        |
| --------- | ---------------------------------------------------------------------------------------------------------------------- |
| Subjects: | Artificial Intelligence (cs.AI); Computer Vision and Pattern Recognition (cs.CV); Machine Learning (stat.ML)           |
| Cite as:  | [arXiv:2610.39302](https://arxiv.org/abs/2610.39302) \[cs.AI\]                                                        |
|           | (or [arXiv:2610.39302v1](https://arxiv.org/abs/2610.39302v1) \[cs.AI\] for this version)                              |
|           | [https://doi.org/10.48550/arXiv.2610.39302](https://doi.org/10.48550/arXiv.2610.39302)  arXiv-issued DOI via DataCite |

## Submission history

From: Randall Balestriero \[[view email](https://arxiv.org/show-email/f8c16729/2610.39302)\] \
 **\[v1\]** Thu, 29 Oct 2026 03:49:53 UTC (3,665 KB)
