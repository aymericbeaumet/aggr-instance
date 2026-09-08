---
title: Kimi K3 (2.8T) at 1 token/s on a MacBook Pro, streamed from four SSDs
link: https://github.com/argonautlabsai/deltafin
source: hnrss-org-frontpage
published: 2026-09-08T20:07:55Z
updated: 2026-09-08T20:07:55Z
first_seen: 2026-09-08T22:56:52.093968329Z
authors:
- Argonautlabs
summary: 'Article URL: https://github.com/argonautlabsai/deltafin Comments URL: https://news.ycombinator.com/item?id=49616257 Points: 181 # Comments: 79'
content: extracted
html: 2026-09-08-kimi-k3-2-8t-at-1-token-s-on-a-macbook-pro-streamed-from.html
preview:
  file: 2026-09-08-kimi-k3-2-8t-at-1-token-s-on-a-macbook-pro-streamed-from.preview-473a5ad27b80.webp
  width: 256
  height: 128
  alt: 'ARGODRIVE Deltafin: Kimi K3 (2.8T MoE) streamed from SSDs on Apple Silicon — fork of gavamedia/deltafin with the ARGODRIVE storage work and benchmark package - argonautlabsai/deltafin'
  color: '#e8e7e9'
images:
- source: https://opengraph.githubassets.com/892371fbd013d4562cc02f0218bdcf26d476f31cafea93d1ad894f6bde257f7f/argonautlabsai/deltafin
  original:
    file: 2026-09-08-kimi-k3-2-8t-at-1-token-s-on-a-macbook-pro-streamed-from.image-4f01c8746a20.png
    width: 1200
    height: 600
  variants:
  - file: 2026-09-08-kimi-k3-2-8t-at-1-token-s-on-a-macbook-pro-streamed-from.image-0413d5d94095.webp
    width: 48
    height: 24
  - file: 2026-09-08-kimi-k3-2-8t-at-1-token-s-on-a-macbook-pro-streamed-from.image-ccd867f658b4.webp
    width: 320
    height: 160
  - file: 2026-09-08-kimi-k3-2-8t-at-1-token-s-on-a-macbook-pro-streamed-from.image-a3afcb420b38.webp
    width: 640
    height: 320
  - file: 2026-09-08-kimi-k3-2-8t-at-1-token-s-on-a-macbook-pro-streamed-from.image-80bae999ba27.webp
    width: 960
    height: 480
  - file: 2026-09-08-kimi-k3-2-8t-at-1-token-s-on-a-macbook-pro-streamed-from.image-ec95a6b69549.webp
    width: 1200
    height: 600
  color: '#fdfdfd'
---

**TL;DR** — Kimi K3 (2.8T-parameter MoE, 1.45 TB of expert weights) running on one M5 Max MacBook Pro with 128 GB, experts streamed from four SSDs.

