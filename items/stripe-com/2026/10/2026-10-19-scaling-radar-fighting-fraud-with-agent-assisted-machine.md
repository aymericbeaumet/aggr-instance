---
title: 'Scaling Radar: Fighting fraud with agent-assisted machine learning'
link: https://stripe.dev/blog/scaling-radar-fighting-fraud-with-agent-assisted-machine-learning
source: stripe-com
published: 2026-10-19T00:00:00Z
updated: 2026-10-19T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Ryan Drapeau
summary: How Stripe scaled fraud detection and improved it with rapid agent-assisted experimentation for the Radar product.
content: extracted
html: 2026-10-19-scaling-radar-fighting-fraud-with-agent-assisted-machine.html
remote_preview:
  url: https://stripe.dev/og/blog/scaling-radar-fighting-fraud-with-agent-assisted-machine-learning.png
  alt: 'Stripe Developer Blog: Scaling Radar: Fighting fraud with agent-assisted machine learning'
---

Stripe Radar, our fraud prevention solution, helps businesses block fraud while letting legitimate customers pay. Its models draw on connections across the Stripe network. For example, a credit card that is new and untrusted to one business might have years of history elsewhere on Stripe that proves it’s OK or an IP address may link activity across several cards and accounts. Shield is one of those models and focuses on transaction fraud. It learns representations of these core signals, combines them to predict fraud, and scores payments in milliseconds. See [our previous post](https://stripe.dev/blog/how-we-built-it-stripe-radar) if you’d like to learn more about how we initially built it.

We wanted Shield to catch more fraud patterns specific to particular businesses, products, and countries without flagging legitimate payments that happened to look unusual. But even with roughly 800 million transactions in its training set, Shield learned from only a fraction of the available history: we discarded most payments with no reported fraud to keep training efficient. We hypothesized that fuller histories would help Shield distinguish fraud from legitimate activity that was common for a particular card or business, even if it was unusual across the network.

Training on more of the network would stretch both our infrastructure and how we developed models. A single training pass through the data could take a day. The recent payments we needed to learn from often had incomplete fraud reports, making it hard to tell whether the model was improving. Investigating those questions meant running more experiments, each requiring ML engineers to prepare data, monitor training, and debug failures.

Learning from more of the Stripe network required faster training and a better understanding of delayed fraud reports. We also built tools for AI agents to investigate missed fraud and run experiments, using model explanations and results from earlier investigations to test changes to Shield. Read on to learn how we scaled our training approaches and automated more of the experiment lifecycle with agents, improving our model’s recall by over 8%.

## Scaling Radar

For each week of payment data, our training pipeline kept every known fraudulent payment and randomly selected a subset of the remaining payments. This downsampling lets regular retraining finish more quickly. Training on a larger fraction of the network would give the model more examples of less common activity and more data for learning the entity representations of cards, businesses, and other entities.

For the first experiment, we removed downsampling and included all eligible payments from a nine-month window. That expanded the training set from roughly 800 million to 11 billion transactions. The results were encouraging: after one pass through the data, the model was already close to the existing model's performance. By the third pass, it had surpassed it on our initial evaluation.

Each pass, or epoch, took about 24 hours. We wanted to run multiple experiments a week and work toward testing new ideas daily. At that rate, even a three-epoch comparison required roughly three days of training. Testing another feature or training configuration meant waiting days for another result, which made it too slow to explore ideas.

### Keeping the GPUs busy

We optimized, shortening each epoch from 24 hours to 4.8 hours (an 80% reduction) while keeping the dataset fixed at 11 billion examples. Keeping the GPUs busy required tuning data loading, computation, and communication together; speeding up one made the limits of the others easier to see.

![Chart showing training time per epoch dropping from 24 hours to 4.8 hours across four optimization stages: initial implementation, streaming from S3, compiling and parallelizing, and optimizing data movement.](https://stripe.dev/images/scaling-radar-fighting-fraud-with-agent-assisted-machine-learning/image1.png)

Our training pipeline downloaded data from S3 to disk for the data loaders to read. At this scale, the dataset couldn’t be cached in memory, so disk throughput limited how quickly we could supply the GPUs. We changed the data loaders to stream from S3 into memory, which cut time per epoch in half, to 12 hours.

We compiled the tensor operations in Shield's training step with PyTorch's `torch.compile` and tuned the compiler configuration. We also used lower-precision arithmetic to speed up computation. Shield's [ResNeXt](https://arxiv.org/abs/1611.05431) architecture contains parallel branches; executing them concurrently, along with improvements to CPU-to-GPU transfers, brought the time down to seven hours.

Memory use constrained how much data we could prepare before the GPUs. Cutting peak training memory from 1.3 TB to 500 GB let us increase the number of data-loading workers per GPU and prefetch more data. Compressing gradients to BF16 during communication between GPUs, tuning the communication bucket size, and using asynchronous CPU-to-GPU transfers brought the final epoch time to 4.8 hours.

The larger dataset also produced a wider embedding layer. The model grew to roughly 30 million parameters, three times its previous size, without increasing serving latency in our load tests or production scoring. We could now train at the scale we wanted and still make a decision quickly enough to meet our latency constraints.

Unfortunately, the larger model initially looked better, but that result changed as more fraud reports arrived.

### Evolving labels

The new model looked better in offline tests and in early results of a production experiment. But, as more fraud reports arrived, its measured performance fell below the earlier model's, and we had to roll back to a model trained with the earlier sampling strategy.

For a payment made with a stolen card, it could take weeks for the cardholder to notice and report it to their bank. When Stripe receives the fraudulent dispute, we gain a fraud label for a payment we scored weeks earlier. We assigned labels in our training datasets based on the fraud reports available at the time. The most recent weeks of training data contained the most immature labels as not enough time had passed for reports to arrive.

We had to balance how quickly Shield learned new fraud patterns against how much we could trust the labels. Recent payments helped the model keep up with attackers, but including too many meant training on fraud incorrectly labeled as legitimate. Waiting for reports gave us more reliable labels at the cost of learning new behavior later.

![Line chart showing the cumulative share of fraudulent disputes reported over time, reaching 41% by day 14, 66% by day 30, and 86% by day 60 after the payment.](https://stripe.dev/images/scaling-radar-fighting-fraud-with-agent-assisted-machine-learning/image2.png)\
Fraud reports arrive gradually as seen in this cumulative distribution. Payments that appear legitimate in a recent training or evaluation dataset can acquire fraud labels weeks later.

The pool of payments we downsampled contained both legitimate payments and fraud that had not been reported yet. Keeping only a fraction of that pool also kept most of the mislabeled fraud out of training. When we tried removing downsampling, we added more legitimate examples, as intended, but also more fraudulent examples labeled as legitimate, about 20 times more. The number of known fraud examples stayed the same, because we already kept all of them.

The model consequently saw many more examples teaching it that late-reported fraud patterns were legitimate, particularly in the most recent weeks of training. It became worse at detecting fraud that would only be reported weeks later. Understanding failures like this required repeated experiments with Shield's data and training. Faster training made those experiments more practical, but each still took substantial engineering work to get from a hypothesis to a result.

## Giving agents the experiment lifecycle

Each experiment followed a familiar machine learning engineering cycle: investigate a failure, form a hypothesis, implement a change, and evaluate it. Preparing training data, monitoring jobs, and debugging failures took time in every experiment.

We built Agentic MLE, short for machine learning engineer, to take on more of that work. It’s a set of tools and instructions that lets AI agents operate in Stripe's existing ML infrastructure, carrying an experiment from an idea to a result: explore the data, implement a feature, compute its historical values, train a model, and evaluate it against a baseline.

![Diagram of the ML experiment lifecycle as a loop: explore data, prototype features, compute training data, train and evaluate model with features, and deploy feature with updated model.](https://stripe.dev/images/scaling-radar-fighting-fraud-with-agent-assisted-machine-learning/image4.png)

Agents work across the experiment lifecycle. Engineers review hypotheses and results and decide what to deploy.

Feature experiments are built on [Shepherd](https://stripe.dev/blog/shepherd-how-stripe-adapted-chronon-to-scale-ml-feature-development), our feature platform, which computes historical training features and serves their online equivalents from the same definitions.

As with our work on [minions](https://stripe.dev/blog/minions-stripes-one-shot-end-to-end-coding-agents-part-2), we needed to make our operational knowledge available to the agents. They need to have the right datasets, entity keys, time stamps, and sampling rules for feature experiments, be able to reproduce knowledge at the time of payments for historical feature backfills, and diagnose failed distributed training jobs before rerunning them. We organized this knowledge into reusable procedures for the platform and model-specific documentation that an agent loads for the task at hand.

Agentic MLE uses tools to query data, inspect workflow executions, and read experiment results. It writes the code for a candidate feature and runs the jobs needed to test it. Engineers review the proposed experiment and its results at defined points. Within a run, it inspects failures, correct configurations, and retries failures as a human would have done.

### Learning from a fraud attack

We put those tools to work on a fraud attack in which Shield was assigning low risk scores to fraudulent payments. We gave the agents attack data and results from earlier experiments and investigations into Shield's behavior, including our scaling work. An agent used model explanations to identify which features pushed scores higher or lower on the missed fraud, giving us testable hypotheses. We could remove features that made the traffic look safe, or make combinations of suspicious activity more explicit. Humans helped prioritize 38 ideas, then agents implemented the changes, trained models, debugged failed runs, and evaluated the results.

Several agents proposed experiments with stochastic weight averaging, or SWA, which averages parameter snapshots from a training run into a single model. Shield already used SWA, but began averaging over halfway through the run. Training loss was falling while validation loss rose, giving agents a reason to test whether including earlier snapshots could reduce overfitting.

Stopping training early improved overall ranking but reduced recall in our narrow false-positive range. The best results combined averaging at epoch 0 and shortening the training run to eight epochs.

![Diagram comparing two stochastic weight averaging configurations: the original configuration averages epochs 8-13 over a 14-epoch run, while the winning configuration averages epochs 0-7 over a shortened 8-epoch run.](https://stripe.dev/images/scaling-radar-fighting-fraud-with-agent-assisted-machine-learning/image3.png)

In a controlled comparison, the new configuration improved recall by over 8%, translating to an estimated tens of millions of dollars a year in additional fraud prevention for Stripe’s merchants.

## Where we go next

We learn from each experiment, especially ones that do not lead to production, and use those learnings to improve our models and systems. The infrastructure improvements that sped up our training enabled us to rapidly test a wide range of hypotheses through Agentic MLE. Our goal is for agents to carry out longer investigations between reviews, while engineers set priorities, evaluate the evidence, and decide what to release.

We're hiring engineers to help build these ML systems and infrastructure. If you'd like to work on these problems, [explore our engineering roles](https://grnh.se/b5wzjpge1us?query=engineer).
