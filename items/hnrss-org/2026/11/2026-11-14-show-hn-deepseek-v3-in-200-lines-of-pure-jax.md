---
title: 'Show HN: DeepSeek V3 in ~200 lines of pure Jax'
link: https://github.com/Vaibhaav-Tiwari/deepseek-v3-jax
source: hnrss-org
published: 2026-11-14T15:04:15Z
updated: 2026-11-14T15:04:15Z
first_seen: 2026-11-15T00:13:30.261415141Z
authors:
- VaibhaavTiwari
content: extracted
html: 2026-11-14-show-hn-deepseek-v3-in-200-lines-of-pure-jax.html
remote_preview:
  url: https://opengraph.githubassets.com/b7fc12d0073046e7c5b0ae6d199c5a77d5b953fc395164de5ba869e3ba037fe1/Vaibhaav-Tiwari/deepseek-v3-jax
  alt: 'DeepSeek V3 (MLA, DeepSeekMoE, aux-loss-free balancing, MTP) in 190 lines of pure JAX, trained on a MacBook Air. Write-up: vaibhaav.in/writing/deepseek-v3-jax - Vaibhaav-Tiwari/deepseek-v3-jax'
---

The architecture from the [DeepSeek-V3 technical report](https://arxiv.org/abs/2412.19437) in **190 lines of pure JAX**, pretrained from scratch on a MacBook Air on public-domain detective fiction.

No Flax, no Equinox, no PyTorch. Params are plain dicts of arrays and every part of the model is a function you can read top to bottom in [`deepseek_v3.py`](https://github.com/Vaibhaav-Tiwari/deepseek-v3-jax/blob/main/deepseek_v3.py).

**Read the write-up:** [DeepSeek V3 in 190 lines of pure JAX](https://vaibhaav.in/writing/deepseek-v3-jax/), with diagrams, the code explained piece by piece, and the results.

[![The whole model: 36.2M parameters, 13.6M used per token](https://github.com/Vaibhaav-Tiwari/deepseek-v3-jax/raw/main/post/diagrams/01-whole-model-clean.png)](https://github.com/Vaibhaav-Tiwari/deepseek-v3-jax/blob/main/post/diagrams/01-whole-model-clean.png)

## What's in the 190 lines

| Idea from the paper               | What it does                                                                                                      | Where                         |
| --------------------------------- | ----------------------------------------------------------------------------------------------------------------- | ----------------------------- |
| Multi-head Latent Attention (MLA) | Caches one small latent per token instead of full keys and values: **480 → 144** numbers per token per layer here | `mla`, `init_cache`           |
| Decoupled RoPE                    | One rotary key shared by all heads, kept outside the compressed latent                                            | `rope`, `apply_rope`, `mla`   |
| DeepSeekMoE                       | 1 shared expert that always runs + 16 routed experts, top-4 per token                                             | `route`, `moe`                |
| Auxiliary-loss-free balancing     | A per-expert bias nudges routing toward idle experts; it changes *which* experts are picked, not their weights    | `update_bias`, `route`        |
| Sequence-wise balance loss        | The small complementary loss from the paper (α = 1e-4)                                                            | `seq_balance_loss`            |
| Multi-token prediction (MTP)      | An extra module also predicts the token after next, weighted λ = 0.3 in the loss                                  | `forward`, `loss_fn`          |
| Training and generation           | bf16 compute on f32 master weights; prefill + decode with the MLA cache                                           | `make_train_step`, `generate` |

## Results

One run of the default config on a MacBook Air M5 (24 GB):

|                           |                                                                                              |
| ------------------------- | -------------------------------------------------------------------------------------------- |
| Model                     | 36.2M parameters, 13.6M active per token                                                     |
| Data                      | 207 books, 21.7M training tokens (Hammett repeated 3×); validation is held-out crime fiction |
| Training                  | 12,000 steps, ~98M tokens (~4.5 epochs), ~2.6 h                                              |
| Speed                     | \~10.7k tokens/s (median)                                                                    |
| Validation loss           | 5.02 (step 250) → **3.51**                                                                   |
| Expert load, with bias    | 5.7–6.9% per expert (even is 6.25%), at step 2000                                            |
| Expert load, without bias | 0.09–18.6% per expert, at step 2000                                                          |

With balancing on and off, validation loss at step 2000 was the same (3.921 vs 3.923). The bias evens out the experts without costing anything, which is the paper's claim.

[![Expert load with and without the balancing bias](https://github.com/Vaibhaav-Tiwari/deepseek-v3-jax/raw/main/post/figures/expert_load_final.png)](https://github.com/Vaibhaav-Tiwari/deepseek-v3-jax/blob/main/post/figures/expert_load_final.png)

Its own words, from the prompt "The fog came in off the":

> The fog came in off the bay, and the great trees were spreading and the stars were shining.

This is an architecture demo, not a strong language model. See the write-up for what worked, what didn't, and what's missing.

## Try the trained model

The [v1.0 release](https://github.com/Vaibhaav-Tiwari/deepseek-v3-jax/releases/tag/v1.0) has the exact checkpoint and data from the published run. After [installing](https://github.com/Vaibhaav-Tiwari/deepseek-v3-jax#reproduce-it) (step 1):

```shell
mkdir -p data runs/main
curl -L -o runs/main/ckpt.pkl      https://github.com/Vaibhaav-Tiwari/deepseek-v3-jax/releases/download/v1.0/ckpt.pkl          # 138 MB
curl -L -o data/tokenizer.json     https://github.com/Vaibhaav-Tiwari/deepseek-v3-jax/releases/download/v1.0/tokenizer.json
.venv/bin/python sample.py --prompt "The fog came in off the" --num 3
```

The checkpoint is a Python pickle, which can run code when loaded. Only load checkpoints from sources you trust.

## Reproduce it

You need an Apple Silicon Mac, Python 3.12 and [uv](https://docs.astral.sh/uv/). The published run used 24 GB of memory. No Mac? See [Running on CPU](https://github.com/Vaibhaav-Tiwari/deepseek-v3-jax#running-on-cpu).

**1\. Install**

```shell
git clone https://github.com/Vaibhaav-Tiwari/deepseek-v3-jax.git && cd deepseek-v3-jax
uv venv -p 3.12 .venv
uv pip install --python .venv/bin/python -r requirements.txt
```

Every version in `requirements.txt` is pinned. `jax-metal 0.1.1` only works with `jax==0.4.34`, so don't upgrade JAX.

**2\. Check your setup**

```shell
JAX_PLATFORMS=cpu .venv/bin/pytest tests.py -q    # 39 passed
.venv/bin/python smoke_metal.py                   # optional: what jax-metal supports on your Mac
```

In `smoke_metal.py`, "grad through 2-axis contraction" fails on jax-metal 0.1.1. That's expected, and the model is written to avoid it.

**3\. Build the dataset**

```shell
.venv/bin/python prepare_data.py
```

This downloads the 207 books in [`sources.txt`](https://github.com/Vaibhaav-Tiwari/deepseek-v3-jax/blob/main/sources.txt) (~120 MB, about 7 minutes), cleans them, trains an 8,192-token BPE tokenizer and writes `data/train.bin`, `data/val.bin` and `data/tokenizer.json`. It should print:

```
train: 211 docs, 83.4M chars, 21.69M tokens
val: 4 docs, 1.1M chars, 0.30M tokens
```

The published run's data had these hashes:

```shell
shasum -a 256 data/train.bin data/val.bin data/tokenizer.json
```

```
d37aca97e707bb81b6e4d266cc1adfd1f3e3556457c6eb724e495118a734e0bb  data/train.bin
b7ac285b115e6793b5d36aa5b4bc6e81a5fccfc7af0908a306d3828ebca9c31c  data/val.bin
c5a142b4006a967c880dda44ad792719d53c16d947da3b6c2332d6132d9967cc  data/tokenizer.json
```

You will most likely get different hashes. The Hammett books are pinned to exact Standard Ebooks commits and come out identical, but Project Gutenberg files can't be pinned, and Gutenberg has since re-issued 28 of the 203 books with small edits (mostly accents stripped, like "José" → "Jose"). The token counts come out the same, so training should behave the same.

**To train on the exact same bytes**, skip `prepare_data.py` and download the published data instead:

```shell
mkdir -p data
for f in train.bin val.bin tokenizer.json; do curl -L -o data/$f https://github.com/Vaibhaav-Tiwari/deepseek-v3-jax/releases/download/v1.0/$f; done
shasum -a 256 data/*.bin data/tokenizer.json    # should match the hashes above
```

**4\. Train**

```shell
.venv/bin/python train.py                                         # the main run: 12k steps, ~2.6 h
.venv/bin/python train.py --steps 2000 --out runs/bias_on         # balancing ablation, with the bias
.venv/bin/python train.py --steps 2000 --out runs/bias_off --no-bias   # ...and without it
```

Each run writes `metrics.jsonl` (loss, validation loss, per-expert load, biases, tokens/s) and `ckpt.pkl` (saved at every evaluation) into its `--out` folder, which defaults to `runs/main`. Seeds are fixed, but GPU math on Metal isn't bit-for-bit deterministic, so expect your numbers to land close to the ones above rather than match them exactly.

**5\. Generate**

```shell
.venv/bin/python sample.py --prompt "The fog came in off the" --num 3
```

Defaults: temperature 0.8, top-k 50, 300 new tokens per sample (`--temp`, `--top-k`, `--n`).

**6\. Make the figures**

```shell
.venv/bin/python plots.py                    # needs runs/main, runs/bias_on, runs/bias_off
JAX_PLATFORMS=cpu .venv/bin/python charts.py # the rest of the write-up's charts
```

Both write into `post/figures/`.

To redraw the figures without training, download the logs from the release: `metrics-main.jsonl`, `metrics-bias_on.jsonl` and `metrics-bias_off.jsonl` go into `runs/main/`, `runs/bias_on/` and `runs/bias_off/` as `metrics.jsonl`. `charts.py` also needs the checkpoint and the data.

### Running on CPU

Everything runs without jax-metal. It's much slower, so use the smaller config:

```shell
JAX_PLATFORMS=cpu .venv/bin/python train.py --config small
```

`--config small` (4 layers, 8 experts, top-2, 128-token context) is meant for 8–16 GB Macs too. It has only been checked for a couple of steps, not trained to the end.

## Files

| File                                                                                                                                                                                           | What it is                                                                                                                    |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| [`deepseek_v3.py`](https://github.com/Vaibhaav-Tiwari/deepseek-v3-jax/blob/main/deepseek_v3.py)                                                                                               | **The model**: config, MLA, MoE, balancing, MTP, loss, train step, cached generation. The 190 lines.                          |
| [`train.py`](https://github.com/Vaibhaav-Tiwari/deepseek-v3-jax/blob/main/train.py)                                                                                                           | Training loop: AdamW with warmup and cosine decay, evaluation, checkpoints, logging                                           |
| [`prepare_data.py`](https://github.com/Vaibhaav-Tiwari/deepseek-v3-jax/blob/main/prepare_data.py), [`sources.txt`](https://github.com/Vaibhaav-Tiwari/deepseek-v3-jax/blob/main/sources.txt) | Download, clean and tokenize the corpus                                                                                       |
| [`sample.py`](https://github.com/Vaibhaav-Tiwari/deepseek-v3-jax/blob/main/sample.py)                                                                                                         | Text generation from a checkpoint                                                                                             |
| [`tests.py`](https://github.com/Vaibhaav-Tiwari/deepseek-v3-jax/blob/main/tests.py)                                                                                                           | 39 tests: causal masking, cache vs full forward, routing, balancing, MTP targets, NaN handling, bf16 master weights, and more |
| [`smoke_metal.py`](https://github.com/Vaibhaav-Tiwari/deepseek-v3-jax/blob/main/smoke_metal.py)                                                                                               | Which JAX operations work on Apple's GPU                                                                                      |
| [`plots.py`](https://github.com/Vaibhaav-Tiwari/deepseek-v3-jax/blob/main/plots.py), [`charts.py`](https://github.com/Vaibhaav-Tiwari/deepseek-v3-jax/blob/main/charts.py)                   | The figures, all drawn from the logs, checkpoint and corpus                                                                   |
| [`post/`](https://github.com/Vaibhaav-Tiwari/deepseek-v3-jax/blob/main/post)                                                                                                                  | Diagrams, figures and screenshots from the write-up                                                                           |

"190 lines" counts the non-blank, non-comment lines of `deepseek_v3.py` after its docstring:

```shell
tail -n +5 deepseek_v3.py | grep -cvE '^\s*(#|$)'    # 190
```

## What's simplified

- **Dense MoE compute.** Every expert runs on every token and the unpicked ones are zeroed. The routing math is exact, but there's no sparse dispatch or expert parallelism.
- **No weight absorption in MLA.** The latent is decompressed explicitly, which is easier to read and slower at inference.
- **Not included:** FP8 training, DualPipe, node-limited routing, YaRN context extension, and any post-training.
- **Scale.** 36.2M parameters vs 671B, 6 layers vs 61, a 256-token context vs 128K.

## Data and licensing

All training text is public domain in the United States: Dashiell Hammett's *Red Harvest*, *The Dain Curse*, *The Maltese Falcon* and Continental Op stories (CC0 editions from [Standard Ebooks](https://standardebooks.org)), plus about 200 detective and crime novels from [Project Gutenberg](https://www.gutenberg.org). Every source is listed in [`sources.txt`](https://github.com/Vaibhaav-Tiwari/deepseek-v3-jax/blob/main/sources.txt). Public-domain status outside the US may differ.

## Reference

DeepSeek-AI, [DeepSeek-V3 Technical Report](https://arxiv.org/abs/2412.19437), 2024.

## License

Code: [MIT](https://github.com/Vaibhaav-Tiwari/deepseek-v3-jax/blob/main/LICENSE). The training text is public domain in the US (see above).
