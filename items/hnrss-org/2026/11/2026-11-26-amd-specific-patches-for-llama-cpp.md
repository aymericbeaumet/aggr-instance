---
title: AMD Specific Patches for Llama.cpp
link: https://devnull.land/amd-specific-llama-cpp
source: hnrss-org
published: 2026-11-26T17:11:29Z
updated: 2026-11-26T17:11:29Z
first_seen: 2026-11-26T21:52:31.976721529Z
authors:
- julianlam
content: extracted
html: 2026-11-26-amd-specific-patches-for-llama-cpp.html
---

**tl;dr** — I recommend using [llama-cpp-rdna-boosts](https://github.com/stew675/llama-cpp-rdna-boosts) if you have an AMD graphics card. It’s lean, and increased my token generation from 35 to 55 tokens/second, and my prefill from 700 to 1300 tokens per second. YMMV.

* * *

As of November 2026, an explosion in the number of bespoke inference engines has taken place. Many of them take inspiration from the engines that enabled much of the LLM inference up 'til now: llama.cpp, SGLang, vLLM, etc.

The rationale isn’t that those options are bad, but merely that they are *generalist* engines, optimized for no particular hardware and with a focus on broad compatibility. Others feel the development pace of those engines have slowed down, and with the help of LLMs, they can move faster on their own.

Some examples of these new inference engines\* are Strata, Basalt, RustingStrata, etc…

These new forks generally all have a few things in common:

1. They focus on a specific subset of models and/or hardware
2. They use a frontier model to vibe-code the entire thing
3. They chase ever-higher token generation speeds

… and the results are stunning! Strata is able to run a heavily-quantized model of Qwen 3.8 Flash Next, a **180B parameter model**, on a 16GB VRAM graphics card, and 16GB system RAM, and have it generate tokens at a surprisingly high rate.

In and of itself those three things are not red flags (the second is concerning, of course, but it may be SOP in a year’s time — who knows!), but as these projects have evolved, that relentless chase toward higher token generation speeds has taken an unfortunate turn, in that the optimizations are less about hardware-specific changes, but more about changing how the model **thinks** so that it goes brrr faster.

Strata specifically has taken to preferring to poll experts loaded *in VRAM* even if other experts (pushed to system RAM/disk) would have been normally chosen. This is just one example, but certainly not the last.

* * *

That said, I was fortunate to stumble upon yet another project that promised improvements, delivered, and was more aligned with my software development philosophy.

Enter [**llama-cpp-rdna-boosts**](https://github.com/stew675/llama-cpp-rdna-boosts). It is a repository that contains a minimal set of changes that you apply against a pinned commit of llama.cpp. It is in essence a fork, but one whose changeset is continually re-applied to newer versions of llama.cpp. It contains AMD-specific improvements that drastically improve prefill and token-generation speeds.

1. I appreciate llama.cpp’s approach to software development. They are a stable base to branch off from. Contributions are vetted throughly and applied judiciously. ✅
2. The *rdna-boosts* repo is a set of small patches that are applied to base llama.cpp. Fewer moving parts mean fewer things to maintain ✅
3. The changes are (somewhat opaquely, it’s all straight Claudish 😬) outlined in the documentation, ensuring some degree of transparency. ✅

### The setup

If you have ROCm libraries installed on your system, you can build the image yourself. I didn’t want to mess with that, so I used the pre-built images published.

`podman pull ghcr.io/stew675/llama-cpp-rdna-boosts:server-rocm-10.1`

This sets up an image that you can run sandboxed via podman to launch the inference server:

```bash
podman run --rm -it \
    --network=host \
    --device /dev/kfd \
    --device /dev/dri \
    --group-add video \
    -v "$HOME/models:/models" \
    "ghcr.io/stew675/llama-cpp-rdna-boosts:server-rocm-10.1" \
```

My actual launch script passes a few additional flags, but those are the important lines.

I imagine upgrading would be pulling a new image down, though I have not tried this yet.

### The numbers

On my setup, an AMD R9700 connected to my laptop via an eGPU interface, this improved prefill and token generation rates when running [ukisai/Swift-1.5-Qwen3.8-27B-GGUF (Q6\_K\_L)](https://huggingface.co/ukisai/Swift-1.5-Qwen3.8-27B-GGUF) with MTP enabled:

|                  | Before    | After      |
| ---------------- | --------- | ---------- |
| Prefill          | \~700 t/s | \~1300 t/s |
| Token Generation | \~35 t/s  | \~55 t/s   |

* * *

Strata and its ilk were always promising the moon and it was never clear what steps they took to make that happen. I’m very much looking forward to what the rdna-boosts community come up with next.

\* It’s worth noting that between these two groups, [vllm-radiance](https://codeberg.org/StillDeadcode/vllm-radiance/) made an appearance, which was a fork of vLLM with AMD-specific patches. I tried this too, but the build steps were so onerous and vLLM so overbuilt for my specific use case (1 machine, 1 developer), that I abandoned it and went back to vanilla llama.cpp

[← Back to all posts](https://devnull.land/)
