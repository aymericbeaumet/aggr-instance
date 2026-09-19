---
title: 'NeoMME: an efficient Multimodal-native and Multilingual Encoder'
link: https://huggingface.co/blog/Hcompany/neomme
source: huggingface-co-blog
published: 2026-09-03T13:13:48Z
updated: 2026-09-03T13:13:48Z
first_seen: 2026-09-19T21:30:23.395188495Z
content: extracted
html: 2026-09-03-neomme-an-efficient-multimodal-native-and-multilingual.html
preview:
  file: 2026-09-03-neomme-an-efficient-multimodal-native-and-multilingual.preview-fe7bf1ee65b2.webp
  width: 256
  height: 130
  color: '#efedf7'
images:
- source: https://cdn-uploads.huggingface.co/production/uploads/6264f9655f6f2e14d6ac981c/GJ6FUbgFpq1x8RNOqzmz-.webp
  original:
    file: 2026-09-03-neomme-an-efficient-multimodal-native-and-multilingual.image-67de3d6254b8.webp
    width: 1280
    height: 648
  variants:
  - file: 2026-09-03-neomme-an-efficient-multimodal-native-and-multilingual.image-9aff4730e38d.webp
    width: 320
    height: 162
  color: '#fefefe'
- source: https://img.shields.io/badge/Collection-FFD21E?style=for-the-badge&logo=huggingface&logoColor=000
  original:
    file: 2026-09-03-neomme-an-efficient-multimodal-native-and-multilingual.image-51c65a9c6872.png
    width: 128
    height: 28
  variants:
  - file: 2026-09-03-neomme-an-efficient-multimodal-native-and-multilingual.image-ae963f1825e7.webp
    width: 128
    height: 28
  color: '#fed11e'
- source: https://img.shields.io/badge/HF_Paper-FFD21E?style=for-the-badge&logo=huggingface&logoColor=000
  original:
    file: 2026-09-03-neomme-an-efficient-multimodal-native-and-multilingual.image-e59182ea1f7f.png
    width: 109
    height: 28
  variants:
  - file: 2026-09-03-neomme-an-efficient-multimodal-native-and-multilingual.image-92e245ebc541.webp
    width: 109
    height: 28
  color: '#fed11e'
- source: https://img.shields.io/badge/arXiv-2609.01657-b31b1b.svg?style=for-the-badge
  original:
    file: 2026-09-03-neomme-an-efficient-multimodal-native-and-multilingual.image-25c4567f38d6.png
    width: 165
    height: 28
  variants:
  - file: 2026-09-03-neomme-an-efficient-multimodal-native-and-multilingual.image-301cb7b44c03.webp
    width: 165
    height: 28
  color: '#b31b1b'
- source: https://github.com/tonywu71/colpali-cookbooks/blob/6ef1332da6bcb48c7ef1f19b25bfa555be7031a8/assets/neomme/neomme_logo.webp?raw=true
  original:
    file: 2026-09-03-neomme-an-efficient-multimodal-native-and-multilingual.image-d7a055795ce4.webp
    width: 2048
    height: 738
  variants:
  - file: 2026-09-03-neomme-an-efficient-multimodal-native-and-multilingual.image-9dc819bc5b4d.webp
    width: 320
    height: 115
  color: '#160969'
- source: https://github.com/tonywu71/colpali-cookbooks/blob/481c6ec8866f6f5d39d2d9dd06b5547c2d9a67fa/assets/neomme/architecture.webp?raw=true
  original:
    file: 2026-09-03-neomme-an-efficient-multimodal-native-and-multilingual.image-e901e045f8e3.webp
    width: 4644
    height: 1975
  variants:
  - file: 2026-09-03-neomme-an-efficient-multimodal-native-and-multilingual.image-8a038d714a30.webp
    width: 320
    height: 136
  - file: 2026-09-03-neomme-an-efficient-multimodal-native-and-multilingual.image-90a6fcb3cc76.webp
    width: 640
    height: 272
  - file: 2026-09-03-neomme-an-efficient-multimodal-native-and-multilingual.image-a4c2825f0e6a.webp
    width: 960
    height: 408
  color: '#fcfbf7'
