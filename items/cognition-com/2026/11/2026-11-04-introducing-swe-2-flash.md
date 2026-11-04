---
title: Introducing SWE-2 Flash
link: https://cognition.com/blog/swe-2-flash
source: cognition-com
published: 2026-11-04T18:00:00Z
first_seen: 2026-11-04T21:49:17.908350494Z
content: extracted
html: 2026-11-04-introducing-swe-2-flash.html
remote_preview:
  url: https://cognition.com/images/swe-2-flash/cover.jpg
  alt: Introducing SWE-2 Flash
---

We’re introducing SWE-2 Flash, a fully American coding model that sets a new bar for intelligence at low cost and high speed. For the first time, we added a mid-training stage before reinforcement learning in our recipe. This results in significant gains over the base model, [Inkling-Small](https://thinkingmachines.ai/news/inkling-small/) from Thinking Machines, improving Terminal-Bench 4.0 from 0.9% to 20.0% and FrontierCode 2.0 from 3.2% to 24.0%.

On leading evals, SWE-2 Flash outperforms all American open-weight models, even models that are 3x larger. To balance transparent access and cyber risks, we are building a program to securely share the weights with industry partners and researchers.

As model capabilities continue to advance, we remain focused on lowering the cost of everyday engineering work in addition to pushing the capabilities frontier. Today, smaller models are increasingly able to handle a significant portion of the work that software agents do, such as automations, code review, feature development, and updating documentation.

We developed SWE-2 Flash to satisfy this growing need. It is available today in Devin [Desktop](https://devin.ai/desktop) and [CLI](https://devin.ai/cli), at one-fifth the price and 3x the speed of its much larger sibling, SWE-2.

#### Generation speed

Each model’s own run on Terminal-Bench 4.0’s gsea-proteomics task, replayed at a simulated 200 vs. 60 tokens per second. Transcripts are shortened to equal length for comparison.

## [Training](https://cognition.com/blog/swe-2-flash#training)

We began with a large-scale mid-training stage on top of [Inkling-Small](https://thinkingmachines.ai/news/inkling-small/), a model with 276B total and 12B active parameters. A key challenge was determining the right data mix: which sources to include, and in what proportions. To guide these decisions, we curated a high-quality held-out coding dataset that reflects model intelligence in real-world software engineering scenarios.

We then used token efficiency plots to evaluate different data sources and identify the most effective mixtures. Starting with a baseline training run, we measured how many tokens each candidate data mix needed to reach the same validation loss as the baseline. By plotting the ratio of these token counts on the y-axis and baseline tokens on the x-axis, we could directly compare the training efficiency of different mixtures.

We iterated on the composition of our mid-training data and converged on a dataset composed of a few trillion tokens of high-quality scientific articles, blogs, code, and other sources. Training proceeded in three stages, so that the model would see the scarcest, highest-quality data later in training. Throughout each stage, held-out validation loss improved at a stable log-linear rate.

#### Validation loss across mid-training

We followed the mid-training phase with large-scale reinforcement learning. While we made some tweaks to our RL recipe from SWE-2, most of the improvements came from the much higher quality and diversity of our RL tasks.

On quality, we carefully audited the instructions, environment setup, and verifiers for our tasks. On diversity, we curated several new task types covering a far wider range of capabilities than before, to build the general reasoning and problem-solving skills that real work in Devin demands. Training on this data led to significant improvements on nearly every benchmark we measured.

#### Rollout score vs. benchmark average during RL

## [Origins & Cyber](https://cognition.com/blog/swe-2-flash#origins-cyber)

For sensitive, mission-critical work, customers need more than a capable model behind an API. As cybersecurity grows into a major use case, the stakes get even higher. Enterprises and governments need observability to host and control deployments on their own infrastructure, including on-prem. Researchers in academia and industry need to study how models behave and how they can be misused.

One of our goals with SWE-2 Flash is to provide an option to industry partners and researchers that allows transparent access to weights along with the ability to fine-tune and run it in sensitive environments. To help defenders, we targeted cybersecurity work in our training, raising the model’s ExploitBench score from 11.3% to 34.8%. These same capabilities can also be used by malicious attackers, as demonstrated by a [recent surge in cyber attacks](https://devin.ai/blog/devin-security-2), so we are not sharing the weights publicly yet. As a potential solution that balances both transparency and safety, we are building a program to securely share the weights with trusted industry partners and research labs. More on this in a few weeks.

[Free through 11/30 Try it today in Devin Desktop and CLI Get SWE-2 Flash](https://devin.ai/desktop)