- **1.00 tok/s** steady decode over a 512-token answer; 1.13 over 128; 0.96 on the public 17-token prompt (upstream reported 0.68).
- **The honest limit:** a 512-token prompt takes ~6.3 minutes to its first token. Cause found (prefill re-reads each layer's experts 8×), fix planned, not built.
- **Useful findings:** one drive gives ≈52% of four-drive speed, two ≈73%, three ≈90% — the slowest of each layer's 16 reads sets the pace, not total bandwidth.
- Every number is one cold run with the exact prompt; per-run logs and placement manifests are in [`k3-public-bench/`](https://github.com/argonautlabsai/deltafin/blob/main/k3-public-bench).
- Fork of [gavamedia/deltafin](https://github.com/gavamedia/deltafin) (MIT), who built the engine — see [`CREDITS.md`](https://github.com/argonautlabsai/deltafin/blob/main/CREDITS.md). Instruments: [ARGODRIVE](https://github.com/argonautlabsai/argodrive).

* * *

## ARGODRIVE Deltafin benchmarks — M5 Max, 128 GB, experts streamed from four SSDs

[](https://github.com/argonautlabsai/deltafin#argodrive-deltafin-benchmarks--m5-max-128-gb-experts-streamed-from-four-ssds)

Measured 2026-09-08 with this fork's configuration of record ([`k3-public-bench/env.sh`](https://github.com/argonautlabsai/deltafin/blob/main/k3-public-bench/env.sh)); every number is one cold run with the exact prompt, and the per-run logs are in [`k3-public-bench/results/`](https://github.com/argonautlabsai/deltafin/blob/main/k3-public-bench/results).

| test                                                                        | drafter off  | drafter on       |
| --------------------------------------------------------------------------- | ------------ | ---------------- |
| steady decode, 512 generated tokens                                         | 0.9232 tok/s | **1.0015 tok/s** |
| steady decode, 128 generated tokens                                         | 0.9261       | **1.1252**       |
| 17-token prompt from issue #15 (upstream reported 0.684 there), median of 3 | —            | **0.9631**       |
| time to first token, 512-token prompt                                       | ≈376 s       | ≈375 s           |

[![Decode speed by number of drives](https://github.com/argonautlabsai/deltafin/raw/main/k3-public-bench/results/charts/drive-ladder.svg)](https://github.com/argonautlabsai/deltafin/blob/main/k3-public-bench/results/charts/drive-ladder.svg)

[![Per-drive draw under the engine vs standalone ceiling](https://github.com/argonautlabsai/deltafin/raw/main/k3-public-bench/results/charts/drive-draw.svg)](https://github.com/argonautlabsai/deltafin/blob/main/k3-public-bench/results/charts/drive-draw.svg)

Drive-count ladder on the same prompts: one drive ≈52% of the four-drive speed, two full mirrors ≈73%, three ≈90% ([`results/SCALING.md`](https://github.com/argonautlabsai/deltafin/blob/main/k3-public-bench/results/SCALING.md)). Why prefill is slow and what fixes it: [`results/PREFILL.md`](https://github.com/argonautlabsai/deltafin/blob/main/k3-public-bench/results/PREFILL.md). Definitions, identity scope and precision statement: [`k3-public-bench/README.md`](https://github.com/argonautlabsai/deltafin/blob/main/k3-public-bench/README.md).

* * *

```
	____       _ _         __ _
	|  _ \  ___| | |_ __ _ / _(_)_ __
	| | | |/ _ \ | __/ _` | |_| | '_ \
	| |_| |  __/ | || (_| |  _| | | | |
	|____/ \___|_|\__\__,_|_| |_|_| |_|
```

## Run the *full, never-pruned*, 2.8-trillion-parameter [Kimi K3](https://huggingface.co/moonshotai/Kimi-K3) on consumer hardware, as "fast" as possible

[](https://github.com/argonautlabsai/deltafin#run-the-full-never-pruned-28-trillion-parameter-kimi-k3-on-consumer-hardware-as-fast-as-possible)

Deltafin is a single native binary that runs full Kimi K3. Nothing pruned. Nothing skipped. K3 decides every token.

All 16 experts, every single token. No shortcuts, no "close enough." It's exactly what Moonshot shipped.

The quality rule is simple: **K3 itself decides every token**, and nobody else. Small draft models are allowed to guess ahead (that's where much of the speed comes from), but K3 checks every guess, and nothing reaches you without its official sign-off.

### Upstream benchmarks on an M1 Max laptop (gavamedia/deltafin, unchanged — not this fork's numbers)

[](https://github.com/argonautlabsai/deltafin#upstream-benchmarks-on-an-m1-max-laptop-gavamediadeltafin-unchanged--not-this-forks-numbers)

- 0.2901 token/s (3.447 s/token) — 1.9% higher throughput than last update

#### Upstream's historical M1 benchmarks:

[](https://github.com/argonautlabsai/deltafin#upstreams-historical-m1-benchmarks)

- 0.2847 token/s (August 2, 2026) — 7.0% higher throughput
- 0.2660 token/s (July 30, 2026) — 102.9% higher throughput
- 0.1311 token/s (July 28, 2026) — 829.8% higher throughput
- 0.0141 token/s (July 27, 2026)

[![model](https://camo.githubusercontent.com/b78abff773c2f118212cb4f37162fe4bb86957446fb49a29a7f7b4e45e42ae62/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f6d6f64656c2d4b696d695f4b335f2543322542375f322e38545f4d6f452d626c756576696f6c6574)](https://camo.githubusercontent.com/b78abff773c2f118212cb4f37162fe4bb86957446fb49a29a7f7b4e45e42ae62/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f6d6f64656c2d4b696d695f4b335f2543322542375f322e38545f4d6f452d626c756576696f6c6574) [![platforms](https://camo.githubusercontent.com/197fb5d81e42386aeaac806fb05b572cbec82dec9ce4cb72d0ffc51265418af5/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f706c6174666f726d732d6d61634f535f61726d36345f2543322542375f4c696e75785f7838362d2d3634253246616172636836342d696e666f726d6174696f6e616c)](https://camo.githubusercontent.com/197fb5d81e42386aeaac806fb05b572cbec82dec9ce4cb72d0ffc51265418af5/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f706c6174666f726d732d6d61634f535f61726d36345f2543322542375f4c696e75785f7838362d2d3634253246616172636836342d696e666f726d6174696f6e616c) [![accelerators](https://camo.githubusercontent.com/5b18301eb463fe23d66f12cf031e0cdf8c1c9068731805875175246221ba6cc0/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f616363656c657261746f72732d4d50532532464d6574616c5f2543322542375f435544415f2543322542375f4350552d396366)](https://camo.githubusercontent.com/5b18301eb463fe23d66f12cf031e0cdf8c1c9068731805875175246221ba6cc0/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f616363656c657261746f72732d4d50532532464d6574616c5f2543322542375f435544415f2543322542375f4350552d396366) [![experts](https://camo.githubusercontent.com/6f8d6f1e9a12b6af4409fa1f959d8d72a869cc7bdb6de6eb8b965d736643a3b5/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f726f7574696e672d616c6c5f31365f657870657274732d7465616c)](https://camo.githubusercontent.com/6f8d6f1e9a12b6af4409fa1f959d8d72a869cc7bdb6de6eb8b965d736643a3b5/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f726f7574696e672d616c6c5f31365f657870657274732d7465616c) [![runtime](https://camo.githubusercontent.com/dcc3742377abbb37b5fe712de2af9ea0411a20b73ef8bb297e50b5ae7219e4f5/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f72756e74696d652d6f6e655f636f6d70696c65645f62696e6172792d627269676874677265656e)](https://camo.githubusercontent.com/dcc3742377abbb37b5fe712de2af9ea0411a20b73ef8bb297e50b5ae7219e4f5/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f72756e74696d652d6f6e655f636f6d70696c65645f62696e6172792d627269676874677265656e) [![license](https://camo.githubusercontent.com/b8cadaa967891081f8f165695470689986c028821dd8a040132f6e661795dc0d/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f6c6963656e73652d4d49542d626c7565)](https://camo.githubusercontent.com/b8cadaa967891081f8f165695470689986c028821dd8a040132f6e661795dc0d/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f6c6963656e73652d4d49542d626c7565)

* * *

## Mission Statement

[](https://github.com/argonautlabsai/deltafin#mission-statement)

**Pure raw uncut K3 quality, as fast as possible.** Speed must *never come from reducing model quality*. Deltafin keeps all 16 routed experts and the full K3 target as the sole authority for every single token.

Our goal is to squeeze out every last drop of efficiency possible when running a huge model like K3, with all options on the table... *except for reducing quality*.

* * *

## But Why?!?

[](https://github.com/argonautlabsai/deltafin#but-why)

Deltafin is not a product pitch. It is an experiment in how far consumer hardware can be pushed, and what we can learn by attempting something so challenging.

Kimi K3 targets infrastructure on the scale of 16 nodes and roughly 4.8 TB of aggregate VRAM. That means the full 2.8T parameters and the 1M-token context window, with the expert bank never pruned. On any home setup, this is an extreme constraint. Every 1% improvement is very hard-won. But each gain can teach something.

**Research and exploration is the point.** *That* is our mission. Not everything has to be a "minimum viable product" to impress venture capitalists. If Deltafin helps make frontier models usable on a $15,000 home setup, instead of a [$2,000,000 infrastructure](https://www.thundercompute.com/blog/nvidia-h200-pricing) like Kimi recommends, we believe that is worthwhile progress on our self-hosted AI journey. Plus everything learned along the way could even benefit other projects in unexpected ways.

> “We choose to run the full 2.8-trillion-parameter model locally, and do the other things, not because they are easy, but because they are hard.” — John F. Kennedy probably

Other projects appear to run full K3, somehow faster. But look closer: they've re-encoded K3's expert bank down to ~3 bits. Clever engineering toward a different goal: the smallest K3 that fits and is "close enough." Those weights are no longer the ones Moonshot released, and nobody, including them, has measured what those compromises cost.

Deltafin is the other experiment: every expert byte exactly as Moonshot shipped it, made as fast as physics allows.

* * *

## 1\. New Installation

[](https://github.com/argonautlabsai/deltafin#1-new-installation)

Deltafin installs almost everything it needs. See [Requirements](https://github.com/argonautlabsai/deltafin/blob/main/docs/REQUIREMENTS.md) if you're missing anything.

```
# 1. Get it
git clone https://github.com/gavamedia/deltafin.git
cd deltafin

# 2. Build it
cargo build --locked --release

# 3. Download the FULL 1.7 TB K3 model to disk (optional, but fastest)
./target/release/deltafin setup --full
```

Or, if you don't have enough disk space:

```
# 3. Stream K3 as you use it (slower, but 215 GB to start) 
./target/release/deltafin setup --stream
```

`setup --stream` installs the resident model and fetches exact experts on-demand only, initially running far more slowly when routes have no local cache yet. As you build up your cache over time, this can be a way to save space, storing only the parts of the model you use, running entirely off cache on disk.

#### Default DSpark (and optional Qwen)

[](https://github.com/argonautlabsai/deltafin#default-dspark-and-optional-qwen)

The normal setup includes [Inferact's Kimi-K3-DSpark](https://huggingface.co/Inferact/Kimi-K3-DSpark). It takes **6.635 GiB on disk** and approximately **4.49 GiB** when admitted at runtime. Deltafin avoids materializing DSpark's redundant copy of K3's embedding. Chat and server requests use DSpark automatically when beneficial; any failures, insufficient headroom, or bad live economics simply leaves full K3 running by itself.

Qwen is a separate add-on for faster raw text continuation:

```
# 4. Optionally install qwen later
./target/release/deltafin setup-qwen
```

Qwen speeds up raw completion only: the small models guess what comes next, K3 checks the guess, and you get identical output in less time. That helps code autocomplete and other `/v1/completions` traffic, plus `deltafin run --prompt ...` — one measured 17-token completion ran **2.7× faster** with the same output IDs.

This adds **4.337 GiB** on disk, and because Qwen will *not* improve chat speed, we make it an optional add-on. You can add it to a fresh install with `deltafin setup --full --include-qwen`, or add it later with the command above.

## 2\. Upgrading

[](https://github.com/argonautlabsai/deltafin#2-upgrading)

From the Deltafin folder:

```
./target/release/deltafin upgrade
```

`upgrade` gets what you need, and rebuilds the binary. Models, converted weights, and caches are left alone. It never re-runs setup or re-downloads K3.

* * *

#### NOTE: Upgrading from our old python version? Inspect it first:

[](https://github.com/argonautlabsai/deltafin#note-upgrading-from-our-old-python-version-inspect-it-first)

```
git status --short

# ⬆️ Continue only when that returns nothing

git pull --ff-only
cargo build --locked --release
./target/release/deltafin upgrade
```

Continue only when `git status --short` is empty. If it lists files, preserve or commit that work yourself, rather than allowing an upgrade procedure to guess. Existing model data remains in the same repository-root directories.

* * *

`upgrade` needs a clean, non-diverged branch, and it remembers how the binary was built, so an NVIDIA/CUDA build stays a CUDA build rather than quietly falling back to CPU. Anything unexpected safely stops the upgrade.

`upgrade` ignores build environment variables — it reuses whatever the binary was already built with. So to switch configuration (CPU to CUDA, say, or a moved LibTorch tree), run `cargo build --locked --release` yourself once with the new variables set; see [Requirements](https://github.com/argonautlabsai/deltafin/blob/main/docs/REQUIREMENTS.md). That becomes the recorded setup, and later upgrades keep it.

## 3\. Use from the command line

[](https://github.com/argonautlabsai/deltafin#3-use-from-the-command-line)

```
# Chat: apply K3's audited chat template and stop at the model's end marker.
./target/release/deltafin run --chat \
  --prompt "What are the three largest moons of Saturn?"

# Raw continuation: cap output because raw text has no chat end boundary.
./target/release/deltafin run \
  --prompt "The capital of France is" --max-new 17

# Add cumulative throughput and native transaction statistics.
./target/release/deltafin run \
  --prompt "The largest planet in our solar system is" --max-new 17 --stats
```

Without `--stats`, generated text streams normally instead of printing one diagnostic line per token. `--max-new N` limits new tokens; it does not alter the prompt or context. Chat output stops at K3's control boundary, while raw completion should normally use a bound.

Long conversations are far slower than short completions — prefill and cache grow with history, and startup prints the actual usable context bound.

## 4\. Use through an OpenAI-compatible server

[](https://github.com/argonautlabsai/deltafin#4-use-through-an-openai-compatible-server)

```
./target/release/deltafin serve --host 127.0.0.1 --port 8000
```

```
curl http://127.0.0.1:8000/v1/chat/completions \
  -H 'Content-Type: application/json' \
  -d '{"model":"deltafin-kimi-k3","stream":true,"messages":[{"role":"user","content":"Hello!"}]}'
```

The native server implements `/v1/chat/completions`, `/v1/completions` and `/v1/models`, including server-sent-event streaming. Point an OpenAI-compatible client at `http://127.0.0.1:8000/v1` and use any non-empty local API key expected by that client.

The server implements a deliberately small, strictly-checked subset of the OpenAI API — text-only, one generation at a time, always greedy and reproducible — and refuses anything it cannot honor exactly with a normal OpenAI-shaped error instead of silently ignoring it. Growing chats automatically benefit from exact conversation-state reuse, draft-verified DSpark speedups and an exact-response memo; every accepted field, refusal rule and caching detail is in [the server reference](https://github.com/argonautlabsai/deltafin/blob/main/docs/SERVER.md).

The default server response ceiling is one million tokens. Lower it with `--max-tokens N` when integrating clients, and raise client timeouts because full K3 responses are slow. Request JSON is bounded by `--max-request-bytes` (128 MiB by default). Keep the server on loopback unless you add your own authentication and network boundary.

* * *

## Documentation

[](https://github.com/argonautlabsai/deltafin#documentation)

- [How the native runtime works](https://github.com/argonautlabsai/deltafin/blob/main/docs/NATIVE-RUNTIME.md) — the one-binary in-process design: Rust core, C-ABI providers, router tracing, expert prefetch and native tokenization.
- [OpenAI-compatible server reference](https://github.com/argonautlabsai/deltafin/blob/main/docs/SERVER.md) — exactly which API fields are accepted or refused, the automatic chat speedups, and the exact-response memo.
- [Health checks](https://github.com/argonautlabsai/deltafin/blob/main/docs/HEALTH-CHECKS.md) — read-only, network-free auditors that verify the runtime and each installed component after an install, upgrade or problem.
- [Native storage preparation](https://github.com/argonautlabsai/deltafin/blob/main/docs/STORAGE.md) — the default row-int8 resident spine that setup prepares for you, selecting the original BF16 explicitly, packing either into contiguous DFSP files, and lossless scale4 expert sidecars.
- [Performance reference](https://github.com/argonautlabsai/deltafin/blob/main/docs/PERFORMANCE.md) — how to reproduce measurements with the benchmark harness, and the established M1 Max reference results.
- [Configuration](https://github.com/argonautlabsai/deltafin/blob/main/docs/CONFIGURATION.md) — the few flags and environment variables that matter, and the quality guard behind them.
- [Supported platforms](https://github.com/argonautlabsai/deltafin/blob/main/docs/PLATFORMS.md) — what each host class runs (MPS/Metal, CUDA, native CPU) and the evidence status per platform.
- [Development reference material](https://github.com/argonautlabsai/deltafin/blob/main/docs/DEVELOPMENT-REFERENCE.md) — why historical `tools/*.py` files remain in the tree as frozen reference material the native runtime never executes.

## Credits

[](https://github.com/argonautlabsai/deltafin#credits)

Deltafin exists because other people published amazing work:

- [Moonshot AI](https://huggingface.co/moonshotai/Kimi-K3) released K3's weights, architecture and readable model semantics.
- [Inferact](https://huggingface.co/Inferact/Kimi-K3-DSpark) released the unchanged K3-specific DSpark checkpoint; [TorchSpec](https://github.com/lightseekorg/TorchSpec) documents its training framework, and the [vLLM team](https://vllm.ai/blog/2026-07-27-k3) published K3 integration and recurrent/attention cache research. Deltafin's native runtime, verifier, scheduling and state transactions are its own.
- [GigaToken](https://github.com/marcelroed/gigatoken), by [Marcel Rød](https://github.com/marcelroed), inspired Deltafin's automatic stable-order parallel tokenization path for large server histories.
- [Maurice Brown (`trumb`)](https://github.com/trumb) contributed Linux, aarch64, x86 SIMD and NVIDIA findings plus DGX Spark measurements in [pull request #2](https://github.com/gavamedia/deltafin/pull/2). Deltafin retained those findings behind reviewed capability and ABI gates.
- [colibri](https://github.com/JustVugg/colibri) demonstrated aggressive MoE streaming and router-lookahead ideas. [ds4 / DwarfStar](https://github.com/antirez/ds4) provided especially clear prior art for exact expert streaming, cache ownership and correctness-first measurement.
- [Qwen](https://huggingface.co/Qwen/Qwen3-1.7B-Base) supplies the optional 0.6B/1.7B proposal-only raw-completion models.
- [flash-linear-attention](https://github.com/fla-org/flash-linear-attention), [PyTorch](https://github.com/pytorch/pytorch), [llama.cpp/ggml](https://github.com/ggml-org/llama.cpp), [tiktoken](https://github.com/openai/tiktoken) and the broader local-model community supplied essential semantics and prior art.

Exact provenance and distribution boundaries are recorded in [Third-party provenance and notices](https://github.com/argonautlabsai/deltafin/blob/main/docs/THIRD_PARTY_NOTICES.md).

## License

[](https://github.com/argonautlabsai/deltafin#license)

Deltafin's tracked project code is [MIT](https://github.com/argonautlabsai/deltafin/blob/main/LICENSE). Kimi K3 weights, the DSpark checkpoint, optional Qwen checkpoints and all other third-party material retain their upstream terms. Deltafin is an independent project with no affiliation to Moonshot AI.
