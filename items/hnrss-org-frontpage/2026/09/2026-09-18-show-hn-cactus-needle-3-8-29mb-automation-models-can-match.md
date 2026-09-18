---
title: 'Show HN: Cactus Needle 3: 8-29MB automation models can match DeepSeek V4 Flash'
link: https://cactuscompute.com/needle
source: hnrss-org-frontpage
published: 2026-09-18T00:11:44Z
updated: 2026-09-18T00:11:44Z
first_seen: 2026-09-18T21:49:41.060765696Z
authors:
- HenryNdubuaku
summary: 'Hey HN, Henry from Cactus here. We submitted Needle 2 here a few weeks ago, and the feedback in the discussion thread was incredibly valuable, thanks! Thanks to all that feedback, we’ve been able to move quickly to release Needle 3 and I''d love to hear what you think again. The key features: 1) Automation (tool calls & structured JSON output): Needle still doesn''t chat by design, its quite challenging to pack general capacity into such small models, so we focus on tool calls and structured JSON. If no tool you declared fits the request, you get an empty list back (note for when playing with the demo). 2) Intelligence Laddering: Every layer (2 to 20) is a deployable subnetwork, so one set of weights, 25 to 121 million parameters at 2-bit, shipping as 8-29MB binaries. On a Raspberry Pi 5 it decodes at up to 4k tokens/sec and prefills at up to 10k. 3) Monarch Hadamard MLP: replaces the dense FFN with three learnable Walsh-Hadamard-initialized Kronecker (Monarch) factor pairs interleaved with per-channel diagonal scales, fixed permutations, a SiLU nonlinearity, and a rank-8 input-conditioned gate, so each token gets a fully mixed nonlinear transform of its d_model channels at O(d√d) parameters and compute instead of the O(d²) a dense 4x-expansion MLP would cost. 4) Performance: On Mobile Actions (phone commands, scored on the exact call) the 20-layer model gets 86.0 through the shipped 2-bit binary; LFM2.5 1.2B is at 82.4, Qwen3.5 0.8B at 76.0, Apple''s on-device model at 57.6, all at f16. More results on the link, we do not win everywhere ofc. 5) Multilingual: Needle 3 now supports English, French, Spanish, German, Dutch, Italian, Polish, with more languages coming. 6) Finetuning: You can achieve DeepSeek v4 Flash grade performance on a narrow task with just 4L, stress on "narrow task", we found that production users often prefer tuning before production. 7) Triggers: Grounding is a common challenge for tool call, at least for Needle 2, so we added support case-insensitive regular expressions matched against each request to gate false negatives. 8) Confidence: Every response also carries a calibrated confidence score, the minimum of a judgement on the finished call and its decode probability. Act above your threshold, show the call and ask below it, or escalate to a bigger model. 9) Supported Platforms: macOS, Linux on x86-64, ARM64, ARMv7, RISC-V and MIPS32, Windows x64 and ARM, Android, iOS, watchOS, tvOS, the browser as WebAssembly, and a WASI component. Thanks for reading and as always, thoughts appreciated! Comments URL: https://news.ycombinator.com/item?id=49748553 Points: 130 # Comments: 69'
content: extracted
html: 2026-09-18-show-hn-cactus-needle-3-8-29mb-automation-models-can-match.html
preview:
  file: 2026-09-18-show-hn-cactus-needle-3-8-29mb-automation-models-can-match.preview-43735514907a.webp
  width: 256
  height: 134
  color: '#040404'
