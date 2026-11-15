---
title: Understanding Positional Embeddings in Modern LLMs
link: https://magazine.sebastianraschka.com/p/understanding-positional-embeddings
source: magazine-sebastianraschka-com
published: 2026-11-15T12:10:37Z
updated: 2026-11-15T12:10:37Z
first_seen: 2026-11-15T14:41:43.195736799Z
authors:
- Sebastian Raschka, PhD
summary: The Methods Behind How LLMs Represent Token Positions
content: extracted
html: 2026-11-15-understanding-positional-embeddings-in-modern-llms.html
remote_preview:
  url: https://substackcdn.com/image/fetch/$s_!8RKL!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb461a2cb-11ab-474a-affe-0ee99e5a8782_2850x2777.png
---

The Methods Behind How LLMs Represent Token Positions

This article explains how LLMs encode and process token position information by covering classic and contemporary methods like RoPE. It’s essentially a comprehensive journey where we will work through the mechanisms with visual examples, a little math, and a small PyTorch implementation. And, of course, we will then also look at how modern architectures adapt or omit explicit positional encodings.

Below, you can find a preview of which positional embedding choices are used in 2026 open-weight models. We will discuss these choices later after covering the underlying mechanisms.

[![Figure 1 — gallery 2026 collage](https://substackcdn.com/image/fetch/$s_!8RKL!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb461a2cb-11ab-474a-affe-0ee99e5a8782_2850x2777.png "Figure 1 — gallery 2026 collage")](https://substackcdn.com/image/fetch/$s_!8RKL!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb461a2cb-11ab-474a-affe-0ee99e5a8782_2850x2777.png)

Figure 1: A preview of the gallery’s 2026 architectures, grouped by positional encoding. Architecture figures taken from my [LLM gallery](https://sebastianraschka.com/llm-architecture-gallery/).

But why spending so much time writing/reading about this rather minor building block? LLMs keep evolving, and as I was preparing a new article on the latest LLM architecture trends this year, I realized that I never covered the positional information methods used in LLMs in any article so far. So, instead of letting the other article get out of hand, I thought it might make sense to publish this as a standalone building block for the upcoming LLM architecture and trends analysis.

Also, over the years, many readers have asked me for a tutorial on relative positional embeddings (aka RoPE) to follow up on the learned absolute embeddings used in GPT. It’s been on my TODO list for a long time, so I finally revisited my old draft and put this walkthrough together, which will cover

- different positional methods in LLMs;

- (finally) how RoPE works;

- what methods are used by recent open LLMs.

Note that this article assumes some familiarity with basic LLM architectures and works well as a follow-up to [Build a Large Language Model (From Scratch)](https://sebastianraschka.com/books/#build-a-large-language-model-from-scratch). If the concepts and computation of the attention internals, particularly query, key, and value vectors, are a little fuzzy, I recommend checking my [self-attention walkthrough](https://magazine.sebastianraschka.com/p/understanding-and-coding-self-attention) as a refresher:

[![Understanding and Coding Self-Attention, Multi-Head Attention, Causal-Attention, and Cross-Attention in LLMs](https://substackcdn.com/image/fetch/$s_!3NS4!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F69bfee26-ea3b-42a6-8a1a-6b8187852082_738x564.png)](https://magazine.sebastianraschka.com/p/understanding-and-coding-self-attention)

The article is also very technical and likely too detailed for some. However, I really wanted to have this foundation out there for future articles to reference. And no worries, the next one will return to a more comfortable style.

**PS: I started experimenting with Substack’s new “perks” system. As a thank-you to supporters, I’ve added [image exports and metadata](https://magazine.sebastianraschka.com/perks) from the architecture gallery for you to use in your own analyses.**

PPS: If you are already familiar with how position embeddings work, feel free to fast-forward to Section 6, *Positional choices in modern architectures*.

Note that this article may be best read in a web browser than an email client. I recommend checking out the [web view here](https://magazine.sebastianraschka.com/p/understanding-positional-embeddings), which also has a navigation bar on the left side.

Let’s start with the overall architecture. A tokenizer encodes the input text into token IDs. Next, the input token embedding layer of an LLM maps each of these IDs to a vector of numbers. We usually refer to these entries in that vector as “features” (similar to classic ML). The transformer blocks inside the LLM then modify these vectors using attention and feedforward layers. Lastly, the model’s output layer produces logit scores (converted into probabilities) for the next token.

The following shows this path in GPT-2 (a relatively classic LLM) and MiniMax-M3 (a more recent LLM released this year; a placeholder for most other recent LLMs). As we can see, the architectures are not too different from each other, but before we get to the positional information parts, let’s focus on the orange blocks for now (the token embedding layers) and the transformer blocks above them; the other architecture differences aren’t needed for this explanation.

[![Figure 2 — token embedding architectures](https://substackcdn.com/image/fetch/$s_!3vjo!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4ab287c2-c3ba-46db-9ed0-1e6eacf78f43_2592x2521.png "Figure 2 — token embedding architectures")](https://substackcdn.com/image/fetch/$s_!3vjo!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4ab287c2-c3ba-46db-9ed0-1e6eacf78f43_2592x2521.png)

Figure 2: Token embedding layers in GPT-2 and MiniMax-M3, highlighted in orange. (Side note: M3’s first three out of 60 feedforward layers are dense not MoEs.)

A token embedding layer, shown in the figure above, maps each token ID to a vector. Position information is then added afterward. Concretely, and as we will discuss in more detail later, GPT-2 adds a position vector to each token embedding before the modified embedding enters the first transformer block. On the other hand, MiniMax-M3 adds the positional information in each transformer block using a method called RoPE inside attention.

We will talk about these methods in more detail later, but first, let’s start with the big picture information explaining why explicit positional information is added or needed.

Consider the following two input sentences as a simple example:

1. “The red fox jumps over the gray fox.”

2. “The gray fox jumps over the red fox.”

Changing the word order changes which fox does the jumping, as illustrated in the following figure.

[![Figure 3 — word order](https://substackcdn.com/image/fetch/$s_!KoQ2!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb8ff5039-8ec8-447c-a444-0345619c8b7b_1846x1304.png "Figure 3 — word order")](https://substackcdn.com/image/fetch/$s_!KoQ2!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb8ff5039-8ec8-447c-a444-0345619c8b7b_1846x1304.png)

Figure 3: Changing the word order changes which fox jumps.

For the examples in this article, we will treat each word as one token for simplicity. (As you know, real tokenizers can split words into smaller subword tokens.) The first sentence then has eight token positions, from 0 through 7.

![Figure 4 — token ids and positions](https://substackcdn.com/image/fetch/$s_!Bxjt!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff5386e76-8459-4196-a797-430e922fce3b_2238x580.png "Figure 4 — token ids and positions")

Figure 4: Token IDs and positions in the toy sentence.

(If you are interested in how vocabularies are built, check out my [tokenizer materials on GitHub](https://github.com/rasbt/LLMs-from-scratch/tree/main/ch02/05_bpe-from-scratch).)

Note that “fox”, which appears twice in the input sentence, gets the same token ID 2 in both cases. And since they both have the same token ID, irrespective of the position in the input sentence, they would get the same embedding vector from the embedding layer.

Now, let’s consider a regular self-attention mechanism with no positional encoding (and no causal mask), where every input token can attend to every other token in the input. In this scenario, swapping any two words, like “red” and “gray”, would still result in the same embeddings and attention scores between each token pair.

GPT-style models do use a causal (autoregressive) mask, which restricts each token to itself and earlier tokens. Interestingly, this causal mask gives the computation some implicit order information. In our example, “jumps” can attend to “red” in the first sentence and “gray” in the second. So, a causal LLM can theoretically already distinguish these sentences even without explicit positional embeddings. (We will return to this idea later when discussing the “NoPE” concept.)

Nonetheless, GPT-2 and most newer LLMs include explicit positional information because it gives attention a direct signal about positions. That’s because even though the causal mask determines which tokens are accessible and provides implicit order information, it doesn’t distinguish a separation of one position from a separation of ten, for example. So, explicit positional methods are still useful (or even essential), since they supply that information.

If this all sounded confusing, don’t worry, it was more meant as an overview, and we will step through these concepts a bit more slowly in the remaining article.

We briefly talked about *implicit* positional information through causal attention masks (and we will revisit this again in more detail later). However, we now switch gears and talk about the *explicit* ways to add positional information.

The perhaps simplest and most direct (but not “cheapest”) approach is to give each sequence position its own vector. We add that vector to the token embedding, so the resulting, modified embedding vector is then passed to the first transformer block. This exact procedure is discussed in the following subsections.

To illustrate the use of an explicit, learned positional vector to the input embeddings, let’s consider GPT-2 as a concrete example. GPT-2 learns a table of position vectors during training. The following figure illustrates where these vectors are used in the architecture, that is, immediately after the token embedding lookup and before the first transformer block.

[![Figure 5 — gpt2 architecture](https://substackcdn.com/image/fetch/$s_!GDv8!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fda2b0f15-7f45-4aa4-8f1b-a0777a0da4fc_1710x2197.png "Figure 5 — gpt2 architecture")](https://substackcdn.com/image/fetch/$s_!GDv8!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fda2b0f15-7f45-4aa4-8f1b-a0777a0da4fc_1710x2197.png)

Figure 5: GPT-2 adds absolute position information before the transformer blocks, as highlighted in purple. The dimensions shown are for [GPT-2 XL](https://huggingface.co/openai-community/gpt2-xl/blob/main/config.json).

The positional embedding layer depicted in the figure above follows the same implementation as the token embedding layer, i.e., they have exactly the same output dimensions so that the resulting input vectors can be added to each other. The code below shows that with an excerpt of a GPT-2 code implementation:

And in mathematical terms, we can write it as follows. Let *m* be a position index and *xm* the token ID at that position. The input to the first block is

![Equation 1: h_m = E_{\text{token}}(x_m) + P_m.](https://substackcdn.com/image/fetch/$s_!AjJR!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F10fa6ac9-e4e7-4b16-b4c3-43d1671030c3_2240x79.png "Equation 1: h_m = E_{\text{token}}(x_m) + P_m.")

Here,

- *E*token is the embedding layer,

- *Pm* is the position info vector selected by *m*,

- and *hm* is the resulting summed embedding and position vector.

The two vectors have the same width, so we add their entries element-wise. Figure 6 below shows this visually with a small numerical example.

[![Figure 6 — absolute embeddings](https://substackcdn.com/image/fetch/$s_!yuT0!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F546edc50-7b1a-40ba-a125-da3fbdaade26_2586x964.png "Figure 6 — absolute embeddings")](https://substackcdn.com/image/fetch/$s_!yuT0!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F546edc50-7b1a-40ba-a125-da3fbdaade26_2586x964.png)

Figure 6: Token and position embeddings are added element-wise. (I am following the convention here that neural network diagrams are usually read from bottom to top.)

In the figure above, at position 0, adding the token embedding `[1, 1, 1]` to the position embedding `[1.1, 1.2, 1.3]` gives `[2.1, 2.2, 2.3]`. By the way, the values in the figure are chosen to keep the illustration simple; in a real example, both embedding layers (tables) are initialized with small random values and learned during training.

So, what we covered in this section is a so-called *absolute* positional method because the positional embedding *Pm* depends on the token’s own position *m*. In the previous example sentence, the two “fox” tokens have initially the same token embeddings, but because we are adding different positional vectors *P2* and *P7*, they have different values before they enter the first transformer block. And consequently, the query, key, and value vectors inside the attention mechanisms then also receive representations that already include position information.

Note that the original [GPT-2 implementation](https://github.com/openai/gpt-2/blob/master/src/model.py) has 1,024 learned position vectors. This means it only works with inputs with 1024 or fewer tokens. If we want to extend the context window, we would have to extend this embedding table. And since the embedding table (layer) is updated like any other layer during training, the context length would have to be decided before the training starts.

Extending the context would be very expensive since the table can become very large. For example, in the case of GPT-2 Medium, the positional embedding layer weights only make up 0.30% of the total weights at 1,024 positions. However, if we increased the context length to a nowadays common 100k range, they would make up >22% of the total model weights, which would be very uneconomic.

The original [“Attention Is All You Need”](https://arxiv.org/abs/1706.03762) transformer from 2017, which predates GPT, uses the same addition mechanism, but computes its position vectors with fixed sine and cosine functions. Figure 7 below shows this addition before both the encoder and decoder stacks. The encoder processes the input sequence, and the decoder generates the output sequence. (The reason why it’s introduced here is that the sine and cosine functions will become relevant again later when discussing RoPE.)

[![Figure 7 — original transformer architecture](https://substackcdn.com/image/fetch/$s_!d7HH!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5958afa7-673d-44df-a1af-112044196dde_2620x2800.png "Figure 7 — original transformer architecture")](https://substackcdn.com/image/fetch/$s_!d7HH!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5958afa7-673d-44df-a1af-112044196dde_2620x2800.png)

Figure 7: Sinusoidal position encodings added to the token embeddings as highlighted in purple.

The original transformer also scales its token embeddings by √(*d*model) before adding the position vectors, where *d*model is the embedding width.

So, while GPT-2 in the previous section had the formula

![Equation 2: h_m = E_{\text{token}}(x_m) + P_m.](https://substackcdn.com/image/fetch/$s_!AjJR!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F10fa6ac9-e4e7-4b16-b4c3-43d1671030c3_2240x79.png "Equation 2: h_m = E_{\text{token}}(x_m) + P_m.")

the formula used in the original transformer (depicted in Figure 7) is:

![Equation 3: h_m = \sqrt{d_{\text{model}}} E_{\text{token}}(x_m) + P_m.](https://substackcdn.com/image/fetch/$s_!08dk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa42a0af2-997b-49a7-87e6-3beaf7788bae_2240x95.png "Equation 3: h_m = \sqrt{d_{\text{model}}} E_{\text{token}}(x_m) + P_m.")

This scaling by √(*d*model) is a minor technical detail, but for those who are curious, the motivation behind this scaling factor is not explained in the transformer paper, but as I discussed with colleagues on a Twitter thread many years ago, it’s likely to put the token embeddings and sinusoidal positional encodings on comparable numerical scales at initialization, so the position vectors don’t dominate the sum when we add them.

In the GPT-2 case, this scaling wasn’t necessary because the original GPT-2 code initialized the token embeddings to have an initial standard deviation of 0.02 and the position embeddings to have an initial standard deviation of 0.01 so that they were already roughly on the same scale.

Now, the more interesting point is how the position embeddings *Pm* are computed.

Concretely, each position vector has the following entries:

![Equation 4: \begin{aligned} P_{m,2i} &= \sin\left(\frac{m}{10000^{2i/d_{\text{model}}}}\right), \\ P_{m,2i+1} &= \cos\left(\frac{m}{10000^{2i/d_{\text{model}}}}\right). \end{aligned}](https://substackcdn.com/image/fetch/$s_!Z3Wh!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F00bc80dd-1b3c-4c66-90c0-111bfaf26d2d_2240x296.png "Equation 4: \begin{aligned} P_{m,2i} &= \sin\left(\frac{m}{10000^{2i/d_{\text{model}}}}\right), \\ P_{m,2i+1} &= \cos\left(\frac{m}{10000^{2i/d_{\text{model}}}}\right). \end{aligned}")

As before, *m* denotes the position. The index *i* indexes a pair of features within the vector.

Here, a feature is one number in the position vector. For example, let’s assume we have a small vector with 8 values (*d*model = 8) that has entries numbered 0 through 7. (In the original paper this was 512 for the small model and 1024 for the bigger model, instead of 8, but that would be hard to visualize.) We group these into four pairs so that *i* goes from 0 through 3.

[![Figure 8 — sinusoidal feature pairs](https://substackcdn.com/image/fetch/$s_!HkhF!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe441fd8e-780b-4f66-9254-8ee2a7994a4f_1593x773.png "Figure 8 — sinusoidal feature pairs")](https://substackcdn.com/image/fetch/$s_!HkhF!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe441fd8e-780b-4f66-9254-8ee2a7994a4f_1593x773.png)

Figure 8: Sine and cosine entries for each feature pair.

Here, the indices 2*i* and 2*i* + 1 select the two entries corresponding to pair *i*.

As shown in the equation earlier, the angle at position *m*, which is then fed into the sine or cosine functions to obtain *Pm*, 2*i* or *Pm*, 2*i* + 1, respectively, is computed as follows:

![Figure 9 — angle and frequency](https://substackcdn.com/image/fetch/$s_!9Nx0!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fae693dce-b196-480f-ace0-2d8a888bf745_2234x806.png "Figure 9 — angle and frequency")

Figure 9: The angle inside sine and cosine is the token position multiplied by the pair’s angular frequency.

Figure 10 shows this calculation for all four pairs of our 8-feature example at position *m* = 2.

[![Figure 10 — sinusoidal calculations](https://substackcdn.com/image/fetch/$s_!63Px!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa6e0a4d2-6a7c-443d-9be0-102807f8da7a_2600x1063.png "Figure 10 — sinusoidal calculations")](https://substackcdn.com/image/fetch/$s_!63Px!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa6e0a4d2-6a7c-443d-9be0-102807f8da7a_2600x1063.png)

Figure 10: The four sine and cosine pairs at position 2.

For an eight-feature embedding, the four frequencies are 1, 0.1, 0.01, and 0.001 radians per position. At position 2, the first pair in row 1 is (sin 2, cos 2) ≈ (0.909, −0.416).

The second pair, in row 2, changes more slowly, giving (sin 0.2, cos 0.2) ≈ (0.199, 0.980).

The figure below visualizes the sine and cosine calculations for different pairs, which are then collected in a position vector.

[![Figure 11 — sinusoidal encoding](https://substackcdn.com/image/fetch/$s_!UhK9!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2fd5a766-4363-4189-9675-bcf9bd8e700d_2624x2023.png "Figure 11 — sinusoidal encoding")](https://substackcdn.com/image/fetch/$s_!UhK9!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2fd5a766-4363-4189-9675-bcf9bd8e700d_2624x2023.png)

Figure 11: Sampling sine and cosine functions to form a position vector.

In short, this method is a (more complicated) alternative to obtaining positional embedding vectors compared to the GPT-2 method discussed earlier.

Similar to the GPT-2 embeddings, these are also absolute embeddings because each vector is computed from its own position. However, in contrast to the GPT-2 approach, the sine and cosine functions stay fixed during training. (Also, they can be extended and evaluated beyond the training positions, like beyond the context length of 1024, although that alone doesn’t guarantee that the model will perform well there, because it hasn’t seen those during training.)

To summarize, Figure 12 below compares the two ways of obtaining a position vector we discussed so far.

[![Figure 12 — fixed vs learned positions](https://substackcdn.com/image/fetch/$s_!jj3h!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3032a17a-5c43-487e-bcce-560d0a957b12_2616x1804.png "Figure 12 — fixed vs learned positions")](https://substackcdn.com/image/fetch/$s_!jj3h!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3032a17a-5c43-487e-bcce-560d0a957b12_2616x1804.png)

Figure 12: Fixed sinusoidal and learned absolute positional embeddings.

This sinusoidal method may look like an overly complicated way to obtain positional embeddings, however, understanding it will be useful when talking about RoPE in the next section.

Also, there is some rationale behind it. The authors chose sine and cosine because they make the shifting by a certain offset gives a certain deterministic difference. For example, the difference between position 2 to 5 and the difference between 102 to 105 is both 3, which then results in the same rotation.

According to the [Attention Is All You Need](https://arxiv.org/abs/1706.03762) paper:

> “...easily learn to attend by relative positions, since for any fixed offset *k*, *PE*pos + *k* can be represented as a linear function of *PE*pos.”

Or in less technical terms, the authors hypothesized that this property would make it easier for attention to learn relationships such as “three tokens earlier.”

Figure 13 below illustrates this for one sine/cosine pair. A rotation is a linear transformation, and both three-position shifts use the same rotation.

[![Figure 13 — sinusoidal relative shift](https://substackcdn.com/image/fetch/$s_!PFDm!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8cfa51c1-e3d5-4791-9899-4f159d43e890_2453x1299.png "Figure 13 — sinusoidal relative shift")](https://substackcdn.com/image/fetch/$s_!PFDm!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8cfa51c1-e3d5-4791-9899-4f159d43e890_2453x1299.png)

Figure 13: For a pair with angular frequency 0.1 radians per position, a shift of three positions always rotates the pair by 0.3 radians.

So, the sinusoidal method above is still considered an absolute positional encoding because the vector added to each token embedding represents that token’s position counted from the start of the sequence.

As shown in the previous figure, for example, tokens at positions 2 and 5 receive the position vectors *P2* and *P5*. If we move those tokens to positions 102 and 105, they’re still three positions apart. But each token gets a different position vector because its absolute position *m* has changed.

So their attention score can change even when the token embeddings stay the same. The sinusoidal structure still gives the model a way to learn relative relationships.

Anyways, RoPE, which we will discuss below, builds this relative relationship directly into the attention calculation. RoPE, which [Su et al. introduced in the 2021 RoFormer paper](https://arxiv.org/abs/2104.09864), stands for Rotary Position Embedding and is considered a relative position method.

In attention, each token’s current representation is projected into a query, a key, and a value vector. Query-key dot products determine attention scores. After masking and softmax, the resulting weights determine how the value vectors are mixed. For a refresher, see my [Understanding and Coding Self-Attention, Multi-Head Attention, Causal-Attention, and Cross-Attention in LLMs](https://magazine.sebastianraschka.com/p/understanding-and-coding-self-attention) article:

[![Understanding and Coding Self-Attention, Multi-Head Attention, Causal-Attention, and Cross-Attention in LLMs](https://substackcdn.com/image/fetch/$s_!3NS4!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F69bfee26-ea3b-42a6-8a1a-6b8187852082_738x564.png)](https://magazine.sebastianraschka.com/p/understanding-and-coding-self-attention)

RoPE uses the token’s position to rotate its query and key vectors before their dot product. The value vectors bypass this rotation. Figure 14 below shows an overview of where RoPE is applied in a relatively recent MiniMax-M3, where QK-Norm normalizes queries and keys before RoPE.

[![Figure 14 — minimax m3 architecture](https://substackcdn.com/image/fetch/$s_!xNEB!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F309c52ca-ad27-4336-a463-cde0050b3425_1764x2311.png "Figure 14 — minimax m3 architecture")](https://substackcdn.com/image/fetch/$s_!xNEB!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F309c52ca-ad27-4336-a463-cde0050b3425_1764x2311.png)

Figure 14: MiniMax-M3 applies RoPE to queries and keys inside attention.

As shown in the figure above, the LLM architecture has no explicit position vector added alongside the initial token embedding. Its explicit position signal is injected inside attention instead.

Side note: MiniMax-M3 rotates only part of each query and key head, a variation known as “partial RoPE” that we will cover later in Section 6.1. But to understand the basic RoPE operation first, let’s walk through the method step by step using a small example.

Attention usually divides its query, key, and value features into multiple heads, which are processed in parallel. Hence, the full mechanism is usually called Multi-head Attention (note that Grouped Query Attention also uses multiple heads). Here, each attention head computes its own attention scores and weighted sums. RoPE then acts on pairs of numbers within each query and key head.

To make this concrete, Figure 12 below follows a token representation through the learned query and key projections, which are computed via the weight matrices *WQ* and *WK*. In this toy example, each projection produces 16 features, which are reshaped into two heads with eight features each. In the figure below, every small cell contains one scalar feature, meaning one numerical entry in a vector (the numbers are not shown due to space reasons).

[![Figure 15 — inputs and pairs](https://substackcdn.com/image/fetch/$s_!0J_2!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3a99d70f-f237-4af5-8f0d-9f672e8193a7_2755x2720.png "Figure 15 — inputs and pairs")](https://substackcdn.com/image/fetch/$s_!0J_2!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3a99d70f-f237-4af5-8f0d-9f672e8193a7_2755x2720.png)

Figure 15: Projecting a hidden state, reshaping into heads, and pairing features for RoPE.

For the first eight-feature head, `[1, 2, 3, 4, 5, 6, 7, 8]`, we pair each feature in the first half with the corresponding feature in the second half. This gives `(1, 5)`, `(2, 6)`, `(3, 7)`, and `(4, 8)`. This *split-half* convention is used in LLM implementations, including my [Qwen3 implementation](https://github.com/rasbt/LLMs-from-scratch/blob/main/ch05/11_qwen3/standalone-qwen3.ipynb). (We will compare it with the original, alternative adjacent-feature pairing method later after implementing the basic operation.)

Each pair gets a rotation angle determined by the token position *m* and that pair’s frequency *θi*. The corresponding query and key pairs use the same frequency. Figure 16 below shows how we rotate the pairs, put the updated features back in their original locations, and compute the query-key dot product.

[![Figure 16 — position rotations](https://substackcdn.com/image/fetch/$s_!LQ-n!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe010e216-c965-4313-ace6-6682016a3177_2671x1543.png "Figure 16 — position rotations")](https://substackcdn.com/image/fetch/$s_!LQ-n!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe010e216-c965-4313-ace6-6682016a3177_2671x1543.png)

Figure 16: Pairwise rotations produce the rotated query and key vectors used to compute attention scores.

The next section will walk through the figure(s) a bit more slowly.

For the following example, we’ll use the same two positions as before. The query comes from “the” at position *m* = 5, and the key comes from the first “fox” at position *n* = 2. Since, in this example, the key comes before the query, the causal mask allows the query to attend to it.

To illustrate the effect that the position has, and keeping everything fixed, let’s assume we start both pairs from the same two values, (1, 5).

![Figure 17 — query pair closeup](https://substackcdn.com/image/fetch/$s_!LgZZ!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6690385d-65a0-41e7-b2f8-7d0ce501c264_2650x461.png "Figure 17 — query pair closeup")

Figure 17: The first feature pair, (1, 5), takes one entry from each half of the query head.

Furthermore, for this example, let’s use *θ* = 20° per token position (*π* /9 radians per position). At position 2, we rotate the key’s arrow counterclockwise by 2 × 20° = 40°. At position 5, we rotate the query’s arrow by 5 × 20° = 100°.

[![Figure 18 — rotations at positions 2 and 5](https://substackcdn.com/image/fetch/$s_!RhWQ!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F03367132-6eb1-44cd-85fd-afb47314c6a9_2672x1716.png "Figure 18 — rotations at positions 2 and 5")](https://substackcdn.com/image/fetch/$s_!RhWQ!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F03367132-6eb1-44cd-85fd-afb47314c6a9_2672x1716.png)

Figure 18: Starting from (1, 5), RoPE rotates the key pair at position 2 by 40° and the query pair at position 5 by 100°.

To illustrate what’s happening in the figure above, for example, plugging cos 40° ≈ 0.766 and sin 40° ≈ 0.643 into the rotation from Figure 16 gives the new key coordinates:

![Equation 5: k' = (1\cos40^\circ - 5\sin40^\circ,\;1\sin40^\circ+5\cos40^\circ) \approx (-2.448,\;4.473).](https://substackcdn.com/image/fetch/$s_!DBMJ!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fcfe39910-8f5a-4edf-88e2-018f960a0435_2240x82.png "Equation 5: k' = (1\cos40^\circ - 5\sin40^\circ,\;1\sin40^\circ+5\cos40^\circ) \approx (-2.448,\;4.473).")

Then, changing 40° to 100°, using the same equation as above, the rotated query pair becomes *q′* ≈ (−5.098, 0.117). But the important point is that both arrows still have length √26. Only the rotation changes but the vectors keep the same lengths after the transformation.

So, for a query pair (*a*, *b*) at position *m*, the two operations here are.

![Equation 6: \begin{aligned} a' &= a\cos(m\theta)-b\sin(m\theta), \\ b' &= a\sin(m\theta)+b\cos(m\theta). \end{aligned}](https://substackcdn.com/image/fetch/$s_!YiBj!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6de4a384-3c18-4bbc-9346-08ae327ee6f2_2240x154.png "Equation 6: \begin{aligned} a' &= a\cos(m\theta)-b\sin(m\theta), \\ b' &= a\sin(m\theta)+b\cos(m\theta). \end{aligned}")

For the key pair at position *n*, we use the same equations with *nθ*.

![Equation 7: \begin{aligned}a' &= a\cos(n\theta)-b\sin(n\theta), \\b' &= a\sin(n\theta)+b\cos(n\theta).\end{aligned}](https://substackcdn.com/image/fetch/$s_!HQdc!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0eccfd5c-a1f5-4f7f-b40a-4ec197d7a218_2240x154.png "Equation 7: \begin{aligned}a' &= a\cos(n\theta)-b\sin(n\theta), \\b' &= a\sin(n\theta)+b\cos(n\theta).\end{aligned}")

(Section 4 later shows how RoPE calculates the angular frequency for each feature pair.)

To close this section, for those more comfortable reading code, everything about RoPE illustrated so far can be written in code, as follows:

The resulting values are:

```
Original key:   (1, 5)
Original query: (1, 5)
Rotated key:    (-2.448, 4.473)
Rotated query:  (-5.098, 0.117)
```

If we rotate all 4 pairs in the 8-feature query (and key) vector(s), we get the vectors below. (We’ll come back to how we go from rotating one pair to rotating all of them later.)

```
Original key:   (1, 2, 3, 4, 5, 6, 7, 8)
Original query: (1, 2, 3, 4, 5, 6, 7, 8)
Rotated key:    (-2.448, 1.577, 2.951, 3.994, 4.473, 6.125, 7.021, 8.003)
Rotated query:  (-5.098, 0.928, 2.877, 3.986, 0.117, 6.256, 7.051, 8.007)
```

Note that, as shown above, the query and key heads have eight features each, which we previously grouped into 4 pairs with two elements each, like (1, 5), and (2, 6), and so on. For simplicity here we’ll follow just the first pair in each head, (1, 5), formed by taking the entries at indices 0 and 4:

```
Original key:   (1, 5)
Original query: (1, 5)
Rotated key:    (-2.448, 4.473)
Rotated query:  (-5.098, 0.117)
```

As shown above, in our example, those entries have values (1, 5) in both heads before the rotation.

Also, as in the code above, let’s use the notation *q* =(*q1*, *q2*) and *k* =(*k1*, *k2*) for these two pairs. To compute their dot product, using the standard dot product formula, we multiply corresponding coordinates and add the products:

![Equation 8: q\cdot k=q_1k_1+q_2k_2.](https://substackcdn.com/image/fetch/$s_!NqAC!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff26d2b45-d5fd-49cb-ae1c-baae68e98d77_2240x73.png "Equation 8: q\cdot k=q_1k_1+q_2k_2.")

Before rotation, the pairs have a dot product of 1 × 1 + 5 × 5 = 26. Using the rounded coordinates from the previous Figure 18 gives the contribution from the first pair after rotation:

![Equation 9: q'\cdot k'\approx(-5.098)(-2.448)+(0.117)(4.473)\approx13.00.](https://substackcdn.com/image/fetch/$s_!scnp!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd175c135-b520-49e6-8d10-f18888e3743c_2240x82.png "Equation 9: q'\cdot k'\approx(-5.098)(-2.448)+(0.117)(4.473)\approx13.00.")

We can see why by writing the dot product in terms of the lengths of the two vectors (arrows in Figure 18) and the angle *ϕ* between them.

![Equation 10: q'\cdot k'=\lVert q'\rVert\,\lVert k'\rVert\cos\phi.](https://substackcdn.com/image/fetch/$s_!GpbH!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F176ef9de-46fa-4bf4-8a1c-a90d14c13874_2240x82.png "Equation 10: q'\cdot k'=\lVert q'\rVert\,\lVert k'\rVert\cos\phi.")

In both cases, the vector length is √26. The query (position 5) is rotated by 100° and the key (position 2) is rotated by 40°. The vectors (arrows) start from the same direction, so they end up being 60° apart after rotation. Their exact dot product is 26 × cos(60°) = 13.

Now let’s see what happens if we move just the key from position 2 to position 3, but we keep its starting coordinates (1, 5) fixed. The key’s rotation changes now from 2 × 20° = 40° to 3 × 20° = 60°. The query still rotates by 100°, so the angle between the arrows shrinks to 40° and the dot product changes from 13 to 26 × cos(40°) ≈ 19.92. So, moving the key one position closer changed the dot product even though we started with the same feature values.

Why do these dot products matter? In attention, we sum the contributions from all feature pairs to get the query-key dot products. RoPE can change those dot products and thus the weights that softmax assigns to the value vectors.

![Equation 11: \operatorname{Attention}(Q',K',V) =\operatorname{softmax}\!\left(\frac{Q'(K')^\top}{\sqrt{d}}\right)V.](https://substackcdn.com/image/fetch/$s_!haoa!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe2351921-4dc0-4904-9d35-941e3de77a9f_2240x162.png "Equation 11: \operatorname{Attention}(Q',K',V) =\operatorname{softmax}\!\left(\frac{Q'(K')^\top}{\sqrt{d}}\right)V.")

In the attention equation above, *Q′* and *K′* are matrices that contain the rotated queries *q′* and keys *k′* for one head, *V* contains the values, and *d* is the head dimension.

Now that we illustrated the computation of the dot products, which is at the core of the attention mechanism, let’s return to positions *m* = 5 and *n* = 2. If we shift both positions to the right by three positions, we get *m* = 8 and *n* = 5. Each rotation gains 60° (the query goes from 100° to 160°, and the key goes from 40° to 100°), however, the angle between the vectors stays the same. The dot product for this feature pair is still 13.

[![Figure 19 — position shift dot products](https://substackcdn.com/image/fetch/$s_!2bNG!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5c627967-3788-4e17-85ef-8d6e716d2284_2525x785.png "Figure 19 — position shift dot products")](https://substackcdn.com/image/fetch/$s_!2bNG!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5c627967-3788-4e17-85ef-8d6e716d2284_2525x785.png)

Figure 19: The first pair’s dot product at three different position settings.

The key is that the difference between the rotation angles is

![Equation 12: n\theta-m\theta=(n-m)\theta.](https://substackcdn.com/image/fetch/$s_!Q7jd!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0a220d40-c355-48e0-9bfc-6318fe46e2ef_2240x79.png "Equation 12: n\theta-m\theta=(n-m)\theta.")

If we keep the original query and key vectors fixed and shift both token positions by the same amount, RoPE adds the same rotation to both vectors. The angle between them stays the same, so their dot product stays the same. Note that Figure 20 below uses smaller angles than the actual example (20° per position) to keep the picture readable.

[![Figure 20 — relative position](https://substackcdn.com/image/fetch/$s_!jxZL!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F88495cb6-f380-43db-aeab-0f50afbd41b0_2610x888.png "Figure 20 — relative position")](https://substackcdn.com/image/fetch/$s_!jxZL!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F88495cb6-f380-43db-aeab-0f50afbd41b0_2610x888.png)

Figure 20: A shared position shift preserves the relative angle and dot product for fixed query and key features.

The examples above and Figure 20 illustrate the main idea behind RoPE. We use each token’s position to rotate the corresponding query and key in the attention module. In the query-key dot product, what matters is how much one vector rotates relative to the other. For each feature pair, this difference in rotation depends only on the offset between the two tokens.

To illustrate this in the big picture context, in Figure 21, each row collects the scores for one query. If we consider row 5, the query for “the”, and column 2, the key for “fox”, the attention score between those two is computed as the dot product between the two rotated versions of the query and key vector.

[![Figure 21 — query key score map](https://substackcdn.com/image/fetch/$s_!0dw5!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2a413998-3f9a-477b-9fb6-d475bdedf847_2791x1962.png "Figure 21 — query key score map")](https://substackcdn.com/image/fetch/$s_!0dw5!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2a413998-3f9a-477b-9fb6-d475bdedf847_2791x1962.png)

Figure 21: The attention scores for one head before softmax. Gray cells mark future keys excluded by the causal mask.

What’s the point, again? Let’s once more consider “The red fox jumps over the gray fox.” If we add 100 tokens (e.g., think system prompt) before the sentence, the first “fox” and the second “the” move from positions 2 and 5 to 102 and 105. However, a prefix of any length shifts both positions equally, so they still stay exactly three positions apart. This means, each vector (arrow) gains the exact same additional rotation, which preserves the angle between them and their dot product (assuming we hold the unrotated query and key vectors fixed; in a real model, a new prefix of 100 tokens can change those vectors by changing what the tokens attend to).

Figure 22 tracks this comparison in the attention score matrix. The right panel shows the rows and columns for the sentence after we add the prefix. In our toy example, the first pair still contributes 13 to the dot product.

[![Figure 22 — shifted query key score map](https://substackcdn.com/image/fetch/$s_!yQkh!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F19ac0a71-c4d4-4cbe-b1ec-04371b5b3a58_2691x1745.png "Figure 22 — shifted query key score map")](https://substackcdn.com/image/fetch/$s_!yQkh!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F19ac0a71-c4d4-4cbe-b1ec-04371b5b3a58_2691x1745.png)

Figure 22: The highlighted score moves from row 5, column 2 to row 105, column 102. It keeps the same value if we hold the query and key vectors fixed before rotation.

As you may recall, in the original transformer’s sinusoidal encoding, a fixed offset also corresponds to a fixed rotation within each sine/cosine pair. But that shared rotation applies only to the position vectors (added to the input vector) instead of the key and query vectors inside the attention mechanism.

Figure 23 compares what this shift changes in the three methods we covered so far.

[![Figure 23 — position shift comparison](https://substackcdn.com/image/fetch/$s_!qAmy!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fbf8cae0f-aa03-454c-b94e-dde8dd5ae680_2640x2488.png "Figure 23 — position shift comparison")](https://substackcdn.com/image/fetch/$s_!qAmy!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fbf8cae0f-aa03-454c-b94e-dde8dd5ae680_2640x2488.png)

Figure 23: The same position shift under sinusoidal encodings, GPT-2’s learned position embeddings, and RoPE.

To see how the relative offset affects the dot product, we focused on one feature pair in the query and key. Now let’s put all four pairs together and see how RoPE rotates the full eight-feature vectors in one attention head. (Please feel free to skip this section if it is too much technical detail.)

Figure 24 picks up where Figure 17 left off. We rotate each of the four pairs, then put the results back in the query’s original feature order.

[![Figure 24 — four pair rotation](https://substackcdn.com/image/fetch/$s_!j9TT!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F114f482b-e080-4ce2-9698-f489a9b305eb_2774x2836.png "Figure 24 — four pair rotation")](https://substackcdn.com/image/fetch/$s_!j9TT!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F114f482b-e080-4ce2-9698-f489a9b305eb_2774x2836.png)

Figure 24: RoPE rotates each pair of query features at its own angle. (The same can be applied to the key, which is not shown to keep the illustration simple).

Figure 24 above shows the rotated query *q′* (at the bottom). Similarly, we could get the rotated key *k′* by repeating the same steps at the key’s position.

Recall the two equations from Section 3.3. For query pair *i* with input values (*ai*, *bi*), let *ci* = cos(*mθi*) and *si* = sin(*mθi*). These are the cosine and sine of the rotation angle for that pair at query position *m*:

![Equation 13: \begin{aligned} a_i' &= c_i a_i - s_i b_i, \\ b_i' &= s_i a_i + c_i b_i. \end{aligned}](https://substackcdn.com/image/fetch/$s_!ee1I!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fdd9437a4-f85c-4e46-b7c7-673e39afd841_2240x155.png "Equation 13: \begin{aligned} a_i' &= c_i a_i - s_i b_i, \\ b_i' &= s_i a_i + c_i b_i. \end{aligned}")

Figure 25 below shows the same four rotations between the four pairs as a single matrix multiplication, *q′* = *Rmq*.

[![Figure 25 — full rotation matrix](https://substackcdn.com/image/fetch/$s_!yWYT!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5fd7a9e5-0263-4d31-bae4-787834c245b7_2634x2850.png "Figure 25 — full rotation matrix")](https://substackcdn.com/image/fetch/$s_!yWYT!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5fd7a9e5-0263-4d31-bae4-787834c245b7_2634x2850.png)

Figure 25: The four rotations from Figure 24 written as one matrix multiplication. The pair labels show where each rotation goes in the full matrix.

A full attention head uses multiple frequencies so that some feature pairs change more drastically (”quickly” in RoPE terms) with position and others change more gradually (”slowly” in RoPE terms). This serves the same purpose as the multiple frequencies in the original transformer’s sinusoidal encodings.

However, instead of us assigning these manually, for a head with *d* features, standard full RoPE computes one frequency for each of its *d* /2 pairs as follows:

![Equation 14: \theta_i=b^{-2i/d}.](https://substackcdn.com/image/fetch/$s_!UFXx!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Febff1490-d57b-4eb3-a837-0a3426a18f0c_2240x84.png "Equation 14: \theta_i=b^{-2i/d}.")

Here,

- *d* is the head dimension (it has to be even so that all features can be arranged in pairs).

- *i* is the index of the pair; it ranges from 0 to *d* /2−1.

- *b* is a chosen base value (hyperparameter) that determines the spacing of the frequencies.

- *θi* is pair *i* ‘s frequency in radians per token position.

The original [RoFormer paper](https://arxiv.org/html/2104.09864v5#S3.SS3) uses *b* = 10,000, borrowing the original transformer’s frequency formula with head dimension *d* in place of embedding width *d*model.

For example, with head dimension *d* = 8 and base *b* = 10,000, we get frequencies \[1, 0.1, 0.01, 0.001\] radians per position.

If you want to see RoPE in a complete model, I recommend checking out my end-to-end [Qwen3 from-scratch implementation](https://github.com/rasbt/LLMs-from-scratch/blob/main/ch05/11_qwen3/standalone-qwen3.ipynb).

Since code can be an effective medium for explaining and understanding concepts, the following subsection will walk you through the core parts that we have covered so far, if you are interested. Otherwise, please feel free to fast forward to section 6, on *Positional choices in modern architectures*.

So, for the following code snippets, we’ll use the same split-half rotation convention as mentioned earlier (the interleaved convention will also be covered, no worries). First, we’ll apply it to one query vector, then we’ll use it on queries and keys inside the attention mechanism.

First, let’s reuse the 8-feature query from Figure 24. For convenience, below are its four feature pairs before rotation.

![Detail from Figure 24 — original query pairs](https://substackcdn.com/image/fetch/$s_!Wytv!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4bea76ec-3543-4b3a-a9ff-870d91b41255_1800x178.png "Detail from Figure 24 — original query pairs")

Detail from Figure 24. The eight query features form four pairs in the split-half layout.

In the earlier non-coding example, we chose a frequency of 20° per token position for the first pair to keep the math simple. Here, we’ll use the original RoPE formula from the previous Section 4 above, *θi* = *b*−2*i*/*d*, with head dimension *d* = 8 and base *b* = 10,000.

Figure 26 shows how the two functions work together to apply these rotations.

[![Figure 26 — pytorch walkthrough](https://substackcdn.com/image/fetch/$s_!OZLU!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3b99b926-16c6-443c-bcb0-155ba1e63fc2_2596x1542.png "Figure 26 — pytorch walkthrough")](https://substackcdn.com/image/fetch/$s_!OZLU!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3b99b926-16c6-443c-bcb0-155ba1e63fc2_2596x1542.png)

Figure 26: *rope\_params* returns the cosine and sine tables for token position 2. *rope\_split\_half* takes those tables and the original query and returns the rotated query.

As shown in the figure above, first, `rope_params` determines how far to rotate each pair, returning two “tables” that store the cosine and sine of those angles. Or, in other words, `rope_params` computes the cosine and sine of each rotation angle, which depends on the token’s position and the feature pair’s frequency.

The `indices` array in the `rope_params` function contains the 2*i* values from the frequency formula in Section 4:

![Equation 15: \theta_i = b^{-2i/d}.](https://substackcdn.com/image/fetch/$s_!UFXx!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Febff1490-d57b-4eb3-a837-0a3426a18f0c_2240x84.png "Equation 15: \theta_i = b^{-2i/d}.")

For our eight-feature query, the `indices` are `[0, 2, 4, 6]`, that is, one 2*i* value for each feature pair. Suppose we use the common base *b* = 10,000. And following our earlier examples, we choose a small head dimension of *d* = 8. In this case, the `inv_freq` then computes the four pair frequencies *θi* = *b*−2*i*/*d* as follows:

![Equation 16: [\theta_0,\theta_1,\theta_2,\theta_3] = \left[10{,}000^{-0/8},\;10{,}000^{-2/8},\;10{,}000^{-4/8},\;10{,}000^{-6/8}\right] = [1,\;0.1,\;0.01,\;0.001].](https://substackcdn.com/image/fetch/$s_!2Mbm!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9a1fef03-0b62-4701-921b-2961ec0efd83_2240x121.png "Equation 16: [\theta_0,\theta_1,\theta_2,\theta_3] = \left[10{,}000^{-0/8},\;10{,}000^{-2/8},\;10{,}000^{-4/8},\;10{,}000^{-6/8}\right] = [1,\;0.1,\;0.01,\;0.001].")

As we can see above, the resulting `inv_freq` has one frequency *θi* for each of the four feature pairs ( measured in radians) per token position.

Next, the `angles` then multiplies each pair’s frequency *θi* with the token position *m*, which computes the rotation angle *mθi*. In our example, with `positions = [2]`, we get:

![Equation 17: [2\theta_0,\;2\theta_1,\;2\theta_2,\;2\theta_3] = [2\times1,\;2\times0.1,\;2\times0.01,\;2\times0.001] = [2,\;0.2,\;0.02,\;0.002].](https://substackcdn.com/image/fetch/$s_!G-9t!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F14cc892f-79ac-4dec-84cd-039f81e722c9_2240x79.png "Equation 17: [2\theta_0,\;2\theta_1,\;2\theta_2,\;2\theta_3] = [2\times1,\;2\times0.1,\;2\times0.01,\;2\times0.001] = [2,\;0.2,\;0.02,\;0.002].")

Finally, `rope_params` takes the cosine and sine of each angle and outputs them as two so-called cosine and sine tables, `cos` and `sin`. In both tables, each row is a token position and each column is a feature pair. Since we only use token position 2 here, each table has shape `(1, 4)`, with the single row shown below:

![Equation 18: \begin{aligned} \texttt{cos}:\quad [\cos(2),\;\cos(0.2),\;\cos(0.02),\;\cos(0.002)] &\approx [-0.416147,\;0.980067,\;0.999800,\;0.999998], \\ \texttt{sin}:\quad [\sin(2),\;\sin(0.2),\;\sin(0.02),\;\sin(0.002)] &\approx [0.909297,\;0.198669,\;0.019999,\;0.002000]. \end{aligned}](https://substackcdn.com/image/fetch/$s_!bLWh!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa6c52934-fa04-4d92-84d0-7bad016d1208_2240x148.png "Equation 18: \begin{aligned} \texttt{cos}:\quad [\cos(2),\;\cos(0.2),\;\cos(0.02),\;\cos(0.002)] &\approx [-0.416147,\;0.980067,\;0.999800,\;0.999998], \\ \texttt{sin}:\quad [\sin(2),\;\sin(0.2),\;\sin(0.02),\;\sin(0.002)] &\approx [0.909297,\;0.198669,\;0.019999,\;0.002000]. \end{aligned}")

For the example in Figure 26, each table has one row for token position 2 and four columns, one for each feature pair. Figure 27 shows the rotation angle for each pair and the corresponding entries in the two tables.

![Figure 27 — cos sin tables](https://substackcdn.com/image/fetch/$s_!1YrR!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F871d5113-e3f8-407b-9747-5051495fa9d3_2676x1033.png "Figure 27 — cos sin tables")

Figure 27: *The* two tables returned by *rope\_params* for token position 2. *The* highlighted column gives the cosine and sine used to rotate the first feature pair, *(1, 5)*.

In short, `rope_params` computes the cosine and sine of each rotation angle, which depends on the token’s position and the feature pair’s frequency.

In a real-world scenario, if we pass `rope_params` 1,024 token positions for a context size of 1024, and a head dimension of 128, it returns two tensors, `cos` and `sin`, each with shape `(1024, 64)` since a head with 128 features has 64 pairs.

The tables have no batch or head dimension, and we broadcast each `(1024, 64)` table across the batch and head dimensions. So, if we have multi-head attention or grouped query attention with 32 heads, feature pair 0 at token position 2 uses the same rotation angle in all 32 heads. Each head rotates its own feature values, so the rotated queries can still differ across heads. Also, the same entries can be reused across queries, keys, and heads that use the same positions and frequencies. In short, we only need to (pre)compute these tables once per model for all positions up to a maximum supported context size.

Technically, it would also be possible to store these tables alongside the model weights; but they are so incredibly cheap to compute that storing them on disk would be overkill.

Next, `rope_split_half` uses those cosines and sines to rotate the actual features in the query and key vectors inside attention.

Considering the simple 8-feature example once more, the `.chunk` in the code snippet above splits the query into `[1, 2, 3, 4]` and `[5, 6, 7, 8]` and lines up the two entries in each pair. For example, the first slot of each half gives us `(1, 5)`, the second `(2, 6)`, and so on. The two expressions inside `torch.cat` compute the rotated halves using the equations from Section 3.3:

![Equation 19: \begin{aligned} a_i' &= a_i\cos(m\theta_i) - b_i\sin(m\theta_i), \\ b_i' &= a_i\sin(m\theta_i) + b_i\cos(m\theta_i). \end{aligned}](https://substackcdn.com/image/fetch/$s_!cIqH!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fbcf9b781-1fac-40b7-83e7-54f6c36e3fec_2240x155.png "Equation 19: \begin{aligned} a_i' &= a_i\cos(m\theta_i) - b_i\sin(m\theta_i), \\ b_i' &= a_i\sin(m\theta_i) + b_i\cos(m\theta_i). \end{aligned}")

Note that here, everything denoted with *ai* corresponds to the first half of the query, and everything denoted with *bi* comes from the second half. The first expression inside `torch.cat` computes all the *ai′* values, and the second computes all the *bi′* values.

Then, we concatenate the halves, which puts each value back in its original index position via `torch.cat`.

Here’s the code that produces the rotated query in the earlier Figure 26. The shown reshape `(1, 1, 1, 8)` just brings the simple toy array into the expected format: one batch item, one head, one token, and eight features.

With several positions, each token simply picks up its own row from the `cos` and `sin` tables. Through broadcasting, PyTorch reuses these same tables across all batch items and attention heads.

So far, we coded the RoPE utility functions. Let’s now use it in the real context, the attention mechanism. Here, we use the standard frequency schedule for two 8-feature heads and four token positions. We use random query, key, and value tensors to keep the example self-contained, but as mentioned before, you can optionally also check the end-to-end [Qwen3 from-scratch implementation](https://github.com/rasbt/LLMs-from-scratch/blob/main/ch05/11_qwen3/standalone-qwen3.ipynb) with real query, key, and values produced by the pre-trained model.

Note that only `q` and `k` but not `v` pass through RoPE. The resulting raw attention scores are stored in a tensor of shape `(1, 2, 4, 4)`, that is, one row for each query position, and a column for each key position, and 4x4 since we have four tokens where each attends to each other.

Then, using a conventional attention code implementation, the causal mask replaces the scores for future keys with negative infinity, so they get zeroed after the softmax. The resulting `context` tensor has shape `(1, 2, 4, 8)`; the 4 because we have four tokens, and the 8 because the embedding dimension was originally 8.

We can also check that our claim about relative offsets holds, meaning that adding 3 to each position should change the position but preserve the query-key offsets:

I mentioned the *split-halves* convention many times throughout this article. The [original RoPE paper](https://arxiv.org/abs/2104.09864) uses the alternative *interleaved* convention, as does the [original LLaMA implementation](https://github.com/meta-llama/llama/blob/llama_v1/llama/model.py) (but as mentioned before, most code implementations now use the split-halves, because it makes it easier, in code, to rotate all pairs at once with element-wise sine and cosine operations.)

To illustrate the difference between the two layouts, Figure 28 below uses the same 8-feature vector in both rows.

![Figure 28 — feature pairing](https://substackcdn.com/image/fetch/$s_!pSrd!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F376d895d-ffc0-41ec-8cf5-5e84834412fc_2641x804.png "Figure 28 — feature pairing")

Figure 28: Split-half pairs each feature in the first half with the corresponding feature in the second half. Interleaved pairs adjacent features.

The interleaved implementation uses the same two equations, but selects alternating features and interleaves the outputs.

But mathematically, the layouts are equivalent when we account for the feature order. So, rearranging the entries `[1, 2, 3, 4, 5, 6, 7, 8]` to `[1, 5, 2, 6, 3, 7, 4, 8]` makes the split-half pairs adjacent with pairs (1, 5), (2, 6), and so on. If we apply the same rearrangement to both queries and keys, it results in identical dot products.

The outputs retain the reordered features, so we compare them with the split-half outputs in that same order. Applying the two layouts to the original vector however would give different scores.

RoPE has been a staple in most (open-weight) LLMs since the [original LLaMA model](https://scontent.fcps4-2.fna.fbcdn.net/v/t39.2365-6/333007794_1182140292435357_4481174526219500228_n.pdf?_nc_cat=101&_nc_map=urlgen_bucketless&ccb=1-7&_nc_sid=3c67a6&_nc_ohc=SrzHHMeB_5gQ7kNvwGdTCab&_nc_oc=AdonSC2oKCA4Qqs-KfR7_70m4s4_1QpPwBxDyuqu_KXyCswnUioaZNf1qyZckeLl6qPZge_29DAoPzzJbtkmdFP2&_nc_zt=14&_nc_ht=scontent.fcps4-2.fna&_nc_gid=aOwcXBC5dn_BAgAvPaHuBA&_nc_ss=7b289&oh=00_AQR_JQr4O14JgKxukyTSvBG_TEEoi37pQAo0flfY68Vquw&oe=6AFEA511). Of course, proprietary models may use it as well, but it’s always hard to say anything with 100% certainty about proprietary models.

However, in recent years, many architectures have been moving away from full-blown RoPE in every transformer block. In the following subsection, we will discuss some of these variants.

For instance the full RoPE implementation we discussed and implemented in the sections above rotates *every* feature pair in *every* query and key head.

In the following examples in the subsections above below, we will see that models only rotate some features within a head, other models skip RoPE in some transformer blocks, and some models use a different approach and add position information directly to their attention scores.

As discussed throughout the article, regular “full” RoPE rotates all dimensions in each query and key head. Partial RoPE is a variant that rotates only a subset, while leaving the remaining dimensions unchanged.

For example, [MiniMax-M3’s configuration](https://huggingface.co/MiniMaxAI/MiniMax-M3/blob/main/config.json) uses 64 rotary dimensions within each 128-dimensional head. That gives 32 rotated feature pairs and 64 unchanged dimensions. The figure below illustrates how that looks like.

[![Figure 29 — partial](https://substackcdn.com/image/fetch/$s_!HikP!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4515ea8c-8759-4aca-a824-64f0f42d080e_2616x1657.png "Figure 29 — partial")](https://substackcdn.com/image/fetch/$s_!HikP!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4515ea8c-8759-4aca-a824-64f0f42d080e_2616x1657.png)

Figure 29: Full RoPE and partial RoPE in a 128-dimensional head.

Note that if we look past the first transformer block, the unrotated features can still contain position information inherited from earlier transformer blocks.

Anyways, what’s the point behind partial RoPE? It saves some memory. For example, in the MiniMax-M3 example above, rotating half the features reduces the size of the precomputed cosine and sine tables also in half. So, with partial RoPE, instead of 64 columns, now each table only needs 32 columns. However, note that the KV cache still stores the full key and value vectors for each cached token.

For MiniMax-M3’s 1M-token context, rotating 64 of the 128 features halves the size of the precomputed cosine and sine tables, saving 0.134 GB (128 MiB) if the tables are stored in BF16. If the tables themselves were stored in NVFP4, the savings would be 0.038 GB (36 MiB), including the block scales.

Note that in experiments with 1B to 8B models, the *[Fractional Rotation, Full Potential?](https://arxiv.org/abs/2603.11611)* paper found that rotating around 10% of each head’s features often gave similar training loss and benchmark accuracy to full RoPE. However, full RoPE did better on some evaluations.

So, in my opinion, partial RoPE is hardly worth it.

NoPE was coined in the [The Impact of Positional Encoding on Length Generalization in Transformers](https://arxiv.org/abs/2305.19466) paper and means *no explicit positional encoding*. The idea or observation is that an attention layer without any explicit position method still has a causal mask, where a query can attend only to itself and earlier tokens. And this causal masking implicitly provides positional information.

Let’s use the first four tokens from our earlier example, `The red fox jumps`. In Figure 30, the query for `fox` at position 2 can attend to `The`, `red`, and itself. The query for `red` at position 1 can attend to `The` and itself.

[![Figure 30 — nope prefix mask](https://substackcdn.com/image/fetch/$s_!2NXi!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8ebf07fb-5de9-4446-abb3-4fdd15d1f6b1_2357x1614.png "Figure 30 — nope prefix mask")](https://substackcdn.com/image/fetch/$s_!2NXi!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8ebf07fb-5de9-4446-abb3-4fdd15d1f6b1_2357x1614.png)

Figure 30: Example of a causal mask in the attention mechanism.

So, the next question is how can the model use the causal mask as a position signal? Here’s a toy example adapted from the paper.

Suppose one feature of the value vectors is 1 for `The` and 0 for the other three tokens. Since the input only contains only one `The`, every visible prefix contains exactly one 1. If all non-masked keys get the same attention score, the softmax assigns every visible token equal weight.

So at `fox`, attention averages 1, 0, and 0, which gives 1/3. At `jumps`, it averages four values and gets 1/4. In other words, this output feature equals one over the prefix length, which a later layer can use as a signal for position.

[![Figure 31 — nope prefix average](https://substackcdn.com/image/fetch/$s_!QWKi!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fae9e1c64-4cd3-4184-a0d8-b17873dd710a_2640x1386.png "Figure 31 — nope prefix average")](https://substackcdn.com/image/fetch/$s_!QWKi!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fae9e1c64-4cd3-4184-a0d8-b17873dd710a_2640x1386.png)

Figure 31: With uniform attention, the contribution from *The* shrinks as the prefix grows. *The* query for *fox* assigns *The* a weight of 1/3.

Does NoPE work in practice? The researchers of the NoPE paper trained a tiny 107M-parameter decoder-only transformers from scratch on tasks such as copying, sorting, and addition to do some ablation.

When they tested the small model on examples longer than those seen during training, interestingly, NoPE ranked first overall across the ten tasks and ahead of RoPE. The researchers behind the [NoPE paper](https://arxiv.org/html/2305.19466v2) also conducted a separate experiment with a larger 1.3B-parameter models trained on code. Here, NoPE also handled contexts beyond the training length, however, its next-token predictions got worse at the longest lengths tested.

Some of today’s open-weight LLMs even go so far and use NoPE in all layers. However, nowadays, it’s more common for models to omit RoPE in at least some layers, where layer here means transformer block.

To give a concrete example, [SmolLM3](https://huggingface.co/blog/smollm3#architecture-and-training-details) (3B parameters) applies RoPE in three layers and skips it in the fourth, repeating that pattern through 36 layers. Interestingly, the SmolLM3 authors mentioned that this layer pattern improved long-context performance.

(Also note that any later layer, following the first one, even though it may skip the rotation, still receives the inputs from preceding transformer blocks, so its queries and keys still receive some sort of position information from earlier layers.)

[![Figure 32 — smollm3 layer pattern](https://substackcdn.com/image/fetch/$s_!iLab!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F26b1e107-4221-4ca8-a579-ac2603366c99_2804x1737.png "Figure 32 — smollm3 layer pattern")](https://substackcdn.com/image/fetch/$s_!iLab!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F26b1e107-4221-4ca8-a579-ac2603366c99_2804x1737.png)

Figure 32: SmolLM3’s architecture (left) and the positional method used in each of its 36 transformer blocks (right). Every fourth block skips RoPE. Architecture diagram from my [LLM Architecture Gallery](https://sebastianraschka.com/llm-architecture-gallery/).

Before we get to the overview and look at what modern architectures use, there’s one more method to introduce. For instance, Thinky’s [Inkling](https://thinkingmachines.ai/news/introducing-inkling/#architecture) uses a learned relative position bias in both its sliding-window and global attention layers. It computes the usual scaled dot product between a query and key, and then it adds a certain number to that score before the softmax in the attention score computation. That number depends on the query token’s hidden state and how many positions back the key is.

Let’s reuse our example sentence, “The red fox jumps over the gray fox.” (It’s the last time, I promise!) In the figure below, the “the” at position 5 is the query and “fox” at position 2 is the key. Inkling then uses the three-position gap between query and key and the hidden state for “the” to compute the bias, then adds it to the scaled dot product.

[![Figure 33 — relative position bias](https://substackcdn.com/image/fetch/$s_!U-18!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Feb6d1530-6222-49d1-a269-675b1e013253_2658x1232.png "Figure 33 — relative position bias")](https://substackcdn.com/image/fetch/$s_!U-18!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Feb6d1530-6222-49d1-a269-675b1e013253_2658x1232.png)

Figure 33: Inkling adds a learned relative position bias to the scaled query-key dot product before softmax. The numbers are illustrative.

This is not an entirely new idea. Some of you may recall Google’s popular [T5](https://arxiv.org/abs/1910.10683) (Text-to-Text Transfer Transformer) in 2019, which used a simpler version of this idea.

[![Figure 34 — inkling architecture](https://substackcdn.com/image/fetch/$s_!PMsu!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2d4cfb41-fe76-49ff-b58f-f88c2bec5f5e_2689x2772.png "Figure 34 — inkling architecture")](https://substackcdn.com/image/fetch/$s_!PMsu!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2d4cfb41-fe76-49ff-b58f-f88c2bec5f5e_2689x2772.png)

Figure 34: The original Inkling (975B) architecture. The learned relative position bias feeds into the attention module. [LLM Architecture Gallery](https://sebastianraschka.com/llm-architecture-gallery/)

Now, after all this build up, the interesting question is: which architecture uses which method? And what are the recent trends?

To answer this, I grouped the entries in my LLM Architecture Gallery by their listed release dates and compared the 2025 releases with the earlier and later releases in the 2026 snapshot. Each gallery card counts once, so multiple variants of an architecture can contribute to the results. [LLM Architecture Gallery](https://sebastianraschka.com/llm-architecture-gallery/)

[![Figure 35 — gallery position methods](https://substackcdn.com/image/fetch/$s_!eqMT!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb16bfe6e-e42b-4c9a-917e-2380d612d02a_2705x2244.png "Figure 35 — gallery position methods")](https://substackcdn.com/image/fetch/$s_!eqMT!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb16bfe6e-e42b-4c9a-917e-2380d612d02a_2705x2244.png)

Figure 35: The architecture diagrams illustrate which attention layers use RoPE, and the bars show the share of gallery entries in each category. Diagrams from my [LLM Architecture Gallery](https://sebastianraschka.com/llm-architecture-gallery/).

As we can see in Figure 35 above, using RoPE in *every* softmax attention layer is still the most common choice. However, its use fell from 2025 to 2026, and then also in the latter half of 2026, which could be a trend.

Consequently, there is a growing number of models that mix RoPE and NoPE. The groups are still small, so it’s hard to say whether these trends will continue.

The *NoPE only* entries were all recurrent hybrids with softmax attention layers that use no explicit positional encoding. The “Other” category here refers to xLSTM 7B (2025) and Inkling (2026). xLSTM has no softmax attention layers. Inkling adds a learned bias to the attention scores before softmax, as we saw in Section 6.3.

Among the models that use RoPE, partial RoPE became more common from 2025 to 2026. It went from a minority of the RoPE models in 2025 to a majority in both 2026 groups, as shown in Figure 36.

[![Figure 36 — gallery partial rope](https://substackcdn.com/image/fetch/$s_!p9wN!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F16975bd7-c1e0-443b-8241-63f91bc7e303_2734x938.png "Figure 36 — gallery partial rope")](https://substackcdn.com/image/fetch/$s_!p9wN!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F16975bd7-c1e0-443b-8241-63f91bc7e303_2734x938.png)

Figure 36: Among gallery entries that use RoPE, partial RoPE rotates a subset of query and key features in at least one attention layer. Full-width entries rotate all query and key features in every layer that uses RoPE.

If you’d like to look up an individual model from the 2026 groups, Figure 37 below shows the 2026 gallery overview with the individual architectures labeled by position type. You can also browse the gallery interactively [here](https://sebastianraschka.com/llm-architecture-gallery/).

[![Figure 37 — gallery 2026 collage](https://substackcdn.com/image/fetch/$s_!X1PL!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3dbac37d-1290-4ab9-92a8-6fe7f54d62e2_2850x2777.png "Figure 37 — gallery 2026 collage")](https://substackcdn.com/image/fetch/$s_!X1PL!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3dbac37d-1290-4ab9-92a8-6fe7f54d62e2_2850x2777.png)

Figure 37: The individual 2026 gallery entries behind the two 2026 bars in Figure 35.

So, to recap, we discussed GPT-2 and the original transformer that add a position vector to each token embedding before the first transformer block. Then, modern LLMs switched to RoPE, which encodes relative position inside attention by rotating queries and keys. Surprisingly, RoPE remains as the most popular positional embedding method today.

Recently, however, Inkling picked up an idea from T5 to add a learned relative position bias to the attention scores before softmax. And then we also have NoPE-RoPE architectures, which omit RoPE in some layers.

It may be boring news, but as mentioned before, RoPE is still the predominant position embedding method, and I think this will carry over into 2027 for sure. Perhaps, though, we’ll see more architectures with an Inkling-like design coming up in 2027.

While this may sound “boring”, I am glad that I finally got this article on RoPE (and other position info methods) out, after so many years, so that I can add it to my collection of LLM “fundamentals” explainers.

If you are interested in the other architecture building blocks of LLMs, I still highly recommend starting with a GPT-2-style model, which will give you a solid understanding of the building blocks with the least amount of friction. And from there, you can then layer on additional methods (such as RoPE).

If you’d like to build that GPT-2-style model yourself, my first [Build a Large Language Model (From Scratch)](https://sebastianraschka.com/books/#build-a-large-language-model-from-scratch) book walks through all the building blocks depicted in the figure below.

For the methods that branch off from the GPT-2 baseline, I’ve collected some of my relevant blog articles below, starting with a refresher on self-attention:

- **Self-attention, causal masking, and multi-head attention:** [Understanding and Coding Self-Attention, Multi-Head Attention, Causal-Attention, and Cross-Attention in LLMs](https://magazine.sebastianraschka.com/p/understanding-and-coding-self-attention)

- **Modern attention variants (GQA, MLA, sliding-window attention, sparse attention, and hybrids):** [A Visual Guide to Attention Variants in Modern LLMs](https://magazine.sebastianraschka.com/p/visual-attention-variants)

- **The KV cache.** [Understanding and Coding the KV Cache in LLMs from Scratch](https://magazine.sebastianraschka.com/p/coding-the-kv-cache-in-llms)

- **SwiGLU feed-forward layers and normalization (LayerNorm and RMSNorm):** [From GPT-2 to gpt-oss: Analyzing the Architectural Advances](https://magazine.sebastianraschka.com/p/from-gpt-2-to-gpt-oss-analyzing-the)

- **Mixture-of-experts layers and expert routing.** [The Big LLM Architecture Comparison](https://magazine.sebastianraschka.com/p/the-big-llm-architecture-comparison)

- **Residual connections and DeepSeek’s manifold-constrained hyper-connections (mHC):** [Recent Developments in LLM Architectures: KV Sharing, mHC, and Compressed Attention](https://magazine.sebastianraschka.com/p/recent-developments-in-llm-architectures)

- **Putting some of the pieces together in Qwen3:** [Understanding and Implementing Qwen3 From Scratch](https://magazine.sebastianraschka.com/p/qwen3-from-scratch) walks through implementing the dense and mixture-of-experts versions in PyTorch.

**Thanks for reading and supporting my work!**

PS: I started experimenting with Substack’s new “perks” system. As a thank-you to supporters, I’ve added [image exports and metadata](https://magazine.sebastianraschka.com/perks) from the architecture gallery for you to use in your own analyses.
