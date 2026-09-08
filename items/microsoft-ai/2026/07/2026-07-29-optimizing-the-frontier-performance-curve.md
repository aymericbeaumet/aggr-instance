---
title: Optimizing the frontier performance curve
link: https://microsoft.ai/news/optimizing-the-frontier-performance-curve/
source: microsoft-ai
published: 2026-07-29T22:30:23Z
updated: 2026-07-29T22:30:23Z
first_seen: 2026-09-08T10:17:10.166315Z
authors:
- kyleburgess@microsoft.com
summary: The post Optimizing the frontier performance curve appeared first on Microsoft AI.
content: extracted
html: 2026-07-29-optimizing-the-frontier-performance-curve.html
preview:
  file: 2026-07-29-optimizing-the-frontier-performance-curve.preview-655a60f8b157.webp
  width: 256
  height: 157
  alt: Nine app icons, including mail, JetBrains, GitHub, PowerPoint, Excel, Visual Studio Code, and Microsoft OneDrive, displayed in a row on a blurred green and pink gradient background with dotted lines.
  color: '#758384'
images:
- source: https://microsoft.ai/wp-content/uploads/2026/07/MAI-7.29-Blog-Post-v3b.png
  original:
    file: 2026-07-29-optimizing-the-frontier-performance-curve.image-c1a12d89a716.png
    width: 1484
    height: 910
  variants:
  - file: 2026-07-29-optimizing-the-frontier-performance-curve.image-2799eee77616.webp
    width: 48
    height: 29
  color: '#587268'
---

Tokenmaxxing has been the story of the last few months, but token efficiency is the next big focus across the industry. How do we get the best possible performance per token invested, and the best real customer outcome per dollar invested?

To build a frontier firm, you have to optimize frontier performance against cost. Choosing where you want to sit on that curve is critical. By co-optimizing your models, harnesses, and RLEs you can pick a point on the curve that suits your firm.

In most cases, frontier generalist models aren’t necessary for every task. By tuning models for a specific product, you can maintain or even exceed frontier performance, while reducing token costs dramatically.

This is where we have focused our MAI hill-climbing machine over the last quarter, and the results are pretty cool. This week we released MAI-Cyber-1-Flash optimized for our MDASH harness.

Together, the system **delivers 96% on CyberGym (on the benchmark’s any crash score; outperforms Mythos on the CyberGym leaderboard) at 50% of the cost** when compared against our best offering in MDASH today. And remarkably, we serve it on H100s too.

It was designed to handle up to 90% of tasks efficiently, so that MDASH can reserve the largest and most expensive models in our fleet (in this case GPT 5.4) for the 10% of exceptionally hard problems that truly need them.

As Satya mentioned today in our Q4 Earnings call, since last quarter, we’ve shipped more than a dozen new models across image, voice, transcription, coding and security, and they’re already powering many of Microsoft’s most widely used products to maintain or improve quality while using significantly fewer tokens, in many cases saving 50-90% of GPU costs:

- We built MAI-Code-1-Flash hand-in-hand with our colleagues at GitHub, where **since June millions of developers have used it in their daily work. 10% higher code accept rate and 10% lower median token usage** than GPT-5.4 Mini and Claude Haiku 4.5 in VS Code, and already showing improved retention.
- We then trained that same checkpoint inside an Excel RL environment to achieve comparable performance to GPT-5.6 for the most common tasks while being more cost-efficient, and **small enough to serve on an A100 or H100 vs only the latest and most expensive accelerators.**
- MAI-Image-2.5-Flash, is now the end-to-end default in Bing Image Creator, in **production in PowerPoint where it is reducing GPU costs up to 84% compared with GPT-Image-2**, and is the default for key OneDrive editing scenarios, where it has increased save rates by 26% and delivers up to 2.5x greater token efficiency.
- **MAI-Voice-2-Flash now powers Dynamics 365 Contact Center, where customers like T-Mobile and EasyJet build their call center agents**, reducing GPU costs by up to 89%.
- MAI-Transcribe-1.5 now serves Dragon Copilot’s multilingual workflow across 58 languages — a solution used by 170,000 medical providers that processed 28 million patient encounters last quarter, where **our tests show a 50% relative in reduction transcription and language-identification error rates.**

And what’s more, by co-designing our models with our own silicon, we are seeing **40% better performance per watt running MAI models on Maia 200.**

But the benefit is not only cost. It’s resilience. Every business now must assume that any one model it depends on could disappear, through a security incident, a business or policy misalignment, or a geopolitical shift.

Every model in a product or agentic system should be substitutable, and that’s only possible when you build the harness, context, memory and action space independently of a single model family. That’s the hill-climbing machine we’ve built.

We think this is the beginning of a genuinely new performance curve. Its shape represents a system rather than a model, and traversing this curve delivers better quality, lower cost, and more choice.

This has been a summer of hard but wonderful work by the team. We are keenly aware of how early this is, and of how much we still have to learn. But the direction is clear, we are hill-climbing to move the frontier on the cost-to-outcome curve, and we will keep sharing what we learn along the way. There is much more to come.
