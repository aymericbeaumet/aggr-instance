---
title: A Beginner's Guide to Local AI with Inkling, Qwen, and Unsloth
link: https://www.gpupartner.com/blog/beginners-guide-local-ai-inkling-qwen-unsloth
source: hnrss-org
published: 2026-11-22T21:01:16Z
updated: 2026-11-22T21:01:16Z
first_seen: 2026-11-23T01:33:27.333975467Z
authors:
- Retell15
content: extracted
html: 2026-11-22-a-beginner-s-guide-to-local-ai-with-inkling-qwen-and-unsloth.html
remote_preview:
  url: https://www.gpupartner.com/blog/beginners-guide-local-ai-inkling-qwen-unsloth/opengraph-image-17m7t5?6653c98c6cd84ba6
---

We have AI at home! Set up your own free personal ChatGPT on your PC with Unsloth and Inkling or Qwen. It's private, and the whole house can use it.

With the launch of [open-weight](https://www.gpupartner.com/reference/open-weights) models like Qwen, cloud-based LLM providers seem like they're more of infrastructure or appliance businesses when it comes to most routine tasks that an AI model can handle.

OpenAI and Anthropic have displayed a pattern of dropping model quality in the 'latest' version and controlling exactly what you're allowed to run on their models. Add to this ballooning costs and privacy concerns, and we get a pretty good case for moving away from this 'convenient' cloud experience.

That's basically where I was too. I wanted to offload certain token-heavy research, data collection, or coding tasks to an AI model without staring at usage limits all day. So, as any reasonable human being would do, I decided to set up my own [local AI](https://www.gpupartner.com/reference/local-llm): a Claude and ChatGPT replacement that runs at home on my own hardware.

Since this was my first attempt, my primary focus was keeping it simple and hosting it on Windows. If you're a beginner, you can follow along to get one going on your home network too.

Once you have the hardware sorted, it takes about 10-15 minutes (excluding download times).

### The short version

Install Unsloth Desktop, download a quantized AI model that fits your GPU's VRAM, set your context size, and turn on LAN access so everyone at home can use it.

## What Do You Need to Run a Local AI at Home?

Not much (other than the hardware). Here's a short list:

- **A Windows PC with a graphics card (GPU**). The more video memory (VRAM) it has, the bigger the model it can run. I'm using an [RTX 5090](https://www.gpupartner.com/tools/vram-calculator/machines/rtx-5090) with 32GB VRAM for this setup.
- **Unsloth Desktop**. A free app that 'serves' the AI model to you through a chat interface. [Get it from Unsloth](https://unsloth.ai/docs/desktop).
- **An AI model sized to fit your GPU**. We'll pick one in the next section.

That's it. No terminal commands, Python environments, or subscriptions required.

## Which AI Model Can Your GPU Run?

It depends on how much [VRAM](https://www.gpupartner.com/reference/vram) your graphics card has. As a rough guide, 8-12GB of VRAM handles smaller 9B-12B models, while 32GB comfortably runs 27B-35B models with plenty of room for long conversations.

On consumer-grade hardware, we'll need smaller models. The full-size Qwen model sits at around 56GB and won't really fit on a consumer graphics card unless you're running a Mac with unified memory. That's where quantization comes in.

### What Is Quantization?

[Quantization](https://www.gpupartner.com/reference/quantization) shrinks a model by compressing weights and moving to a lower precision. You basically trade some accuracy for a model that actually fits on your GPU and runs faster.

![The same close-up of an RTX 5090 fan shown three times: at 8 bits per pixel with 256 shades of grey, at 4 bits with 16 shades and faint banding, and at 2 bits with 4 shades, where it turns blotchy.](https://cdn.gpupartner.com/prod/quantization-same-photo-8-bit-4-bit-2-bit.png)

The same photo stored at 8, 4 and 2 bits per pixel. A model's weights lose detail the same way going from Q8 to Q4 to Q2.

If you're wondering whether this tradeoff is worth it, well, it depends. You'll find that certain types of workloads don't really care and give similar outputs, while others may be nearly unusable.

### Which Model Should You Run for Your VRAM?

For this guide, we'll be going with Qwen3.8-27B with 4-bit quantization on my system with an RTX 5090. Going up a tier is an option thanks to its 32GB of VRAM, but I wanted to allow for a larger [context window](https://www.gpupartner.com/reference/context-window) (how much of the conversation the model can keep in mind at once) rather than cramming in the largest model that fits.

![Bar chart of Qwen3.8-27B memory with a 128K context: 16-bit 61.0 GB, 8-bit 36.7, 6-bit 30.4, 5-bit 27.6, 4-bit 24.9, 3-bit 21.9, 2-bit 18.9. A line marks a 32 GB card; 6-bit and below fit under it.](https://cdn.gpupartner.com/prod/qwen3-8-27b-memory-by-precision-128k-context-chart.png)

Qwen3.8-27B at each precision with a 128K context, from our [VRAM calculator](https://www.gpupartner.com/tools/vram-calculator/qwen3.8-27b). At 4-bit, the model and the context fit a 32 GB card with room to spare.

If you want to run a model of this caliber with excellent performance, a hilariously overpriced RTX 5090 (remember when $1,999 felt too high?) is unfortunately still not a terrible 'deal' these days. The other option is way more expensive [enterprise-grade GPUs](https://www.gpupartner.com/inventory/gpu).

If you're on a GPU with less VRAM, you can still get something very usable going right away. The table below gives you a rough idea of where you can start.

| **Size**    | **VRAM** | **Models to try**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | **Version (quant)**     | **Context size** | **Cards at this size**                                                                                                                                                                                                                                                                                                                                                                                                                          |
| ----------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------- | ---------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Small       | 8-12 GB  | [Qwen3.5-9B](https://www.gpupartner.com/tools/vram-calculator/qwen3.5-9b), [Gemma 4 12B](https://www.gpupartner.com/tools/vram-calculator/gemma-4-12b)                                                                                                                                                                                                                                                                                                                                                                                                                  | UD-Q4\_K\_S or Q4\_K\_M | 8K-16K           | [RTX 5060 8GB](https://www.gpupartner.com/tools/vram-calculator/machines/rtx-5060), [RTX 3060 12GB](https://www.gpupartner.com/tools/vram-calculator/machines/rtx-3060-12gb), [RTX 5070 12GB](https://www.gpupartner.com/tools/vram-calculator/machines/rtx-5070)                                                                                                                                                                            |
| Medium      | 16 GB    | [Qwen3-14B](https://www.gpupartner.com/tools/vram-calculator/qwen3-14b), [gpt-oss-20b](https://www.gpupartner.com/tools/vram-calculator/gpt-oss-20b) (MoE), [Gemma 4 26B](https://www.gpupartner.com/tools/vram-calculator/gemma-4-26b-a4b) (MoE), [Qwen3-Coder-30B](https://www.gpupartner.com/tools/vram-calculator?m=qwen3-coder-30b-a3b-instruct&hw=rtx-5060-ti-16gb) (MoE), [Qwen3.6-35B](https://www.gpupartner.com/tools/vram-calculator?m=qwen3.6-35b-a3b&hw=rtx-5060-ti-16gb) (MoE), [Inkling 2 Mini](https://unsloth.ai/docs/models/inkling-2-mini) (MoE) | Q4\_K\_M or Q5\_K\_M    | 16K-32K          | [RTX 5060 Ti 16GB](https://www.gpupartner.com/tools/vram-calculator/machines/rtx-5060-ti-16gb), [RTX 5070 Ti](https://www.gpupartner.com/tools/vram-calculator/machines/rtx-5070-ti), [RTX 5080](https://www.gpupartner.com/tools/vram-calculator/machines/rtx-5080), [RTX PRO 2000](https://www.gpupartner.com/inventory/gpu/pny-nvidia-rtx-pro-2000-blackwell-16gb-gddr7)                                                                 |
| Large       | 24 GB    | [Gemma 4 31B](https://www.gpupartner.com/tools/vram-calculator/gemma-4-31b), [Qwen3.8-27B](https://www.gpupartner.com/tools/vram-calculator/qwen3.8-27b), [Inkling 2 Mini](https://unsloth.ai/docs/models/inkling-2-mini)                                                                                                                                                                                                                                                                                                                                              | UD-Q4\_K\_M or Q4\_K\_S | 16K-32K          | [RTX 3090](https://www.gpupartner.com/tools/vram-calculator/machines/rtx-3090), [RTX 4090](https://www.gpupartner.com/tools/vram-calculator/machines/rtx-4090), [RX 7900 XTX](https://www.gpupartner.com/tools/vram-calculator/machines/rx-7900-xtx), [RTX PRO 4000](https://www.gpupartner.com/inventory/gpu/pny-nvidia-rtx-pro-4000-blackwell-24gb-gddr7)                                                                                 |
| Extra Large | 32 GB+   | [Qwen3.8-27B](https://www.gpupartner.com/tools/vram-calculator/qwen3.8-27b), [Inkling 2 Mini](https://unsloth.ai/docs/models/inkling-2-mini)                                                                                                                                                                                                                                                                                                                                                                                                                            | UD-Q4\_K\_XL or Q6\_K   | 64K-128K+        | [RTX 5090 32GB](https://www.gpupartner.com/tools/vram-calculator/machines/rtx-5090), [RTX PRO 4500 32GB](https://www.gpupartner.com/inventory/gpu/pny-nvidia-rtx-pro-4500-blackwell-32gb-gddr7), [RTX PRO 5000 48GB](https://www.gpupartner.com/inventory/gpu/pny-nvidia-rtx-pro-5000-blackwell-48gb-gddr7), [RTX PRO 6000 96GB](https://www.gpupartner.com/inventory/gpu/pny-nvidia-rtx-pro-6000-blackwell-workstation-edition-96gb-gddr7) |

Models marked MoE ([mixture of experts](https://www.gpupartner.com/reference/mixture-of-experts)) only run a few billion of their parameters for each token. They keep a decent speed on a smaller card with the rest held in system RAM, which is why a 35B model sits in the 16 GB row. On an RTX 5060 Ti 16GB, [our calculator](https://www.gpupartner.com/tools/vram-calculator?m=qwen3.6-35b-a3b&hw=rtx-5060-ti-16gb) puts Qwen3.6-35B at about 47 tokens per second this way.

Feel free to experiment with several models until you find one that fits your needs. You can also use [GPUPartner's VRAM Calculator](https://www.gpupartner.com/tools/vram-calculator) to get an estimate.

### Can You Run Inkling 2 on an RTX 5090?

Inkling 2 Mini, yes. Inkling 2 Small, no.

[Inkling 2 Mini](https://unsloth.ai/docs/models/inkling-2-mini) is the newest model in this size class. Thinking Machines Lab released it on November 17, 2026: 26.4B parameters, Apache 2.0, vision support and a 1M-token context. Unsloth's recommended file, UD-Q4\_K\_XL, is 17.6 GB. That is within 0.1 GB of the Qwen3.8-27B file in this guide, so it sits on a 32GB card the same way and leaves about 14GB for context.

Nothing else in this guide changes. Update Unsloth Desktop first, because Inkling 2 needs a recent build, then search for Inkling 2 Mini in the same drop-down in Step 2.

| **Inkling 2 Mini**  | **Memory needed** | **Fits entirely on** |
| ------------------- | ----------------- | -------------------- |
| 2-bit               | 10-11 GB          | 12 GB card           |
| 3-bit               | 13-15 GB          | 16 GB card           |
| 4-bit (recommended) | 17-18 GB          | 24 GB card           |
| 8-bit (lossless)    | 24 GB             | 32 GB card           |

Those figures are Unsloth's. They count RAM and VRAM together and cover the model alone, so a long context needs more. Inkling 2 Mini is a mixture-of-experts model with 5.4B parameters active, so the 4-bit file also runs on a 16 GB card with the remainder in system RAM, a little slower.

[Inkling 2 Small](https://unsloth.ai/docs/models/inkling-2-small) is small in name only. It is a 594B model, and the smallest file Unsloth publishes is 171.7 GB. That is unified-memory territory: a [256GB Mac Studio](https://www.gpupartner.com/inventory/systems/apple-mac-studio-m3-ultra-256gb-2tb) or a [DGX Station](https://www.gpupartner.com/inventory/systems/nvidia-dgx-station-1x-nvidia-gb300-grace-blackwell-ultra-desktop-superchip), not a graphics card.

### Can You Run a Local AI on a Mac?

Yes. You'll find good alternatives in [Apple's Mac lineup](https://www.gpupartner.com/inventory/systems/apple-mac-studio-m3-ultra-256gb-2tb) thanks to its [unified memory](https://www.gpupartner.com/reference/unified-memory). Macs are pretty good at running larger models, even if they don't offer the best performance. Just make sure you grab the MLX flavor that is custom-built for Apple Silicon.

The same trade applies to unified-memory PCs like the [MSI PRO MAX EDGE AI+ 11M](https://www.gpupartner.com/inventory/systems/msi-pro-max-edge-ai-plus-11m-128gb) and NVIDIA's [DGX Spark](https://www.gpupartner.com/inventory/systems/nvidia-dgx-spark-128gb-4tb).

## How Do You Set Up a Local AI with Unsloth Desktop?

It takes three steps: install Unsloth Desktop, download your model, and set the context size. I'll go over these steps in more detail below so you can follow along.

### Step 1: Install Unsloth Desktop

First, we'll need a solution that 'serves' the AI model to you. My pursuit of simplicity narrowed my choices down to LM Studio or Unsloth Desktop. Both abstract away a ton of terminal commands and handle Python environments and dependencies behind a nice, simple UI, which makes them excellent picks for a first home setup.

I landed on Unsloth Desktop mostly because, well, I liked the name better. Also, it didn't try to push the very thing I was trying to escape when I opened its website: a subscription.

The community seems to like it overall too, so that's another point in its favor.

Once you've clicked through the installer and launched Unsloth, you'll see a window like this one.

![Unsloth Desktop home screen on first launch: sidebar with New chat, Model hub and Library, a Select model drop-down, and an empty chat box.](https://cdn.gpupartner.com/prod/unsloth-desktop-first-launch-home-screen.png)

Unsloth Desktop on first launch, before any model is loaded.

### Step 2: Download a Quantized Model

On the top left of this main window, you can pick the model that you want to use from the drop down. We'll be using Qwen3.8-27B.

![Unsloth Desktop model picker open on the Recommended tab, with Qwen3.8-27B-GGUF loaded at the top of the list.](https://cdn.gpupartner.com/prod/unsloth-desktop-model-picker-recommended-list.png)

The model picker. Qwen3.8-27B is first on the Recommended list.

You should see a submenu like the one below when you click on a model. I'll be using Unsloth's UD-Q4\_K\_XL version. The M and S versions are smaller, but the XL version behaves much closer to the full-size model without filling up our VRAM, so it seemed like a good pick for my GPU.

![Qwen3.8-27B quantization list in Unsloth Desktop, UD-Q4_K_XL marked downloaded at 18 GB with a gear icon beside it.](https://cdn.gpupartner.com/prod/unsloth-desktop-quantization-list-ud-q4-k-xl.png)

Every quantization of Qwen3.8-27B with its download size. The gear next to the downloaded one holds the context setting.

### Step 3: Set the Context Size

Once the download completes, you should see a gear icon to the right of the model as shown in the screenshot above. Here, you can set your context size manually or leave it on Auto if you aren't sure how much you need.

I'll be setting mine to 128K (unquantized) and might reassess after I've run the model through a few of my routine tasks for a few weeks. Again, you can use this [VRAM calculator](https://www.gpupartner.com/tools/vram-calculator/qwen3.8-27b) to help you with these numbers for [your specific GPU](https://www.gpupartner.com/tools/vram-calculator/machines).

At this point, our first local AI is up and running, and you can start chatting with it right inside Unsloth.

Remember, your mileage may vary depending on your setup.

## How Do You Share a Local AI Across Your Home Network?

Short answer: Unsloth Desktop has LAN sharing built in. Set a Remote password, start LAN access, and share the address it gives you with anyone on your home network.

Once Unsloth Desktop is set up with a model, you might want to give family members or friends access too. For now, we'll only serve it over our local network, so my family can use it without needing physical access to this PC.

Head into Unsloth Desktop settings (gear icon on bottom left), and look for the 'Remote & LAN' tab.

![Unsloth Desktop Remote & LAN settings, showing the Remote access and LAN access cards, each with a Start button.](https://cdn.gpupartner.com/prod/unsloth-desktop-remote-lan-access-settings.png)

Settings, Remote & LAN. LAN access is the second card. The Remote access card above it is for a later part.

Most of this section will be greyed out until you set a Remote password. Once you've set a password, you'll see a 'Start' button for LAN access. Click that and you should see an address that anyone on your home network can open in a browser to use a local AI model within a chat interface. They'll sign in as 'unsloth' with the Remote password you set.

Just keep in mind that the PC needs to stay on with Unsloth running for anyone to reach it.

## Can You Access Your Local AI Across the Internet? Soon™

Not with this setup alone. Everything so far is limited to your local home network.

If you just need a chat interface like ChatGPT, Gemini, etc. to work within your home network, you're basically done at this point! You and anyone on your network can start talking to an AI model hosted on your machine right away.

In the next parts, we'll cover performance, tunneling, as well as setting up an agent that runs locally and uses only the web tools you allow it to use. This will let you automate more complex tasks like coding, research, and so much more without being tied to a subscription or usage limits and censorship.

As always, my focus remains simplicity that allows my family to use it without too much guidance or help.

PS. If you have any recommendations for agentic harnesses or even models to try, feel free to [let me know](https://www.gpupartner.com/contact) and I'll give them a go!

## Sources

| **Source**                                                                                                  | **Details**                                                                            |
| ----------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------- |
| [Unsloth Desktop documentation](https://unsloth.ai/docs/desktop)                                           | Free, open-source app for macOS, Windows and Linux; runs GGUF and MLX models           |
| [Unsloth Desktop: LAN access](https://unsloth.ai/docs/desktop/lan)                                         | Remote & LAN settings, LAN access start and port behaviour                             |
| [Qwen3.8-27B model card](https://huggingface.co/Qwen/Qwen3.8-27B)                                          | Official 16-bit weights (~56 GB) and parameter count                                   |
| [Unsloth Qwen3.8-27B GGUF](https://huggingface.co/unsloth/Qwen3.8-27B-GGUF)                                | Quantized files and sizes, including UD-Q4\_K\_XL (~18 GB)                             |
| [Unsloth: Inkling 2 Mini run-locally guide](https://unsloth.ai/docs/models/inkling-2-mini)                 | 26.4B parameters, Apache 2.0, 1M context, UD-Q4\_K\_XL at 17.6 GB, memory by precision |
| [Unsloth: Inkling 2 Small run-locally guide](https://unsloth.ai/docs/models/inkling-2-small)               | 594B parameters, smallest file 171.7 GB                                                |
| [Thinking Machines Lab: Inkling-2-Mini model card](https://huggingface.co/thinkingmachines/Inkling-2-Mini) | Official weights, released November 17, 2026                                           |
| [NVIDIA GeForce RTX 5090](https://www.nvidia.com/en-us/geforce/graphics-cards/50-series/rtx-5090/)         | 32 GB GDDR7 consumer GPU used for this build                                           |
