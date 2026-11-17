---
title: Cloudflare Joins the Jevolution and Labels Itself a Neolab
link: https://latent.space/p/cloudflare/
source: latent-space
published: 2026-11-17T15:00:00Z
updated: 2026-11-17T15:00:00Z
first_seen: 2026-11-17T18:26:16.162530996Z
authors:
- Richard MacManus
summary: Cloudflare made its name speeding up the web. Now it's building System One models to make decisions and pull data in milliseconds.
content: extracted
html: 2026-11-17-cloudflare-joins-the-jevolution-and-labels-itself-a-neolab.html
remote_preview:
  url: https://beehiiv-images-production.s3.amazonaws.com/uploads/asset/file/48e5d6c0-f996-45a2-a80c-d0bc9f0eda0a/cloudflare-neolab-feature.jpg?t=1794916474
  alt: Cloudflare Joins the Jevolution and Labels Itself a Neolab
extra:
  thumbnail: https://beehiiv-images-production.s3.amazonaws.com/uploads/asset/file/48e5d6c0-f996-45a2-a80c-d0bc9f0eda0a/cloudflare-neolab-feature.jpg?t=1794916474
---

Cloudflare made its name speeding up the web. Now it's building System One models to make decisions and pull data in milliseconds.

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/48e5d6c0-f996-45a2-a80c-d0bc9f0eda0a/cloudflare-neolab-feature.jpg?t=1794916435)

