---
title: Is it agentic enough? Benchmarking open models on your own tooling
link: https://huggingface.co/blog/is-it-agentic-enough
source: huggingface-co-blog
published: 2026-06-18T00:00:00Z
updated: 2026-06-18T00:00:00Z
first_seen: 2026-09-19T21:30:23.395188495Z
content: extracted
html: 2026-06-18-is-it-agentic-enough-benchmarking-open-models-on-your-own.html
preview:
  file: 2026-06-18-is-it-agentic-enough-benchmarking-open-models-on-your-own.preview-9485ae65723d.webp
  width: 256
  height: 128
  color: '#222e35'
images:
- source: https://huggingface.co/blog/assets/is-it-agentic-enough/thumbnail.png
  original:
    file: 2026-06-18-is-it-agentic-enough-benchmarking-open-models-on-your-own.image-a6ccb613ebf6.png
    width: 1441
    height: 723
  variants:
  - file: 2026-06-18-is-it-agentic-enough-benchmarking-open-models-on-your-own.image-7159976fdde7.webp
    width: 320
    height: 161
  - file: 2026-06-18-is-it-agentic-enough-benchmarking-open-models-on-your-own.image-3ca781aeed0c.webp
    width: 640
    height: 321
  - file: 2026-06-18-is-it-agentic-enough-benchmarking-open-models-on-your-own.image-4e7f53e912ff.webp
    width: 960
    height: 482
  - file: 2026-06-18-is-it-agentic-enough-benchmarking-open-models-on-your-own.image-44e3b63934ae.webp
    width: 1280
    height: 642
  - file: 2026-06-18-is-it-agentic-enough-benchmarking-open-models-on-your-own.image-51dc93cb6d01.webp
    width: 1441
    height: 723
  color: '#0a1627'
- source: https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/is-it-agentic-enough/img_6.png
  original:
    file: 2026-06-18-is-it-agentic-enough-benchmarking-open-models-on-your-own.image-1ef199f520fe.png
    width: 1386
    height: 602
  variants:
  - file: 2026-06-18-is-it-agentic-enough-benchmarking-open-models-on-your-own.image-ca554bf61171.webp
    width: 320
    height: 139
  - file: 2026-06-18-is-it-agentic-enough-benchmarking-open-models-on-your-own.image-177d7ed639c9.webp
    width: 640
    height: 278
  - file: 2026-06-18-is-it-agentic-enough-benchmarking-open-models-on-your-own.image-c570dddfce07.webp
    width: 960
    height: 417
  - file: 2026-06-18-is-it-agentic-enough-benchmarking-open-models-on-your-own.image-08552f3cf950.webp
    width: 1280
    height: 556
  - file: 2026-06-18-is-it-agentic-enough-benchmarking-open-models-on-your-own.image-209db4078c10.webp
    width: 1386
    height: 602
  color: '#0b0f17'
- source: https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/is-it-agentic-enough/img_11.png
  original:
    file: 2026-06-18-is-it-agentic-enough-benchmarking-open-models-on-your-own.image-9ef71c3e000f.png
    width: 1132
    height: 307
  variants:
  - file: 2026-06-18-is-it-agentic-enough-benchmarking-open-models-on-your-own.image-6a1b355d496f.webp
    width: 320
    height: 87
  - file: 2026-06-18-is-it-agentic-enough-benchmarking-open-models-on-your-own.image-376486900aa4.webp
    width: 640
    height: 174
  - file: 2026-06-18-is-it-agentic-enough-benchmarking-open-models-on-your-own.image-ac1ff499425c.webp
    width: 960
    height: 260
  - file: 2026-06-18-is-it-agentic-enough-benchmarking-open-models-on-your-own.image-b1bfbacb7532.webp
    width: 1132
    height: 307
  color: '#121726'
