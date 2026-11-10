---
title: 'E-means: Outperforming k-means in one-tenth the epochs'
link: https://e-means.github.io/
source: hnrss-org
published: 2026-11-09T18:45:43Z
updated: 2026-11-09T18:45:43Z
first_seen: 2026-11-10T03:25:21.974411074Z
authors:
- porridgeraisin
content: extracted
html: 2026-11-09-e-means-outperforming-k-means-in-one-tenth-the-epochs.html
remote_preview:
  url: https://e-means.github.io/assets/emeans_vs_lloyd_readme_v9.png
  alt: e-means against Lloyd’s k-means, in epochs, CPU time and GPU time
extra:
  content:interactive: true
---

I’m excited to open-source e-means: a modern, robust k-means algorithm developed at Meta FAIR. A replacement of Lloyd's algorithm, e-means delivers a ~10× speedup in both epochs and wall-clock time while achieving lower loss.

## An order of magnitude less epochs

Across a [large range of scales and datasets](https://e-means.github.io/#validation), e-means reaches a test loss below Lloyd's at 25 epochs, in 1.2 to 3 epochs. Measured end to end, that is a 10–45× wall-clock reduction on GPU against the state-of-the-art Lloyd implementation (Flash-KMeans[^2]), and 8–15× on CPU against Faiss[^1]. Speed and quality usually trade against each other in k-means, while e-means achieves both better speed and loss.

![e-means against Lloyd’s k-means, in epochs, CPU time and GPU time](https://e-means.github.io/assets/emeans_vs_lloyd_readme_v9.png)

## The k-means problem

Given a fixed dataset of n points X = {x1, ..., xn} ⊂ Rd and an integer k, the k-means problem is to find k centroids C = (c1, ..., ck) minimizing the mean squared distance between each point and its nearest centroid:

\\\[ \\mathrm{MSE}(X, C) = \\frac{1}{n} \\sum\_{i=1}^{n} \\|x\_i - c\_{a(i)}\\|^2, \\qquad a(i) = \\arg\\min\_{j \\in \\{1,\\dots,k\\}} \\|x\_i - c\_j\\|^2 \\\]

\\\[ \\mathrm{MSE}(X, C) = \\frac{1}{n} \\sum\_{i=1}^{n} \\|x\_i - c\_{a(i)}\\|^2 \\\]

\\\[ a(i) = \\arg\\min\_{j \\in \\{1,\\dots,k\\}} \\|x\_i - c\_j\\|^2 \\\]

It's a static dataset, unlike codebook learning inside VQ-VAEs, where the distribution to cluster is constantly moving.

## The e-means project

The goal of e-means is to provide a modern optimization rule for the k-means problem. We started this project from the following observations:

- k-means is one of the rare problems in machine learning where full-batch optimization is still widely used.
- Mini-batch k-means methods in the literature exist[^3],[^4],[^5], but are mostly based on the MacQueen update rule, released in 1967.
- Modern deep learning offers optimization tools that can enhance mini-batch methods.

For e-means, we chose to do mini-batch updates with exponential moving averages[^6], which we found to be most efficient for the k-means problem. Contrary to previous mini-batch approaches, e-means requires no hyperparameter tuning because it automatically sets the batch size and step size with scaling laws (see [Automatic hyperparameter tuning with scaling laws](https://e-means.github.io/#scaling-laws)). In addition, e-means improves the initialization and the dead-cluster revival strategy, adds a cosine-scheduled decay rate, and introduces a finalization stage that reduces the training loss for a negligible runtime penalty at the end of training. e-means is highly parallelizable, and comes with specialized kernels that translate its algorithmic advantage into wall-clock speedups.

Moving-average updates

Each mini-batch updates every cluster's count and sum. The centroid is their ratio.

No hyperparameter tuning

Everything is derived from scaling laws.

Start and finish

Novel initialization and finalization methods. The run ends on each cluster's exact last-epoch mean.

Cosine scheduling

The step α anneals from α0 to 0.1α0, as a learning rate does.

Dead-cluster revival

Clusters with size below a threshold are revived by splitting a large cluster.

Specialized kernels

GPU kernels fuse the nearest-centroid assignment, turning the epoch savings above into matching wall-clock speedups.

## The centroid update rules

The centroid update rule is the central component of e-means. This section aims to provide an overview of update rules for k-means clustering. All methods maintain, for each cluster j, a sum Sj and a count Nj of the points assigned to it, and take the centroid as their ratio:

\\\[ c\_j = S\_j / N\_j \\\]

They differ only in how Sj and Nj are updated. Below, sj and nj denote the sum and the count of the points in the current batch that are assigned to cluster j (both zero if none are).

**Lloyd’s algorithm**[^7] assigns every point to its nearest centroid and recomputes the statistics from scratch, using the full dataset as the batch:

\\\[ S\_j \\leftarrow s\_j, \\qquad\\quad N\_j \\leftarrow n\_j \\\]

so cj is the exact mean of the points currently assigned to it. Each update costs one full pass over the data, and the procedure is repeated a fixed number of times (typically 20 to 50 iterations).

**MacQueen’s update**[^3] accumulates the statistics without ever discarding them:

\\\[ S\_j \\leftarrow S\_j + s\_j, \\qquad\\quad N\_j \\leftarrow N\_j + n\_j \\\]

so cj is the uniform mean of every point assigned to cluster j since the start of training, including points assigned under earlier, possibly poor, centroid positions. This was later partially addressed by Staleness-Reduction Mini-Batch K-Means[^8], a hybrid method between k-means and MacQueen. They obtained important speedups, but only on very low numbers of clusters.

**e-means** works on mini-batches and replaces this sum with an exponential moving average:

\\\[ S\_j \\leftarrow (1-\\alpha)\\,S\_j + \\alpha\\, s\_j, \\qquad\\quad N\_j \\leftarrow (1-\\alpha)\\,N\_j + \\alpha\\, n\_j \\\]

\\\[ S\_j \\leftarrow (1-\\alpha)\\,S\_j + \\alpha\\, s\_j \\\]

\\\[ N\_j \\leftarrow (1-\\alpha)\\,N\_j + \\alpha\\, n\_j \\\]

so older assignments are forgotten at rate α, and the centroid tracks recent assignments rather than the whole history. This update rule is typically used for moving distributions, hence their adoption for training VQ-VAEs[^6]. What we found is that it’s also extremely efficient for static datasets if it’s combined with e-means’ other components, and hyperparameters are properly tuned.

Why exponential moving average updates and not gradient descent?

The e-means project started from previous work at Meta FAIR by Hugo Touvron and Hervé Jégou, who had found that mini-batch k-means by gradient descent could beat Lloyd’s algorithm. While I could reproduce these results, I found that the performance of gradient-descent based centroid updates is unstable at large learning rates, unlike exponential moving average updates. I suspect that is because EMA updates guarantee that the centroid is always in the convex hull of points assigned to it, while gradient descent updates may overshoot past that hull.

![Test error against the effective step for exponential moving averages and plain gradient descent, at a large batch](https://e-means.github.io/assets/ema_vs_sgd_large_batch.svg)\
Test error against the effective step a centroid takes, at a large batch, for exponential-moving-average updates (EMA) and plain gradient descent (SGD), each at every step setting tried. DEEP, 1 million vectors, and SIFT, 10 million vectors, 16,384 clusters, 5 epochs, median over 3 seeds. The vertical axis is percent above the best test error in the panel. At its best setting, gradient descent ends 1.6% (DEEP) and 0.4% (SIFT) above the moving average.

In order to achieve the lowest loss at a fixed number of epochs, we chose exponential moving average updates, hence the name e-means for our method.

Lloyd is a special case of e-means: hyperparameter study

If you set the batch to the full dataset, B = n, *and* α = 1, then Nj and Sj become exactly the count and sum of the whole cluster, so one e-means iteration becomes exactly one Lloyd iteration. Lloyd is an extreme case of e-means without cosine scheduling on the learning rate. However, our hyperparameter search found that this extreme setting is highly suboptimal, which explains why e-means largely outperforms Lloyd. Below, the loss is measured for every batch size B and step α: Lloyd lands in a corner, well outside the region where the loss is lowest.

![Validation loss for every batch size and constant step at 5 epochs; the lowest losses lie on a diagonal band, and Lloyd, at full batch and step 1, sits in the top-right corner 1.8% above the best](https://e-means.github.io/assets/loss_map_t5.svg)\
Validation loss for every batch B and step α, relative to the best cell, after 5 epochs. DEEP, n = 106, k = 1024. The step is constant here (no cosine schedule) so that Lloyd is exactly one of the cells: full batch, α = 1. The lowest losses follow a diagonal band, a larger batch calls for a larger step.

This hyperparameter study motivated us to derive scaling laws that automatically pick the optimal batch size B and step size α.

## e-means better escapes local minima

To better understand why e-means reaches better losses than Lloyd at convergence, we plotted a few trainings on toy datasets to evaluate their ability to escape local minima. Repeating trainings over 200 seeds, we found that e-means is much less likely to fall in pathological configurations compared to Lloyd.

Two blobs of very different sizes, k = 8. Lloyd puts two centroids on the small blob and leaves the large one short. e-means starts in the same trap and leaves it within its first epochs, ending with seven centroids on the large blob and one on the small. Over 200 starts Lloyd stays stuck on 26% of starts, while e-means stays stuck on 3.5% of times.

Nine blobs in a 3×3 grid with heavier corners, k = 10. Lloyd leaves the centre blob without a centroid and crowds three into a corner. e-means falls into the same arrangement, then climbs out around epoch 10. Over 200 starts Lloyd stays stuck more than 1% worse on 43% of starts, while e-means is more than 1% worse on 4%.

Four blobs on a square around a heavier centre blob, k = 5. Lloyd puts two centroids on one corner blob and strands one in the gap between two others, which then share it. e-means, from the same start, gives every blob one. Over 200 starts Lloyd stays stuck on 15.5% of starts, while e-means stays stuck on 0%.

Five blobs on a line, k = 5. Lloyd leaves a light middle blob without a centroid and gives an end blob two. e-means, from the same start, gives every blob one. Over 200 starts Lloyd stays stuck on 46% of starts, while e-means stays stuck on 0%.

Our explanation for that is that mini-batches provide more noisy updates that are more likely to explore the optimization landscape and escape local minima. Lloyd, with its always decreasing loss, is more likely to exploit found local minima and never escape them.

## Automatic hyperparameter tuning with scaling laws

An essential property of e-means is that practitioners don't need to set any additional hyperparameter compared to Lloyd's algorithm. We fitted two scaling laws to set the batch size `B` and step size α0 from any problem's characteristics `(n, k, T)`, and found them to exhibit strong generalization on unseen datasets and problem scales.

\\\[ B = c\_B \\, k^{b\_k} \\, n^{b\_n} \\, T^{b\_T} \\\]

\\\[ \\alpha\_0 = \\min\\!\\Big(c\_\\alpha \\, k^{a\_k} \\, B^{a\_B} \\, n^{a\_n} \\, T^{a\_T},\\; 1\\Big) \\\]

| law                  | constant | exponent of | regret, held-out datasets | regret, held-out size | R²    |       |       |      |
| -------------------- | -------- | ----------- | ------------------------- | --------------------- | ----- | ----- | ----- | ---- |
| step α0              | 0.824    | −0.46       | 0.90                      | −0.45                 | −0.11 | 0.07% | 0.07% | 0.95 |
| batch B, default     | 0.035    | 0.13        | n/a                       | 0.88                  | 0.81  | 0.19% | 0.20% | 0.96 |
| batch B, quality law | 0.0044   | 0.13        | n/a                       | 0.88                  | 0.81  | 0.07% | 0.06% | n/a  |

Regret = (MSE with the law − best MSE over the grid) / best MSE, median over configurations. Held-out datasets: SIFT, SPACEV, and the VIBE[^9] datasets arXiv and AG-News. Held-out size: one value of n left out of the fit and predicted.

We propose two equations to choose the batch, the default law and the quality law. The “quality law” trades off throughput for better clustering quality against the “default” law. Below we plot the training data for our scaling laws: validation MSE against step size α0 (left) and batch size B (right).

![The two scaling laws fit on deep1b](https://e-means.github.io/assets/paper_laws.svg)

## The importance of dead-cluster revival

Toy example showing the importance of dead-cluster revival.

The main issue with mini-batch k-means is that it is prone to “dead” clusters, centroids that end up with zero assigned points during optimization. This reduces the effective size of the codebook, which is especially problematic for the large codebooks increasingly used today.

A common mitigation is to reinitialize clusters that fall below a certain size threshold, as done in EnCodec[^10], where dead centroids are simply replaced by random samples drawn from the incoming batch. While this helps, it does not fully resolve the issue. Instead of sampling arbitrary points from the batch, we revive dead clusters by splitting large, existing ones: we sample an active cluster with probability proportional to its size and split it into two. We found this approach, adapted from Faiss k-means, to be significantly more effective at eliminating dead clusters and preserving codebook capacity.

| dataset                       | revival policy         | dead clusters | of codebook | imbalance | test MSE    |
| ----------------------------- | ---------------------- | ------------- | ----------- | --------- | ----------- |
| DEEP                          | **proportional split** | **47**        | **0.3%**    | **1.194** | **0.38762** |
| resample from batch (EnCodec) | 2,343                  | 14.3%         | 1.423       | 0.39124   |             |
| off                           | 4,313                  | 26.3%         | 1.647       | 0.39448   |             |
| SIFT                          | **proportional split** | **10**        | **0.1%**    | **1.186** | **0.19623** |
| resample from batch (EnCodec) | 750                    | 4.6%          | 1.233       | 0.19684   |             |
| off                           | 2,080                  | 12.7%         | 1.404       | 0.19797   |             |

1 million vectors, k = 16,384, 5 epochs, mean over 5 seeds; only the revival policy varies. A cluster counts as dead when fewer than ten vectors are assigned to it at the end, a thousandth of an average cluster. Imbalance: how uneven the cluster sizes are (1 = all equal). From the [paper](https://arxiv.org/abs/2611.09501)’s revival ablation table.

## Extensive experimental validation

We validated e-means on 24 different embedding and synthetic datasets and configurations (see [paper](https://arxiv.org/abs/2611.09501)). For the video we ran 234 configurations of e-means against k-means (16 datasets, 100k to 100M points): explore every curve in the [results map](https://e-means.github.io/results-map.html).

![Convergence curves vs Lloyd at 25 iterations](https://e-means.github.io/assets/paper_epochs_3arm.png)\
Train (top) and test (bottom) MSE, divided by Lloyd’s after 25 epochs. Initialized from random vectors.

![Test error against wall-clock time on GPU, 10 million vectors, k=65,536](https://e-means.github.io/assets/paper_gpu_10m.svg)\
Test MSE against wall-clock time on one H100, 10 million vectors, 65,536 clusters. 25 epochs of a plain PyTorch Lloyd (data on the GPU, bf16) take 142 to 155 seconds, against 14 to 22 seconds for Flash-KMeans[^2]. Labels on the curves mark the number of epochs.

![Epochs e-means needs to reach Lloyd's 25-epoch test error on the 11 VIBE embedding datasets: 1.6 to 2.4](https://e-means.github.io/assets/vibe_epochs_bar.svg)\
Epochs e-means needs to reach the test error Lloyd reaches in 25 epochs, on the 11 datasets of the VIBE embedding benchmark (text, code, image and multimodal embeddings, d = 200 to 3,072, k = 8,192 or 16,384). Budgets spaced 0.2 epochs apart, median over five seeds. Every dataset needs between 1.6 and 2.4 epochs, against Lloyd’s 25.

## Library and code

from faiss.contrib.e\_means import Emeans # NumpyEmeans for NumPy em = Emeans(d, k, nepochs=T) # T epochs (5 by default) centroids, labels = em.train(x) # x: a PyTorch tensor (n, d) # labels: each vector's cluster in the last epoch

Repo: [faiss/contrib/e\_means](https://github.com/facebookresearch/faiss/tree/main/contrib/e_means).

### Citing this work

If you use e-means, please cite the paper ([arXiv:2611.09501](https://arxiv.org/abs/2611.09501)):

@misc{bessa2026emeansoutperformingkmeansin, title={e-means: outperforming k-means (in one-tenth the epochs)}, author={Swann Bessa and Pierre-Emmanuel Mazaré and Gergely Szilvasy and Hugo Touvron and David Picard and Matthijs Douze and Hervé Jégou}, year={2026}, eprint={2611.09501}, archivePrefix={arXiv}, primaryClass={cs.LG}, url={https://arxiv.org/abs/2611.09501}, }

## References

[^1]: M. Douze, A. Guzhva, C. Deng, J. Johnson, G. Szilvasy, P.-E. Mazaré, M. Lomeli, L. Hosseini, H. Jégou. "The Faiss library." *arXiv:2401.08281*, 2024.

[^2]: S. Yang, H. Xi, Y. Zhao, M. Li, X. Fan, J. Zhang, H. Cai, Y. Lin, X. Li, K. Keutzer, S. Han, C. Xu, I. Stoica. "Flash-KMeans: Fast and Memory-Efficient Exact K-Means." *arXiv:2603.09229*, 2026.

[^3]: J. MacQueen. "Some Methods for Classification and Analysis of Multivariate Observations." *Proceedings of the Fifth Berkeley Symposium on Mathematical Statistics and Probability*, 1:281–297, 1967.

[^4]: D. Sculley. "Web-Scale K-Means Clustering." *WWW*, 2010.

[^5]: J. Newling, F. Fleuret. "Nested Mini-Batch K-Means." *NeurIPS*, 2016.

[^6]: A. van den Oord, O. Vinyals, K. Kavukcuoglu. "Neural Discrete Representation Learning." *NeurIPS*, 2017.

[^7]: S. Lloyd. "Least squares quantization in PCM." *IEEE Transactions on Information Theory*, 28(2):129–137, 1982 (circulated as a Bell Labs technical note since 1957).

[^8]: X. Zhu, J. Sun, Z. He, J. Jiang, Z. Wang. "Staleness-Reduction Mini-Batch K-Means." *IEEE Transactions on Neural Networks and Learning Systems*, 35(10):14424–14436, 2024.

[^9]: E. Jääsaari, V. Hyvönen, M. Ceccarello, T. Roos, M. Aumüller. "VIBE: Vector Index Benchmark for Embeddings." *arXiv:2505.17810*, 2025.

[^10]: A. Défossez, J. Copet, G. Synnaeve, Y. Adi. "High Fidelity Neural Audio Compression." *Transactions on Machine Learning Research*, 2023. arXiv:2210.13438.
