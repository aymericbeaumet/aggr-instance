---
title: I Built Non-Autoregressive Decision Models a Year Ago. Then a Frontier Lab Called It a "Breakthrough"
link: https://dev.to/nandakishor_m_6cc0adfde9f/i-built-non-autoregressive-decision-models-a-year-ago-then-a-frontier-lab-called-it-a-18me
source: lobste-rs-top-1w
published: 2026-09-19T13:41:57Z
updated: 2026-09-19T13:41:57Z
first_seen: 2026-09-21T17:01:01.954977951Z
authors:
- dev.to via Yogthos
labels:
- ai
summary: Comments
content: extracted
html: 2026-09-19-i-built-non-autoregressive-decision-models-a-year-ago-then.html
preview:
  file: 2026-09-19-i-built-non-autoregressive-decision-models-a-year-ago-then.preview-914fda6cc741.webp
  width: 256
  height: 134
  color: '#2c3149'
images:
- source: https://media2.dev.to/dynamic/image/width=1200,height=627,fit=cover,gravity=auto,format=auto/https%3A%2F%2Fdev-to-uploads.s3.us-east-2.amazonaws.com%2Fuploads%2Farticles%2Fokldcp34z2nxkvu9ml2b.png
  original:
    file: 2026-09-19-i-built-non-autoregressive-decision-models-a-year-ago-then.image-d7ddc2b756d1.webp
    width: 1200
    height: 627
  variants:
  - file: 2026-09-19-i-built-non-autoregressive-decision-models-a-year-ago-then.image-9ebbe1eef167.webp
    width: 320
    height: 167
  - file: 2026-09-19-i-built-non-autoregressive-decision-models-a-year-ago-then.image-5573bed3fb29.webp
    width: 640
    height: 334
  color: '#1d283b'
- source: https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.us-east-2.amazonaws.com%2Fuploads%2Farticles%2Fz8lepygpvckfgnrtgp7r.png
  original:
    file: 2026-09-19-i-built-non-autoregressive-decision-models-a-year-ago-then.image-a9dbf52019c3.webp
    width: 800
    height: 640
  variants:
  - file: 2026-09-19-i-built-non-autoregressive-decision-models-a-year-ago-then.image-2260493d5baa.webp
    width: 320
    height: 256
  - file: 2026-09-19-i-built-non-autoregressive-decision-models-a-year-ago-then.image-9a9d82fa8c23.webp
    width: 640
    height: 512
  color: '#fbfbfb'
---

### From our March 2025 arXiv paper on RL conversion trajectories to building a sub-40ms open-weight System 1 decision engine with RLCD.

Everyone in AI right now is talking about a new kind of model: an architecture that is not auto-regressive, does not generate text, and gives lightning fast probability predictions over a structured JSON schema.

Seeing the hype online feels both validating and deeply frustrating.

