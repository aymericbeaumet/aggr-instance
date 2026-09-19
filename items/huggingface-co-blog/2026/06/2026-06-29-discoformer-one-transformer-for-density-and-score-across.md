---
title: 'DiScoFormer: One transformer for density and score, across distributions'
link: https://huggingface.co/blog/allenai/discoformer
source: huggingface-co-blog
published: 2026-06-29T18:02:48Z
updated: 2026-06-29T18:02:48Z
first_seen: 2026-09-19T21:30:23.395188495Z
content: extracted
html: 2026-06-29-discoformer-one-transformer-for-density-and-score-across.html
preview:
  file: 2026-06-29-discoformer-one-transformer-for-density-and-score-across.preview-58dcc19998b2.webp
  width: 256
  height: 144
  color: '#909a9f'
images:
- source: https://cdn-uploads.huggingface.co/production/uploads/638e39b249de7ae552d977b5/AnOX0pzQm-Hep_CLFOVM7.png
  original:
    file: 2026-06-29-discoformer-one-transformer-for-density-and-score-across.image-4770b21b3f84.png
    width: 1920
    height: 1080
  variants:
  - file: 2026-06-29-discoformer-one-transformer-for-density-and-score-across.image-fd85376981c9.webp
    width: 320
    height: 180
  - file: 2026-06-29-discoformer-one-transformer-for-density-and-score-across.image-b56a9a75c9b7.webp
    width: 640
    height: 360
  - file: 2026-06-29-discoformer-one-transformer-for-density-and-score-across.image-6eb0c79d5968.webp
    width: 960
    height: 540
  - file: 2026-06-29-discoformer-one-transformer-for-density-and-score-across.image-0fd2917ab502.webp
    width: 1280
    height: 720
  - file: 2026-06-29-discoformer-one-transformer-for-density-and-score-across.image-d9686354cb26.webp
    width: 1600
    height: 900
  - file: 2026-06-29-discoformer-one-transformer-for-density-and-score-across.image-ad96b64d673c.webp
    width: 1920
    height: 1080
  color: '#fafafa'
- source: https://cdn-uploads.huggingface.co/production/uploads/638e39b249de7ae552d977b5/RsD3JLEW1EABr7i2WCKlF.png
  original:
    file: 2026-06-29-discoformer-one-transformer-for-density-and-score-across.image-4770b21b3f84.png
    width: 1920
    height: 1080
  variants:
  - file: 2026-06-29-discoformer-one-transformer-for-density-and-score-across.image-fd85376981c9.webp
    width: 320
    height: 180
  - file: 2026-06-29-discoformer-one-transformer-for-density-and-score-across.image-b56a9a75c9b7.webp
    width: 640
    height: 360
  - file: 2026-06-29-discoformer-one-transformer-for-density-and-score-across.image-6eb0c79d5968.webp
    width: 960
    height: 540
  - file: 2026-06-29-discoformer-one-transformer-for-density-and-score-across.image-0fd2917ab502.webp
    width: 1280
    height: 720
  - file: 2026-06-29-discoformer-one-transformer-for-density-and-score-across.image-d9686354cb26.webp
    width: 1600
    height: 900
  - file: 2026-06-29-discoformer-one-transformer-for-density-and-score-across.image-ad96b64d673c.webp
    width: 1920
    height: 1080
  color: '#fafafa'
- source: https://cdn-uploads.huggingface.co/production/uploads/638e39b249de7ae552d977b5/tNRWwYaFG4jyW5f2xWdoY.png
  original:
    file: 2026-06-29-discoformer-one-transformer-for-density-and-score-across.image-a7b3b9fa7748.png
    width: 2048
    height: 1224
  variants:
  - file: 2026-06-29-discoformer-one-transformer-for-density-and-score-across.image-addfce974137.webp
    width: 320
    height: 191
  - file: 2026-06-29-discoformer-one-transformer-for-density-and-score-across.image-9735d28dacba.webp
    width: 640
    height: 383
  - file: 2026-06-29-discoformer-one-transformer-for-density-and-score-across.image-c87f4a5dc1c7.webp
    width: 960
    height: 574
  - file: 2026-06-29-discoformer-one-transformer-for-density-and-score-across.image-973d927059a2.webp
    width: 1280
    height: 765
  - file: 2026-06-29-discoformer-one-transformer-for-density-and-score-across.image-6c4ef43f23d7.webp
    width: 1600
    height: 956
  - file: 2026-06-29-discoformer-one-transformer-for-density-and-score-across.image-6a1732f0a0d7.webp
    width: 2048
    height: 1224
  color: '#f9f1e8'
---

