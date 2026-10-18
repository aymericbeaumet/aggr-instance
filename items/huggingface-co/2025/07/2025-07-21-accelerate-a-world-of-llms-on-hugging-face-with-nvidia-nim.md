---
title: Accelerate a World of LLMs on Hugging Face with NVIDIA NIM
link: https://huggingface.co/blog/nvidia/multi-llm-nim
source: huggingface-co
published: 2025-07-21T18:01:30Z
updated: 2025-07-21T18:01:30Z
first_seen: 2026-10-04T12:50:30.804479567Z
content: extracted
html: 2025-07-21-accelerate-a-world-of-llms-on-hugging-face-with-nvidia-nim.html
---

AI builders want a choice of the latest [large language models](https://www.nvidia.com/en-us/glossary/large-language-models/) (LLM) architectures and specialized variants for use in [AI agents](https://www.nvidia.com/en-us/glossary/ai-agents/) and other apps, but handling all the diversity can slow testing and deployment pipelines. In particular, managing and optimizing different inference software frameworks to achieve best performance across varied LLMs and serving requirements is a time-consuming bottleneck to getting performant AI apps in the hands of end-users.

NVIDIA AI customers and ecosystem partners leverage [NVIDIA NIM](https://developer.nvidia.com/nim) inference microservices to streamline deployment of the latest AI models on NVIDIA accelerated infrastructure, including LLMs, multi-modal and domain-specific models from NVIDIA, Meta, Mistral AI, Google and hundreds more innovative model builders. We’ve seen customers and partners deliver more innovation, faster, with a simplified, reliable approach to model deployment, and today we’re excited to unlock over 100,000 LLMs on Hugging Face for rapid, reliable deployment with NIM.

## A Single NIM Microservice for Deploying a Broad Range of LLMs

NIM now provides a single docker container for deploying a broad range of LLMs supported by leading inference frameworks from NVIDIA and the community including NVIDIA TensorRT-LLM, vLLM and SGLang. When an LLM is provided to the NIM container, it performs several steps for deployment and performance optimization, without manual configuration:

| LLM Adaptation Phase                      | What NIM Does                                                                                                                                                        |
| ----------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| *Model Analysis*                          | NIM automatically identifies the model's format, including Hugging Face models, TensorRT-LLM checkpoints, or pre-built TensorRT-LLM engines, ensuring compatibility. |
| *Architecture and Quantization Detection* | It identifies the model's architecture (e.g., Llama, Mistral) and quantization format (e.g., FP16, FP8, INT4).                                                       |
| *Backend Selection*                       | Based on this analysis, NIM selects an inference backend (NVIDIA TensorRT-LLM, vLLM, or SGLang).                                                                     |
| *Performance Setup*                       | NIM applies pre-configured settings for the chosen model and backend and then starts the inference server, reducing manual tuning efforts.                           |

Table 1. NVIDIA NIM LLM adaptation phases and functionality

The single NIM container supports common LLM weight formats, including:

- **Hugging Face Transformers Checkpoints**: LLMs can be deployed directly from Hugging Face repositories with`.safetensors` files, removing the need for complex conversions.
- **GGUF Checkpoints:** Quantized GGUF checkpoints for supported model architectures can be deployed directly from HuggingFace or from locally downloaded files
- **TensorRT-LLM Checkpoints**: Models packaged within a `trtllm_ckpt` directory, optimized for TensorRT-LLM, can be deployed.
- **TensorRT-LLM Engines**: Pre-built TensorRT-LLM engines from a `trtllm_engine` directory can be used for peak performance on NVIDIA GPUs.

## Getting Started

To use NIM, ensure your environment has NVIDIA GPUs with appropriate drivers (CUDA 12.1+), Docker installed, an [NVIDIA NGC](https://catalog.ngc.nvidia.com/) Account and API Key for NIM Docker images, and a Hugging Face account and API token for models requiring authentication. Learn more about environment prerequisites in the [NIM documentation](https://docs.nvidia.com/nim/large-language-models/latest/getting-started.html).

Environment setup involves setting environment variables and creating a persistent cache directory. Ensure the `nim_cache` directory has correct Unix permissions, ideally owned by the same Unix user launching the Docker container, to prevent permission issues. Commands use `-u $(id -u)` to manage this.

For ease of use, let’s store some of the frequently used information in environment variables.

```bash
# A variable for storing the NIM docker image specification
NIM_IMAGE=llm-nim
# Populate with your Hugging Face API token.
HF_TOKEN=<your_huggingface_token>
```

### Example 1: Deploying a Model

Deploying an LLM from Hugging Face is demonstrated with Codestral-22B:

```bash
docker run --rm --gpus all \
  --shm-size=16GB \
  --network=host \
  -u $(id -u) \
  -v $(pwd)/nim_cache:/opt/nim/.cache \
  -v $(pwd):$(pwd) \
  -e HF_TOKEN=$HF_TOKEN \
  -e NIM_TENSOR_PARALLEL_SIZE=1 \
  -e NIM_MODEL_NAME="hf://mistralai/Codestral-22B-v0.1" \
  $NIM_IMAGE
```

For locally downloaded models, point `NIM_MODEL_NAME` to the path and mount the directory:

```bash
docker run --rm --gpus all \
  --shm-size=16GB \
  --network=host \
  -u $(id -u) \
  -v $(pwd)/nim_cache:/opt/nim/.cache \
  -v $(pwd):$(pwd) \
  -v /path/to/model/dir:/path/to/model/dir \
  -e HF_TOKEN=$HF_TOKEN \
  -e NIM_TENSOR_PARALLEL_SIZE=1 \
  -e NIM_MODEL_NAME="/path/to/model/dir/mistralai-Codestral-22B-v0.1" \
  $NIM_IMAGE
```

While deploying a model, feel free to inspect the output logs to get a sense of the choices NIM made during model deployment. Deployed models are available at `http://localhost:8000`, with API endpoints at [`http://localhost:8000/docs`](http://localhost:8000/docs).

Additional arguments are available by the underlying engine. You can inspect the full list of such arguments by running nim-run --help in the container, as shown below.

```bash
docker run --rm --gpus all \
  --network=host \
  -u $(id -u) \
  $NIM_IMAGE nim-run --help
```

### Example 2: Specifying a Backend

To inspect compatible backends or choose a specific one, use `list-model-profiles`:

```bash
docker run --rm --gpus all \
  --shm-size=16GB \
  --network=host \
  -u $(id -u) \
  -v $(pwd)/nim_cache:/opt/nim/.cache \
  -v $(pwd):$(pwd) \
  -e HF_TOKEN=$HF_TOKEN \
  $NIM_IMAGE list-model-profiles --model "hf://meta-llama/Llama-3.1-8B-Instruct"
```

This command shows compatible profiles, including for LoRA adapters. To deploy with a specific backend like vLLM, use the `NIM_MODEL_PROFILE` environment variable, using the output supplied by `list-model-profiles`:

```bash
docker run --rm --gpus all \
  --shm-size=16GB \
  --network=host \
  -u $(id -u) \
  -v $(pwd)/nim_cache:/opt/nim/.cache \
  -v $(pwd):$(pwd) \
  -e HF_TOKEN=$HF_TOKEN \
  -e NIM_TENSOR_PARALLEL_SIZE=1 \
  -e NIM_MODEL_NAME="hf://meta-llama/Llama-3.1-8B-Instruct" \
  -e NIM_MODEL_PROFILE="e2f00b2cbfb168f907c8d6d4d40406f7261111fbab8b3417a485dcd19d10cc98" \
  $NIM_IMAGE
```

### Example 3: Quantized Model Deployment

NIM facilitates deploying quantized models. It automatically detects the quantization format (e.g., GGUF, AWQ) and selects the appropriate backend using standard deployment commands:

```bash
# Choose a quantized model and populate the MODEL variable, for example:
# MODEL="hf://modularai/Llama-3.1-8B-Instruct-GGUF"
# or
# MODEL="hf://Qwen/Qwen2.5-14B-Instruct-AWQ"
docker run --rm --gpus all \
  --shm-size=16GB \
  --network=host \
  -u $(id -u) \
  -v $(pwd)/nim_cache:/opt/nim/.cache \
  -v $(pwd):$(pwd) \
  -e HF_TOKEN=$HF_TOKEN \
  -e NIM_TENSOR_PARALLEL_SIZE=1 \
  -e NIM_MODEL_NAME=$MODEL \
  $NIM_IMAGE
```

For advanced users, NIM offers customization through environment variables such as `NIM_MAX_MODEL_LEN` for context length. For large LLMs, `NIM_TENSOR_PARALLEL_SIZE` enables multi-GPU deployment. Ensure `--shm-size=<shared memory size>` is passed to Docker for multi-GPU communication.

The NIM container supports a broad range of LLMs supported by NVIDIA TensorRT-LLM, vLLM and SGLang, including popular LLMs and specialized variants on Hugging Face. For more details on supported LLMs, see the [documentation](https://docs.nvidia.com/nim/large-language-models/latest/supported-llm-agnostic-architectures.html).

## Build with Hugging Face and NVIDIA

NIM is designed to simplify AI model deployment on NVIDIA accelerated infrastructure, speeding innovation and time to value for high performance AI builders and enterprise AI teams. We look forward to engagement and feedback from the Hugging Face Community.

Get started with a developer example in an NVIDIA-hosted computing environment at [build.nvidia.com](https://build.nvidia.com/nvidia/bring-llm-to-nim).
