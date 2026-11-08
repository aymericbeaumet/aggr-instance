---
title: 'Llama-modchip: 284B DeepSeek on 64GB RAM, no GPU'
link: https://github.com/synchopate/llama-modchip
source: hnrss-org
published: 2026-11-08T10:22:04Z
updated: 2026-11-08T10:22:04Z
first_seen: 2026-11-08T18:46:19.638047103Z
authors:
- joshcsimmons
content: extracted
html: 2026-11-08-llama-modchip-284b-deepseek-on-64gb-ram-no-gpu.html
remote_preview:
  url: https://opengraph.githubassets.com/46688153567a08bd5393b9328de7c1b02594ab0285ddaca6b76b436be34e6636/synchopate/llama-modchip
  alt: 🐋 Run 284B DeepSeek-V4-Flash (&lt;59GiB RAM, 44.4 tok/s pp, 9.1-14.7 tok/s) &amp; 30B MoEs (25-111 tok/s) on a pure 64GB CPU at 256K ctx (0 GPUs). SwiGLU-H2, Stockfish-PVS L3 &amp; 13ms Claude Code...
---

## ♟️🐋 llama-modchip (v2.4.0)

### Break the 64 GB RAM Wall: Run 284B DeepSeek-V4-Flash (< 59 GiB RAM) & 30B MoEs (up to 111 tok/s) at 256K Context on Pure CPU — Zero GPUs Required.

