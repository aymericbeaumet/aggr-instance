---
title: Predicting LoRA size for any dataset
link: https://engram.com/blog/lora-size
source: hnrss-org
published: 2026-11-24T22:34:29Z
updated: 2026-11-24T22:34:29Z
first_seen: 2026-11-26T02:47:10.395377406Z
authors:
- jxmorris12
content: extracted
html: 2026-11-24-predicting-lora-size-for-any-dataset.html
remote_preview:
  url: https://engram.com/_next/static/media/og.0j-w_d5pzme3m.jpg?dpl=dpl_GCLvFwRSxoRUZRnsjhnQcLryLcr4
  alt: Text compressed through a bottleneck into the low-rank update of a weight matrix, W' = W + BA
extra:
  content:interactive: true
---

Given an SFT dataset, we can predict exactly the LoRA size required to fit it.

> **TLDR:**
>
> - We develop a predictive theory of the capacity of a LoRA adapter as a function of its rank.
> - Unlike full-parameter fine-tuning, LoRA learns a *decreasing* amount per-parameter.
> - Given an SFT dataset, we can predict the LoRA size required to fit it.

At Engram, we train lots of models. We train models for individual users and update them when information changes. A major focus of our research is how to customize and adapt models efficiently at scale.

This post focuses on LoRA [1](https://engram.com/blog/lora-size#sidenote-1). LoRA has become the predominant approach for customizing models in industry. While there exists a large body of knowledge on how to set the hyperparameters and train LoRA effectively [2](https://engram.com/blog/lora-size#sidenote-2)[, 3](https://engram.com/blog/lora-size#sidenote-3), it’s still unclear how to set LoRA’s rank. Although it’s agreed-upon that LoRA underperforms full-finetuning on larger datasets, it’s not clear when this is the case. How big does a dataset have to be to require a larger LoRA?

In practice, smaller LoRAs enjoy a number of systems advantages, being lighter to load [4](https://engram.com/blog/lora-size#sidenote-4)[, 5](https://engram.com/blog/lora-size#sidenote-5), faster to generate text from [6](https://engram.com/blog/lora-size#sidenote-6), and easier to store on disk. For larger models, rank-1 can store quite a lot; some datasets, like RL data, can be represented in a small number of bits and often fit within a rank-one LoRA adapter (or smaller\![7](https://engram.com/blog/lora-size#sidenote-7)[, 8](https://engram.com/blog/lora-size#sidenote-8)[, 9](https://engram.com/blog/lora-size#sidenote-9)).

In this post, we develop a predictive theory and practical approach for sizing LoRA adapters to a new dataset. The idea is simple: the size of the LoRA required depends on how many bits it takes to encode the dataset under the base model. We use this idea to derive an exact formula for how many bits a single LoRA adapter can learn as a function of its rank.

We then extend our idea to measure the capacity of LoRA on full transformers. Since theory can’t predict the nonlinear behavior of deep transformers, we turn to an empirical solution: we generate synthetic datasets of exactly $N$ bits under the base model and train on them to measure LoRA’s capacity. Our capacity predictions hold up on real datasets: across a variety of settings, we can accurately determine the necessary rank of the adapter.

## Information theory and LLMs

We begin this post with a basic primer on the connection between information theory and language modeling. Remember that our goal is to find the smallest adapter that fits a given dataset. To do that, we need to measure adapters and datasets in the same unit: bits.

**Language modeling and Shannon**

Language models assign probabilities to strings:

$$
p_\theta(x) = \prod_i p_\theta(x_i \mid x_{<i})
$$

A language model is, at its core, a text compressor [10](https://engram.com/blog/lora-size#sidenote-10): a model that assigns high probability to data can encode it efficiently. And we’ve known this since the 1950s! By Shannon’s source coding theorem [11](https://engram.com/blog/lora-size#sidenote-11), a model’s quality (its perplexity, or cross-entropy) corresponds directly to its effectiveness as a compressor. The number of bits needed to encode a string $x$ under a model $\theta$ is its negative log-likelihood:

$$
\text{bits}_\theta(x) = -\sum_i \log_2 p_\theta(x_i \mid x_{<i})
$$

Here’s an example of what it’s like to view training loss as *bits*. Given some fixed-size training dataset, we can directly convert loss to bits learned by $\theta$[12](https://engram.com/blog/lora-size#sidenote-12):

Figure 1: Loss

Storage

storage training loss

**Step 3,981** rank-6 LoRA, mlp\[23\]

Stored

6.67 KB

Dataset size

6.75 KB

Share stored

98.8%

As a model’s loss drops, the amount it stores about the training data rises. We can measure this exactly in bits.

**Generating *exactly* N bits using an LLM**

To measure the capacity of LLMs being trained with adapters, we need to generate data with a controllable number of bits. Prior work in this space creates data of fixed size with random generative processes, like randomly generated tables of facts [13](https://engram.com/blog/lora-size#sidenote-13) or random strings of tokens.

To generate truly random data from an LLM, we leverage a tool called arithmetic coding. Arithmetic coding gives us the ability to transform probability distributions into codes; it represents the entire code using a single floating point number in $[0, 1)$ that we can turn into a sequence using $p_\theta$.

We take this one step further. A pretrained open-source model like Qwen or Inkling already provides likelihoods for text, so we can run arithmetic coding in reverse: start from $N$ random bits and decode them into text. The result is a sample from $p_\theta$ whose code length under the model is $N$ bits. One can think of this as an implementation of Neural Linguistic Steganography [14](https://engram.com/blog/lora-size#sidenote-14) using randomly selected codes.

Here’s an example of arithmetic coding in use. The following panel allows you to generate strings with exact bit counts under SmolLM2:

Figure 2: Decoding Bits into Text

Prompt **SmolLM2-135M** · 23 tokens, code length 70.3 bits

Code

110 001010 10 00011111100 101110 0101 1 101 1001 00010 0011 0 1 1 1111 11 01100100001

Text

Once upon a time, there lived a brave little queen named M ina. She had many dreams for her kingdom, but they were always won

**“␣won**”token 23 of 23

Bits it settled

01100100001

Probability

0.04%

Code length

11.34 bits

Rank among tokens

248

1. `␣finding` `01100101010` 0.04%
2. `␣noisy` `1001101000··` 0.04%
3. `␣won` `01100100001` 0.04%
4. `␣bound` `01100011111` 0.04%
5. `␣unclear` `10010010000` 0.04%

Code lengths come out slightly longer than the number of encoded bits because tokens are indivisible. In this example, `queen` is worth eleven bits (surprising!), while `little` carries just two bits. Later in this post we will generate many strings like this to build a dataset of exactly known size, then train an adapter on it to measure how many bits it can learn.

## How much can LoRA store?

Given a matrix $W$ to adapt, LoRA works by training two matrices whose product is a low-rank update to $W$:

$$
W' = W + \frac{\alpha}{r} BA, \quad B \in \mathbb{R}^{m \times r}, \; A \in \mathbb{R}^{r \times n}
$$

Here $W$ is an $m \times n$ matrix being adapted and $A$ and $B$ are the matrices trained via LoRA. Since $r$ is often much smaller than $m$ and $n$, the total number of parameters trained by the LoRA is typically much smaller than training all of $W$.

The parameter $\alpha$ is a rank-independent scaling factor; in this work, we fix $\alpha=256$ to avoid conflating rank with learning rate.

### Sizing LoRA, in theory

Prior work has estimated that the capacity of full transformer training scales roughly linearly with parameter count [15](https://engram.com/blog/lora-size#sidenote-15)[, 16](https://engram.com/blog/lora-size#sidenote-16)[, 17](https://engram.com/blog/lora-size#sidenote-17). We don’t intuitively expect this to be true with LoRA, since some dimensions influence the model outputs more than others. Instead, we expect diminishing returns to capacity as rank increases. This is exactly what we see in practice, and it turns out we can predict the capacity curve quite cleanly using a simple statistical model.

Suppose we sample data from a model and then adapt one of its weight matrices to memorize that data. How much can we expect to increase the log-likelihood (and hence decrease the code length) with a rank-$r$ adapter? To second-order, $\log p_\theta(x)$ looks like an inverted bowl in parameter space. How high we can climb on that bowl can be determined from the gradient and Hessian at the point $\theta$, and these are both random functions of the training data $x$. Since the data is sampled from $p_\theta$, the statistics of the gradient and Hessian are determined by the Fisher information matrix, which greatly simplifies the calculations.

After some algebra, we find that the capacity of a rank-$r$ adapter is approximately,

$$
C(r) \approx \frac{1}{2\ln 2}\,\mathbb{E}\left[\sum_{i=1}^{r}\sigma_i(Z)^2\right]\ \text{bits}, \qquad Z\in\mathbb{R}^{m\times n},\ Z_{ij}\stackrel{\mathrm{iid}}{\sim}\mathcal{N}(0,1)
$$

where $\sigma_i(Z)$ is the $i$-th singular value of the random matrix $Z$. The capacity grows roughly linearly at low rank,

$$
C(r) \approx \frac{(\sqrt m+\sqrt n)^2}{2\ln 2} \cdot r \qquad \text{for } r \ll \min(m,n),
$$

but it plateaus as the rank increases and the singular values decrease. At full rank, the capacity is $\tfrac{mn}{2\ln 2}$ bits, recovering a classical result from Wilks’ theorem in statistics [18](https://engram.com/blog/lora-size#sidenote-18).

For an 800M parameter Qwen3.5 model, which has $512 \times 1024$ linear layers in its attention value projections (v\_proj), the theory predicts about 378k bits, or 0.48 bits-per-parameter at full rank and 1.3-1.4 bits-per-parameter at low rank.

When we measure capacity using actual training, this prediction turns out to be quite accurate; $C(r)$, the blue curve, is accurate at every rank:

Figure 3: Theory Predicts Capacity

measured (LoRA on v\_proj) theory, C(r) linear, 1.39 bits/param

**Rank 128** LoRA on v\_proj

Parameters

197k

Dataset size

2.69 MB

Stored

23.9 KB

Bits per parameter

0.97

Theory, C(r)

23.9 KB

The theory accurately predicts the capacity of LoRAs of different ranks.

### Sizing LoRA, in practice

Theory predicts around 0.5-1.4 bpp when perturbing individual weight matrices. What about full transformers? When we adapt several matrices *jointly*, the capacity is more than the sum of its parts. We see super-additive scaling of capacity thanks to complementary interactions from adapting multiple weights at once [19](https://engram.com/blog/lora-size#sidenote-19).

While the theory above cannot explain these super-additive effects yet, we do have a way to generate a dataset of a fixed size (measured above) and a way to measure how much a model learns from data (by checking its loss).

**Extrapolating to the infinite-training regime.** We fit a function to estimate the asymptote of the loss curve under infinite training, similar to [20](https://engram.com/blog/lora-size#sidenote-20). This modeling allows us to train for a fixed number of steps and predict the asymptote of the loss under high compute, rather than perform the full training. It also allows us to avoid constantly sweeping learning rates, since different learning rates converge to the same capacity under infinite training.

We find a simple log-logistic curve fits our training runs well:

$$
\text{bits}(T) = \text{bits}_\infty \cdot \frac{T^c}{T^c + \tau^c}
$$

Here $T$ is the number of training steps, $\tau$ is the step where the adapter has stored half its eventual bits, and $c$ sets how sharp the rise is; we fit this on runs of 1k steps and up, and report $\text{bits}_\infty$ as the adapter’s capacity.

Naively, one might expect the total capacity to be the sum of each adapter’s capacity, and hence the bits stored per parameter to remain roughly constant as we adapt more and more layers. However, as we increase the number of layers, total capacity actually rises from 1.5 bits-per-parameter to 3.3:

Figure 4: Storage Grows with Depth

**top 24 MLPs** 24 nonlinear layers

LoRA

rank 4 1.33M params

Layers

0–23 (24)

Modules

gate\_proj, up\_proj, down\_proj (72 adapted)

Bits/param, infinite

3.29

Bits/param, 16k steps

2.85

Theory prediction

1.30

It turns out that models with more layers can store more per-parameter: a single layer with LoRA stores just 1.5 bits-per-parameter, while a 24-layer model can store 3.3 bits-per-parameter.

For a fixed-depth model, we see that increasing the LoRA rank also increases total capacity. Our technique for generating data for $\theta$ across sizes allows us to easily measure the capacity of LoRAs across ranks:

Figure 5: Storage Versus Dataset Size

rank 1 rank 2 rank 4 rank 8

**Rank 8** 442k params

Dataset size

556 KB

Stored, infinite

164 KB

Share stored

29%

Stored at 1k steps

87 KB

Stored at 2k steps

100 KB

Stored at 4k steps

114 KB

Rank capacity

164 KB

Bits per parameter

2.96

After enough training, LoRA stores the entire dataset, if it has capacity. If not, it stores a number of bits consistent with its rank.

We can also easily study how capacity interacts with training. Although the majority of our results assume capacity in the infinite-training regime, we can quickly model how even in the case of LoRA, capacity rises with the number of training steps [21](https://engram.com/blog/lora-size#sidenote-21). This finding is intuitive, since training for longer stores more bits:

Figure 6: Capacity Grows with Training

50 steps 100 steps 200 steps 400 steps 800 steps 1,600 steps

**1,600 steps** rank 1 55.3k params

Dataset size

111 KB

Stored

12.9 KB

Share stored

12%

Training budget

1,600 steps

Bits per parameter

1.86

When we train for a finite number of steps, we see that LoRAs store more with more training.

To test how much models forget about what they already know, we can measure likelihood on samples from the base model; this gives us a measurement of the total number of bits the model forgets during the training process. Across all the models trained for this post, we observe a remarkably predictable trend where models forget roughly as much as they learn:

Figure 7: Conservation of Capacity

**v\_proj** rank 512

Learning

45.8 KB

Forgetting

45.8 KB

Forgetting / learning

1.00

Training step

16,000 of 16,000

Dataset size

527 KB

To learn a certain amount of new information, models have to forget the same or more information to “free” capacity. We find this equivalence holds across all settings examined in the post.

This finding implies that training is quite efficient: as we add new information to a model, it forgets a comparable amount. A model that learns 10 KB of new information might forget 12 KB of old information. This setting provides a clean testbed for future research to investigate the connection between learning & forgetting.

## Sizing LoRA capacity with real datasets

Remember that our goal is to estimate the rank of LoRA required to fit some arbitrary SFT dataset. To do this, we need a method for measuring the size of datasets. In the previous section, we used log-likelihood under $\theta$ as a way to generate synthetic data of a precise size. Following the same logic, we can measure the log-likelihood of real data to compute its size under $\theta$.

### Measuring the size of full datasets

We first extend the model’s context window to 2M tokens via YaRN [22](https://engram.com/blog/lora-size#sidenote-22). If the data still doesn’t fit into context, we estimate its size in bits under the model by cutting it into windows of 2M tokens and summing the log likelihood:

$$
\text{bits}_\theta(D) \approx \sum_{k=1}^{K} \text{bits}_\theta\big(x^{(k)}\big) = -\sum_{k=1}^{K} \sum_{i} \log_2 p_\theta\big(x^{(k)}_i \mid x^{(k)}_{<i}\big)
$$

where $x^{(1)}, \dots, x^{(K)}$ are consecutive 2M-token windows of the dataset $D$.

Although this gives us a slight overestimate for the total information content of a dataset, in practice we find the error to be small and the estimates useful enough to predict rank within 5% error.

### Testing on real workspaces

We now test our ability to properly allocate adapters and train. We test on datasets of varying sizes scattered across the public internet as well as our and our partners’ workspaces.

For each new dataset, we first measure its size in bits under the model and read the predicted rank $r$ off the synthetic capacity curve. We then train LoRAs on the real dataset itself at ranks from $r/2$ to $2r$, and report the measured rank as the smallest rank that stores 95% of the dataset.

Figure 8: Predicting LoRA Rank

prediction real

### mlp\[23\]

### All-linear

**Engram (XS**)mlp\[23\]

Tokens

22k

Size

6.8 KB

Stored

6.69 KB

Predicted rank

3.45

Measured rank

4.26

```
If you have any questions or need assistance, don’t hesitate to reply directly. Thanks again for your interest in joining us – we’re eager to see where this journey takes us!
```

Main result: Our predictions (blue) match the LoRA rank required to fit a dataset.

Even though our capacity estimates assume the training data is drawn from the base model, our predictions are extremely accurate on real datasets: the orange dots, the smallest LoRAs required to fit each dataset in practice, fall almost on the blue line, our estimate of a given LoRA rank's capacity.

## Conclusion

This post described our work to automatically provision adapters of a fixed size by using information theory to estimate adapter capacity and relate it to the size of a given dataset. This gives a way to mint PEFT modules on-the-fly of the minimum size required for a given task.

In practice, we don’t always use LoRA. We’re exploring other options for parameter-efficient learning such as trainable prefixes [23](https://engram.com/blog/lora-size#sidenote-23), sparse memory layers [24](https://engram.com/blog/lora-size#sidenote-24), steering vectors [25](https://engram.com/blog/lora-size#sidenote-25), GDN state, and trainable experts [26](https://engram.com/blog/lora-size#sidenote-26). We find the landscape of PeFT methods contains a number of tradeoffs, and different modules work better for different tasks. We’re also working on other parts of this stack, such as more accurately estimating loss without training and understanding capacity in the finite-compute regime.

On a broader scale, examining how model capacity interacts with data size provides us a clean playground to investigate fundamental questions around how models learn.

## Acknowledgements

We thank Henry Smith and Jacob Portes for their useful feedback on this post.

## Citation

Engram. “[Predicting LoRA size for any dataset](https://engram.com/blog/lora-size).” 2026.

BibTeX

```
@misc{engram2026loramemorize,
  author = {{Engram}},
  title = {Predicting LoRA size for any dataset},
  year = {2026},
  howpublished = {Engram blog},
  url = {https://engram.com/blog/lora-size}
}
```

## Appendix

### A: Theory

Intuitively, the capacity of the adapter is the number of bits it can store about the data it is trained on. We formalize this concept as the decrease in code length of training data, on average over the training distribution. When the training distribution differs from the base model, the decrease in code length comes from a combination of learning the *signal* — the change in distribution — and *noise* — the particulars of the individual training samples. We can simplify our study of capacity by sampling from the base model, eliminating the signal so that any reduction in code length comes from memorizing the noise in the training samples. While this is unrealistic, it gives conservative worst-case estimates of capacity per parameter, and as we will empirically demonstrate, the theory is predictive in more realistic settings as well.

Let $p_{\theta+\delta}$ denote the adapted model after a perturbation $\delta$ to the model parameters. Here, the perturbation is trained to maximize the log-likelihood of the training data. To second order, the objective is approximately,

$$
\log p_{\theta+\delta}(x) \approx \log p_\theta(x) + \delta^\top g  + \frac{1}{2} \delta^\top H \delta
$$

where $g = \nabla_\theta \log p_\theta(x)$ is the gradient (i.e., the score function) and $H = \nabla^2 \log p_\theta(x)$ is the Hessian. With enough samples $x \sim p_\theta$, the Hessian $H$ is well-approximated by the negative Fisher, $H \approx -F$.

Here, $\delta \in \mathbb{R}^{mn}$ is a vector obtained by raveling a matrix-valued adapter $\Delta \in \mathbb{R}^{m \times n}$. If we optimize to find the best rank-$r$ adapter, the approximate increase in log-likelihood is,

$$
\max_{\operatorname{rank}\Delta \le r}\ \Big[\; \underbrace{\delta^\top g - \tfrac{1}{2}\delta^\top F\delta}_{Q(\delta)}\; \Big]
$$

To obtain a closed-form expression for the approximate increase in log-likelihood, we need one further assumption. As written above, the Fisher information is a matrix $F \in \mathbb{R}^{mn \times mn}$. If the Fisher information matrix admits a Kronecker-factorization [27](https://engram.com/blog/lora-size#sidenote-27), $F = F_1 \otimes F_2$, where $F_1 \in \mathbb{R}^{m \times m}$ and $F_2 \in \mathbb{R}^{n \times n}$ are invertible matrices, then in terms of the matrix-valued perturbation:

$$
Q(\Delta) \approx \langle \Delta, G \rangle - \frac{1}{2} \langle F_1 \Delta, \Delta F_2 \rangle
$$

where $G \in \mathbb{R}^{m\times n}$ is the gradient $g$ reshaped into a matrix. Now reparameterize in terms of $\widetilde{\Delta} = F_1^{1/2} \Delta F_2^{1/2}$. Then,

$$
Q(\widetilde{\Delta}) = \langle \widetilde{\Delta}, Z \rangle -\frac{1}{2} \langle \widetilde{\Delta}, \widetilde{\Delta} \rangle = -\frac{1}{2} \|Z - \widetilde{\Delta}\|_F^2 + c
$$

where $Z = F_1^{-1/2} G F_2^{-1/2}$ and $c = \tfrac{1}{2}\|Z\|_F^2$ is constant in $\widetilde \Delta$.

Maximizing subject to the rank-$r$ constraint amounts to setting $\widetilde{\Delta}^\star$ to the truncated SVD of $Z$ with the top $r$ singular values. With this optimal choice, the approximate increase in log-likelihood is,

$$
Q(\widetilde{\Delta}^\star) = \frac{1}{2} \sum_{i=1}^r \sigma_i(Z)^2
$$

The capacity of a rank-$r$ adapter is the expected increase, measured in bits. Recall that the entries of $G$ are gradients of the log-likelihood — that is, the score functions. Thus, in expectation, $G$ is mean zero and has covariance equal to the Fisher information matrix. When we multiplied $G$ by the inverse square roots to obtain the matrix $Z$, we effectively whitened the covariance so that $Z$ has mean-zero, uncorrelated, unit-variance entries, and by a martingale CLT argument, they are approximately i.i.d. Gaussian. Thus, the capacity is approximately,

$$
C(r) \approx \frac{1}{2 \ln 2} \mathbb{E} \left[ \sum_{i=1}^r \sigma_i(Z)^2 \right]; \quad Z \in \mathbb{R}^{m\times n}, \; Z_{ij} \sim \mathcal{N}(0,1).
$$

The singular values of $Z$ follow a predictable distribution from random matrix theory. In the limit $m, n \to \infty$ with $m/n \to \gamma \in (0, 1]$, assuming w.l.o.g. that $m \leq n$, the distribution of the $m$ rescaled squared singular values $\lambda = \sigma^2/n$ (equivalently, the eigenvalues of $ZZ^\top/n$) converges to the **Marchenko–Pastur law**[29](https://engram.com/blog/lora-size#sidenote-29) with density

$$
\rho_\gamma(\lambda) = \frac{\sqrt{(\lambda_+ - \lambda)(\lambda - \lambda_-)}}{2\pi\gamma \lambda}, \qquad \lambda_\pm = (1 \pm \sqrt\gamma)^2.
$$

with support $\lambda \in [\lambda_-, \lambda_+]$. Let $t_r$ be the upper $r/m$ quantile of $\rho_\gamma$, i.e., a fraction $r/m$ of the rescaled squared singular values lie above it. The first equation below is this definition. Summing the captured values, each worth $\sigma^2 = n\lambda$, gives the second:

$$
r = m\int_{t_r}^{\lambda_+} \rho_\gamma(\lambda) \, d\lambda, \qquad C(r) \approx \frac{mn}{2\ln 2}\int_{t_r}^{\lambda_+} \lambda\, \rho_\gamma(\lambda)\, d\lambda
$$

The approximation becomes exact as the matrix grows with $r/m$ fixed. At 512×1024 it is within 0.4% of the exact $C(r)$.

The integrals are still a bit unwieldy, but substituting $\lambda = 1 + \gamma + 2\sqrt\gamma\cos\varphi$ leads to a closed-form expression for the curve $\varphi \mapsto (r(\varphi), C(r(\varphi)))$, with $\varphi \in [0, \pi]$:

$$
\begin{align*}
r(\varphi) &= m \cdot \frac{(1+\gamma)\varphi - 2\sqrt\gamma \sin\varphi - 2(1-\gamma)\arctan\!\left(\frac{1-\sqrt\gamma}{1+\sqrt\gamma}\tan\frac\varphi2\right)}{2\pi\gamma}, \\
C(r(\varphi)) &= \frac{mn}{2\ln 2}\cdot\frac{\varphi - \sin\varphi\cos\varphi}{\pi}
\end{align*}
$$

To get $C(r)$ at a given rank $r$, solve the first equation for $\varphi$ numerically (it is monotone in $\varphi$) and substitute into the second.

**Low-rank limit.** For $r \ll m$, the tail above $t_r$ shrinks to the edge $\lambda_+$, so each direction captures about $n\lambda_+ = (\sqrt m + \sqrt n)^2$, and

$$
C(r) \approx \frac{r(\sqrt m+\sqrt n)^2}{2\ln 2}
$$

Normalizing by the number of LoRA parameters gives

$$
\frac{C(r)}{r(m+n)} \approx \frac{1}{\ln 2}\left(\frac12+\frac{\sqrt{mn}}{m+n}\right)\ \text{bits per LoRA parameter}
$$

For Qwen3.5-0.8B this gives 1.40 bits per LoRA parameter for v\_proj (512×1024) and 1.32 for up\_proj (1024×3584 with the smaller dimension first).

### B: Experimental setup

**Optimization**. All training used the AdamW optimizer with a short linear warmup, cosine decay to zero, gradient-norm clipping, and no weight decay. All experiments are conducted on [Qwen3.5-0.8B](https://huggingface.co/Qwen/Qwen3.5-0.8B), although we expect our findings to transfer cleanly to other base models.

We use a combination of internal and external datasets:

- **Engram (XS):** internal emails (22k tokens, 6.8 KB)
- **Engram (MD):** internal research notes (2.2M tokens, 628 KB)
- **Harvey C&H (XS, SM):** documents from a synthetic law firm’s workspace (451k tokens, 94 KB; 4.0M tokens, 821 KB)
- **Developer Docs:** a public developer-documentation site (751k tokens, 131 KB)
- **[swebench-django](https://huggingface.co/datasets/princeton-nlp/SWE-bench):** the Django repository’s source code from [SWEBench](https://arxiv.org/abs/2310.06770) (2.5M tokens, 550 KB)
- **[MATH-500](https://huggingface.co/datasets/HuggingFaceH4/MATH-500):** competition math problems with solutions (141k tokens, 20 KB)
- **[GSM8K](https://huggingface.co/datasets/openai/gsm8k):** grade-school math word problems with solutions, train split (1.3M tokens, 236 KB)
- **[Tulu 3](https://huggingface.co/datasets/allenai/tulu-3-sft-mixture):** 2,000 samples from the Tulu 3 SFT mixture (1.8M tokens, 666 KB)
- **[No Robots](https://huggingface.co/datasets/HuggingFaceH4/no_robots):** human-written instructions and responses (2.6M tokens, 1.1 MB)
- **[OpenThoughts3](https://huggingface.co/datasets/open-thoughts/OpenThoughts3-1.2M):** 250 long reasoning traces (3.9M tokens, 1.2 MB)
- **[MMMLU](https://huggingface.co/datasets/openai/MMMLU):** multilingual MMLU questions in 14 languages, 70% subset (20.5M tokens, 8.6 MB)
- **[enwik8](https://huggingface.co/datasets/LTCB/enwik8):** the first 100 MB of English Wikipedia (27M tokens, 9.5 MB)

It can be useful to examine dataset “size” along two dimensions, *bits* and *tokens*.

- We might consider the total entropy of a dataset under $\theta$ – in some sense, the amount of information the dataset “has” to contribute to the model.
- We can also simply count the cumulative number of tokens.

Both metrics are useful for different cases. We visualize these datasets along both axes:

Figure 9: Dataset Size in Bits

workspaces public datasets enwik8 0.5, 1, 2, 4 bits/token

**Engram (XS**)Workspace

Tokens

22k

Size

6.8 KB

Bits per token

2.47

Each dataset's size under $\theta$ in bits vs. its number of tokens.
