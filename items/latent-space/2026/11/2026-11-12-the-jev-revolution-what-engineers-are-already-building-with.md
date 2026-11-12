---
title: 'The Jev Revolution: What Engineers Are Already Building with System One Models'
link: https://latent.space/p/jev-revolution/
source: latent-space
published: 2026-11-12T05:08:20Z
updated: 2026-11-12T05:08:20Z
first_seen: 2026-11-12T10:54:03.804437502Z
authors:
- Richard MacManus
summary: At AI Engineer Code Summit, lead Jev creator Diogo Almeida called on developers to automate everything. Also, Glean and others showed how they’re using Jev.
content: extracted
html: 2026-11-12-the-jev-revolution-what-engineers-are-already-building-with.html
remote_preview:
  url: https://beehiiv-images-production.s3.amazonaws.com/uploads/asset/file/b4181d5a-0460-4828-b350-07feca728023/jevalution-feature.jpg?t=1794458692
  alt: 'The Jev Revolution: What Engineers Are Already Building with System One Models'
extra:
  thumbnail: https://beehiiv-images-production.s3.amazonaws.com/uploads/asset/file/b4181d5a-0460-4828-b350-07feca728023/jevalution-feature.jpg?t=1794458692
---

At AI Engineer Code Summit, lead Jev creator Diogo Almeida called on developers to automate everything. Also, Glean and others showed how they’re using Jev.

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/b4181d5a-0460-4828-b350-07feca728023/jevalution-feature.jpg?t=1794458681)

Today at the [AI Engineer Code Summit](https://ai.engineer/code/2026/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv), the main theme was **system one models** — non-generative models that return structured choices, scores, or probabilities. They’re also typically much faster and cheaper than reasoning LLMs (a.k.a. system two models).

And who better to give an inspirational rallying cry for system one models than **Diogo Almeida**, co-founder and CEO of **TypeSafe AI**. Almeida was lead creator of **Jev**, the product that kicked off this trend less than two months ago.

Jev was an instant revelation for AI engineers when it was released in September. In [a podcast with Latent Space on launch](https://latent.space/p/jev/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) (which quickly became our most popular episode of the year), Almeida explained the core problem with making LLMs the heart of software: **reliability**.

In his keynote at AIE Code today, he took it a step further by claiming that Jev is **“reliable intelligence”** that should be used for **“just automating everything.”**

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/97094656-707a-4de6-b950-c244384f1606/image.jpeg?t=1794458718)

## Integrating system one models into code

Of particular interest to AI engineers is Almeida’s point that system one models should become integrated with code. As he put it in his keynote, **“code is the language of automation**, therefore you need to integrate with code.”

Latent Space founder swyx interprets this as a call to **“liberally sprinkle ‘System One’ model calls everywhere INSIDE application code** to make smarter software instead of increasingly convoluted forms of ‘concat strings together and hope for the best" AI engineering.’”

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/95b5c4b1-80ba-46d1-a81e-49960b47f405/image.jpeg?t=1794458718)

The term **“smart software”** was used throughout Almeida’s presentation. It sounds like the kind of term used by companies like Microsoft or IBM during Web 2.0, but in the AI era it does actually signify **a harsh reality about LLM-driven software: sometimes it’s just not very smart.** As Almeida quipped during his presentation, “you get Einstein 95% of the time, but Mr. Bean 5% of the time.”

“And if you can't tell which is which, how the fuck are you going to automate anything,” he added.

Almeida ended with an impassioned plea to **“automate everything.”** Like an AI-pilled, pink-haired Steve Jobs, he called out to “the dreamers and the people who don't want to be limited by everything that has happened so far,” promising that TypeSafe AI wants to “really fight for you, and we want this to be **as reliable as a fourth logic gate.**”

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/1c9ba0ac-1ed9-45e8-8ff2-ec5b7a145534/image.jpeg?t=1794458718)

## How Glean uses Jev

With the unenviable task of immediately following Almeida on stage, **Chinmay Goyal** from [**Glean**](https://latent.space/p/glean-model-routing/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) presented a more prosaic version of Jev usage. He explained that system one models are used at Glean alongside LLMs and other types of classifiers, and that Jev is especially suited for **“zero-shot flexibility.”**

“As the taxonomy of the decision space changes, we don't have to fine-tune these models again and again,” he said.

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/2cd5055c-dc56-40bf-81e3-4ab3eb806967/image.jpeg?t=1794458718)

In a short interview after his presentation, Goyal told Latent Space that their customers don’t actually need to know which type of model they’re using.

**“Jev powers a significant number of flows** \[but\] the user just needs to have a product that works; that is always our aim with building anything. So we want to hide away all the complexity of whether we are using Jev or we are using a full-blown LLM.”

In the conclusion of his presentation, Goyal offered some rules on when to use Jev:

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/90b6d44f-f945-4d83-98e4-f22d53730d36/image.jpeg?t=1794458719)

## Jev patterns and prototypes

Next up at AIE Code were **Chibi Vikram** and **Kevin Bernstein** from **UiPath**, a business orchestration and automation platform. One of their examples showed **a test of Jev ranking document chunks**. They held a “tournament” where Jev picked the most relevant chunks from small groups, then the winners advanced to another round, and so on. This whole process **took under a second**, even with thousands of Jev calls.

“We realized that **Jev is a building block that we can adjust**,” said Vikram about this and other experiments, “and we have to find these new strategies where we will be able to apply this building block. It might look less intelligent on its own, but **with these strategies and patterns, we can get more intelligent** — with even the current Jev model that we have.”

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/5fb5c868-1c88-40f9-a6fd-231c9f7e29aa/image.jpeg?t=1794458720)

**Scott Roberts**, a Principal Engineer at **Workday**, finished the ‘system one’ session at AIE Code with some prototypes his team has run with Jev.

In one, an interface starts to fill in a time-off card while the user is still typing. It was an interesting showcase of Almeida’s idea that **a fast model call can change what the software itself does.** Roberts described it as going “from predicting what you want to say to predicting what you want to do.”

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/c66b3fc3-2e38-444d-818f-d4450e9b6b6a/image.jpeg?t=1794458718)

## Like the early internet

The biggest learning I took from the System One presentations today is that a model like Jev can **open up surprising new ways to build software**. I was sitting next to a conference attendee who heads up AI at his company. He told me that in his spare time, he used Jev to create a website that sorts and ranks news about fantasy football.

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/c05a1cd7-f766-493f-bc09-e62ce4474960/image.jpeg?t=1794458718)

But the final word has to go to Almeida, who likened the new opportunities opened up by system one models **to the experimentation of the early internet.**

“I think that there's going to be an explosion of what software can do, and we will want people to be exploring those boundaries of software. Not necessarily because it's valuable, but **because they can, because we have new frontiers to explore**, and this is what the early internet was like.”
