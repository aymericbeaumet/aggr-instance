---
title: Jev in 25 Lines of Python
link: https://www.nobodywho.ai/posts/jev-in-25-lines/
source: hnrss-org-frontpage
published: 2026-09-23T07:26:23Z
updated: 2026-09-23T07:26:23Z
first_seen: 2026-09-23T10:05:50.962357490Z
authors:
- bashbjorn
summary: 'Article URL: https://www.nobodywho.ai/posts/jev-in-25-lines/ Comments URL: https://news.ycombinator.com/item?id=49812769 Points: 139 # Comments: 48'
content: extracted
html: 2026-09-23-jev-in-25-lines-of-python.html
preview:
  file: 2026-09-23-jev-in-25-lines-of-python.preview-71869b19c020.webp
  width: 256
  height: 256
  alt: My name is Jev
  color: '#876f67'
images:
- source: https://www.nobodywho.ai/assets/images/blog/2026/jev-in-25-lines/jev.png
  original:
    file: 2026-09-23-jev-in-25-lines-of-python.image-db9f18eb316b.png
    width: 500
    height: 500
  variants:
  - file: 2026-09-23-jev-in-25-lines-of-python.image-c6f8a6d5a491.webp
    width: 320
    height: 320
  - file: 2026-09-23-jev-in-25-lines-of-python.image-59696ada65a0.webp
    width: 500
    height: 500
  color: '#171316'
---

![My name is Jev](https://www.nobodywho.ai/assets/images/blog/2026/jev-in-25-lines/jev.png)

Everyone and their mom is talking about [Jev](https://typesafe.ai/blog/introducing-system-one-models-and-jev). Jev this, Jev that. Everyone on Twitter is all over Jev, how it's the next frontier of large language models and the AI paradigm. We don’t really think so. So here's Jev in 25 lines of Python.

Load the model.

```
# /// script
# requires-python = ">=3.12"
# dependencies = ["huggingface-hub", "llama-cpp-python", "numpy"]
# ///

import numpy
from llama_cpp import Llama

# Really, you can use any GGUF model from https://huggingface.co/models?library=gguf

model = Llama.from_pretrained(
    repo_id="Qwen/Qwen3-0.6B-GGUF",
    filename="Qwen3-0.6B-Q8_0.gguf",
    n_ctx=512,
    logits_all=True,
    verbose=False,
)
```

Load the prompt and define your choices.

```
labels = ["A", "B", "C"]
choices = ["Legitimate", "Spam", "Phishing"]
email = "Payroll asks for your password on a non-company sign-in page."
options = "\n".join(
    f"{label}. {choice}" for label, choice in zip(labels, choices, strict=True)
)
prompt = f"""<|im_start|>system
Choose one option.<|im_end|>
<|im_start|>user
Email: {email}\n\n{options}<|im_end|>
<|im_start|>assistant
<think>\n\n</think>\n\n"""
model.eval(tokens=model.tokenize(text=prompt.encode(), add_bos=False, special=True))
```

Massage the logits into probabilities.

```
logits = model.scores[model.n_tokens - 1]
token_ids = [model.tokenize(text=label.encode(), add_bos=False)[0] for label in labels]
choice_logits = numpy.asarray([logits[token_id] for token_id in token_ids])
logprobs = choice_logits - numpy.logaddexp.reduce(choice_logits)
probabilities = numpy.exp(logprobs)

for name, scores in (
    ("Logits", choice_logits),
    ("Log probabilities", logprobs),
    ("Probabilities", probabilities),
):
    values = numpy.round(scores.astype(float), 3).tolist()
    print(f"{name}:", dict(zip(choices, values, strict=True)))

# Logits: {'Legitimate': 26.254, 'Spam': 27.262, 'Phishing': 29.614}
# Log probabilities: {'Legitimate': -3.482, 'Spam': -2.474, 'Phishing': -0.122}
# Probabilities: {'Legitimate': 0.031, 'Spam': 0.084, 'Phishing': 0.885}
```

There. That’s Jev.

## But no, you don’t understand Jev!

Yeah, we know.

- We don't call it a [System One decision model](https://typesafe.ai/blog/introducing-system-one-models-and-jev).
- We didn’t call an API.
- We didn't create a bunch of synthetic data.
- We didn't train a model with [Reinforcement Learning for Calibrated Decisions (RLCD)](https://typesafe.ai/blog/introducing-system-one-models-and-jev) to calibrate the decisions and probabilities (even though they are [not always correct](https://arcturus-labs.com/blog/2026/09/16/typesafes-jev-trades-text-generation-for-instant-calibrated-decisions/)).

## But yes. This is Jev.

- It classifies: it gets a prompt with choices and outputs probabilities.
- It's fast.
- It's local.
- You don't send your data anywhere else.

And we like not sending your data anywhere else. Check out [NobodyWho](https://github.com/nobodywho-ooo/nobodywho).

*(note: this is a parody blog post, see these links for better/more complete open implementations of Jev: [OpenJev](https://openjev.com/), [openjev-sglang](https://github.com/ekzhang/openjev-sglang), and [OpenJev on DiffusionGemma](https://github.com/razorback16/openjev).)*

* * *

Everything NobodyWho do is open-source, please leave a [star on Github](https://github.com/nobodywho-ooo/nobodywho) to support us ❤️

Published Sep 22, 2026 by Duarte O.Carmo

 Technical
