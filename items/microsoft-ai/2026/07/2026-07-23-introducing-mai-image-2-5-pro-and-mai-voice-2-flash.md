---
title: Introducing MAI-Image-2.5-Pro and MAI-Voice-2-Flash
link: https://microsoft.ai/news/introducing-mai-image-2-5-pro-and-mai-voice-2-flash/
source: microsoft-ai
published: 2026-07-23T16:30:00Z
updated: 2026-07-23T16:30:00Z
first_seen: 2026-09-07T17:03:44.343711062Z
authors:
- alenabu@microsoft.com
summary: The post Introducing MAI-Image-2.5-Pro and MAI-Voice-2-Flash appeared first on Microsoft AI.
content: extracted
html: 2026-07-23-introducing-mai-image-2-5-pro-and-mai-voice-2-flash.html
preview:
  file: 2026-07-23-introducing-mai-image-2-5-pro-and-mai-voice-2-flash.preview-b8e68f2c739f.webp
  width: 256
  height: 168
  alt: Two overlapping speech bubbles, one pink and one green, with a flower in the center where they meet. The background is light gray.
  color: '#cdcbca'
images:
- source: https://microsoft.ai/wp-content/uploads/2026/07/Voice_Image_Blog_Illustration_v6-1.jpg
  original:
    file: 2026-07-23-introducing-mai-image-2-5-pro-and-mai-voice-2-flash.image-0222c7147961.jpg
    width: 2048
    height: 1344
  variants:
  - file: 2026-07-23-introducing-mai-image-2-5-pro-and-mai-voice-2-flash.image-e3b0ba8bbbed.webp
    width: 48
    height: 32
  color: '#dee3e7'
- source: https://microsoft.ai/wp-content/uploads/2026/07/Frame-2147265586.jpg
  original:
    file: 2026-07-23-introducing-mai-image-2-5-pro-and-mai-voice-2-flash.image-ccc2f24316cc.jpg
    width: 2048
    height: 1024
  variants:
  - file: 2026-07-23-introducing-mai-image-2-5-pro-and-mai-voice-2-flash.image-b8a2ce2d83c5.webp
    width: 48
    height: 24
  color: '#043895'
---

Models

 Superintelligence team

 July 23, 2026

 Models

 Superintelligence team