- source: https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/is-it-agentic-enough/img_13.png
  original:
    file: 2026-06-18-is-it-agentic-enough-benchmarking-open-models-on-your-own.image-59161cbad743.png
    width: 791
    height: 441
  variants:
  - file: 2026-06-18-is-it-agentic-enough-benchmarking-open-models-on-your-own.image-f40ad22354f3.webp
    width: 320
    height: 178
  - file: 2026-06-18-is-it-agentic-enough-benchmarking-open-models-on-your-own.image-a8259895e893.webp
    width: 640
    height: 357
  - file: 2026-06-18-is-it-agentic-enough-benchmarking-open-models-on-your-own.image-b5ce7facd504.webp
    width: 791
    height: 441
  color: '#0b0f17'
- source: https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/is-it-agentic-enough/img_12.png
  original:
    file: 2026-06-18-is-it-agentic-enough-benchmarking-open-models-on-your-own.image-eaf0eafe7a5f.png
    width: 793
    height: 443
  variants:
  - file: 2026-06-18-is-it-agentic-enough-benchmarking-open-models-on-your-own.image-e6d35c66c580.webp
    width: 320
    height: 179
  - file: 2026-06-18-is-it-agentic-enough-benchmarking-open-models-on-your-own.image-a6ca20a952b7.webp
    width: 640
    height: 358
  - file: 2026-06-18-is-it-agentic-enough-benchmarking-open-models-on-your-own.image-113ba6b107ce.webp
    width: 793
    height: 443
  color: '#0b0f17'
- source: https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/is-it-agentic-enough/img_14.png
  original:
    file: 2026-06-18-is-it-agentic-enough-benchmarking-open-models-on-your-own.image-df937b609c12.png
    width: 963
    height: 483
  variants:
  - file: 2026-06-18-is-it-agentic-enough-benchmarking-open-models-on-your-own.image-3378810e0759.webp
    width: 320
    height: 160
  - file: 2026-06-18-is-it-agentic-enough-benchmarking-open-models-on-your-own.image-7aab2b37a433.webp
    width: 640
    height: 321
  - file: 2026-06-18-is-it-agentic-enough-benchmarking-open-models-on-your-own.image-2c222a07fffb.webp
    width: 963
    height: 483
  color: '#0b0f17'
- source: https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/is-it-agentic-enough/img_15.png
  original:
    file: 2026-06-18-is-it-agentic-enough-benchmarking-open-models-on-your-own.image-20eb79ab96c4.png
    width: 955
    height: 489
  variants:
  - file: 2026-06-18-is-it-agentic-enough-benchmarking-open-models-on-your-own.image-65df8a7a0758.webp
    width: 320
    height: 164
  - file: 2026-06-18-is-it-agentic-enough-benchmarking-open-models-on-your-own.image-c0d8218e7501.webp
    width: 640
    height: 328
  - file: 2026-06-18-is-it-agentic-enough-benchmarking-open-models-on-your-own.image-1f75830593aa.webp
    width: 955
    height: 489
  color: '#0b0f17'
- source: https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/is-it-agentic-enough/img_16.png
  original:
    file: 2026-06-18-is-it-agentic-enough-benchmarking-open-models-on-your-own.image-a8057a2b7053.png
    width: 951
    height: 496
  variants:
  - file: 2026-06-18-is-it-agentic-enough-benchmarking-open-models-on-your-own.image-e9096ca4997d.webp
    width: 320
    height: 167
  - file: 2026-06-18-is-it-agentic-enough-benchmarking-open-models-on-your-own.image-41337b5e77e1.webp
    width: 640
    height: 334
  - file: 2026-06-18-is-it-agentic-enough-benchmarking-open-models-on-your-own.image-ad10ed14ecac.webp
    width: 951
    height: 496
  color: '#0b0f17'
