---
title: 'Building AI for Reliable Execution: Lessons From Industrial Robotics'
link: https://www.latent.space/p/standard-bots
source: latent-space
published: 2026-10-10T14:04:11Z
updated: 2026-10-10T14:04:11Z
first_seen: 2026-10-10T15:51:15.675508634Z
authors:
- Richard MacManus
summary: Inside Standard Bots’ AI stack, pretrained models learn factory tasks from demonstrations and improve through corrections from real deployments.
content: extracted
html: 2026-10-10-building-ai-for-reliable-execution-lessons-from-industrial.html
remote_preview:
  url: https://substackcdn.com/image/fetch/$s_!yzyh!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe58f7268-94a0-42f5-b0fc-9ecf300bb03d_2560x1440.png
---

[![](https://substackcdn.com/image/fetch/$s_!yzyh!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe58f7268-94a0-42f5-b0fc-9ecf300bb03d_2560x1440.png)](https://substackcdn.com/image/fetch/$s_!yzyh!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe58f7268-94a0-42f5-b0fc-9ecf300bb03d_2560x1440.png)

When you think of robotics and AI, you probably first think of **full humanoid robots** like Figure’s [AI-powered machines](https://www.figure.ai/helix) and 1X’s [NEO home robots](https://www.1x.tech/neo). Those may well be the future, but arguably more important in 2026 is **industrial robots** — which are typically *not* humanoids.

[Standard Bots](https://standardbots.com/) claims to be “America’s largest AI-native industrial robot manufacturer.” It recently [raised $200 million](https://www.bloomberg.com/news/articles/2026-06-09/standard-bots-raises-200-million-to-manufacture-robots-in-us) at a $1 billion valuation, in a series C round led by General Catalyst and RoboStrategy, a fund focused on robotics. Its customers include **NASA, Amazon and Lockheed Martin.**

We spoke to **[Evan Beard](https://www.linkedin.com/in/evanbeard/)**, co-founder and CEO, and **[Leif Jentoft](https://www.linkedin.com/in/leifjentoft/)**, Head of AI, to find out more about Standard Bots’ AI stack and how its models work.

To set the scene, Standard Bots’ industrial robot arms are designed for tasks such as machine tending, welding, and assembly. Here’s a quick demo by Jentoft:

[www.youtube.com](https://www.youtube.com/watch?v=tQa6yvAwLUI)

According to Beard, Standard Bots has a **shared base model** that customers adapt through demonstrations and fine-tuning. Jentoft added that Standard Bots runs **a range of models**, including a zero-shot perception system for machine tending.

Beard says its largest model is in the **low billions of parameters** — so it’s not that large by frontier lab standards.

“We believe data quality matters far more than raw volume,” said Jentoft, “and we focus on **getting the most out of targeted data** rather than chasing the largest possible dataset.” (This matches [the “bitterest lesson” of Jev creator Diogo Almeida](https://www.latent.space/p/jev), who told us recently that the right task and the right data can matter more than compute.)

“Our market access gives us unique access to the highest value data,” he added. “**In-situ interventions can fix an edge case** with a few dozen examples.”

[![](https://substackcdn.com/image/fetch/$s_!J3By!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0db1b551-c180-466f-b9e7-687852e84d1d_2048x1035.png)](https://substackcdn.com/image/fetch/$s_!J3By!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0db1b551-c180-466f-b9e7-687852e84d1d_2048x1035.png)

*Setting up part identification with ClickFind.*

Beard notes that Standard Bots **focuses on short-horizon tasks** to meet production requirements for cycle time and reliability. The company uses conventional programming for parts of a workflow that don’t require learned behavior.

Model training happens in the cloud but **inference runs locally**, said Beard.

I asked Jentoft what the learned model does in a typical robot routine, and which parts of the workflow use conventional programming?

As an example, he brought up its machine-tending solution for high-mix manufacturing.

“We’ve built a zero-shot system that lets a user tell the robot which parts to look for on a given task. **The model handles perception — locating and identifying the parts** — while conventional programming handles the motion and the cell logic around it.”

Here’s a demonstration of the machine-tending:

[www.youtube.com](https://www.youtube.com/watch?v=J9NnslgGp8I)

“Our backbone for this task is **trained on over a billion images**, which is what lets the robot distinguish between lighting conditions, material types, and an object versus its background,” Jentoft said.

Back in April, we had [Applied Intuition on the podcast](https://www.latent.space/p/appliedintuition) to talk about **“Physical AI”** and how it differs from on-screen AI. One of the learnings was that the Physical AI isn’t just constrained by model intelligence: **the hard part is deploying models onto real hardware**, under safety, latency, power, cost, and reliability constraints.

But for Standard Bots, that can also be a strength, since it controls the “full stack” from software to hardware. Jentoft points out that it controls the robotic arm, end effector, control system, and AI.

“That lets us **co-optimize the models and the control policies**,” he said. “Despite claims elsewhere, no model today is truly hardware-agnostic, and co-optimizing low-level control and higher-level functions is **a major advantage for both performance and iteration speed.”**

[![](https://substackcdn.com/image/fetch/$s_!Z9hh!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F66302fb5-239d-453b-af14-483b003b5b8a_1584x1068.png)](https://substackcdn.com/image/fetch/$s_!Z9hh!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F66302fb5-239d-453b-af14-483b003b5b8a_1584x1068.png)

**Skild**, a company aiming to build “**general purpose robotic intelligence,”** might quibble with the hardware-agnostic claim — its goal is to achieve [cross-hardware generalization](https://www.skild.ai/blogs/building-the-general-purpose-robotic-brain).

Beard had mentioned that inference is done locally, which is especially important for its core customers: factories.

“For factories, which is most of what we’re doing right now — and certainly so much opportunity that we see there — this is something where **you need to do inference on-premise**,” he said.

[![](https://substackcdn.com/image/fetch/$s_!R8pK!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6cfd90ff-b106-45f5-9789-771ee2196595_1814x1280.png)](https://substackcdn.com/image/fetch/$s_!R8pK!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6cfd90ff-b106-45f5-9789-771ee2196595_1814x1280.png)

*An AI agent building a machine-tending routine.*

For machine tending, the model identifies parts while conventional programming handles motion. Jentoft also described how models can generate actions that feed into the robot’s control system.

“Wrist cameras and other **sensors** feed raw pixels and signals into the system over **internal gigabit Ethernet**, routed internally so no cables tangle,” he explained. “**GPUs at the edge** process that data to generate **action chunks**, which stream to low-level control.”

He reiterated that inference is run on-premises deliberately.

“Cloud compute for robotics is operationally extremely hard — **most factories and warehouses don’t have reliable internet**, and that’s even more true for mobile robots. Uptime is crucial to customer acceptance, so we keep the loop local.”

Standard Bots uses simulation where it can, but Beard says some production tasks — including those involving liquids, suction, or cutting flexible material — are **difficult to reproduce in current simulators.**

Real-world demonstrations are another part of the learning process. Here’s a demo of teaching a robot using a handheld touch device:

[www.youtube.com](https://www.youtube.com/watch?v=Ov07Vy6yKf8)

So how does it deal with failure in a robot task, and do those kinds of learnings flow back to the model?

“We capture **failure signals and human corrections** from deployments, and how we use that data depends on the customer,” Jentoft replied.

He noted that many of its defense customers “deploy in air-gapped environments, where nothing comes back.” But for customers that aren’t air-gapped, “fleet learning delivers enough benefit to their own applications that we typically don’t see pushback on contributing data in exchange for that performance.”

Standard Bots has already expanded its platform so that external developers can use it. With [StandardOS](https://standardbots.com/developers), it offers **a set of APIs and SDKs.**

According to Beard, developers can use Standard Bots’ APIs to build robotics applications with whichever parts of its stack they need, including bringing their own models. He cited [NVIDIA Cosmos](https://www.nvidia.com/en-eu/ai/cosmos/), an open family of omnimodal world models for physical AI, as an example ([version 3 was released late-May](https://www.latent.space/p/ainews-nvidia-cosmos-3-nemotron-3)).

Today, this requires developers to write their own integration code. But Standard Bots plans to make collecting data, training models, and deploying them onto its robots easier.

[![](https://substackcdn.com/image/fetch/$s_!iute!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fff90bb80-df6a-481a-b149-782ecefa9f6d_2048x1216.png)](https://substackcdn.com/image/fetch/$s_!iute!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fff90bb80-df6a-481a-b149-782ecefa9f6d_2048x1216.png)

*AI robot learning workflow with NVIDIA software; via Standard Bots.*

Standard Bots isn’t the sexiest robotics company out there, but it is already being deployed for industrial automation at NASA, Amazon, Lockheed Martin, and others.

For AI engineers, perhaps the most useful lesson is how Standard Bots adapts AI to a specific job. It keeps **the model’s role focused** and uses **corrections from real deployments** to fix edge cases. You can, of course, apply that same approach to software agents.

As Jentoft said — and it was a recent lesson from Jev too — it’s all about “getting the most out of targeted data rather than chasing the largest possible dataset.”
