---
title: Google's DeepCrossAttention achieves comparable model quality up to 3x faster
link: https://research.google/blog/supercharging-transformer-residual-connections/
source: hnrss-org
published: 2026-11-27T23:06:29Z
updated: 2026-11-27T23:06:29Z
first_seen: 2026-11-28T04:41:26.390181955Z
authors:
- ComputedCamp995
content: extracted
html: 2026-11-27-google-s-deepcrossattention-achieves-comparable-model.html
remote_preview:
  url: https://storage.googleapis.com/gweb-research2023-media/images/HO_previewImage1.width-800.format-jpeg.jpg
---

Think of a traditional deep learning model like a giant game of "Telephone": data passes through layer after layer, with each layer changing the information slightly. If a network has dozens or hundreds of layers, the original learning signal can become severely degraded or lost by the time it reaches all layers, which is known as the [vanishing gradient problem](https://en.wikipedia.org/wiki/Vanishing_gradient_problem) in AI.

To overcome this challenge and enable the training of networks with dozens or hundreds of layers, [transformers](https://en.wikipedia.org/wiki/Transformer_\(deep_learning\)) rely heavily on [residual connections](https://arxiv.org/abs/1512.03385), also known as “skip” connections, which act like an information highway or a shortcut. Instead of forcing the data to pass exclusively through the complicated layer, a duplicate copy of the original input skips right past this layer and is added directly to its output. This ensures that the model always has the original raw data to refer back to.

The standard residual connection implements a simple operation: [point-wise summation](https://en.wikipedia.org/wiki/Pointwise) of a layer's output with its input. While computationally efficient, this simple process has a key limitation: it treats all prior representations with equal weight, leading to information dilution in deep networks. As features propagate through many layers, specific, high-resolution details from earlier layers can be averaged out or distorted, making it difficult for the network to selectively retrieve them. This forces the network to recompute them, reducing learning efficiency.

In our [ICML 2025](https://research.google/conferences-and-events/google-at-icml-2025/) paper, ["DeepCrossAttention: Supercharging Transformer Residual Connections](https://arxiv.org/abs/2502.06785)," we studied the shortcomings of the traditional residual architecture and introduced DeepCrossAttention (DCA), which generalizes residual connections by allowing each layer to dynamically select and route features from the history of preceding layers. By allowing the model to dynamically choose which previous layers are most important at any given moment, DCA achieves comparable or improved learning, while reducing the number of parameters needed by up to 30-40%.

## How it works

Instead of treating skip connections as a rigid, additive pipeline, DCA reframes residual learning as a dynamic routing problem. Rather than passing all prior layers forward with equal weight, DCA introduces a lightweight, layer-wise module that decides how to best combine previous embeddings on a per-token basis.

The architecture introduces two primary mechanisms:

- **Dynamic layer routing**: For every token processed, DCA computes a unique set of mixing weights using a static, learnable base combined with an input-dependent projection. This allows the network to adaptively shift its processing structure — ranging from purely sequential to fully parallel — based on the specific context of the token.
- **Depth-wise cross-attention (split Q/K/V)**: While standard transformer attention operates *across tokens* in a sequence, DCA applies attention *across layers* in depth. Each token queries a dynamically weighted mix of all prior layer embeddings, unlocking the model's full historical context without the computational penalty usually required to scale traditional attention.

By allowing the network to dynamically select and retrieve high-resolution features from earlier layers, DCA eliminates the information dilution inherent to classic residual connections.

## Experiments and results

To test whether DCA can overcome information dilution in practice, we evaluated our approach across controlled synthetic setups, large-scale language modeling benchmarks, and image classification tasks.

DCA demonstrated significant improvements in convergence speed, training stability, and final model quality across these experiments.

On the colossal, cleaned version of [Common Crawl's web crawl corpus](https://www.tensorflow.org/datasets/catalog/c4) (C4) and [1 Billion Word Language Model Benchmark](https://www.tensorflow.org/datasets/catalog/lm1b) ([LM1B](https://www.tensorflow.org/datasets/catalog/lm1b)) datasets, DCA reached the performance of standard transformers in up to five times fewer training steps. When trained for the same duration or number of steps, DCA consistently achieved lower perplexity — how well a probability model predicts a sample — than standard residual connections, effectively extracting more representational capacity from the same training budget. To achieve a perplexity of <14 on LM1B, a standard transformer with fixed width required 42 layers, whereas the DCA model achieved a lower perplexity with only 30 layers — a 29% reduction in depth and parameter count.

One of the most common trade-offs in AI is that better models are often much larger and slower to run. DCA adds a tiny number of parameters — less than 0.2% of the total model size — making it negligible in terms of compute and memory footprint, while increasing memory bandwidth, as representations from different layers are presented intact rather than blended together. Interestingly, because DCA can be initialized to behave identically to a standard residual network, it doesn’t require training from scratch: retrofitting DCA onto an already pre-trained transformer and briefly continuing training reduced perplexity by 0.19 on LM1B, compared to just 0.02 for continued standard training.

## A broader shift toward dynamic residual routing

As large language models (LLMs) are used across a broader range of domains, optimizing their training processes and underlying architectures is critical for scaling and unlocking new capabilities. DCA and similar dynamic residual techniques are already seeing significant adoption and success. The rapid emergence of these methodologies highlights dynamic routing as a highly active and promising area of research.

### Attention residuals

Developed by Moonshot AI, [attention residuals](https://arxiv.org/pdf/2603.15031) share DCA's core principles of replacing fixed addition with selective retrieval. Instead of blindly summing every prior layer's output, they use a learnable softmax attention mechanism paired with a layer-specific pseudo-query. This allows each layer to selectively weight and retrieve representations from earlier depths. By preventing the unbounded growth of hidden states and the dilution of early-layer information, this method improves gradient stability and boosts performance on reasoning-heavy tasks like coding and mathematics. To maintain computational efficiency at scale, a "Block AttnRes" variant partitions layers into blocks, recovering the benefits of full attention with significantly less memory overhead. The recent Inkling 2 model developed by Thinking Machines also adopted this design in addition to DCA’s cross attention (Q/K/V split) mechanism.

### Hyper-connections

[Hyper-connections](https://proceedings.iclr.cc/paper_files/paper/2025/file/f1e8ff97057e0c3f6bcb7018c78d4df1-Paper-Conference.pdf) expand the residual stream into multiple parallel channels, offering a form of memory scaling that goes beyond simply increasing model width or depth. They use a fixed-size stack where layer outputs are aggregated and mixed using learnable mixing matrices. However, this unconstrained mixing breaks the traditional identity mapping property of residual connections, which can cause amplification or attenuation of signals, and lead to training instability at scale.

### Manifold-constrained hyper-connections

To address instability in standard hyper-connections, [manifold-constrained hyper-connections](https://arxiv.org/abs/2512.24880) apply specific constraints to the mixing matrices. By forcing these matrices to live on a defined manifold — specifically, constraining them to be doubly stochastic using the [Sinkhorn-Knopp algorithm](https://en.wikipedia.org/wiki/Sinkhorn%27s_theorem) — manifold-constrained hyper-connections restore stable signal propagation. This approach prevents exploding or vanishing gradients while preserving the rich information flow and expressivity of the widened residual streams, allowing models to scale robustly.

## Conclusion

DCA demonstrates that transitioning from static residual connections to dynamic, attention-based routing is a powerful strategy for building more efficient models. By leveraging an expanded residual stream and depth-wise cross-attention, DCA empowers deep networks to selectively retrieve and integrate token embeddings across different layers. We expect that next-generation models will dynamically route and weigh past layer representations based on the specific tokens or inputs being processed. This will allow for the effective use of all the model layers as specialized experts working in tandem, with maximum information sharing and no redundancy, leading to more expressivity and deeper interactions. We look forward to seeing how these multi-depth interactions will shape the next generation of transformer architectures.

## Acknowledgements

*This work was a collaboration between researchers at Google and academic partners, including Mike Heddes, Adel Javanmard, MohammadHossein Bateni, and Vahab Mirrokni, whose contributions were critical for the development of this work.*
