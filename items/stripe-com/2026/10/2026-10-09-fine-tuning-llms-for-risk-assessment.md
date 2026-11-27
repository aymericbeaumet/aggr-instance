---
title: Fine-tuning LLMs for risk assessment
link: https://stripe.dev/blog/training-llms-for-risk-assessment
source: stripe-com
published: 2026-10-09T00:00:00Z
updated: 2026-10-09T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Hangzhi Guo
summary: SFT helps Stripes make more precise risk assessments. We explain the training approach and investigate what the model learns about legitimate businesses.
content: extracted
html: 2026-10-09-fine-tuning-llms-for-risk-assessment.html
remote_preview:
  url: https://stripe.dev/og/blog/training-llms-for-risk-assessment.png
  alt: 'Stripe Developer Blog: Fine-tuning LLMs for risk assessment'
---

> This is the first of two posts on fine-tuning LLMs for risk assessment. Here, we focus on learning from labels; the second post will explore learning from explanations.

## High-stakes risk assessment

Stripe powers millions of businesses moving trillions of dollars annually while operating self-serve, globally adopted, financial infrastructure. To achieve this safely, we make thousands of risk decisions every single second. Is this transaction legitimate? Is the business trustworthy? Is this product set allowed by this financial partner? Has this business become suddenly popular, or is it under attack? Should we adjust a business’ credit reserves?

A false negative lets through fraud and abuse, reducing trust and making the internet economy less safe . A false positive can stymie legitimate economic activity and meaningfully affect a business’ livelihood. Balancing these risks, in near real-time at high precision, requires models to connect patterns in our data with an understanding of how the business operates in the wider economy.

Several properties of LLMs make them well suited to these risk decisions. LLMs bring world knowledge from pretraining that helps interpret Stripe’s internal data in the context of real-world business practices. LLMs also provide a flexible way to integrate heterogeneous features, including numerical, categorical, text, and time-series data. Combining these inputs in conventional tabular or sequence models often requires feature engineering, type-specific encodings, or specialized architectures. With LLMs, we can serialize these features into a shared text representation and process them with a single model, enabling it to reason jointly over different feature types.