- source: https://github.com/tonywu71/colpali-cookbooks/blob/481c6ec8866f6f5d39d2d9dd06b5547c2d9a67fa/assets/neomme/encoder-stack.webp?raw=true
  original:
    file: 2026-09-03-neomme-an-efficient-multimodal-native-and-multilingual.image-12e95fa81472.webp
    width: 2813
    height: 781
  variants:
  - file: 2026-09-03-neomme-an-efficient-multimodal-native-and-multilingual.image-f64d21008222.webp
    width: 320
    height: 89
  - file: 2026-09-03-neomme-an-efficient-multimodal-native-and-multilingual.image-a1d7f8d7cfc6.webp
    width: 640
    height: 178
  color: '#fcfaf7'
- source: https://github.com/tonywu71/colpali-cookbooks/blob/481c6ec8866f6f5d39d2d9dd06b5547c2d9a67fa/assets/neomme/masked-diffusion.webp?raw=true
  original:
    file: 2026-09-03-neomme-an-efficient-multimodal-native-and-multilingual.image-d4bb892d66d7.webp
    width: 1920
    height: 572
  variants:
  - file: 2026-09-03-neomme-an-efficient-multimodal-native-and-multilingual.image-8bedac002177.webp
    width: 320
    height: 95
  - file: 2026-09-03-neomme-an-efficient-multimodal-native-and-multilingual.image-6b83b761b358.webp
    width: 640
    height: 191
  color: '#fbfaf6'
- source: https://github.com/tonywu71/colpali-cookbooks/blob/481c6ec8866f6f5d39d2d9dd06b5547c2d9a67fa/assets/neomme/retrieval-heads.webp?raw=true
  original:
    file: 2026-09-03-neomme-an-efficient-multimodal-native-and-multilingual.image-ef84dd717800.webp
    width: 2550
    height: 744
  variants:
  - file: 2026-09-03-neomme-an-efficient-multimodal-native-and-multilingual.image-0614a65c8497.webp
    width: 320
    height: 93
  - file: 2026-09-03-neomme-an-efficient-multimodal-native-and-multilingual.image-3089d453517e.webp
    width: 640
    height: 187
  color: '#fdfbf7'
- source: https://github.com/tonywu71/colpali-cookbooks/blob/c5e737e7e474363822b6f2d95290c0120e0fa993/assets/neomme/vidore-v3-model-size.webp?raw=true
  original:
    file: 2026-09-03-neomme-an-efficient-multimodal-native-and-multilingual.image-04f74cf24557.webp
    width: 2223
    height: 1263
  variants:
  - file: 2026-09-03-neomme-an-efficient-multimodal-native-and-multilingual.image-4b7c460aa1d9.webp
    width: 320
    height: 182
  - file: 2026-09-03-neomme-an-efficient-multimodal-native-and-multilingual.image-d242f0d04e08.webp
    width: 640
    height: 364
  - file: 2026-09-03-neomme-an-efficient-multimodal-native-and-multilingual.image-e053e497e500.webp
    width: 960
    height: 545
  color: '#fdfdfd'
- source: https://github.com/tonywu71/colpali-cookbooks/blob/481c6ec8866f6f5d39d2d9dd06b5547c2d9a67fa/assets/neomme/compression-frontier-260m.webp?raw=true
  original:
    file: 2026-09-03-neomme-an-efficient-multimodal-native-and-multilingual.image-a9ee2f282f40.webp
    width: 1742
    height: 1021
  variants:
  - file: 2026-09-03-neomme-an-efficient-multimodal-native-and-multilingual.image-ae57fb65a14c.webp
    width: 320
    height: 188
  - file: 2026-09-03-neomme-an-efficient-multimodal-native-and-multilingual.image-aad6b9757b36.webp
    width: 640
    height: 375
  color: '#fdfdfc'
