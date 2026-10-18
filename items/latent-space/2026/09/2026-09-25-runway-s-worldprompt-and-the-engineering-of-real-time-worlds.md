---
title: Runway’s WorldPrompt and the Engineering of Real-Time Worlds
link: https://www.latent.space/p/runway
source: latent-space
published: 2026-09-25T01:30:57Z
updated: 2026-09-25T01:30:57Z
first_seen: 2026-09-27T19:29:15.293681927Z
authors:
- Richard MacManus
summary: GWM Worlds 2 uses persistent context and timed actions to steer a world model generating video and audio in real time.
content: extracted
html: 2026-09-25-runway-s-worldprompt-and-the-engineering-of-real-time-worlds.html
preview:
  file: 2026-09-25-runway-s-worldprompt-and-the-engineering-of-real-time-worlds.preview-96a5fb14a86c.webp
  width: 256
  height: 128
  color: '#4f4b42'
images:
- source: https://substackcdn.com/image/fetch/$s_!hB4W!,w_1200,h_600,c_fill,f_jpg,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-video.s3.amazonaws.com%2Fvideo_upload%2Fpost%2F217289983%2Ffa899450-7781-454b-838b-85a733c16fce%2Ftranscoded-1790297181.png
  original:
    file: 2026-09-25-runway-s-worldprompt-and-the-engineering-of-real-time-worlds.image-8b1651fb73fe.jpg
    width: 1200
    height: 600
  color: '#443837'
- source: https://substackcdn.com/image/fetch/$s_!DK3x!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd0ffa910-ac51-45f1-8fb1-e9c2dcda1e8b_1800x1130.png
  original:
    file: 2026-09-25-runway-s-worldprompt-and-the-engineering-of-real-time-worlds.image-31a49bf35b88.png
    width: 1456
    height: 914
  color: '#f7f9fa'
- source: https://substackcdn.com/image/fetch/$s_!QtUx!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F462b0bdb-0368-49a9-9b29-d6edda97fe9b_1852x832.png
  original:
    file: 2026-09-25-runway-s-worldprompt-and-the-engineering-of-real-time-worlds.image-18f866cdd23c.jpg
    width: 1456
    height: 654
  color: '#1a1a1a'
- source: https://substackcdn.com/image/fetch/$s_!c-7m!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa228c2e6-edb0-4b6e-beda-158ec9da9edf_2066x410.png
  original:
    file: 2026-09-25-runway-s-worldprompt-and-the-engineering-of-real-time-worlds.image-7ec0869241b1.jpg
    width: 1456
    height: 289
  color: '#f8f9fb'
- source: https://substackcdn.com/image/fetch/$s_!Si0m!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F86460b95-67ad-4064-b1cc-0cea6ee480e3_1348x652.png
  original:
    file: 2026-09-25-runway-s-worldprompt-and-the-engineering-of-real-time-worlds.image-5b98e02c0d6d.jpg
    width: 1348
    height: 652
  color: '#fafafa'
- source: https://substackcdn.com/image/fetch/$s_!RtOZ!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff95952e9-4d93-4707-aa3b-3a41ebe50f0f_1990x1296.png
  original:
    file: 2026-09-25-runway-s-worldprompt-and-the-engineering-of-real-time-worlds.image-ecc4c7d8776a.jpg
    width: 1456
    height: 948
  color: '#1a1a1a'
- source: https://i.ytimg.com/vi/fGRd5gYhztg/hqdefault.jpg
  original:
    file: 2026-09-25-runway-s-worldprompt-and-the-engineering-of-real-time-worlds.image-10902a6e3a40.jpg
    width: 480
    height: 360
  color: '#000101'
extra:
  audio_type: audio/mpeg
  audio_url: https://api.substack.com/feed/podcast/217289983/80ecbf8ba268d83a6cde12d5f719e123.mp3
  duration_seconds: 5781
---

