---
title: Learning from explanations
link: https://stripe.dev/blog/learning-from-explanations
source: stripe-com
published: 2026-11-19T00:00:00Z
updated: 2026-11-19T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Hangzhi Guo
summary: Training on explanations helps Stripe make more precise risk assessments. We explain our two-stage SFT approach and how on-policy distillation restores the model’s ability to explain its decisions.
content: extracted
html: 2026-11-19-learning-from-explanations.html
remote_preview:
  url: https://stripe.dev/og/blog/learning-from-explanations.png
  alt: 'Stripe Developer Blog: Learning from explanations'
---

## Why explanations matter

In an [earlier post](https://stripe.dev/blog/training-llms-for-risk-assessment), we showed that supervised fine-tuning (SFT) on fraud labels improves risk assessment. But the review labels tell the model only whether an account is fraudulent, leaving the model to infer which evidence supports that assessment. Explanations make the reasoning explicit: which signals matter, why they matter, and how to weigh them when they conflict. Could teaching a model not just the answer, but the reasoning behind it, make its decisions better still?

The answer is yes, but with an important caveat. When we simply added explanations to the training target, effectively training the models on labels and explanations together, the models produced worse decisions than they did when trained on labels alone. That’s because standard SFT loss weights each target token equally, and the gradient is therefore dominated by the much longer explanation rather than the decision we ultimately care about.

That’s why we separate these objectives with two-stage SFT: first train on labels and explanations, then on labels alone. Our hypothesis was that the first stage would teach useful connections between evidence and decisions, while the second focuses training on predicting the correct label. This approach improved decision performance across multiple risk assessment tasks. In fraud review, it achieved 39% higher recall than label-only SFT at human-review precision.

The second stage introduced another tradeoff: better decisions, but poorer explanations. Because the second stage is trained to stop after the label, the model often skips the explanation entirely, and the explanations it does write are lower quality. To recover these capabilities, we use on-policy distillation (OPD), with the explanation-trained checkpoint guiding the model on its own generated responses. In our evaluation, OPD restored explanation quality while retaining the decision gains.

![Learn from labels and explanations, optimize decisions with label-only training, then recover explanation quality through on-policy distillation](https://stripe.dev/images/learning-from-explanations/image10.png)\
**Figure 1.** Learn from labels and explanations, optimize decisions with label-only training, then recover explanation quality through on-policy distillation.

## Synthesizing explanations with a teacher model

To train on explanations, we first need them for every training sample. Human reviewers at Stripe explain their assessments in review notes. These notes can be valuable training examples: they connect evidence to a decision. However, their quality varies, and notes are not available for every account. To get a high-quality explanation for every account, we use a teacher model to synthesize explanations from serialized account records and review guidelines.

We use rejection sampling with privileged information to construct the explanation dataset. Given a serialized account record and the domain-guided prompt from the first post, the teacher generates both an estimated probability of fraud and a structured explanation. The explanation follows the guidelines the way a reviewer would. It identifies relevant evidence and connects it to a risk assessment. The teacher does not see the ground-truth label when generating its assessment. Instead, we use that label for rejection sampling to retain only examples where the teacher’s prediction agrees with it.

Figure 2 shows an example for the mock account from the first post. The teacher sees the serialized account record and the review guidelines, but not the label. Its assessment covers each dimension of the guidelines.

![The mock Good Bufo LLC merchant record from the first post goes into the teacher model, which is prompted with the review guidelines. A label check compares the teacher's fraud probability with the ground-truth label, which is added only after the teacher runs. The resulting training target puts the label first, followed by an assessment for each review dimension.](https://stripe.dev/images/learning-from-explanations/image13.png)\
**Figure 2.** Synthesizing a training target for a mock account. The teacher sees the account record and the review guidelines, but not the label. We keep the example because the teacher’s fraud probability agrees with the ground-truth label. The target puts that label first, followed by the teacher’s assessment of each review dimension.

## Two-stage SFT

With an explanation for every account, we train the model in two stages. Stage 1 learns from labels and explanations together, and Stage 2 trains on labels alone.

### Stage 1: Knowledge distillation

Knowledge distillation transfers the teacher’s domain-guided explanations into the student through SFT. Carrying forward the notation from the first post, let $u_i = [y_i; e_i]$ be the Stage 1 target containing the label $y_i$ followed by its explanation $e_i$, and let $\mathcal{D}_E = \{(s(x_i), u_i)\}_i$ be the explanation-supervision dataset. Stage 1 minimizes the expected loss between this target and the model’s prediction:

$$
\theta_1^* = \underset{\theta}{\arg\min}\; \mathbb{E}_{(s(x_i), u_i) \sim \mathcal{D}_E} \left[\mathcal{L}\left(u_i, p_{\theta}\!\left(\cdot \mid s(x_i)\right)\right)\right]
$$

Here $\theta_1^*$ is the Stage 1 model and $\mathcal{L}$ is the autoregressive cross-entropy loss over the label-plus-explanation target.

Unlike standard chain-of-thought setups, where reasoning precedes the answer, we place the label before the explanation so that the label-token probabilities used to compute the fraud score depend only on the account record. If the explanation comes first, the label distribution is also conditioned on the generated explanation. Once that explanation commits to a verdict, the fraud score can collapse toward 0 or 1, reflecting agreement with the verdict rather than uncertainty about the account. In addition, generating a long explanation before the label can degrade calibration as errors accumulate ([Cao et al., 2025](https://arxiv.org/abs/2511.11966)).

### Stage 2: Decision calibration

Stage 2 calibrates the model’s decisions through label-only training. It starts from $\theta_1^*$ and trains on a label-only dataset $\mathcal{D}_L = \{(s(x_i), y_i)\}_i$. As in the first post, the objective is the negative log-likelihood of the label:

$$
\theta_2^* = \underset{\theta}{\arg\min}\; \mathbb{E}_{(s(x_i), y_i) \sim \mathcal{D}_L} \left[-\log p_{\theta}\!\left(y_i \mid s(x_i)\right)\right]
$$

### Why two stages?

Because standard SFT spreads the gradient across thousands of explanation tokens, joint training dilutes the decision signal (Figure 3a): the loss gives explanation generation far more weight than the decision we ultimately care about. Two-stage SFT lets us use richer supervision and then focus training on decision accuracy. Knowledge distillation expands each training target from a single label into thousands of tokens connecting how evidence in an account record supports that decision. We hypothesize that the knowledge distillation stage builds useful internal representations from this dense supervision, which Stage 2 then refines through label-only training so we optimize directly for decision accuracy without explanation tokens dominating the objective. Ultimately, two-stage training trades extra training compute to extract richer signals from existing data.

Figure 3b compares label-only SFT, joint label-and-explanation SFT (Stage 1 only), and two-stage SFT on the same cohort. At human-review precision, two-stage SFT achieves 39% higher recall than label-only SFT, while joint training falls below label-only SFT. Explanations improve decision performance when label-only training follows them, but not when the model learns labels and explanations together.

![Left: one decision token next to a grid of 1,000 explanation tokens, shaded as evidence and reasoning or as wording and formatting, with a bar showing each token type's share of the loss. The decision token gets 0.1%. Right: precision-recall curves for two-stage SFT, label-only SFT, and joint label-and-explanation SFT](https://stripe.dev/images/learning-from-explanations/image11.png)\
**Figure 3.** (a) Target tokens for one account record with a 1,000-token explanation. Tokens differ in what they say about the decision: some cite evidence or reasoning, while many only carry wording and formatting. The loss weighs them all equally, so the decision token gets 0.1% of the training signal. The split between token types is illustrative. (b) Precision-recall curves for two-stage SFT, label-only SFT, and joint label-and-explanation SFT on the same cohort. The arrow marks the recall gain of two-stage SFT over label-only SFT at human-review precision.

### Scaling explanation supervision

We evaluate how risk assessment performance scales with explanation supervision on two risk assessment tasks: credit decisions and fraud decisions. For each task, we increase the sample size of explanations in Stage 1 and evaluate decision PR-AUC after label-only training in Stage 2.

Figure 4 shows that PR-AUC of both risk assessment tasks increases when the Stage 1 dataset size increases. Compared to label-only SFT, credit decision PR-AUC improves by 9.8% with 15,000 Stage 1 examples, and fraud decision PR-AUC improves by 3.2% with 25,000 examples. This result highlights that more explanation supervision in Stage 1 improves the final classifier, even after Stage 2 trains only on labels.

![PR-AUC as the Stage 1 training set grows, for the credit decision up to 15,000 examples and for the fraud decision up to 25,000 examples. Each curve uses its own scale. Both curves rise at every evaluated size.](https://stripe.dev/images/learning-from-explanations/image7.png)\
**Figure 4.** PR-AUC as the Stage 1 training set grows. The y-axis scales each decision separately. Each curve runs from its PR-AUC without Stage 1 data at the bottom to its PR-AUC with the largest Stage 1 set at the top. Both decisions improve monotonically at every evaluated data size.

### Explanations transfer across tasks

Next, we evaluate how explanations transfer across tasks. We compare three training setups: label-only SFT, two-stage SFT with explanations from the target task, and two-stage SFT with explanations from another task. All three train on the target task’s labels in Stage 2.

For cross-task training, we initialize Stage 2 from a checkpoint trained on another task's explanations, rather than explanations specific to the target task. On both credit and fraud decisions, this approach captures most of the improvement from same-task explanations. For credit decisions, PR-AUC rises from 0.578 with label-only SFT to 0.631 with cross-task explanations, compared with 0.635 with same-task explanations. For fraud decisions, PR-AUC rises from 0.710 to 0.739, compared with 0.747.

These results suggest that Stage 1 teaches connections between evidence and decisions that are useful beyond the task on which they were learned. An explanation-trained checkpoint may therefore provide a useful starting point for a new risk assessment task, even before task-specific explanations are available.

| **Stage 1 training**     | **Credit Decisions** | **Fraud Decisions** |
| ------------------------ | -------------------- | ------------------- |
| No Stage 1               | 0.578                | 0.710               |
| Stage 1 on another task  | 0.631                | 0.739               |
| Stage 1 on the same task | 0.635                | 0.747               |

## Recovering explanations with on-policy distillation

The gains from Stage 2 come with a cost. The model often fails to generate explanations, and the explanations it does produce are low quality. Explanations helped the model learn to make better decisions, but label-only training weakens its ability to state the connections between evidence and a decision. We still need those explanations to inspect an assessment or investigate a mistake. So how can we restore reliable explanations while keeping the decision gains from Stage 2?

### Stage 3: On-policy distillation

To restore the explanation capability, we apply on-policy distillation (OPD) as Stage 3 of training. In this stage, we use the Stage 1 model as a frozen teacher and initialize the student from the Stage 2 checkpoint. Given a serialized account record $s(x_i)$, the student samples a response $z=(z_1,\ldots,z_T)$. The teacher and student then each score every sampled token, conditioned on the same record and the same student-generated prefix $z_{<t}$. The per-token reward is the difference between their log probabilities:

$$
r_t = \log p_{\mathrm{teacher}}\!\left(z_t \mid s(x_i), z_{<t}\right) - \log p_{\mathrm{student}}\!\left(z_t \mid s(x_i), z_{<t}\right)
$$

The reward is negative wherever the student is more confident in a token than the teacher. Maximizing it minimizes the reverse KL divergence, $D_{\mathrm{KL}}\left(p_{\mathrm{student}} \,\|\, p_{\mathrm{teacher}}\right)$, over the student's own responses. After each update, we sample fresh responses from the updated student and repeat. Because the training data comes from the student's current behavior, the method is on-policy ([Thinking Machines Lab, 2025](https://thinkingmachines.ai/blog/on-policy-distillation/)).

OPD can help recover explanations in two ways. Reverse KL discourages the student from assigning high probability to responses that the teacher considers unlikely. This mode-seeking behavior encourages the student to concentrate on responses supported by the teacher ([Gu et al., 2023](https://arxiv.org/abs/2306.08543)). On-policy sampling also reduces exposure bias. The student learns to continue from its own generated prefixes, including mistakes, as it must at inference time ([Agarwal et al., 2023](https://arxiv.org/abs/2306.13649)). Together, these mechanisms align both the student’s predictions and its training contexts with the behavior we want to recover.

![Off-policy distillation trains the student on teacher-generated tokens with forward KL. On-policy distillation samples tokens from the student, and the teacher scores each token to give a token-level reward under reverse KL](https://stripe.dev/images/learning-from-explanations/image12.png)\
**Figure 5.** Off-policy distillation (SFT) and on-policy distillation. In SFT, the teacher generates the response and the student learns to match it. In OPD, the student generates the response, and the frozen teacher scores each token to give a token-level reward.

### OPD restores explanation quality

We evaluate explanations using two measures. Rubric Score measures the quality of each explanation against an internal evaluation rubric. Review Alignment compares the model’s explanation with the human review of the same account. It measures how closely they agree in their interpretation of the evidence. The two measures use different scales.

| **Model**                       | **Rubric Score** | **Review Alignment** |
| ------------------------------- | ---------------- | -------------------- |
| Stage 1: Knowledge distillation | 0.677            | 37.83                |
| Stage 2: Decision calibration   | 0.191            | 10.92                |
| Stage 3: On-policy distillation | **0.835**        | **41.13**            |

OPD recovers the explanation capability that Stage 2 weakened, and its explanations even score higher than those of the Stage 1 teacher on both measures. This improvement stems from on-policy training: by learning from the student's own rollouts rather than teacher-forced prefixes, Stage 3 overcomes the exposure bias inherent in Stage 1's off-policy SFT.

### OPD preserves decision performance

Recovering explanations is useful only if we keep the decision gains from Stage 2. We therefore check whether distilling from the Stage 1 teacher lowers decision performance.

The table below compares Fraud Review decision performance across the three stages, at human-review precision. OPD does not lower recall; it modestly improves on Stage 2, and both stages exceed the Stage 1 teacher. Together with the explanation results, this suggests that OPD can restore the ability to explain decisions without losing the accuracy gained from label-only training.

| **Model**                       | **Recall @ Human-Review Precision** |
| ------------------------------- | ----------------------------------- |
| Stage 1: Knowledge distillation | 0.471                               |
| Stage 2: Decision calibration   | 0.539                               |
| Stage 3: On-policy distillation | **0.547**                           |

## Improving decisions and explanations

Labels only tell a model which answer is correct, while explanations provide supervision about which evidence matters and how to interpret it. This richer signal enables the model to learn representations that connect evidence and decisions. Across multiple risk assessment tasks, we show that learning from explanations improves decision performance, with gains that scale with explanation data and transfer to adjacent tasks. On-policy distillation restores the explanation quality lost due to catastrophic forgetting during label-only SFT, while preserving those decision gains.