![Two overlapping speech bubbles, one pink and one green, with a flower in the center where they meet. The background is light gray.](https://microsoft.ai/wp-content/uploads/2026/07/Voice_Image_Blog_Illustration_v6-1.jpg)

“MAI-Image-2.5-Pro is a strong leap forward for GenMedia tools. Beyond the impressive image quality, its ability to render text with this kind of accuracy is a real breakthrough. It also understands natural language edits, so creative iteration becomes faster and far more intuitive. Microsoft has firmly established itself among the leaders in generative AI.”

Rob Reilly, Global Chief Creative Officer, WPP

[A year ago](https://microsoft.ai/news/two-new-in-house-models/), we set out to develop purpose-built models in-house at Microsoft AI. Models trained on clean, traceable, enterprise-grade data, without distillation from third-party models, and designed from the ground up to serve the people who use Microsoft products every day.

Today, that work is showing up where it matters in the products you already rely on. The models [we previewed at Build](https://microsoft.ai/news/building-a-hillclimbing-machine-launching-seven-new-mai-models/) are not just topping leaderboards, they are running in production, at scale, efficiently powering experiences for millions of users across a growing portfolio of Microsoft products, including Bing, PowerPoint, OneDrive, Dynamics 365, and Azure.

**Introducing two new model variants**

Real-world use cases are not one-size-fits-all. A creative studio chasing maximum fidelity has very different needs from a customer service center serving millions of calls daily. That’s why we’re building families of models: to give every product the right balance of quality, speed, and cost.

Today we’re expanding those options:

- **[MAI-Image-2.5-Pro](https://ai.azure.com/catalog/models/MAI-Image-2.5-Pro) is now in public preview.** For use cases where quality is top of mind. Hero imagery, detailed editing, precise in-image text rendering. Pro is our highest-fidelity image model to date, priced at $5 per 1M text input tokens, $8 per 1M image input tokens, and $106 per 1M image output tokens.
- **[MAI-Voice-2-Flash](https://ai.azure.com/catalog/models/MAI-Voice-2-Flash) is now in public preview.** First introduced at Build, Flash is built for speed and scale. It’s the fast, efficient path for high-volume voice experiences where responsiveness is everything, all while retaining the natural prosody and high acoustic quality found in MAI-Voice-2. Flash is 2x faster than MAI-Voice-2 and 32% cheaper, priced at $15 per 1M characters.

MAI-Voice-2-Flash and MAI-Image-2.5-Pro sit alongside our [existing production models](https://ai.azure.com/catalog/models?source=microsoft) so builders can pick the point on the quality-speed-cost curve that fits their job.

![A collage of eight images: perfume ad, lemons with a drink, purple tote bag, dog on a crosswalk, city park, two books about birds, feet on checkered tiles, and a modern bathroom with blue accents.](https://microsoft.ai/wp-content/uploads/2026/07/Frame-2147265586.jpg)

**Purpose-built models for our Microsoft products**

Leaderboards are great for benchmarking the quality of our models, but the proof lies in serving users in real product use cases. Microsoft rigorously evaluates all models before deciding on the best one for use in production environments. MAI models are outperforming competitive models for quality, latency and efficiency for more of Microsoft’s product surfaces:

- **Bing Image Creator is now 100% in-house by default.** With [MAI-Image-2.5](https://microsoft.ai/models/mai-image-2-5/)‘s new precise image editing capabilities, it is now the default model powering Bing Image Creator end-to-end, for high-quality generation and greater creative control.

- **Image generation and editing capabilities available in PowerPoint.** MAI-Image-2.5 is now in production in PowerPoint for image-to-image capabilities, reducing GPU costs up to 84% compared with GPT-Image-2.

- **Image editing in OneDrive.** MAI-Image-2.5 is now the default model for key OneDrive production image-editing scenarios. Since rollout, it has increased save rates by 26%, reduced P95 latency by approximately 25%, and delivered 2.5x greater efficiency under medium-utilization production workloads.

- **Voice in the call center.** MAI-Voice-2-Flash now powers Dynamics 365 Contact Center, the enterprise platform for building call center agents used by customers like T-Mobile and EasyJet, bringing our most expressive, natural sounding speech to brand defining conversations while reducing GPU costs up to 89%.

- **MAI-Voice-2-Flash integrated in Azure Voice Live.** Customers can quickly build voice agents in Voice Live, powered by MAI‑Voice‑2‑Flash’s natural, expressive voices and low latency. Voice Live gives developers a scalable path to building high‑quality agents that support speech‑to‑speech interactions.

Each of these enhancements is a step toward the same goal: Microsoft products, powered by Microsoft models, built to serve the people who use them.

**Built with the experts**

Some of today’s most consequential and challenging fields, such as medicine or software engineering, demand more than a general-purpose model. They demand deep expertise, tight feedback loops, and models that are capable of solving complex, real-world challenges. Microsoft AI partners directly with domain specialists to adapt and refine our models for their unique needs:

- **MAI is partnering with Dragon Copilot, a solution used by 170,000 medical providers, which processed 28 million patient encounters last quarter.**[MAI-Transcribe-1.5](https://microsoft.ai/models/mai-transcribe-1-5/) now supports Dragon Copilot’s multilingual workflow, replacing the previous model with our own best-in-class model across 58 languages. In internal evaluations on multilingual recordings, the model delivers a 50% relative reduction in both transcription and language-identification error rates across most languages, and early research shows promising improvements in the downstream accuracy of medical notes.

None of this is an endpoint. It’s the compounding result of one decision: build models in-house so they can be shaped around the people who use our products, and offer options that deliver customers more choice and better value. That’s what “powered by MAI” means, model by model, product by product. We’re just getting started. In the meantime, get started with the models in [Foundry](https://ai.azure.com/catalog/models?source=microsoft). You can also learn more about [MAI-Image-2.5-Pro](https://review.learn.microsoft.com/en-us/azure/foundry/foundry-models/how-to/use-foundry-models-mai-image?branch=pr-en-us-13470&tabs=python) and [MAI-Voice-2-Flash](https://learn.microsoft.com/en-us/azure/ai-services/speech-service/mai-voices) on Microsoft Learn or try them out in the [MAI Playground](https://playground.microsoft.ai/chat).

## Build the Future With Us

We’re a lean, fast-moving lab made up of some of the world’s most talented minds. We have an exciting roadmap of compute at MAI, with our next-generation GB200 cluster now operational. And we have an ambitious mission we truly believe in. We’re also fortunate to partner with incredible product teams giving our models the chance to reach billions of users and create immense positive impact. If you’re a brilliant, highly-ambitious and low ego individual, you’ll fit right in—come and join us as we work on our next generation of models!

[Explore all jobs](https://microsoft.ai/careers)

## Related Stories