**📄 Tech report:** [arxiv.org/abs/2511.05924](https://arxiv.org/abs/2511.05924)

[![DiScoFormer One Transformer for density and score across distributions - Google -image-1](https://cdn-uploads.huggingface.co/production/uploads/638e39b249de7ae552d977b5/RsD3JLEW1EABr7i2WCKlF.png)](https://cdn-uploads.huggingface.co/production/uploads/638e39b249de7ae552d977b5/RsD3JLEW1EABr7i2WCKlF.png)

Many problems in machine learning and the sciences come down to the same task: you have a collection of data points and want to recover the distribution they came from—which values are common, and which are rare. Pinning down that distribution means estimating two quantities: the distribution's density and, more useful as dimensionality grows, its score. The density is the smooth version of a histogram—high where points cluster and low where they're scarce. The score—the gradient of the log-density—points in the direction the density rises fastest: move a point along the score and it heads toward a more probable region.

Diffusion-based generative models (the technology behind AI image generators like Stable Diffusion and DALL-E) start from random noise and repeatedly follow the score, turning that noise into a realistic image. The same score drives Bayesian sampling and the particle simulations used to model systems such as plasma.

Extracting the density and score from a finite sample is challenging, and today's tools force a trade-off between generalizability and accuracy. One classical approach, kernel density estimation (KDE), computes the density at any location from the data points around it: the closer and more numerous they are, the higher the density. It needs no training and applies to any distribution, but its accuracy falls off sharply as dimensionality grows. Alternatively, neural score-matching models trained to predict the score stay accurate even in high dimensions, but each needs to learn the distribution and must be retrained from scratch for another.

We introduce a new solution called the [DiScoFormer (Density and Score Transformer)](https://arxiv.org/pdf/2511.05924)—one model that, given a set of data points, estimates both the density and the score of the distribution in a single forward pass without retraining.

## [](https://huggingface.co/blog/allenai/discoformer#training-a-transformer-for-density-and-score-estimation) Training a transformer for density and score estimation

[![DiScoFormer One Transformer for density and score across distributions - Google -image-2](https://cdn-uploads.huggingface.co/production/uploads/638e39b249de7ae552d977b5/tNRWwYaFG4jyW5f2xWdoY.png)](https://cdn-uploads.huggingface.co/production/uploads/638e39b249de7ae552d977b5/tNRWwYaFG4jyW5f2xWdoY.png)

DiScoFormer maps an entire sample to the density and score of the distribution behind it using stacked layers of transformer blocks. The model utilizes cross-attention, which allows it to evaluate density and score at any point—not just where you have data. Score and density share a mathematical relationship: score is the gradient of the logarithm of density. We leverage this by having a shared backbone with two output heads, one for the density and one for the score.

This coupling does more than save parameters. The score head has to match the gradient of the log-density head at every query, so any gap between them is a label-free consistency loss. We use this at inference—hold the context fixed, take a few gradient steps on that consistency loss, and DiScoFormer adapts itself to an out-of-distribution input on the spot, no ground-truth density or score required.

There's a mathematical reason why the transformer architecture fits this task. Kernel density estimation has a single bandwidth—how far each point's influence reaches, fixed in advance and applied identically everywhere. Attention is a strict generalization of it: we analytically show that a single attention head's weights are nearly a Gaussian kernel over the data, so one cross-attention block can already reproduce KDE's density and score. From there the model goes further, learning several such scales at once and adapting them to the data. DiScoFormer doesn't discard the classical method for a black box but instead includes KDE as a special case and improves on it.

What data did we use to train DiScoFormer? We relied on Gaussian Mixture Models for two primary reasons. Firstly, GMMs are universal density approximators—with enough components they match essentially any smooth distribution to arbitrarily small error. Secondly, GMMs have closed-form densities and scores, so we always have an exact target to supervise against. We employ both of these properties by drawing a new GMM for every batch, giving the model virtually unlimited examples of target distributions and supervising each against a given GMM's exact density and score.

## [](https://huggingface.co/blog/allenai/discoformer#performance) Performance

Across the board, DiScoFormer beats KDE at both density and score estimation, and the gap widens exactly where KDE struggles. In 100 dimensions, it isn't close—against the best hand-tuned KDE, it cuts score error by about 6.5x and density error by more than 37x, and it keeps improving as you add samples, while KDE runs out of memory. It also travels far outside its training data, staying accurate on mixtures with more modes than it ever saw during training and on non-Gaussian shapes like the Laplace and Student-t. KDE's main advantage remains speed, especially when datasets are small.

The part about DiScoFormer that we find most promising is that score estimation is a shared dependency across many fields, such as generative modeling, Bayesian inference, and scientific computing. A pretrained, plug-in estimator that stays accurate in high dimensions and removes the need to retrain per problem could cut that cost across all of them at once—one model, reused everywhere score and density show up.

We encourage you to [read our technical report](https://arxiv.org/pdf/2511.05924) for more details.
