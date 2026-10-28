---
title: Can Frontier Models See Past a Decoy? Elorian Tests Visual Reasoning
link: https://latent.space/p/elorian/
source: latent-space
published: 2026-10-28T21:53:36.421640500Z
updated: 2026-10-28T21:53:36.421640500Z
first_seen: 2026-10-28T21:53:36.421640500Z
authors:
- Richard MacManus
summary: A new decoy counting benchmark exposes a gap between seeing and reasoning. Elorian’s Glimpse tries to bridge it by thinking with images.
content: extracted
html: 2026-10-28-can-frontier-models-see-past-a-decoy-elorian-tests-visual.html
remote_preview:
  url: https://beehiiv-images-production.s3.amazonaws.com/uploads/asset/file/c0263f23-d8f4-45a5-8e75-1ea88650605f/Elorian_Decoy_Feature_Image.png?t=1793221147
  alt: Can Frontier Models See Past a Decoy? Elorian Tests Visual Reasoning
extra:
  thumbnail: https://beehiiv-images-production.s3.amazonaws.com/uploads/asset/file/c0263f23-d8f4-45a5-8e75-1ea88650605f/Elorian_Decoy_Feature_Image.png?t=1793221147
---

A new decoy counting benchmark exposes a gap between seeing and reasoning. Elorian’s Glimpse tries to bridge it by thinking with images.

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/db8957d5-feab-4142-a4ff-19335e7766be/Elorian_Decoy_Feature_Image.png?t=1793221877)

This month [Elorian](https://elorian.ai/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) launched its first **visual reasoning model**, [Glimpse](https://elorian.ai/blog/introducing-glimpse?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv). It’s being promoted as “a new category of visual thinking model: a system that reasons about the physical world from what it sees.”

So how does Glimpse compare to both frontier models and world models in vision tests? **A new benchmark called DecoyCount**, co-released by Elorian, provides some answers.

We quizzed CEO [Andrew Dai](https://www.linkedin.com/in/andrewdai/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) on the benchmark, and also talked about Elorian’s positioning in a crowded models landscape.

## DecoyCount results

Elorian **emerged from stealth in April** with [$55 million in seed funding](https://www.wsgr.com/en/insights/wilson-sonsini-advises-elorian-on-dollar55-million-seed-financing-as-company-emerges-from-stealth.html?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv), co-led by Striker Venture Partners, Menlo Ventures and Altimeter Capital. Now, in partnership with AI data company [Mercor](https://www.mercor.com/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv), Elorian is announcing the **DecoyCount** benchmark, which tests **whether language models genuinely count objects** or rely on visual shortcuts and substitution heuristics.

According to [a paper accompanying the release](https://arxiv.org/abs/2610.34855?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv), **frontier models scored below 50% accuracy** on DecoyCount — the highest was GPT-6 Astra, with 43.17%. Elorian’s new model isn’t mentioned in the paper, but Dai told us that Glimpse **scored 50.48% in an internal test.**

Regarding the frontier models, the paper concludes that “stronger general reasoning does not by itself generalize to aligned visual reasoning.”

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/604f2ace-8b41-4834-af03-75ee649ab47f/image.png?t=1793221074)

How the benchmark works: “DecoyCount exposes interpretable counting failures across seven mechanisms.”

To make sure it was a like-for-like comparison with the frontier models, we asked whether the paper’s evaluation images were withheld from Glimpse’s training?

“The evaluation images and text were withheld from our training, and our contamination check found no overlap in the training data checked,” Dai replied, via email. “Without any cues, **Glimpse scored 50.48% on the 1,660 DecoyCount items, making it the only model in the comparison to clear 50%.** The closest was GPT-6 Astra at 43.17%, about 7 points behind, and Gemini-3.8-flash scored 32.71% while Qwen3.8-max scored 28.31%, and Claude-Fable-5.1 came in at 25.22%.”

Dai did note that Glimpse’s results “come from a single run of the model on its own, without an agent or tool harness,” whereas the six models from the paper were each averaged across three runs under their reported settings.

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/99cce327-2633-472b-af86-3655b621fb79/image.png?t=1793221073)

DecoyCount benchmark results for frontier models over 3 runs each.

But what exactly is DecoyCount revealing that existing model benchmarks miss? According to Dai, it **tests both vision** ***and*** **reasoning together** — which most benchmarks elide.

“If a person sees an apple in front of a mirror, we know that there’s just one apple and it’s being reflected. But a model might miss the apple entirely (a vision problem) or count the apple and its reflection as two (a reasoning problem). But most benchmarks that test for this error only check the final number, so both mistakes are treated the same way, which is an issue if you want to fix the root problem and **can’t tell if it’s an issue of vision or judgement.**”

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/b95ade03-ba1a-40ee-aa44-31b79c8224e2/image.png?t=1793221073)

Frontier Model Comparison in DecoyCount paper

## What is visual thinking exactly?

Andrew Dai isn’t a Johnny-come-lately on the AI scene. In 2015, while working at Google, he co-authored a paper entitled [Semi-supervised sequence learning](https://scholar.google.com/citations?view_op=view_citation&hl=en&user=2r2NuDAAAAAJ&citation_for_view=2r2NuDAAAAAJ%3AW7OEmFMy1HYC&utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv). In this paper, Dai and his then colleague **Quoc Le** demonstrated an early version of the **pretrain-then-fine-tune approach** later adopted in large language models. Over the decade that followed, Dai worked for Google DeepMind on GLaM MoE, PaLM 2 and then Gemini.

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/7e31049d-856c-4a35-bd2c-4d94762664a6/Screenshot_2026-10-28_at_21.22.43.png?t=1793222913)

Dai only left Google this year, in order to co-found Elorian. Part of his motivation was to **push the boundaries of what vision models can do.** His approach, he explained to Latent Space, is based on a concept in human psychology called **visual thinking**.

When most people “think about spatial problems or visual problems,” he explained, **“they visualize the problem \[by\] generating images and editing images in their mind.”**

He gave an example of buying a sofa and mentally picturing what it will look like in your living room.

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/b6cc8976-a012-42f9-842d-a2e4fc8976c9/image.png?t=1793221073)

