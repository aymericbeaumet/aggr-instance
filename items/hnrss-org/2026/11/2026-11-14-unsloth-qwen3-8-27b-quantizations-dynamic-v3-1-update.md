---
title: Unsloth Qwen3.8-27B Quantizations – Dynamic v3.1 Update
link: https://unsloth.ai/docs/models/qwen3.8
source: hnrss-org
published: 2026-11-14T00:06:04Z
updated: 2026-11-14T00:06:04Z
first_seen: 2026-11-14T09:19:50.800387300Z
authors:
- Retro_Dev
content: extracted
html: 2026-11-14-unsloth-qwen3-8-27b-quantizations-dynamic-v3-1-update.html
remote_preview:
  url: https://unsloth.ai/docs/~gitbook/image?url=https%3A%2F%2F2815821428-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Forganizations%252FHpyELzcNe0topgVLGCZY%252Fsites%252Fsite_mXXTe%252Fsocialpreview%252FOeFDVbzp4cgwaId7PbvL%252Funsloth%2520docs%2520pic.png%3Falt%3Dmedia%26token%3Df34a6bcc-db15-449a-bc2d-e0e8f52fe953&width=1200&height=630&sign=9a9d6aae5e35625d4a32adaf8cf74290&sv=3
---

Qwen3.8-27B is a 27B parameter dense model Qwen, featuring **vision** and reasoning capabilities, a **256K context** window, and runs locally on **17GB RAM/VRAM** setups. Qwen3.8 excels at agentic coding, vision and chat tasks, and can now run via Unsloth GGUFs, NVFP4 and [Unsloth Desktop](https://unsloth.ai/docs/models/qwen3.8#run-qwen3.8-in-unsloth-desktop).

**Nov 11 Update**: Qwen3.8-27B GGUFs now use [Unsloth Dynamic V3.1](https://unsloth.ai/docs/basics/dynamic-3.0-ggufs) which improves average accuracy by 5-10% accuracy at comparable sizes including Terminal Bench, DeepSWE and more benchmarks. We're working on an even better version of Dynamic quants which will be applied to **Qwen4** and future models.

[Run Qwen3.8 Guide](https://unsloth.ai/docs/models/qwen3.8#run-qwen3.8-guide)\
[Download Unsloth](https://unsloth.ai/download)

Thank you Qwen for day zero access. Unsloth quants also include:

- **Developer Role Support** for agentic tools like Codex

-
- **Tool calling:** Improved parsing nested objects to make tools succeed more

### Dynamic V3.1 Update (November 11th 2026)

We're excited to release new Qwen3.8-27B GGUFs using our updated [**Unsloth Dynamic**](https://unsloth.ai/docs/basics/dynamic-3.0-ggufs) **V3.1 (preview) algorithm** which improves Unsloth quants by 5-10% accuracy at the same size on most benchmarks and is the best in class for most tasks including coding, agentic workloads and general chat.

Most quants from 2bit until 6bit are updated. We found 3bit UD3 quants to do on par with UD3.1, so we do not update them. We are actively working on making all quants better!

**We ran agentic benchmarks including Terminal Bench 2.1, DeepSWE v1.1**, and general benchmarks like LiveCodeBench v6, GPQA Diamond, MMLU-Pro, AIME25 and many others. We used 256K context lengths for all agentic benchmarks, and 128K for the rest. We followed the same methodology for Qwen3.8-27B as [DeepSeek-V4.1-Flash](https://unsloth.ai/docs/models/deepseek-v4.1-flash) and [Qwen3.8-Flash-Next](https://unsloth.ai/docs/models/qwen3.8-next) which also utilized our V3.1 preview algorithm.

#### Dynamic V3.1 Full Breakdown

Click below to see full details of benchmarking and more:

Click to expand Full details and breakdown

#### Exllama-v3 considerations

To compare GSQ-RCO and Exllama-v3, we had to exclude the token embedding layer from the x axis, as Exllama-v3 uses BF16 for it.

If you consider the full download size and total RAM usage for Exllama-v3, then adding the BF16 embedding table and including all embedding sizes changes the plots. Our main plots's x axis removes the embedding layer sizes from all quant providers. **However on pure RAM usage and disk space usage vs accuracy, the below plot is the true reflection of actual usage.**

We have not investigated why Exllama-v3 keeps token embeddings in BF16, but as the lm\_head is still quantized heavily, the embedding layer might have "absorbed" most of the quantization damage from it. We would be glad to benchmark new Exllama-v3 non BF16 token embedding quants like using 8-bit for example, and see if benchmarks change.

#### GSQ-RCO results are misleading

For LiveCodeBench, GSQ-RCO showed Unsloth UD3 quants doing consistently worse than ISTA's. However we **found the opposite - UD3 quants (our OLD quants) do better than ISTA on LCB on all sizes except the smallest.** UD3.1 further improves on LCB as well.

We approximate that GSQ-RCO used a context budget of 96K, as that best fits the data for the runs we have. At 96K, GSQ-RCO does somewhat do better than UD3, but at 128K, UD3 does much better.

On another benchmark, for ISTA GSQ-RCO's AIME25 benchmarks, their quants reach 100%, 100%, 100% and 96.7% accuracy recovery. And they said our quants do worse.

Through out investigations, we estimate ISTA only did 1 trial - we did 14 trials, and we show on average the recovery rate is 93% to 96% - not 100% accuracy recovery. **And in fact our UD3 old quants do BETTER on average than their quants on 14 runs vs 1 run of theirs**, so we draw the opposite conclusion if you run the trials more.

We also checked and graded our answers using Math Verify [Dynamic 3.1 GGUFs](https://unsloth.ai/docs/basics/dynamic-3.0-ggufs/dynamic-3.1-ggufs) and also LightEval and we find exactly the same conclusions. We also tried greedy decoding (temperature of 0), and this could not get the 100% accuracy recovery ISTA found across multiple greedy runs, as GPUs themselves are non deterministic. That is why you benchmarks should be run multiple times. Greedy decoding is also not representative of real world use cases.

We also checked 96K (which is most likely ISTA's budget) and 128K, and our quants still do on average better.

#### KLD of Unsloth v3.1 vs v3

We did KLD for multiple domains for Unsloth UD3.1 quants and UD3 quants - we reduce KLD throughout most quants and most domains - we did not replace the 3-bit ones, as we found UD3 to do mostly great, and the accuracy recovery on benchmarks was not significant enough with UD3.1 to warrant an update, so we did not update them.

#### Benchmark details and setups

We follow the same procedure for all benchmarks as for Qwen3.8-Flash-Next, DeepSeek-V4.1-Flash as well as discussed in [Dynamic 3.1 GGUFs](https://unsloth.ai/docs/basics/dynamic-3.0-ggufs/dynamic-3.1-ggufs). We used temperature 1.0, top\_p 0.95, top\_k 20, min\_p 0, no presence or repetition penalty, preserve\_thinking and xhigh reasoning for Qwen3.8-27B.

### ⚙️ Usage Guide

#### Qwen3.8-27B Requirements:

Qwen3.8-**27B** 4-bit quants work on 16-19GB VRAM like RTX 5080, 4090 or a Mac with 24GB RAM. **Table: Hardware requirements** (units = total memory: RAM + VRAM, or unified memory)

If you want to use [MTP](https://unsloth.ai/docs/models/mtp) for faster inference, prepare to have 1-2GB extra headroom.

### Recommended Settings

Qwen3.8-27B is a **hybrid thinking** model with different default settings for thinking and non-thinking modes. Extra high is enabled by default so if you want shorter thinking traces, you can [adjust the thinking effort](https://unsloth.ai/docs/models/qwen3.8#thinking--preserve-thinking):

- **Maximum context window:** `262,144` (can be extended to 1M via YaRN)

- Thinking Mode: `temperature=1.0`, `top_p=0.95`, `top_k=20`, `min_p=0.0`, `presence_penalty=0.0`, `repetition_penalty=1.0`

- Instruct (or non-thinking) mode: `temperature=0.7`, `top_p=0.80`, `top_k=20`, `min_p=0.0`, `presence_penalty=1.5`, `repetition_penalty=1.0`

### 💡 Thinking + Preserve Thinking

Qwen3.8 has **Preserve Thinking** which leaves the thinking trace from the previous conversation. This increases the number of tokens you use, but could increase accuracy in continued conversations. [Unsloth](https://unsloth.ai/docs/models/qwen3.8#run-qwen3.8-in-unsloth-desktop) has 'Think' and Preserved Thinking toggles for Qwen3.8 (see right):

Qwen3.8-27B comes with support for `reasoning_effort`, which can be used to adjust reasoning depth and control cost. These toggles are automatically enabled in Unsloth:

- `xhigh` (default): for complex tasks demanding thorough analysis

- `medium`: balancing accuracy and speed

- `low`: efficient reasoning optimizing for speed and cost

- none

To change [thinking / reasoning](https://unsloth.ai/docs/models/qwen3.8#how-to-enable-or-disable-reasoning-and-thinking) effort in `unsloth run` or `llama-server`, use `--chat-template-kwargs '{"reasoning_effort":"medium"}'`

If you're on **Windows** Powershell, use: `--chat-template-kwargs "{\"reasoning_effort\":\"medium\"}"`

Change `medium` to your desired reasoning level.

## Run Qwen3.8 Guide

You can now run Qwen3.8-27B in llama.cpp and Unsloth Desktop. Feel free to change quantization type.

[Run in Unsloth Desktop](https://unsloth.ai/docs/models/qwen3.8#run-qwen3.8-in-unsloth-desktop)\
[Run in llama.cpp](https://unsloth.ai/docs/models/qwen3.8#run-qwen3.8-in-llama.cpp)\
[NVFP4 Guide](https://unsloth.ai/docs/models/qwen3.8#run-qwen3.8-in-llama.cpp)\
[Qwen3.8-2.4T-A95B Guide](https://unsloth.ai/docs/models/qwen3.8#qwen3.8-2.4t-a95b)

### 🦥 Run Qwen3.8 in Unsloth Desktop

Qwen3.8 can run in [Unsloth Desktop](https://unsloth.ai/docs/models/qwen3.8#run-qwen3.8-in-unsloth-desktop), an open-source UI app for local AI. **Unsloth automatically offloads to RAM and detects multiGPU setups**. With Unsloth Desktop, you can run models locally on **MacOS, Windows**, Linux and:

- Search, download, [run GGUFs](https://unsloth.ai/docs/new/studio#run-models-locally), MLX and safetensor models

-
-
-
- Fast CPU + GPU inference via MLX and llama.cpp

-

1

#### Install Unsloth

[Download Unsloth](https://unsloth.ai/download)

Or, if you prefer to install manually:

**MacOS, Linux, WSL:**

**Windows:**

Install Unsloth Desktop:

Update Unsloth Desktop:

Install Unsloth Desktop:

Update Unsloth Desktop:

2

#### Search and download Qwen3.8

Go to [Unsloth Chat](https://unsloth.ai/docs/new/studio/chat) or Model hub and search for Qwen3.8 in the search bar and download your desired model and quant.

3

#### Run Qwen3.8

Inference parameters should be auto-set when using Unsloth, however you can still change it manually. You can also edit the context length, chat template and other settings.

For example using Unsloth Desktop with the 397GB Qwen3.8 (-91% smaller) allows you to toggle thinking modes, allow inline canvas, web search and code execution and much more.

4

#### Serve Qwen3.8 with Unsloth API

You can use `unsloth run` command and serve Qwen3.8 via an API using `llama-server` runtime flags, including context sizing, GPU layers, threading, sampling, networking, and tool configuration. For more info see our [API docs](https://unsloth.ai/docs/basics/api) or [unsloth start](https://unsloth.ai/docs/integrations/unsloth-start).

5

#### Unsloth is now ready

You can also do many other things with Qwen3.8 via Unsloth Desktop like:

-
- **Train models:** Fine-tune text, diffusion, [embedding](https://unsloth.ai/docs/basics/embedding-finetuning), and more

- **Generate media:** Create and train [images](https://unsloth.ai/docs/basics/diffusion-image), video, [TTS](https://unsloth.ai/docs/basics/text-to-speech-tts-fine-tuning) locally

### 🦙 Run Qwen3.8 in llama.cpp

1

Download the latest version of llama.cpp. You can follow the build instructions below as well. Change `-DGGML_CUDA=ON` to `-DGGML_CUDA=OFF` if you don't have a GPU or just want CPU inference. **For Apple Mac / Metal devices**, set `-DGGML_CUDA=OFF` then continue as usual - Metal support is on by default.

2

3

To run the model in llama-cli, follow the code snippets below: Remember to [change settings](https://unsloth.ai/docs/models/qwen3.8#recommended-settings) according to your use-case.

### ⚡️NVFP4

Like Qwen3.6, we’re also releasing new [dynamic NVFP4 Qwen3.8](https://unsloth.ai/docs/basics/nvfp4)-27B quants that run **\~1.5× faster** than BF16 checkpoints, with **better performance** and comparable file sizes. Run Qwen3.8-27B NVFP4 **1.5x faster** on **24GB VRAM**. We also added **FP8 KV cache calibration** for 2x longer context lengths! NVFP4 requires NVIDIA's Blackwell GPUs like RTX 50X, DGX Spark (see [Qwen3.8-27B](https://unsloth.ai/docs/models/qwen3.8#dgx-spark-with-nvfp4-quants)), B200, B300 GPUs. For older GPUs, our GGUFs work well! You can run NVFP4 quants in [vLLM](https://unsloth.ai/docs/models/qwen3.8#vllm) only for now (SGLang is not supported).

See below for previous benchmarks conducted for Qwen3.6 as well comparing to other NVFP4 implementations which use 16bit activations vs our NVFP4 activations:

All benchmarks use 1x B200 128 concurrency. Higher concurrency can boost 35B to 17,561 tokens / s.

For accuracy benchmarks we ran KLD and Top-1 agreement over Code, Chat and many domains. NVFP4 is consistency 92% to 97% accuracy recovery vs BF16

For accuracy benchmarks For Qwen 3.6, we conducted MMLU-Pro, AIME 2025, GPQA for FP8, BF16, NVIDIA's NVFP4 and our NVFP4s - we show our faster quants do similarly on all:

To run NVFP4 quants, see below for commands to run Qwen3.8-27B in [vLLM](https://unsloth.ai/docs/basics/inference-and-deployment/vllm-guide) or [SGLang](https://unsloth.ai/docs/basics/inference-and-deployment/sglang-guide):

#### **vLLM:**

To install vLLM in a separate venv:

Then to serve the 27B variant:

To enable MTP / speculative decoding (faster decode but somewhat less throughput), use:

If you get Torchcodec issues, be sure to do the below then relaunch vllm.

#### **SGLang:**

If using SGLang, you must use **SGLang version v0.5.19** otherwise it will not work since we quantize the lm\_head to FP8.

vLLM has a `CompressedTensorsW8A8Fp8` kernel which supports this, whilst SGLang v0.5.19 cannot load the FP8 lm\_head. v0.5.19 works now!

For sglang after installing the latest:

You can then serve it:

To enable MTP, use:

### 🤯Quantization Analysis

NVFP4 quants are 1.5x faster than BF16 and retains 92 to 97% top-1 accuracy.

Top-1 accuracy plot as shown in UD-3 fro Qwen3.8-27B:

And mean KLD for Qwen3.8:

### 📊 Benchmarks

#### Qwen3.8-**27B**

See further below for table benchmarks:

#### Text Performance

Click to expand full table

## **Qwen3.8-2.4T-A95B**

Qwen3.8-**2.4T**-A95B is a 2.4T parameter (95B active) model with rivaling GPT-5.6 Sol. Full-precision Qwen3.8-2.4T-A95B requires 4.9TB of storage and 1-bit [Unsloth](https://github.com/unslothai/unsloth) Dynamic GGUFs takes **397GB (91% smaller)**, and larger IQ1\_S takes 508GB.

### ⚙️ Usage Guide

#### Qwen3.8-**2.4T** Requirements:

### Recommended Settings

Qwen3.8-2.4T is **thinking-only**, while Qwen3.8-Max is hybrid.

- Context length = up to `1,010,000`

- `temperature=1.0`, `top_p=0.95`, `top_k=20`, `min_p=0.0`, `presence_penalty=0.0`, `repetition_penalty=1.0`

If the model fits, you will get ~20 tokens/s generation when using B200s and >120 tokens / s throughput. Best rule of thumb: RAM+VRAM ≈ the quant size; otherwise it’ll still work, just much slower due to disk offloading.

### Run Qwen3.8-2.4T Guide

You can now run Qwen3.8 in llama.cpp and Unsloth Desktop. For the large Qwen3.8-2.T model, we will be utilizing the 397GB `IQ1_XXXS` quant (named Q1\_0) for best results in terms of accessibility and accuracy and it will require at least 450GB RAM. Feel free to change quantization type.

[Run in Unsloth Desktop](https://unsloth.ai/docs/models/qwen3.8#run-qwen3.8-in-unsloth-desktop)\
[Run in llama.cpp](https://unsloth.ai/docs/models/qwen3.8#run-qwen3.8-in-llama.cpp)

### 🦥 Run in Unsloth Desktop

Qwen3.8 can run in [Unsloth Desktop](https://unsloth.ai/docs/models/qwen3.8#run-qwen3.8-in-unsloth-desktop), an open-source UI app for local AI. **Unsloth automatically offloads to RAM and detects multiGPU setups**. With Unsloth Desktop, you can run models locally on **MacOS, Windows**, Linux. Go to [Unsloth Chat](https://unsloth.ai/docs/new/studio/chat) or Model hub and search for Qwen3.8 in the search bar and download your desired model and quant. Then run it.

### Qwen3.8-2.4T-A95B New 1-bit data-types

We extended IQ1\_S in llama.cpp which is 1.5625 bits per weight to 1.1875 bpw by reducing the number of entries in the codebook - we found this works well for large models, and can still retain a lot of accuracy - we also found these new data-types to be fine for post training quantization (PTQ) without the need for QAT or QAD (quantization aware training / distillation). [Qwen3.8-**2.4T-A95B**-GGUF](https://huggingface.co/unsloth/Qwen3.8-2.4T-A95B-GGUF)

Click for detailed analysis

Due to naming issues, we used TQ2\_0, TQ1\_0 and Q1\_0 otherwise it won't pop up in the HF repo.

We are still running benchmarks for the new data-types, but for other large models, we get **good results without any QAT / QAD**:

### 🦙 Run Qwen3.8-2.4T in llama.cpp

1

We need to use the specific IQ1\_XXXS branch [here](https://github.com/unslothai/llama.cpp/pull/61). You can follow the build instructions below as well. Change `-DGGML_CUDA=ON` to `-DGGML_CUDA=OFF` if you don't have a GPU or just want CPU inference. **For Apple Mac / Metal devices**, set `-DGGML_CUDA=OFF` then continue as usual - Metal support is on by default.

2

Download the model via (after installing `pip install huggingface_hub`). You can choose `Q1_0` for `IQ1_XXXS` or other quantized versions like `Q8_0` . If downloads get stuck, see: [Hugging Face Hub, XET debugging](https://unsloth.ai/docs/basics/troubleshooting-and-faqs/hugging-face-hub-xet-debugging)

3

To run the model in llama-cli, follow the code snippets below. Remember to [change settings](https://unsloth.ai/docs/models/qwen3.8#recommended-settings) according to your use-case.

4

To run the general UD-IQ1\_S you can do:

5

Then to run it:

#### Qwen3.8-**2.4T Benchmarks**