I worked on this literally one year back in March 2025. I spent months of hard work, sweat, and sleepless nights building it, published an arXiv paper ([arXiv:2503.23303](https://arxiv.org/abs/2503.23303)), released the model weights on Hugging Face ([sales-conversion-model-reinf-learning](https://huggingface.co/DeepMostInnovations/sales-conversion-model-reinf-learning)), published the open dataset ([saas-sales-conversations](https://huggingface.co/datasets/DeepMostInnovations/saas-sales-conversations)), built a PyPi package, and posted the whole approach on Reddit ([r/LocalLLaMA post](https://www.reddit.com/r/LocalLLaMA/s/6eGEwsAz43)).

Then in September 2025, I published a second paper ([arXiv:2510.01237](https://arxiv.org/abs/2510.01237)), laying out the exact framework for schema-based decisions guided by reinforcement learning. For anyone curious, the guiding brain in my system was always reinforcement learning, not just an embedding model or an autoregressive LLM.

And then in September 2026, a well-funded frontier lab called TypeSafe AI (founded by Diogo Almeida, a co-inventor of ChatGPT at OpenAI) launched Jev. They proposed the exact same non-autoregressive decision concept as if it was a brand-new scientific breakthrough. Except they launched without technical papers, without open weights, and with zero open training datasets.

My earlier model used PPO over sequence representations to output turn-by-turn conversion trajectories (probabilities from 0.0 to 1.0) in vertical sales conversations. Jev generalized parallel sampling using what they called RLCD (Reinforcement Learning for Calibrated Decisions) to output confidence distributions and schema choices horizontally, charging $0.042 per million input tokens with typical response times around 150 ms.

It is incredibly frustrating when something you poured your heart into for months as an open-source researcher gets overlooked because it was built for a vertical use case, while a funded lab packages the same core idea horizontally and gets all the glory. But that is the open-source story in general 🙂.

Instead of staying bitter, I decided to take everything I learned from my March 2025 and September 2025 papers, fix every architectural limitation of the old approach, and build a completely open, horizontal System 1 decision model: **RL Agent**.

And because we built it properly on a bidirectional encoder, our model runs in **33 to 38 milliseconds on a GPU**, making it roughly 4x faster than Jev's published 150 ms latency, and it is 100% open-source.

Here is the full story of how it works, the architecture, the math of RLCD with strictly proper scoring rules, and why non-autoregressive decision models are the future.

* * *

## 1. The Real Problem: Why LLMs are Terrible for Decisions

Every modern AI pipeline has a giant bottleneck: we use generative LLMs for simple reflex decisions.

When a customer support ticket arrives, or an email hits your inbox, or a user submits a prompt to your API, you usually only need to answer a few simple questions:

- Which department should this go to?
- Is this email a phishing attack?
- Is this prompt trying to jailbreak the system?
- How urgent is this issue on a scale of 0 to 3?

Calling an 8B or 70B generative LLM for this is complete overkill. You wait 500ms to 2,000ms for tokens to stream out, spend real money on inference, and then have to write regex or JSON parsers to extract a clean label from free-form text. Worst of all, LLMs love to hallucinate and generate fake confidence. When an LLM outputs `"confidence: 0.95"`, it is just predicting tokens that sound confident. There is zero mathematical calibration behind it.

We needed a model that works like the human brain's System 1: instant, reflex decisions with honest, calibrated probabilities, taking only 30 to 40 milliseconds on standard hardware.

* * *

## 2. The Three Decision Primitives

Following the System 1 philosophy, RL Agent accepts a **state** (raw text, email, ticket, or JSON document) along with one or more **typed questions**, and evaluates all of them in a single, parallel forward pass.

It uses three primitives:

1. **choice**: Pick one option from a dictionary of criteria. Returns the chosen label, probabilities for each candidate option, and a confidence score. (Great for department routing, intent detection, topic classification).
2. **score**: Place the state on an ordinal rubric like levels 0, 1, 2, 3. Returns an expected score value, probabilities across levels, and confidence. (Great for customer frustration, urgency, prompt harm severity).
3. **noul**: A direct boolean question returning calibrated probability P(true) from 0.0 to 1.0. (Great for detecting phishing, spam, jailbreaks, or churn risk).

Because the output space is strictly probabilities and numbers, the model never generates text, cannot hallucinate, and broken JSON is physically impossible.

* * *

## 3. Model Architecture: 421M Parameters

In my March 2025 work, I used frozen sequence embeddings combined with a separate PPO value network. It worked for turn-by-turn sales prediction, but it was not end-to-end and could not handle dynamic new questions at runtime.

For RL Agent, we built a 421M-parameter end-to-end architecture with two tightly coupled components:

```
State (Text or JSON) + Typed Questions & Options
                       │
                       ▼
Prompt Construction:
[CLS] <type> question: <instructions> [SEP] [MASK] opt0 [MASK] opt1 ... [SEP] <state> [SEP]
                       │
                       ▼
┌──────────────────────────────────────────────────────────────┐
│ ENCODER: ModernBERT-large (395M, Bidirectional)              │
│ • 28 layers, hidden dim 1024, 16 heads, GeGLU MLP            │
│ • Full sequence bidirectional attention across state and opts│
└──────────────────────────────┬───────────────────────────────┘
                               │ Hidden states [L × 1024]
                               ▼
               + Question-Type Embedding (choice=0, score=1, noul=2)
                               │
┌──────────────────────────────▼───────────────────────────────┐
│ DECISION HEAD TRANSFORMER (~25.2M parameters)                │
│ • 2 TransformerEncoder layers (d=1024, 16 heads, FFN=4096)   │
│ • Pre-LayerNorm, Dropout 0.1                                 │
└──────────────┬───────────────────────────────┬───────────────┘
               │                               │
               ▼ Extract [MASK] markers        ▼ Pooled [CLS] state
┌──────────────────────────────┐ ┌─────────────────────────────┐
│ OPTION SCORER (~1.05M)       │ │ ACT / ESCALATE HEAD (~0.26M)│
│ • LayerNorm(1024)            │ │ • Input: [CLS] (1024)       │
│ • Linear(1024 -> 1024)       │ │   + 4 distribution stats    │
│ • GELU()                     │ │ • Linear(1028 -> 256)       │
│ • Linear(1024 -> 1)          │ │ • GELU()                    │
│ Yields 1 scalar logit per opt│ │ • Linear(256 -> 2)          │
└──────────────┬───────────────┘ └─────────────┬───────────────┘
               │                               │
               ▼                               ▼
       Logits ÷ Temperature           P(Act) vs P(Escalate)
         -> Softmax                                  
               │
               ▼
 [ Probabilities + Confidence ]
```

### ModernBERT-large Backbone

We use **ModernBERT-large** (395M parameters) as our base encoder. ModernBERT has 28 layers, hidden dimension 1024, 16 attention heads, GeGLU intermediate layers (dim 2624), and supports 8,192 tokens via rotary position embeddings (RoPE). Because it is fully bidirectional, every token attends to all parts of the state and options simultaneously.

### The \[MASK\] Option Extraction Mechanism

For every question, our `build_sequence` function packs the prompt:

```
[CLS] choice question: Which team should handle `body`? [SEP]
[MASK] billing: payments [MASK] tech: bugs [MASK] other: general [SEP]
{"subject": "Refund request", "body": "I was billed twice..."} [SEP]
```

Each option gets a `[MASK]` token. After passing through ModernBERT and the 2-layer decision head transformer, we use `torch.gather` to pull the hidden states specifically at those marker positions.

The Option Scorer MLP projects each 1024-dim marker state to a scalar logit. A softmax over the options belonging to that question yields the candidate probability distribution:

```
p = softmax(z / T)
```

where T is a temperature fitted per question type and option count.

### The Act vs Escalate Head

In real automated systems, you want to know when to trust the model and when to call a human. We added an Act/Escalate head that takes the pooled \[CLS\] token (1024 dim) concatenated with 4 distribution features:

- Top probability: max(p)
- Top-2 margin: p\_top1 - p\_top2
- Normalized entropy: H(p) / log(K)
- Option budget ratio: K / 255

This 1028-dim vector passes through a 2-layer MLP to output \[P(act), P(escalate)\].

### Single Forward Pass for Multiple Questions

If you have an email and ask 5 questions about it, all 5 sequences are collated into one batch. ModernBERT processes them all in a single forward pass in about 35 milliseconds on a GPU.

* * *

## 4. Training with RLCD: The Mathematics of Calibration

How do you train this with reinforcement learning so the probabilities are genuinely calibrated?

### The Trap with Normal Classification and Naive RL

If you train a classifier with cross entropy:

```
Loss_CE = -sum(y_k * log(p_k))
```

the loss can only be minimized when the winner logit approaches infinity. The model gets overconfident.

And if you try standard reinforcement learning with a binary reward (+1 if correct, 0 if wrong), the expected reward is:

```
E[R] = sum(y_k * p_k)
```

The policy gradient forces the highest probability toward 1.0 and all others toward 0.0. In other words, naive RL maximizes accuracy by destroying calibration. It turns your model into a confidently wrong machine.

### Strictly Proper Scoring Rules

The foundation of RLCD (Reinforcement Learning for Calibrated Decisions) is using a strictly proper scoring rule as the reward.

In decision theory, a scoring rule S(q, y) assigns a score when the model reports distribution q and the true outcome is y. It is strictly proper if and only if the expected score is uniquely maximized when q equals the true distribution p:

```
E_{y ~ p}[S(q, y)] <= E_{y ~ p}[S(p, y)], with equality if and only if q = p
```

This guarantees that the model receives the highest possible reward only when it reports honest probabilities.

Our composite reward combines three proper scores:

```
Reward(q, y) = S_log(q, y) + 0.5 * S_sph(q, y) - 1.0 * S_rps(q, y)
```

#### 1. Logarithmic Score (S\_log)

```
S_log(q, y) = sum(y_k * log(max(q_k, 1e-12)))
```

Penalizes the model heavily if it assigns low probability to the true outcome. We clamp the floor to -9.21 for numerical stability.

#### 2. Spherical Score (S\_sph)

```
S_sph(q, y) = sum(y_k * q_k) / sqrt(sum(q_k^2))
```

A bounded score in \[0, 1\] that rewards placing probability mass on the correct class without the extreme gradient spikes of pure log-loss.

#### 3. Ranked Probability Score (S\_rps)

Used for ordinal score questions (rubrics like 0, 1, 2, 3). If true urgency is level 3, guessing level 2 is much better than guessing level 0. RPS measures the squared distance between the cumulative distributions:

```
S_rps(q, y) = (1 / (K - 1)) * sum((CDF_q(i) - CDF_y(i))^2)
```

Subtracting RPS from the reward teaches the model distance along a rubric.

### The Policy Gradient Update (REINFORCE with Group Baseline)

We train with pure policy gradient (zero supervised cross-entropy loss):

1. **Gaussian Exploration**: For each question, we sample G = 8 noisy candidate logits:

```
   z_noisy = z + epsilon, where epsilon ~ Normal(0, sigma^2)
```

We project the noise vector so its sum across options is zero (epsilon - mean(epsilon)), since adding a constant to all logits cancels out in softmax. The exploration standard deviation sigma decays from 1.0 down to 0.3.

1. **Noisy Distribution**:

```
   q_noisy = softmax(z_noisy)
```

1. **Reward Evaluation**:

```
   r_noisy = Reward(q_noisy, y)
```

1. **Group Mean Advantage (GRPO style)**: We compute advantages relative to the group mean across the 8 samples:

```
   Advantage = (r_noisy - mean(r)) / (std(r) + 1e-6)
```

1. **Policy Loss**: Using the Gaussian log-probability:

```
   Loss_policy = -mean(Advantage * log_prob(z_noisy | z))
```

1. **Cost-Sensitive Act Head**: The act head samples actions in {act, escalate} and receives rewards from a cost matrix:
   - Action correct: +1.0
   - Action incorrect: -3.0
   - Escalated: -0.5

Taking an automated action is profitable only when:

```
   P(correct) * (+1.0) + (1 - P(correct)) * (-3.0) > -0.5 => P(correct) > 2.5 / 4.0 = 0.625
```

The policy automatically learns to act only when confidence is above 62.5%.

* * *

## 5. Multi-Turn Trajectories with TD(lambda)

In my March 2025 paper, we predicted sales conversion across turns using PPO. One big issue we uncovered back then was leakage: if you feed the whole conversation embedding at turn 1, the model sees future signals.

In RL Agent, we solved this cleanly:

- Conversations are sliced turn-by-turn into prefixes (turn 1, turn 2, turn 3...). The model only receives context up to that turn.
- We apply Temporal Difference learning with Monte Carlo return targets (TD(lambda = 1.0)):

```
G_T = y (terminal outcome)
G_t = (1 - lambda) * V(s_{t+1}) + lambda * G_{t+1}
```

With lambda = 1.0, early turns are trained directly against the real terminal outcome of the conversation, learning what early dialogue patterns genuinely lead to conversion or churn without bootstrapping from the model's own guesses.

* * *

## 6. Confidence via Normalized Entropy

For every question, the model returns a confidence score between 0.0 and 1.0. We calculate this using normalized Shannon entropy:

```
Confidence = 1 - (Entropy(p) / log(K))
           = 1 + sum(p_k * log(max(p_k, 1e-12))) / log(K)
```

where K is the number of options:

- If the model is completely unsure, all options have probability 1/K. Entropy equals log(K), so confidence is exactly **0.00**.
- If the model is completely certain, one option has probability 1.0 and entropy is 0, so confidence is **1.00**.

* * *

## 7. Dataset Pipeline: Zero Synthetic Shortcuts

Many teams try to build these models by asking an LLM to generate synthetic training questions and labels. That is a mistake. When you train a calibration model on synthetic labels, you simply calibrate your model to the LLM's own mistakes and hallucinations.

We built our training pipeline using 100% human-labeled, real-world public datasets across:

- **Support Triage & Intents**: Real customer service conversations, banking intents, and ticket routing queues.
- **Inference & Fact Checking**: Premise-hypothesis pairs, fact verification, and contradiction checks.
- **Content Safety**: Human consensus labels for toxic remarks, harassment, and severe abuse.
- **Security & Guardrails**: Real jailbreak prompts and prompt injection attacks.
- **Rubrics & Quality**: Multi-axis human rubric ratings for helpfulness, complexity, and correctness.
- **Multi-Turn Trajectories**: SaaS sales and support interactions with verified final outcomes.

To stop the model from taking shortcuts, our data pipeline dynamically shuffles option orders, paraphrases questions, alternates between raw text and nested JSON states, and injects random distractor questions.

* * *

## 8. Real-World Benchmarks: Laya vs. TypeSafe Jev

[![](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.us-east-2.amazonaws.com%2Fuploads%2Farticles%2Fz8lepygpvckfgnrtgp7r.png)](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.us-east-2.amazonaws.com%2Fuploads%2Farticles%2Fz8lepygpvckfgnrtgp7r.png)

We ran extensive evaluations on our fine-tuned checkpoint across 25,424 total test questions (23,024 in-task questions and 2,400 zero-shot questions from task families never seen during training).

Then we compared our numbers directly against TypeSafe Jev's public launch numbers.

### Head-to-Head Comparison

| Metric / Dimension                   | TypeSafe Jev (Published)                  | Laya (Fine-Tuned Checkpoint)         | Analysis / Advantage                                            |
| ------------------------------------ | ----------------------------------------- | ------------------------------------ | --------------------------------------------------------------- |
| **P50 Latency (1 Question)**         | \~400 ms avg (70 to 500 ms, 150 ms best)  | **38.4 ms** (p95: 42.1 ms)           | **Laya is ~10.4x faster on avg (4x faster than Jev best-case)** |
| **Batched Latency (10 Questions)**   | \~1,500 ms (serial) / ~400 ms             | **156.0 ms** (p95: 158.4 ms)         | **Laya evaluates 10 questions in the time Jev answers 1**       |
| **Batched Latency (50 Questions)**   | Multi-second / rate-limited               | **721.4 ms**                         | High-throughput parallel mini-batching                          |
| **Benchmark Accuracy**               | **67.8%** (across 4 production workflows) | **83.8%** in-task macro accuracy     | **Laya achieves +16.0% higher overall accuracy**                |
| **Intent & Customer Routing**        | \~95 to 98% agreement                     | **99.1% accuracy** (ECE: 0.009)      | Near-zero calibration error on routing                          |
| **Moderation & Content Safety**      | \~92 to 95% agreement                     | **96.7% accuracy** (ECE: 0.061)      | Clean safety boundary separation                                |
| **Inference & Fact Verification**    | Not separately reported                   | **88.3% accuracy** (ECE: 0.054)      | Full bidirectional attention captures contradictions            |
| **Instruction-Following Tasks**      | Proprietary internal set                  | **87.8% in-task / 86.3% zero-shot**  | Proven generalization across unseen tasks                       |
| **Email Triage & Phishing**          | Vendor custom workflow                    | **73.2% accuracy** (ECE: 0.017)      | Tailored email cleaning and phishing filters                    |
| **Selective Automation (@ 50% Cov)** | Claims human escalation                   | **92.2% accuracy** (ECE: 0.041)      | Safe automated gating (confidence >= 0.85)                      |
| **Model Weights & Code**             | Closed-source / proprietary API           | **100% Open-source Apache 2.0**      | Full data sovereignty and transparency                          |
| **Inference Cost**                   | $0.042 / 1M input tokens recurring        | **$0.00 / self-hosted**              | Runs on commodity GPUs, Mac MPS, or CPU                         |
| **Multi-Turn Trajectory Modeling**   | Static state snapshots                    | **TD(lambda = 1.0) prefix modeling** | Real temporal credit assignment                                 |
| **Deployment Mode**                  | Cloud-only egress                         | **Air-gapped / Local / On-Device**   | Zero data egress (HIPAA/GDPR compliant)                         |

### Detailed In-Task Performance (23,024 Evaluated Questions)

| Task Family                     | Questions (N) | Accuracy  | ECE (Calibration) | NLL       |
| ------------------------------- | ------------- | --------- | ----------------- | --------- |
| **Intent and routing**          | 1,475         | **99.1%** | 0.009             | 0.181     |
| **Moderation and safety**       | 2,708         | **96.7%** | 0.061             | 0.153     |
| **Topic classification**        | 749           | **93.9%** | 0.029             | 0.196     |
| **Emotion and tone**            | 1,825         | **90.6%** | 0.018             | 0.238     |
| **Inference and fact checking** | 3,022         | **88.3%** | 0.054             | 0.340     |
| **Instruction-following tasks** | 600           | **87.8%** | 0.046             | 0.302     |
| **Robustness checks**           | 744           | **85.1%** | 0.108             | 1.058     |
| **Reading comprehension**       | 770           | **84.7%** | 0.083             | 0.409     |
| **Email triage & phishing**     | 2,691         | **73.2%** | 0.017             | 0.595     |
| **Search relevance**            | 733           | **62.8%** | 0.066             | 0.728     |
| **Response quality scoring**    | 3,146         | **58.1%** | 0.023             | 1.009     |
| **Overall Macro In-Task**       | **23,024**    | **83.8%** | **0.060**         | **0.468** |

### Zero-Shot Generalization (2,400 Questions Never Trained On)

| Task Family (Zero-Shot)         | Questions (N) | Accuracy  | ECE       | NLL       |
| ------------------------------- | ------------- | --------- | --------- | --------- |
| **Instruction-following tasks** | 600           | **86.3%** | 0.045     | 0.319     |
| **Moderation and safety**       | 600           | **79.7%** | 0.171     | 1.415     |
| **Emotion and tone**            | 600           | **58.3%** | 0.318     | 1.976     |
| **Sentiment and rating**        | 600           | **36.2%** | 0.291     | 1.798     |
| **Overall Macro Zero-Shot**     | **2,400**     | **65.1%** | **0.207** | **1.377** |

### Selective Automation in Practice

Because the model's confidence scores are calibrated using normalized entropy, you can use them directly in code to gate actions:

- Taking all answers: **83.8% accuracy**.
- Answering only the top 80% most confident predictions: **89.4% accuracy**.
- Answering only the top 50% most confident predictions: **92.2% accuracy**.

This means that if you set an automated routing threshold of confidence >= 0.85, the model can automate roughly half of your incoming support tickets, security flags, or email triages with 92%+ precision, escalating the genuinely tricky cases to humans.

* * *

## 9. How to Run It in Python

You can run Laya directly with our PyPI package in one line:

```
pip install laya
```

### Quickstart Code

```
import laya

# Download and load the fine-tuned model from Hugging Face
agent = laya.load("convaiinnovations/laya")

# Define your input state (raw text or JSON dict)
state = {
    "from": "user@company.com",
    "subject": "Charged twice on March invoice",
    "body": "Hi, we were billed twice for invoice 4411. Please refund the duplicate today or we will cancel our plan."
}

# Define your typed questions
questions = {
    "department": {
        "type": "choice",
        "instructions": "Which department should handle this email?",
        "criteria": {
            "billing": "invoices, payments, refunds",
            "technical": "bugs, outages, system errors",
            "sales": "pricing, new contracts",
            "other": "everything else"
        }
    },
    "urgency": {
        "type": "score",
        "instructions": "How urgent is this request?",
        "criteria": ["not urgent", "soon", "critical deadline or blocking issue"]
    },
    "churn_risk": {
        "type": "noul",
        "instructions": "Does the user threaten to cancel or leave?"
    },
    "is_phishing": {
        "type": "noul",
        "instructions": "Is this email a phishing or scam attempt?"
    }
}

# Run all questions in one single forward pass (~35 ms on GPU)
result = agent.predict(state, questions)
answers = result["answers"]

print("Department :", answers["department"]["choice"])
# -> billing (confidence: 0.94)

print("Urgency    :", answers["urgency"]["score"])
# -> 1.84 / 2.0

print("Churn Risk :", answers["churn_risk"]["noul"])
# -> 0.892 (89.2% probability)

print("Phishing   :", answers["is_phishing"]["noul"])
# -> 0.008 (0.8% probability)
```

### Automated Gating Logic

Because probabilities are mathematically calibrated, routing logic is simple code:

```
dept = answers["department"]["choice"]
conf = answers["department"]["confidence"]

if conf >= 0.85:
    route_automatically(dept)
else:
    send_to_human_triage(dept, reason=f"Low confidence ({conf:.2f})")
```

* * *

## 10. Links and Resources

- **Hugging Face Model:** [convaiinnovations/laya](https://huggingface.co/convaiinnovations/laya)
- **PyPI Package:** [pypi.org/project/laya](https://pypi.org/project/laya/)
- **GitHub Repository:** [github.com/NandhaKishorM/laya](https://github.com/NandhaKishorM/laya)
- **Live Interactive Space:** [convaiinnovations/laya-demo](https://huggingface.co/spaces/convaiinnovations/laya-demo)

* * *

## Conclusion

It took a year of research, from our March 2025 arXiv paper to today, but the core realization remains: **not every AI problem requires an autoregressive chatbot**. For high-volume classification, guardrails, routing, and triage, a 421M bidirectional decision model trained with RLCD delivers sub-40ms execution, zero hallucinations, and honest confidence scores you can actually rely on in production.

And this time, it is fully open-source.

*Built by Nandakishor M*