![Serializing business features into LLM input](https://stripe.dev/images/training-llms-for-risk-assessment/image2.gif)\
**Figure 1.** Serializing business features into LLM input. A feature serializer converts heterogeneous features into a text-based business record that an LLM can reason over to assess risk.

The problem is that LLMs are word prediction machines, and are not designed for the reliably rigorous statistical analysis Stripe needs as inputs to our risk decisions.

We confirmed this intuition in early testing by asking a model to assess the likelihood that a business was fraudulent. We compared zero-shot prompting, which relied solely on the model’s pretraining, with domain-guided prompting where we made Stripe’s review criteria explicit, including which signals to consider and how to interpret them in context.

For each business, we collected the features used in human reviews, including card payments, money movement, and account activity, and serialized them into a text-based account record (see Figure 1). The model produced a verbalized probability of fraud, which we used as the risk score ([Tian et al. 2023](https://aclanthology.org/2023.emnlp-main.330/)). This continuous score allowed us to rank accounts and set thresholds for routing them to human reviewers, balancing fraud detection against disruption to legitimate businesses.

The LLMs performed poorly against human review. While domain-guided prompting outperforms zero-shot, it fails to reach the human reviewer’s precision at the same recall. Both model predictions and initial human-review decisions are evaluated against labels that incorporate subsequent evidence and outcomes. It’s worth noting that for these results, and the others shared below, that fraud prevalence and the mix of cases affect precision, so results on accounts selected for review should not be read as performance across all Stripe businesses.

| **Method**              | **Precision @ Human-Review Recall** |
| ----------------------- | ----------------------------------- |
| Zero-shot               | 0.487                               |
| Domain-guided prompting | 0.706                               |
| Human review            | 0.790                               |

So how can we get a word predictor to natively conduct and output high quality statistical analysis?

## Supervised fine-tuning (SFT)

To fit an LLM into this analytical structure, we explored labels-only supervised fine-tuning (SFT) of an open-weight 4B-parameter LLM. Unlike prompting, SFT updates the model’s weights to predict each account’s review label from its serialized record. Let $x_i$ be the state of the business’s account $i$, $s(x_i)$ its text serialization, and $y_i \in \{0,1\}$ the review label, where $y_i = 1$ denotes fraud. The training dataset is $\mathcal{D} = \{(s(x_i), y_i)\}_i$. The target response contains only the label $y_i$, so the objective reduces to the negative log-likelihood of that label:

$$
\theta^* = \underset{\theta}{\arg\min}\; \mathbb{E}_{(s(x_i), y_i) \sim \mathcal{D}} \left[-\log p_{\theta}\!\left(y_i \mid s(x_i)\right)\right]
$$

Although the model is trained to output a discrete label, we can derive a continuous risk score from its next-token distribution. For single-token class labels, we normalize the probabilities of the fraud and non-fraud tokens at the answer position. If $\ell_1$ and $\ell_0$ are the log probabilities of the fraud and non-fraud label tokens, the fraud score is

$$
r(x) = \frac{e^{\ell_1}}{e^{\ell_1} + e^{\ell_0}}
$$

This score lets us rank accounts and choose a decision threshold. Unlike the prompted baseline, which generates a numerical risk estimate as text, this approach derives the score directly from the label-token probabilities.

For example, suppose fraud is encoded as yes and non-fraud as no, each represented by a single token. If the model assigns probability 0.18 to `yes`, 0.72 to `no`, and 0.10 to all other tokens, normalizing over the two valid labels yields a fraud score of 0.18 / (0.18 + 0.72) = 0.20 (see Figure 2 for illustration). Rather than retaining only the predicted label, we use the relative probability assigned to each class to obtain a continuous score.

![Computing a fraud score from the next-token distribution](https://stripe.dev/images/training-llms-for-risk-assessment/image4.gif)\
**Figure 2.** Illustration of computing a fraud score from the next-token distribution.

And how does this adapted model perform relative to our human reviewers?

![Precision-recall curves on a selected fraud-review cohort](https://stripe.dev/images/training-llms-for-risk-assessment/image1.png)\
**Figure 3.** Precision-recall curves on a selected fraud-review cohort.

The fine-tuned 4B-parameter model achieved higher precision against historical review labels, achieving approximately 24% higher average precision than domain-guided prompting and 70% higher than zero-shot prompting. These results suggest that Stripe’s risk distribution differs from the broader pretraining distribution. Injecting domain knowledge through context helps bridge that gap, but fine-tuning on our internal data captures nuances that cannot be exhaustively conveyed in written guidelines.

### Does SFT learn new knowledge?

The gains in decision accuracy alone do not reveal whether SFT changes how the model represents risk signals, such as suspicious payment patterns or legitimate business activity. To investigate this, we compared the internal representations of this open-weight 4B with and without training on risk review data. We used linear probing ([Alain and Bengio, 2016](https://arxiv.org/abs/1610.01644)) to measure how risk-relevant concepts could be linearly decoded from each model’s hidden representations. For each model, we extracted the final layer’s hidden state at the answer-prefill position, then trained logistic regression classifiers on these frozen representations.

We probed for 44 concepts using 34 binary and 10 multiclass classification targets, including risk and benign signals, account metadata, business size, case types, and evidence sufficiency. Probe labels came either from the source dataset or from the teacher-model annotations. The benign concepts capture characteristics associated with legitimate businesses, such as a consistent business description, a credible online presence, or an established processing history. Their absence isn't a risk signal; it only means that evidence isn't there. Risk signals are the fraud patterns reviewers flag during business review.

![Linear-probe ROC-AUC before and after SFT](https://stripe.dev/images/training-llms-for-risk-assessment/image3.png)\
**Figure 4.** Linear-probe ROC-AUC before and after SFT for benign signals, risk signals, and attribute groups.

#### SFT makes benign signals more accessible

Figure 4 shows that linear-probe ROC-AUC improves more for benign signals (0.583 -> 0.642) than for risk signals (0.702 -> 0.729). Before SFT, benign signals are close to chance (0.5) in the base model's representations, so the model carries little information about what a legitimate business looks like. After SFT, eight of the nine benign signals become easier to recover, including a consistent business description (0.614 -> 0.707) and an established processing history (0.644 -> 0.712).

These results suggest that SFT helps the model recognize when suspicious-looking activity makes sense for a legitimate business. A model that better recognizes legitimate businesses is less likely to flag them, which aligns with the higher precision at the same recall in Figure 3 and reduces false positives.

### SFT improves risk assessments across tasks

Across multiple risk assessment tasks at Stripe, supervised fine-tuning increased precision by more than 21% at the same recall and consistently outperformed XGBoost. These results make SFT a strong foundation for production risk systems that require both precise risk evaluations and the flexibility to use rich, heterogeneous evidence.
