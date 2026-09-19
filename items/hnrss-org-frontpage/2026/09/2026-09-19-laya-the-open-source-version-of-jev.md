---
title: Laya the open source version of Jev
link: https://laya.convaiinnovations.com/
source: hnrss-org-frontpage
published: 2026-09-19T10:46:58Z
updated: 2026-09-19T10:46:58Z
first_seen: 2026-09-19T13:22:28.237536491Z
authors:
- nandakishor_ml
summary: 'Article URL: https://laya.convaiinnovations.com/ Comments URL: https://news.ycombinator.com/item?id=49765348 Points: 105 # Comments: 17'
content: extracted
html: 2026-09-19-laya-the-open-source-version-of-jev.html
preview:
  file: 2026-09-19-laya-the-open-source-version-of-jev.preview-4beed818d6fa.webp
  width: 256
  height: 219
  alt: 'Laya versus TypeSafe Jev: comprehensive benchmarks'
  color: '#e0e5e4'
---

Everyone in AI right now is talking about a new kind of model: an architecture that is not autoregressive, does not generate text, and gives lightning-fast probability predictions over structured schemas.

Seeing the hype online feels both validating and deeply frustrating.

I worked on this literally one year back in March 2025. I spent months of hard work, sweat, and sleepless nights building it, published an arXiv paper ([arXiv:2503.23303](https://arxiv.org/abs/2503.23303)), released the model weights on Hugging Face ([sales-conversion-model-reinf-learning](https://huggingface.co/DeepMostInnovations/sales-conversion-model-reinf-learning)), published the open dataset ([saas-sales-conversations](https://huggingface.co/datasets/DeepMostInnovations/saas-sales-conversations)), built a PyPI package, and posted the whole approach on Reddit ([r/LocalLLaMA discussion](https://www.reddit.com/r/LocalLLaMA/s/6eGEwsAz43)).

Then in September 2025, I published a second paper ([arXiv:2510.01237](https://arxiv.org/abs/2510.01237)), formalizing the framework for schema-based decisions guided by reinforcement learning. The guiding brain in my system was always reinforcement learning, not just an embedding model or an autoregressive LLM.

And then in September 2026, a well-funded frontier lab called TypeSafe AI (founded by Diogo Almeida, a co-inventor of ChatGPT at OpenAI) launched Jev. They proposed the exact same non-autoregressive decision concept as if it was a brand-new scientific breakthrough. Except they launched without technical papers, without open weights, and with zero open training datasets.

My earlier model used PPO over sequence representations to output turn-by-turn conversion trajectories (probabilities from 0.0 to 1.0) in vertical sales conversations. Jev generalized parallel sampling using what they called RLCD (Reinforcement Learning for Calibrated Decisions) to output confidence distributions and schema choices horizontally, charging $0.042 per million input tokens with typical response times around 150 ms.

Instead of staying bitter, I decided to take everything I learned, fix every architectural limitation of the old approach, and build a completely open, horizontal System 1 decision model family: **Laya**.

And because we built it properly on bidirectional encoders, our models run in **32.8 milliseconds on a single GPU (7.2 ms/question batched)**, making it **6 to 8 times faster than Jev**, with full support for over 100 languages, zero API subscription costs, and 100% open-source Apache 2.0 weights.

* * *

## 1\. The Core Realization: System 1 vs System 2

Every modern AI pipeline has a giant bottleneck: we use generative LLMs for simple reflex decisions.

When a customer support ticket arrives, or an email hits your inbox, or a user submits a prompt to your API, you usually only need to answer simple, structured questions:

- Which department should this ticket route to?
- Is this incoming email a phishing attack or spam?
- Is this prompt trying to jailbreak or inject instructions?
- How urgent is this issue on an ordinal rubric (0 to 3)?
- Does this query require code execution or a simple factual reply?

Calling an 8B, 70B, or frontier generative LLM for this is complete overkill. You wait 500 ms to 2,000 ms for tokens to stream out, spend real money on inference, and then have to write regex or JSON parsers to extract a clean label from free-form text. Worst of all, LLMs love to hallucinate and generate fake confidence. When an LLM outputs `"confidence: 0.95"`, it is just predicting tokens that sound confident. There is zero mathematical calibration behind it.

We needed a model that works like the human brain's System 1: instant reflex decisions with honest, calibrated probabilities, taking only 30 to 35 milliseconds on standard commodity hardware.

* * *

## 2\. The Three Decision Primitives

Laya evaluates typed questions over any state (raw text, email, ticket, or JSON document) in **a single forward pass**. It relies on three primitives:

1. **choice**: Pick one option from a dictionary of criteria. Returns the selected key, probability distribution across all options, and a calibrated confidence score.
2. **score**: Place the state on an ordinal rubric (levels 0, 1, 2, ...). Returns the expected level, the distribution over rubric ranks, and confidence.
3. **noul**: A direct boolean question returning calibrated probability P(true) from 0.0 to 1.0 (with P(false) = 1 - P(true) by construction).

Because the output space consists purely of probabilities and numbers, the model never generates text, cannot hallucinate, and schema violations or malformed JSON are physically impossible.

* * *

## 3\. The Three Checkpoints & Bundled Hub Architecture

One model cannot be optimal for every task and language. We released three specialized checkpoints, now consolidated under a single repository hub on Hugging Face:

| Checkpoint                                                                                               | Backbone Encoder         | Params | Context         | Primary Strength                                                                       |
| -------------------------------------------------------------------------------------------------------- | ------------------------ | ------ | --------------- | -------------------------------------------------------------------------------------- |
| [convaiinnovations/laya](https://huggingface.co/convaiinnovations/laya)                                 | ModernBERT-large         | 421M   | 512             | English text classification, guardrails, email triage                                  |
| [convaiinnovations/laya-multilingual](https://huggingface.co/convaiinnovations/laya-multilingual)       | mmBERT-base (256k vocab) | 322M   | 1024 (up to 8k) | 100+ languages, 2.2x faster, cross-lingual NLI                                         |
| [convaiinnovations/laya-typed-decisions](https://huggingface.co/convaiinnovations/laya-typed-decisions) | ModernBERT-large         | 421M   | 1024            | Agent observability, customer service, invoice processing, security alerts (0.766 acc) |

### Selective Subfolder Downloads

Rather than forcing users to manage three separate repositories or download 2.5 GB of combined weights, the main repository [convaiinnovations/laya](https://huggingface.co/convaiinnovations/laya) bundles all three. Using Hugging Face's `allow_patterns`, Laya's SDK downloads only the specific subfolder requested:

```
# Downloads English model (~808 MB)
agent_en = laya.load("convaiinnovations/laya")

# Downloads ONLY the multilingual subfolder (~647 MB), not the entire 2.5 GB bundle
agent_ml = laya.load("convaiinnovations/laya", subfolder="multilingual")
```

* * *

## 4\. Why Routing Is Essential: The Multi-Script Reality

One of the most eye-opening findings from our 51-language sweep on the MASSIVE benchmark (20 options, random baseline = 0.050) was how English models fail outside Latin script.

ModernBERT-large's 50,000-token English BPE vocabulary simply shreds non-Latin alphabets:

- **Khmer:** 0.000 accuracy at **0.952 mean confidence**. Not one correct decision in 100 questions, while reporting ~95% confidence.
- **Armenian:** 0.050 accuracy (exact coin-flip random) at 0.885 confidence.
- **Hebrew:** 0.060 accuracy at 0.964 confidence.
- **Bengali:** 0.080 accuracy at 0.945 confidence.
- **Hindi:** 0.100 accuracy at 0.941 confidence.

This is the crucial lesson: **the model's own confidence gives no warning when it cannot read the input script**. Across 51 languages, the English checkpoint's mean confidence never drops below 0.885, regardless of whether its accuracy is 82% or 0%.

Therefore, **confidence gating cannot protect you**. The decision of which model to use must be made *before* the forward pass.

### Sub-Millisecond Pure Python Routing

Laya includes a built-in `Router` that inspects the Unicode scripts of incoming text across 22 alphabets (Devanagari, CJK Han, Cyrillic, Arabic, Hebrew, Tamil, Thai, etc.) and analyzes Latin stopword distributions:

- Standard English text: **0.09 ms** detection overhead.
- Devanagari / Indic text: **0.54 ms** detection overhead.
- Large 200-row nested JSON documents: **0.73 ms** detection overhead.

Compared to a 33 ms forward pass, routing overhead is negligible (<2%). And with `Router(preload=True)`, all required models stay resident in VRAM/RAM, completely eliminating the 7 to 10-second cold-swap penalty when traffic alternates between languages.

```
from laya import Router

# Preload checkpoints into memory for instant sub-35ms routing
router = Router(preload=True)

# English -> automatically routed to ModernBERT-large
res_en = router.predict({"body": "I was charged twice, please refund."}, questions)

# Hindi -> automatically routed to mmBERT-base (100+ languages)
res_hi = router.predict({"body": "मुझसे दो बार शुल्क लिया गया, कृपया पैसे वापस करें।"}, questions)

# Explicit override when you already know the domain
res_spec = router.predict(state, questions, model="typed-decisions")
```

* * *

## 5\. Head-to-Head: Laya (with Routing) vs TypeSafe Jev

We benchmarked Laya directly against TypeSafe Jev across public datasets and standard benchmarks. Every Laya number is measured; Jev numbers are published by third-party independent studies (AbdelStark, nibzard) and TypeSafe AI.

| Benchmark / Metric                 | TypeSafe Jev 1.13.0    | Laya (Routed)           | Advantage / Delta                     |
| ---------------------------------- | ---------------------- | ----------------------- | ------------------------------------- |
| typed-decisions (2,000 decisions)  | 0.727                  | 0.766                   | +3.9% (beats 0.735 teacher ceiling)   |
| AG News (4 labels)                 | 0.910                  | 0.950                   | +4.0% higher accuracy                 |
| DAIR Emotion (6 labels)            | 0.480 (Brier 0.846)    | 0.595                   | +11.5% higher (Jev had 16% zero prob) |
| Calibration Error (ECE)            | 0.246                  | 0.081                   | 3x better probability calibration     |
| Latency P50 (1 Question)           | 236 – 276 ms           | 32.8 ms                 | 7.8x faster execution                 |
| Latency P50 (10 Questions Batched) | \~1,500 ms (serial)    | 72.3 ms (7.2 ms/q)      | 20x faster on batched calls           |
| Usable Languages (> 3x random)     | No published benchmark | 45 of 51 languages      | Global language coverage              |
| Cost per 1M tokens                 | $0.042 (metered API)   | $0.00 (self-hosted)     | 100% free Apache 2.0                  |
| Model Weights & Code               | Closed proprietary API | Open-source safetensors | Air-gapped & on-premise capable       |

### Real-World Application Workflows

Across 9 evaluated enterprise workflows, Laya demonstrates production-ready decision quality:

- **Email Spam Filtering (Enron):** **0.993 accuracy**, 0.993 F1, 0.013 ECE.
- **Phishing Detection:** **0.980 accuracy**, 0.979 F1, 0.012 ECE.
- **LLM Guardrails & Jailbreaking (held-out ToxicChat):** **0.755 – 0.762 accuracy**. At 50% selective coverage, accuracy reaches **0.931**.
- **RAG Passage Relevance Filtering:** **0.657 accuracy** in single forward pass.
- **Support Ticket Queue Routing (10-way):** **0.522 accuracy**.

* * *

## 6\. Honest Limitations: Where Laya Has Ceilings

Too many AI announcements hide their weaknesses. We believe in engineering honesty:

1. **Choice questions degrade with >20 options:** In our stress test on Banking77 (77 labels), Laya scored 0.425 against Jev's 0.870. This is an architectural budget constraint: options share a 192-256 token `head_max_len` budget, leaving only ~3-4 tokens per candidate at 77 options. *Recommendation:* Keep choice schemas under 20 options, or use a two-step coarse-to-fine hierarchy.
2. **Zero-shot vs. Fine-tuning:** Out-of-the-box base models score ~0.35 on the typed-decisions benchmark (near random). The 0.766 score is achieved by fine-tuning on the benchmark's train split. Treat Laya as a fast foundation model to specialize, not as an omniscient zero-shot oracle.
3. **Temperature Calibration:** Base weights ship with raw temperature logits. Fitting a single scalar temperature per question type on your domain distribution cuts expected calibration error from 0.466 to 0.081.

* * *

## 7\. Quickstart: Running Laya in 30 Seconds

```
pip install laya>=0.3.3
```

Here is a complete example running multi-schema decisions with automatic language routing:

```
import laya
from laya import Router

# Initialize router with preloading (avoids swap delay)
router = Router(preload=True)

# Define complex state
ticket = {
    "ticket_id": "TCK-8821",
    "customer": "enterprise_user",
    "subject": "System downtime and billing dispute",
    "body": "Our production API has been failing since 6 AM. We lost critical transactions. We demand an immediate SLA refund."
}

# Define multiple questions of different primitives
questions = {
    "queue": {
        "type": "choice",
        "instructions": "Which engineering queue owns this ticket?",
        "criteria": {
            "infrastructure": "server outages, network downtime, database failures",
            "billing": "refunds, SLA credits, invoice disputes",
            "security": "breaches, vulnerability reports",
            "support": "general customer inquiries"
        }
    },
    "urgency": {
        "type": "score",
        "instructions": "How urgent is this ticket?",
        "criteria": ["low priority", "medium", "high priority", "critical blocker"]
    },
    "churn_risk": {
        "type": "noul",
        "instructions": "Does the customer threaten to cancel or express severe churn intent?"
    }
}

# Single forward pass: evaluates all questions simultaneously
res = router.predict(ticket, questions)

print("Routing Decision :", res["routing"]["model"])
# -> english

print("Assigned Queue   :", res["answers"]["queue"]["choice"])
# -> infrastructure (confidence: 0.96)

print("Urgency Score    :", res["answers"]["urgency"]["score"])
# -> 2.87 / 3.0

print("Churn Risk       :", f"{res['answers']['churn_risk']['noul']:.1%}")
# -> 91.4%
```

* * *

## 8\. Resources & Community

- **Hugging Face Model Hub:** [convaiinnovations/laya](https://huggingface.co/convaiinnovations/laya) (holds all 3 checkpoints)
- **Live Interactive Space:** [convaiinnovations/laya-demo](https://huggingface.co/spaces/convaiinnovations/laya-demo) (try 8 workflows & multilingual routing live on ZeroGPU)
- **GitHub Repository:** [github.com/NandhaKishorM/laya](https://github.com/NandhaKishorM/laya) (code, router, and reproducible benchmark harnesses on the `research` branch)
- **PyPI Package:** [pip install laya](https://pypi.org/project/laya/)
- **Kaggle 2xT4 Fine-Tuning Notebook:** [laya\_finetune\_typed\_decisions\_2xT4\_kaggle.ipynb](https://github.com/NandhaKishorM/laya/blob/main/notebooks/laya_finetune_typed_decisions_2xT4_kaggle.ipynb) (train your own custom System 1 model in ~4 hours on free Kaggle GPUs)

* * *

## Conclusion

It took a year of research, from our March 2025 arXiv paper to today, but the core realization remains: **not every AI problem requires an autoregressive chatbot**.

For high-volume classification, guardrails, routing, and triage, a sub-35ms bidirectional decision model trained with RLCD delivers 7.8x faster execution than proprietary alternatives, zero hallucinations, global language routing, and honest confidence scores you can actually branch on in production code.

And best of all, it is 100% open-source for the entire community.