- source: https://github.com/tonywu71/colpali-cookbooks/blob/c5e737e7e474363822b6f2d95290c0120e0fa993/assets/neomme/indexing-throughput-l40s.webp?raw=true
  original:
    file: 2026-09-03-neomme-an-efficient-multimodal-native-and-multilingual.image-0d3e50ba2b4d.webp
    width: 3035
    height: 1595
  variants:
  - file: 2026-09-03-neomme-an-efficient-multimodal-native-and-multilingual.image-bf14e0883e49.webp
    width: 320
    height: 168
  - file: 2026-09-03-neomme-an-efficient-multimodal-native-and-multilingual.image-0411cbea71fb.webp
    width: 640
    height: 336
  - file: 2026-09-03-neomme-an-efficient-multimodal-native-and-multilingual.image-02b6838faed8.webp
    width: 960
    height: 505
  - file: 2026-09-03-neomme-an-efficient-multimodal-native-and-multilingual.image-0d5bb4445e91.webp
    width: 1280
    height: 673
  color: '#fdfdfd'
---

[![Hugging Face](https://img.shields.io/badge/Collection-FFD21E?style=for-the-badge&logo=huggingface&logoColor=000)](https://hf.co/collections/Hcompany/neomme) [![Hugging Face Paper](https://img.shields.io/badge/HF_Paper-FFD21E?style=for-the-badge&logo=huggingface&logoColor=000)](https://huggingface.co/papers/2609.01657) [![arXiv](https://img.shields.io/badge/arXiv-2609.01657-b31b1b.svg?style=for-the-badge)](https://arxiv.org/abs/2609.01657)

![NeoMME logo](https://github.com/tonywu71/colpali-cookbooks/blob/6ef1332da6bcb48c7ef1f19b25bfa555be7031a8/assets/neomme/neomme_logo.webp?raw=true)

## [](https://huggingface.co/blog/Hcompany/neomme#tldr) TL;DR

We introduce ***NeoMME***, a family of 260M and 800M multilingual multimodal encoders. Unlike many generative visual language models, *NeoMME* does not use a separate pretrained vision tower or a causal language model. A single bidirectional Transformer processes both text tokens and raw image patches, and we train the entire model from scratch with a masked discrete-diffusion objective.

We fine-tuned *NeoMME* for visual document retrieval using ColPali's page-image approach. *NeoMME*-Retriever returns dense and late-interaction embeddings in one forward pass. Both model sizes lie on the ViDoRe v3 Pareto frontier for nDCG@10 and model size. At a matched 2048×2048 image input size on an NVIDIA L40S GPU, the 260M model encodes about 51 pages per second, or about twice ColModernVBERT's throughput. Hierarchical token pooling and asymmetric quantization reduce late-interaction index storage from roughly 1.5 MB to 6 kB per page (255× smaller) while retaining more than 95% of baseline nDCG@10.

*NeoMME* is available in Hugging Face Transformers. We release all model checkpoints under the Apache 2.0 license.

- 🤗 [*NeoMME* collection](https://hf.co/collections/Hcompany/neomme)
- 📄 [Technical report](https://huggingface.co/papers/2609.01657)
- 🔎 [Visual RAG demo](https://huggingface.co/spaces/tonywu71/neomme-retriever-demo)

## [](https://huggingface.co/blog/Hcompany/neomme#why-another-multimodal-encoder) Why another multimodal encoder?

Many recent visual document retrievers are adapted from pretrained generative visual language models. A separately pretrained vision encoder produces visual features, which a projector maps into the language model's input space. A causal decoder then processes the combined image and text representations. Retrieval, classification, and token labeling do not generate text autoregressively, so they do not require a causal decoder or the parameter and compute overhead of this architecture.

[ModernBERT](https://huggingface.co/blog/modernbert) brought efficient architecture and training improvements to bidirectional encoders. For visual document retrieval, [ModernVBERT](https://huggingface.co/blog/paultltc/modernvbert) applied a bidirectional ModernBERT-style text encoder while retaining a separate pretrained SigLIP2 vision tower. We wanted to push this even further by designing and training a multimodal encoder without having to carry over the parameter and compute overhead of a VLM.

***NeoMME*** (pronounced "nee-oh-me", IPA /ˈniː.oʊ.mi/) is a multilingual, multimodal foundation encoder that generates vector representations for input text and/or images using a single Transformer encoder. It is not based on an existing pretrained vision tower, text encoder, or text decoder.

![Comparison of dual-tower, VLM, ModernVBERT, and NeoMME input paths](https://github.com/tonywu71/colpali-cookbooks/blob/481c6ec8866f6f5d39d2d9dd06b5547c2d9a67fa/assets/neomme/architecture.webp?raw=true)

Unlike dual-tower and VLM encoders, *NeoMME* processes image patches and text tokens in one bidirectional Transformer, without a pretrained vision tower or a pretrained text encoder or decoder.

Images and text use the same computational path, so *NeoMME* can more easily support pretraining, fine-tuning, parallelization, and serving across both modalities.

## [](https://huggingface.co/blog/Hcompany/neomme#neomme-encoder-backbone) *NeoMME* encoder backbone

### [](https://huggingface.co/blog/Hcompany/neomme#one-transformer-for-images-and-text) One Transformer for images and text

*NeoMME* comes in two sizes, [260M](https://huggingface.co/Hcompany/NeoMME-260M) and [800M](https://huggingface.co/Hcompany/NeoMME-800M). Both variants share the same architecture:

- **Native multimodal inputs:** text inputs use factorized token embeddings, while images are divided into a grid of non-overlapping 32×32 patches and projected with a small MLP. Both enter the same Transformer encoder.
- **Dynamic image resolution:** images keep their aspect ratio and size. This allows the model to use more tokens on a high-resolution, information-dense document page than on a smaller image with less content.
- **Long bidirectional context:** both models have a context length of 16,384 tokens (enough for up to two standard 3840×2160 4K UHD images). Most layers use symmetric sliding-window attention, while every sixth layer and the final layer use global attention.
- **A modern encoder stack:** *NeoMME* uses recent encoder improvements such as grouped-query attention, query-key normalization, gated attention, 2D rotary position embeddings, and squared-ReLU MLPs, among others.
- **Multilingual text:** we trained a BPE tokenizer with a 131k-token vocabulary from scratch on multilingual text, code, mathematics, and machine-produced image transcripts.

![Alternating sliding-window and global-attention layers in NeoMME](https://github.com/tonywu71/colpali-cookbooks/blob/481c6ec8866f6f5d39d2d9dd06b5547c2d9a67fa/assets/neomme/encoder-stack.webp?raw=true)

Alternating sliding-window and global-attention layers in the *NeoMME* encoder stack.

### [](https://huggingface.co/blog/Hcompany/neomme#learning-from-images-through-masked-text) Learning from images through masked text

We pretrain *NeoMME* from scratch as a discrete masked-diffusion text denoiser. For each text-only example, we sample a corruption rate uniformly between 0 and 1. Each eligible text token is then independently masked at that rate.

Multimodal examples use corruption rates between 0.3 and 1. The image patches remain visible while *NeoMME* reconstructs masked text. With light masking, the model can often recover a missing word from the surrounding text alone. For example, "cat" is a plausible completion of "The \[MASK\] sat on the mat," even without an image. But high masking forces the model to learn image-grounded descriptions with little to no signal from the non-masked input text tokens.

![Effect of text corruption on the textual and visual evidence available to NeoMME](https://github.com/tonywu71/colpali-cookbooks/blob/481c6ec8866f6f5d39d2d9dd06b5547c2d9a67fa/assets/neomme/masked-diffusion.webp?raw=true)

Higher text corruption removes language-only shortcuts and encourages *NeoMME* to use visible image evidence.

Pretraining mixes multilingual text, code, mathematics, natural images, and document images. Each model processes about 524 billion packed input tokens, including 290 billion tokens from text-only examples. This text budget is relatively small compared with ModernBERT's 2 trillion training token budget. Hence, we chose the NorMuon optimizer to improve data efficiency during training.

## [](https://huggingface.co/blog/Hcompany/neomme#neomme-retriever) *NeoMME*-Retriever

To get a meaningful downstream evaluation of the backbone, we fine-tune *NeoMME* for visual document retrieval using the page-image methodology introduced by [ColPali](https://huggingface.co/blog/manu/colpali). While traditional text-based retrieval consists of retrieving text chunks, *NeoMME*-Retriever ranks document page screenshots and bypasses all the preprocessing OCR steps necessary to extract text from PDFs. Treating the pages as images preserves layout, charts, tables, font type and size, and other visual clues that cannot be captured even by a perfect OCR model.

### [](https://huggingface.co/blog/Hcompany/neomme#a-dual-head-design-for-dense-and-late-interaction-retrieval) A dual-head design for dense and late-interaction retrieval

*NeoMME*-Retriever reuses the *NeoMME* backbone but adds two jointly trained heads on top of it for retrieval:

- The dense head averages the backbone's hidden state vectors into a normalized vector (mean pooling). Dense embeddings are most common today: they are compact and work naturally with approximate nearest-neighbor (ANN) techniques for fast retrieval.
- The late-interaction head projects each text token or image patch from the backbone's output hidden states to a 128-dimensional normalized vector. Compared to dense embeddings, the finer granularity preserves local matches between individual query tokens and image regions.

![Late-interaction and dense retrieval heads for NeoMME](https://github.com/tonywu71/colpali-cookbooks/blob/481c6ec8866f6f5d39d2d9dd06b5547c2d9a67fa/assets/neomme/retrieval-heads.webp?raw=true)

Late-interaction and dense retrieval heads for both *NeoMME* model sizes.

> Omar Khattab, who introduced late-interaction in ColBERT, [explains why](https://x.com/lateinteraction/status/1894696983077785980) the term is more precise than "multi-vector." It describes the granularity and learnability of the scoring function, not simply the number of stored vectors.
>
> To learn more about late-interaction, we recommend reading [this crash course](https://meet.ameliechatelain.com/lectures/multi-vector-search/) by Amélie Chatelain.

One *NeoMME*-Retriever forward pass returns both representations, which gives you flexibility no matter your use case and infrastructure. We recommend using late-interaction embeddings in general since they are more powerful and can be used easily with open-source libraries like [NextPlaid](https://github.com/lightonai/next-plaid). However, if you have a very large corpora, you can run a single forward pass with *NeoMME*-Retriever to get the dense embedding, retrieve a small number of documents through an ANN index, and then use late-interaction to rerank the retrieved candidates.

### [](https://huggingface.co/blog/Hcompany/neomme#competitive-retrieval-at-compact-model-sizes) Competitive retrieval at compact model sizes

We report nDCG@10 on ViDoRe v3. *NeoMME*-Retriever-260M reaches 0.523, the highest score among evaluated models strictly below 800M parameters. It is within 0.002 nDCG@10 of ColQwen2.5 while using about 14× fewer parameters. *NeoMME*-Retriever-800M reaches 0.556, within 0.009 nDCG@10 of the similarly sized [Vultron Retriever Flash (0.8B)](https://huggingface.co/vultr/VultronRetrieverFlash-Qwen3.5-0.8B). Both *NeoMME*-Retriever models lie on the model-size Pareto frontier.

![ViDoRe v3 nDCG@10 versus model size](https://github.com/tonywu71/colpali-cookbooks/blob/c5e737e7e474363822b6f2d95290c0120e0fa993/assets/neomme/vidore-v3-model-size.webp?raw=true)

ViDoRe v3 nDCG@10 versus model size.

ViDoRe v1 and v2 use nDCG@5. On both benchmarks, *NeoMME*-Retriever-260M outperforms ColModernVBERT and the twice-larger ColSmol-500M. *NeoMME*-Retriever-800M outperforms ColPali v1.3 while using 3.6 times fewer parameters.

Visual document retrieval performance on the ViDoRe benchmarks.
| Model details                                                                      | ViDoRe (nDCG@*k*) |           |           |           |
| ---------------------------------------------------------------------------------- | ----------------- | --------- | --------- | --------- |
| [ColModernVBERT](https://huggingface.co/ModernVBERT/colmodernvbert)               | 250M              | 0.261†    | 0.407‡    | 0.806‡    |
| [ColSmol-256M](https://huggingface.co/vidore/colSmol-256M)†                       | 256M              | 0.207     | 0.348     | 0.797     |
| [*NeoMME*-260M](https://huggingface.co/Hcompany/NeoMME-260M-Retriever)‡           | 260M              | **0.523** | **0.522** | **0.860** |
| [ColSmol-500M](https://huggingface.co/vidore/colSmol-500M)                        | 500M              | 0.340‡    | 0.455†    | 0.825†    |
| [Vultron Flash](https://huggingface.co/vultr/VultronRetrieverFlash-Qwen3.5-0.8B)† | 850M              | **0.565** | **0.604** | **0.882** |
| [*NeoMME*-800M](https://huggingface.co/Hcompany/NeoMME-800M-Retriever)‡           | 800M              | 0.556     | 0.559     | 0.874     |
| [ColQwen2.5-v0.2](https://huggingface.co/vidore/colqwen2.5-v0.2)†                 | 3.75B             | **0.524** | **0.601** | **0.895** |
| [ColPali v1.3](https://huggingface.co/vidore/colpali-v1.3)†                       | 2.92B             | 0.430     | 0.547     | 0.848     |

† Scores from MTEB. ‡ Results from our own evaluations.

### [](https://huggingface.co/blog/Hcompany/neomme#making-high-resolution-retrieval-practical-for-late-interaction) Making high-resolution retrieval practical for late-interaction

Late-interaction storage scales linearly with the number of vectors in the output embedding. Higher-resolution images contain more patches, so they produce larger embeddings. For example, a 2048×2048 square page produces embeddings containing 4,200 vectors with *NeoMME*-Retriever, or about 2.1 MB in float32. Across the ViDoRe v3 benchmark, the measured average is about 1.5 MB per document.

To reduce the storage footprint of the late-interaction index, we combine two complementary compression methods:

1. [Hierarchical token pooling](https://www.answer.ai/posts/colbert-pooling.html) clusters similar document vectors in a given multi-vector embedding and replaces each cluster with its mean, hence reducing the number of vectors stored for each page.
2. [Asymmetric quantization](https://www.mixedbread.com/blog/asymmetric-quant) quantizes document embeddings to int8 or binary. Because query embeddings are not stored and only generated on-the-fly, they can be kept at a higher precision.

We tested this setup on ViDoRe v3. With a pooling factor 10 and int8 queries and documents, storage decreased from about 1.5 MB to 39 kB per page, a 39× reduction, while keeping more than 99% of the baseline nDCG@10. A more aggressive configuration uses pooling factor 8, int8 queries, and binary documents. That version uses 6 kB per page (255× smaller) and keeps more than 95% of the original retrieval quality.

![Quality and storage frontier for the NeoMME-260M late-interaction index](https://github.com/tonywu71/colpali-cookbooks/blob/481c6ec8866f6f5d39d2d9dd06b5547c2d9a67fa/assets/neomme/compression-frontier-260m.webp?raw=true)

Quality and storage frontier for the *NeoMME*-260M late-interaction index on ViDoRe v3. Labels show pool factor, retained quality, compression, and storage.

Users can pick a compression setting from that frontier based on storage budget and required retrieval quality.

#### [](https://huggingface.co/blog/Hcompany/neomme#fast-inference-for-cheaper-multimodal-corpus-indexing) Fast inference for cheaper multimodal corpus indexing

Before you can search a corpus, a retriever model must turn your documents into embeddings, which will be stored in a vector store like Qdrant, Weaviate, or Milvus. Faster encoding makes building and adding new documents to the index faster, thus reducing the GPU uptime and compute cost required.

So we measured image encoding speeds for *NeoMME*-Retriever against other multimodal document retrievers. We used preprocessed image tensors and calibrated the batch size separately for each model and image size. At a matched 2048×2048 input size on one NVIDIA L40S, *NeoMME*-Retriever-260M encodes about 51 pages per second, nearly twice ColModernVBERT's 26 pages per second. Both 260M and 800M *NeoMME*-Retriever models are also faster than the other models we compared on smaller input images.

![Document-encoding throughput across image resolutions on an NVIDIA L40S](https://github.com/tonywu71/colpali-cookbooks/blob/c5e737e7e474363822b6f2d95290c0120e0fa993/assets/neomme/indexing-throughput-l40s.webp?raw=true)

Document-encoding throughput by retriever and input resolution on one NVIDIA L40S.

### [](https://huggingface.co/blog/Hcompany/neomme#try-neomme-retriever-yourself) Try *NeoMME*-Retriever yourself!

*NeoMME*-Retriever ([260M](https://huggingface.co/Hcompany/NeoMME-260M-Retriever) and [800M](https://huggingface.co/Hcompany/NeoMME-800M-Retriever)) returns dense and multi-vector embeddings together. The example below scores two text queries against two document-page images with MeanMaxSim late interaction and dense cosine similarity.

Click to see the complete 🤗 `transformers` example snippet

```
# accelerate is an optional dependency needed only when using device_map="auto".
pip install -U accelerate "transformers @ git+https://github.com/huggingface/transformers.git@main" "sentence-transformers>=6.0.0"
```

```
from typing import Any, Literal

import requests
import torch
from PIL import Image
from sentence_transformers.util import cos_sim, mean_maxsim

from transformers import BatchFeature, NeoMMEForRetrieval, NeoMMEProcessor


def encode(
    messages: list[list[dict[str, Any]]],
    task: Literal["query", "document"],
) -> BatchFeature:
    return processor.apply_chat_template(
        messages,
        task=task,
        tokenize=True,
        return_dict=True,
        return_tensors="pt",
        processor_kwargs={"padding": "longest"},
    )


model_name = "Hcompany/NeoMME-260M-Retriever"
processor = NeoMMEProcessor.from_pretrained(model_name)
model = NeoMMEForRetrieval.from_pretrained(model_name, device_map="auto")

# Document images (our corpus)
image_urls = [
    "https://github.com/tonywu71/colpali-cookbooks/blob/6ef1332da6bcb48c7ef1f19b25bfa555be7031a8/examples/data/shift_kazakhstan.jpg?raw=true",
    "https://github.com/tonywu71/colpali-cookbooks/blob/6ef1332da6bcb48c7ef1f19b25bfa555be7031a8/examples/data/energy_electricity_generation.jpg?raw=true",
]
documents = [Image.open(requests.get(url, stream=True).raw) for url in image_urls]

# Queries
queries = [
    "Quelle partie de la production pétrolière du Kazakhstan provient de champs en mer ?",
    "Which hour of the day had the highest overall electricity generation in 2019?",
]

document_messages = [
    [{"role": "user", "content": [{"type": "image", "image": document}]}] for document in documents
]
query_messages = [[{"role": "user", "content": query}] for query in queries]

inputs_documents = encode(document_messages, "document").to(model.device)
inputs_text = encode(query_messages, "query").to(model.device)

with torch.inference_mode():
    document_outputs = model(**inputs_documents)
    query_outputs = model(**inputs_text)

late_scores = mean_maxsim(
    query_outputs.embeddings,
    document_outputs.embeddings,
    a_mask=inputs_text["attention_mask"],
    b_mask=inputs_documents["attention_mask"],
)
dense_scores = cos_sim(query_outputs.dense_embeddings, document_outputs.dense_embeddings)

# Expected: late_scores[0, 0] > late_scores[0, 1] and late_scores[1, 1] > late_scores[1, 0].
print(late_scores, dense_scores)
```

### [](https://huggingface.co/blog/Hcompany/neomme#fine-tuning-with-sentence-transformers) Fine-tuning with Sentence Transformers

We provide separate [dense](https://huggingface.co/Hcompany/NeoMME-260M-Retriever-ST-dense) and [late-interaction](https://huggingface.co/Hcompany/NeoMME-260M-Retriever-ST-late) checkpoints for fine-tuning with [Sentence Transformers v6](https://huggingface.co/blog/multi-vector-encoder). Following the same pattern as text encoders such as ModernBERT, Sentence Transformers loads the backbone through [`NeoMMEModel`](https://huggingface.co/docs/transformers/main/en/model_doc/neomme#transformers.NeoMMEModel) rather than the dual-head [`NeoMMEForRetrieval`](https://huggingface.co/docs/transformers/main/en/model_doc/neomme#transformers.NeoMMEForRetrieval) class. Sentence Transformers currently supports one retrieval head per model, so each checkpoint lets you fine-tune the dense or late-interaction head independently. To train both heads together, use `NeoMMEForRetrieval` with a custom `Trainer`.

### [](https://huggingface.co/blog/Hcompany/neomme#from-retrieval-to-visual-rag) From retrieval to visual RAG

Visual document retrieval can be used as the first stage of a visual retrieval-augmented generation (RAG) system. Unlike text RAG, which retrieves extracted text chunks, visual RAG retrieves the original page images and sends them to a visual language model. The model can then use tables, plots, diagrams, and page layout that text extraction may flatten or omit. Here is how visual RAG works:

1. Indexing: Convert each PDF page to an image, generate an embedding with a retrieval model, and store the embeddings in a vector store.
2. Retrieval: Generate an embedding for the user's query with the same model and retrieve the top-k most relevant pages.
3. Generation: Append the images after the query in the chat message (*e.g.*, `{query}{img_1}{img_2}...{img_k}`) and send it to a VLM to generate the answer.

You can test visual RAG directly with *NeoMME*-Retriever in our HF Space: 🤗 [tonywu71/neomme-retriever-demo](https://huggingface.co/spaces/tonywu71/neomme-retriever-demo).

## [](https://huggingface.co/blog/Hcompany/neomme#conclusion) Conclusion

*NeoMME* replaces separate pretrained image and text encoders with one long-context bidirectional Transformer. We train it from scratch to process both multilingual text tokens and raw 32×32 image patches.

*NeoMME*-Retriever is a fine-tuned version of *NeoMME* for visual document retrieval. One forward pass produces both dense and late-interaction representations. The 260M model outperforms all evaluated models strictly below 800M parameters and, at a matched 2048×2048 input size, encodes pages at about 2× ColModernVBERT's throughput. To reduce the large storage footprint of late-interaction embeddings for high-resolution documents, we experimented with hierarchical token pooling and asymmetric quantization and managed to reduce the late-interaction embeddings from roughly 1.5 MB to 6 kB per page, a 255× compression, while retaining more than 95% of the baseline nDCG@10.

We release all *NeoMME* model checkpoints and a day-zero Hugging Face Transformers implementation to allow practitioners to build efficient multimodal and multilingual representation models on top of our work.

## [](https://huggingface.co/blog/Hcompany/neomme#acknowledgements) Acknowledgements

*NeoMME* began as a side quest between two good friends. We worked with limited time and compute, and we decided to share the results so the community can build on them. We thank H Company for supporting the work and providing the compute used to train *NeoMME*.

## [](https://huggingface.co/blog/Hcompany/neomme#citation) Citation

```
@misc{lac2026neommesingletowermultimodalnativemultilingual,
      title={NeoMME: A Single-Tower Multimodal-Native Multilingual Foundation Encoder for Efficient Fine-Tuning and Inference},
      author={Aurélien Lac and Tony Wu},
      year={2026},
      eprint={2609.01657},
      archivePrefix={arXiv},
      primaryClass={cs.IR},
      url={https://arxiv.org/abs/2609.01657},
}
```
