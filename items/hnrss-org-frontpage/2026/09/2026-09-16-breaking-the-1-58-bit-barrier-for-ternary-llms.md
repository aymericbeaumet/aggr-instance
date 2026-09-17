---
title: Breaking the 1.58-bit Barrier for Ternary LLMs
link: https://arxiv.org/abs/2609.16338
source: hnrss-org-frontpage
published: 2026-09-16T20:59:24Z
updated: 2026-09-16T20:59:24Z
first_seen: 2026-09-17T01:40:53.278229139Z
authors:
- matt_d
summary: 'Article URL: https://arxiv.org/abs/2609.16338 Comments URL: https://news.ycombinator.com/item?id=49732931 Points: 138 # Comments: 16'
content: extracted
html: 2026-09-16-breaking-the-1-58-bit-barrier-for-ternary-llms.html
preview:
  file: 2026-09-16-breaking-the-1-58-bit-barrier-for-ternary-llms.preview-41145fe12e79.webp
  width: 256
  height: 149
  alt: arXiv logo
  color: '#ece6e5'
images:
- source: https://arxiv.org/static/browse/0.3.4/images/arxiv-logo-fb.png
  original:
    file: 2026-09-16-breaking-the-1-58-bit-barrier-for-ternary-llms.image-88f5088404c9.png
    width: 1200
    height: 700
  variants:
  - file: 2026-09-16-breaking-the-1-58-bit-barrier-for-ternary-llms.image-fc6386ce1e2e.webp
    width: 320
    height: 187
  - file: 2026-09-16-breaking-the-1-58-bit-barrier-for-ternary-llms.image-49eabc2e18a4.webp
    width: 640
    height: 373
  - file: 2026-09-16-breaking-the-1-58-bit-barrier-for-ternary-llms.image-f275dcf4bd8a.webp
    width: 1200
    height: 700
  color: '#fefefe'
---

## Title:Breaking the 1.58-bit Barrier for Ternary LLMs

[View PDF](https://arxiv.org/pdf/2609.16338) [HTML (experimental)](https://arxiv.org/html/2609.16338v1)

> Abstract:Ternary Large Language Models (LLM) store every weight as one of three symbols $\\{-1,0,+1\\}$, so the cost of a ternary model is conventionally referenced to the information-theoretic $\\log\_2 3 \\approx 1.585$ bits per weight. The prevailing deployment format packs five ternary weights into one byte (five-trit packing), and due to the power-of-two group sizes used in practice this rounds up to $1.625$ bits per weight. This effective storage bit-width treats the three symbols $\\{-1,0,+1\\}$ as equiprobable. We measure the actual symbol distribution of 29 ternary LLM models and find that zeros account for up to $51.5\\%$ of all weights. Motivated by this finding, we introduce BITCOS, a simple distribution-adaptive layout comprised of a dense presence bitmap plus a compacted sign vector, and costs $2 - z$ bits per weight element given a zero density $z$ in the model's weights. BITCOS stores weights more compactly than the five-trit packing in 26 of the 29 tested models, and reaches $1.485$ bits per weight on the sparsest of them. BITCOS is amenable to efficient unpacking on modern processors and GPUs, and we present optimized unpacking sequences for AVX-512, AVX2 and Intel Xe2 GPUs. Measured against production state-of-the-art ternary matrix-vector multiplication kernels, at the zero densities real-world ternary models exhibit, the realized gain with our proposed layout is up to $1.28\\times$. Finally, we illustrate end-to-end LLM inference results on 5 different platforms (client and server CPUs, integrated and discrete Xe2 GPUs) where decode throughput improves by up to $1.18\\times$ on CPUs and $1.27\\times$ on GPUs.

Subjects:

Artificial Intelligence (cs.AI); Machine Learning (cs.LG)

Cite as:

[arXiv:2609.16338](https://arxiv.org/abs/2609.16338) \[cs.AI\]

(or [arXiv:2609.16338v1](https://arxiv.org/abs/2609.16338v1) \[cs.AI\] for this version)

[https://doi.org/10.48550/arXiv.2609.16338](https://doi.org/10.48550/arXiv.2609.16338)

arXiv-issued DOI via DataCite (pending registration)

## Submission history

From: Evangelos Georganas \[[view email](https://arxiv.org/show-email/6908fb0d/2609.16338)\] \
 **\[v1\]** Mon, 14 Sep 2026 20:54:24 UTC (144 KB)
