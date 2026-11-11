---
title: 'Gemini Decisions API: Fast, calibrated multimodal decisions'
link: https://blog.google/innovation-and-ai/technology/developers-tools/gemini-decisions-api/
source: blog-google
published: 2026-11-11T18:00:00Z
updated: 2026-11-11T18:00:00Z
first_seen: 2026-11-11T21:51:32.649893059Z
labels:
- ai
- developer tools
summary: Meet the Gemini Decisions API, powered by our state-of-the-art model Gemini Decision for fast, calibrated multimodal decisions.
content: extracted
html: 2026-11-11-gemini-decisions-api-fast-calibrated-multimodal-decisions.html
remote_preview:
  url: https://storage.googleapis.com/gweb-uniblog-publish-prod/images/gemini__decisions__blog-header_2784x1566.width-1300.png
---

Introducing the Gemini Decisions API, powered by our new Gemini Decision 1 model. Evaluate up to 1,000 yes/no, choice, and rubric-scoring questions in a fast, single pass across text, images, audio, video, and PDFs.

* * *

Min Choi

Product Manager, Google DeepMind

Felix Yu

Principal Scientist, Google DeepMind

* * *

Today, we’re introducing the Gemini Decisions API, a new endpoint for fast, closed-set decisions powered by our new Gemini Decision 1 model (gemini-decision-1), available through the [Gemini API](https://aistudio.google.com/docs/decisions-api) in [Google AI Studio](https://aistudio.google.com/apps/bundled/gemini-decisions-api-playground?fullscreenApplet=true).

The Decisions API gives you purpose-built primitives for bounded choices like tool selection, policy checks, model routing, and context grading. You can pass one multimodal input across text, images, audio, video, and PDFs (up to 256K tokens) along with up to 1,000 independent questions. Rather than generating answer text token by token, Gemini Decision 1 scores your candidate options in a single forward pass, returning probabilities and confidence scores in 150-250 ms end-to-end latency at $0.10 per 1M input tokens with zero output tokens billed.

![Comparison of Gemini Decision and other APIs against various benchmarks](https://storage.googleapis.com/gweb-uniblog-publish-prod/images/decisions-architecture_f0NFXU9.width-2000.format-webp.webp)

## Strong performance and calibration

On benchmarks spanning agent routing, reasoning, ranking, and multimodal understanding, Gemini Decision 1 leads across the suite, with the lowest calibration error.

![Comparison of the Decisions API and other APIs to benchmark](https://storage.googleapis.com/gweb-uniblog-publish-prod/images/gemini-decision__benchmarks__tab.width-2000.format-webp_GHqdFZw.webp)

Gemini Decision 1 scores options without token-by-token decoding, keeping median latency 50-100 ms of model compute (150-250 ms end-to-end), across multimodal inputs and contexts up to 16K tokens, remaining nearly flat from 1 to 100 questions per request. With zero output tokens billed, Gemini Decision 1 with the Decisions API is nearly 5x cheaper across the shared benchmark suite than Gemini 3.5 Flash-Lite with the Interactions API.

## Native multimodal evaluation across three decision types in one pass

Built on Gemini’s native multimodal architecture, Gemini Decision 1 can process text, images, audio, video, and PDFs directly in a single forward pass. You can combine modalities in one shared input, such as pairing a call recording with account metadata or a scanned PDF claim with damage photos, and tune resolution on image and video inputs to control visual detail and token cost.

You can evaluate multiple questions against that shared input in a single request, mixing yes/no checks, multiple choice, and rubric scores. Every answer includes a confidence score (from 0 to 1) so your code can route ambiguous cases to a fallback or human reviewer:

- **noul (Yes/No):** Estimates the probability that a yes/no condition holds, with optional true and false description criteria.
- **choice (Multiple Choice):** Selects from 2 to 255 choices, returning the winner and normalized probabilities for every candidate.
- **score (Ordered Rubric):** Grades inputs across an ordered rubric (starting at 0), returning a continuous weighted score and per-level probabilities.

The `input` field uses the same `Content` block format as the [Interactions API](https://aistudio.google.com/docs/interactions-overview). Here is how to check a photo of a breakroom counter, together with a text note, against all three question types in one request:

```py
from google import genai

client = genai.Client(http_options={"api_version": "v1beta"})

result = client.decisions.create(
    model="gemini-decision-1",
    input=[{
            "type": "image",
            "uri": "https://storage.googleapis.com/generativeai-downloads/images/spill.jpg",
            "mime_type": "image/jpeg",
        },
        {
            "type": "text",
            "text": "Breakroom counter check before the 9 AM client meeting.",
        },
    ],
    questions={
        "has_spill": {
            "type": "noul",
            "instructions": "Is there an uncleaned liquid spill on the counter?",
        },
        "next_action": {
            "type": "choice",
            "instructions": "Which cleanup action fits the scene?",
            "criteria": {
                "wipe_with_cloth": "Small counter spill with a cloth or towel already nearby.",
                "dispatch_janitorial": "Large floor hazard, broken glass, or active leak.",
                "ready_for_meeting": "Counter is clean and ready.",
            },
        },
        "severity": {
            "type": "score",
            "instructions": "Rate the severity of the counter mess.",
            "criteria": [
                "Clean and ready.",
                "Minor localized spill.",
                "Major spill or safety hazard.",
            ],
        },
    },
)
```

![Gemini detected a spill with a next_action wipe_with_cloth](https://storage.googleapis.com/gweb-uniblog-publish-prod/images/decisions-hero-hud_l8rnHOK.width-2000.format-webp.webp)

## How teams are building with the Decisions API

Teams across Google are already using the Decisions API as a low-latency control plane, shipping features simply by defining options rather than training custom classifiers or waiting on generative calls:

- **Dynamic model routing in Gemini Notebook:** Gemini Notebook uses the Decisions API as a real-time router to instantly direct incoming queries to the optimal model tier and thinking level for each request. The Decisions API classifies intent in milliseconds to serve everyday queries with up to 40% lower latency while reserving deep reasoning models for complex tasks.
- **Rapid design iteration in Stitch:** The new [rapid canvas experiment in Stitch](https://stitch.withgoogle.com/rapid) uses the Decisions API to generate playable wireframes of a product flow in seconds and to apply edits made by voice or tap almost instantly. When the flow feels right, one click hands the screens off to the full Stitch experience for deeper design polish.
- **Emoji selection in CC:** CC from Google Labs uses the Decisions API to pick the right emoji reaction from a palette of more than 200 options in a fraction of a second, so reactions appear instantly without adding latency.
- **Safety layer for MCP tools in Search**: Google Search uses the Decision API to detect adversarial behavior and content against adversarial evals with high efficacy within the latency envelope.

Stitch Rapid uses the Decisions API to design an entire product flow in seconds (1x speed).

## Start building today

Gemini Decision 1 is priced at $0.10 per 1M input tokens and $0 for output tokens, with an API no-cost tier you can use in [Google AI Studio](https://aistudio.google.com/apps/bundled/gemini-decisions-api-playground?fullscreenApplet=true) for prototyping. Your shared `input` is billed once per request no matter how many questions you evaluate (each question adds only its own instruction and criteria tokens). There are no output-token charges, variable thinking-token bills, or separate audio transcription and OCR costs.

You can start building today by grabbing an API key in [Google AI Studio](https://aistudio.google.com/api-keys), trying our [Doorstep Parcels](https://aistudio.google.com/apps/bundled/doorstep-parcels-qa?fullscreenApplet=true), [Speed Sketch](https://aistudio.google.com/apps/bundled/speed-sketch?fullscreenApplet=true), and [Find the Zoo Keeper](https://aistudio.google.com/apps/bundled/find-the-zoo-keeper?fullscreenApplet=true) demo apps, and following the Python, JavaScript, and REST guides in the [Decisions API documentation](https://aistudio.google.com/docs/decisions-api).

Ecosystem integrations are rolling out across [OpenRouter](https://openrouter.ai/google/gemini-decision-1), [LangChain](https://docs.langchain.com/oss/python/integrations/providers/google), [Vercel](https://ai-sdk.dev/providers/ai-sdk-providers/google), [LiteLLM](https://docs.litellm.ai/docs/providers/gemini), [Pydantic AI](https://pydantic.dev/docs/ai/models/google/), [Pipecat](https://docs.pipecat.ai/api-reference/server/classifiers/google), [LiveKit](https://docs.livekit.io/agents/integrations/google/), and [OpenCode](https://opencode.ai/) alongside the `google-genai` SDKs, with support [coming soon](https://forms.gle/NFt1W6pL1gCY9FGM8) to Gemini in Google Cloud.

## Get the latest news from Google in your inbox

Sign up for our newsletters with product updates, event information, special offers, and more.

Your information will be used in accordance with [Google's privacy policy.](https://policies.google.com/privacy) You may opt out at any time.
