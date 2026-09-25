---
title: Ollaya – Ollama for open-source, Jev-style decision models
link: https://ollaya.dev/
source: hnrss-org-frontpage
published: 2026-09-25T18:33:50Z
updated: 2026-09-25T18:33:50Z
first_seen: 2026-09-25T21:48:34.486565621Z
authors:
- Ardakilic
summary: 'Article URL: https://ollaya.dev/ Comments URL: https://news.ycombinator.com/item?id=49848269 Points: 209 # Comments: 59'
content: extracted
html: 2026-09-25-ollaya-ollama-for-open-source-jev-style-decision-models.html
preview:
  file: 2026-09-25-ollaya-ollama-for-open-source-jev-style-decision-models.preview-b6fe57af0e57.webp
  width: 256
  height: 134
  alt: Ollaya — run decision models locally
  color: '#171818'
images:
- source: https://ollaya.dev/static/og.png?v=7bd2eae221
  original:
    file: 2026-09-25-ollaya-ollama-for-open-source-jev-style-decision-models.image-247c3494d4de.png
    width: 1200
    height: 630
  color: '#0a0a0a'
---

Ask typed questions about any text or JSON and get calibrated answers in milliseconds. Private, open source, on your own hardware.

[Download](https://ollaya.dev/download)\
[Browse models](https://ollaya.dev/search)

```
ollaya run laya --preset triage \
```

```
"I was charged twice this month and want a refund."
```

Answers returned by the model
| Question          | Answer                   | Probability |
| ----------------- | ------------------------ | ----------- |
| intent            | refund                   | 1.00        |
| is\_urgent        | no                       | 0.87        |
| frustration       | 1.59 / 3 clearly annoyed | 0.36        |
| refund\_requested | yes                      | 0.88        |
| churn\_risk       | no                       | 0.89        |

Real output: routed to laya:en, answered in 8.9 ms on an RTX 4090.

## Fast

Decisions in milliseconds.

A decision model answers in a single forward pass, with no token-by-token generation. On your own GPU, a five-question request to Laya takes about 10 ms, end to end through the HTTP API.

Every model, one scale · median latency, lower is better

- laya:multilingual 8.1 ms
- laya:en 9.6 ms
- gliclass 14.7 ms
- nli 20.4 ms
- decider:0.8b 155 ms
- decider:2b 190 ms
- TypeSafe Jev hosted API 236–276 ms

Ollaya: median of a five-question request through the HTTP API on an NVIDIA RTX 4090 (laya in fp16, the others in fp32). Jev: median request latency of the hosted API in third-party benchmarks ([AbdelStark/jev-benchmarks](https://github.com/AbdelStark/jev-benchmarks), [nibzard/decision-model-benchmark](https://github.com/nibzard/decision-model-benchmark)), which includes the network. Setups differ, so read it as an order-of-magnitude comparison.

## Drop-in compatible

Speaks TypeSafe's API.

Ollaya serves `/v1/systemone` and `/v1/models` with TypeSafe's request and response shapes. The official TypeSafe Python SDK 0.7.1 works unchanged against a local server.

Request

```
# Point the TypeSafe SDK at Ollaya
export TYPESAFE_BASE_URL=http://localhost:11435
export TYPESAFE_API_KEY=local        # any value works
export TYPESAFE_DEFAULT_MODEL=laya

# …or call the compatible endpoint directly
curl http://localhost:11435/v1/systemone -d '{
    "model": "laya",
    "state": "Can I get an invoice for last month?",
    "questions": {
      "intent": {
        "type": "choice",
        "instructions": "What does the customer want?",
        "criteria": {
          "invoice": "Needs an invoice or receipt",
          "refund": "Wants money back",
          "other": "Anything else"
        }
      }
    }
  }'
```

Response

```
{
  "model": "laya:en",
  "answers": {
    "intent": {
      "type": "choice",
      "choice": "invoice",
      "confidence": 0.9547,
      "probabilities": {
        "invoice": 0.9698,
        "refund": 0.0172,
        "other": 0.013
      }
    }
  },
  "usage": {
    "input_tokens": 43,
    "output_tokens": 0
  }
}
```

[TypeSafe compatibility guide](https://ollaya.dev/docs/typesafe-compatibility)

## Open models

Open weights, ready to pull.

Start with Laya from Convai Innovations: an English model, a 100+ language model, a model fine-tuned for typed decisions, and a router that picks for you.

## Your data stays yours

Private by default.

Tickets, emails and user messages are often the most sensitive data you have. With Ollaya they are scored where they already live.

## Platforms

Runs where you work.

A desktop app and a command line for macOS, Windows and Linux, and a Docker image for servers. Every model runs on the CPU; an NVIDIA GPU on Linux, in WSL 2 or in Docker takes a request down to milliseconds.

| Platform                                     | Desktop app                                 | Command line                           | GPU |
| -------------------------------------------- | ------------------------------------------- | -------------------------------------- | --- |
| Desktop app Menu bar app .dmg                | Command line Install script                 | GPU CPU only                           |     |
| Desktop app Desktop app .exe or .msi         | Command line PowerShell script              | GPU CPU only NVIDIA via WSL 2          |     |
| Desktop app Desktop app AppImage, .deb, .rpm | Command line Install script systemd service | GPU NVIDIA, CUDA 13                    |     |
| Desktop app Not available                    | Command line Install script systemd service | GPU CPU only                           |     |
| Desktop app Not available                    | Command line Install script Same as Linux   | GPU NVIDIA, CUDA 13                    |     |
| Desktop app Not available                    | Command line Image on GHCR                  | GPU NVIDIA, CUDA 13 :cuda image, amd64 |     |

[Install for your platform](https://ollaya.dev/download)

NVIDIA GPUs need driver R580 or newer; the installers fetch the CUDA libraries only when they find one. On Apple, AMD and Intel GPUs, models run on the CPU.

## Get up and running in minutes.

One binary, one command: `ollaya run laya`.

[Download](https://ollaya.dev/download)

macOS, Windows, Linux and Docker · Apache-2.0 · [GitHub](https://github.com/ollaya-dev/ollaya)