A conceptual visual reasoning loop

There’s also **manipulating an image in your mind**; for example if you’re counting people around a table, you might mentally “cross them off one by one,” Dai said.

This is what Elorian aims to do in its models: teach them visual thinking “that involves generation and editing images as part of the reasoning trace, along with language.”

The key point is that image generation isn’t just an output for a vision model — **the generated or edited image is part of a chain of reasoning.**

## How is Elorian different from World Models?

Visual reasoning is still a work in progress, but there have been some impressive releases this year in **“world models” — where vision is a key component**. Two that stand out are World Labs’ **Atlas** in September (the company was subsequently [acquired by AMD](https://latent.space/p/ainews-amd-buys-world-labs-for-82b/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv)) and Google’s [just released](https://latent.space/p/ainews-anthropic-ends-coding-interviews-hunger-beats-tenure/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) [**Genie 4**](https://latent.space/p/ainews-anthropic-ends-coding-interviews-hunger-beats-tenure/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv).

When World Labs [announced Atlas](https://www.worldlabs.ai/blog/atlas?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv), it was described as an “omni world model for spatial intelligence.” But what Atlas enables — basically a 3D simulation of a world — isn’t the same as visual reasoning.

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/578fca6b-2e3f-494d-a4b4-772cce27ddb9/image.png?t=1793221074)

According to Dai, world models enable “direct action on a world,” whereas Elorian is more focused on what he calls “indirect action.” By that he means **using visual reasoning to inform a decision or workflow (what Glimpse does)**, rather than building simulated environments that allow navigation and manipulation (what Atlas does).

As an example, you might use visual reasoning to decide whether to reject a user-contributed video for violating a policy. “You don't need a 3D rendering to do that,” said Dai.

## Applications for visual reasoning

Other business opportunities for Elorian include robotics, engineering, and defense applications. Dai added that architects need to regularly check building codes and compliance — an ideal task for a vision reasoning model.

In two **robotics benchmarks**, WGO Bench and ERQA, Elorian **claims to rate equal or higher than frontier models**, and at a much cheaper price:

Getting back to counting decoys as one measure of success for vision models, Dai argues that reasoning about what a model sees is crucial to its **accuracy and effectiveness in real-world applications.**

He gives an example of a warehouse robot counting items in a box.

“A familiar 12-slot tray might only hold 10 parts, and a 24-pack label doesn’t mean 24 items are still in the box,” said Dai. “A model relying on what it expects to see will face many issues in real-world situations, and Glimpse is designed to work through the image instead of jumping to a quick conclusion. **We’ve designed Glimpse to ground the count in visible evidence** as opposed to basic cues that could lead other models astray.”

## Why Elorian thinks AGI requires visual reasoning

Elorian goes so far as to claim that **visual reasoning is a foundational requirement for AGI** (artificial general intelligence). From the [Glimpse launch post](https://elorian.ai/blog/introducing-glimpse?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv):

*“We believe building the full stack across systems, data, model architecture, and training — from the ground up for multimodal intelligence — can unlock AGI in the physical world.”*

Dai acknowledges there’s “still a huge gap between humans and machines.” But he thinks visual reasoning is the key to closing that gap.

**“I think the way to think about visual reasoning is as a fundamental step of reasoning in human intelligence,”** he said. “And ultimately, if we want to build anything that's like AGI, we can't just skip visual.”