- source: https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/is-it-agentic-enough/img_5.png
  original:
    file: 2026-06-18-is-it-agentic-enough-benchmarking-open-models-on-your-own.image-4e5b5cd308d9.png
    width: 1393
    height: 578
  variants:
  - file: 2026-06-18-is-it-agentic-enough-benchmarking-open-models-on-your-own.image-5397dbe1b77f.webp
    width: 320
    height: 133
  - file: 2026-06-18-is-it-agentic-enough-benchmarking-open-models-on-your-own.image-7f72226e31aa.webp
    width: 640
    height: 266
  - file: 2026-06-18-is-it-agentic-enough-benchmarking-open-models-on-your-own.image-2b1aa1133063.webp
    width: 960
    height: 398
  - file: 2026-06-18-is-it-agentic-enough-benchmarking-open-models-on-your-own.image-dbfa175fd9c9.webp
    width: 1280
    height: 531
  - file: 2026-06-18-is-it-agentic-enough-benchmarking-open-models-on-your-own.image-d5507c00cb18.webp
    width: 1393
    height: 578
  color: '#0b0f17'
- source: https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/is-it-agentic-enough/img_7.png
  original:
    file: 2026-06-18-is-it-agentic-enough-benchmarking-open-models-on-your-own.image-3a6764ca5569.png
    width: 1403
    height: 585
  variants:
  - file: 2026-06-18-is-it-agentic-enough-benchmarking-open-models-on-your-own.image-9e62e8b48cb7.webp
    width: 320
    height: 133
  - file: 2026-06-18-is-it-agentic-enough-benchmarking-open-models-on-your-own.image-d865bf6f7d90.webp
    width: 640
    height: 267
  - file: 2026-06-18-is-it-agentic-enough-benchmarking-open-models-on-your-own.image-4e1833a7bcce.webp
    width: 960
    height: 400
  - file: 2026-06-18-is-it-agentic-enough-benchmarking-open-models-on-your-own.image-88181cbae07b.webp
    width: 1280
    height: 534
  - file: 2026-06-18-is-it-agentic-enough-benchmarking-open-models-on-your-own.image-603b6fc71ec3.webp
    width: 1403
    height: 585
  color: '#0b0f17'
- source: https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/is-it-agentic-enough/img_17.png
  original:
    file: 2026-06-18-is-it-agentic-enough-benchmarking-open-models-on-your-own.image-b2b8932be9ba.png
    width: 743
    height: 323
  variants:
  - file: 2026-06-18-is-it-agentic-enough-benchmarking-open-models-on-your-own.image-1f7113842f77.webp
    width: 320
    height: 139
  - file: 2026-06-18-is-it-agentic-enough-benchmarking-open-models-on-your-own.image-cb4f929c5384.webp
    width: 640
    height: 278
  - file: 2026-06-18-is-it-agentic-enough-benchmarking-open-models-on-your-own.image-a46a8577d74d.webp
    width: 743
    height: 323
  color: '#0b0f17'
- source: https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/is-it-agentic-enough/img_10.png
  original:
    file: 2026-06-18-is-it-agentic-enough-benchmarking-open-models-on-your-own.image-3a8894fe8350.png
    width: 1673
    height: 488
  variants:
  - file: 2026-06-18-is-it-agentic-enough-benchmarking-open-models-on-your-own.image-29f9b16d797b.webp
    width: 320
    height: 93
  - file: 2026-06-18-is-it-agentic-enough-benchmarking-open-models-on-your-own.image-a76fe9d3532c.webp
    width: 640
    height: 187
  - file: 2026-06-18-is-it-agentic-enough-benchmarking-open-models-on-your-own.image-2f0fd7bfd76b.webp
    width: 960
    height: 280
  - file: 2026-06-18-is-it-agentic-enough-benchmarking-open-models-on-your-own.image-1ed74c93c94c.webp
    width: 1280
    height: 373
  - file: 2026-06-18-is-it-agentic-enough-benchmarking-open-models-on-your-own.image-0b98b3457017.webp
    width: 1600
    height: 467
  - file: 2026-06-18-is-it-agentic-enough-benchmarking-open-models-on-your-own.image-cb3a9347cacd.webp
    width: 1673
    height: 488
  color: '#131826'