Cloudflare has been one of the essential infrastructure companies of the web for the past decade and a half. But now, it’s turning into a **neolab** — at least according to a presentation at [AI Engineer Code Summit](https://ai.engineer/code/2026/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) by **Michelle Chen**, who runs Cloudflare’s AI Platform on the product side.

We’ll get into what exactly a neolab is in a minute (the short version: they build models and they’re not called OpenAI or Anthropic). Cloudflare is claiming to be one because it too has begun to launch models. Two [System One models](https://latent.space/p/jev/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv), to be precise: **Clef and Clairic.** As Chen explained in her talk, **Clef is a decision model** directly inspired by [TypeSafe AI’s revolutionary Jev model](https://latent.space/p/jev-revolution/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv), while **Clairic** is a new type of system one model that Cloudflare calls **an “extract” model**.

In an interview with Latent Space after her presentation, Chen hinted that Cloudflare won’t *just* be producing system one models. **There will be large language models, too.**

“We're not being a neolab \[to\] only launch system one models, even though it makes sense for our infrastructure. We've kind of entered this Pandora's box now, right? We opened it, now the possibilities are endless.”

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/907af965-20e0-42ef-b089-09438b423a5e/image.jpeg?t=1794916465)

Chen said it’s “not just system one models, but **potentially reasoning models** that we've been experimenting with, and other LLMs and whatnot.”

However, she added that these probably **won’t be pre-trained models** built from scratch, like the frontier labs produce.

“I don't think that's our focus, but I think we can take existing resources out there and make \[them\] a lot more efficient and effective.”

She noted that **“most of our team really just does post-training,”** so that will be the focus in model development.

## What is a neolab and is Cloudflare really one?

The term neolab literally means a ‘new lab’ and it was coined last November [by](https://www.theinformation.com/articles/investors-chase-neolabs-outflank-openai-anthropic?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) [**Stephanie Palazzolo**](https://www.theinformation.com/articles/investors-chase-neolabs-outflank-openai-anthropic?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) from The Information, who defined it as “a new generation of research-focused AI startups.”

**Deedy Das**, a partner at Menlo Ventures and an AI influencer, runs [a big list of neolabs](https://debarghyadas.com/neolabs/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv). It includes companies like **Thinking Machines Lab, Poolside, and TypeSafe AI** itself. According to Das, neolabs employ elite researchers, spend a lot of compute (“usually to train its own AI model”), *don't* have significant revenue traction, and they’re typically startups that have raised a lot of money.

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/a03e1bb7-8505-4cee-897c-c8e4195e3bfd/image.png?t=1794916466)

*Deedy Das’ list of neolabs — note that TypeSafe is among the top 20.*

Cloudflare doesn’t tick at least half of those boxes, since it’s **a public company with significant revenue traction**. In fact, earlier this month Cloudflare [announced record earnings](https://www.cloudflare.com/press/press-releases/2026/cloudflare-announces-third-quarter-2026-financial-results/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv). “The third quarter was Cloudflare's best quarter ever,” said CEO **Matthew Prince**. “We crossed **$3 billion in annualized revenue** with revenue of $782 million, up 39% year over year.”

So Cloudflare as a “neolab” is pushing the accepted definition. However, as Chen argued in her presentation, **building models is part of a natural progression for the company**: from infrastructure, to compute (e.g. the Workers platform, including now for agents), to storage, to [serving open weight models](https://blog.cloudflare.com/workers-ai-large-models/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) like Kimi, GLM and DeepSeek (on Workers AI), to now building its own models.

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/8bd3b40a-ef63-4df9-ae6e-53e29bd1bfc9/image.jpeg?t=1794916465)

## Jev as the “east wind”

Chen readily admits that **Jev inspired Cloudflare’s pivot** to building models. Referencing a Chinese proverb, she framed Jev’s arrival in September as “the east wind” that unexpectedly changed how Cloudflare viewed AI models.

“And so when the system one and Jev paradigm started coming on board,” Chen said in her presentation, “we were like, this makes so much sense for us to adopt, right? **We don't have to send every request to an LLM.** We can send some of it to a System One model.”

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/d856bbcc-2768-475e-b817-d5f5591b2faf/image.png?t=1794916466)

In our conversation, Chen commented on how it took only two weeks for them to build **Clef — which is a copy of Jev** — whereas it took TypeSafe AI two years to build Jev. She meant that as a compliment for how hard TypeSafe AI worked to create this new type of model.

“All credit to TypeSafe, really, for actually attacking this space and **inventing something new here**, that people could relate to so strongly and be able to use it. And, yeah, we came up with a model super quickly, but they put in a lot of legwork and we knew exactly what the API schema should look like and whatnot.”

**Cloudflare is, at its core, still an infrastructure company** — speed and low latency is its meat and potatoes. The main benefit of System One models, Chen remarked, is that they allow **more work to be done by smaller, faster models**, where output is measured in milliseconds rather than seconds.

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/3afe8bb2-7058-416e-8cea-322a4d199863/image.png?t=1794916466)

## Clairic and what extraction models do

While Clef was a clone of Jev, Cloudflare soon came up with **an original twist on System One with Clairic**, [launched last month](https://blog.cloudflare.com/clairic-extraction-model/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) as “the first model in a new class: System One extraction models.”

“What Clairic does is it can take unstructured input, and it can extract, compute, estimate and forecast values from those inputs,” Chen explained in her presentation.

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/41461a45-cc69-49cf-ab80-794592ae632d/image.png?t=1794916466)

In our interview, Chen said that her team wanted to use System One principles to find other areas where LLMs were “overkill” and didn’t always return the right answer.

One of the problems they settled on was that **LLMs don’t always deal well with unstructured input.** Because LLMs are autoregressive, Chen explained, they are prone to hallucinate and they sometimes return malformed JSON.

“So we used the same System One principles \[as Jev/Clef\] there, and decided on extraction.”

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/85271a05-de05-4016-b086-439fe07a47d5/image.png?t=1794916466)

*Some examples of system one use cases at Cloudflare.*

She used the example of an invoice, where an LLM will sometimes get an important detail wrong — like the order number.

“When you send it \[the order\] to an LLM, they're like, wait, I think the order number's three, maybe it's four, maybe it's five. But it's like, **the number's right there, you just have to read it!**”

So the idea with Clairic is that it will extract a typed value — like a number — from unstructured data, in one forward pass.

## Why Cloudflare is in the lab now

Using the label “neolab” sounds a bit like a marketing gimmick, but it also makes a ton of sense for Cloudflare to produce system one models — and even, in future, reasoning models.

**“Being a neolab means that we can create what we are missing,”** Chen said in her presentation.

In our interview, she also mentioned that creating their own models allows Cloudflare to derisk its AI operations.

**“I think there's a lot of risk in the AI space**, because just like Jev was closed source and we couldn't run it, there \[are\] a lot of models out there \[with\] maybe regulatory risks coming up, like the Chinese open source models. I think lots of model labs want to actually make money, as well — so enforcing quite expensive licenses.”

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/cc145e55-e196-4f0a-943b-00bab3b35fc6/image.png?t=1794916466)

If you look at [Cloudflare’s product page](https://www.cloudflare.com/products/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) now, it resembles what Amazon’s cloud computing product page looked like at the height of Web 2.0 — i.e. there’s a lot there!

But there’s still room for several more models.