[![Release](https://camo.githubusercontent.com/c7103e9201092fe81e41c48b539af91e6a47d0dbccc69778f0cfc44641dfe0c3/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f72656c656173652d76322e342e302d3366623935303f7374796c653d666c61742d737175617265)](https://github.com/synchopate/llama-modchip/blob/main/RELEASE_NOTES.md) [![License](https://camo.githubusercontent.com/6749e5d3b258ea2632620a4473962d5dd37c50b2b2ab21783d611b5ab0a27110/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f6c6963656e73652d417061636865253230322e302d3538613666663f7374796c653d666c61742d737175617265)](https://github.com/synchopate/llama-modchip/blob/main/LICENSE) [![arXiv Preprint PDF](https://camo.githubusercontent.com/6f9db8fdd195899893170188c7e60a8d3ea7726d58f121aa1b1b06ef78ca9b8c/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f61725869762532305044462d6c6c616d612d2d6d6f64636869702d2d70617065722e7064662d6233316231623f7374796c653d666c61742d737175617265)](https://github.com/synchopate/llama-modchip/blob/main/paper/llama-modchip-paper.pdf) [![Technical Paper](https://camo.githubusercontent.com/f12a015846062e2179250983f8e2cd64f1926bbb92b6e14e72c40aba70a24973/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f70617065722d4152434849544543545552452e6d642d6263386366663f7374796c653d666c61742d737175617265)](https://github.com/synchopate/llama-modchip/blob/main/ARCHITECTURE.md) [![RouteCrack + Stockfish](https://camo.githubusercontent.com/9b478ec15eaa6e2bf7f9f64c18df7173e2f9ac901fc094b3a9f9d6590a588c79/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f647261667465722d53746f636b666973682d2d505653253230763225323028312e36344d422532304c33292d6432393932323f7374796c653d666c61742d737175617265)](https://github.com/synchopate/llama-modchip/blob/main/speculative/ROUTECRACK_TECHNIQUES.md) [![Submit Benchmark](https://camo.githubusercontent.com/71de05df17797ce7f3c92301cdc865b94d64a515b4df2e5a24c5bc660ef5354e/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f6c6561646572626f6172642d5375626d6974253230596f75722532304350552d3265613034333f7374796c653d666c61742d737175617265)](https://github.com/synchopate/llama-modchip/issues/new?template=cpu_benchmark_submission.yml)

[**⚡ Quickstart**](https://github.com/synchopate/llama-modchip#-quickstart) · [**🧩 Supported Models**](https://github.com/synchopate/llama-modchip#-supported-models--model-specific-optimizations) · [**🖥️ Supported Chips & Platforms**](https://github.com/synchopate/llama-modchip#%EF%B8%8F-supported-chips--processor-specific-optimizations) · [**🏆 Leaderboard**](https://github.com/synchopate/llama-modchip#-community-cpu--hybrid-leaderboard) · [**📄 Preprint PDF (`paper/`)**](https://github.com/synchopate/llama-modchip/blob/main/paper/llama-modchip-paper.pdf) · [**📐 Architecture**](https://github.com/synchopate/llama-modchip/blob/main/ARCHITECTURE.md) · [**❓ FAQ**](https://github.com/synchopate/llama-modchip/blob/main/FAQ.md)

**284.33B** Frontier MoE params

**56.56 GiB** checkpoint on disk

**57.61 GiB** peak VmRSS @ 256K ctx

**44.36 tok/s** 284B CPU prefill

**60.4–111.6 tok/s** 30B MoE CPU decode

**13.1 ms** Claude Code restore

**0** GPUs required

**Real-time Frontier & Agentic MoE inference on the CPU workstation you already own — 100% in RAM, zero NVMe swap:**

| the machine you have                    | RAM      | recommended model                        | speed (0 GPUs)                        | what is going on                                                                       |
| --------------------------------------- | -------- | ---------------------------------------- | ------------------------------------- | -------------------------------------------------------------------------------------- |
| a developer laptop / mini-PC            | 16–32 GB | **Nemotron-3.5-30B-A3B** (`14.35 GiB`)   | **25.0–60.4 tok/s**                   | 46 Mamba-2 SSM layers + 1.64 MB L3 Stockfish-PVS drafter; 256K ctx in 6 GiB KV         |
| a 64 GB desktop / server                | 64 GB    | **DeepSeek-V4-Flash 284B** (`56.56 GiB`) | **44.4 tok/s pp · 9.1–14.7 tok/s tg** | SwiGLU-H2 shaves 20.31 GiB; entire 284B model + 256K ctx sits in RAM (57.61 GiB VmRSS) |
| a hybrid desktop (CPU + 16 GB GPU)      | 32–64 GB | **Nemotron-30B / Qwen3-30B**             | **80.80 tok/s**                       | hot attention trunk on GPU (`-ngl 99`), cold routed MoE experts in CPU DDR5            |
| an 8-channel workstation (Threadripper) | 128 GB+  | **DeepSeek-284B / Nemotron-30B**         | **111.62 tok/s** (30B)                | 8-CCD AVX-512BW `_1x8` register tiling saturates 8-channel DDR5 memory bandwidth       |

> **TL;DR — The 3 Paradigm-Breaking Pillars of `llama-modchip`:**
>
> - 🐋 **284.33B Frontier MoE in a 64 GB CPU Box:** Shrinks `DeepSeek-V4-Flash` from `76.87 GiB` (which crashes in OOM on 64 GB Linux) down to **`56.56 GiB` on disk / `57.61–58.9 GiB` `VmRSS` at full 256K context** (**`44.36 tok/s` prefill**, **`9.06–10.74 tok/s` pure CPU decode**, **`11.5–14.7 tok/s` with PVS**) using closed-form **Analytical `SwiGLU-H2` Second-Order Hessian Compression** + **`Fractal-64` (`Q2_0`, `3.75 bpw`) MLA transcoding** + **4-Level Register (`ZMM` AVX-512BW) + L1D (`32 KB`) + L2 (`1 MB`) + L3 (`64 MB`) Whole-Model Tiling** + **$L\_2$-Quadrature MoE Pruning**.
> - ⚡ **30B MoE at GPU Speeds on Pure CPU:** Runs `NVIDIA-Nemotron-3.5-30B-A3B` in **`14.35 GiB`** at **`25.0–60.4 tok/s`** on 16-core Zen 5 (**`111.6 tok/s`** on Threadripper, **`80.8 tok/s`** on AMD Vulkan Hybrid, **`44.97 tok/s`** on AWS Graviton3 ARM64) via a **`1.64 MB` `Stockfish-PVS v2 + RouteCrack` L2/L3 Drafter (`96.4%` acceptance)** and `_MM_HINT_NTA` L3 cache isolation (`HARPCache`).
> - 🤖 **Zero-API Claude Code (`./modchip claude`):** Eliminates the 45-second CPU agent prompt re-evaluation bottleneck via **`/dev/shm` Slot-0 KV Warm Restore (`18.9 ms` on Nemotron-30B / `13.1–32.6 ms` on DeepSeek-284B)** with all 40 Claude Code tools active.

[![llama-modchip v2.4.0 — 284.33B DeepSeek-V4-Flash in 57.61 GiB RAM & 60.4 tok/s Nemotron-30B on Pure CPU](https://github.com/synchopate/llama-modchip/raw/main/assets/launch_284b_64gb.gif)](https://github.com/synchopate/llama-modchip/blob/main/assets/launch_284b_64gb.gif)

* * *

## ⚡ Quickstart

### 🐧 Linux & 🍎 macOS (1-Command Automated Setup & Chat)

```shell
git clone https://github.com/synchopate/llama-modchip.git
cd llama-modchip
./modchip chat
```

### ⚡ Zero-Compile Direct Run (Pre-Compiled Linux `x86_64` Binary + Hugging Face `.gguf`)

> **⚠️ Important Compatibility Note (`GGML_TYPE_Q2_0 = 40`):** Our **`Fractal-64`** models use custom block-64 `BF16`-scale Gray-coded tensors (`GGML_TYPE_Q2_0`, type ID `40`) and custom `deepseek4` / `Stockfish-PVS` kernels. Opening them in unpatched `llama.cpp`, Ollama, or LM Studio will fail with `unknown tensor type 40`. Use either `./modchip` above or our **standalone single-file pre-compiled binary (`llama-server-modchip-linux-x86_64`, zero `.so` dependencies)** bundled directly inside the Hugging Face repositories and [GitHub Release `v2.4.0`](https://github.com/synchopate/llama-modchip/releases/tag/v2.4.0):

```shell
# 1. For 30B Nemotron-3.5-A3B (14.35 GiB — runs in 16–32 GB RAM at 25–60.4 tok/s):
hf download Synchopate/NVIDIA-Nemotron-3.5-30B-A3B-Fractal-64-CPU-GGUF \
  llama-server-modchip-linux-x86_64 NVIDIA-Nemotron-3.5-30B-A3B-Fractal-64.gguf --local-dir .
chmod +x llama-server-modchip-linux-x86_64
./llama-server-modchip-linux-x86_64 -m NVIDIA-Nemotron-3.5-30B-A3B-Fractal-64.gguf -c 262144 -t 16 --port 8080

# 2. For 284B DeepSeek-V4-Flash (56.56 GiB — fits in 64 GB RAM at 9.1–14.7 tok/s):
hf download Synchopate/DeepSeek-V4-Flash-0731-Fractal-64GB-CPU-GGUF --local-dir ./dsv4
chmod +x ./dsv4/llama-server-modchip-linux-x86_64
./dsv4/llama-server-modchip-linux-x86_64 -m ./dsv4/DeepSeek-V4-Flash-0731-Fractal-00001-of-00003.gguf -c 32768 -t 16 -fa on --port 8080
```

### 🪟 Windows 10/11 (1-Click Hybrid GPU + CPU MoE Pack)

Open **[`windows/`](https://github.com/synchopate/llama-modchip/blob/main/windows/README.md)** and double-click **[`windows/AVVIA_MODCHIP.bat`](https://github.com/synchopate/llama-modchip/blob/main/windows/AVVIA_MODCHIP.bat)** (or run `./modchip start` in Git Bash). It automatically detects your CPU cores, free RAM, and GPU VRAM (`nvidia-smi`), downloads official `llama.cpp` CUDA 12.4 / Vulkan binaries, computes the optimal `--n-cpu-moe` split, and opens the Web Chat UI (`http://127.0.0.1:8080`).

| `./modchip` Command             | What It Does                                                                                                       |
| ------------------------------- | ------------------------------------------------------------------------------------------------------------------ |
| **`./modchip chat`**            | 💬 Interactive terminal chat with live `tok/s`, TTFT, and `Stockfish-PVS` acceptance telemetry                      |
| **`./modchip web`**             | 🌐 Opens the **Dark-Mode Web Chat & Live Telemetry HUD** (`http://127.0.0.1:8787`)                                  |
| **`./modchip bench`**           | 📊 Runs a 30-second 2-stage speed test and prints a shareable **CPU Benchmark Scorecard**                           |
| **`./modchip claude`**          | 🤖 Launches **Claude Code CLI** (`40 tools`) wired to the local 256K engine (`18.9–32.6 ms` warm restore)           |
| **`./modchip eval`**            | 🧪 Runs the reproducible **100-Point Quality, Tool-Calling, Coding & 8.5K Needle Gate**                             |
| **`./modchip quantize`**        | 🗜️ Transcodes `BF16`/`Q4_0`/`IQ2_XXS` GGUFs into **`Fractal-64`** (`3.75 bpw`) or **`Fractal-MXFP4`** (`4.25 bpw`) |
| **`./modchip status` / `stop`** | 🔍 Inspects CPU readiness, `2MB HugePages`, and `/dev/shm` IPC, or stops background services                        |

### 🏆 Can your CPU run a 284B Frontier MoE in 64 GB RAM? Test in 2 seconds (Zero Downloads):

```shell
curl -sL https://raw.githubusercontent.com/synchopate/llama-modchip/main/scripts/check_cpu_readiness.py | python3
```

👉 **Paste your terminal scorecard in a [📊 1-Click GitHub Benchmark Issue](https://github.com/synchopate/llama-modchip/issues/new?template=cpu_benchmark_submission.yml) or [GitHub Discussions](https://github.com/synchopate/llama-modchip/discussions) to be added to the Official Community CPU Leaderboard!**

* * *

## 🧩 Supported Models & Model-Specific Optimizations

Each model family has a dedicated deep-dive optimization guide with exact mathematical proofs, operator-level latency breakdowns, and quality gate benchmarks:

| Model Family                                                            | Total / Active Params                                   | Disk (`GGUF`) & `256K` RAM (`VmRSS`)                                      | Key Optimizations Implemented                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  | Verified Speed (`16C Zen 5` / Hybrid)                                                                                                                                                 | Detailed Guide                                                                                                      |
| ----------------------------------------------------------------------- | ------------------------------------------------------- | ------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------- |
| **🚀 `NVIDIA-Nemotron-3.5-30B-A3B`** *(`Fractal-64` / `MXFP4` / `Q4_0`)* | `30.2B` / **`3.0B`** *(`23 SSM + 23 MoE + 6 GQA`)*      | **`14.35 – 17.59 GiB`** *(`15.4 – 21.7 GiB` RSS)*                         | • **Block-64 `Fractal-64` (`3.75 bpw`) & `MXFP4` (`4.25 bpw`)** (`100/100` Quality Gate) • **`Stockfish-PVS v2 (M=7)` + `RouteCrack`** (`1.64 MB` L2/L3 Zobrist TT) • **`432 MB` 256K EAKV (`Q8_0/Q4_0`)** + `_MM_HINT_NTA` L3 isolation • **`18.9 ms` `/dev/shm` Slot-0 Restore** (`~6,800x` faster warm turns)                                                                                                                                                                                               | **`25.0 – 60.4 tok/s`** (`0 GPU`) **`80.8 tok/s`** (`Vulkan Hybrid`) **`111.6 tok/s`** (`Threadripper`)                                                                               | 📖 **[Nemotron Guide](https://github.com/synchopate/llama-modchip/blob/main/docs/NEMOTRON_OPTIMIZATIONS.md)**       |
| **🐋 `DeepSeek-V4-Flash-0731-Fractal`** *(`SwiGLU-H2 + Fractal-64`)*     | **`284.33B`** / **`13.17B`** *(`43L MLA + 256+1s MoE`)* | **`56.56 GiB`** *(`-20.3 GiB`)* *(**`58.9 GiB` RSS** — fits in `64 GB`!)* | • **Analytical 2nd-Order `SwiGLU-H2` MoE + `Fractal-64` MLA Transcoding** • **4-Level Register (`ZMM` AVX-512BW) + L1D (`32KB`) + L2 (`1MB`) + L3 (`64MB`) Multi-Column (`{8,4,3,2}`) Prefill Tiling (`25.0 -> 44.36 tok/s`, `+77%`)** • **Register-Stationary AVX-512 `MQA8` FlashAttention (`2.0x`) + AVX-512 `quantize_row_q8_K`** • **$L\_2$-Quadrature MoE Pruning + L1-Local Gate-First SwiGLU (`10.74 tok/s`)** • **Exact SVD of `wo_b * wo_a`** (`99.82%` CosSim at `-25%` rank) & $O(1)$ Rollback Fix | **`43.2–44.4 tok/s`** *(Prefill)* **`9.06–9.95 tok/s`** *(Lossless)* **`9.87–10.15 tok/s`** *(`Top-5 L2`)* **`10.41–10.74 tok/s`** *(`Top-4+SwiGLU`)* **`11.5–14.7 tok/s`** *(`PVS`)* | 📖 **[DeepSeek-V4 Guide](https://github.com/synchopate/llama-modchip/blob/main/docs/DEEPSEEK_V4_OPTIMIZATIONS.md)** |
| **⚡ `Qwen3-30B-A3B` & `Qwen3-14B`** *(`Q3_K_M` / `Q4_K_M`)*             | `30.5B` / **`3.3B -> 1.7B`** *(`48L GQA`, `128` exp)*   | **`13.70 – 17.28 GiB`** *(`14.5 – 19.4 GiB` RSS)*                         | • **Norm-Preserving Dynamic MoE Top-K (`8 -> 4`, `+38.5%` decode)** • **3:1 Interleaved SWA Safety Lock (`LLAMA_SWA_STRIDE=4`)** • **Windows `11 GB VRAM + 16 GB RAM` Hybrid Split (`--n-cpu-moe N`)**                                                                                                                                                                                                                                                                                                         | **`34.80 tok/s`** (`0 GPU` Top-4) **`50–58 tok/s`** (`14B` RTX 2080 Ti)                                                                                                               | 📖 **[Architecture §3.9](https://github.com/synchopate/llama-modchip/blob/main/ARCHITECTURE.md)**                   |
| **🏎️ `NVIDIA-Nemotron-H-8B-128K`** *(`Q8_0` / `Q4_K_M`)*                | `8.0B` Dense *(`48 SSM + 4 GQA`)*                       | **`4.60 – 8.00 GiB`** *(`210 MB` KV @ `128K`)*                            | • **48-Layer Mamba-2 SSM Constant-Memory Backbone** • **100% GPU VRAM Residency on `8–11 GB` Cards (`0 GB` System RAM)**                                                                                                                                                                                                                                                                                                                                                                                       | **`100+ tok/s`** (`RTX 2080 Ti`) **`147.6 tok/s`** (`CPU pp512`)                                                                                                                      | 📖 **[Nemotron Guide](https://github.com/synchopate/llama-modchip/blob/main/docs/NEMOTRON_OPTIMIZATIONS.md)**       |

* * *

## 🖥️ Supported Chips & Processor-Specific Optimizations

Full microarchitectural details and tuning flags for every supported processor and GPU hybrid are documented in **[`docs/HARDWARE_OPTIMIZATIONS.md`](https://github.com/synchopate/llama-modchip/blob/main/docs/HARDWARE_OPTIMIZATIONS.md)**:

| Processor / Platform                             | Verified Hardware                                                  | Hardware-Specific Optimizations Active                                                                                                                                                                                                                                      | Platform Guide & Scripts                                                                                                                                                                                    |
| ------------------------------------------------ | ------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **AMD Zen 5 (`Desktop / Mobile / Server`)**      | `Ryzen 9 9955HX`, `9950X`, `EPYC 9005 Turin`                       | • Single-cycle 512-bit **AVX-512 (`VPDPBUSD`, `VPRORVQ`, `VBMI`, `BF16`)** • **`48 KB` L1D 3D Tiling (`TILE_I0=64`)** on multi-head `wo_a` projections • **`_MM_HINT_NTA` Victim-L3 Isolation (`64 MB`)** + **`2MB` THP (`6.14 GB` L2 STLB)**                               | 📖 **[Hardware Guide §2.1](https://github.com/synchopate/llama-modchip/blob/main/docs/HARDWARE_OPTIMIZATIONS.md)** [`install.sh`](https://github.com/synchopate/llama-modchip/blob/main/install.sh)        |
| **AMD Zen 4 Multi-CCD (`Threadripper / EPYC`)**  | `Threadripper PRO 7985WX` (`64C/128T`, `8 CCDs`, `256 MB` L3)      | • **Dynamic sysfs L3 CCD Topology Discovery** ([`scripts/llama_modchip.py`](https://github.com/synchopate/llama-modchip/blob/main/scripts/llama_modchip.py)) • **Deterministic `4C/CCD` Stride (`-t 32`)**: avoids Infinity Fabric saturation (`+54%` faster than `-t 64`) | 📖 **[Hardware Guide §2.2](https://github.com/synchopate/llama-modchip/blob/main/docs/HARDWARE_OPTIMIZATIONS.md)** [Issue #1](https://github.com/synchopate/llama-modchip/issues/1)                        |
| **AMD 3D V-Cache + Radeon Vulkan Hybrid**        | `Ryzen 9 7945HX3D` (`128 MB` L3) + `RX 9070 XT 16GB` (`RADV`)      | • **Tensor-Role Splitting (`-ngl 99 --n-cpu-moe N --load-mode none`)** • Native **Vulkan `SSM_SCAN` + `Fractal-MXFP4` (`15.2 GB`) GPU Shaders** (`80.8 tok/s` at 256K)                                                                                                      | 📖 **[Hardware Guide §2.3](https://github.com/synchopate/llama-modchip/blob/main/docs/HARDWARE_OPTIMIZATIONS.md)** [`FAQ.md` Q8](https://github.com/synchopate/llama-modchip/blob/main/FAQ.md)             |
| **Intel / AMD + NVIDIA RTX (`Windows 10/11`)**   | `Core i9-9900K / 14900K` + `RTX 2080 Ti 11GB / 3090 / 4090`        | • **Single-Cycle AVX2 `vpsllvd + vpmaddwd` Sub-Scale Fusion** • **Live-VRAM `--n-cpu-moe` Solver (`550 MB` WDDM Reserve)** + `_WIN32` `CreateFileMappingA`                                                                                                                  | 📖 **[Windows Guide](https://github.com/synchopate/llama-modchip/blob/main/windows/README.md)** [`windows/modchip_win.ps1`](https://github.com/synchopate/llama-modchip/blob/main/windows/modchip_win.ps1) |
| **Apple Silicon (`M1 / M2 / M3 / M4`)**          | `Apple M4 Max` (`16C+40C`, `64 GB`), `M4 Pro` (`14C+20C`, `24 GB`) | • **Zero-Copy Unified Memory Metal Shaders + ARM64 NEON/AMX** (`238.9 GB/s` DRAM BW on M4 Max) • `0`-Swap `128K–256K` Context Budgeting (`384 MB` KV) + `Stockfish-PVS v2`                                                                                                  | 📖 **[Apple Silicon Guide](https://github.com/synchopate/llama-modchip/blob/main/apple-silicon/README.md)** [Issue #8](https://github.com/synchopate/llama-modchip/issues/8)                               |
| **ARM64 Cloud & Edge (`AWS Graviton / Ampere`)** | `AWS Graviton3 m7g.2xlarge` (`8 vCPU Neoverse-V1`, `32 MB` L3)     | • **ARMv8.4-A DotProd (`SDOT`) + 256-Bit SVE** + 1-vCPU-per-core pinning (`-t 8`) • `32 MB` System Level Cache (`SLC`) retention (`44.97 tok/s` decode)                                                                                                                     | 📖 **[Hardware Guide §1](https://github.com/synchopate/llama-modchip/blob/main/docs/HARDWARE_OPTIMIZATIONS.md)** [Issue #2](https://github.com/synchopate/llama-modchip/issues/2)                          |
| **AMD Zen 4 Compact Mini-PC (`Linux`)**          | `Ryzen 7 8745H / 8745HS` (`8C/16T`, `16 MB` L3 + `Radeon 780M`)    | • **AVX-512 VNNI** + `16 MB` L3 window auto-tuning (`19.48–23.38 tok/s` on `96 GB` DDR5-5600) • Optional `Radeon 780M` UMA iGPU offload                                                                                                                                     | 📖 **[Fedora Guide](https://github.com/synchopate/llama-modchip/blob/main/fedora/README.md)** [Issue #9](https://github.com/synchopate/llama-modchip/issues/9)                                             |

* * *

## 📊 Headline Benchmarks: Pure CPU & Hybrid Summary

### 1\. `DeepSeek-V4-Flash 284B` (`13.17B` Active) on a `64 GB` Zen 5 CPU (`AMD Ryzen 9 9955HX`, `0 GPUs`)

Full operator-level profiling and mathematical proofs in **[`docs/DEEPSEEK_V4_OPTIMIZATIONS.md`](https://github.com/synchopate/llama-modchip/blob/main/docs/DEEPSEEK_V4_OPTIMIZATIONS.md)**:

| `DeepSeek-V4-Flash-0731-Fractal` (`56.56 GiB` @ `256K` Context)       | Prefill (`pp512`)                      | Pure Decode (`N=1`)                   | With `Stockfish-PVS`    | **Prob Cosine Sim** | **KL-Divergence ($\\Delta$ PPL)** | **Top-1 / Top-3 Match** |
| --------------------------------------------------------------------- | -------------------------------------- | ------------------------------------- | ----------------------- | ------------------- | --------------------------------- | ----------------------- |
| **Standard `IQ2_XXS` (`76.87 GiB`, Unpatched)**                       | 🔴 *OOM on 64GB*                        | `5.91 tok/s` (`169.3 ms`)             | N/A *(Rollback Bug)*    | `1.00000`           | `0.0000` (`0.00%`)                | `100% / 100%`           |
| **1\. `v2.4.0` Lossless (`Top-6` All Layers, Default)**               | **`43.16 – 44.36 tok/s`** *(`+77.1%`)* | **`9.06 – 9.95 tok/s`**               | **`11.5 – 14.7 tok/s`** | **`1.00000`**       | **`0.0000` (`0.00%` Bit-Exact)**  | **`100% / 100%`**       |
| **2\. `v2.4.0` Sweet-Spot (`Top-5` on `il >= 3`, $L\_2$ Quadrature)** | **`43.16 – 44.36 tok/s`**              | **`9.68 – 9.87 tok/s`**               | **`12.2 – 15.1 tok/s`** | **`0.99714`**       | **`0.0042 nats` (`+0.42%` PPL)**  | **`95.8% / 100%`**      |
| **3\. `v2.4.0` `Top-5 L2` + Gate-First SwiGLU (`80/128` $L\_2$)**     | **`43.16 – 44.36 tok/s`**              | **`10.15 tok/s` (`98.5 ms`)**         | **`12.8 – 15.6 tok/s`** | **`0.99385`**       | **`0.0531 nats`**                 | **`100% / 100%`**       |
| **4\. `v2.4.0` Turbo `10+` (`Top-4 L2` + Gate-First SwiGLU)**         | **`43.16 – 44.36 tok/s`**              | **`10.12 – 10.74 tok/s` (`93.2 ms`)** | **`13.0 – 16.0 tok/s`** | **`0.99126`**       | **`0.0281 – 0.0694 nats`**        | **`100% / 100%`**       |

### 2\. `NVIDIA-Nemotron-3.5-30B-A3B` (`3.0B` Active) at `256K` Context (`AMD Ryzen 9 9955HX`, `0 GPUs`)

Full ablation tables and `100/100` Quality Gate report in **[`docs/NEMOTRON_OPTIMIZATIONS.md`](https://github.com/synchopate/llama-modchip/blob/main/docs/NEMOTRON_OPTIMIZATIONS.md)**:

| Quantization Profile                | Disk Size       | Idle `VmRSS` (`256K`)         | Quality Gate (`100` pts) | Prefill (`pp512`)     | Base Decode (`N=1`)          | `Stockfish-PVS v2 (M=7)` Refactoring                 |
| ----------------------------------- | --------------- | ----------------------------- | ------------------------ | --------------------- | ---------------------------- | ---------------------------------------------------- |
| **`Baseline-Q4_0` (Official GGUF)** | `17.59 GiB`     | `21.41 GiB`                   | `100.0 / 100.0`          | `241.0 – 306.8 tok/s` | `19.54 tok/s`                | `40.91 avg / 45.43 peak tok/s`                       |
| **`Fractal-MXFP4` (`4.25 bpw`)**    | `15.95 GiB`     | `17.46 GiB`                   | **`100.0 / 100.0`**      | `241.0 – 306.8 tok/s` | **`26.08 tok/s` (`+33.5%`)** | **`48.39 avg / 60.40 peak tok/s`** *(`80.8` Vulkan)* |
| **`Fractal-64-BF16` (`3.75 bpw`)**  | **`14.35 GiB`** | **`15.40 GiB` (`-6.01 GiB`)** | **`100.0 / 100.0`**      | `241.0 – 306.8 tok/s` | **`25.02 tok/s` (`+28.0%`)** | **`48.39 avg / 60.40 peak tok/s`** *(`96.4%` acc)*   |

* * *

## 🏆 Community CPU & Hybrid Leaderboard

Run **`./modchip bench`** (or **`windows\BENCHMARK_PC.bat`** on Windows) and **[➕ Submit Your Scorecard (1-Click GitHub Issue)](https://github.com/synchopate/llama-modchip/issues/new?template=benchmark.yml)**!

| Hardware Platform (`CPU / SoC / Hybrid`)              | Cores / Cache                                    | Memory                                 | Prefill (`tok/s`)                              | Decode — Prose / C++20                              | Decode — `Stockfish M=7` Burst                                | Config & Contributor                                                                                                                            |
| ----------------------------------------------------- | ------------------------------------------------ | -------------------------------------- | ---------------------------------------------- | --------------------------------------------------- | ------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| **AMD Threadripper PRO 7985WX (`Zen 4`)**             | `64C/128T` (`8 CCDs`, `256 MB` L3)               | `256 GB` 8-Ch DDR5-6000 (`0 GPU`)      | **`386.00`** *(`-t 32`)*                       | **`73.84 – 80.14 tok/s`** *(`70.0%` acc)*           | **`111.62 tok/s` warm** / **`80.14` cold** *(`-t 32` 4C/CCD)* | `@aifartist` ([Issue #1](https://github.com/synchopate/llama-modchip/issues/1#issuecomment-6595559921))                                        |
| **AMD Ryzen 9 7945HX3D + RX 9070 XT (`Hybrid`)**      | `16C/32T` (`128 MB` 3D V-Cache) + `16 GB` Vulkan | `94 GB` DDR5 + `16 GB` VRAM            | **`563.00`** *(256K ctx)*                      | **`75.10 tok/s`** *(`MXFP4` 15.2GB)*                | **`80.80 tok/s`** *(Vulkan `SSM_SCAN` + `MXFP4` MoE)*         | `Community (RADV/Vulkan)` ([`FAQ.md`](https://github.com/synchopate/llama-modchip/blob/main/FAQ.md))                                           |
| **AMD Ryzen 9 9955HX (`Zen 5`)**                      | `16C/32T` (`2 CCDs`, `64 MB` L3)                 | `64 GB` DDR5 (`0 GPU`)                 | **`241.0`** *(Peak **`306.8`**)*               | **`25.02 – 34.5 tok/s`** *(`Fractal-64` / `MXFP4`)* | **`48.39 avg / 60.40 peak`** *(`len 6.90`)*                   | `@synchopate` ([`docs/NEMOTRON_OPTIMIZATIONS.md`](https://github.com/synchopate/llama-modchip/blob/main/docs/NEMOTRON_OPTIMIZATIONS.md))       |
| **Apple M4 Max (`16C CPU + 40C GPU`)**                | `16C` (`ARM64 NEON` + `40C Metal`)               | `64 GB` LPDDR5X-8533 (`238.9 GB/s` BW) | **`~451.0`** *(Audit)*                         | **`49.2 – 57.7 tok/s`** *(Prose / Code)*            | **`70.9 – 120.0 tok/s`** *(Agent / Refactor)*                 | `@jbeckman1974` ([Issue #8](https://github.com/synchopate/llama-modchip/issues/8))                                                             |
| **Apple M4 Pro (`14-Core SoC`)**                      | `14C CPU + 20C Metal`                            | `24 GB` Unified (`0 Swap`)             | **`256.0 – 549.0`**                            | **`47.0 – 48.0 tok/s`** *(128K)*                    | **`49.0+ tok/s`**                                             | `@jarorosFork` ([`apple-silicon/`](https://github.com/synchopate/llama-modchip/blob/main/apple-silicon/README.md))                             |
| **AWS Graviton3 `m7g.2xlarge` (`ARM64 Neoverse-V1`)** | `8 vCPU` (`32 MB` L3, `SVE-256`)                 | `31 GB` DDR5 (`0 GPU`)                 | **`101.67`** *(`-t 8`)*                        | **`41.05 tok/s`** *(C++20)*                         | **`44.97 tok/s`** *(Refactoring)*                             | `@gerchik` ([Issue #2](https://github.com/synchopate/llama-modchip/issues/2))                                                                  |
| **AMD EPYC 7402P (`Zen 2 Rome` · `AVX2-only`)**       | `24C/48T` (`8 CCDs`, `128 MB` L3)                | `128 GB` 8-Ch DDR4 (`0 GPU`)           | **`158.73`** *(`-t 24`)*                       | **`27.29 tok/s`** *(C++20, `567 ms` TTFT)*          | **`31.22 tok/s`** *(Cold Run 1, `66.7%` acc)*                 | `@AdamKlob` ([Issue #6](https://github.com/synchopate/llama-modchip/issues/6))                                                                 |
| **AMD Ryzen 9 9950X (`Zen 5 Desktop`)**               | `16C/32T` (`2 CCDs`, `64 MB` L3)                 | `64 GB` DDR5-6800 (`0 GPU`)            | **`135.64`** *(v2.1 `Q4_0`)*                   | **`20.78 tok/s`** *(C++20, `526 ms` TTFT)*          | **`24.67 tok/s`** *(Cold Run 1, `66.5%` acc)*                 | `@nadams` ([Issue #5](https://github.com/synchopate/llama-modchip/issues/5))                                                                   |
| **AMD Ryzen 7 8745H w/ Radeon 780M (`Zen 4`)**        | `8C/16T` (`16 MB` L3, `AVX-512`)                 | `96 GB` DDR5-5600 (`0 GPU`, Arch)      | **`128.81`** *(`-t 8`)*                        | **`19.48 tok/s`** *(C++20, `686 ms` TTFT)*          | **`23.38 tok/s`** *(`69.8%` PVS acc)*                         | `@arch-btw` ([Issue #9](https://github.com/synchopate/llama-modchip/issues/9))                                                                 |
| **AMD Ryzen 7 5700U (`Zen 2 Mobile 15W`)**            | `8C/16T` (`8 MB` L3, `AVX2`)                     | `64 GB` DDR4 (`0 GPU`, Debian)         | **`42.80`** *(`-t 8`)*                         | **`12.92 tok/s`** *(C++20, `1720 ms` TTFT)*         | **`14.35 tok/s`** *(`66.5%` PVS acc)*                         | `@JamesStallings` ([Issue #10](https://github.com/synchopate/llama-modchip/issues/10))                                                         |
| **Intel Core i9-11950H (`Tiger Lake-H`)**             | `8C/16T` (`24 MB` L3, `AVX-512`)                 | `128 GB` DDR4-3200 (`0 GPU`, Fedora)   | **`85.39`** *(`-t 8`)*                         | **`10.76 tok/s`** *(C++20, `1106 ms` TTFT)*         | **`13.37 tok/s`** *(`69.8%` PVS acc)*                         | `@ELO-I` ([Issue #11](https://github.com/synchopate/llama-modchip/issues/11))                                                                  |
| **Windows 11 Hybrid (`i9-9900K + RTX 2080 Ti 11GB`)** | `8C/16T` (`16 MB` L3) + `11 GB` CUDA             | `16 GB` DDR4 + `11 GB` GDDR6           | *Auto CUDA 12.4*                               | **`50–58 tok/s`** *(`14B`) / 30B Hybrid*            | **`100+ tok/s`** *(`Nemotron-H-8B` 128K)*                     | [`windows/`](https://github.com/synchopate/llama-modchip/blob/main/windows/README.md)                                                          |
| **AMD Ryzen 7 8745HS (`GEEKOM A8`)**                  | `8C/16T` (`16 MB` L3)                            | `64 GB` DDR5-5600                      | *Verified Fedora 44*                           | *Verified (`ngram`)*                                | *Verified (`CPU` & `780M`)*                                   | `@jarorosFork` ([`fedora/`](https://github.com/synchopate/llama-modchip/blob/main/fedora/README.md))                                           |
| **🐋 `DeepSeek-V4-Flash 284B` on Ryzen 9 9955HX**      | `16C/32T` (`2 CCDs`, `64 MB` L3)                 | `64 GB` DDR5 (`58.9 GiB` RSS @ `256K`) | **`43.16 – 44.36 tok/s`** *(`32.6 ms` Slot-0)* | **`9.06 – 10.74 tok/s`** *(`13.17B` active)*        | **`11.50 – 14.69 tok/s`** *(`94.5%` PVS acc)*                 | `@synchopate` ([`docs/DEEPSEEK_V4_OPTIMIZATIONS.md`](https://github.com/synchopate/llama-modchip/blob/main/docs/DEEPSEEK_V4_OPTIMIZATIONS.md)) |

**🛠️ Click to expand: Repository Structure & Manual CMake Build Instructions**

```
llama-modchip/
├── modchip                                    # Unified CLI (setup | start | chat | bench | eval | quantize | web | claude | stop)
├── install.sh                                 # 1-Command Automated Installer (Linux)
├── README.md / ARCHITECTURE.md / FAQ.md       # Clean Overview, Full Mathematical Paper & Engineering FAQ
├── RELEASE_NOTES.md                           # v2.4.0 Release Notes
├── docs/
│   ├── NEMOTRON_OPTIMIZATIONS.md              # Deep-dive on Nemotron-3.5-30B-A3B & Nemotron-H-8B optimizations
│   ├── DEEPSEEK_V4_OPTIMIZATIONS.md           # Deep-dive on DeepSeek-V4-Flash 284B profiling, SVD & L2-quadrature MoE
│   └── HARDWARE_OPTIMIZATIONS.md              # Chip-by-chip optimizations (Zen 5, Threadripper, Vulkan, CUDA, Metal, ARM64)
├── patches/
│   ├── deepseek-v4-flash-fractal-256k.patch   # Unified v2.4.0 patch (DeepSeek-V4-Flash 284B + Nemotron-3.5 30B, Linux/macOS/Windows)
│   └── zen5-nemotron-256k-optimizations.patch # Standalone ggml/llama core patch for Nemotron-3.5
├── windows/                                   # 1-Click Windows 10/11 Hybrid GPU+CPU MoE Pack (AVVIA_MODCHIP.bat, modchip_win.ps1)
├── apple-silicon/                             # Apple M4 Pro (24 GB) Metal setup & 128K benchmarks
├── fedora/                                    # Fedora 44 / AMD Ryzen 7 8745HS (GEEKOM A8) all-in-one script
├── speculative/                               # RouteCrack + Stockfish-PVS v2 (rc_stockfish.h, ngram-map.cpp, 96+ techniques)
├── benchmarks/                                # eval_quality_and_tools.py (100-pt Quality Gate) & results/*.json
├── gateway/sse_gateway.py                     # Agentic SSE Proxy + Compact Tool Table + Dark-Mode Web Chat HUD (:8787)
└── scripts/                                   # build_fractal_moe_gguf.sh, transcode_dsv4.{c,py}, chat.py, llama_modchip.py
```

* * *

## 🏢 Enterprise & Hardware Sponsorships

`llama-modchip` demonstrates that modern multi-core CPUs and compact Mini-PCs with 64–128 GB of DDR5 RAM can run **284B Frontier MoEs (`DeepSeek-V4-Flash`)** and **30B Hybrid MoEs (`Nemotron-3.5-30B-A3B`)** locally at full `256K` context—eliminating $30,000+ multi-GPU hardware requirements for air-gapped enterprise code intelligence, security auditing, and autonomous agent fleets.

We actively partner with **hardware manufacturers, cloud silicon teams, and AI research labs** to build and validate architecture-specific kernel patches:

- 🖥️ **CPU & Workstation OEMs (`AMD Ryzen AI / Threadripper / EPYC`, `Minisforum`, `Framework`, `System76`, `Apple`, `Intel`)**: Sponsor a physical test unit or workstation to receive dedicated L1D/L2/L3 cache-tiling & SIMD kernels and permanent verification on the **Official README CPU Leaderboard**.
- ☁️ **Cloud & ARM64 Silicon Teams (`AWS Graviton4`, `AmpereOne`, `Google Axion`, `Azure Cobalt`)**: Sponsor cloud compute credits to validate `SVE2` / `SME` / multi-socket NUMA kernels.
- 🤝 **Enterprise Support & Custom Model Transcoding**: Reach out via [GitHub Issues](https://github.com/synchopate/llama-modchip/issues) or [GitHub Sponsors](https://github.com/sponsors/synchopate) to sponsor custom `SwiGLU-H2 + Fractal-64` model transcoding and on-premise deployment.