---

[![Benchmarking transformers revisions across different metrics](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/is-it-agentic-enough/img_6.png)](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/is-it-agentic-enough/img_6.png)

*Benchmarking transformers revisions across different metrics*

\

> This is a human-made, agent-focused blogpost.

Coding agents increasingly work with our software instead of us: describe a task, and the agent picks the library, writes the calls, runs them, and debugs its own mistakes. When the library gets in the way, it will happily bypass it and rewrite the logic from scratch. This introduces a new concept in library development: the code should not only be correct and fast, but should be designed so that an agent can drive it effectively. A clunky API or stale docs annoy us developers, but it now also sends the agent down a longer, more expensive path.

Most benchmarks just look at the final answer. We wanted the whole process instead: not just whether the agent got it right, but how much work it took to get there, and how that shifts across models, library revisions, and tasks. We measured exactly that, using `transformers` as our case study.

Here, we will introduce a tool specific benchmark focusing on how the answer was found, and provide a simple implementation of one such harness, running entirely on open models driven by the [pi](https://www.npmjs.com/package/@mariozechner/pi-coding-agent) coding agent, with the full sweep of models × revisions × tasks fanned out across [Hugging Face Jobs](https://huggingface.co/docs/huggingface_hub/guides/jobs) so every run sees identical hardware.

But, ***how do you optimize software for agents?***

We're strong believers in the following two software principles:

- If it isn't tested, then it doesn't work
- If it isn't documented, then it doesn't exist

This remains the same within the realm of agentic-optimized tooling, and, for once, the two are directly tied to each other.

You want your tool to exist for an agent: it needs to be discoverable. The API needs to be clear and the docs need to be extensive. They need to be structured in a way that the agent has rapid access to the useful files and examples. If you want your tool to work for an agent, then you should test it for agentic-use.

## [](https://huggingface.co/blog/is-it-agentic-enough#testing-software-for-agentic-use) Testing software for agentic-use

We'll use `transformers` as an example throughout this blogpost: agents *using* it to solve ML tasks (classifying text, captioning images, transcribing audio), not contributing code to it; though the harness was designed to work with any tool that can be operated from the command line.

Our intuition on `transformers` was that usage could be dramatically simplified with a few changes: a CLI, a Skill, and self-contained, task-specific examples. This is the same recipe recently applied to the [`hf` CLI, redesigned to be agent-optimized](https://huggingface.co/blog/hf-cli-for-agents), where agents used 1.3–1.8× (and up to 6×) fewer tokens. We wanted to know whether that kind of win generalizes, and whether it could be useful for transformers as well.

Intuition is a powerful tool, but we wanted more evidence before we opened PRs that add several thousand lines of code to such a widely used codebase as `transformers`. We set out to measure what success looks like.

### [](https://huggingface.co/blog/is-it-agentic-enough#not-all-successes-are-equal) Not all successes are equal

Two agents can both produce the correct label for a sentiment-classification task, but one:

- writes a 40-line Python script, imports `transformers`, debugs a shape error, re-runs twice, and finally prints the answer;

while the other

- types `transformers classify --model ... --text "..."` and is done in one call.

Both reach `POSITIVE (0.9999)`, and here are the two paths an agent actually took on this exact task:

```
# Task: classify the sentiment of "I absolutely loved the movie, it was fantastic!"

- # one agent: pipe a script into python and parse the output
- python - <<'PY'
- from transformers import AutoTokenizer, AutoModelForSequenceClassification
- import torch
- import torch.nn.functional as F
-
- model = AutoModelForSequenceClassification.from_pretrained("distilbert/distilbert-base-uncased-finetuned-sst-2-english")
- tokenizer = AutoTokenizer.from_pretrained("distilbert/distilbert-base-uncased-finetuned-sst-2-english")
- inputs = tokenizer("I absolutely loved the movie, it was fantastic!", return_tensors="pt")
- with torch.no_grad():
-     logits = model(**inputs).logits
- probs = F.softmax(logits, dim=1)
- idx = torch.argmax(probs, dim=1).item()
- print(model.config.id2label[idx], probs[0][idx].item())
- PY

+ # the other agent: one command
+ transformers classify \
+   --model distilbert/distilbert-base-uncased-finetuned-sst-2-english \
+   --text "I absolutely loved the movie, it was fantastic!"
```

Both methods reach the same result. But they have very different profiles in **cost, latency, token usage, and failures**.

If your evaluation only checks the final string, you're blind to these as well as whether a change you shipped to the library (a CLI improvement, better error messages, a Skill) actually helped agents.

Our goal with this harness is to evaluate how much work an agent has to do to perform a given task, and whether changes to the library improve performance.

### [](https://huggingface.co/blog/is-it-agentic-enough#how-do-we-run-evaluations) How do we run evaluations?

A few words on how we'll evaluate agents here.

We run every task under three variants (or "tiers"); three different ways an agent can come at `transformers`:

```
bare     pip install transformers, and nothing else
clone    the full transformers source, checked out in the working directory
skill    a packaged Skill: the CLI's docs + task examples, loaded in context
```

These aren't nested: `skill` doesn't contain `clone` (it ships curated docs, not the source tree), and neither strictly contains the other, each gives the agent a different kind of help. As we'll see, a model can sometimes do better on `clone` than on `skill`.

A few more choices:

- For now we only focus on deterministic tasks which can provide an exact match, as they provide a very nice ground for experimentation. Model-as-a-judge and other schemes are the obvious next steps for other tasks.
- Every run is its own Hugging Face Job: one per (model × revision × task), so the whole sweep runs in parallel on identical hardware, which keeps the comparison fair at scale.
- Results and traces land in a Hugging Face Bucket: fast, no versioning needed, and handles very high write concurrency.

### [](https://huggingface.co/blog/is-it-agentic-enough#which-models-to-benchmark-against) Which models to benchmark against?

Not all models driving agents are equal, and their difference changes what you should look at when running them.

*Large open models*

At one end, you have the largest, most capable open models. On reasonably common tasks, these should get the right answer, eventually. For them, task completion saturates near 100% and stops telling you much about your tool; a more relevant benchmark is the effort it took the agent to get there: how many turns, tokens and seconds it took, and whether they walked a clean path or used deprecated APIs.

*Local*

Local models vary widely in size, and so do their abilities. Metrics such as **"match %"** are more relevant than for their larger counterparts, as you can see how model sizes/capabilities affect results on your specific tool.

This harness not only provides guidance to library maintainers on how to improve a repository for agent interactions, it also helps assess how different agents and models perform on the tasks users care about.

The harness scores every run on several axes, so that you can ask what actually matters for each class of model:

- **match %**: did the final answer contain the expected result (per-task, case-insensitive substring / regex / exact, all explicit in the report);
- **median time** and **median tokens** (new vs. cached vs. generated);
- **runs with error %**: including a guard that flags runs which produced *nothing* (0 output tokens, no tool calls, no answer) so silent failures don't masquerade as "0";
- **marker adoption**: tool-defined behavior markers; see below for an explanation of what this is.

All of it lands in a report you can directly examine:

\
 *The live report: Overview, Coverage, and Results, all client-side.*

And because it captures the native agent trace of every run, numbers are just the beginning: you can read exactly what the agent did, command by command. The traces are shareable through the Hub's [agent-traces viewer](https://huggingface.co/docs/hub/agent-traces):

![A run rendered in the Hub's agent-traces viewer: MiniMax-M2.7 on the answer-question task](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/is-it-agentic-enough/img_11.png)\
 *A run rendered in the Hub's agent-traces viewer: MiniMax-M2.7 on the answer-question task.*\
 [**Open this trace on the Hub ↗**](https://huggingface.co/buckets/lysandre/transformers-agentic-use/tree/traces/22404f7951/pi/MiniMaxAI--MiniMax-M2.7/bare__answer-question__run1.jsonl)

Before the results, a quick recap of the setup. Each run varies four things: the **model** driving the agent, the **`transformers` revision** it runs against, the **task**, and the **tier** (`bare` / `clone` / `skill`). As discussed, we look at different metrics for the two different model categories.

### [](https://huggingface.co/blog/is-it-agentic-enough#large-open-models-hold-the-model-vary-the-revision) Large open models: hold the model, vary the revision

Since a large open model will usually get to the correct result, what you're really measuring is the effort it took to do so. Did it take ten turns or one? Did it follow an API path you deprecated because it trusted obsolete documentation? Did it hit an error you hadn't foreseen?

The natural experiment is to fix one strong model and vary the tool's revisions: the successive git versions of `transformers` we test against, from released tags like `v5.8.0` and `v5.9.0` to the specific commit that introduces the CLI and Skill. We want to watch whether the load it puts on the agent goes up or down. We used the harness on `transformers` to check whether adding a dedicated CLI and Skill actually lightened the agents' work.

For the three large models we used in our tests, the average time spent on all tasks indicates that the Skill commit results in less time spent working on the tasks:

![Median time per revision, by tier](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/is-it-agentic-enough/img_13.png)\
 *Median time per revision, by tier: the skill commit (green dot) is the fastest.*

On the other hand, in the experiments in which we cloned the repository, we can see a significant increase in token consumption due to the commit that introduced the CLI and examples, as we'll see in a moment.

![Median new tokens per revision, by tier](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/is-it-agentic-enough/img_12.png)\
 *Median new tokens per revision, by tier: the clone variant jumps once the CLI lands in the repo.*

Reading the clone-variant traces explains why. The commit adds a command, but it also ships the CLI's implementation and a set of `cli/agentic/*.py` usage examples into the repository directly.

On the `clone` variant the agent has a full transformers checkout in front of it, and roughly a third of the runs go read the new surface (the `/cli/` tree and the example scripts) to learn the interface before calling it. This raises the median input from ~4k to ~6.4k tokens.

The two charts are then two sides of one tradeoff: the commit buys the large models less time (they reach for the CLI instead of debugging Python) at the cost of more tokens (they read the code that taught them the CLI). A tradeoff worth knowing about before merging PRs.

One caveat works in the CLI's favor, though, which isn't benchmarked yet: the cost of reading it is amortized with successive runs. Our setup is built for one-off experiments. Each run is a fresh agent that rediscovers the CLI from scratch, so it pays the discovery cost every time. In real usage an agent learns the interface once and then solves task after task within the same session, amortizing that cost across many requests. The token bump we measure here is closer to a worst case than to what a user would see day to day.

### [](https://huggingface.co/blog/is-it-agentic-enough#small-models-hold-the-revision-vary-the-model) Small models: hold the revision, vary the model

Open models give us fine-grained control over the variables that matter most here: size, configuration, quantization, provider, training, and anything that would differ from one model to the next. They're also where a good tool surface matters most: a small model asked to "use `transformers` to do X" on a `bare` environment can guess an API that changed some releases ago, may do unnecessary tool calls, and can get the wrong answer.

So here the experiment is the opposite of the above: hold the revision and sweep the model. This helps see which models actually take care of the task, not just by token count and time, but down to which ones can't reliably handle the tool calls. Our intuition is that the smaller the model, the harder both tool use and the task get; we ran the harness across a range of model sizes to test exactly that:

![Match % across models, by tier](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/is-it-agentic-enough/img_14.png)\
 *Match % across models, by tier: the skill tier lifts the larger models but drops the smaller ones.*

which also seems to be correlated with the number of tokens ingested

![Median new tokens across models, by tier](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/is-it-agentic-enough/img_15.png)\
 *Median new tokens across models, by tier.*

> A note on fair comparison: naively averaging across tasks is misleading when coverage is uneven (a model that only finished the quick tasks looks fast). The report has a **"shared tasks only"** toggle (across models and/or revisions) so you compare like-for-like, and a **Coverage** heatmap so you can see exactly which task × revision × model cells actually ran.

## [](https://huggingface.co/blog/is-it-agentic-enough#tweaking-the-tool-markers-and-results) Tweaking the tool: markers and results

Two things come together here: how to look past whether the agent succeeded to what it did and how it did it; as well as the first results we pulled out of the harness.

### [](https://huggingface.co/blog/is-it-agentic-enough#whats-a-marker) What's a marker?

Match %, tokens, and time tell you the cost of a run but don't tell you much about what happened under the hood.

This is why we've introduced the concept of markers. A marker is a named pattern the profile (the small per-tool plugin that teaches the harness how to build and drive a given library) matches against a run.

It is a one-line label for a behavior you care about, checked against the shell commands the agent ran, the code it wrote, the files it read, or its final answer. A run can fire several markers or none; the report shows how often each one fired, per model and per revision.

For `transformers` we declare a handful but we'll only look at the two most relevant ones:

- **`cli`**: the agent invoked the `transformers` command-line tool (e.g. `transformers classify …`) instead of writing Python.
- **`pipeline`**: it reached for the high-level `pipeline(...)` Python API.

These are what we watch to see whether a change actually shifted the agent's behavior. Interestingly here, the larger the model, the more it leverages the new context instead of using its memory; therefore leveraging the newly introduced CLI.

![CLI adoption by tier across models](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/is-it-agentic-enough/img_16.png)\
 *CLI adoption by tier across models: only the skill tier reaches for it, and more so as models grow.*

CLI adoption is new: the CLI lands in a single commit, isn't in any model's training data, and is only lightly documented. The effect is clear: it's the Skill variant, the one that ships the CLI's documentation, that actually reaches for it, at 55.3%.

### [](https://huggingface.co/blog/is-it-agentic-enough#is-the-cli--skill-commit-helping) Is the CLI + Skill commit helping?

Comparing the commit across model sizes, the CLI + Skill helps the bigger models: on the `skill` tier, Kimi and the other large agents reach for the CLI and finish in fewer turns. (On `clone` they spend *more* input tokens first, reading the new CLI code, as we saw above, so the win shows up in time and turns, not raw tokens.)

![Kimi-K2.6, GLM-5.1, and MiniMax-M2.7 across revisions](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/is-it-agentic-enough/img_5.png)\
 *Kimi-K2.6, GLM-5.1, and MiniMax-M2.7 across revisions*

But in some smaller-model settings, it appears to hurt performance. One plausible explanation is that small models lean on memorized API patterns, reproducing `pipeline(...)` snippets they've seen in their training data. The new concepts are then a larger surface for them to get wrong. You can watch this directly on the harness: lower match %, more retries, the `cli` marker barely firing. It is particularly striking on the Qwen3-4B model: the Skill barely changes its match rate yet its cost distribution is significantly affected.

Almost all of that comes from the `clone` tier. The checkout now contains the CLI's implementation and `cli/agentic/*.py` examples, and the 4B agent reads them in bulk: its median new tokens jump from ~2.4k to ~23k, with time and output skyrocketing as well, for no gain in accuracy.

![Qwen3-4B cost distributions across revisions: elapsed, new tokens, repeat tokens, out tokens](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/is-it-agentic-enough/img_7.png)\
 *Qwen3-4B across revisions. The CLI + Skill commit fans the cost distribution wide open, on the `clone` tier the agent reads the newly-shipped CLI source in bulk (~10× the new tokens), for no gain in match %. (`repeat tokens` stays flat: this setup uses no prompt caching.)*

Sometimes, though, the Skill breaks correctness outright. Reading the traces shows how, for example for Qwen3-14B: adding the Skill drops its overall match rate from 67% (bare) to 43%, and on the simplest tasks the collapse is very visible: `classify-sentiment` goes from 100% on the `clone` variant to **0%** with the Skill.

![Qwen3-14B classify-sentiment match % by tier across revisions](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/is-it-agentic-enough/img_17.png)\
 *Qwen3-14B on `classify-sentiment`, by tier: `clone` (blue) holds at 100% across revisions, but the Skill variant (green) collapses to 0% at the CLI + Skill revision.*

Looking at the traces, the model mistakes the CLI for a *tool it can call directly* (as in an agentic-harness tool, like web-search). The Skill is **not** an executable tool: it's documentation loaded into the agent's context, and the `transformers` CLI is only ever meant to be run from the shell (via `bash`); so this will not work.

Qwen3-14B reads the Skill and, in 39 of its 56 Skill runs, either emits a `transformers(command="classify", ...)` tool call (a tool that was never registered) or, finding nothing like it among its `read`/`bash`/`edit`/`write` tools, concludes it *can't* run a model and gives up. Either way, rather than fall back to the one-line `pipeline(...)` that scored 100% on the `clone` checkout, it declares the task impossible.

![Qwen3-14B gives up on classify-sentiment under the Skill variant](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/is-it-agentic-enough/img_10.png)\
 *Qwen3-14B on classify-sentiment (Skill variant): it reasons that read/bash/edit/write can't run a model, and gives up.*

This is exactly what we built the harness to catch: the same change that speeds the large models ends up breaking the small ones, which seemed a bit counterintuitive to us at first and something we'd likely have shipped as-is. The takeaway for maintainers: **agent-facing APIs should be evaluated across model sizes, because a new affordance can reduce work for strong models while adding ambiguity for smaller ones.** It also hints at a fix: rather than hand-write a Skill and check it after the fact, you could generate and validate one against the weaker models up front.

This is exactly what [Upskill](https://huggingface.co/blog/upskill) does: it turns a strong model's solution into a Skill only when it measurably helps the smaller ones.

## [](https://huggingface.co/blog/is-it-agentic-enough#trying-it-yourself) Trying it yourself

The harness is one CLI, `agent-eval`. Install it, run a suite, fan it out across models × revisions on HF Jobs, and publish the report as a Hugging Face Space.

> **Trusted local use only.** The harness runs a coding agent with bypassed permissions and executes code from whatever revision you point it at, and traces can contain prompts, output, and local paths. See [SECURITY.md](https://github.com/huggingface/is-it-agentic-enough/blob/main/SECURITY.md) before pointing it at code you didn't write or sharing results.

The full, kept-current setup and usage instructions live in the [README](https://github.com/huggingface/is-it-agentic-enough).

## [](https://huggingface.co/blog/is-it-agentic-enough#closing) Closing

Checking the final answer tells you whether an agent *can* use your library. It doesn't tell you what it costs: the turns, tokens, errors, and the path it took to get there. This harness measures that, across the revisions and models you pick.

On `transformers`, it caught something we'd have shipped on faith: the CLI + Skill helps the largest open models and hurts the smallest ones. Worth knowing before merging!

It's profile-based, and designed to be adaptable: point it at your own library, define a few tasks and their expected answers, get the same report. Code and tasks are in the [repo](https://github.com/huggingface/is-it-agentic-enough), traces are on the Hub. Let us know if you use it for your project!

## [](https://huggingface.co/blog/is-it-agentic-enough#acknowledgements) Acknowledgements

This harness stands entirely on [pi](https://www.npmjs.com/package/@mariozechner/pi-coding-agent), Mario Zechner's coding-agent CLI: it drives every open-model run, and only needs an `HF_TOKEN` to serve a model, which is what made the open-model sweep practical at all.

Thanks to the model builders and inference providers behind the models we swept. Across the board they performed well above what the `bare` baseline would suggest.