images:
- source: https://cactuscompute.com/opengraph-image?9df76a35fe8276e2
  original:
    file: 2026-09-18-show-hn-cactus-needle-3-8-29mb-automation-models-can-match.image-3b99c6702985.png
    width: 1200
    height: 630
  variants:
  - file: 2026-09-18-show-hn-cactus-needle-3-8-29mb-automation-models-can-match.image-6aeff60c1758.webp
    width: 320
    height: 168
  - file: 2026-09-18-show-hn-cactus-needle-3-8-29mb-automation-models-can-match.image-ca96370dec84.webp
    width: 640
    height: 336
  - file: 2026-09-18-show-hn-cactus-needle-3-8-29mb-automation-models-can-match.image-f9ca91299049.webp
    width: 960
    height: 504
  - file: 2026-09-18-show-hn-cactus-needle-3-8-29mb-automation-models-can-match.image-8231dc2fde7a.webp
    width: 1200
    height: 630
  color: '#000000'
---

One set of weights, every depth from 2 to 20 layers a model of its own: an intelligence ladder.

[Get started with Needle 3 customisation](https://cactuscompute.com/dashboard)

Today we release Needle 3: a foundation model for mobile, wearables, robots, smart home, automotive and microcontrollers. The whole model is a single **8-29 MB** binary built on our [Simple Attention Network](https://cactuscompute.com/blog/simple-attention-networks), and we trade general chat capacity to **beat models 10x its size** on mobile tool calls and match 2-3x bigger models on extraction.

Tool calls

Given the functions your app exposes, Needle picks the right ones and fills every argument from what the user said. Ask for two things and you get two calls in order; ask for something no tool covers and you get an empty list, not a guess.

Structured extraction

Declare a shape, hand over messy text, get typed fields back: an invoice, a booking, a notification, a form. The decode grammar guarantees the output parses. Extraction generalised well to classification problems too.

Text embedding

The same model returns a vector for a sentence, so an app can search, match and route locally: find the note you mean, pick the tool closest to a request, collapse duplicate alerts.

What that looks like in a product:

Smart home

Go from pressing buttons to talking to the house. "Dim the bedroom and lock up" becomes two calls, executed offline, with no hub round trip.

Robots

Give a vacuum or a small robot nuanced instructions: "clean the kitchen but leave the bedroom", "go back to the dock when you are done". Each becomes a sequence of moves it can execute.

Phones

An assistant that acts on the device instead of answering: make an album from last weekend's photos, open a site, dim the screen, find the lease in your files.

Wearables

Read a notification into structured data on the wrist: a card charge into merchant, amount and date; a message into a reply; a complaint into a sentiment flag.

AR glasses

Navigation and nearby search from a short request, with no phone or network in the loop.

Automotive

Climate, media, navigation and calls from requests in the cabin, with the tool set pinned so it survives a long drive's worth of conversation.

Computers

Plain-English control of the machine in front of you: draft the mail, start the timer, copy the address, open the tab.

Search and matching

Embeddings that never leave the device: semantic search over notes, messages and documents; a query matched to the closest of hundreds of tools; near-duplicate alerts merged on a watch.

## Model

[Intelligence laddering.](https://cactuscompute.com/blog/intelligence-ladders) Every layer of Needle 3 is a sub-network with monotonically increasing capacity. Developers can choose the right size from the 2-layer (2L) subnetwork to 20 layers (20L). Each subnetwork is amenable to fine-tuning, such that 4L can match DeepSeek V4 Flash when tuned on downstream tasks for one epoch. Intelligence laddering produces 9 to 29 MB CQ2-bit binaries and supports a wide range of tiny devices.

Inputs

Text prompts, plus tool definitions or an extraction schema

Outputs

Structured JSON with tool calls or extractions

Model

29-121M Laddered Simple Attention Networks, CQ2 quantisation

Training

360B tokens of proprietary structured dataset

Speed

400-4k tokens/s decode and 1-10k tokens/s prefill on a Raspberry Pi 5

## Get started

Install the Python package. The inference engine is fetched once from Hugging Face and cached; there is nothing else to build.

Needle reads your tool descriptions to decide what to call and how to fill arguments, so describing them well is the whole game.

**Simple**: decorate a function. The signature gives the argument types, the docstring is the tool description, and `run()` completes the loop: the model picks the call, Needle executes your function, feeds the result back, and returns the final response with the executed tool results attached as `results`.

**Route by pattern**: when a description cannot enumerate every phrasing, give a tool `triggers`, regular expressions matched against each request. A match restricts the decode to the matched tools and requires a call, so the request reaches the tool you named instead of being refused or misrouted, and the call ships even below the confidence floor. A match restricts the whole turn, so a catch-all should exclude the nouns other tools own, e.g. `^(?![\s\S]*\b(lights?|doors?)\b)[\s\S]*\b(turn|switch)\b[\s\S]*\b(on|off)\b`; then "switch the fan on and dim the kitchen lights" still reaches both tools.

**Extraction**: to pull structured data out of text, declare the shape and call `extract()`. Pass a Pydantic model and you get a typed object back.

Every turn returns one JSON object:

**Confidence gating and routing**: every response carries a `confidence` score from a calibrated head, and the engine already applies a floor of 0.1. Below it, the call is withheld into `suppressed_calls` and `function_calls` is empty. Above it, the score is yours to route on: act at once when it is high, show the call and ask when it is middling, and treat an empty result as a refusal. A tool with `triggers` always produces a call for a matching request, so the score is what tells you whether to run it or confirm it.

**Writing tools**: the model reads a schema literally, so a narrow tool with a plain description beats a broad one. One tool per action, described by the actions it covers ("Turn a room's lights on or off") rather than a category. Name enum options after what a user says (`action: ["increase", "decrease"]`) and keep synonyms in the description. Give a required argument a `default` when a request may leave it out; a required argument with no default and no evidence in the request is withheld rather than guessed. Put value formats in descriptions (`"City, ST"`, `"e.g. T-1042"`). Add `triggers` to intents that must always reach a tool, and keep the toolset per turn small, since every extra tool is a chance to misroute.

**Fine-tune**: the Python package is the quick path. LoRA on the frozen base at the full 20 layers, then a 4-bit `.cact` of any subnetwork that runs on the same engine.

## Cactus Platform

Needle was designed to be customised. Its capacity is a ladder, and a subnetwork as small as 2 layers, fine-tuned on one product's tools, runs optimally on devices far smaller than the full model needs. Constraining the capacity to a narrow, well-defined task is what lets it reach frontier-level accuracy there: fine-tuning on DroidCall lifts every subnetwork by 18 to 36 points, and from 4 layers up the tuned subnetwork passes DeepSeek V4 Flash, starting at 29M parameters (Figure 3).

The [Cactus Platform](https://cactuscompute.com/dashboard) is the full path: Cactus datasets, the 2-bit quantisation behind the shipped model, evaluation design and tracking, full-depth fine-tunes and dataset management, all on our infrastructure and training pipeline, no need to build your own.

## Deploy

Every deployment target ships a prebuilt engine under 1 MB that loads the `needle3.cact` weights at start. `needle build` fetches the engine for a platform and puts the weights beside it, at the full 20 layers or any smaller subnetwork:

| Target         | Platform folder                                                      | Ships                             |
| -------------- | -------------------------------------------------------------------- | --------------------------------- |
| macOS          | macos-arm64                                                          | needle CLI, libneedle.a, needle.h |
| Linux          | linux-x86\_64, linux-arm64, linux-armv7, linux-riscv64, linux-mipsel | needle CLI, libneedle.a, needle.h |
| Windows        | windows-x86\_64, windows-arm64                                       | needle.exe, libneedle.a, needle.h |
| Android        | android-arm64, android-armv7, android-riscv64                        | needle CLI, libneedle.a, needle.h |
| iOS            | ios-arm64, ios-sim-arm64                                             | libneedle.a, needle.h             |
| tvOS, watchOS  | tvos-arm64, watchos-arm64                                            | libneedle.a, needle.h             |
| Browser        | wasm                                                                 | needle.js, needle.wasm, needle.h  |
| WASI component | wasm-component                                                       | needle.component.wasm, needle.wit |

One folder per target; `needle build --platform` downloads it and places `needle3.cact` beside the engine.
