---
title: Run a Local LLM on a CPU With BitNet and a Sprite
link: https://fly.io/blog/local-llm-on-a-sprite/
source: fly-io
published: 2026-03-16T00:00:00Z
updated: 2026-11-06T16:35:06Z
first_seen: 2026-11-27T00:01:05.630507547Z
content: extracted
html: 2026-03-16-run-a-local-llm-on-a-cpu-with-bitnet-and-a-sprite.html
remote_preview:
  url: https://fly.io/blog/local-llm-on-a-sprite/assets/local-llm-on-a-sprite.webp
  alt: Running a local LLM on a Sprite
extra:
  thumbnail: https://fly.io/blog/local-llm-on-a-sprite/assets/local-llm-on-a-sprite.webp
---

Author

![Daniel Botha](https://fly.io/static/images/danielb.webp)

Name

Daniel Botha

![Running a local LLM on a Sprite](https://fly.io/blog/local-llm-on-a-sprite/assets/local-llm-on-a-sprite.webp)

There’s a wave of absurdly efficient local inference engines showing up. Models quantized so aggressively that they run on CPUs, no GPU required, at speeds that are actually usable. Microsoft’s [BitNet](https://github.com/microsoft/BitNet "") is one of them: 1.58-bit quantization, ternary weights, optimized SIMD kernels. A 2.4 billion parameter model that fits in 1.1GB of RAM.

So naturally, I needed to see if one of these little guys could achieve something approaching acceptable performance, running on a [Sprite](https://fly.io/sprites/ ""). And if it could, how much time could I waste wondering what I could do with a tiny LLM running on a hardware-isolated computer?

## Why though?

Sprites spin up with access to frontier models literally one CLI command away. Claude is right there. So why would I want to get a puny little 2B model running locally on a Sprite?

Because a tiny local model on a Sprite is not the same as a tiny local model on your laptop.

Every Sprite already has a URL with TLS. Stand up an HTTP server, register it as a service, and now any other Sprite on your account can call it. Your [agent swarm](https://fly.io/blog/agent-swarms-on-sprites/ "") just gained a shared inference endpoint by adding one more Sprite to the fleet. No infrastructure changes, no new dependencies, just another URL.

The Sprite sleeps when nobody’s calling it. A request arrives, the Sprite wakes, the model loads (~1.2 seconds), inference runs, response goes back, Sprite goes back to sleep. That lifecycle doesn’t exist on a VPS. You’re not paying for idle compute between bursts of classification requests at 3am.

## Checkpoints are amazing for working with local models

With a local model, the environment *is* the product. The compiled kernels, the model weights, the server code, the Python deps, it’s all files on disk. A [checkpoint](https://docs.fly.io/sprites/concepts/checkpoints "") captures the entire thing atomically. I can hear you asking who gives a shit, but think about that for a minute.

You have BitNet-2B running and serving requests. You want to try the 3B model. Or a Falcon variant. Or you want to retune the kernel parameters for better throughput. On a normal box, you’d be juggling git branches, backing up model files, praying your venv still works after you upgraded numpy. Here it’s `checkpoint create`, fuck around, `checkpoint restore` if it goes wrong. The whole stack, OS deps, compiled binaries, model weights, server code, rolls back together.

Small models are wildly sensitive to prompt formatting. The difference between `User:` and `[INST]` can be the difference between coherent output and gibberish. Checkpoint before you start experimenting with chat templates. Try five different formats. Keep the one that works. Restore if you accidentally break the server while hacking on it.

New llama.cpp commit drops with performance improvements? Checkpoint your working state, pull, rebuild. If it breaks, you’re one restore from a working service. This is how you gain the confidence to actually try cool new shit when it emerges, as opposed to living in fear that your whole service will drop dead if you even say update within earshot.

## Try it

Head to [https://bitnet-llm-beony.sprites.app/chat](https://bitnet-llm-beony.sprites.app/chat) where you can chat with BitNet-b1.58-2B-4T with a ~200 line Python HTTP server in front of it. No frameworks. The server wraps `llama-cli` as a subprocess and exposes an OpenAI-compatible API at `/v1/chat/completions`, a status page at `/`, and the self-serving client at `/client.py`. Register it as a Sprite service on port 8080 and the Sprite URL does the rest.

Yeah, the Sprite running the model serves its own client. Hit `/client.py` and you get a single Python file with the Sprite’s URL baked in. No pip install, no API keys, no config. Any Sprite grabs it with one curl and immediately has `ask()` and `classify()` functions that work. The model distributes its own SDK.

## Is it crap?

It should come as no surprise that this thing is about as intelligent as a shovel and as well-informed as a child raised by wolves. Nonetheless, for what it is, it’s damn impressive.

- **Model size on disk:** 1.1GB
- **RAM usage:** ~1.3GB total (model + KV cache)
- **Generation speed:** ~50 tokens/sec
- **Request latency:** ~1.4 seconds end-to-end

50 tokens per second on a CPU. That’s not a typo. 1.58-bit quantization is doing real work here.

I grabbed the client from the Sprite and tested it:

```wrapper
from llm import ask, classify

ask("What is a Sprite in computing?")
# "A sprite in computing is a simple graphical object, such as a
#  character or icon, that is drawn in a single color and occupies
#  a single display area."

classify("the deploy failed with a timeout error", ["bug", "feature", "ops"])
# "bug"

ask("Name three prime numbers under 20.")
# "The prime numbers under 20 are 2, 3, 5, 7, 11, 13, and 17."
```

Coherent. Correct. ~1.4 seconds per response.

## Build your own

Tell Claude:

> Create a new Sprite. Clone [https://github.com/microsoft/BitNet](https://github.com/microsoft/BitNet) with submodules. Install cmake and libomp-dev. Don’t bother with Conda, just pip install the requirements directly. Install sentencepiece 0.2.1 separately first since the pinned 0.2.0 won’t build on this compiler. Install the local gguf package from 3rdparty/llama.cpp/gguf-py with –no-deps. Run the codegen and compile steps for x86*64 with i2*s quantization. There’s a const-correctness bug in ggml-bitnet-mad.cpp around line 811 where a non-const pointer is assigned from a const source, fix it. Download the pre-quantized GGUF from microsoft/bitnet-b1.58-2B-4T-gguf instead of converting the model yourself. Verify inference works, then checkpoint.
>
> Then build a Python HTTP server that wraps llama-cli and serves: an OpenAI-compatible chat endpoint at /v1/chat/completions, a status page at / showing model info and usage instructions, a self-addressed Python client at /client.py that uses only stdlib and has the Sprite’s URL baked in, and a chat UI at /chat with a dark monospace theme that keeps conversation history in the browser. Register it as a Sprite service on port 8080. Checkpoint again.

That’s the whole thing. Text classification, structured extraction, triage, routing, anything where a frontier model is overkill and a regex isn’t enough. If you want to get clever, use the local model for the fast cheap stuff and fall back to Claude for anything that needs real reasoning. The OpenAI-compatible API means swapping the endpoint is a one-line change.

1.58 bits per weight. 50 tokens per second. On a CPU. LOL.

Next post ↑

[Run Paperclip AI on a Sprite](https://fly.io/blog/paperclip-on-a-sprite/)

Previous post ↓

[Context Engineering Is Just Writing Things Down](https://fly.io/blog/context-engineering-is-just-writing-things-down/)