Earlier this month, **world model** company Runway [introduced GWM Worlds 2](https://runway.com/research/introducing-gwm-worlds-2), a research preview that “turns high-fidelity video and audio generation into **real-time interactive simulation**.” Runway calls this an “autoregressive diffusion” model; with autoregressive describing how it generates over time.

One new feature in particular caught our eye: **WorldPrompt**, a proposed input format for specifying a generated world and the actions within it. It allows you to **fix some aspects of a simulated environment** — including the first frame — and then create a series of **timestamped events**. The events, or actions, can even be prompted in real-time.

To understand the implications of WorldPrompt, we spoke to **[Kamil Sindi](https://www.linkedin.com/in/kamilsindi/)**, Runway’s CTO, and **[Robin Kahlow](https://www.linkedin.com/in/robin-kahlow/)**, its Principal Research Scientist for generative video and multimodal AI. We also have exclusive comments from **[Anastasis Germanidis](https://www.linkedin.com/in/agermanidis/)**, co-founder & co-CEO of Runway, courtesy of a podcast swyx and Vibhu did with him.

[www.youtube.com](https://www.youtube.com/watch?v=fGRd5gYhztg)

First, some context about **world models that can generate interactive video and audio in real-time**.

Runway is reportedly valued at $**5.3 billion**, based on its most recent fund raise of $315 million [in February](https://news.crunchbase.com/venture/gen-ai-video-startup-unicorn-runway-seriese-raise/). Its first release, GWM Worlds, was launched last December.

Alongside Runway, there are several other notable projects in this domain: Google DeepMind’s [Genie 3](https://deepmind.google/models/genie/) (which [also generates at](https://deepmind.google/blog/genie-3-a-new-frontier-for-world-models/) 720p and 24 fps), [Odyssey-2 Pro](https://odyssey.systems/the-gpt-2-moment-for-world-models), and World Labs’ [RTFM](https://www.worldlabs.ai/blog/rtfm) (Real-Time Frame Model). We’ve summarized their differences in the following table:

[![](https://substackcdn.com/image/fetch/$s_!DK3x!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd0ffa910-ac51-45f1-8fb1-e9c2dcda1e8b_1800x1130.png "")](https://substackcdn.com/image/fetch/$s_!DK3x!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd0ffa910-ac51-45f1-8fb1-e9c2dcda1e8b_1800x1130.png)

Given the **complexity and massive latency demands** of real-time video and audio generation (which we’ll get into below), all of the projects listed above have **limitations**. For instance, Google notes that Genie 3 “can currently support a few minutes of continuous interaction, rather than extended hours.”

But as our interviews with Runway show, real progress is being made.

WorldPrompt, a new feature in GWM Worlds 2, helps differentiate Runway from its competition. You can think of it as a **control layer for characters, cameras and the environment**. As Kahlow put it, it’s a way to “control all the different subjects in the world” — similar to a computer game.

“Like, if there’s an NPC \[Non-Player Character\] somewhere, the NPC might walk up to you and say something. So you could achieve the same thing with this kind of model, where you can have **very detailed control over everything in the scene.** ”

[![](https://substackcdn.com/image/fetch/$s_!QtUx!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F462b0bdb-0368-49a9-9b29-d6edda97fe9b_1852x832.png "")](https://substackcdn.com/image/fetch/$s_!QtUx!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F462b0bdb-0368-49a9-9b29-d6edda97fe9b_1852x832.png)

As the name suggests, World*Prompt* is a prompting mechanism — not a programming language. So, unlike virtual world games like Minecraft or Roblox, GWM Worlds 2 doesn’t offer scripting capabilities or the ability to control state. But there’s a power to that, as Sindi pointed out.

**“You can create promptable worlds on-demand with video and audio in sync**, across all these different domains and environments. That’s not a distant-future hypothetical thing,” he said.

But there are also **limitations to prompting a world model**. We asked how reliably the model would follow an instruction to create, for example, a law of gravity or a certain ability in a character?

“Yeah, so it’s a research preview,” Kahlow replied. “So it’s not perfect, of course, and there are still flaws. It really depends on how difficult the action is. I would say movement works quite reliably.”

Sindi added that more training plus scaling the data and models is resulting in “better following.”

Despite the current limitations of GWM Worlds 2 — especially if you compare it to pre-designed and scriptable worlds like Minecraft or Roblox — the true promise of world models like Runway is that they’ll eventually lead to **fully self-generated, real-time games and experiences**. Which is an extremely hard engineering problem, as Kahlow reminded us.

“There are two challenges. **One is making the model not generate a whole clip at once.** So instead, you want it to generate frame by frame while you’re looking at it. And the other challenge is actually **making the generation fast, so you can play it in real time.** ”

[![](https://substackcdn.com/image/fetch/$s_!c-7m!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa228c2e6-edb0-4b6e-beda-158ec9da9edf_2066x410.png "")](https://substackcdn.com/image/fetch/$s_!c-7m!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa228c2e6-edb0-4b6e-beda-158ec9da9edf_2066x410.png)

High-level view of GWM Worlds 2 process

GWM Worlds 2 offers real-time interactive worlds streamed in continuous 720p video at 24 frames per second (fps) and audio at 48,000 Hz.

Runway achieved this firstly by taking its foundational audio-video generation model and **fine-tuning it to the new WorldPrompt format**, so the model can follow that. It then post-trains the model to **generate autoregressively**.

“And after that, we work on making it real-time through **distillation methods**,” Kahlow added.

**Co-CEO Anastasis Germanidis** offered more technical details in our podcast with him. He told us that the process starts from “**bidirectional diffusion** that basically generates an entire video at once and \[makes\] it **autoregressive**.” This allows the model to “generate one frame or a few frames at a time.”

[![](https://substackcdn.com/image/fetch/$s_!Si0m!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F86460b95-67ad-4064-b1cc-0cea6ee480e3_1348x652.png "")](https://substackcdn.com/image/fetch/$s_!Si0m!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F86460b95-67ad-4064-b1cc-0cea6ee480e3_1348x652.png)

Autoregressive causal diffusion vs traditional video models; image [via Runway](https://runway.com/news/research/towards-instant-video-generation)

Germanidis described two possible forms of distillation in order to make it real-time: **distilling a larger model into a smaller one** or **reducing its diffusion steps**. As a general example, he said a model might go from around 50 denoising steps to four, with some quality loss but potentially comparable results.

Germanidis admitted that there were issues with how it generates real-time interactive video.

“The biggest challenge with autoregressive models is **error accumulation**,” he said. “You’re feeding generated frames back into the model to generate the next frames, and **if there are any small errors, they accumulate over time.** ”

Sindi told us there are also challenges dealing with “infinite generations” of content.

“There’s all these challenges around what context to keep, what to discard that’s not important. And so there’s all these optimizations we have to think about, **so we’re not blowing up our GPU memory.** ”

Another current limitation is **long-term memory**. “The model does not have perfect memory,” Kahlow said. “That’s still an open research problem.”

While performance is the primary challenge for Runway at this time, its world model also has to produce **plausible consequences** when a user takes different actions.

Germanidis used the example of simulating football; he pointed out that online video training data contains more successful goals than failed goal attempts, so a video model might render the first more convincingly.

“If I take this action versus this action, **you want it to generate equally realistic outcomes,** ” he told us. “That’s, I think, the big gap between video models and world models: that idea of **counterfactual generation**.”

Sindi told us that evaluation gets harder the more complex interactions get.

“If you have this multi-prompt, multi-character, multi-scene \[environment\], how do you really understand **what was causal and what was not?** ”

To try and solve that, Runway has some automated verifiable tests. But since GWM Worlds 2 is a research preview, Kahlow noted that doing tests yourself is also advisable — “trying out your model to see what doesn’t work is really important.”

**Gaming** is the obvious use case for what Runway is building, but there are others. Kahlow mentioned **robotics** — for example using a simulated environment to test how a robot works.

Another, more intriguing, use case is to use it to **test agents at scale**.

“Having thousands of simulated environments is much less challenging if you have a suitable model like GWM Worlds,” Kahlow said.

[![](https://substackcdn.com/image/fetch/$s_!RtOZ!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff95952e9-4d93-4707-aa3b-3a41ebe50f0f_1990x1296.png "")](https://substackcdn.com/image/fetch/$s_!RtOZ!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff95952e9-4d93-4707-aa3b-3a41ebe50f0f_1990x1296.png)

But how does an agent know what’s changed in the world — is there a structured state that it can read, or is it just the generated video and audio that it’s consuming and understanding?

“So there’s **no structured state** here,” Kahlow replied. “It’s just observing the same thing you might observe in real life, just \[in this case\] from cameras.”

Sindi noted that GWM Worlds can also be used for “**synthetic data generation** for agents.”

Finally, Germanidis suggested there’s potential to use these world models *alongside* reasoning models.

“You’re maybe using some reasoning \[for\] planning of the scene, and then you’re passing it into the diffusion head that’s actually generating the pixels.”

- **LinkedIn:** [https://www.linkedin.com/in/agermanidis/](https://www.linkedin.com/in/agermanidis/)

- **X:** [https://x.com/agermanidis](https://x.com/agermanidis)

**00:00:00** Introduction

**00:05:17** Runway’s Origins and the Bet on Generative Video

**00:12:23** The Stable Diffusion Story

**00:18:44** Gen-2, Controllability, and the Weekend Hack

**00:23:02** From Video Generation to World Models

**00:28:03** Learning From the World, Not Just Language

**00:35:04** Sora, Runway’s Existential Crisis, and Gen-3

**00:39:39** Why Real-Time Video Is Inevitable

**00:43:06** Interface World Models: Software Without Code

**00:50:25** The Fully Neural Operating System

**00:55:11** World Models for Robotics

**01:02:32** Robot Policies and World Action Models

**01:07:47** The Lucid Dream Test

**01:11:41** Video Agents and Omni Models

**01:23:12** Artists, AI, and Creative Workflows

**01:27:14** Physical AI and the Future of World Models

**Swyx \[00:00:00\]:** Okay, we’re here with, Anastassios from Runway, with, me and Vibhu in the studio. Welcome.

**Anastasis \[00:00:08\]:** Good to be here.

**Swyx \[00:00:09\]:** Congrats on all your success and progress with Runway. You’re opening offices all over the world. Did you envision this when you first started out?

**Anastasis \[00:00:16\]:** Not quite. I think even when we started, we had this idea that, It was more a matter of when, not if, we were seeing the early generative models of 2016, 2017, and just extrapolating, assuming, we resolution, quality increases predictably over time. There’s gonna be a point where most of content will be generated, and that was maybe the initial thesis of Runway was we will need, as a result of those generative models, rethink how creative tools are made. and as we built out the research behind, our generative models, it then became clear that they were useful far beyond that as well.

**Swyx \[00:00:57\]:** And it is more obvious now with, like, the real-world stuff and the world models that we’ll talk about later. I’m just kinda curious how you go from a background in, like, Zocdoc and, computer vision into Runway. Like, take us back to that early conversations with Chris and, whoever else is on your founding team.

**Anastasis \[00:01:14\]:** I was always splitting through those two worlds. One was the I had my own art practice. I was making a lot of interactive art, I think for a long time. and then on the other side, I was working in startups, and I was working as a ML engineer, as a backend engineer at different companies. I’ve always been interested in, coding and computation, and especially interested in simulation and brought it back into my early artwork as well. And at the same time, I was interested in

**Swyx \[00:01:43\]:** The personal site has a few, right?

**Anastasis \[00:01:44\]:** Yeah.

**Swyx \[00:01:45\]:** Is there one that we should pull up? Just in case there’s something that’s like. I just like to go down memory lane.

**Anastasis \[00:01:50\]:** Yeah.

**Swyx \[00:01:50\]:** Okay, what is this?

**Anastasis \[00:01:51\]:** So this was, a project that I made, I think back in 2015, where I built this software that would give, voice instructions to people in a gallery space. So it would coordinate interactions between people. And so it will first give you an identity, like you’re an, architect, you’re 30 years old, and, you like sports. and then it would match you with another person, and you have this completely generated interaction. language models were not quite there at the time, and so it was it was a mix of some templates and some, like, some Markov chain-generated text, and it would just completely simulate these small talk conversations between, everyone in the gallery space. so was always very fascinated on the one hand with, generative models and, like, the early machine learning work that was being at that time. But at the same time, there was this separate thread of simulation and what it means. Like, what can we learn about humans by creating those very simple models of their interactions and their behavior?

**Vibhu \[00:02:56\]:** Did you generate the prompts or, the 30-year-old, whatever? Was it you generating them? How’d you, how’d you

**Anastasis \[00:03:03\]:** Exactly. So the program would just generate- those, from. Yeah, a lot of it would be Mad Libs style of just

**Vibhu \[00:03:10\]:** Yes

**Anastasis \[00:03:10\]:** You have lists of different professions, lists of different,

**Vibhu \[00:03:14\]:** Hobbies

**Anastasis \[00:03:15\]:** Personality types, lists of different, ages, things like that. And then it would just combine those things together. And then maybe the next project we go is, Uncanny Valley, Uncanny Road, which was

**Swyx \[00:03:27\]:** Gans

**Anastasis \[00:03:27\]:** One of the first projects that, we built with, one of my two co-founders, Chris. This was taking, pix2pixHD, which was one of the early image-to-image models that NVIDIA released back in 2016 or 2017. and it was a model that would take a semantic map of a scene and then generate a photorealistic, let’s call it, output. very early days, so it was not very high-fidelity outputs, but it w I think was the first image-generation model that could generate at 1K resolution. And it was all trained on self-driving datasets. So the semantic categories it would support were only, things you would encounter on the road. So it would be pedestrians, traffic signs,

**Vibhu \[00:04:16\]:** Stoplights

**Anastasis \[00:04:17\]:** Bikes, stoplights. And so that was one of our first indications that we built this and people were making all this, like, very surreal imagery of, yeah, a million plus a million pedestrians or a million traffic signs or, like, gigantic humans. And it was a indication that you could take a model that was trained on this very boring dataset, essentially, of, like, not that many interesting things happen when you’re on the road, and then you can repurpose it and go very out of distribution and make something that was artistically compelling. And that was It’s a summary of the thesis of Runway in some ways, that you can take the same generative models, and if you look at them from another direction, if you build interesting tools around them and you give them to artists, they’re gonna do things that you don’t expect.

**Vibhu \[00:05:02\]:** Very cool. I like the, UX of it. You’re just given an empty canvas, try whatever, do whatever. And then the other one, like, you see everyone with wired headphones? Like, that’s, that’s a sign that it’s, it’s very

**Anastasis \[00:05:16\]:** The Apple

**Vibhu \[00:05:17\]:** Yeah

**Anastasis \[00:05:17\]:** Apple, your version.

**Vibhu \[00:05:17\]:** Original ads. Yeah. Take us to today. You’ve been doing this for seven years at Runway. How have we got to this? Like, how do we go from driving simulator data to all this? And you cover the whole stack of generative media?

**Anastasis \[00:05:33\]:** Interestingly, we’re almost back in, we’re, we’re full circle. We’re, we’re now applying our models and beyond creative tools into real-world scenarios. But it was a, it was a long journey. It was very early on we realized the first version of Runway was a way to easily use the, all the open source model of the day, things like pix2pix to. and give them to artists. That was the initial idea, is those models are too difficult to use if you’re not a machine learning engineer. Like, what happens when you give them to artists? Very quickly, we realized we needed to build a research org, inside of Runway, and that happened maybe on year one. And, a lot of the mandate there was. The image-generation models of the time, the video generation models of the time, or there were barely any video generations all the time, but they were not quite there where they could be productionized and brought into tools that would be part of creative workflows. so we need to push the frontier of the research. And so maybe the first four years of Runway, research was almost happening on the background until there was a moment in 2022, with latent diffusion, with, DALL-E 2, where, there was that step function change, and you guys maybe remember around the time.

**Swyx \[00:06:49\]:** I started in this space because of latent diffusion and Stable Diffusion.

**Anastasis \[00:06:54\]:** Yeah.

**Swyx \[00:06:54\]:** Because I was like, “Wow, this is not only, like, feasible, it is doable on consumer hardware.”

**Anastasis \[00:07:01\]:** Exactly, yeah.

**Vibhu \[00:07:01\]:** I think the delta is also huge. Like, I learned pix2pix. Like, this was intro to ML, the TensorFlow, like, Jupyter, Google Colab notebooks were like this, and then you have a sudden step function change, with diffusion and whatnot. Any other ones since that. Like, there were clear examples of what early diffusion were to get to here. Any other changes in key technology research?

**Anastasis \[00:07:26\]:** Between, 2018 when we started and 2022?

**Vibhu \[00:07:29\]:** Yeah.

**Anastasis \[00:07:29\]:** So one of the early work that we did in Runway was solving segmentation, image and video segmentation. It was a very important problem because most VFX involves essentially separating

**Swyx \[00:07:42\]:** Rotoscope

**Anastasis \[00:07:42\]:** Subjects. Yeah, rotoscoping. Extremely manual process. Nobody enjoys doing that. and so a lot of the early days of Runway was building this tool. It was called Green Screen, and it was for a long time the main thing that people were using Runway for. It ended up being used in, Everything Everywhere All at Once and a bunch of other high-visibility films and series. But that was essentially, Runway for a long time was a post-production tool until latent diffusion and generat- Gen-1, Gen-2, happened.

**Swyx \[00:08:12\]:** Cool. let’s, let’s go past that moment. You’ve come a long way. Then you started releasing your own models. Maybe describe that journey as well.

**Anastasis \[00:08:20\]:** Yeah, so we go to the other point, yeah, in mid-2022 when it became clear that we’re doing research at a fairly small scale of compute, and it became clear that, like, scaling laws would apply to, image and video gen in the same way that we’re applying to language generation. So we made a big bet, and I think at so at the time, we signed this deal to build a cluster of a thousand A100s, which at the time we were a Series B startup. That was a almost, slightly irrational decision maybe, but we really believed that if we trained a video model at a large scale, we would get, like, a great model at the end. And at the time, the goal or we set the goal around fall of 2022 of what is, what does the latent diffusion, Stable Diffusion moment look like for video? And at the time, the best model of the time was called CogVideo. it was one of the early video models. It was very 256 by 256 resolution, very not very high quality. and so we decided we’re gonna build out this cluster, and we’re gonna just invest in, like, in building out our own video model. it became clear as we’re training Gen-1 that it was difficult to get to fully. we wanted to build text-to-video, but it became clear to us that an easier starting point would be to start from video to video. Because when you have a stronger conditioning, it’s, it’s an easier problem to restylize an existing video versus generate the video from scratch. And so we released Gen-1 first back in, it was January of, 2023. Yeah.

**Vibhu \[00:10:04\]:** It’s just a fun visual podcast, honestly. Like, if we can see February 2023, what was the state of stuff?

**Anastasis \[00:10:10\]:** It’s so interesting ‘cause at the time when you see those results, you think this is so incredible, and this is like, it’s almost like image generation or video generation is solved. And then you look back a few years after, and it’s like, it’s It’s just like you get used to the results very quickly, with those models. But at the time when we started seeing those results, it was, it felt quite incredible, and the level of, like, quality that you could get. And, so the Gen-1 was a depth-conditioned video model, so it would turn. it would take a input video, it would predict. it would it would first convert it into the depth map, and then we would generate, pixels with a latent diffusion model.

**Swyx \[00:11:01\]:** Yeah, very effective.

**Vibhu \[00:11:02\]:** Yeah. I didn’t realize how distracting the blog post would be. Sorry.

**Anastasis \[00:11:05\]:** Yeah, but, one of my favorite examples of on those, on Gen-1 was both, if you go up to mode three or mode two, there was this storyboard use case where people would make

**Vibhu \[00:11:18\]:** Ooh

**Anastasis \[00:11:18\]:** Would

**Vibhu \[00:11:20\]:** You can mess around with the

**Anastasis \[00:11:20\]:** Make a city out of books or out of boxes, and then they would shoot a video with their phone and then translate it into a photo-photorealistic output. There was all these ways in which those models were starting to be used for storyboarding and also for really. and then if you go to mode four, like, of taking untextured 3D scenes and then turning them into photorealistic output. So we saw a lot of use cases early on where people that were familiar, were power VFX editors would just take a blender, render, and then they would get translated in with Gen-1 or create a scene in Unity and then take a capture a video of it and then translate into, restylize it. So I still think video to video is powerful. I think we had a recent video-to-video model as well, and it’s one of my favorite ways of using those models is essentially using them to use ground truth video as, like, the initial inspiration and then translate into different styles or different outputs.

**Swyx \[00:12:23\]:** But I think we’re gonna go into, like, the rest of Runway and catch people up to speed today. I did wanna cover the, let’s call it the Stable Diffusion controversy, or, what happened with Stability AI, whatever. I think there was a two sides of the story. I think there’s part of that is a normal thing of, like, people, join and leave companies, but what is the, retrospective now that, there’s been some years behind it?

**Anastasis \[00:12:49\]:** Yeah, it’s a very, it’s a very long story to go into. I think it would

**Swyx \[00:12:53\]:** Which I remember you wrote a really long post about.

**Anastasis \[00:12:56\]:** We would probably cover the whole hour to go into it in more detail. But, essentially, there was the latent diffusion paper that came in, I think that was at the end of, 2021. And then Patrick Esser, who was one of the researchers behind, latent diffusion, and he worked at Runway at the time, he built latent diffusion in collaboration with Robin Rumbach and a few other folks back, in the in, CompVis, which was, a lab

**Swyx \[00:13:26\]:** Like a research group, yeah.

**Anastasis \[00:13:27\]:** And, after releasing the early latent diffusion model, they, essentially they were. the goal was to keep working on versions of the model, scale it up, incorporate new data, incorporate new tasks. And Stable Diffusion was the same model, but trained on more compute, and then with a few more tricks, like a classifier-free guidance paper came at some point, I think in the early 2022. And that

**Swyx \[00:13:52\]:** Which, like, was a big prompting improvement.

**Anastasis \[00:13:55\]:** Yeah.

**Swyx \[00:13:55\]:**?

**Anastasis \[00:13:56\]:** That improved results. it was trained on better data, so like, the esthetic subset of LAION, but it was effectively, the same underlying architecture. And there was that big training run, that, happened on Stability’s cluster. Stability financed that run. And looking back at that story, I think it was the work to build and train that model was done. It was a, it was a research project. It was done as part of, like, continuation of the latent diffusion work. It then, I think it the model became very successful, and it, I think there were the. And I think as a result of its success, other companies tried to, figure out the commercialization path for it. But for us, it was very important that we try to, we make sure that we. It was meant to be an open source research project, and so the we decided that we should continue releasing versions of it, since that was the original goal of Stable Diffusion, and that led to releasing Stable Diffusion 1.5. There was maybe a day of, a bit of, miscommunication there, but ultimately that was resolved very quickly within hours. so yeah, there was

**Swyx \[00:15:12\]:** Okay

**Anastasis \[00:15:12\]:** Not a nice

**Swyx \[00:15:13\]:** I just wanted to. you have to

**Anastasis \[00:15:15\]:** Yeah.

**Swyx \[00:15:15\]:** You’re one of the main players in that journey, and so it’s nice to hear from the source of, like, what happened. Yeah.

**Anastasis \[00:15:22\]:** Yeah. I think it’s all, it’s all in the past now

**Swyx \[00:15:26\]:** Yeah

**Anastasis \[00:15:26\]:** I would say. and, like, both companies, Stability took its own path, Runway took its own path.

**Swyx \[00:15:32\]:** Yeah. There’s still. James Cameron is backing the new Stability, whatever they’re doing with the Hollywood studios.

**Anastasis \[00:15:38\]:** Right.

**Swyx \[00:15:38\]:** I don’t know what they are doing. I think one thing that impresses me, and I’m happy to move on, is that back in the that time, let’s say, like 2021, 2022, there was this community of people that you were involved in that was researching all this stuff, right? And, like, from everyone I talked to who was active then, it seemed like it was fairly obvious that somebody would do the hero training run that would produce Stable Diffusion. So, like, I guess the question is, like, you had the you were you had made investments. You were you had the foresight. Is it accurate to say, like, that is reflective of, like, what people were thinking at the time? Or was it still very much like, “Well, we’ll use it as, like, a post-production tool or something. I don’t know.”? Like, where in the sentiment were we that maybe you can think back to, like, what the community was like back then?

**Anastasis \[00:16:28\]:** I reminisce and I think very fondly those early years, from like 2018 to 2022, because it was a very small community that, as you said, were very convinced that this was gonna be a big thing. And at the time, anyone who. Because it was such a small circle and, everyone who would, like, be part of that circle and, like, make projects with it would, immediately get, go viral. so like

**Swyx \[00:16:55\]:** And you didn’t know who they are, right? They’re just some name on a, GitHub or Hugging Face somewhere.

**Anastasis \[00:16:59\]:** Exactly, yeah. So I remember one of the first big viral moments of creative AI was, there was the neural style transfer paper

**Swyx \[00:17:09\]:** Huh

**Anastasis \[00:17:09\]:** That

**Swyx \[00:17:10\]:** Something dreaming?

**Anastasis \[00:17:11\]:** I think it was called neural style transfer.

**Swyx \[00:17:14\]:** Okay.

**Anastasis \[00:17:14\]:** There was also Deep Dream, the puppy slice

**Swyx \[00:17:16\]:** Yes

**Anastasis \[00:17:16\]:** Which was, also really cool. but, yeah, there was this project that, Jim Kogan, who was an early advisor of Runway and one of those,

**Swyx \[00:17:25\]:** Marketing guys

**Anastasis \[00:17:26\]:** Big, creative AI, folks, he literally just, like, showed a video of himself taking the New York Subway and going over the Williamsburg Bridge and then stylized it with, I think in the style of Van Gogh or, like, one, painter. And that was. Like, at the time, that was, like, so cool and it went viral and it was completely revelation to people that you could do this with generative models. And that was only, it was less than. It was maybe 10 years ago. So just, like, as an indication of, like, how quickly things have gone.

**Vibhu \[00:18:02\]:** It’s pretty crazy. Like, even since then, you’ve got people at every level of the stack. You’ve got devs, creatives, artists, hobbyists. You’ve got everyone using it. And for people that tried stuff early, they’ll remember how hard it was to use regular diffusion, right? Like, nowadays, you can use your favorite ChatGPT image gen or whatever, give a sentence, get a beautiful output. But diffusion was like, the whole ultra HD, 4K, high resolution. Like, prompting these things was very different. anything you learned on the tooling side, like from the offerings you guys have now, so like creatives, devs, you really took the. Research and brought it to everyone to use. anything interesting there to share?

**Anastasis \[00:18:44\]:** We had to build the entire model serving infrastructure for video diffusion models. There was nothing else, already, like, because we had Gen-2 was the first text-to-video model, I think, out in the market. So many things that we learn over time. I think the I think the biggest one was, like, we. it was very clear early on that text-to-video was not gonna be the answer. Like, you. Like, people wanted a lot more control than that, and so we invested in, like, control building on top of those models very quickly. how do you use the camera trajectory as control? How do you use an initial input frame as control? So that was a very early learning for us. With text-to-video was, like Gen-2 was an amazing, step function improvement in the quality of video models, but it was used much more in an exploratory way because there was nothing to ground it to. There was no reference that you could bring into it. There was no. You couldn’t really control the camera motion. You couldn’t control the object motion. And so the first year, in 2023, was really all about what are all the interesting ways in which we can condition those models? And it was a lot of just post-training rounds on top of the base model to figure out, like, what, -- how do people wanna control them? And so there was, like, this quick succession of the we it was called Motion Brush, which was you could, like, you could draw arrows and dictate where things should move in the scene.

**Vibhu \[00:20:09\]:** That’s so cool.

**Anastasis \[00:20:09\]:** There was camera control that was you could just describe, like, how you want the camera to move in the scene. And because we work with filmmakers from the most of the history of Runway, we immediately got this feedback and got this, decided that this was worth investing in. And so control ability became a big theme, I think, very early on as we were building, as we were building those models. Something fun that I haven’t really talked about too much was just how Gen-2 came to be out of Gen-1. So it was a bit strange because we announced Gen-2 two months after Gen-1 and

**Vibhu \[00:20:43\]:** We’re accelerating.

**Anastasis \[00:20:44\]:** It was before Gen-1 was even generally available. But Gen-1 was a depth-to-video model, so it would take a depth map and it would convert it into RGB. and we couldn’t get, text or image-to-video to work directly, and that’s why we started from depth to video. but, and we had discussions of like, okay, we need to spend the next six months investing in text-to-video, maybe increasing the compute scale or the model scale, like train a larger model. And I had this weekend project idea, which was, what if I take a model that, starts from text input and converts to depth maps and then use Gen-1 to convert the depth maps Into RGB?

**Vibhu \[00:21:29\]:** It would probably work.

**Anastasis \[00:21:30\]:** And so Gen-2 was that.

**Vibhu \[00:21:32\]:** Oh. The hackathon pipeline.

**Swyx \[00:21:35\]:** The weekend hackathon pipeline.

**Anastasis \[00:21:36\]:** Yeah.

**Vibhu \[00:21:37\]:** But it looks good.

**Anastasis \[00:21:38\]:** And it worked pretty well. there were if you, with the knowledge that it has this, like, two-stage pipeline, you can tell in some cases that the structure of the video looks a bit off because you had to generate the depth first before you go into the output video. But it worked and it allowed us to bring this to our, to users very quickly. But it’s now it’s interesting because, like, people are coming back to this almost two-stage approach. Like, if you look at the Reve text-to-image model that came a few months ago, it had this planner model that would generate bounding boxes before it fed that into the diffusion transformer.

**Swyx \[00:22:19\]:** Yeah, Ideogram also the same day.

**Anastasis \[00:22:22\]:** Yeah.

**Swyx \[00:22:22\]:** I remember that was very strange that both of them came out the same day with the same exact innovation.

**Anastasis \[00:22:26\]:** It’s a small community, I think.

**Swyx \[00:22:28\]:** I’m like, this is like, this is completely coincidental, right?

**Anastasis \[00:22:32\]:** People talk. So yeah, there’s, there’s definitely something into this approach. And, now, like every single like, video generation model in production uses a complex prompt completion pipeline under the hood. I think that’s no secret that there is. That

**Swyx \[00:22:48\]:** Humans are terrible at prompting.

**Vibhu \[00:22:51\]:** I think across the board.

**Anastasis \[00:22:51\]:** Yes.

**Vibhu \[00:22:52\]:** But yeah, I think like the original Sora one blog post even told you that what happens after your input is rewriting your prompt. It’s much more descriptive about what you would want.

**Anastasis \[00:23:02\]:** Exactly. I, And there was the DALL-E 3 paper beforehand that, was the first public, description of the fact that synthetic captions and really detailed captions work really well. And then Sora built on that. Yeah, so it was 2023. We were releasing all these updates to Gen-2, like the camera control, Motion Brush. And there was something very interesting about camera control because it was the first time that you felt that instead of, like, you were creating video, you were creating a short video, you were navigating inside the world. And I think camera control was maybe the seed of some of the ideas that we had around world models and really opening up that research direction. We realized, it was this era and this series of, Gen-1 and Gen-2 models really proved to ourselves, yeah, this is the

**Swyx \[00:23:56\]:** Cool.

**Anastasis \[00:23:57\]:** So this is not the original camera control. This was the updated camera control on top of Gen-3. But yeah, I think it made those models usable to filmmakers, I would say. The so camera control was very popular. And so we realized, there is one way of seeing those models, which is, you’re just as content creation machines, and there is the other way, which is you’re. As you’re predicting video in order to predict video well, you need to simulate the world in an increasing and increasing capacity. And if scaling laws apply on video, just like they apply on language models, then as we scale the compute that we put into those models, then they’re gonna be able to simulate physics, they’re gonna be able to simulate human actions and dynamics increasingly well and predictably well. That was the thesis about around our efforts on world models, and we spin up this research group to just focus on the world models and how do we turn the video generation models that we’re building into something broader and something that would be useful beyond, also content creation as well.

**Swyx \[00:25:04\]:** And that was roughly when?

**Anastasis \[00:25:06\]:** Yeah, so that was in

**Swyx \[00:25:06\]:** Oh

**Anastasis \[00:25:07\]:** In late 2023.

**Vibhu \[00:25:08\]:** Interesting. like, I think, a lot of people have been saying a lot of video gen model companies have all pivoted to world models these days, but like, 2023, you’re posting it. one

**Swyx \[00:25:21\]:** It’s, it’s debatable whether it’s a pivot.

**Vibhu \[00:25:23\]:** Yeah.

**Swyx \[00:25:23\]:** Like, arguably

**Vibhu \[00:25:24\]:** Yeah

**Swyx \[00:25:24\]:** That’s what you always had to do anyway, right?

**Anastasis \[00:25:26\]:** It’s in a way an expansion

**Vibhu \[00:25:28\]:** Yeah

**Anastasis \[00:25:28\]:** Of the applications

**Vibhu \[00:25:29\]:** Yeah

**Anastasis \[00:25:29\]:** Of the models as they become more capable.

**Vibhu \[00:25:31\]:** The early signs, it seems like the original models you guy had, guys had, people would say it’s very not bitter lesson pilled, right? You’re adding, rewriting prompts, you’re having all these one-off things, but that’s just the state of the tech as it was versus the future of as you said, you can scale it up as, we can scale up to world models.

**Anastasis \[00:25:50\]:** Yeah. So it just became. And if you looked at the outputs of Gen-2

**Vibhu \[00:25:56\]:** Yeah

**Anastasis \[00:25:56\]:** It was not. I think it was not obvious to people that this would scale to become a general simulator of the world. Like, you had very limited movement, you had, very low fidelity or low resolution, like obvious mistakes in human anatomy, like all kinds of limitations. But it was just, the idea was that’s just GPT-two, and GPT-two, it can barely generate, like, coherent sentences. Similar, Gen-2 can barely create coherent video, but if you scale it up, you’re gonna. There is no reason why it shouldn’t work in a way. It’s, And I think that was. That’s, that’s always the mindset of Runway is like this extrapolation of, like, if, like, even when we started in 2018 and you looked at the results of the day, you need to look more at the trend of, like, where we were in 2018 versus when we were at the, when the first GAN came out in twenty, four 2014 or twenty, fifteen. And, you started from, like, thirty-two by thirty-two images of faces, and then by the time in 2018, you could generate, street images at the 1K resolution. And it was the same with world models, very early signs of something much bigger.

**Swyx \[00:27:08\]:** Yeah. I was gonna say, like, it’s diffusing into focus. Like, if you look at our visible output from year to year, it looks like a diffusion process itself.

**Anastasis \[00:27:17\]:** Yeah.

**Vibhu \[00:27:17\]:** Especially watching the early, like, old blog posts, you can really see the choppiness, the details.

**Anastasis \[00:27:24\]:** Yeah. Like human civilization starting from random noise and then

**Vibhu \[00:27:27\]:** Yeah

**Anastasis \[00:27:27\]:** Denoising into

**Swyx \[00:27:28\]:** Yeah. Just run it a hundred years.

**Anastasis \[00:27:30\]:** Civilization.

**Swyx \[00:27:30\]:** Yeah.

**Vibhu \[00:27:31\]:** That’s how you’re on track, you’re still noising, right?

**Swyx \[00:27:34\]:** Yeah. I like the way that you guys phrased it when you, announced it in June, which is, oh, that you had a video essay. “The human mind is no longer the center of AI. Our world is.” Right? Which is, let’s, let’s call it the past five years of LLM-based AI is very much like trying to emulate human preferences and human speech. But now that’s, like, mostly solved. I think that’s, like, some of the context of your essay, which you also wrote around the time. And now it’s like the focus is on modeling the world accurately.

**Anastasis \[00:28:03\]:** Exactly, yeah. So the way we see it is, there is that, initial mission statement of DeepMind, which is, solve intelligence and then use it to solve everything else. But I think it’s starting from everything else, could be valuable of, like, starting from. there is just so much complexity, and detail in the world that in order to. That it’s, it’s hard to learn directly from just human descriptions of the world. Like, we’re assuming that, like, language models learn from everything that humans have written about the world, like our own understanding as of, the twenty twenties. And there is just so much that we don’t know and so much that’s not captured by existing text, about both the low level dynamics of the world, like we’re not describing in detail. if I tell you to describe, like, how do you tie your shoes, that’s a very difficult thing to describe in words, but it’s very obvious thing to demonstrate. And so I think there’s been. And there’s, more of X paradox, like we’re constantly underestimating all the complexity that goes into very, like, things that we do subconsciously as humans, and we don’t even necessarily always have the words to describe them. And so in my mind, the simulating the world and simulating, physics, simulating the dynamics of the world has always been underestimated, compared to, we place too much emphasis on the things that are easy to talk about. but there is just all this complexity and richness of the world that if we just try and train directly on that observational data instead of training on how people describe the world, we would learn something new that we wouldn’t otherwise know.

**Swyx \[00:29:54\]:** You think that the present architectural paradigm is fine? You don’t need, like, another layer, like JEPA, like another famous, New York AI leader would say?

**Anastasis \[00:30:05\]:** We’re a very pragmatic research lab. If, we have evidence that an approach works better than the approach that we’re taking, then we have no qualms to taking it. We just have seen no indication that video prediction itself doesn’t scale. And even if you look now, not just our work, but the work of others, you’re seeing in robotics some of the most promising work, starts from video prediction models, and then you adapt them to also the action models, for example. so there is very little evidence that you need something else and that your time is better spent on a novel architectural change compared to improving data and improving the, and scaling the current approach. And so, We don’t have any indication that. the, there is that counterargument that I think there was a tweet by Yann LeCun a few days ago that, understanding the dynamics of the world is very different than, generating, cute videos.

**Swyx \[00:31:05\]:** And your answer is no, they’re the same thing.

**Anastasis \[00:31:07\]:** Yeah, they’re the same thing.

**Swyx \[00:31:08\]:** My cat videos are the same as understanding physics.

**Anastasis \[00:31:11\]:** Right, because if you wanna generate. video models can cheat and, like, they could you could give, like, successive dif shots of the scene in a way that doesn’t require you to simulate difficult physics. There is like, all these different ways in which you can hide the deficiencies of the model, and it’s important not to be too tricked by the performance of the current video models. It’s easy to, cherry-pick examples and think that video models are further advanced than they are. So there is a lot more work that we need to do to improve those models. But in my mind, very similar to language, and, like, we’ve. you go from barely coherent sentences to something that, could hold a conversation with a human to something that could can operate autonomously for a day and, like, create entire code bases. And the main difference, there is some architecture improvements along the way, but the main thing is scale. And so it’s the same bet for video, and we have no indications that this is saturating. Like, we have benchmarks that we use for measuring the physics of those models, and we see those predictably improve as we scale those models. So there is. If you want to Google up, Physics-IQ, is one of those benchmarks that measures how well does the model perform at solid mechanics or fluid dynamics or optics.

**Vibhu \[00:32:32\]:** I’m curious if you’ve seen any emergence, any scaling law around this.

**Swyx \[00:32:37\]:** Yeah, he’s saying there is a scaling law, right?

**Anastasis \[00:32:39\]:** Exactly.

**Vibhu \[00:32:40\]:** Yeah,

**Anastasis \[00:32:40\]:** So the way those models, those benchmarks work is you. the researchers have gone and, like, captured, a few videos that are representative of different physical phenomena, and then you can take the first frame and then pass it through an image-to-video model and then generate a rollout that shows what should happen next. So you have, a ball hanging from the ceiling, and then you use that as input, and then you the model predicts how the ball should fall on the ground. and this measures. we have an intuitive understanding of physics. I know, you can imagine what will happen next if I drop this bottle. So it’s measuring that same intuitive physics understanding of those models, and we’ve measured that at different model scales, and we see, and compute scales, and we see that the score on physics IQ predictably improves. There’s other, tricks and techniques that you can make to improve the score even further, but even scale alone helps, in the model learning better physics.

**Swyx \[00:33:40\]:** My main sympathy with Yann LeCun is the, Plato’s cave allegory, right? Like, you’re, you’re, like, learning on the output of a thing, not the internal process of a thing, and it’s very noisy. And, if only you could observe the internals of a thing. It’s hard to observe the internals of a human mind, but you can very much observe, or at least we have a whole branch of science and physics that we’re ignoring on how to model Physics and movement and, gravity and, other interactions. and we’re just, like, throwing away all of that and just saying just scale data, which is very much the lesson of unsupervised learning, but it feels wrong. that’s the main idea.

**Anastasis \[00:34:21\]:** I think the history of machine learning is, at large, it feels wrong.

**Swyx \[00:34:25\]:** Yeah. It’s a bitter lesson, right? Yeah. It’s, it’s, it’s the simple answer to that.

**Vibhu \[00:34:29\]:** I guess, how much can you scale? So, like, even on, let’s say, the video generation side, like, there’s one side of video understanding. Video generation, are we still gonna have tools where it’s like, I wanna generate two hours, twenty hours? there’s a infra way to do it in batches and stitch it together, but, like, do we just keep scaling? Do we just continue long generation consistency, all that at scale? And, like, tying it into where we’re at now from we looked at Runway two to four point five

**Anastasis \[00:34:58\]:** Yeah.

**Vibhu \[00:34:58\]:** Like, technically, what advancements have we made to today, and then where do you see things still going?

**Anastasis \[00:35:04\]:** So part of the answer is definitely scale. and that was. We learned that lesson in a big way for with Gen-3. So Gen-3 was the model we released the year after, like in 2024. That was a few months after Sora was released. so yeah, there’s an interesting story of that came to be as well. Gen-3 for us was, the first time that we really needed to build. we had to learn all the lessons that the language model world learned in two in three years in the span of a few months. one of the biggest changes of Sora was using diffusion transformers instead of convnets. So a lot of the early, latent diffusion models were all, convnets for the diffusion model part. And the diffusion transformer paper came at some point in 2023, and it showed scaling laws for image, diffusion transformers. And we realized at that point that we needed to invest in infrastructure for model parallelism, for really scaling training to larger than, a few billion parameter models. And we spent maybe the, most of the fall of 2023 building out our infrastructure for distributed training. And we had a lot of false starts and a lot of failure in trying to scale, image and video diffusion transformers. And at that point, February 2024, Sora comes out, and the results are

**Anastasis \[00:36:35\]:** Very much superior to what Gen-2 could produce. There were a lot of, a lot of chatter on Twitter about Runway. Runway’s done. like, there is no way Runway will catch up. And if you remember, also OpenAI in the early twenty-It felt very, like it’s a

**Swyx \[00:36:56\]:** To the moon

**Anastasis \[00:36:57\]:** It’s a formidable opponent now, but at that point, it, they were on the top of their game. nobody could even get close to them. There was maybe Gemini was just the first version of Gemini had just released. So when OpenAI came with Sora and it was such a big jump of like quality, it gave me, there was like an existential crisis for a few hours. But that, I think the amazing thing about Runway and like I think the, we’ve been around eight years now, which is almost we’re dinosaur in AI, and we had to like, we had there was a lot of those moments we had to learn, adapt very quickly and build out skill set in the team that we didn’t have. And so, if you ask anyone what is their favorite time at Runway that was there during that time, it was that push in like three months to get to a model better than Sora. and it, we scaled 10x the model scale, the model size and the, compute that we were training on. we figured out model parallelism. We had zero expertise in that. And then we came out with Gen-3 during that summer. So that was a big turning point, I think, for the company where the research org grew very quickly, and we really started pursuing this vision of the general world model, in earnest, I think after Gen-3 was out.

**Swyx \[00:38:12\]:** Yeah. that’s the amazing thing about building when you’re building. There’s no stack to. You have to invent everything yourself. You have to be completely full stack. Now I think like there are inference specialists like Fal or whatever that can help with like, model serving, and I think you guys work with them as well. but yeah, like it’s, it. But at the time, it was just. It’s very interesting to think about what you do when Sora comes out and people are questioning whether your company should still exist.

**Anastasis \[00:38:41\]:** Yeah. And yeah, there was no, there was no VLM of diffusion models. Like, we had to build the whole model serving infrastructure and make things efficient. And a few months after we released Gen-3, we released the Turbo version, which I think was the first step-distilled model in production.

**Swyx \[00:38:56\]:** That was a whole trend that we covered as well. Yeah.

**Anastasis \[00:38:59\]:** So that allowed us, to serve those models at the larger scale, ‘cause I think the first version of Gen-3 was quite, expensive to serve.

**Swyx \[00:39:09\]:** I think the whole like trend in like consistency models, Lightning and, Turbo and all these things somehow didn’t really stick around. I don’t know if you have any reflections on this. Because at the time, I was like, “Well, everything should start with a distilled model first, and then you can upscale,” right? It. your bigger models just turn into fancy upscalers, but like you should always draft with a smaller model and faster model, right? Because you can get it so quickly, like near real-time.

**Anastasis \[00:39:39\]:** Yeah. I would not be so sure to say that didn’t stick around. I think that, it’s, it’s likely to. that there is a lot of step-distilled models that are actively used in production. there is still a gap in quality compared to the, non-distilled model. but in my mind, we’re still. there is a two to three year offset from language models. So the things that, So it’s just a matter of time before there is better distillation techniques. we use. Right now we have a real-time model core character that I think is the largest deployment of real-time video models, that’s a step-distilled model, and it’s actively being used. It’s a very specific use case compared to a general video model. So this is a

**Swyx \[00:40:27\]:** Very cool, by the way.

**Anastasis \[00:40:27\]:** This is avatars stuff, right?

**Swyx \[00:40:28\]:** Consistency, character.

**Anastasis \[00:40:30\]:** Yeah. So this is a talking avatar, model. we were able to. we optimized the hell out of it, and it generates at 24 FPS, and it’s a, it’s a step-distilled autoregressive video model. So if we look at our world model direction, a big component of it is starting from the bidirectional diffusion that generates entire video at once and making autoregressive shows. So you generate one frame or a few frames at a time. so there’s a lot that goes into that pipeline of getting to a real-time model. It’s first you need to make it into a causal autoregressive model, and then you just turn it into. You need to do some additional step distillation to get it to be real-time. and I think that part is just starting. I’ll be very surprised if we’re, two years from now, we don’t primarily use real-time models. To me, real-time video generation is just inevitable that, it has much better user experience, it’s much cheaper to serve, and, the quality gap between the base model and the real-time model is only gonna close as we figure out better, distillation techniques. And we made a lot of progress there internally on maintaining the quality of the base model when we distill them.

**Swyx \[00:41:49\]:** How much of this is transferable? So is it the same base model? Like if you’re doing diffusion across the whole sequence and you’re converting it to step autoregressive distillation, is this like distillation where you still need to train both, you can use the same base and converter? What’s that process like to go from regular model to something that’s real-time on a technical level?

**Anastasis \[00:42:11\]:** So the nice thing about diffusion models is you have, two axes of distillation. So there is the. You can distill to a smaller model, which resembles what you do in LLMs, or you can distill in terms of taking less steps, less diffusion steps. So you could take a model that generates in fifty steps and generate in four steps and get to, You have some performance, degradation, but very often you get comparable outputs. So you can even take the large frontier model and distill it with step distillation and get to a real-time performance, and that’s what we’ve seen. So, depending on the use case, in some cases we might also serve with a smaller model, but in a lot of use cases, we just use the

**Swyx \[00:42:56\]:** Step distillation

**Anastasis \[00:42:56\]:** The frontier model, and we’re able to make it work in real-time.

**Swyx \[00:42:59\]:** I think this might be a good time to cut over to his laptop to show off some of the real-time stuff that you’re doing.

**Anastasis \[00:43:06\]:** This is one of the research updates that we did recently. so we’ve been working and f in getting our general world models to, different applications. one of them that we think is very compelling is using general world models as essentially, an interface, a universal interface to software. This is a version of our world model that’s called an interface world model. and the idea is that it essentially, replaces, the, front end of a software application. It renders the pixels directly of an interface and is trained to predict what happens next as a result of, a click or another interaction you have with the interface. So this is all pixels. it’s there is no HTML, CSS, React that’s powering this interface. This is directly at the output of our real-time, video generation model, and it takes clicks directly as input.

**Swyx \[00:44:09\]:** And drags, click and drag.

**Anastasis \[00:44:12\]:** Right. So it supports

**Swyx \[00:44:13\]:** Ooh.

**Anastasis \[00:44:14\]:** Yeah, clicks. It supports drags. it also supports scrolling. and the amazing thing about this is that you can effectively describe in the prompt how you want different elements, like what do you want the behavior of different elements to be. So it’s almost you’re you can turn, an interface from, markup language description of, like, an HTML interface, and instead you can just describe the interface. if I press this button, I expect this to happen. If I press this button, this should happen. And it’s useful, we believe, both for prototyping, for, like, just testing, like, what different interactions would feel like. you can also add audio to it. So it’s a video audio generation model. So you get you essentially can describe both what the visual outcome should be of your click and also what the if there is a sound effect that comes out of it. So we believe that’s gonna be a much more flexible way of building software. Just render. It just, in why generate the code that generates the pixels? Just generate the pixels directly.

**Anastasis \[00:45:18\]:** It’s the end-to-end philosophy applying applied to front ends.

**Anastasis \[00:45:25\]:** So we think there is a few interesting use case. So you can build creative tools on top of it.

**Anastasis \[00:45:32\]:** We think that, for any use case that involves a lot of exploration or, like, educational use case where you wanna learn about a new concept and you want some visualization and like, and open-ended exploration, we think those this is a very powerful, approach. you can imagine new forms of, design, industrial design software that could emerge as a result of those models. And this is all, generated in real-time as well. So, you can build a lot of interesting camera transitions and forms of interaction that are very difficult to build otherwise. And one way in which we evaluate this is what if you try to generate the same interface with Claude by just, prompting Claude, “Here’s an image reference of my interface that I made in Figma or that I created somewhere else. create this particular interaction,” which in this case it’s, drag that object, upwards. and beyond it being slower, it’s also very difficult to capture some interactions by just fully, with just LLMs. So we think that this is likely to be the way that a lot of the future, like, software in the future will be created. and one of the additional benefits is personalization might be a lot easier done with those models. Like, you can essentially try out different prompts based on who is visiting the interface. You can, more easily, prompt engineer the interface to have larger size, text for more accessibility reasons, or you can make this or, like, if you have a particular aesthetic preferences. So we’re very excited about this approach. It’s early days, and I think we’ll need to, make it more cost-effective as well to serve those models ‘cause, running a real-time video model versus just purely rendering HTML, there’s -- the computational needs are much higher. but we do see a lot of potential in this approach to building front-end interfaces.

**Swyx \[00:47:47\]:** So we covered this similar thing with Flipbook before with our, Ethan Hara episode with Groq, video. And yeah, I think it’s very engaging visually. I think it’s maybe very good for education, but it’s it does sound expensive. I think there’s an upper bound to how expensive it will be, though, right? Like, the inference cost will go down over time. You’ll figure out ways to optimize it. Effectively, when it pauses, you don’t you’re not receiving human input. You don’t have to generate anything, right? So.

**Anastasis \[00:48:14\]:** Yeah, you could also. Like, in this case, you have ambient motion, so there is parts of the screen that might. if you’re let’s say you wanna, visit Paris and then you get this interface that allows you to explore.

**Swyx \[00:48:29\]:** People walking. Yeah.

**Anastasis \[00:48:29\]:** You have people walking or, like, things happening. But, it’s, it’s a no Yeah, it makes it more expensive because you need to run the model all the time. Maybe you have some looping mechanism so you don’t need to do that. But all those things, I think, is stuff we’ll need to figure out.

**Swyx \[00:48:44\]:** Yeah.

**Anastasis \[00:48:44\]:** I think our first consideration is let’s make this clearly find some use cases where it’s clearly a much more compelling interaction compared to traditional interfaces. And then it’s a matter of time before it becomes more cost-effective to serve.

**Swyx \[00:48:58\]:** Yeah. When it comes to the people walking, I think the approach that makes the most sense to me is Nick.

**Anastasis \[00:49:04\]:** Nick.

**Swyx \[00:49:04\]:** Oh, God. I keep messing up their name. With Chris Manning and Fanny Yan. I don’t know if you’ve come across them, where they. Mapped to some game engine. I think it’s Unity or something, or Godot. And they you can script some NPC behavior behind that and train on that. Whereas here, you can really imagine whatever you want. Like, that is a UI, right? Like, and it feels, like, more tractable, I guess, to, create a world model of software that is interactable because we have many of examples of that, and you can, do your fancy RL environment stuff on that than it is scaling up to embodied and real-world physical use cases. But this is a nice first step.

**Vibhu \[00:49:43\]:** Or, there’s the opposite of you have, like, one B models, three 50 million parameter language models. It just gets so small that they’re just predicting, like, fishes moving.

**Swyx \[00:49:53\]:** Small models are now 120 B, so.

**Vibhu \[00:49:57\]:** Ultra mini on device.

**Vibhu \[00:49:58\]:** But, no, I think it, like, it puts it into perspective, at least the car one for me, like, the applications, right? The amount of work to do that, sure, you only make one model year car per year, but applying this, it’s also a cost-saving to have to manually make all this, right? So it opens up a lot of possibilities, too. I’m curious if you extend this out two, three years, so where do you see things going even further?

**Anastasis \[00:50:25\]:** Effectively, the end game of something like interface world models is you have, a fully neural operating system. So I think, Andrej Karpathy has written about that quite a while back. But it’s, You, I think to me it’s, it’s a bit, it’s a bit odd that, we have, for example, with an interaction with an LLM of today, you have this LLM that can talk to you about anything. It can You can take the conversation in any direction. You can It’s very general, so it can solve all those different tasks, but you interact with it through a very rigid interface. And so to me, it’s just a matter of time before the interface itself becomes learnable and becomes, part of the whole loop of, like, you’re not just delivering. You’re delivering an application end-to-end, and that means you’re delivering the language model, but you’re also delivering the render and the pixels and that’s also a learnable component. And the concept of applications might not necessarily. I think we’ll need to figure out new abstractions for software. the concept of application comes from this idea that you need, separate code bases to describe, to, for, to power each individual, tool and each individual application. But you might think of something a lot more unified if you’re. if you have, a video model that’s generating the interface as you go. so it can take context from an LLM and allow you to combine different functionalities that traditionally would live in different applications. So it’s a, it’s a way to solve, software end-to-end, effectively. We also see this as a powerful way to train computer use agents as well. so this is, one way to see this as. And in general, with world models, there is those two directions. One is world models for humans and world models for

**Swyx \[00:52:24\]:** Agents

**Anastasis \[00:52:24\]:** To train agents.

**Swyx \[00:52:25\]:** Yeah.

**Anastasis \[00:52:25\]:** And so for every new work of, world models that we do, we have this both uses become possible. So this is a powerful synthetic data generator for training computer use models. It could become, a live, RL environment that you could use to do online RL with a computer use agent, and you can get wide diversity of different interactions, kinds of interfaces, just generated on the fly that, to improve the how robust the, your agent, becomes. So that’s the same also with the world models that we’re working on for a robotics use case as well.

**Swyx \[00:53:02\]:** Is there a research breakthrough that you’re Waiting for that would unlock the next set of use cases that you really wanna pursue?

**Anastasis \[00:53:10\]:** Long context is a very important one, so being able to maintain consistency for long periods of time, and that depends on the use case. So for our characters model, for example, or for the interface world model, it’s easier to maintain long sessions of interaction. If you go into more open-ended worlds that you navigate and you take arbitrary actions in, we, like, there is more the context at which you can and duration which you can generate becomes limited much more quickly.

**Swyx \[00:53:40\]:** Yeah.

**Anastasis \[00:53:40\]:** So we see more degradation and error accumulation happening. so the biggest challenge with autoregressive models is error accumulation, is you’re feeding generative frames back into the model to generate the next The next frames. And if there is any small errors, they accumulate over time. That’s not a new problem. It’s a problem that LLMs also have, and we’ve seen the ability to generate now really long outputs. So it’s a solved problem, but it’s definitely still a challenge.

**Swyx \[00:54:08\]:** Yeah. And what is the state of the art? so for Grok, it would be like 10 to 20 seconds of context going in there for video.

**Anastasis \[00:54:16\]:** With our characters models, we’re able to generate up to 30 minutes of video autoregressively.

**Swyx \[00:54:21\]:** Yeah. But that’s just for the avatars.

**Anastasis \[00:54:24\]:** Yeah. So if we look at, GWM Worlds, which is more our open-ended world exploration model, it’s, it’s on the order of a few minutes, which is Yeah, so

**Swyx \[00:54:35\]:** Probably enough for people because you have to cut to the next scene anyway, right?

**Anastasis \[00:54:40\]:** Yeah, it’s not, it’s not the ideal game experience if you have to restart every few minutes. So I think. But, I think it’s. Yeah, for certain kinds of game experiences, you can work around it. ideally, you are able to just generate forever, and it doesn’t, it doesn’t degrade. And I think that’s a matter of time before we get there.

**Swyx \[00:54:59\]:** Yeah. Genie has, like, one, max one minute?

**Anastasis \[00:55:01\]:** Right. Yeah.

**Vibhu \[00:55:02\]:** This was your. You did a study on robotics. I think I also have just your Runway Robotics page, though. Is this better?

**Anastasis \[00:55:11\]:** So last year we released Gen-4.5, so that was our latest base model. We’ve been As I mentioned, we’ve been doing all this work in world models, and which essentially a lot of our approach to world models is how do you take a bidirectional diffusion model and make it autoregressive and make it accept actions? So instead of being a video you watch, it becomes a simulation that you step in, and you can, control it every step of the way. You can explore counterfactuals, like what happens if I take this action versus if I take this action. And GWM-1 was the it’s the world model that we built on top of Gen-4.5. So we did all this autoregressive and like, distillation, auto-regressive and then step distillation on top of Gen-4.5. And one of the biggest use case that we saw for GWM-1 was in robotics. One thing we like to say is we as we scaled video models, we accidentally, created one a state-of-the-art model for robotics, by just scaling video models. So we realized at some point, mid last year that robotics labs that are coming up to us and asking to use video models for synthetic data, asking us to post-train our video models to work really well for robotics, so that they can use that to generate variations. That was the first use case that we saw. And then increasingly became clear that the models will be useful beyond just creating synthetic data to train robotic policies. They would also be very useful as simulators. So that means that you can use, a video model online to test how your robotic action model performs. So you can take an action role and then get the outcome of the action inside the world model and then continue that loop like this closed loop simulation. And you can use that to evaluate how well your robotics model works. and the biggest thing that I think you need to solve if you want to build a simulator is establishing real-world correlation that if you take an action inside the world model, if you take the same action in the real-world, you get a similar outcome. So that was the goal of some work that we did earlier this year. So if you go to the first link. So that was, essentially wanted to establish that, real to sim correlation for our world model, so that if you do a series of actions inside the world model and if you do the same actions in the real-world, you get similar outcomes. And we took our GWM-1 model and we used some benchmark data that there is this Roborina, benchmark that’s very commonly used to evaluate how well do different action models perform. And we use the same scenarios and settings and embodiments inside our world model, and we measure the correlation of how well did the action model perform inside the world model versus in the real-world. And we saw that we could get very good correlation between our world model and reality. And that means that if you want to evaluate how well your robotic policies perform, you can scale that much faster inside simulation instead of having to do that with actual physical hardware. And so that was a first indication that our models could be, quite useful in robotics. And we saw as we were working with robotics labs that became like the first use case where they could use video models in a way that feed into their training pipeline.

**Vibhu \[00:58:40\]:** Can I ask what

**Anastasis \[00:58:41\]:** Yeah

**Vibhu \[00:58:41\]:** The difference was from four point five to solving that? So the sim to real gap has always been the issue, right? You train a robotics model on video data, it doesn’t generalize to real-world, and the simulation had an issue. So seems like you solved it, but how?

**Anastasis \[00:58:56\]:** Yeah. So a big problem with simulators is, if you’re trying to simulate rigid objects, like it works quite well if you can describe the physics of objects very accurately, then you’re able to use, Isaac Sim or MuJoCo or one of the traditional simulators. But for more complex interactions with cloth, for example, or, like slippery surfaces, with the all the complexity that you want to be able to solve with the manipulation, with an action model that solves manipulation tasks, it’s very difficult and so time-consuming to build, for each of those environments and each of those tasks, build the simulated version of that, the digital twin of that environment. Whereas with a world model, you just need to provide the first frame and then you just can roll out the policy inside the first frame. So whereas, we compare it to methods that required like 3D scanning an environment and then 3D scanning each individual object before you can now, you can bring that to simulation. whereas with a world model, you just take a picture of the environment and then you’re able to test how your policy performs. Our general thesis on robotics is, there is companies that are leveraging a lot of teleoperation data to train robotics action models. There is now companies that are using, humie data, which is, essentially human, egocentric video where humans use robotic creepers to perform different manipulation tasks. And then there is companies that are focusing on egocentric data, which is, you strap a GoPro on someone’s head and then you capture them performing a task. We think that, and all those are great source of data for training robotics models, but the most plentiful source of video data is third-person video data. It’s And if How do we as humans learn how to perform different tasks? A lot of it is by observing others perform those tasks. We don’t learn from first person. We do some trial and error and like, to learn different things, but. Ultimately, a lot of what we learn how to do in the world, we learn by watching other people do it. And that’s how when you’re pre-training a video model, you’re essentially doing that. It’s a lot of third-person video footage of people performing different tasks in the world, people doing sports, people doing household tasks. And our main thesis is that video pre-training, once you do that, you can then adapt a model to be useful in robotics use cases with way fewer hours of actual robotic data. So you require way less teleoperation data, which is very difficult to scale. and even if you look at egocentric data, which is a bit more easy to scale compared to teleoperation data, which requires actual hardware,

**Anastasis \[01:01:55\]:** It’s still three hours of magnitude less of that exists in the world compared to third-person video data out there. And so our thesis is and generally, like the most plentiful source of data will ultimately wins. Third-person video data pre-training is the right starting point for models that, you want them to generalize and be able to deal with new environments, new tasks, things that you haven’t seen during training. That’s the motivation for why we think our models are especially useful in robotics, settings, and we’ve seen that to be the case, as well.

**Swyx \[01:02:32\]:** You said pre-training. So maybe it’s like third-person pre-training, first-person SFT? Is there like a curriculum that you can introduce?

**Anastasis \[01:02:41\]:** Exactly. So if we look at GWM Worlds, so GW so GWM Robotics. So digitally in robotics, it starts from Gen-4.5.

**Vibhu \[01:02:49\]:** It’s the same video diffusion backbone, right?

**Anastasis \[01:02:53\]:** Exactly, yeah. So you start from the base video model, the one you’re using to generate, cats and dogs and other interesting stuff, and then you, fine-tune on a very small number of hours of robotic data. So it’s something on the order of hundreds of hours compared to if you were to pre-train a robotics model. The current pre-trainings go up to, a hundred thousand or like millions of hours of data. And you’re able to get quite good performance, quickly, because the model leverages all the things that it has learned about the world, physics and human dynamics and the tasks that people care about from pre-training. And ultimately, you want those models to generalize. You don’t want to just be able to perform the tasks that it has been doing training. And the diversity of actions and environments that you have with a pre-training video dataset is much larger than, what you can realistically capture manually.

**Vibhu \[01:03:54\]:** How is the scale looking like for the post-training? Like, do you still wanna do, is it like roughly ninety percent of the compute in regular video diffusion model and then scale up a lot, or do it like we want different robotic models for different tasks, or just the one base really good world model can also apply to robotics?

**Anastasis \[01:04:14\]:** So currently, we are post-training our models for specific, embodiments that we for particular partners. So if they have a particular single-arm robot or a bimanual robot or a humanoid robot, we would post-train our GWM robotics model on their particular dataset. Over time, we see the different variants of GWM unifying. Like, I would expect, if a year from now or two years from now, you have a single world model that can simulate manipulation tasks, it can simulate navigation, which is a lot of the gaming world models are navigational world models. You’re moving around the space, and it will also simulate human behavior. So that’s the character models. So instead of having three different models, you have a single model that’s able to. ideally, you’re able to simulate what it’s like to be in the world. You’re moving around an environment. You’re maybe performing different tasks. you’re talking to other people. And that happens with, the same, a single real-time video model that’s generating that.

**Vibhu \[01:05:17\]:** Do you think you can solve self-driving? So if you are learning to drive a car in a simulator, you have a world model. Your robot is car can manipulate so many axes. How far off are you from something like that?

**Anastasis \[01:05:31\]:** So world models

**Vibhu \[01:05:32\]:** Or a really good ADAS system?

**Anastasis \[01:05:33\]:** World models are definitely being applied to, self-driving, research right now, mainly for evaluation use cases, but our focus has been more on robotic manipulation. We’ve done some work on AV, world models as well. but yeah, we do think that world models are and video models are the best starting point for both simulators and also policy and the action models. So that’s, that’s the other side to this, is that once you have a great world model, then you can just add an action head, and it can predict actions as well. One way to think about it is if you take the starting frame of a scene with a robotic arm and you ask, you prompt the model, generate the arm picking up an object, it would And if it generates an accurate enough video, then it should also be able to generate the exact poses, in 3D that the arm should take to perform the same action. So this is the direction that’s now the popular term for it is world action models, which is you’re starting from a video model, and then you’re adding an action head to predict the actions, and it becomes a policy, essentially.

**Swyx \[01:06:43\]:** One thing I’m also impressed by is how much data you need to train these kinds of models. You probably can’t say exactly how much, but like, the original, diffusion models, and from what I know, even of the open source Chinese models, it’s not that much data. Isn’t it surprising?

**Anastasis \[01:07:02\]:** What do you define as much data?

**Swyx \[01:07:05\]:** Yeah, and it just comes, goes in. Is the token count still relevant?

**Anastasis \[01:07:09\]:** So it’s a bit more complicated and,

**Swyx \[01:07:11\]:** What is just gigabytes, right?

**Anastasis \[01:07:13\]:** Yeah, hours of video, right?

**Swyx \[01:07:15\]:** Yeah. Yeah. I feel like something that’s interesting is it seems like the, let’s call it tokens to param counts in language models has really, maybe they’re three years ahead or whatever, seems to be a lot higher than, video models still, even though technically video has more information, per bit. I don’t know if it seems intuitive or maybe there’s just a lot of, like the variability between a pixel to the next pixel is not that high. So, like, maybe there’s just a lot of information that is repeated.

**Anastasis \[01:07:47\]:** My answer would be it’s still very early. Like, the training video models will scale way further than it

**Swyx \[01:07:55\]:** Yeah

**Anastasis \[01:07:55\]:** Currently is, and you’ll have capabilities that go much further than the current models can do. So one thought experiment that, I like to use, it’s, it’s almost like the Turing test of video models or like the Turing test of world models, go, I call it the lucid dream test. It’s you have a

**Swyx \[01:08:14\]:** You mean the actual person lucid dream?

**Anastasis \[01:08:17\]:** It comes from this idea

**Swyx \[01:08:18\]:** Lucid rains, right?

**Vibhu \[01:08:19\]:** Lucid dreams is telling you’re dreaming while you’re

**Swyx \[01:08:22\]:** Yeah.

**Anastasis \[01:08:23\]:** Yeah, exactly. So lucid dreaming is when you realize you’re

**Swyx \[01:08:25\]:** In a dream

**Anastasis \[01:08:26\]:** Inside a dream, and then you

**Vibhu \[01:08:28\]:** Play around

**Anastasis \[01:08:28\]:** Be able to control what happens in

**Swyx \[01:08:30\]:** No, there’s also an inference guy called Lucid Rains. Yeah. Or quantization

**Anastasis \[01:08:33\]:** Very prolific, person. Yeah. So let’s say you have a VR headset and you’re in a room with and you’re wearing a VR headset, and that VR headset, most of today’s VR headsets have a pass-through mode, so you can see directly what’s in front of you in the world, or you can render something inside the VR headset. And there’s gonna be a point where those interactive real-time video models become good enough where you wear the headset and you’re in the same room and you’re walking around and you’re kinda and you’re interacting with objects. You’re able to move freely in that room and do, and interact with any object. And at the end, someone asks you, “Did you were you using pass-through mode, or were you -- or was this, rendered or generated, footage?” And if you cannot tell for sure if that was what you were seeing as you were interacting with and moving around the world was generated or it was, pass-through mode and was just what was happening in front of you, that’s an indication that the models have become good enough. And we’re not, we’re not close to that yet. And a lot of it is just this idea of really simulating dynamics and counterfactuals well. Like, if you ask a video model to generate a person scoring a goal versus a person failing to score a goal, it would do a better job at scoring the goal because there is a bias from the training distribution. There is a lot more videos of the person succeeding at scoring the goal. But if you have an interactive model, you want it to be able to generate counterfactuals. Like, if I take this action versus this action, you want it to generate equally realistic outcomes. so that’s, I think, the big gap between video models and world models is that idea of the counterfactual generation. And if you want a great model for robotics, you wanna simulate failure very well, because whether you’re using it for evaluation or you’re using it as a in an online RL loop in the future, you wanna be able to have the model try and fail to do things and improve. and so in order to do that, you need to be able to simulate things failing.

**Swyx \[01:10:42\]:** This is the only domain where you have too many successful examples and not enough bad examples. Should be easy to generate failure.

**Vibhu \[01:10:51\]:** Oddly enough, I think, like, early image video models weren’t good at being human realistic, right? Like, you see aa lot of the high-res 4K, like, professional photography, but not just everyday life, like normal picture, right? Everything looks like it’s professionally generated, like professional pictures, but not just like normal, like, messy cables on a desk.

**Swyx \[01:11:13\]:** Okay, so there’s, there’s this stuff. one thing we also covered that you guys have, video agents that you launched. I guess, how does the traditional, let’s call it frontier, like, autoregressive LLMs, like, feed in, to all this? They’re driving ro your robotics models, or are they driving others, your video agents, production, anything where you see the overlap of autoregressive and diffusion, let’s call it?

**Anastasis \[01:11:41\]:** Yeah, so harnesses are really important across all those different use cases. So we have this video agent, which is essentially an LLM that is very effective at tool use of different, image models, video models, and helps you through creating a project end-to-end. So, very often in, like, a traditional advertising flow, you have a brief, you start from it, and then you generate some a storyboard, and then you generate the video. A video agent and, or runway agent helps you through that whole process, and it helps you also analyze performance data. For example, how well did this ad perform versus this ad, and then generate me more of the based on those learnings, figure out what to generate. We think that the harness is a very important piece of the pipeline. as I mentioned, all the video production, all the production video models use some prompt completion that happens, and we expect, that to become more and more complex and more, you generate longer and more detailed descriptions before you use the diffusion transformer. I do think eventually, there’s increasingly this unification into omni models where you have the you’re training the models end-to-end to both do autoregressive text prediction and also, diffusion as well. So you’re predicting the next token, of like you’re, you’re maybe using some reasoning and planning of the scene, and then you’re passing it into the diffusion head that’s generating the pixels.

**Swyx \[01:13:10\]:** Yeah. I think currently maybe only Gemini and Qwen do it. I-I’m not sure which of the Chinese models are omni, but yeah, it’s, it’s not, it’s not a very well, popularized modality, I guess.

**Vibhu \[01:13:25\]:** It’s an interesting use case when you think about it, right? Because not only do you have to end at like language model reason, diffusion had generate, you don’t have to output there. You can go back in to feed that output to the same model, reason again on improvements, and it can do a lot of loops just in its own. I guess the question is like, do we need that or can we just do agent scaffold, like do it outside the model? Is there a big benefit to doing it in?

**Anastasis \[01:13:54\]:** I think there’s generally the trend of something is first done by a harness and then it becomes part of the model, right? So you had the chain of thought prompting where you had to do this super detailed system prompts to

**Swyx \[01:14:07\]:** Yeah, step by step

**Anastasis \[01:14:08\]:** Get the output. And now the model generates the reasoning trace by itself before it gives you an answer. And in the, in video models similarly, a lot of the video models of the early days were single-shot video models, and you had to use some orchestrator to turn, generate multiple shots in parallel, and then turn it into an actual video.

**Swyx \[01:14:28\]:** Or in ComfyUI, just all over the, all these nodes.

**Anastasis \[01:14:31\]:** Yeah, like a spaghetti workflow. and now you have multi-shot video generation where you have the you directly generate multiple shots. And there is a benefit to that because then the video model learns some. to generate a single shot well, you need to figure out a lot of stuff about the world. to generate multi-shot video well, you also need to get some, like, video editing instincts. Like, you need to figure out what is the right pacing of shots. And also, LLMs are not that good at it. Like, they’re not that great video editors. If you ask a LLM to take some videos and then auto-create a edited video out of that, it would feel uncanny. So I don’t think LLMs are that good yet at being video editors. And I think there’s benefit to learning that end-to-end. so I would expect, the training generally is the things that, you need the harness for eventually get injected into the model itself, and you learn that end-to-end.

**Swyx \[01:15:33\]:** Do you find that you need to hire engineers who can. or researchers who are also artists to infuse that taste, or do you have artists in residence to distill them?

**Anastasis \[01:15:44\]:** We have a large creative team that’s very actively involved in the, in training those models, like on the, in every part of the way. And like, how do you caption video as well so that you capture the stuff that you need for, like, the cinematography, the aesthetics, the camera direction in as detailed ways as possible so that you’re able at inference time to elicit that through the model? we have our creative team also does a lot of evaluation of like, what constitutes a usable video out of those models. And so they’re very involved through every part of the process. And I think that’s one of the special things of Runway is just that mix between like creatives and researchers sitting by, side by side and working together to build the next generation of our models. I think that’s been a really important piece to, how we’ve operated as a company.

**Swyx \[01:16:37\]:** Yeah. In some senses, though, you can only do this in New York.

**Vibhu \[01:16:40\]:** It’s

**Swyx \[01:16:40\]:** Maybe, you have other offices, but like, I try to find some poetic, significance in the fact that you are a big New York company.

**Anastasis \[01:16:49\]:** As there’s a few parts to being New York. there is that intersection of all those different industries and, like, media, advertising, like

**Swyx \[01:16:57\]:** Yeah, this is very advertising.

**Anastasis \[01:16:59\]:** The, like the art scene is New York. Not to say anything bad about San Francisco, but, it’s. There is more going on. There is that component, and there’s also, I think we benefit from being outsiders and thinking of things a bit differently, like not being in the same, like, hive mind of,

**Swyx \[01:17:19\]:** BВС

**Anastasis \[01:17:19\]:** ASI, of Bay Area and, like, taking. and also taking our time to get where we are today. Like, building the, growing the team intentionally and bringing people who are, yeah, both on the creative side and also on the engineering research side. There’s huge talent pool of amazing people in New York, so that hasn’t really been a problem.

**Swyx \[01:17:41\]:** Congrats on everything. what are you hiring for? what should people look forward to, for the future of Runway?

**Anastasis \[01:17:49\]:** We’re hiring across the board. I think this is probably the most open roles we’ve ever had in the history of Runway. we’re growing our research team quite significantly. So if you’re, if you’re excited about video models, if you’re excited about world models, if you’re excited especially about robotics, the robotics team, we’re hiring roles in the robotics across, software, hardware, and research. so definitely reach out.

**Swyx \[01:18:13\]:** And, a lot of people don’t have direct robotics background, but what should they have, if they want to be useful in robotics?

**Anastasis \[01:18:21\]:** So ideally, some experience with learned policies, would be

**Swyx \[01:18:26\]:** Just RLs

**Anastasis \[01:18:27\]:** Good for robotics. but we tend to hire generalists as a philosophy and, like, people who learn really quickly. but some experience in the, in domain expertise in robotics is something that we’re, we’re definitely looking for the next months. and then we’re scaling the go-to-market team significantly. There is, a wide, like, very active enterprise adoption happening around video models at the moment, and, we’re really trying to, respond to all the demand.

**Swyx \[01:19:00\]:** Yeah. Great. You wanna talk about the, open source robotics stuff?

**Vibhu \[01:19:04\]:** Sure. It was just random notes we had.

**Vibhu \[01:19:07\]:** NVIDIA launched Cosmo. I guess it’s interesting. So, you’re a founding member AI labs to build open source world models in physical AI. - Anything else to talk on here is open research?

**Anastasis \[01:19:20\]:** The biggest thing is that, as I mentioned, while models are still, early, like there is still so much that we you can scale and those models further, so much more advancements and things that we can figure out and how to improve those models further. And I think this is, it’s important that some of this research happens in the open and figuring out what is some incentives for different companies to come together to bring some of that research into the open and open source. And so Cosmos Coalition was a initiative that we co-founded with NVIDIA to bring some of that research as open source. And that could mean open weight model releases. It could mean benchmarks that measure physics and things that people care about when building world models. It could mean infrastructure. So really, how do we grow the ecosystem of world models and make that something that also it’s easier for a developer, a researcher that’s just starting out that is excited about world models to contribute to the field.

**Swyx \[01:20:19\]:** I think it’s a there’s some amount of like, is this also our response against the Chinese world models that are being released, or is there not part of the consideration?

**Anastasis \[01:20:29\]:** I do think it’s, it’s important for NVIDIA models, if you look at the leaderboards of video models, I would say right now the majority of models at the top ten, top twenty are Chinese models. There is, only a handful of companies that are made it to the leaderboard from like the US or the West.

**Swyx \[01:20:50\]:** Yeah. We’re doing better with images, but with video we’re very behind, right?

**Anastasis \[01:20:53\]:** And so I think it’s definitely important that we invest more broadly as a community to make sure that we can those models can we have competitive models

**Swyx \[01:21:02\]:** Yeah

**Anastasis \[01:21:02\]:** Out there.

**Swyx \[01:21:03\]:** But like what’s to stop us from just distilling from them?

**Anastasis \[01:21:06\]:** I don’t know if that’s the best long-term

**Swyx \[01:21:08\]:** Not gonna mention that they won’t

**Anastasis \[01:21:09\]:** That you’re bounded by the performance that you can. It’s, it’s almost a bit of a pessimistic

**Swyx \[01:21:14\]:** Like

**Anastasis \[01:21:14\]:** View that you can get better. you can

**Swyx \[01:21:17\]:** It’s free data. it’s, you might as well. Like if they’re, they’re doing it for like, for the text language side, they might as well do it for the video side the other way.

**Anastasis \[01:21:25\]:** Yeah, I do think we’re, we’re quite capable of training great models

**Swyx \[01:21:29\]:** Okay

**Anastasis \[01:21:30\]:** Without distillation at the moment. Yeah.

**Swyx \[01:21:32\]:** Yeah.

**Vibhu \[01:21:32\]:** So anything you have to say on benchmarks and evals? Like, I feel like what I’m hearing is a lot of people really like arenas for video and image models, customers and whatnot as well. They only want the best on the leaderboard, and they refer to arenas a lot more than language models seem to do. But any notes on benchmarks, what’s lacking? How does the average person compare while these both look really hyper-realistic? More than that, outside of we did talk about like robotic simulation, the physics and all that, but anything to say?

**Anastasis \[01:22:05\]:** I think it’s the opposite in some ways. I think people, generally creatives and artists and marketers, other like people that are using our platforms, I think rely less on, arena scores. And it’s, it’s just so easy to, generate with a bunch of different models and then compare the results visually. Like one nice thing about image and video models is you can immediately tell with your eyes like what feels good from an aesthetic standpoint. Like any artifacts, any issues with the physics of those models, you can immediately tell. and so that’s it’s easier, I would say, to evaluate, as a human. there is also those models than it is in language models where you have those very complex math and coding and, tests where it becomes a lot more harder, I think, for humans to evaluate and can discriminate between the performance of models at a time. So I think in practice, people just test out the same prompt with a bunch of different models and see what the results look like. And right now in Runway, you can use our models and you can use third-party models as well. So it’s, it’s very easy to do that.

**Swyx \[01:23:12\]:** Amazing. We’re gonna end with the AI Runway AI Summit. The last societal issue, I guess, I don’t know if this is a thing, is the, you are at the tension between artists and creatives and AI. A lot of people in that community hate AI. the people that are in the Runway community don’t mind using tools. it’s just another brush. But, how have you seen the sentiment change?

**Anastasis \[01:23:37\]:** Our perspective, yes, it’s just another branch, brush. It’s just another camera. It’s, it’s the latest of a long generation of tools.

**Swyx \[01:23:46\]:** Technology in art.

**Anastasis \[01:23:47\]:** Technology.

**Swyx \[01:23:47\]:** Yeah.

**Anastasis \[01:23:47\]:** And art and technology have evolved together. I think there’s been a pretty significant shift over the past few months, and it came. some of it you can see with a lot of public figures speaking out in favor of AI and being, like in Cannes, you saw a few directors speaking in favor of AI. We had Ron Howard in our film festival. There is, Mark Scorsese also adopting AI models. So you have more of those stories coming out every day of like a well-known figure, speaking in favor of AI. And it’s just a matter of, in my mind, it’s those models are becoming more and more demystified. I would say I have also a bit of a hot take that one of the things that made the initial response to those models maybe a bit more heated than it needed to be was this idea of text to video of, you have a single text description and you get back a -full video.

**Anastasis \[01:24:48\]:** Yeah, there was a misconception. you can generate it to our feature-length film, but the models of today now take a lot of references. They take they are very controllable. And I think when people see a tool that allows, affords many degrees of freedom and control, they respond to it differently. And it matters less that it’s a generative model than the fact that you can steer it to the direction that you want. and so. I think when people look at, complex workflows on top of those models, when they look at, all the ways in which you can steer them and you can provide now with some of the latest models up to fifty references, like the conversation becomes a bit different because it feels much more like a

**Swyx \[01:25:34\]:** Storyboard

**Anastasis \[01:25:35\]:** A tool

**Swyx \[01:25:35\]:** Yeah

**Anastasis \[01:25:35\]:** Versus, like, something that a magical entity that figures out, like, the, your entire film for you.

**Vibhu \[01:25:43\]:** Any notes on, like, workflows changing for people in the field? Like, I think engineering at least has had a lot of people where they’re like expectations have changed. I’m, ten X, a hundred X more productive, and you can get a lot more done. same thing as, you’re making dev tools for creatives. any notes there? Like, there’s some people that don’t wanna adopt, some that do. Like, anything?

**Anastasis \[01:26:08\]:** Yeah. So I think, in terms of, like, what people care about, I see that we have gone through a few stages. So we started from a stage where the main thing that people were looking for was quality. Like, as, we scale those models, the quality improved dramatically. That’s something that people still care about, but it’s, it’s now in addition to controllability, like being able to steer those models with references, with, different kinds of inputs, with storyboards. And now my sense is increasingly people are gonna care about latency more and more. As those models become better, the ability to iterate very quickly becomes more important. And, like, if you can, with a single prompt generate ten different, outputs, like, almost instantly, you can explore way faster than before. And you get some of the magic that characterized the creative tools of the past, like Photoshop was instant. and we lost some of that with generative models. You’re waiting for two minutes to get back a video, and I think we’re gonna bring, some of that back now with the

**Vibhu \[01:27:08\]:** Real-time

**Anastasis \[01:27:08\]:** Real-time models.

**Vibhu \[01:27:09\]:** Yeah. Exciting. And

**Swyx \[01:27:11\]:** Exciting. the last thing we’ll plug is this one, Runway

**Vibhu \[01:27:14\]:** Summit

**Swyx \[01:27:14\]:** Summit. You’re finally doing this in SF?

**Anastasis \[01:27:18\]:** Yeah. So, we’re very excited about this. So this is, in late September thirtieth, we’re doing a summit on, primarily focused on physically high and real-time video generation. We have panelists from NVIDIA, Physical Intelligence, Botco, DeepMind. Yeah, it’s gonna be, I think, a very interesting series of conversations. We try to make the panels really technical and, elicit actual substantive discussion and hopefully some interesting disagreements and interesting debates on things. And, yeah, the there’s tickets available. Hope people can join.

**Swyx \[01:27:56\]:** Since you mentioned it, what disagreements and debates should people think about, or do you expect?

**Anastasis \[01:28:04\]:** So it’s things like, there is, one debate right now in the robotics world is, VLA’s versus world action models.

**Swyx \[01:28:11\]:** Okay.

**Anastasis \[01:28:11\]:** So there is labs that are really betting on one of those two directions. there is like what is the best source of data to train robotics models?

**Swyx \[01:28:21\]:** There’s just the third-party, first-party that we talked about.

**Anastasis \[01:28:24\]:** Yeah. There is, the people who really believe in further scaling teleop data versus leveraging more large-scale video data. So that, those are some of the. And then there is, the world models debates of predict pixels directly versus something like JEPA versus a more 3D-based, 3D-based approach. so I think we’re at a nice time in world models because there is still that active debate happening on, like, what is the best long-term direction. I feel very strongly that it’s video predict pixels directly and scaling video generation models is the right approach. But it’s, I think there is a lot of interesting, debate happening, by researchers on, like, what is the best path to take.

**Swyx \[01:29:09\]:** It’s interesting that it’s all on, like, let’s call it the policy layer and the data model layer. Is the physical side is completely solved? Like, all the sensors, all the actuators, all these things are. We have everything that we need?

**Anastasis \[01:29:23\]:** I don’t think that’s, solved either.

**Anastasis \[01:29:25\]:** It’s definitely,

**Vibhu \[01:29:27\]:** Different problems.

**Swyx \[01:29:28\]:** It’s, it’s like

**Anastasis \[01:29:29\]:** Yeah

**Swyx \[01:29:29\]:** I wanna dream about all these things, and then I get, I buy a robot or I buy, I try to assemble my own, and I can’t even get the motors to, like, work right. Right? Like, and it’s you’re dealing with very sensitive, equipment that has, voltage and power and, like, heat and all these things which, you, abstracted away. We’re sitting here, we’re talking about software and talking about models, but, like, really you have to deal with those kinds of things too.

**Anastasis \[01:29:56\]:** Yeah. And, I think I’m, I’m, I’m generally also not opposed to incorporating other modalities into our models like we’ve seen.

**Swyx \[01:30:04\]:** Yes.

**Anastasis \[01:30:05\]:** The simplest case is they can generate video and audio at the same time. So they can generate RGB, and they can also generate, they can generate sound and audio. But my. I’ve written about this as like what does the maximalist version of a world model look like is you’re incorporating more and more modalities from the universe And you’re training a model on different scales of observations as well.

**Swyx \[01:30:28\]:** X-rays.

**Anastasis \[01:30:29\]:** And so, yeah,

**Vibhu \[01:30:30\]:** You got a good essay that people should read on

**Anastasis \[01:30:33\]:** Yeah. Yeah

**Vibhu \[01:30:33\]:** Real-world.

**Swyx \[01:30:33\]:** No, Meta released a model that was, like, six modalities in one, right?

**Vibhu \[01:30:37\]:** Yeah.

**Swyx \[01:30:37\]:** I forget what the name of the thing was, but it was like, yeah, okay, depth is one of them, but depth is like a transformation of RGB in some sense.

**Vibhu \[01:30:45\]:** ImageBind.

**Swyx \[01:30:45\]:** ImageBind, yeah.

**Vibhu \[01:30:45\]:** Yeah.

**Swyx \[01:30:46\]:** What other modalities? They had heat?

**Vibhu \[01:30:47\]:** Audio, depth, heat, text,

**Swyx \[01:30:51\]:** Whatever IMU is.

**Swyx \[01:30:52\]:** I do think, like, you might as well do ultraviolet. You might as well do, like, just whatever other modality you feel like, ‘cause it’s all data to the model.

**Anastasis \[01:31:01\]:** Yeah. And, a big bet is also that there is transfer between all those modalities.

**Swyx \[01:31:05\]:** Yeah. Yeah.

**Anastasis \[01:31:05\]:** So one of my favorite, examples, which is quite old at this point, is there was this fine-tune of, Stable Diffusion that was called Riffusion Which was

**Swyx \[01:31:14\]:** The music one. Yeah.

**Anastasis \[01:31:15\]:** Yeah, just fine-tuning, Stable Diffusion on spectrograms.

**Swyx \[01:31:18\]:** Spectrograms.

**Anastasis \[01:31:18\]:** And it became a quite capable music generator. Right? So there is probably Spatial patterns, so like spatial-temporal patterns if we’re talking about video that emerge at different scales and different modalities. And so there is some degree of, meta-learning that the model has done that allows it to learn faster if you start from a just a model trained on images and train it to predict audio than if you train from scratch on just audio. and there is some other interesting examples. So there is this project called The Well. It’s, it’s a dataset of physics and numerical simulations in physics and biology and a bunch of other domains. So it’s, so it’s essentially different physical systems across very different scales of space and time, from like astrophysics to low-level like atomistic interactions. And we’ve seen. we’ve done some work on this, and we’ve seen that we can take our video model where, real-world video looks nothing like this, and you can fine-tune it on those numerical simulations and just treat them as RGB frames. And you get reasonable performance much quicker than if you just train from scratch.

**Anastasis \[01:32:36\]:** Yeah.

**Vibhu \[01:32:36\]:** I think we’ve seen this across languages where

**Swyx \[01:32:38\]:** Yeah, DeepSeek-OCR as well.

**Vibhu \[01:32:40\]:** Yeah, DeepSeek-OCR.

**Swyx \[01:32:41\]:** Like, you don’t have to tokenize text. Like, you can just throw them in as images.

**Vibhu \[01:32:44\]:** There’s a lot that happens in that base pre-training. Like, there was an argument a long time ago of people saying, “Oh, humans have so many, sensory representations, right? Smell, touch.” Models have a whole two more modalities that we’ll like, that we don’t even have data for. And it’s like, okay, you take AQI sensor, like you can try this stuff, but there’s so much happening in just the base trainer on that you don’t get as much from these little things.

**Anastasis \[01:33:10\]:** Yeah, exactly. And I think that’s what it solves is data scarcity.

**Vibhu \[01:33:13\]:** Yeah.

**Anastasis \[01:33:13\]:** So you don’t have as much. You have so much video data available, but you don’t have, like olfactory data that

**Vibhu \[01:33:21\]:** The cool thing is it goes the other way too, right? So if you wanna do physics, like if you wanna measure this or you wanna have a diffusion model do audio, it transfers really well. So like in your case, the little bit of post-training for robotics gets a video model to use its fundamentals in another domain. So we can apply that to other stuff too.

**Anastasis \[01:33:40\]:** Yeah. And if we look at, like how do you make those models more useful for in scientific domains, and if you look at AlphaFold, they’ve had all these very. Because of the data, the limited amount of data that it needs to be trained on, it’s it’s very fine-tuned architecture just to solve, protein structure prediction. But if you take all those disparate sources of scientific data and you bring them together under a single model, like I think that’s an approach that can help us solve new kinds of problems across science by leveraging all the learnings from one modality or one set of, data to another. So very early days for that direction, but I do think that’s where ultimately what the end game of simulating the world is. You’re not just using RGB. You’re using RGB as a starting point, but you can incorporate more and more modalities of the universe and leverage the transfer that happens from learning from one to the other.

**Vibhu \[01:34:42\]:** I guess the follow-up there is what’s the drawback of omni? Like, why is everything not an omni model? Also, why not now, and why. Would you start from language backbone or image video backbone and then go omni from there? Does it matter?

**Anastasis \[01:34:57\]:** Yeah. We need to take it one step. We need to solve robotics first, and then we can go into

**Swyx \[01:35:02\]:** Solve everything now.

**Anastasis \[01:35:05\]:** Yeah. I do think there is a lot of open-ended research that needs to happen for, those omni models. There is a lot of things that require careful consideration when you’re bringing multiple modalities into a single model to predict. But I think, I expect those to be solvable.

**Swyx \[01:35:23\]:** Wonderful. you’ve been very generous with your time. Congrats on all your success, and, yeah, I’m excited for the, AI Summit, or physical AI Summit.

**Anastasis \[01:35:32\]:** Yeah, thanks for having me.

**Swyx \[01:35:33\]:** And yeah, and people should check out the film festival if it’s in town, right?

**Anastasis \[01:35:37\]:** Yeah.

**Swyx \[01:35:37\]:** Yeah. You’ll be gonna be touring all over the place.

**Anastasis \[01:35:39\]:** Yeah. Next year we’re probably gonna do that. So we do film festivals every May or June of

**Swyx \[01:35:45\]:** Yeah.

**Anastasis \[01:35:45\]:** And we did the last one in New York, LA, Tokyo, and at the AI Engineer,

**Swyx \[01:35:52\]:** Yeah

**Anastasis \[01:35:53\]:** Fair.

**Swyx \[01:35:53\]:** Yeah. Yeah.

**Anastasis \[01:35:54\]:** So yeah, hopefully even more places next year.

**Swyx \[01:35:57\]:** No, I think like someday, you will be hosting the Oscars of AI video, and, I think people should like take this very seriously as like a potential career they can have.

**Anastasis \[01:36:07\]:** The Oscars of AI video will be called the Oscars.

**Swyx \[01:36:10\]:** All right. All right. Thank you.

**Anastasis \[01:36:14\]:** Thank you.
