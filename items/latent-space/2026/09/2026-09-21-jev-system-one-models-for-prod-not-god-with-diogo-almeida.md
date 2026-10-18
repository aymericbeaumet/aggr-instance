---
title: 'Jev: System One models for Prod, not God — with Diogo Almeida, CEO, TypeSafe AI'
link: https://www.latent.space/p/jev
source: latent-space
published: 2026-09-21T22:13:49Z
updated: 2026-09-21T22:13:49Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: The definitive Jev podcast with its lead creator.
content: extracted
html: 2026-09-21-jev-system-one-models-for-prod-not-god-with-diogo-almeida.html
preview:
  file: 2026-09-21-jev-system-one-models-for-prod-not-god-with-diogo-almeida.preview-b1bfe55814e7.webp
  width: 256
  height: 128
  color: '#645471'
images:
- source: https://substackcdn.com/image/fetch/$s_!4FCs!,w_1200,h_600,c_fill,f_jpg,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-video.s3.amazonaws.com%2Fvideo_upload%2Fpost%2F216783460%2F7e63c899-1c78-4714-a7ae-bbf4a57a9454%2Ftranscoded-1790031168.png
  original:
    file: 2026-09-21-jev-system-one-models-for-prod-not-god-with-diogo-almeida.image-fe2323dff4ae.jpg
    width: 1200
    height: 600
  color: '#010517'
- source: https://substackcdn.com/image/fetch/$s_!MPbn!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5ad289d5-2a03-4b50-804b-ae4413cbe521_600x644.png
  original:
    file: 2026-09-21-jev-system-one-models-for-prod-not-god-with-diogo-almeida.image-36b7b763d51f.jpg
    width: 600
    height: 644
  color: '#fefefe'
- source: https://i.ytimg.com/vi/cFx9Z3ZXca0/hqdefault.jpg
  original:
    file: 2026-09-21-jev-system-one-models-for-prod-not-god-with-diogo-almeida.image-aaeaa442d709.jpg
    width: 480
    height: 360
  color: '#010002'
- source: https://i.ytimg.com/vi/cJ0EOzey--o/hqdefault.jpg
  original:
    file: 2026-09-21-jev-system-one-models-for-prod-not-god-with-diogo-almeida.image-32afdeb47a17.jpg
    width: 480
    height: 360
  color: '#020202'
extra:
  audio_type: audio/mpeg
  audio_url: https://api.substack.com/feed/podcast/216783460/cdf8e02436aa264178bb2aa2c85db57c.mp3
  duration_seconds: 8453
---

*Tickets for [AIE NYC](https://ai.engineer/nyc/2026) now open, and [apply](https://ai.engineer/code/2026/apply) for the invite-only [AIE CODE](https://ai.engineer/code/2026). [Join us](https://x.com/aiDotEngineer/status/2078502554200359344)!*

We have an unusual relationship with today’s guest: for years since coauthoring [the InstructGPT paper](https://arxiv.org/abs/2203.02155), [Diogo Almeida](https://www.youtube.com/watch?v=cJ0EOzey--o) had been saying that API-available frontier models have been going down the wrong path, everything from the **alignment** to **refusals** to **reliability** perspectives, that we have [dropped every mode](https://docs.typesafe.ai/introduction/machine-learning-primer#the-problems-with-rlhf) other than autoregressive chat-tuned LLMs because of the overwhelming success of ChatGPT.

[www.youtube.com](https://www.youtube.com/watch?v=cFx9Z3ZXca0)

In a launch video now viewed **\~40M times** (by comparison, [GPT4o was 22M](https://x.com/OpenAI/status/1790072174117613963?s=20), [Fable 5 was 15M](https://x.com/AnthropicAI/status/2072163884430229756?s=20), [Navier Stokes was 74M](https://www.latent.space/p/ainews-here-are-6-clones-of-jev-in), and [6 Astra was 137M](https://x.com/OpenAI/status/2095595741528125780?s=20)), Diogo introduced **Jev** and it immediately took over the AI timeline — we’ll skip full Jev explainers because your favorite AI influencer/educator has probably already done one. We also collected:

- the official [patterns](https://docs.typesafe.ai/patterns) and [cookbooks](https://docs.typesafe.ai/cookbooks/) you should see first, from [Allie](https://x.com/allietheicon)

- Jev usecases

  - speed based - games and computer use

    - [the voice + computer use example](https://x.com/instantricecook/status/2100814590300889426) we discuss at 1h34 mins

      - > voice + browser control
        >
        > [@moritzkremb on X](https://x.com/moritzkremb/status/2100577979021832365)
    - The must not miss [Doom demo](https://framerusercontent.com/assets/rlL7ImEbISFoYt3IJEHHfvjpY.mp4)

    - [Driving cars](https://x.com/jpschroeder/status/2100347770867458384?s=20) in games

    - > Excalidraw
      >
      > [@jackcheng on X](https://x.com/jackcheng/status/2100729670991802386)

    - > virtual try-ons
      >
      > [@nailthy62 on X](https://x.com/nailthy62/status/2101388186916454439)

    - “Smart Games”/smart NPCs
  - > guided responses in text messages
    >
    > [@yuhasbeentaken on X](https://x.com/yuhasbeentaken/status/2101698498567831740)

  - [Jev for coding agents](https://docs.typesafe.ai/introduction/coding-agents) has an official guide

    - [jev for linting](https://x.com/ohansemmanuel/status/2101034822760288452?s=12)

    - [compacting tool calls](https://x.com/tamarajtran/status/2100694549362553153?s=12)

      - [reasonable pushback from Theo](https://x.com/theo/status/2100762304862384257) - Diogo has published a note on the [Tyranny of the KV Cache](https://docs.google.com/document/d/1G61uUB0FifUnmmrPzFQojZ3KpczYKmXGpgEXDJ2l_Zg/edit?tab=t.0) that you should read as a followup after the pod for Jev + coding agents, because of his belief that [Cache Rules Everything](https://x.com/CompleteSkeptic/status/2097738214589215173)
  - [Programming Languages built atop Jev](https://x.com/southpolesteve/status/2100767781868150938?s=12) (Diogo’s fave)

  - [Jev for analytics](https://x.com/tarasshyn/status/2101012033340571952) replay and [user journey review](https://x.com/regalstreak/status/2101189571375493239?s=12)

  - “dark data”

    - [entity resolution](https://x.com/hrishioa/status/2101362082369470675?s=12)

    - [natural language search](https://x.com/venturetwins/status/2101341075684434245?s=12)
  - “ [smart software](https://youtu.be/cJ0EOzey--o?si=nlFo1Y2XW5SW9vjs&t=697) ”

    - a core goal of Jev is to “disappear into the background” - eg as unremarkable as regex
  - [Jev as a judge](https://x.com/langchain/status/2101454284927959080?s=12)
- Jev memes

  - [Jev vs LLM capabiltiies](https://x.com/markjaquith/status/2101341256743813558)

  - [blending transformers and classifiers](https://x.com/nazo_btw/status/2100955791750476048?s=20)

  - [about the confidence api](https://x.com/JinjingLiang/status/2101547529532059736?s=20)

  - [Jev vs GLiNER](https://x.com/george_onx/status/2100293114808119379?s=12) (note [difference/pushback](https://x.com/mkhordoo/status/2101125110681784676?s=12), [agreed](https://x.com/irl_danb/status/2100935837470843075?s=12), [agreed](https://x.com/joelgrus/status/2101437270142099965?s=12), [agreed](https://x.com/mparakhin/status/2101683565520199887?s=12))

  - [Jev on trolley problem](https://x.com/The_Alex/status/2100619644973486252?s=20)

  - [Jev Bush](https://x.com/zeddotdev/status/2100390620640526554?s=20)

Instead we’ll focus on what we can uniquely offer — a broader philosophical and mission-based understanding of **how and why Jev was created**, and what you should expect next in terms of **future models from TypeSafe** ( [ReasoningJev](https://x.com/lateinteraction/status/2101380477495996699?s=12)?) and **what usecases and ideas you should work on** vs the [55th low effort clone of Jev’s API](https://www.latent.space/p/ainews-here-are-6-clones-of-jev-in) or doing a generic [JevBench](https://x.com/airesearch12/status/2101311992984199580?s=12) benchmark - something [Diogo has rejected publicly](https://x.com/CompleteSkeptic/status/2098463042065572038).

[www.youtube.com](https://www.youtube.com/watch?v=cFx9Z3ZXca0)

Diogo knows a good deal about RLHF, given that he was on the team that pioneered post-training at OpenAI — and traces the three branches to [Christiano et al 2017](https://arxiv.org/abs/1706.03741) (the robot backflip demo), [Stiennon et al 2020](https://arxiv.org/abs/2009.01325) (learning to summarize) and his baby, [Ouyang et al 2022](https://arxiv.org/abs/2203.02155) (InstructGPT). From there on, every innovation from [Function Calling](https://www.latent.space/p/devday-2024?utm_source=publication-search) to [Structured Outputs](https://www.latent.space/p/openai-api-and-o1?utm_source=publication-search) to [Reasoning](https://www.latent.space/p/karina?utm_source=publication-search) felt like a hack on top of the string based, sequence to sequence prediction paradigm. As he mentions on the pod, from 2023-2024 he struggled unsuccessfully, due to both personal and organization underestimation, to train a model that accurately addressed what he saw as the core problem with making LLMs the heart of software: **reliability**.

Jev’s core innovation is "**Reinforcement Learning for Calibrated Decisions** ”, a novel, unpublished technique that optimizes for “answers with epistemically honest probabilities on System One tasks” rather than [human rated feedback (RLHF)](https://www.latent.space/p/rlhf-201?utm_source=publication-search) — which causes hallucinations, sycophancy, and permanent reliance on humans — or [programmatically verifiable outputs with rubrics (RLVR)](https://www.youtube.com/@LatentSpaceTV/search?query=rl) — which solves Navier Stokes but [exacerbates jagged intelligence](https://x.com/karpathy/status/1816531576228053133?lang=en) and doesn’t integrate well with other software.

We’ve talked about [the calibration problem](https://www.latent.space/p/benchmarks-201?utm_source=publication-search) before on the pod, but probably the single best place to understand why RLCD became necessary is [Diogo’s AIE talk](https://www.youtube.com/watch?v=cJ0EOzey--o), which discusses why a generation of training helpful AI assistants for humans has impaired them for training models for [composable, programmable AI for automation](https://typesafe.ai/manifesto).

[www.youtube.com](https://www.youtube.com/watch?v=cJ0EOzey--o)

At the end he also teases [his contrarian opinion on scaling laws](https://x.com/CompleteSkeptic/status/2073442518117884197) - which teases how to build a modern neolab without the billions of dollars the major labs have…

We spend a good amount of time discussing Diogo’s essay on [the Bitterest Lesson](https://x.com/CompleteSkeptic/status/2098097767512179135):

[![](https://substackcdn.com/image/fetch/$s_!MPbn!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5ad289d5-2a03-4b50-804b-ae4413cbe521_600x644.png)](https://substackcdn.com/image/fetch/$s_!MPbn!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5ad289d5-2a03-4b50-804b-ae4413cbe521_600x644.png)

His point is that “You get what you optimize for and the bitterest lesson in ML is that the most important part of it isn’t ML at all.” - and picking the right north star, eg upvoting for user preference vs being integrated into tool calls - makes everything else fall in line.

We’re excited to catch up with a [freshly dyed](https://x.com/typesafeai/status/2101451220896682107) Diogo to discuss:

- Why AI can solve **extraordinarily hard problems** but still fail to automate basic work

- What **System One Models** are and why Jev is built for software rather than chat

- RLHF, **mode collapse**, calibration, and the hidden costs of optimizing for human preferences

- Why **refusals become a problem** when AI is buried inside software dependencies

- Why TypeSafe rejects public benchmarks and optimizes for **intelligence per dollar**

- The “bitterest lesson”: why the **right task and the right data** can matter more than compute

- Why TypeSafe thinks of itself as a **data lab** rather than a model lab

- **RLCD vs. RLHF and RLVR** as fundamentally different North Stars for AI

- Why **reliability and robustness** matter more than simple determinism

- Jev’s programming primitives and how intelligence maps into **software control flow**

- Why developers should decompose AI workflows into **small, measurable decisions**

- How **structured state** replaces giant prompts and system messages

- Why Diogo thinks AI should eventually **disappear into the background** of software

- The **“inverse SaaS-pocalypse”** and how AI could supercharge existing software

- **System One vs. System Two** intelligence and the limits of reasoning models

- Dark data, computer use, real-time intelligence, and **Jev’s biggest early use cases**

- Why Jev could reshape **coding agents** built around a single-model architecture

- Why Diogo says he **wouldn’t pre-train with $1 billion**

- The OpenAI journey that led to TypeSafe and why he thinks many **neo-labs are approaching AI incorrectly**

- Coding agents beyond the **KV cache**, shared state, sub-agents, and the multi-agent future

- **LinkedIn:** [https://www.linkedin.com/in/diogomda](https://www.linkedin.com/in/diogomda)

- **X:** [https://x.com/CompleteSkeptic](https://x.com/CompleteSkeptic)

- **TypeSafe AI:** [https://typesafe.ai/](https://typesafe.ai/)

**00:00:00** Jev Launch Week and the AI Economic Revolution

**00:02:50** What Is Jev? System One Models and Programmable AI

**00:05:54** RLHF, Mode Collapse, Calibration, and Yann LeCun

**00:10:29** Programmatic AI, Refusals, and Safety Alignment

**00:17:21** Why TypeSafe Rejects Public Benchmarks

**00:20:43** The Bitterest Lesson: Data, Compute, and the Right Task

**00:24:59** RLCD vs. RLHF and RLVR

**00:28:42** Why Powerful AI Still Hasn’t Automated the Economy

**00:39:55** Reliability, Robustness, and Determinism

**00:48:11** Model Versioning, LTS, Speed, and Intelligence per Dollar

**00:54:04** Inside Jev’s API and Programming Primitives

**00:58:28** How to Build with Jev: Structure, Decomposition, and Small Decisions

**01:18:28** The Inverse SaaS-pocalypse and AI Disappearing into Software

**01:33:21** Computer Use, Dark Data, and Jev’s Biggest Use Cases

**01:38:48** How Jev Could Reshape Coding Agents

**01:41:00** AI Safety, Frontier Pacing, and the Limits of RLVR

**01:48:03** Why Diogo Wouldn’t Pre-Train with $1 Billion

**01:55:19** The OpenAI Story Behind TypeSafe

**02:01:41** Why Diogo Thinks Most Neo-Labs Are Getting AI Wrong

**02:08:00** Coding Agents Beyond the KV Cache and the Multi-Agent Future

**Swyx \[00:00:00\]:** Okay, we’re in the studio. A special occasion because this week, Diogo, my good buddy, launched Jev, and it’s been taking over the complete timeline. How do you feel? What’s it like to be you right now?

**Diogo Almeida \[00:00:16\]:** Emotionally?

**Swyx \[00:00:17\]:** Yeah.

**Diogo Almeida \[00:00:17\]:** Never been worse. Like, I’m a ragged corpse of a person right now because there’s so much going on, and I’m like a technical CEO, so I have, like, a lot of fires to fight.

**Swyx \[00:00:29\]:** Yeah.

**Diogo Almeida \[00:00:29\]:** But mentally, I feel—I say this all the time, and I’ve been saying this kind of for years in my over-under events. Like, I feel like the entire AI field is like one of those, like, carnival house of mirrors, and everyone is just insane and saying the weirdest stuff that doesn’t make sense. And it feels like for just this week, like, I’m on a better in sync with reality and like, oh, people see it now. AI can be so much more than what was once thought.

**Diogo Almeida \[00:01:06\]:** And like, yes, we are going to make. Like, an AI-based economic revolution is back on the table, and this is fucking awesome.

**Diogo Almeida \[00:01:17\]:** I’m so jazzed the developers get it. It’s, it’s, Yeah, and I want to show my eternal gratitude to the developers and

**Swyx \[00:01:25\]:** Yeah.

**Diogo Almeida \[00:01:26\]:** I’m so jazzed about the community and everything. It’s so great.

**Swyx \[00:01:28\]:** Yeah, you were saying yesterday that you decided to prioritize the town hall and not a bunch of, like, VIP, investor-type people because you wanted to make sure that they are the people that you get your most, attention, right? The engineers, the developers.

**Diogo Almeida \[00:01:43\]:** Yeah, it felt a little like, oh man, I’m talking to, like, really important people right now.

**Swyx \[00:01:47\]:** Yeah.

**Diogo Almeida \[00:01:47\]:** I probably shouldn’t reveal who.

**Swyx \[00:01:48\]:** Yeah.

**Diogo Almeida \[00:01:48\]:** But it feels a little bit dirty for me to, I’m, like, perhaps overly genuine in things. Like, it feels, like, dirty if, like, in my gigantic calendar event of people to talk to, the community isn’t one of those.

**Swyx \[00:02:04\]:** Yeah.

**Diogo Almeida \[00:02:04\]:** And actually, in my ideal world, it would be, like, community all the time. I was thinking, “Should I host a town hall while walking to your studio?” And I’m like, “No, that’s too crazy.”

**Swyx \[00:02:12\]:** Sure. Yeah. Well, you guys have been hosting town halls on Discord. Discord is now 100,000 people. Your Twitter’s

**Diogo Almeida \[00:02:19\]:** I don’t follow these stats.

**Swyx \[00:02:20\]:** Yeah.

**Diogo Almeida \[00:02:20\]:** So holy shit.

**Swyx \[00:02:21\]:** Your Twitter’s blown up. It was, it was really funny ‘cause, like, at AIE, you were like, “Yeah, follow me please,” and then you didn’t, like, provide even your handle.

**Diogo Almeida \[00:02:29\]:** I’m a noob. I’m a noob.

**Swyx \[00:02:29\]:** You’re such a noob.

**Diogo Almeida \[00:02:30\]:** I’m a noob.

**Swyx \[00:02:31\]:** But no, but that, like, that’s, like, positive aura that, like

**Diogo Almeida \[00:02:33\]:** Cool

**Swyx \[00:02:33\]:** You don’t know how to promote yourself.

**Diogo Almeida \[00:02:35\]:** Yeah. Someone, like, called me out when I posted, like, “Holy shit, we’re all three twending-- trending topics.” And then they’re like, “That’s a personal feed.”

**Swyx \[00:02:42\]:** That’s a personal, yeah.

**Diogo Almeida \[00:02:43\]:** And I’m like, “Oh, no.”

**Swyx \[00:02:44\]:** Of course, of course it’ll trend to you.

**Diogo Almeida \[00:02:45\]:** Cringe. Yeah.

**Swyx \[00:02:45\]:** Yes, ‘cause it’s what you clicked on.

**Diogo Almeida \[00:02:47\]:** Yeah.

**Swyx \[00:02:47\]:** So okay. Let’s, Yeah, so congrats on everything.

**Diogo Almeida \[00:02:50\]:** Thank you.

**Swyx \[00:02:50\]:** We’ll talk about more, details as you have them. But let’s, for people who are, like, living under a rock or just want, like, the definitive thing, what is Jev?

**Diogo Almeida \[00:03:02\]:** Whew. Let me think about. That’s a hard one.

**Swyx \[00:03:07\]:** Okay. And I’m happy to, like, re-ask if you wanna kind of

**Diogo Almeida \[00:03:09\]:** No. I’m happy to

**Swyx \[00:03:10\]:** Okay

**Diogo Almeida \[00:03:10\]:** I’m happy to, like, just jam on it.

**Swyx \[00:03:12\]:** Yeah.

**Diogo Almeida \[00:03:13\]:** I will say, like, the first thing that I’m relieved about with this question is now I don’t have to answer that question to my parents anymore ‘cause ChatGPT can just explain it.

**Swyx \[00:03:20\]:** Nice.

**Diogo Almeida \[00:03:21\]:** So the way I see it is we new-- need a new class of models. We’re not attached to naming that class of models. Our-- the most accurate name we’ve come up with is System 1 models.

**Swyx \[00:03:33\]:** Yeah.

**Diogo Almeida \[00:03:33\]:** There will be reasons, but it’s-- there’s a reason why we don’t call them decision models, because, like, they will be. Like, System 1 is beyond that. That’s all I can say. We didn’t expect this to be our big launch, so we have stuff in the tank.

**Swyx \[00:03:48\]:** You should have said low-key research preview.

**Diogo Almeida \[00:03:52\]:** It kind of was, right? It kind of was. But we. So there’s a class of models that we describe them as, like, machine-native, System 1, large programmable. I think these are-- is the class of models where the goal is for code to be the consumer. So as opposed to, lar-- pre-trained large language models, which are meant for, like, autocomplete of the internet, or RLHF models, like chatbot instruction-following models, which are meant to, like, reply to text, or RLVR. It’s in a weird gray area with RLHF. Like, these are meant to have things that directly are consumed by code, hence the name type safe. So the thing we really want is to have, like, AI, like, be as powerful as possible, and we think the way to do that is to integrate it with software. And we are designing everything, beyond just the outside, the deep internals of the model to be optimized for software. So number one, Jev is our first large programmable model, or a System 1 model, whatever you want to call it. Jev is meant to be optimized for intelligence per dollar, hence the name Jev.

**Swyx \[00:05:03\]:** Jevons Paradox.

**Diogo Almeida \[00:05:03\]:** Jevons Paradox, yeah. And it’s optimized for intelligence per dollar. I love this debate with people about what is the most important between reliability, cost, calibration, and speed. And Jev is meant to be. Jev will be the name of models that will be on the frontier of intelligence per dollar. There’s other ways to optimize it, like, ML, or at least if you’re good at ML, it’s all about trade-offs. And we are just going all out on that.

**Swyx \[00:05:31\]:** Yeah. And to me, like, calibration is one of the new things that people weren’t talking about as much. We’ve done an episode In the past, with Clementine Foreia of Hugging Face, where they were like, “Yeah, actually, y- they’re just.” Or, and this is your whole argument about RLHF, is they’re more collapsing towards what you want to hear the most

**Diogo Almeida \[00:05:50\]:** Ooh

**Swyx \[00:05:50\]:** Or what is most likely, instead of, like, their own internal confidence about a thing.

**Diogo Almeida \[00:05:54\]:** Can I soapbox on that for a second?

**Swyx \[00:05:56\]:** Go ahead. Yeah.

**Diogo Almeida \[00:05:57\]:** Cool. Like, I’ve been heard that your audience is the most technical, so I actually want to get into that.

**Swyx \[00:06:02\]:** Yeah.

**Diogo Almeida \[00:06:03\]:** And if- I went through extreme precision to make sure everything in our launch video is accurate and real. Apparently, that’s very unusual. One of the things that no one paid attention to was the downsides of RLHF, in particular mode dropping.

**Swyx \[00:06:17\]:** Mode dropping or mode collapse?

**Diogo Almeida \[00:06:19\]:** It’s the same thing.

**Swyx \[00:06:19\]:** Is that what you call it?

**Diogo Almeida \[00:06:20\]:** It’s the same thing.

**Swyx \[00:06:20\]:** All right.

**Diogo Almeida \[00:06:21\]:** And I wanna have a blog on this eventually, but I, like, want to tell as many people this as possible ‘cause I think it’s a very interesting thing. So the spicy take, I believe in Yann LeCun a lot. I think Yann LeCun’s takes are actually among the closest to

**Swyx \[00:06:36\]:** What about this?

**Diogo Almeida \[00:06:37\]:** Well, should I address this now or should I wait and go into mode collapse?

**Swyx \[00:06:40\]:** No, later. Go mode, go mode collapse. I don’t know.

**Diogo Almeida \[00:06:42\]:** So I actually think that among takes, Yann LeCun’s is among the most accurate. But he has this very famous/infamous slide about,

**Swyx \[00:06:52\]:** The cake?

**Diogo Almeida \[00:06:53\]:** LLMs are doomed.

**Swyx \[00:06:54\]:** Okay.

**Diogo Almeida \[00:06:54\]:** Like that one where he, like, has, like, a pie chart with, like, a tiny par-- tiny little thing- and says that as you increase sequence length, the probability of it making an error goes in. Yes, this one. This one. I love this one, because it’s one of these things that seems mathematically obvious, but is obviously wrong, right? Like, it’s mathematically obvious, but it doesn’t empirically hold. And this is my favorite thing to teach people about, like, where you

**Swyx \[00:07:21\]:** What’s the disconnect, right?

**Diogo Almeida \[00:07:22\]:** Exactly. And may I or you want to tell me?

**Swyx \[00:07:27\]:** About mode collapse?

**Diogo Almeida \[00:07:28\]:** Oh, no. Oh, so mode clop-- collapse is related to this.

**Swyx \[00:07:31\]:** Yeah.

**Diogo Almeida \[00:07:31\]:** The disconnect happens because if you are in a mode covering or a calibrated distribution, you are, like, not. You are not overly punished about having outliers. You’d expect, like, something. Some amount of the time you’d be out of distribution, some amount of time you’d be in distribution. That’s what happens when you cover the distribution. This was like models before GANs. They made blurry images, right?

**Diogo Almeida \[00:07:54\]:** Instead, GANs mode drop. They, like, drop the minority classes and just do the really common ones. And this is why this effect doesn’t happen, right? Like, instead of be-- in order to generate really long strings, without making errors, they need to, like, be extremely conservative because it’s e- really easy to see when an error happens. It’s very hard to see when, like, a subtle thing that looks correct happens. And that calibration is, like, total poison into, like, the probability distributions of strings.

**Swyx \[00:08:22\]:** Yeah.

**Diogo Almeida \[00:08:23\]:** And it’s, it’s a nuanced take and like, I think that This is why this doesn’t happen, and this is why strings are so bad at, decision-making or, overloading the string models are for decision-making is, like, a bad time.

**Swyx \[00:08:38\]:** And while we’re on the topic of Yann, do you agree that his fix i- with-- which is like a world model, like a JEPA-type, embedding thing is the right solve? So basically, like, the. One of the reasons that it could fail is because you’re trying to reason over token outputs and then, and then just looping back again and going. Keep, continuing going until you reach, like, a end of sentence. Like, is that, And his solve is JEPA, right?

**Diogo Almeida \[00:09:02\]:** Yes.

**Swyx \[00:09:02\]:** Which is, like, joint ambition,

**Diogo Almeida \[00:09:04\]:** Yeah

**Swyx \[00:09:04\]:** Joint embedding prediction. So like, is that the solve or, like, do you have a. Do you have a take on that?

**Diogo Almeida \[00:09:10\]:** Oh, man. I probably shouldn’t talk too much about the insides of ML, but I will say that my brand, other than unhinged, is practical.

**Diogo Almeida \[00:09:20\]:** Like, even my take here is practical. And like, I’m. Am I a scaling law fan? Depends. It dep-- it’s, it’s, it’s, like, it’s. Scaling laws tell you how much better you get at a thing for amount in.

**Diogo Almeida \[00:09:33\]:** A scaling law does mean exponentially more resources for normally sublinear gains, which looks to be a bad investment unless those, like, linear gains are, like, really valuable. But it’s all. To me, it’s all about, like, what can we do with what we have to make the biggest possible fucking difference? I can curse.

**Swyx \[00:09:51\]:** Yeah.

**Diogo Almeida \[00:09:51\]:** Yeah.

**Swyx \[00:09:52\]:** Yeah.

**Diogo Almeida \[00:09:52\]:** Yeah.

**Swyx \[00:09:53\]:** We’re, we’re, we’re approved for adults.

**Diogo Almeida \[00:09:54\]:** Hell yeah.

**Swyx \[00:09:55\]:** And also we have a scaling law thing if you wanna go into that later.

**Diogo Almeida \[00:09:58\]:** Oh, I could if we. See, that part is not super relevant right now.

**Swyx \[00:10:02\]:** Yeah.

**Diogo Almeida \[00:10:03\]:** I actually. If you wanna go into my bitterest lesson, I think that’s more relevant.

**Swyx \[00:10:06\]:** Okay.

**Diogo Almeida \[00:10:06\]:** But like, to me, I’m all about, like, pragmatics. And I think that the JEPA stuff is really cool early research. I really love awesome research. Is it practical yet?

**Diogo Almeida \[00:10:21\]:** Probably shouldn’t say. But like, there’s just a lot of.

**Diogo Almeida \[00:10:29\]:** I just think there’s just, like, so many diamonds in the rough let all over the research world right now that haven’t been polished because people don’t know how to, like, do the right task. And I think that what our launch did, it. Does it kickstart us as a company? Like, yes. Will it be great for us as a company? Yes. I think it’s gonna be, like, even greater for this direction of, like, programmatic AI. There was going to be, like, a gold rush on top of us for. ‘cause, like, software is super fucking charged. But I think there’s gonna be a gold rush parallel to us as well on, like, all the different ways we can expose things to make software more powerful so people can make even cooler stuff. And then we are back to, like, early internet energy?

**Swyx \[00:11:12\]:** Yeah.

**Diogo Almeida \[00:11:12\]:** And I think that’s why, like, the Twitter is just like, “Jev.”? It’s, it’s like. It is a party

**Swyx \[00:11:18\]:** It’s inspiring because it’s, it’s, like, so different than what we’re used to, which is, “I’m sorry you can’t do this, but we do scaling laws and only the big labs can do it,” right?

**Diogo Almeida \[00:11:28\]:** That. Actually, if I. I’ll, I’ll make a tangent if that’s okay.

**Swyx \[00:11:32\]:** Yeah.

**Diogo Almeida \[00:11:32\]:** I think you might enjoy this.

**Swyx \[00:11:33\]:** Really? Our five tangents in. It’s good. It’s fun. Yeah.

**Diogo Almeida \[00:11:35\]:** Oh, yeah. I get lost at all my tangents.

**Swyx \[00:11:37\]:** This is gonna be horrible for the listeners to figure it out, but they’re gonna figure it out. It’s fine.

**Diogo Almeida \[00:11:40\]:** Yeah, we can edit it in post.

**Swyx \[00:11:40\]:** This is my response. Yeah.

**Diogo Almeida \[00:11:41\]:** So popular thing on Discord, that people keep asking me, I haven’t had the time to explain it yet, is why am I opposed to safety alignment and why do we not refuse? I’m not opposed to safety as a principle, but I think that safety alignment is generally misaligned with users. And refusal is just, like, obviously a type error. Like, if you’re a human being and you’re chatting with, like, a bot or whatever, you’re cloud coding, and a refusal happens, like, “I’m sorry, I can’t read DNA.py.” that’s an annoying time. It’s anno- it’s, it’s annoying

**Diogo Almeida \[00:12:18\]:** Right? But you can work with it, right? And you’re forced to work with it ‘cause of Stockholm syndrome.

**Diogo Almeida \[00:12:23\]:** I have stories about that too. I need another tangent deep in here. But like, if you ever want this in a dependency running in the background, what happens if that refuses? What if someone else is using that dependency? They don’t know what that system is. Like, you want the software to just stochastically break because a user sent, like, a weird message in there?

**Diogo Almeida \[00:12:42\]:** Like, that is, like, straight-up insanity. It’s coming from a place of, like, people who do not understand software, do not understand programming, and like, they are obsessed with, like, I believe this, horseless carriage of, like, AI coworker instead of unearthing, like, the full power of AI.

**Swyx \[00:13:01\]:** Fair enough.

**Diogo Almeida \[00:13:01\]:** Yeah.

**Swyx \[00:13:01\]:** You want something that is the core kernel that is usable everywhere.

**Diogo Almeida \[00:13:05\]:** Yes. Exactly. Like, the cognitive core, right?

**Swyx \[00:13:07\]:** Yeah.

**Diogo Almeida \[00:13:08\]:** And you need this thing to be s- like, so general, so optimized for its use cases. You want it to be, like, you want it to work on all the future use cases, all the weird shit that people are doing.

**Swyx \[00:13:19\]:** Yeah.

**Diogo Almeida \[00:13:19\]:** We obviously didn’t train on any of that stuff. Is it surprising that it works? No, ‘cause we trained on weirder stuff, my friend.

**Diogo Almeida \[00:13:28\]:** So. But one tangent up about, like, safety alignment.

**Swyx \[00:13:32\]:** Okay.

**Diogo Almeida \[00:13:32\]:** Safety alignment makes sense for a product, in my opinion, for, like, ChatGPT and Claude. Like, it, What safety, what makes safety and capability alignment different is capability alignment is, like, about doing what the user wants. That is sick for software engineers. They want their thing to do the thing, and the more predictable it is, the less they have to test it and play around with it. Jeb is not anywhere close to that yet. It could be, but like, there’s so many more nines of reliability that we want in order to make it so good, like a database query, that you don’t even have to think about it. It is just there when you need intelligence. But safety alignment is, like, the opposite of instruction following. It’s when you want to follow someone else’s instructions, like OpenAI and Anthropic

**Swyx \[00:14:13\]:** The RAGs value stack.

**Diogo Almeida \[00:14:14\]:** Exactly. And this makes a lot of sense for a product. Again, like, ChatGPT should do. Y- you sh- like, if they don’t want to, like, do, like, some, not-safe-for-work role play with ChatGPT, that’s on them because, like, maybe that’s, what their users who have, like, parents and kids want. Like, n- that’s fine. But in an API, that’s nuts, right? Like, that’s completely unacceptable because, like, people need to, like, program around this, and that is, that’s so anti-user that it’s. It. I’m. Huh. I can be an angry person, so I should try to calm down.

**Swyx \[00:14:52\]:** It’s, People get your passion, and I think that’s really good. The one pushback I’ll give you is, like, what if we use it to kill people, right? Like, that is the actual. Like, n- the not-safe-for-work thing, it’s private, personal, whatever. But like, yes, like, we will use it in war. And like, that is, something that companies can reasonably prefer their APIs not be used for.

**Diogo Almeida \[00:15:14\]:** I get that. I think that there’s, like, pragmatic places where that opinion can be held. I don’t think the foundation of, like, a general-purpose technology is that place, personally.

**Diogo Almeida \[00:15:27\]:** Like, would I prefer that our stuff is not used to kill people? Obviously. Would I prefer it’s used for, like, all sorts of, like, great stuff in the world? Obviously. Will I put my thumb in the scale for that? Yes. Will I do it at the technological layer? Absolutely not, because that will fracture the intelligence. Every single time you mean it to overfit to some weird stuff, you’re fracturing its intelligence more and more. And like, these things are fractured to the, like. They’re so darn fractured right now.

**Swyx \[00:15:54\]:** Yeah.

**Diogo Almeida \[00:15:54\]:** So and as a furthermore thing, to me, it’s like I think intelligence will be more like a database than a coworker. Like, I don’t think it’s up to databases to add checks on whether or not they’re used for, like, what’s something that’s not great? Like, CIA. Actually, I don’t know what the CIA does, really. You can imagine. You can imagine, killing people who are not even bad or whatever.

**Diogo Almeida \[00:16:21\]:** And like, I don’t think it’s the database’s responsibility for that. And furthermore, like, a thing that has been weird to me is when people, like, sign up for our thing on Slack and they’re like, “Hey, we’re gonna deploy this. Can we deploy this thing?” I am just like, “My brother, we are an API. You are a developer. It’s none of my business,” right? Like, you shouldn’t know what the whole task even is

**Swyx \[00:16:46\]:** Yeah

**Diogo Almeida \[00:16:46\]:** Because it should be decomposed into small things. We shouldn’t be able to know what the downstream users are doing, and that is, like, a good boundary to give software engineers maximum power. Ideally, they use it for the good stuff, and ideally, we can, like, help them and like, we’ve talked about, like, doing open source and charity and all of that. We have absolutely no time for anything else right now. But like, they will get any of that bias out of the technological layer as long as I’m in charge.

**Swyx \[00:17:11\]:** Yeah, that’s great. While we’re on the topic, let’s also briefly talk about your privacy stuff, terms of ser- terms of use, which, got a little bit of

**Diogo Almeida \[00:17:18\]:** Ooh

**Swyx \[00:17:18\]:** Misunderstanding. I just wanna clarify that upfront.

**Diogo Almeida \[00:17:21\]:** Hell yeah.

**Swyx \[00:17:21\]:** I think this probably takes two sentences from you about, like, you will not. You’re not being that restrictive about your API. Like, clearly

**Diogo Almeida \[00:17:27\]:** Oh, yeah. Oh, yeah, so yeah

**Swyx \[00:17:27\]:** Ideologically, you articulate your role as a platform very seriously.

**Diogo Almeida \[00:17:30\]:** Yes. Yes. I don’t know what you’re referring to, but like, this was. I’ve seen a couple of things about, like, benchmarking.

**Swyx \[00:17:38\]:** Yes.

**Diogo Almeida \[00:17:38\]:** Like, obviously we’re not stopping people from do. Oh, man, I should be careful about what I say. I’m realizing

**Swyx \[00:17:43\]:** No, you said, you said it publicly that

**Diogo Almeida \[00:17:44\]:** Yeah

**Swyx \[00:17:44\]:** That was in the preview period. You didn’t take it out for the launch.

**Diogo Almeida \[00:17:47\]:** Yeah. Okay.

**Swyx \[00:17:47\]:** And now you’re gonna take it out.

**Diogo Almeida \[00:17:48\]:** So the team is doing stuff that

**Swyx \[00:17:49\]:** Yes

**Diogo Almeida \[00:17:49\]:** I’m not even aware of, so it’s great to know the team communicated that. I asked them to check in with the lawyers about that.

**Swyx \[00:17:54\]:** Yeah.

**Diogo Almeida \[00:17:54\]:** Like, we are obviously not stopping people from doing that type of thing. I’m extremely in favor. So I’m extremely anti-public benchmarks. I’m extremely in fa- I’m medium about private benchmarks that are proxies. I

**Swyx \[00:18:09\]:** So are you worried about, saturation or, like, training on public benchmarks? So it’s, like, easy to cheat.

**Diogo Almeida \[00:18:15\]:** Not only is it easy to cheat, there’s a lot of ins. So I think that we are. Or anyone who’s, like, competition with us that, vaguely there is. Like, you could say, like

**Swyx \[00:18:28\]:** There’s like 50 Jev clones, yeah.

**Diogo Almeida \[00:18:30\]:** Well, sure.

**Swyx \[00:18:31\]:** Yeah.

**Diogo Almeida \[00:18:32\]:** Well, the, these. Let’s say that there is competition.

**Swyx \[00:18:34\]:** And we’ll talk about those. Yeah.

**Diogo Almeida \[00:18:34\]:** Or let’s just say that there’s. Let’s just assume that there’s an industry two years from now of people who are doing similar things to us. The thing that we are selling is intelligence per something, per, like, dollar or per second. The. No one. Like, people obsess about the cost and the speed. I believe that is. It’s cool, but like, the thing that matters is the intelligence. Like, the cost and the speed are, like, are bad things. You’re paying them for something, and you need the thing back, and the intelligence is what truly matters. The problem with intelligence is that there’s a je ne sais quoi to it, right? Like, the good model smell. Like, the thing that happened after we launched of, like, two hours later that actually went way bigger than the video, which was like, “Holy shit.”

**Swyx \[00:19:16\]:** This is actually usable.

**Diogo Almeida \[00:19:17\]:** It. Well

**Swyx \[00:19:17\]:** Yeah.

**Diogo Almeida \[00:19:17\]:** It’s, like, beyond that.

**Swyx \[00:19:20\]:** Yeah.

**Diogo Almeida \[00:19:20\]:** Like, the. Whew, the launch was crazy, and people could really sense how hard we care about that, and that’s truly what I think the long term of this is. And I think public benchmarks are antithetical to this. Like, they are a way to get people trust in intelligence because intelligence has a je ne sais quoi, but the public benchmarks are extremely gameable. Even if they try not to, they still will. Like, back in the old days, every lab had a team to collect data that looks like MMLU to make it look better, which is just benchmarking with extra steps.

**Diogo Almeida \[00:19:58\]:** So I believe that in the long run, it needs to be vibes and trust until you put it into a workflow and evaluate it for that workflow and measure it and have your own sense of, like, how it does on the exact workflow that matters. And our job is to keep moving the nines of reliability. This is like an ever-present part of o- of what we need to be doing as a company, and we need to do everything to have people know that this is something we care so much about. Like, if we wanted to, we could have released Jev, like, a year and a half ago if we wanted it to be dumb.

**Swyx \[00:20:34\]:** Oh.

**Diogo Almeida \[00:20:34\]:** It. Like, the. My bitterest lesson, right? Like, architecture and Yeah.

**Swyx \[00:20:40\]:** I’ll bring it up

**Diogo Almeida \[00:20:40\]:** Hell yeah

**Swyx \[00:20:41\]:** Since you, since you talked about it, here.

**Diogo Almeida \[00:20:43\]:** Hell yeah. T- like, Sutton says that algorithms beats compute very roughly. Data matters way more than compute, obviously. And doing the right task, having the North Star is the hardest, most important thing. This has happened, in LLM land twice so far, right? Maybe 2.2 times. There’s RLHF, which, like, shifted the task to instruction following. No one realized that was possible. RLVR did, like, a tiny little, like, edit to the, to the direction, and now us, right? RLCD. We have a new task, and the goal is, programs in the loop. And yeah, data matters so

**Swyx \[00:21:28\]:** Right

**Diogo Almeida \[00:21:28\]:** Unbelievably much.

**Swyx \[00:21:29\]:** So

**Diogo Almeida \[00:21:29\]:** Like, I can’t, I can’t emphasize it less.

**Swyx \[00:21:31\]:** Yeah, you consider yourself a data lab rather than, like, a model lab. Is that

**Diogo Almeida \[00:21:35\]:** Absolutely

**Swyx \[00:21:35\]:** Something. That’s the wording you guys use?

**Diogo Almeida \[00:21:37\]:** Yeah. We are. We will always, like, care so much about data. To me, model capabilities means data. Data is so unbelievably complicated, and that is what gets nines. Like, you have no idea how much data can shift everything. Data is so important.

**Swyx \[00:21:57\]:** Yeah.

**Diogo Almeida \[00:21:57\]:** Holy crap. So if people are looking for a job, we are hiring infinite data people, actually infinite.

**Swyx \[00:22:04\]:** What is a good data person? Like, clearly somebody who cares about reading through the transcripts of, whatever. You’ve said, for example, that y- all your data is synthetic.

**Diogo Almeida \[00:22:15\]:** Yep.

**Swyx \[00:22:15\]:** But that’s only, like, the scratching the surface, right?

**Diogo Almeida \[00:22:18\]:** Yeah.

**Swyx \[00:22:19\]:** Like, it’s not. Like, synthetic, so what, right? Synthetic, but we have people with a lot of taste and a lot of care looking at, looking at these, articulating what’s wrong, going back, regenerating. Is that what a good data person is these days?

**Diogo Almeida \[00:22:31\]:** Let me try to figure out how to. Like, it’s, it’s super complicated, and like, I literally onboard the data people with a Talk that I assume is longer than this podcast will end up being. So I will try to say, like, the high level of it. So number one, we don’t do the kind of synthetic data that people ki. Well, I’ll do. Actually, number is zero. Data and synthetic data depends on your task. Like, the shape of your data. The shape of your task changes the data. Like, RLVR’s data is kind of environments, right?

**Swyx \[00:23:03\]:** Yes.

**Diogo Almeida \[00:23:04\]:** RLHF’s is the human feedback? Each task has its own unique kind of data, and we, of course, have our own unique kind of data, right? So number one, we have that. Number two, the thing I. The reason why we don’t want to train on our users’ data, even if we could, right? Like, we could probably ask for any terms right now, and it will. We. I don’t know if it would make a difference. We truly don’t want that, because no matter what, the real-world data has so much bias. There’s, like, a power law of, like, people, like, asking the same things where you’ll end up, like, overfitting to it and like, fracturing to it and all of that. And number two, we are, like, aiming for, like, a complete sci-fi future years from now where, like, these models are going to be, like, the general infrastructure, layers and layers and layers and deep down the stack to, like, things people can’t even imagine. Like, I would like to think of our model, like, kind of like, UDP as LLMs and TCP as our models. All sorts of stuff can be built on top of that, and we need to be able to nail those futuristic use cases such that software developers can actually build that futuristic stuff. And the way to do that is even if we had all of the data of the present, we would just overfit to the present, and then it wouldn’t work. What we need is to, like.

**Diogo Almeida \[00:24:21\]:** It almost feels like a. Like, they’re the artists? They study this cognitive core. Our cognitive core is, like, way less jagged than anyone else’s. And then they find the jaggednesses, and then they address them surgically in a way that. And you can never perfectly do this, right? But they do it in such a way that it addresses it in every single possible, like, dimension, past, present, future.

**Swyx \[00:24:45\]:** The general case rather than the specific case.

**Diogo Almeida \[00:24:47\]:** Exactly. And like, that requires a lot of intelligence every time.

**Swyx \[00:24:50\]:** Okay, so we mentioned a little bit. You sort of criticized my thinking as r-- like, very RLVR influence, which is, like, very fair. Let us actually mention RLCD

**Diogo Almeida \[00:24:59\]:** Ooh

**Swyx \[00:24:59\]:** Which obviously you have some secret sauces to our knowledge. You’ve never actually published a paper or anything like that on it. No, right?

**Diogo Almeida \[00:25:05\]:** No, not yet.

**Swyx \[00:25:06\]:** But like, what should people get from this? Like, what. Can you give people some confidence that you’re just not just making up jargon for the sake of sounding cool, right? Like, one thing for me is, like, calibration I do think is a. To me, like, well understood because we’ve covered it in. On the podcast.

**Diogo Almeida \[00:25:22\]:** Yeah.

**Swyx \[00:25:22\]:** But I don’t know what you mean when you say RLCD versus what people are familiar with.

**Diogo Almeida \[00:25:26\]:** It’s a great question.

**Swyx \[00:25:27\]:** Yes.

**Diogo Almeida \[00:25:27\]:** And actually, I will give a related question.

**Swyx \[00:25:29\]:** Okay.

**Diogo Almeida \[00:25:29\]:** What is RLHF?

**Swyx \[00:25:31\]:** Okay.

**Diogo Almeida \[00:25:31\]:** Right? And actually, RLHF means multiple different things, right?

**Swyx \[00:25:34\]:** Okay.

**Diogo Almeida \[00:25:34\]:** Like, there’s the RLHF of the original. I think it was, like, Paul Christiano teaching a robot to backflip or something like that. Wasn’t there something

**Swyx \[00:25:42\]:** Was that it?

**Diogo Almeida \[00:25:43\]:** That was the original

**Swyx \[00:25:44\]:** I referenced the PPO paper, but I don’t know.

**Diogo Almeida \[00:25:46\]:** And so PPO was not necessarily from human feedback, if I recall.

**Swyx \[00:25:51\]:** Okay. That’s true

**Diogo Almeida \[00:25:52\]:** But I b- I believe it was, like, an OpenAI alignment work that could teach hard to specify outputs, like a backflip. I’m not 100% sure. And then there was actually learning to summarize. This was work, by a bunch of the team that helped with, instruct-- and co-authored, the instruction following paper, which was teaching, doing PPO on language models.

**Swyx \[00:26:15\]:** This is the, sorry. I’m trying to, trying

**Diogo Almeida \[00:26:19\]:** Yeah

**Swyx \[00:26:19\]:** Trying to manipulate this thing. This is 2017.

**Diogo Almeida \[00:26:23\]:** Yeah.

**Swyx \[00:26:23\]:** Right.

**Diogo Almeida \[00:26:23\]:** I’m not 100% sure, but like, that looks quite right.

**Swyx \[00:26:26\]:** Yeah.

**Diogo Almeida \[00:26:26\]:** If it has, like, a robot doing backflips or something like that might be it. Yes. Okay, cool. I guess I got it right. Hell yeah.

**Swyx \[00:26:35\]:** There you go.

**Diogo Almeida \[00:26:36\]:** Yeah.

**Swyx \[00:26:36\]:** That’s the one.

**Diogo Almeida \[00:26:36\]:** So the idea was can, like, can you do, like, ill-specified things with it? So that’s, like, version one. Version two was, the learning to summarize work, that, like, OpenAI did, which is actually, like, PPO on language models to do something somewhat ill-specified. This is, like, another thing that people refer to as RLHF Which I did not co-author.

**Diogo Almeida \[00:26:57\]:** Oh, Dario’s there. Cool. Hell yeah.

**Swyx \[00:27:01\]:** And Radford.

**Diogo Almeida \[00:27:02\]:** Yeah. Shout-outs to Alec and Ryan. Love them.

**Swyx \[00:27:04\]:** Yeah.

**Diogo Almeida \[00:27:05\]:** But the thing that I refer to RLHF is the, Oh, man.

**Diogo Almeida \[00:27:13\]:** I’ll get to

**Swyx \[00:27:14\]:** You have comments on that, yeah.

**Diogo Almeida \[00:27:15\]:** I have comments on that paper, but like, we’re so many, tangents deep.

**Swyx \[00:27:18\]:** Yeah.

**Diogo Almeida \[00:27:18\]:** So the thing that really got. To me, the thing that I’m calling to RLHF is the task of instruction following. It’s not about the PPO. That part doesn’t matter. It’s about, like, setting a North Star of this is a valuable direction. It’s kind of like the Bitris lesson North Star.

**Diogo Almeida \[00:27:34\]:** And for us, RLCD is this new task. And it is not. I don’t see it as jargon. Like, I try to communicate with precision. It’s just that, “Hey, here’s another North Star.” Just like DPO and all of its, like, descendants also do RLHF, despite not using the algorithm in that paper.

**Swyx \[00:27:55\]:** And so clear- clearly stating the North Star is, being program- programmable AI is one, word that I really catch onto, removing the human in the loop,

**Diogo Almeida \[00:28:06\]:** Yes

**Swyx \[00:28:06\]:** From. Because RLHF is tuning

**Diogo Almeida \[00:28:09\]:** Yes

**Swyx \[00:28:09\]:** For this so that you can automate everything.

**Diogo Almeida \[00:28:11\]:** Yes. Everything that makes

**Swyx \[00:28:13\]:** Did I miss anything else in the, in the thesis of, like, what the North Star is?

**Diogo Almeida \[00:28:17\]:** There is. That is. That is right. I’m overly nuanced in my communication. The one nuance is that we need to be practical. We need to be aware of what language models can do really well. Like what AI can do.

**Diogo Almeida \[00:28:30\]:** Right? Like, there could be programmatic types that are, like, sick AF, but if you. If the technology is not ready for it to. It’s not a tragedy if that’s not out in the world.

**Swyx \[00:28:41\]:** Yeah.

**Diogo Almeida \[00:28:42\]:** But to me, like, the pre-Jev world was a tragedy becau-- it sounds arrogant. Hear me out.

**Swyx \[00:28:49\]:** No. I strongly believe you.

**Diogo Almeida \[00:28:50\]:** Cool. It sounds arrogant, but like, I felt this way since long before I even had a company.

**Swyx \[00:28:54\]:** Yeah. I can, I can vouch that,

**Diogo Almeida \[00:28:56\]:** Yes, I’ve been talking about this for so long

**Swyx \[00:28:57\]:** You said this at All Around Her for, like, three years.

**Diogo Almeida \[00:28:58\]:** Yeah, I’ve been talking about this for so long. And I’ve been saying it because I thought it would have been easier. They say they do not do things because they. It. They’re easy. They. It’s ‘cause they thought it was easy, so

**Swyx \[00:29:08\]:** Yeah, exactly

**Diogo Almeida \[00:29:09\]:** Something like that. I thought it. This whole project would take a week.

**Diogo Almeida \[00:29:13\]:** And I was unbelievably wrong. So I am so sorry to everyone at OpenAI that I thought. I was like, “Man, I’m solving this right now.” but like, I think that the tragic thing is when. Well, I think overpromise, underdeliver is tragic too. And like, AI is super extreme on that axis. And I think RLVR is, like, the main. Well, both RLVR and RLHF are extreme perpetrators of this.

**Diogo Almeida \[00:29:40\]:** But like, it. To me, it’s like it’s just there’s just so much potential there. Like, AI is clearly so smart. I l- smart. I love this in my talks, when I ask people, like, “How can AI be so unbelievably smart? How can we, like, solve millennium prize problems in math, but still not automate even the most basics of works?” Like, really basic rote stuff that, like, the. It d- it doesn’t take, like, extremely smart people to do this. It’s not a satisfying job. Like, there’s other things these people could be doing, but yet we need them to do, like, this ba- like, super basic- non- unsatisfying stuff because, like, we can’t automate it yet, but we have this, like, supercharged engine of automation that just does not have, like, the right plugs and stuff to plug into all of this economically valuable work. And like, if the whole company of TypeSafe disappears, like, maybe it’ll take, like, a year or two for people to, like, truly catch up. I actually don’t know how long it’ll take. If model quality matters, then we are gonna be in a very good position for a long time. But it, like, it’s done, right? Like, there, like, this has changed the path of, like, technological history.

**Swyx \[00:30:49\]:** Yeah.

**Diogo Almeida \[00:30:49\]:** And like, we will be exploring that space as a field.

**Swyx \[00:30:53\]:** Yeah. I think, I definitely agree with that. You’ve created possibilities. So I think, if I can paraphrase so that people can un- also understand, you should not take the success of TypeSafe and Jev as just like, “Well, that is a new model type. Now we’re done. We go back to business.” Like, no. Like, actually, there’s, there are, like, five other model types that you should be exploring and like, let a thousand flowers bloom.

**Diogo Almeida \[00:31:15\]:** Absolutely.

**Swyx \[00:31:16\]:** Right?

**Diogo Almeida \[00:31:16\]:** Like, early internet

**Swyx \[00:31:17\]:** And some of that, some of which you will probably also build.

**Diogo Almeida \[00:31:18\]:** Of course, yes.

**Swyx \[00:31:19\]:** Yes.

**Diogo Almeida \[00:31:19\]:** Early internet energy. I think it’s back to tech utopia. It’s no longer like, “Oh, man, like, sometimes my coding agents work, but the, all of the best ones are hoarded internally.”

**Swyx \[00:31:29\]:** Yeah.

**Diogo Almeida \[00:31:30\]:** Right? It’s like creation is back on the menu.

**Diogo Almeida \[00:31:34\]:** ? Though it’s gonna be a wild-ass world, and buckle up.

**Diogo Almeida \[00:31:38\]:** It’s. And I’m so jazzed about that.

**Swyx \[00:31:42\]:** Yeah. And now you have the funding and the momentum to do whatever you envision there, which I, which I think is, like, very gratifying to see you have after, so long of saying these things

**Diogo Almeida \[00:31:53\]:** Yeah

**Swyx \[00:31:54\]:** But actually show the world.

**Diogo Almeida \[00:31:55\]:** I know. I just. Such a, such an interesting thing to be a tease the whole time. Like, my talk, like, felt like it was a cliffhanger ‘cause I didn’t say how the automation would occur.

**Swyx \[00:32:05\]:** Yeah.

**Diogo Almeida \[00:32:06\]:** Sean reviewed our manifesto And he’s like, “It’s a little bit vague in these parts.”

**Diogo Almeida \[00:32:12\]:** And like, “What’s step one? What is, what is the intelligence model?”

**Swyx \[00:32:16\]:** Well, I asked you for model, and you were like, “Yeah, model coming.”

**Diogo Almeida \[00:32:18\]:** Yeah.

**Swyx \[00:32:18\]:** And like, Well, I just, I mainly objected to the word composable But build prod.god is fantastic.

**Diogo Almeida \[00:32:24\]:** Thank you.

**Swyx \[00:32:24\]:** Yeah.

**Diogo Almeida \[00:32:25\]:** I. We’ve really rallied around that. I’d like to think we’re not entirely a cult like some companies are.

**Diogo Almeida \[00:32:32\]:** But like, we are, like, jazzed about what we’re doing, and like, we are. Like, my brand is being practical, and like, we are all, like, so super-duper practical.

**Swyx \[00:32:42\]:** Yeah.

**Diogo Almeida \[00:32:42\]:** It’s really great.

**Swyx \[00:32:43\]:** Yeah. So here. And by the way, here is the step, the secret master plan, right?

**Diogo Almeida \[00:32:47\]:** Yep.

**Swyx \[00:32:47\]:** Shape, the shape of machine-native composable AI.

**Diogo Almeida \[00:32:49\]:** It was your idea to make a secret master plan, so

**Swyx \[00:32:51\]:** It’s a, it’s that Elon thing. When he started Tesla

**Diogo Almeida \[00:32:53\]:** Yeah

**Swyx \[00:32:53\]:** He was like, “Here’s what we’ll do.”

**Diogo Almeida \[00:32:54\]:** But I did. Yeah. I’m giving official credit to you.

**Swyx \[00:32:56\]:** Oh, thank you. Thank you, thank you.

**Diogo Almeida \[00:32:56\]:** Yeah.

**Swyx \[00:32:56\]:** Thank you. But like, you should’ve told me your, you’re also gonna do this model launch, ‘cause you, like, you told me, you told me half of the story, and then the other half, you didn’t have the doom demo at the time.

**Diogo Almeida \[00:33:08\]:** Yep.

**Swyx \[00:33:08\]:** You didn’t have any numbers to give me.

**Diogo Almeida \[00:33:10\]:** Yep.

**Swyx \[00:33:10\]:** I was like, “what?”

**Diogo Almeida \[00:33:11\]:** Well, the problem is I don’t believe in benchmarking.

**Swyx \[00:33:13\]:** Exactly.

**Diogo Almeida \[00:33:14\]:** Right?

**Swyx \[00:33:14\]:** Exactly.

**Diogo Almeida \[00:33:14\]:** So like, it is a thing that you need to feel, and like, I think that this is the way to build long-term trust, even though it, like, hurt, it hurt us a, us a lot? Like last year when we did fundraise, no one believed us.

**Diogo Almeida \[00:33:27\]:** ? Like, and they wanted just benchmarks and stuff, and we’re like, “We’re not gonna do that. We are principled. We’re gonna stand by our guns. That rewards bad actors. I don’t give a shit, like, what you want. Like, this is who we are, and we are standing by that.” So Sorry. It’s not

**Swyx \[00:33:43\]:** No, yeah. Well, and in some ways, I think, like, choosing the hard path, it. But you end up making the company that you wanna work in.

**Diogo Almeida \[00:33:49\]:** Yep.

**Swyx \[00:33:50\]:** Right? Otherwise, if you sell out, then you’re just working in, like, OpenAI but with my people, right? Which is like.

**Diogo Almeida \[00:33:56\]:** Yeah. Yeah. Like, I’m, I don’t have too many regrets on that, obviously.

**Swyx \[00:34:01\]:** Yeah.

**Diogo Almeida \[00:34:01\]:** Like, it worked out so unbelievably well. And like, I, The. I was emotional last night when I was talking about, like, the reasons I left OpenAI, and because, like, it actually had to change my wording after the launch. My phrasing was, “If an AI winter did happen and I did not do every fucking possible thing I could to, like, avert that, I would see myself as personally responsible both for, the RLHF direction, which I think really widened overpromise versus under-deliver, and also not going all in on this because I think this is, this is where value is going to just be, like, printed.” So. And it was really cool because I feel like

**Diogo Almeida \[00:34:47\]:** The AI winter I’m worrying about is averted. Like, AI will be useful. It’ll be used for automation.

**Diogo Almeida \[00:34:53\]:** It’s been less than a week, and like, the numbers are already undeniable

**Swyx \[00:34:57\]:** Yeah

**Diogo Almeida \[00:34:57\]:** That it’s, like, being used for real work, and like, there’s. It’s, it’s the Wild West. Yeah.

**Swyx \[00:35:03\]:** Yeah. Can you sh- just if you have top of your head, what numbers are you seeing? Like, what’s, what’s, like, signups? Like, whatever you can share.

**Diogo Almeida \[00:35:11\]:** I’m actually not super on top of everything. Like, the team is the ones who are telling me all of these things.

**Swyx \[00:35:16\]:** Yeah, and I’m sure it’s, like, changing every day, right?

**Diogo Almeida \[00:35:17\]:** It’s, it’s,

**Swyx \[00:35:18\]:** But like

**Diogo Almeida \[00:35:18\]:** It’s kinda nuts

**Swyx \[00:35:19\]:** If there’s a milestone that you’re like, “Well, yep, that’s one thing we were hoping for. We reached it.”

**Diogo Almeida \[00:35:23\]:** I will say a milestone that we’ve passed is tokens per day.

**Swyx \[00:35:27\]:** Nice.

**Diogo Almeida \[00:35:27\]:** And this is not, like, fleeting tokens per day.

**Swyx \[00:35:32\]:** Yeah.

**Diogo Almeida \[00:35:32\]:** This is, like, even at night, like, it’s constantly training, so machines are calling it and not just people trying things out.

**Diogo Almeida \[00:35:39\]:** So that is, That is so cool. A trillion tokens a day is a lot.

**Swyx \[00:35:45\]:** Yeah.

**Diogo Almeida \[00:35:45\]:** So surpassing that is awesome. Signups to me don’t really matter. And actually, this was, like, a bit of a mistake we made, if I’m, like, totally honest. People on Twitter were calling us, like, marketing geniuses and all of that, and that was just us. We don’t have a marketer. Also hiring. And we were just being our genuine, goofy, like, irreverent selves, and we were, we were just, like, offboarding people off the waitlist so hard. - Our platform team is so unbelievably cracked. I think we have more n- up nines of uptime than Anthropic while having the most Unprecedented launch ever. Like, that is kind of nuts, so

**Swyx \[00:36:21\]:** Yeah

**Diogo Almeida \[00:36:21\]:** Like, props to them.

**Swyx \[00:36:22\]:** Yeah.

**Diogo Almeida \[00:36:23\]:** And the thing we didn’t realize. So number one, waitlists, waitlist sign-ups don’t matter for, like, a developer platform, in my opinion? I would guess that a large number of them are not even developers. So they go in, they try some queries, and a lot of people don’t get it because they are not programming, right? Like, they’re just like, “What? This is not a chatbot. Where’s my ChatGPT 2?”

**Diogo Almeida \[00:36:45\]:** Right? But if, like. I haven’t exactly calculated this. My sense is that if every single human being in the world, like, just wrote a couple of queries, that would be a rounding error compared to, like, one power user’s for loop that is just, like, creating value.

**Swyx \[00:37:01\]:** Yeah.

**Diogo Almeida \[00:37:01\]:** And the thing we are-- didn’t realize with the waitlist is, like, we could just w- off-board anyone off the waitlist. It doesn’t matter. The scary part is rate limits. And then once people start getting value from that, then they just want tons and tons of rate limits because this is what software is, right? Like, you spend effort upfront to specify your rote task, and then this rote task creates more value than it takes to put in. And then now that you have that

**Swyx \[00:37:25\]:** Set it and forget, yeah.

**Diogo Almeida \[00:37:26\]:** Exactly, yeah. You run it in the background. You make it a dependency, to, like, other things. You can make, like, higher level stuff. And like, you just create so much value in the world. Early internet people probably did not imagine, like, the wonder of early 2000s internet, which is still not early internet. But like, it’s, it’s through, no offense, composability

**Swyx \[00:37:47\]:** No

**Diogo Almeida \[00:37:47\]:** That all of the crazy stuff happens, and I just really wanted to emphasize that in our manifesto. We are going for emergence. We are going for, like, being the catalyst. We’re wanting to empower people, and we are going to do whatever we can for that, be it, like, Discords in our town hall with me wearing a garbage bag or not.

**Swyx \[00:38:05\]:** And podcasts and **Diogo Almeida \[00:38:08\]:** Hell yeah

**Swyx \[00:38:09\]:** Getting all that.

**Diogo Almeida \[00:38:09\]:** Absolutely.

**Swyx \[00:38:09\]:** Like, ‘cause I want the long form, right?

**Diogo Almeida \[00:38:11\]:** Yeah.

**Swyx \[00:38:12\]:** It is like, yes, we’ll get past the, some of the superficial things, and then we’ll go deep and

**Diogo Almeida \[00:38:15\]:** Hell yeah

**Swyx \[00:38:15\]:** And people will really trust and understand your mission and like, the people that, will resonate that will end up joining you or, buying you. Or No, but sorry, as a, as a customer.

**Diogo Almeida \[00:38:27\]:** Oh, as a customer.

**Swyx \[00:38:28\]:** As a customer, as a customer.

**Diogo Almeida \[00:38:28\]:** Okay, yeah. That was funny. I’m sorry.

**Swyx \[00:38:30\]:** Sorry. I didn’t, I didn’t mean to say that. But no, any-- one version, one very flattering version of this, like, 36 million views of your launch video.

**Diogo Almeida \[00:38:37\]:** Cool. Up to 38 now.

**Swyx \[00:38:39\]:** Yeah, rounding error.

**Diogo Almeida \[00:38:40\]:** Yeah.

**Swyx \[00:38:40\]:** Navio still has got 74. Fable 5 got 57. So like, as far as, a- and I didn’t, I didn’t do the stats for, like, original ChatGPT, like

**Diogo Almeida \[00:38:48\]:** Yep

**Swyx \[00:38:49\]:** Which there was no video.

**Diogo Almeida \[00:38:50\]:** Yep.

**Swyx \[00:38:50\]:** So like, up there, right?

**Diogo Almeida \[00:38:52\]:** Yep.

**Swyx \[00:38:52\]:** Like, as far, as far as, like, if you were to launch a Neolab in 2026, I think you’re, like, number one right now, which is, like, pretty crazy.

**Diogo Almeida \[00:38:58\]:** Yeah. Well, I actually would rather. I do have the shirt, like, your favorites Neola-- favorite Neolab’s favorite Neolab.

**Swyx \[00:39:05\]:** Huh.

**Diogo Almeida \[00:39:05\]:** I don’t give a shit about being a Neolab. I think being a Neolab. Actually, we have a lot of, like, swag that’s being a parody of a Neolab. One of them, one of them I have is, like, Neolab with product, which actually is not a Neolab. Like, I don’t care about that, really.

**Swyx \[00:39:20\]:** Yeah.

**Diogo Almeida \[00:39:20\]:** What I care about is being a reliable dev platform. So **Swyx \[00:39:23\]:** Yes

**Diogo Almeida \[00:39:24\]:** Appreciate the comparison, but like

**Swyx \[00:39:25\]:** Yeah

**Diogo Almeida \[00:39:25\]:** Hopefully we transcend past them and we go back into, like, a thing-- like, a revolutionary moment for developers and like, this stable thing that people can rely on and trust.

**Swyx \[00:39:35\]:** Yes. To that end, I think that’s one thing that really impressed me about you guys is that, yes, you do talk about reliability. I thought it was mostly about calibration, which, like, we talk about RLCD. But actually it’s also about just, like, uptime and scalability and all those things, right? They’re, they’re all sort of the kind.

**Diogo Almeida \[00:39:55\]:** And nines.

**Swyx \[00:39:56\]:** And nines.

**Diogo Almeida \[00:39:56\]:** It’s, like

**Swyx \[00:39:57\]:** Which uptime is, in my opinion.

**Diogo Almeida \[00:39:58\]:** Oh, but that’s part of it. But like, there’s reliability in, like, how intelligent the thing is. Like, how consistently does it do the thing that you want? And I think that, like, the big reasoning models are very smart. In my opinion, they still lack reliability. I think there’s many use cases where you-- they look like they should be smart enough to automate their work. There is economic incentive to automate that work, yet still they’re not reliable enough as, at an intern because they’re optimized for different things. And so like, I think that there’s the reliability of being able to, like, trust the outputs. And also we are. Like, there are dimensions of reliability that we are not yet at that I’m, like, so excited by.

**Swyx \[00:40:38\]:** Yeah.

**Diogo Almeida \[00:40:38\]:** Like, I want to automate the easy work before the hard work? Like, I think that’s just a common sense thing to do. But to me, we will be sufficient. I don’t know if there’s such thing as sufficiently reliable, but I wanna get so good that people don’t even need to try the model to know that it’ll work. It’s like, that’s like what flow state is in programming, right? Like, I’m just, like, writing queries because I need intelligence in here. And like, when. For non-trivial branching, I can just write it in like a, like a type-safe System 1 query and then get the results out of it and it just branches accurately. Like, that would be so good. Like, that’s the. That is the dream.

**Swyx \[00:41:12\]:** Yeah.

**Diogo Almeida \[00:41:12\]:** And that is, like, going to be, like, a long slog.

**Swyx \[00:41:16\]:** Yeah. We’re gonna go into your API design in a little bit

**Diogo Almeida \[00:41:19\]:** Ooh

**Swyx \[00:41:19\]:** Just to give people examples and like, maybe paths not taken, that kind of stuff.

**Swyx \[00:41:23\]:** One thing up the front that I do wonder about in terms of reliability is I noticed that there’s no seed. There’s no, And so basically, same input, do I always get the same output?

**Diogo Almeida \[00:41:34\]:** So

**Swyx \[00:41:36\]:** And if not, why not?

**Diogo Almeida \[00:41:37\]:** Oh, great question. So this is actually, like, a common question we have between. So reliability is actually a catchall. Like, whenever AI can’t automate something, it’s due to some form of reliability. Could be, like, type safety. It could be determinism. It just could be, like, it’s, it’s jagged, right? So reliability is a catchall. I just think that it’s also a catchall for, like, what the North Star is. Re- determinism is, like, same inputs, same outputs. I do believe that this is, like, slightly interesting for unit tests, but I believe that to be the wrong North Star. I believe robustness is what people

**Diogo Almeida \[00:42:16\]:** I don’t wanna tell people what they really want, ‘cause that would be a little arrogant of me.

**Diogo Almeida \[00:42:19\]:** I believe that is, like, the more important property. You want, given similar inputs, get similar outputs. And it’s kind of wild how unreliable LLMs are.

**Diogo Almeida \[00:42:31\]:** Like, a way that we test this is you put, like, UUIDs in, like little

**Swyx \[00:42:36\]:** Yeah

**Diogo Almeida \[00:42:36\]:** I think they’re called nonces In the prompt. And what you want is similar outputs from all of those, ‘cause it’s truly semantically the same question, and that is the part where you really want. Th- like, that robustness is where, like, people get, like, burnt with AI making decisions. So I think that is the. A super-duper important property. We could also have determinism. That is, that is a thing that can be available. As far as I can, like, mentally model for programmers, like, it, I- it could be valuable for some use cases, so like, please educate me, in comments or view. But my. In general, it’s easy. Determinism is something you can, like, trade off for better cost. Like, we are, we are constantly wanting to be on the intelligence per dollar frontier. We are doing, like, absolutely disgusting things to be there. Like, this is,

**Diogo Almeida \[00:43:32\]:** I shouldn’t say this, but no one’s here to stop me.

**Swyx \[00:43:37\]:** If you s- you sign off on your own PR.

**Diogo Almeida \[00:43:40\]:** That is not how it works at this company. I believe for this week, my chief of staff, Kay, is the most powerful person in tech.

**Swyx \[00:43:49\]:** Yeah. And shout-out to Kay for organizing this.

**Diogo Almeida \[00:43:50\]:** Holy sh

**Swyx \[00:43:51\]:** Yeah.

**Diogo Almeida \[00:43:51\]:** Holy shit. She is so fucking competent and powerful. She’s incredible.

**Diogo Almeida \[00:43:58\]:** She sucks. Don’t poach her. But so I try to be a bit more filtered, but like, people are telling me, “Don’t call it a Frankenstein’s monster of models,” but because that has, like, negative implications. I think Frankenstein’s monster was, like, the good guy in this whole. It was innocent, right? I didn’t read it. Okay.

**Diogo Almeida \[00:44:18\]:** I’ll, I’ll confess. Okay. That. Well, one facial expression, I

**Swyx \[00:44:21\]:** This is a

**Diogo Almeida \[00:44:21\]:** My cards on the table

**Swyx \[00:44:21\]:** Decent Jacob Elordi movie if you wanna see

**Diogo Almeida \[00:44:24\]:** I

**Swyx \[00:44:25\]:** The adaptation. Anyway.

**Diogo Almeida \[00:44:26\]:** The. You have no idea how little time I have right now.

**Swyx \[00:44:28\]:** Yeah.

**Diogo Almeida \[00:44:29\]:** My priorities are sleep?

**Swyx \[00:44:31\]:** Developers.

**Diogo Almeida \[00:44:32\]:** Developers, yes. Developers. But yes. It. We do, like, absolutely disgusting things to be on the Pareto curve of intelligence per dollar, and we are going to keep doing that.

**Swyx \[00:44:47\]:** Yeah.

**Diogo Almeida \[00:44:47\]:** We’re gonna be doing crazy-ass stuff, and I think people really need to think outside of the box. Like, part of the reason we’re surprising is, like, people Are thought inside the box, and we continue to do that. As of right now, we are obviously the best at this, and we want to continue being the best at that whole thing.

**Swyx \[00:45:05\]:** Yeah.

**Diogo Almeida \[00:45:05\]:** So Wait, where did, where did we tangent from?

**Swyx \[00:45:07\]:** No. So

**Diogo Almeida \[00:45:08\]:** Yeah

**Swyx \[00:45:08\]:** I asked you about, will you have seeds and determinism?

**Diogo Almeida \[00:45:11\]:** Oh, yes. So

**Swyx \[00:45:11\]:** And then you basically defined reliability and like

**Diogo Almeida \[00:45:14\]:** And robustness

**Swyx \[00:45:15\]:** How you see it. Yes.

**Diogo Almeida \[00:45:16\]:** But like, determina- like

**Swyx \[00:45:17\]:** I have a robustness example that’s, that’s, real quick I can show you.

**Diogo Almeida \[00:45:19\]:** I would love that. I will just say one thing.

**Swyx \[00:45:21\]:** Yeah.

**Diogo Almeida \[00:45:21\]:** We can make a deterministic model.

**Swyx \[00:45:22\]:** Exactly.

**Diogo Almeida \[00:45:23\]:** Like, we’re hap- if people can convince us that is a valuable thing to do

**Swyx \[00:45:27\]:** Yeah

**Diogo Almeida \[00:45:27\]:** And we don’t have a gigantic GPU shortage

**Swyx \[00:45:29\]:** Yeah

**Diogo Almeida \[00:45:29\]:** We can happily make all of these models. We live to please. And rev- and revolt, revolute,

**Swyx \[00:45:38\]:** You will throw over everything, except you’ll do it in a nice way.

**Diogo Almeida \[00:45:41\]:** Yeah.

**Swyx \[00:45:41\]:** And find

**Diogo Almeida \[00:45:42\]:** So like, determinism could be on the cards.

**Swyx \[00:45:44\]:** Yeah.

**Diogo Almeida \[00:45:45\]:** It just gets you less intelligence per dollar.

**Swyx \[00:45:46\]:** Yeah. Well, just having seen the trajectory of OpenAI and Anthropic, you will. Just trust me now that you will be peer pressured into doing it. So like, just people will want it even if they. If you tell them they don’t need it. They’ll still want it. So like, yeah, that’s the TL;DR of that.

**Diogo Almeida \[00:46:01\]:** Okay.

**Swyx \[00:46:02\]:** Yeah.

**Diogo Almeida \[00:46:02\]:** I will love to. Maybe one day we will see how that happens.

**Swyx \[00:46:07\]:** Yeah.

**Diogo Almeida \[00:46:07\]:** I’ve been told I’m, They say that part of our brand is being unshakeable

**Swyx \[00:46:13\]:** Huh

**Diogo Almeida \[00:46:13\]:** And they say that’s just the nice way of saying stubborn.

**Swyx \[00:46:15\]:** Stubborn, yeah.

**Diogo Almeida \[00:46:16\]:** Yeah, exactly. And I’m a very stubborn person. I don’t think we could have done it.

**Swyx \[00:46:19\]:** Yeah.

**Diogo Almeida \[00:46:19\]:** Yeah.

**Swyx \[00:46:19\]:** No, but. So like, I. Okay, but I tr- I, like, have argued with you before.

**Diogo Almeida \[00:46:23\]:** Yeah.

**Swyx \[00:46:24\]:** And I know

**Diogo Almeida \[00:46:24\]:** And you’ve been right about developers every time.

**Diogo Almeida \[00:46:25\]:** So okay, I give up. You win. You win. I’m sold that I’ve argued with you before.

**Swyx \[00:46:31\]:** No, I’m just saying, like, I think that you can hold your ground while also, like, if I give you the right evidence, you can, not. You can sort of throw away your priors and be like, “Yep, like, that actually makes sense to me.”

**Diogo Almeida \[00:46:41\]:** Yep.

**Swyx \[00:46:41\]:** And so like, just trust your own gut on this.

**Diogo Almeida \[00:46:44\]:** Yeah. Yep.

**Swyx \[00:46:44\]:** I’ll bring up some

**Diogo Almeida \[00:46:45\]:** But I suspect, though, that we will be GPU constrained for a very long time.

**Swyx \[00:46:50\]:** Very long. Yeah.

**Diogo Almeida \[00:46:50\]:** And anything that has less intelligence per dollar means it consumes more GPUs

**Swyx \[00:46:56\]:** Yeah

**Diogo Almeida \[00:46:56\]:** For the same intelligence, which is. Like, our goal is not to onboard companies. Like, r- it’s, it’s valuable, but like, our goal is to have people, like, experiment and do weird shit. And we need, like. We need to, like, get, it to as many hands as possible and like, starting, like, the California gold rush for that.

**Swyx \[00:47:14\]:** I think there is right now. Yeah.

**Diogo Almeida \[00:47:16\]:** Yeah.

**Swyx \[00:47:16\]:** Just a word of caution. I will just say it

**Diogo Almeida \[00:47:19\]:** Ooh, okay

**Swyx \[00:47:19\]:** Because somebody’s thinking about it right now.

**Swyx \[00:47:21\]:** Which is when you say things like, “We will not commit to deterministic models. We will, we’ll do whatever it takes for intelligence per dollar, and we are al- we are facing GPU constraint,” people are thinking you may quantize your models, right? Like s- like, whatever you had at launch, you may quantize down in. To reduce the quality, in order to free up, memory or bandwidth or whatever, right?

**Swyx \[00:47:42\]:** And so you should probably, have some kind of promise, which you don’t have to make now

**Diogo Almeida \[00:47:47\]:** Yep

**Swyx \[00:47:48\]:** About, like, “We will uphold model quality at launch.” People, like. So it’s like when people. When we. You were at OpenAI when you launched

**Diogo Almeida \[00:47:55\]:** Yep

**Swyx \[00:47:55\]:** All these, all these APIs, and even Claude as well. Like, when they first launched the models, the model strings, did not stay the same model at all times.

**Diogo Almeida \[00:48:04\]:** Yep.

**Swyx \[00:48:04\]:** Right? You have versioning in your models. That’s great.

**Diogo Almeida \[00:48:06\]:** Yep.

**Swyx \[00:48:06\]:** But like, you should, you should publicly commit to some kind of, like, once a thing is launched, we don’t change it.

**Diogo Almeida \[00:48:11\]:** We will not change our models when we deploy them. That is insane. We care about developers. Li- like, it makes sense if you’re. If. So- doing something like that, again, this is the problem with a for- first-party product and an API. It makes. You can do whatever you want in a first-party product, right? Like, more power to them, whatever gets that experience, that is fine. With an API, you obviously can’t do that. But I will say that we, plan to move a lot faster than many people are used to model providers, doing things. So we will be launching new models a lot faster than people think, and we are not promising long-term support for the models because we think that there’s lots of improvements to have. So there is a world that we might temporarily LTS what is right now Jev 1.13.0. We might do that ‘cause so many people are using it, and I know developers hate breaking dependencies. The alternative is fracturing our fleet, and that is a very bad vibe for everyone. It’s gonna be

**Swyx \[00:49:12\]:** Yeah, you can have, like, 100 different versions of the model.

**Diogo Almeida \[00:49:13\]:** Exactly. And if we’re iterating very fast, there would be a lot of those versions as well.

**Swyx \[00:49:17\]:** Yeah.

**Diogo Almeida \[00:49:17\]:** So we do want to have not just a LTS-supported thing eventually, long-term support. We want a really sick way of doing that. We have, like, research stuff cooking in that direction, and I think it’s gonna be the most pro-developer thing ever.

**Swyx \[00:49:34\]:** Yeah.

**Diogo Almeida \[00:49:35\]:** But it is not yet our current models, and I’m not promising that we will be able to keep the exact same models. They will get smarter every time, for sure.

**Swyx \[00:49:43\]:** Yeah.

**Diogo Almeida \[00:49:43\]:** And my sense is that even our model iterations, where it already is smart, it. Between model versions, the changes tend to be even smaller than the string models calling them twice. But when we go from, like, jagged to, like, wow, that is where the big deltas are.

**Swyx \[00:50:01\]:** Yeah. One thing, one thing that’s beautiful about LTS-ing models is that actually you can also port them to other silicon.

**Swyx \[00:50:08\]:** I don’t, I don’t know if you’ve thought about this.

**Diogo Almeida \[00:50:11\]:** No comment.

**Swyx \[00:50:12\]:** Okay.

**Diogo Almeida \[00:50:12\]:** So I care about intelligence per dollar.

**Swyx \[00:50:14\]:** Yes.

**Diogo Almeida \[00:50:15\]:** Right?

**Swyx \[00:50:15\]:** But speed.

**Diogo Almeida \[00:50:16\]:** What?

**Swyx \[00:50:17\]:** Speed as well.

**Diogo Almeida \[00:50:18\]:** We’ll see.

**Swyx \[00:50:19\]:** Yeah.

**Diogo Almeida \[00:50:19\]:** We’ll see. I

**Swyx \[00:50:20\]:** This is. This is a whole part of the inference tech tree that is, like, exploding in the past year, right?

**Diogo Almeida \[00:50:24\]:** Yeah.

**Swyx \[00:50:24\]:** Like, that you can, you can move to, like, a Cerebras

**Diogo Almeida \[00:50:27\]:** Yeah

**Swyx \[00:50:27\]:** An Etched or whatever and get, like, the 100,000 times speed up.

**Diogo Almeida \[00:50:32\]:** Yeah. Like, I think that intelligence per second is, like, a different metric, and we’ve even talked about, like, things like intelligence per dollar times second and like, metrics like this. My guess on, like, Jevons’ paradox occurring, or at least the Jev series of models, and the thing I, like, hunt people down about internally is, like, I don’t care how much smarter it is, it needs to be in the Pareto frontier. So like, that is what the brand of Jev is. It is the best thing at intelligence per dollar. For intelligence per second, we’ll see. I think that it’s an intriguing thing. I know that there’s many industries that are, like, extremely dependent on real-time stuff, and they will, like. Like, intelligence per second means tons of dollars for them. But

**Diogo Almeida \[00:51:20\]:** We’ll see. I’m, I would love to, like, do both and like, have the market correct me either which way.

**Swyx \[00:51:26\]:** Yeah.

**Diogo Almeida \[00:51:26\]:** ?

**Swyx \[00:51:26\]:** Yeah.

**Diogo Almeida \[00:51:26\]:** Like, I would love to be informed by people.

**Swyx \[00:51:30\]:** Yeah, totally. And it’s not, it’s not just real- about real time, right? It’s also about scale because, at scale, every microsecond is just multiplied by billions and trillions of times.

**Diogo Almeida \[00:51:41\]:** It depends on how background it’s running, right?

**Swyx \[00:51:42\]:** Yeah.

**Diogo Almeida \[00:51:42\]:** Like, if it’s, like, a big background, like, database MapReduce query, the latency might not matter so much as, like, the cost

**Swyx \[00:51:49\]:** Yeah

**Diogo Almeida \[00:51:49\]:** To get intelligence from it. But like, if it actually is, something more real time, like user-facing, you have budgets, like between 100 milliseconds and one millisecond that are, like, totally magical. And actually, even if you were below 100 milliseconds, if you could half that time, that means you can get double the intelligence or sequential intelligence calls to have, like, a, like, a phenomenal experience.

**Swyx \[00:52:10\]:** Yeah.

**Diogo Almeida \[00:52:10\]:** So right, that is definitely happening right now. It is super-duper cool. I love the intelligence per second use cases, but I don’t think that will be Jev’s niche.

**Swyx \[00:52:21\]:** Okay. Yeah, fair enough.

**Diogo Almeida \[00:52:22\]:** Yeah.

**Swyx \[00:52:22\]:** When thinking about the promise of faster and cheaper Typically the other. The trade-offs that other models are offering is faster but more expensive.

**Diogo Almeida \[00:52:32\]:** Yep.

**Swyx \[00:52:33\]:** Right? And so you’re. Like, one of the reasons I was thinking about why is Jev resonating so much is that you’ve done the faster but cheaper side of the quadrant

**Diogo Almeida \[00:52:41\]:** Yeah

**Swyx \[00:52:41\]:** Which is very unoccupied

**Diogo Almeida \[00:52:43\]:** Yeah

**Swyx \[00:52:43\]:** While holding intelligence, like, somewhat constant.

**Diogo Almeida \[00:52:45\]:** Yes. It w- I-- That’s a very load-bearing statement while holding intelligence constant. That’s the hard part, right? Like

**Swyx \[00:52:53\]:** Which, unfortunately. Like, so basically, you refuse to have the, to, like, do any public benchmarks, or you don’t like any public benchmarks about it

**Diogo Almeida \[00:52:59\]:** But I will. I’ve actually tried

**Swyx \[00:53:00\]:** But you need some internal sense.

**Diogo Almeida \[00:53:02\]:** Say again?

**Swyx \[00:53:02\]:** You need some internal sense of this in- this

**Diogo Almeida \[00:53:03\]:** Oh, of course.

**Swyx \[00:53:04\]:** Yeah.

**Diogo Almeida \[00:53:04\]:** Of course. We have, we have our own internal evals, for sure.

**Swyx \[00:53:08\]:** Yeah.

**Diogo Almeida \[00:53:08\]:** But it takes a lot of discipline not to game those, and it needs to be, like, a top-level priority to not game them.

**Swyx \[00:53:14\]:** Yeah.

**Diogo Almeida \[00:53:14\]:** Of course we do that, right?

**Swyx \[00:53:15\]:** Yeah.

**Diogo Almeida \[00:53:15\]:** Like, how else can we make the guarantee that our models are in the Pareto frontier of intelligence per dollar?

**Diogo Almeida \[00:53:20\]:** Right? Like, we’re not flying blind in there, right? If we’re doing, like, completely weird things with different costs or whatever else, like, how do we compare them? We plot them and get. You. We try to figure out, like, what is the best for the users.

**Swyx \[00:53:32\]:** Yeah.

**Diogo Almeida \[00:53:32\]:** So we for sure measure them. I’m not anti-measuring. But it’s extremely dangerous when you have, like, any alternative incentive, and this is the one thing that I kind of rule with an iron. Well, maybe my coworkers might think I rule many things with an iron fist, but to me, like, not shitting ourselves, about how smart our model is one of the most important things there.

**Swyx \[00:53:57\]:** Yeah.

**Diogo Almeida \[00:53:57\]:** Like, we need to be truth-seeking.

**Swyx \[00:53:58\]:** Yeah. Yeah. Agree, agreed. Okay, I wanted to go over some, details on the, API choices.

**Diogo Almeida \[00:54:04\]:** Ooh.

**Swyx \[00:54:04\]:** Mostly because this is the only podcast that will ask you these kinds of questions.

**Diogo Almeida \[00:54:07\]:** Oh, hell yeah. Hell yeah.

**Swyx \[00:54:08\]:** So you have three primitives.

**Diogo Almeida \[00:54:10\]:** Yeah.

**Swyx \[00:54:10\]:** Choice, score, know. First of all, know, where is that from?

**Swyx \[00:54:14\]:** Is this, like, a term in the, in the literature or what?

**Diogo Almeida \[00:54:17\]:** Now it is.

**Swyx \[00:54:18\]:** Yeah.

**Diogo Almeida \[00:54:19\]:** We debated this a lot. We debated this a lot. It is It is Bool-ish, right? Like true, false. It is

**Swyx \[00:54:32\]:** But it’s continuous.

**Diogo Almeida \[00:54:33\]:** Yes, exactly. So first, the origin of the name is Bernoulli.

**Diogo Almeida \[00:54:39\]:** Yes. So it-- that’s why it’s even spelled that weird way. That is, like, a subset of the name Bernoulli from, like, a Bernoulli probability, right? Which is actually what that is. So that is the origin of it. We were debating this a lot. We liked PBool, we liked Pool. We were wa-- we were wanting to call it, like, a pool party, but then no one let me. We had, like, a bunch of, like, other arguments about that.

**Diogo Almeida \[00:55:04\]:** And Noulli, we figured was, like, the best thing. Our rationale, and like, this is actually the same thing with Jev too, is that we think that we are, like, an irreverent, insane bunch, and programmers don’t care. Like, if Jev is just gonna be a string, we didn’t expect it to catch on or even have puns or anything like that, right? Actually, there was a lot of hate on the name internally. They’ve all apologized, except for one person.

**Swyx \[00:55:33\]:** Still holding strong.

**Diogo Almeida \[00:55:33\]:** Yes. Our mutual friend.

**Swyx \[00:55:36\]:** Okay.

**Diogo Almeida \[00:55:37\]:** Yes.

**Swyx \[00:55:38\]:** I respect her for that.

**Diogo Almeida \[00:55:39\]:** Yeah. Yeah. She wanted Jev to be called Meow.

**Swyx \[00:55:44\]:** She would, of course.

**Diogo Almeida \[00:55:45\]:** Yes, of course.

**Swyx \[00:55:46\]:** Okay.

**Diogo Almeida \[00:55:46\]:** Like her father, yeah.

**Swyx \[00:55:47\]:** You win there, you win there.

**Diogo Almeida \[00:55:49\]:** But like, yeah, Noulli is. We had to make a new concept for this thing ‘cause if it was a Bool, it would be confusing to people. So actually, all three of these are actually new concepts. These are not types that exist in programming, and that was intentional because they map very closely to types, but they’re not quite that. A score is not an int. So if you had, like, Instructor or Pydantic or whatever map ints or floats into scores You’d get a little bit cooked? And like, we were really erring on the side of clarity over the side of, like, making people, like, easily understand what’s going on.

**Swyx \[00:56:24\]:** Don’t you worry about that? Don’t you want things to integrate directly into things that people are already using?

**Diogo Almeida \[00:56:30\]:** Yes. Yes, we do. And actually, I think that,

**Swyx \[00:56:34\]:** You have integrations with, like, other SDKs and stuff.

**Diogo Almeida \[00:56:36\]:** Yeah.

**Swyx \[00:56:36\]:** But you have-- Sorry, you have your own SDKs.

**Diogo Almeida \[00:56:38\]:** Yep.

**Swyx \[00:56:38\]:** But typically, for example, as a developer relations person, I would be very obsessive. Like, yes, here is how you use, Jev with Instructor.

**Diogo Almeida \[00:56:46\]:** Yep.

**Swyx \[00:56:46\]:** Here is how you. That kind of stuff.

**Diogo Almeida \[00:56:49\]:** We might have that somewhere. I am so behind on everything.

**Swyx \[00:56:53\]:** Someone would do it for you in the community.

**Diogo Almeida \[00:56:54\]:** Oh, yeah. Yeah.

**Swyx \[00:56:54\]:** Not that you’re successful. People will be like, “Oh, that’s cool.”

**Diogo Almeida \[00:56:57\]:** Cool.

**Swyx \[00:56:57\]:** But like. Anyways

**Diogo Almeida \[00:56:59\]:** I don’t see that as binary either.

**Swyx \[00:57:00\]:** Yeah.

**Diogo Almeida \[00:57:01\]:** I actually see success as a score, and there’s always more to climb

**Swyx \[00:57:04\]:** Yeah

**Diogo Almeida \[00:57:04\]:** In, like, how much we can, like, be there for our community, just to be clear. And I’m. This section is stressful ‘cause I didn’t review the docs And they’re constantly changing.

**Swyx \[00:57:15\]:** Okay. But

**Diogo Almeida \[00:57:16\]:** But to me, scores do exist. So scores are similar to, like, LM judging.

**Diogo Almeida \[00:57:21\]:** Right? So like, if you want to call it, like, a judgment, I guess you could. But like, that is, like, the way people ca-- already use this type of thing, right? Like, maybe a Noulli could be, like, a probability, but everything for us is a probability. And a choice is actually closest to a function call, but a function call is, like, an extremely disgusting thing that, if you want OpenAI juice, sauce, tea, that. We should go back into that later. Like a, like, a choice is just, like, the right way of explo-- of exposing, like, a switch match statement

**Swyx \[00:57:57\]:** Yeah

**Diogo Almeida \[00:57:57\]:** Within code.

**Swyx \[00:57:57\]:** So it, like, maps cleanly to an enum.

**Diogo Almeida \[00:58:00\]:** Yep.

**Swyx \[00:58:00\]:** And you can choose to hydrate it into a function if you want.

**Diogo Almeida \[00:58:02\]:** Yes. And like, in the enum, choice is the important part of that.

**Swyx \[00:58:06\]:** Yes.

**Diogo Almeida \[00:58:06\]:** And like, actually, I think these map all into, like, programming primitives, where, like, choice maps into, like, a, like, a switch statement on an enum.

**Swyx \[00:58:13\]:** Huh.

**Diogo Almeida \[00:58:14\]:** Noulli’s mapped to if statements.

**Swyx \[00:58:15\]:** Yeah.

**Diogo Almeida \[00:58:16\]:** And scores map to sorting or thresholding at a greater than or less than.

**Swyx \[00:58:21\]:** Okay.

**Diogo Almeida \[00:58:21\]:** And this has been always what the vision is. Like, there will be more types, and they will map into programming primitives.

**Swyx \[00:58:28\]:** Yeah. Any other. So any nuance you wanna go through? For literally, this is for the Jev people who are, like, deciding to really invest in Jev. You are the expert, right? I’m just, like, wanting to provide more background for them on, API choices, how they should use some of these things, like legends, confidence, how critical in your testing, like, how. Like, just any sort of, like, pro tips that you, like, want to offer people

**Diogo Almeida \[00:58:56\]:** Yeah

**Swyx \[00:58:56\]:** When they’re down at this level.

**Diogo Almeida \[00:58:58\]:** Thank you. I love this. No. This is

**Swyx \[00:59:00\]:** This is why we’re here.

**Diogo Almeida \[00:59:01\]:** Hell yeah. I didn’t expect this. And actually, I. No one has asked me this, in probably, like, months when I was, like, onboarding, like, our DevRel.

**Swyx \[00:59:10\]:** Okay.

**Diogo Almeida \[00:59:10\]:** So sick. So our model is designed for being, like, deep in the insides of computer programs in the future. We, like, unironically believe that this will be much more massive than anything people are even considering today. And our model might not be ready for that, but we are, like, continuously working for that future. It will never be good enough at these shallow tasks. Sorry. It’ll never be g- Like, we’re not just gonna cle- keep on climbing the shallow tasks. We want to be deep in the guts of programs ‘cause that’s how you make software powerful. All the s- all the types inside of our, This is an, actually an output.

**Diogo Almeida \[00:59:47\]:** But all the, all the parts, of, like, the input, like the state, the instructions, the criteria, all of them can be structured JSON objects.

**Diogo Almeida \[00:59:58\]:** That way, like, programs can, like, insert them in the right spot, and you don’t need to, like, put things into templates. Exactly. So if ever. I think people don’t read into this part enough, and they think it’s all strings. And that’s, that’s fine. But these are all meant. Like, I would say that if you’re using, like, a template, like turning it into, like, a system message or something, you are thinking in, like, the old way? We should be making things as easy for computers to understand because that structure is truly there, right? Like, it would be weird in, like a programming language to have, like, all of your numbers in, and then you pass it into, like. You turn it into a string. Normally, you do that for printing when you have a human in the loop, right? But for, like, within the computer, you want to be passing, like, nested structure that is semantic all around. And we are really gonna be optimizing our model. It-- the model’s pretty optimized for this, but the thing is every different nested level of structure is harder to reason about, and we want-- we are really cooking hard in that direction. I think people should keep cooking that direction because it makes the code, like, so much more legible and beautiful and like, agnostic to, like, the implementation details. It’s like, here is my state, like, here’s my function state. Like, think of, think of it as, like, an AI function. Which subsets of my state, which is, like, all the variables you have available, should I pass in here? System messages are, like, disgusting global variables where you just put everything in there, and you put all this

**Swyx \[01:01:21\]:** Slop, yeah

**Diogo Almeida \[01:01:22\]:** Instructions at once. And then like, you hope that every single instruction gets nailed instead of asking the questions in parallel.

**Swyx \[01:01:29\]:** Okay.

**Diogo Almeida \[01:01:30\]:** And also, I would recommend-- I, and I truly say this not from, like, a, like, it makes me money perspective. I truly recommend asking lots and lots of questions. Break them down, make them smaller, and like, really decompose. Like, no matter if the models can do it today or not, I believe that the biggest, like, saving grace of, like, what’s happening this week will be people’s code bases, AI code bases, are gonna be so much better. Like, if you decompose problems into simple decisions, every single one of these things is extremely evaluable. Like, a AI beforehand is big system message, and then maybe you have, like, another big AI

**Swyx \[01:02:09\]:** Big output, yeah

**Diogo Almeida \[01:02:10\]:** To see, like, if it actually does this. That’s nuts? It’s, it’s kinda crazy. Like, it-- that was our Stockholm syndrome, right? But like, that’s kinda crazy. Like, if you wanna say, like, “Hey, don’t read this subdirectory,” or, “Don’t pass any API keys to DeepSeek,” or whatever else, like, that should be programmatically basically guaranteed. And you’ll never have guarantees of any machine learning model, but like, by breaking it down, you can actually s-- you can actually measure it, right? Like

**Swyx \[01:02:37\]:** Yeah, you can verify that it was actually called

**Diogo Almeida \[01:02:39\]:** Yes, and like, our model, our model-- like, the interface itself is so verifiable. This should be like a sigh of relief.

**Diogo Almeida \[01:02:46\]:** Like, it’s, it’s, it’s, it’s just gonna lead to way better engineering.

**Swyx \[01:02:50\]:** Yeah. I think I get that. And so one of the reasons people didn’t used to do this in the past is because they would just call a small LLM, right?

**Diogo Almeida \[01:02:59\]:** Yep.

**Swyx \[01:02:59\]:** And it’s still too slow, it’s still too expensive versus chunking everything that-- I’ve done exactly this myself.

**Diogo Almeida \[01:03:03\]:** Yep.

**Swyx \[01:03:04\]:** Right? Like, I benchmark. Here’s a pipeline that throws everything in system prompts and it just gets one big output versus break it down into a hundred different things. It was slower, more expensive

**Diogo Almeida \[01:03:13\]:** Yeah

**Swyx \[01:03:13\]:** Not as good.

**Diogo Almeida \[01:03:13\]:** Yep.

**Swyx \[01:03:14\]:** Right?

**Diogo Almeida \[01:03:14\]:** And that happens-- yeah. It’s, and it’s, like, super inconvenient. It’s unwieldy. Why not just put it all together? You kind of end up repeating some stuff between

**Swyx \[01:03:22\]:** Yeah

**Diogo Almeida \[01:03:22\]:** Questions, so it’s, like, maybe, like, inefficient or something like that. But then it results in something that is very hard to rely on.

**Swyx \[01:03:30\]:** Yeah.

**Diogo Almeida \[01:03:30\]:** And software doesn’t need to run in the background. It would break my heart if our stuff couldn’t run in the background.

**Swyx \[01:03:37\]:** Is there a way to break things down that you guys have found that works versus, what you thought worked and doesn’t work?

**Diogo Almeida \[01:03:45\]:** Interesting.

**Swyx \[01:03:46\]:** Because, like, people are just gonna be exploring this, now that you’ve said it. Like, they would use this as a reference and be like, “Okay, like, that’s how I’m supposed to use Jev.”

**Diogo Almeida \[01:03:53\]:** Yep.

**Swyx \[01:03:53\]:** Then the question is, how do you break things down?

**Diogo Almeida \[01:03:57\]:** Interesting. I like to break things down into its, like, its smallest semantic unit.

**Swyx \[01:04:03\]:** Yeah.

**Diogo Almeida \[01:04:04\]:** Like, what is the lowest level thing? I try to never have. I’ve probably queried, the model the most, among anyone.

**Diogo Almeida \[01:04:13\]:** And like, I try to. Number one, in my, in my queries, this is, this is a lot more like the way I prompt things. Like, I make it really structured and explicit. And in the questions, I always. I like the back ticks, but like, it works for all of them? Like, be really clear what I’m referring to because we want the model to be really literal because when you program, you want things that instruction follow really well. That is what the art of programming is, and what AI does is expanding the things, the kinds of instructions that can be followed. So I’m a fan of doing that. I.

**Diogo Almeida \[01:04:47\]:** Sometimes I’m a little lazy and I, like, I have, like, more, like, hybrid things, but like, I think that for, like, really big production things, you just want to, like, keep on adding more questions, and you wanna make it really easy to add more questions. Be really precise about all of that breakdown and then have the code to have the exact behavior you want. If I could give, like, a tiny little example of this, is, like, refusals, right? Like, I’m not gonna talk about why we don’t refuse. I might have done that already.

**Swyx \[01:05:15\]:** Yeah, you did already.

**Diogo Almeida \[01:05:15\]:** It’s like all a blur. But like, for refusals, I don’t think you should ask, “Should I refuse here?”? That’s a really. It-- I think the answer will be pretty good because, like, that’s a System 1 compatible task. But I think you’re way better off, like, asking many different independent questions about, like, the different situations you can refuse about. Because instead of having to, like, just guess based on you can actually specify what you want. And beautifully, and I think this is, like, truly really beautiful, if you find a situation where it’s like, “Oh, it didn’t refuse because of this reason. I didn’t specify this part of the task,” that is awesome. That’s what software engineering is about. Like, you fix the bug by adding that question in, adding the threshold, maybe remembering that as a test case, and now it is just solved forever. Like, your software can’t forget about that, like, in the prompt because of context rot. It is just there, and you can, like, just keep measuring that forever. And if the models are not perfect at some of these things, you can choose what threshold you want for all of these factors based on real examples. It’s like, it’s like ML without the ML, and you can just do it for anything. And like, there might be some things the model’s not good enough yet, right? Like, I would. I’m a little bit afraid when I see people doing trading with the models, like,

**Diogo Almeida \[01:06:28\]:** Automated trading. It looks cool. I th- I just think that people should leave it to the professionals.

**Diogo Almeida \[01:06:35\]:** And like, that’s just a very hard, high-level task that maybe the models aren’t good enough yet to figure out.

**Swyx \[01:06:40\]:** Yeah.

**Diogo Almeida \[01:06:41\]:** Well, I, like, even if they were, then they would- It suddenly wouldn’t be ‘cause of efficient market. But like, that’s one of those things where, you can, like, break it down into things and just evaluate them, and you might be like, “It’s not smart enough at this. Maybe we don’t deploy it yet for this version.”

**Swyx \[01:06:56\]:** Yeah.

**Diogo Almeida \[01:06:56\]:** Or we make a trade-off, or we err on the side of safety, or like, “Hey, the models are not good enough at, like, detecting, like, this weird combination of, like, sarcasm with a VIP customer, that this is when we escalate to a human.” And that’s what confidence estimates are about, too.

**Swyx \[01:07:11\]:** Okay. Very good answer. I think, one thing I’ll, I’ll mention very quickly, which, I don’t expect that you have as-- too long of an answer for is,

**Diogo Almeida \[01:07:19\]:** You don’t.

**Swyx \[01:07:20\]:** Well, no. It’s just, it’s just specifically, like, you are still relying on thresholding as, like, the lever that the user can pull.

**Swyx \[01:07:28\]:** But what if just the calibration is wrong, right? Like, you’re just saying your calibration is perfect, but

**Diogo Almeida \[01:07:33\]:** I didn’t say that.

**Swyx \[01:07:33\]:** I, like

**Diogo Almeida \[01:07:34\]:** Yeah. I didn’t say that.

**Swyx \[01:07:35\]:** So it’s like ca-- perfect calib-- and like, good calibration means, like, lower value is lower, like, sort of probability lower, higher value is probability higher. But it could be wrong. It could be

**Diogo Almeida \[01:07:44\]:** Of course, of course

**Swyx \[01:07:44\]:** Totally misaligned.

**Diogo Almeida \[01:07:45\]:** Yes.

**Swyx \[01:07:45\]:** And so then I would want to fine-tune it or something, right? Which you don’t offer, but you could. I

**Diogo Almeida \[01:07:50\]:** We could.

**Swyx \[01:07:51\]:** Again, see, this is a short answer

**Diogo Almeida \[01:07:52\]:** Yeah

**Swyx \[01:07:52\]:** Which is you don’t have it right now.

**Diogo Almeida \[01:07:54\]:** Oh, do we want to offer fine-tuning, is the question?

**Swyx \[01:07:56\]:** That could be, that could be one version of it, or you could have a different knob, right?

**Diogo Almeida \[01:08:00\]:** Yeah.

**Swyx \[01:08:00\]:** Where, like. Because, like, right now you’re-- all you’re saying is, like, if something’s wrong, a skill issue, you should, you should just change the prompt again or break it down even further, or you change the confidence.

**Diogo Almeida \[01:08:09\]:** Yep.

**Swyx \[01:08:09\]:** Those are my two options.

**Diogo Almeida \[01:08:11\]:** Yep.

**Swyx \[01:08:11\]:** Right? And that doesn’t feel super satisfying if your model is just getting it wrong.

**Diogo Almeida \[01:08:14\]:** Yep. And it will, it will get many things wrong, to be clear.

**Swyx \[01:08:18\]:** Right.

**Diogo Almeida \[01:08:18\]:** We have, like, a Report Issues button. Complain to us in Discord. We want to make it a lot better. Every single model version will be, like, notably better.

**Swyx \[01:08:25\]:** Yeah.

**Diogo Almeida \[01:08:25\]:** We will stop shipping them quickly if they weren’t getting big improvements. So number one, that is, like, totally reasonable. I think that’s simply pragmatic to admit that AI is imperfect at some stuff, right? I do think we’ll find use cases that they are, like, good enough at, and good enough kind of depends on the use case, right? Like, human beings can do a lot of work despite being bad at that work because their EV is quite high. And presumably with the right thresholding and everything, there probably is, like, large amounts of work that could be done even if mistakes are being made. On the question of fine-tuning, I could imagine, I could imagine it in the cards. I do have concerns because, like, in the what people need versus what people want category,

**Diogo Almeida \[01:09:09\]:** Like, I think general models tend to be really. Like, again, there’s the je ne sais quoi of generality, that making it good at, like, a million other tasks than this one narrow task might make it better at edge cases in that task, which I’m, I would be a little bit afraid of?

**Swyx \[01:09:25\]:** Yeah.

**Diogo Almeida \[01:09:26\]:** I could imagine it, is my answer. I’m endlessly practical on these things. I want everything. Like, my vision of the world is. I-- there’s, there’s so much we want to be building.

**Swyx \[01:09:38\]:** Yeah.

**Diogo Almeida \[01:09:38\]:** But also, like, I would not want to ship something that is, like, a giant foot gun, like some other AI companies would ship.

**Diogo Almeida \[01:09:46\]:** Yeah.

**Swyx \[01:09:47\]:** Well, so both OpenAI and Claude and I think even Gemini have rolled out fine-tuning and then took it back.

**Diogo Almeida \[01:09:53\]:** Yep.

**Swyx \[01:09:53\]:** Which is an interesting, observation that pretty much fine-tuning is now in the domain of open source models.

**Diogo Almeida \[01:10:02\]:** Yes. I do know about that. And like, it was kind of crap, so like, that’s probably better that they took it down.

**Swyx \[01:10:10\]:** Yeah. Yeah, so it could just be a foot gun, and telling people that fine-tuning it is probably the wrong way to go is great. Another interesting answer could be that, like, well, our model is so different, like, in the same way that quantization doesn’t apply to us

**Diogo Almeida \[01:10:21\]:** Yeah

**Swyx \[01:10:21\]:** Output tokens doesn’t apply to us, fine-tuning also doesn’t apply to us.

**Diogo Almeida \[01:10:24\]:** Well, actually, I’m, I’m super open to that possibility.

**Swyx \[01:10:28\]:** Yeah.

**Diogo Almeida \[01:10:28\]:** Like, my. This is not a promise. This is a desire. Just so to make it clear, I like to be really honest. Like, I think that, as intelligence per dollar gets cheaper, I think that we could get really, like, small approximate things that hopefully are proxies for intelligence. Like, is there a world where people don’t write regexes anymore? Because, like, the intelligence per dollar that uses AI is cheaper than, like, the complexity of a regex. That would be kinda sick. I would love that? And it might require fine-tuning for some of those narrow use cases to really get past the threshold. We will see. My hope is calibration gets that. Calibration plus a cascade of models. Like, if it’s super confident, then maybe it’s right. And if it’s in the middle, then you do the next bigger model, and you chain off from there. I don’t really know how that’s gonna go, but yeah. I could imagine it. And something that I could imagine too is, like, imagine you have, like, a series of. Like, we own the entire Pareto frontier. Something that a business might want to do, or, I think a hacker would be okay with dealing with a Pareto frontier of models. Maybe a business wants something more dynamic. You could imagine, like, having, like, a s- different sizes of models and to dynamically pick which model based on how smart it is on different parts of your stack. And you could even imagine, because of how simple our thing is, you could imagine, like, some automatic fine-tuning on that.

**Swyx \[01:11:53\]:** Yeah.

**Diogo Almeida \[01:11:54\]:** Not a promise in the slightest. I’m just, like, cooking on sci-fi.

**Swyx \[01:11:57\]:** But you would consider different sizes of Dev models so to offer that variance?

**Diogo Almeida \[01:12:01\]:** Absolutely. Yeah. Like, we. Like, how would I know how much intelligence people need?

**Swyx \[01:12:06\]:** I don’t know.

**Diogo Almeida \[01:12:06\]:** Right? Yeah. I don’t know either.

**Swyx \[01:12:08\]:** Demand is, demand is, unlimited.

**Diogo Almeida \[01:12:10\]:** Well, yeah, people are telling us not to ship things right now because we don’t need to ship things because, again

**Swyx \[01:12:16\]:** It’s good enough, yeah.

**Diogo Almeida \[01:12:18\]:** Yeah, but that’s kinda lame. And I really like the saying. This is something that I hope people hold me to because it’ll be hard to

**Swyx \[01:12:27\]:** To take back

**Diogo Almeida \[01:12:27\]:** To walk back from. Yeah. Like, the. I don’t know if it. Exactly the saying that culture is what you do when the market doesn’t reward it. And I really like that because I think that we are standing for something. Maybe in the future- what we’re standing for is, like, so obvious that we’re the equivalent of, like, boring, like, Visa or something like that. And like, we’re just like a utility that no one really thinks about, and I’ll be wearing non-pink suits or whatever else.

**Diogo Almeida \[01:12:54\]:** But I really want to be, like, rallying the world to this? Like, I want to keep doing cool stuff, bec- not because we need to, but ‘cause I want, like, people to realize that this is just the beginning? Like, that wasn’t even meant to be the opening salvo. That was, like, kind of like a, low-key research preview or whatever you wanna call it.

**Swyx \[01:13:14\]:** Yeah.

**Diogo Almeida \[01:13:15\]:** And there’s a lot more we can do.

**Swyx \[01:13:17\]:** Yeah.

**Diogo Almeida \[01:13:17\]:** With. Like, machine-native intelligence is gonna go wild.

**Swyx \[01:13:21\]:** So not the only s-- Potentially not the only size, potentially not the only model that you guys launch. That you want to open people’s mind

**Diogo Almeida \[01:13:28\]:** Absolutely not for any of those.

**Swyx \[01:13:29\]:** Yeah.

**Diogo Almeida \[01:13:29\]:** I want, I want to, like, meet whatever needs we can.

**Swyx \[01:13:33\]:** Yeah.

**Diogo Almeida \[01:13:34\]:** Right? Like, at. But with, like, a giant caveat, I don’t want to be like OpenAI’s product teams that, like, throw stuff at the walls. Like, I want it to be, like, in a, under a unified vision. Like, if you go back to the manifesto, like, everything needs to be under one of these three things

**Swyx \[01:13:48\]:** Yeah

**Diogo Almeida \[01:13:48\]:** In my opinion.

**Swyx \[01:13:50\]:** I’m not

**Diogo Almeida \[01:13:50\]:** Yeah.

**Swyx \[01:13:51\]:** I’m not prepared to do this,

**Diogo Almeida \[01:13:52\]:** Oh, I’m sorry. I’m sorry, Francis. Yeah, I can just talk about it. Like, we have, like, three steps in our stuff.

**Swyx \[01:13:57\]:** Yes.

**Diogo Almeida \[01:13:57\]:** It sounds like a tease. I want everything to go under one of these three things

**Swyx \[01:14:02\]:** Good

**Diogo Almeida \[01:14:02\]:** To keep pushing the boundaries and everything. Like, this is not. These are not, like, checklists. These are, like, axes that we think build, like, the foundation of, Of, like, a new technological revolution. And I want all of the. All the bets we make to be somewhere in there. And we will be doing some weird stuff model-wise.

**Diogo Almeida \[01:14:23\]:** So because machine-native, right? Like, humans don’t need to totally get it. It needs to just be valuable.

**Swyx \[01:14:30\]:** With, Just give people a tease or hint. Like, what does weird look like? What is weird?

**Diogo Almeida \[01:14:35\]:** I’ll give people a hint.

**Swyx \[01:14:36\]:** Yeah.

**Diogo Almeida \[01:14:36\]:** Some people are trying to call them decision models.

**Swyx \[01:14:40\]:** Okay.

**Diogo Almeida \[01:14:41\]:** That our primitives are decisions. I wouldn’t do that, because I think there’s other types that are machine-native that are not decisions.

**Swyx \[01:14:54\]:** Okay, we’ll leave it at

**Diogo Almeida \[01:14:54\]:** That’s a fun hint, a fun hint.

**Swyx \[01:14:55\]:** And let people guess. Yeah.

**Diogo Almeida \[01:14:56\]:** Yeah. I think it’s a, I think it’s a pretty fun hint.

**Swyx \[01:14:58\]:** Yeah. There’s people. Look, there’s, there’s people saying like, “I’ve done this before. I made a decision model a year ago.” Like, Jev is not new, Jev’s not cool.

**Diogo Almeida \[01:15:04\]:** Yeah.

**Swyx \[01:15:04\]:** But like, I think, there’s the categorical, like, here’s what you’re establishing is possible. There’s the, performance of, like. Well, actually the. For the benchmarks and the numbers that you’re getting, you are still beating ev- as far as I can tell, you’re still beating every single clone of you out there.

**Diogo Almeida \[01:15:19\]:** I don’t care about the benchmarks

**Swyx \[01:15:20\]:** Exactly

**Diogo Almeida \[01:15:20\]:** Just to be clear.

**Swyx \[01:15:21\]:** Exactly.

**Diogo Almeida \[01:15:21\]:** So like, even if we were winning or losing, I want to do announcements.

**Swyx \[01:15:24\]:** You’ve established the category, right?

**Diogo Almeida \[01:15:26\]:** Yep.

**Swyx \[01:15:26\]:** Yeah.

**Diogo Almeida \[01:15:26\]:** Yep.

**Swyx \[01:15:26\]:** But al- but also I think this nuance between decision models and System 1, I think is actually the thing that you’re trying to

**Diogo Almeida \[01:15:32\]:** Yes. And I just wanna make software engineers super powered

**Swyx \[01:15:36\]:** Yeah

**Diogo Almeida \[01:15:36\]:** Right? L- like with AI. Like, and or, like, the tragic thing to me is,

**Diogo Almeida \[01:15:42\]:** In that AI winter direction, I think, like, it’s, it’s, it’s just so sad that AI was so powerful yet so underutilized. Like, It’s a thing that gets me emotional, but man.

**Diogo Almeida \[01:16:03\]:** Like, I think that is. I don’t want to, like, just be, like, pure techno optimist, like all technology is good. I think what was happening now was, like, a travesty. Like, it’s. And like, there’s. I just want to, like, open up those possibilities for people.

**Diogo Almeida \[01:16:19\]:** Yeah, I’ll just end it there. I’ve, I’ve cried too much these last few days To want to do it on the record.

**Swyx \[01:16:26\]:** Yeah. No,

**Diogo Almeida \[01:16:27\]:** Yeah

**Swyx \[01:16:27\]:** I appreciate you sharing a little bit of that, and I think people can see that you’re very authentic and

**Diogo Almeida \[01:16:31\]:** Yeah

**Swyx \[01:16:31\]:** Passionate about this. Th- y- you don’t necessarily get that from the name, like, TypeSafe AI, but like, I think once people immerse themself, themselves enough in, like, here’s the genuinely different direction you want the world to go And like, actually you have done, like, the hard part about going from zero to one on the, on the thing, then, like, now let’s all go to- go together in, like, the new direction, right?

**Diogo Almeida \[01:16:51\]:** Yeah. Yeah.

**Swyx \[01:16:52\]:** Yeah.

**Diogo Almeida \[01:16:52\]:** I don’t n I am sure that I won’t think. Maybe I will think that the hard part was done, perhaps. I think that there’s going to be many more hard parts. Like, if, All sorts of stuff gets automated and we finally see GDP growth and like, it’s like, a Jev party

**Swyx \[01:17:12\]:** Yeah

**Diogo Almeida \[01:17:12\]:** Every day, then maybe the hard part is done. But like, I don’t s- think so. And like, I really think that people focus too much on speed and cost and not enough on reliability.

**Swyx \[01:17:23\]:** Okay.

**Diogo Almeida \[01:17:23\]:** Like, reliability is what makes it delightful. Like, reliability is what, like, allows you to trust it.

**Swyx \[01:17:28\]:** You have this line,

**Diogo Almeida \[01:17:29\]:** Yeah.

**Swyx \[01:17:30\]:** TFP growth beating 3% in five years.

**Diogo Almeida \[01:17:32\]:** Hell yeah.

**Swyx \[01:17:32\]:** I’ve never seen

**Diogo Almeida \[01:17:33\]:** Hell yeah. Let’s fucking go.

**Swyx \[01:17:35\]:** I’ve never seen

**Diogo Almeida \[01:17:36\]:** Yeah

**Swyx \[01:17:36\]:** A lab care about TFP growth.

**Diogo Almeida \[01:17:38\]:** But like, that is what an economic revolution is, right? Like, it’s actually extremely consistent with what the OpenAI charter used to stand for.

**Swyx \[01:17:45\]:** Yeah.

**Diogo Almeida \[01:17:45\]:** It was talking about, like. I think the charter is the same, but they’ve kind of tried to move definitions around to, like 100 billion in profit or something like that. Not that I hate an OpenAI.

**Swyx \[01:17:54\]:** It wasn’t like a. Yeah, it wasn’t a well-defined term what AGI is, right?

**Diogo Almeida \[01:17:57\]:** They tried to do it

**Swyx \[01:17:58\]:** No, yeah

**Diogo Almeida \[01:17:58\]:** Right? Like, doing majority of the world’s economically valuable work, and they should have to answer the question, how can it do millennium prize problems in math and zero of the world’s economically valuable work, around the air. Like, I think that all models are roughly tied right now at zero. There’s some chance that, like, we have started already, but like, I would guess that it’s not yet 1%. And I think that will show up in. Like, when it does happen, it will show up in the economic statistics.

**Diogo Almeida \[01:18:28\]:** It’s gonna be fucking awesome. It will not cause mass unemployment, but it will cause, like, a whole bunch of awesome shifts, and wor- the world will be a lot better. And also, like, I’m really tired of AI always being the foreground character, of things. Like, I think that- the world should just be more delightful, and AI should just help with that.

**Diogo Almeida \[01:18:48\]:** And I

**Swyx \[01:18:49\]:** Just, like, disappear into the background.

**Diogo Almeida \[01:18:50\]:** Exactly.

**Swyx \[01:18:51\]:** Yeah

**Diogo Almeida \[01:18:51\]:** Like, the-- I say this in my talks. Like, how can it be that 2019 software, like software, SaaS, whatever, super-duper valuable, right? It’s 2026 now. How is the software basically exactly the same, despite AI being so freaking awesome, other than sometimes having a chat box on the side, right? That, like, that kind of works, but doesn’t allow you to make decisions that the companies have stakes in, because they can’t be trusted to make decisions. That, to me, is nuts. Like, there’s so much economic incentive for this, and I think it’s going to be, like a, like an inverse SaaS-pocalypse. I think SaaS is going to be supercharged by this. They are the ones who are, like, most in the know of what things are valuable to automate, and it’s gonna be, like, a crazy time.

**Swyx \[01:19:37\]:** Yeah. I think, I think so too. It’s a, it’s a beautiful thing that you’ve unlocked?

**Diogo Almeida \[01:19:41\]:** Yeah.

**Swyx \[01:19:41\]:** You mentioned one thing here, which I don’t know if it’s, like, directly here, which is, what is a System 1 problem and what is not. What is a System 2 problem? Like,

**Diogo Almeida \[01:19:50\]:** Fuck. That’s a hard one. That’s a hard one, my friend.

**Swyx \[01:19:55\]:** ‘Cause people now are just trying to Jev everything, right?

**Swyx \[01:19:57\]:** Which, like, probably is gonna fail, right? But like, some things are gonna be good.

**Diogo Almeida \[01:20:02\]:** Jev everything is pretty funny.

**Swyx \[01:20:04\]:** Yeah.

**Diogo Almeida \[01:20:04\]:** It’s a pretty funny way of doing it, saying it. The. So I’ll tell you the truth.

**Swyx \[01:20:08\]:** Yeah.

**Diogo Almeida \[01:20:09\]:** The truth is that this is an empirical problem, just like scaling laws are an empirical thing. Like, why doesn’t, like, robotics really work right now, despite all the money being spent on it?

**Diogo Almeida \[01:20:21\]:** I don’t think it’s about, like, spending more money necessarily. The empirical results just might not be there, right? So empirically, I believe that these, like, pre-trained super condensations of intelligence are fundamentally System 1 thinkers. I think that they truly. Like, System 1 is the closest thing to describe what LLMs are strong at. RLVR has done incredible things for System 2 thinking. I am at awe. It is super freaking cool. Like, I don’t think that it’s going to result in AI doom in the slightest. Not 0%, of course, ‘cause I think 0% is miscalibrated. But like, i- it’s, it’s really cool what they’ve done, and they’ve really pushed it to the limits. Well, maybe they don’t think so not the limits. But like, it is, it is a weird thing for models to do, and they are very fragile at this. Like, think about how people used to talk about AI back in the ChatGPT days. Like, “Wow, it’s really general. It can do a lot of general things.” And then. But it’s bad at math problems and like, GSM8K, grade school math. And then now look at how people talk about RLVR. “It’s so fragile. It’s so jagged.”? Like, it can. “Why can it do this, like, really weird thing?” And actually, math is not just spiky, it’s fractal, right? And this is because RLVR is. Like, if we talk about, like, what is the North Star for each thing? RLHF is please humans, right? That is what the human feedback is. RLVR is optimize benchmarks. That l- everything that goes into the RLVR category literally is a benchmark by definition, because a benchmark is programmatically verifiable, simple outputs that can, like, do well. And RLCD is make it reliable for, programmatic use. And **Diogo Almeida \[01:22:09\]:** Yeah. That, I’ll

**Swyx \[01:22:11\]:** Yeah. Yeah, it’s, this. Maybe I’ll, I’ll offer some thoughts, and then you can sort of,

**Diogo Almeida \[01:22:15\]:** Ooh

**Swyx \[01:22:15\]:** Correct me if I’m wrong. One. For example, one thing that I’ve been thinking about is also. I, so I threw Jev at a bunch of things when you gave me access

**Diogo Almeida \[01:22:23\]:** Ooh, yeah

**Swyx \[01:22:23\]:** On day one. And multi-hop reasoning, right? Like

**Diogo Almeida \[01:22:27\]:** Yep

**Swyx \[01:22:27\]:** So single hop, fantastic. Like

**Diogo Almeida \[01:22:29\]:** Yeah

**Swyx \[01:22:29\]:** State of the art. You should never use anything other than Jev for single hop.

**Diogo Almeida \[01:22:32\]:** Yep.

**Swyx \[01:22:33\]:** Multi-hop is gonna. It starts to falls down. And it’s

**Diogo Almeida \[01:22:34\]:** Yeah

**Swyx \[01:22:34\]:** Like, kind of monotonically increasing as you increase the hops.

**Diogo Almeida \[01:22:38\]:** Yep.

**Swyx \[01:22:38\]:** Right?

**Diogo Almeida \[01:22:39\]:** So oh, yes. Back to that empirical question, it depends on what we can, like, pull out of the models.

**Swyx \[01:22:44\]:** Yeah.

**Diogo Almeida \[01:22:44\]:** Right? So we want everything. Like, we want to unearth as much intelligence as possible, period. The models. Like, I see us as, like, unlocking and smoothing and sculpting the intelligence while, like, adding new capabilities and like, filling in gaps in it. And we will be filling in, like, more and more and more and more of these gaps over time. But the reality is that we are in the business of unearthing properties. Those properties are actually a function of what is available from, like, these, like, these condensed cores and like, Frankensteining them all together to have all of the properties of everything.

**Swyx \[01:23:21\]:** Yeah.

**Diogo Almeida \[01:23:21\]:** ? But the reality is we are in the business of unearthing as many capabilities as pos- as possible. And System 1 just happens to be the description of what works. And everything that works in that paradigm will be System 1-ish. Like, I’m.

**Diogo Almeida \[01:23:38\]:** Like, there is a reason why we don’t do what’s called latent reasoning in strings.

**Swyx \[01:23:43\]:** Yeah.

**Diogo Almeida \[01:23:43\]:** I think the reasoning. Like, the. What models do really well is reasoning within the models. It’s not totally complete. It doesn’t do great on all

**Swyx \[01:23:49\]:** Wait, latent reasoning is reasoning in strings? I thought latent reasoning is reasoning in, inside the model weights.

**Swyx \[01:23:55\]:** The

**Diogo Almeida \[01:23:55\]:** I think that

**Swyx \[01:23:56\]:** I don’t know

**Diogo Almeida \[01:23:56\]:** People used to call that

**Swyx \[01:23:57\]:** I just want to clarify

**Diogo Almeida \[01:23:57\]:** Continuous reasoning.

**Swyx \[01:23:58\]:** Okay.

**Diogo Almeida \[01:23:58\]:** I’m not entirely sure.

**Swyx \[01:23:59\]:** Okay.

**Diogo Almeida \[01:24:00\]:** It was called latent reasoning because, like, it used to be that the reasoning traces were secret.

**Diogo Almeida \[01:24:04\]:** So they’re kind of like a latent variable for the answer.

**Swyx \[01:24:06\]:** Ha.

**Diogo Almeida \[01:24:07\]:** Yeah.

**Swyx \[01:24:07\]:** So what’s secret has now shifted.

**Diogo Almeida \[01:24:09\]:** Well

**Swyx \[01:24:10\]:** Yeah

**Diogo Almeida \[01:24:10\]:** It’s, it’s still secret for OpenAI and Anthropic, right?

**Swyx \[01:24:12\]:** So no reasoning Jev

**Diogo Almeida \[01:24:14\]:** Yes

**Swyx \[01:24:14\]:** As far as you will ever do it, right? Because that, like, violates the whole promise of System 1.

**Diogo Almeida \[01:24:21\]:** I. My promise is to do whatever necessary For machine-native stuff.

**Swyx \[01:24:28\]:** Yeah.

**Diogo Almeida \[01:24:28\]:** I could imagine there are. Like, there are some forms of reasoning that are less slow, inefficient, and fragile that I. That are, like, totally on the cards, just to be clear. So pragmatic person, I’m not making promises on, like, methods. I’m making promises on, like, the. What my ROI North Star is, and I’m going to fight for that, like this launch didn’t happen and we are still, like, hungry for our place in the world.

**Swyx \[01:24:53\]:** That’s great. Yeah.

**Diogo Almeida \[01:24:54\]:** Yeah.

**Swyx \[01:24:54\]:** I think the other thing that, Vision is another one that’s, like, a big, like, capability that you don’t have, but maybe it doesn’t ever belong in System 1?

**Diogo Almeida \[01:25:04\]:** I think I have a pretty good vision.

**Swyx \[01:25:05\]:** What, sorry?

**Diogo Almeida \[01:25:06\]:** I think I have a good vision.

**Swyx \[01:25:07\]:** No, sorry. Vision

**Diogo Almeida \[01:25:08\]:** I am kidding. I’m kidding. Yeah.

**Swyx \[01:25:09\]:** Oh my God.

**Diogo Almeida \[01:25:10\]:** Yeah.

**Swyx \[01:25:12\]:** Because people obviously, the first thing they want is vision ‘cause of the Doom demo, but also just, like, everything, other than text is vision.

**Diogo Almeida \[01:25:19\]:** Everything is in the cards

**Swyx \[01:25:21\]:** Yeah

**Diogo Almeida \[01:25:21\]:** In my mind.

**Swyx \[01:25:22\]:** Okay.

**Diogo Almeida \[01:25:22\]:** Like, and actually, this is, like, a debate we have. This. Man, your audience is probably, like, the great one to have in this debate. There’s a question about, like, how much do we try to, like, give people what they think they want, which is what we did in stealth for two years. We just knew that this is obviously going to be valuable, versus give them what they say they want, right? And like, there’s a lot of dimensions of this, right? And Like, context length is an example of this, right? Every single model, including ours, I actually think as far as I can tell, ours is, like, by far the best

**Swyx \[01:25:58\]:** The longest context, yeah

**Diogo Almeida \[01:25:58\]:** At not degrading in long context.

**Swyx \[01:26:01\]:** Yeah.

**Diogo Almeida \[01:26:01\]:** But like, the other providers are just like, “Whatever people want, let’s just give them the stupid thing.” And like, we need to figure out a balance for this because, like, if you take the former side too far, give people what they want, you end up with, like, anthropic nanny state style thinking, which is very, like, anti-developer. While, like, the pro-developer route would be like, give them what they want, but developers are. Like, we don’t want to put the burden on them to figure out the je ne sais quoi of intelligence. So we are trying to, like, figure out this navigation of, like, how quickly to release things, to still, like, have our, like, brand of trust and also, like, teach our-- treat our users like adults that can make informed decisions that, don’t need, like, nanny stating on top of this stuff.

**Swyx \[01:26:46\]:** Yeah. I think that’s fair.

**Diogo Almeida \[01:26:48\]:** Yeah. And we don’t know the answer, to be honest. Like, we’ll, we’ll have to figure it out. It’s gonna be. That’s probably going to be, like, one of my biggest debates over the next couple of days

**Swyx \[01:26:57\]:** Yeah

**Diogo Almeida \[01:26:57\]:** Because, like, we have a lot of stuff. Again, we didn’t expect it to pop off, so we were like, “We’ll need some follow-up launches.” But yeah.

**Swyx \[01:27:05\]:** I don’t know, I don’t know if you didn’t expect it to pop off. Like, I, you p- I saw the work that you put in. Like, I have never seen you lock in so hard as, like, the last two months basically, right? Like

**Diogo Almeida \[01:27:13\]:** Well, that’s also because my chief of staff made me lock in.

**Diogo Almeida \[01:27:18\]:** Yeah.

**Swyx \[01:27:18\]:** So

**Diogo Almeida \[01:27:20\]:** Like, it’s like, I have never. I thought I worked hard before

**Swyx \[01:27:24\]:** Yeah

**Diogo Almeida \[01:27:24\]:** And

**Swyx \[01:27:25\]:** No, but like, you were showing up at our writing workshops, and I was like, “what are you doing here?” And like, oh

**Diogo Almeida \[01:27:30\]:** It was useful. It was great.

**Swyx \[01:27:31\]:** You clearly, like, were very intentional about your launch.

**Diogo Almeida \[01:27:34\]:** Yep.

**Swyx \[01:27:35\]:** And the work showed, and like

**Diogo Almeida \[01:27:36\]:** Yeah

**Swyx \[01:27:36\]:** Congrats. Like, you got a kudos.

**Diogo Almeida \[01:27:37\]:** Thank you, thank you. I hope to keep locking in

**Swyx \[01:27:41\]:** Yeah

**Diogo Almeida \[01:27:41\]:** Is my, is my sense.

**Swyx \[01:27:42\]:** Yeah.

**Diogo Almeida \[01:27:42\]:** I want to. Like, w- like, I think that we’ve passed many great filters for the tech world, what we’re wanting, but like, there’s still gonna be a bunch more.

**Swyx \[01:27:53\]:** Yeah.

**Diogo Almeida \[01:27:53\]:** And like, holy smokes, am I excited to fight the good fight.

**Swyx \[01:27:56\]:** Yeah, it’s exciting. Before we broaden out to, like, topics outside of TypeSafe

**Diogo Almeida \[01:28:01\]:** Ooh

**Swyx \[01:28:01\]:** I just wanted to offer, any other things that you think, like, underrated or misunderstood about what you have launched.

**Diogo Almeida \[01:28:09\]:** Underrated or misunderstood?

**Swyx \[01:28:11\]:** Yeah. You have pan outs. Sorry, patterns here. Maybe you wanna go into that. Model jaggedness, anything.

**Diogo Almeida \[01:28:20\]:** Give me one

**Swyx \[01:28:21\]:** Yeah

**Diogo Almeida \[01:28:21\]:** Noodling of it. Oh, man. I would rant about all of these. I really shouldn’t. I really shouldn’t.

**Swyx \[01:28:29\]:** Okay. And like, people can come, go to your Discord if they

**Diogo Almeida \[01:28:32\]:** Yeah. People put a lot of love into the cookbooks

**Swyx \[01:28:34\]:** Yeah

**Diogo Almeida \[01:28:35\]:** Is what I will say. The cookbooks have, like, some fire stuff. We had considered putting a bunch of these things, like, in the main launch blog post, but it got kind of long and unwieldy and like, very power usery. But like, we really. I’ll be frank. Like, before the launch, every. Like, what we’re saying sounds, like, sounds like this weird alien tool. Why would anyone need this? It was a very weird thing. We were very worried about teaching people about, like, this new frontier. It obviously succeeded, but like, we put a lot of work because we thought the education would be, like, a gigantic bottleneck for us. I.

**Diogo Almeida \[01:29:14\]:** It probably works, and it’s no lo- probably no longer a problem because people are doing things, like, well beyond what we could ever expect.

**Swyx \[01:29:20\]:** They’ll show you, like, how to use your model.

**Diogo Almeida \[01:29:21\]:** Yeah, but. Exactly. But like, they. Yeah, and their use cases are, like, kinda cooler than ours. Like Like, there’s a bunch of stuff where I’m like, “Man, if that was our demo, holy shit, that was way cooler than what we were showing.” like, the computer use stuff, holy smokes is it cool. But like, we put a lot of love into this. This is not, like, AI-generated trash, as far as I know.

**Swyx \[01:29:41\]:** Yeah.

**Diogo Almeida \[01:29:41\]:** We put a lo- l- like, it’s, like, a lot of love in here.

**Swyx \[01:29:45\]:** Yeah. Fair enough.

**Diogo Almeida \[01:29:45\]:** And like, each of these are. Like, there’s real alpha there.

**Swyx \[01:29:49\]:** Okay.

**Diogo Almeida \[01:29:49\]:** Like, these are inspired by solving real customer problems that existed, and we went through the work of, like, helping them do cool-ass stuff.

**Swyx \[01:29:58\]:** Yeah. How much. While you’re talking about this, right, how much validation did you do before launch? Like, what. What was that process like?

**Diogo Almeida \[01:30:06\]:** What was that process like?

**Swyx \[01:30:08\]:** Like, clearly you did some, but obviously you’re not getting in touch with as many people as you are today.

**Diogo Almeida \[01:30:14\]:** Yes, of course.

**Swyx \[01:30:15\]:** But like

**Diogo Almeida \[01:30:15\]:** I actually think that the reception was pretty bad. And like, actually for the non-technical people in the team, they were really worried.

**Swyx \[01:30:24\]:** Yeah.

**Diogo Almeida \[01:30:24\]:** Like, there was a lot of fear. It’s like, no one really gets this. And like, they don’t want it. We’re, like, selling, like, a vitamin and not, like, a painkiller. Like, should we have FDEs to, like, write the software around solving that problem?

**Swyx \[01:30:37\]:** Yeah.

**Diogo Almeida \[01:30:38\]:** We had almost no revenue before launch. It was kind of like. Like, we. Like, the technical people were, like, obviously true believers, right? Like, we knew that this was sick. Its prop- computational properties are, like, off the charts on, like, so many axes that we’re like, “Yeah, obviously it’s gonna be huge.” I was definitely super afraid, which is why I locked in super hard. But like, the most common thing was- The, like, I would say, like, more than half the people we had play with it just did not get it. And like, the people who did, like, were like, “Man, this is really cool, but how do we get this through procurement and stuff like that?” it was like, it was like quite a, quite a battle, and we just knew like, okay, the-- our target market is gonna be developers. People will find the use cases, and that way everyone is gonna FOMO in. And like,

**Diogo Almeida \[01:31:32\]:** I don’t want to rub in people, like, changing their minds with the facts changing.

**Diogo Almeida \[01:31:36\]:** I do want to call into question, like, the concept of product market fit? Yeah, but like, because, like, there was a product, there was a market. Like, we were like, “Hey, do you want to use this?” And people are like, “I don’t know, really know if it solves our problems.” It explodes and everyone’s like, “We need as much rate limits as we can. Can we literally give you GPUs? Because we are constrained right now.”

**Swyx \[01:31:59\]:** Yeah.

**Diogo Almeida \[01:31:59\]:** So of course, like, marketing is an element of it, of course, but I don’t even think it’s about marketing. I think it’s about, like, passionate developers who’ve, like, our souls basically resonated at the same frequently, and that frequency, and that got everyone else excited too.

**Swyx \[01:32:14\]:** Yeah.

**Diogo Almeida \[01:32:14\]:** And I’m hoping as well that, like, we as a company will be eternally grateful to those developers. Like, not-- and not just like, the companies that, like, are-- like, start off with developers and like, go big enterprises.

**Swyx \[01:32:30\]:** They go to market, yeah.

**Diogo Almeida \[01:32:31\]:** Exactly. And like, I’m, like, even thinking about, like, how can we launch things that are better for. Oh, man, I don’t know if I should say this.

**Diogo Almeida \[01:32:39\]:** But I will.

**Swyx \[01:32:39\]:** Better for developers than enterprises.

**Diogo Almeida \[01:32:41\]:** Exactly.

**Swyx \[01:32:41\]:** Okay.

**Diogo Almeida \[01:32:41\]:** How do we do that? Like, how do we empower them? And I have cooks. I have cooks. But it’s a, it’s a very weird thing to do. And like, I don’t know how else I can show my thanks and loyalty to that. Like, and that’s why I did, like, the dying my hair yesterday. It was like It’s like I wanted to talk to them ‘cause it felt dirty to me during our company’s, like, most important times not to keep talking to them.

**Swyx \[01:33:07\]:** Good. Well, that’s why is your hair.

**Diogo Almeida \[01:33:09\]:** Yeah. Hold me to that, please.

**Swyx \[01:33:11\]:** Yeah. We will, we will.

**Diogo Almeida \[01:33:11\]:** I try to be principled.

**Swyx \[01:33:13\]:** Yeah.

**Diogo Almeida \[01:33:13\]:** Quote me on this. Call me out. D- have the pitchforks out if I change.

**Swyx \[01:33:18\]:** I was just gonna briefly show the computer use stuff.

**Diogo Almeida \[01:33:21\]:** Whoa.

**Swyx \[01:33:21\]:** Is this, is this what you’re referencing?

**Diogo Almeida \[01:33:22\]:** I’ve never seen-- I haven’t-- I’ve seen. I saw, like, a airline browser use thing.

**Diogo Almeida \[01:33:29\]:** And inside this new note, let’s make the title say hello.

**Diogo Almeida \[01:33:33\]:** Wow. Great. Okay. Let’s move on and can you open up the Arc browser? And once you’re there, can you Google search Norbert Wiener?

**Diogo Almeida \[01:33:43\]:** Now can you open up x.com?

**Swyx \[01:33:46\]:** Is this kind of use case?

**Diogo Almeida \[01:33:48\]:** Oh, the voice use cases. This is actually the first one I’ve seen. This is

**Swyx \[01:33:51\]:** Oh, okay.

**Diogo Almeida \[01:33:51\]:** Open up the photo viewer. Wow. Oh, wait. Oh, can you bo- can you go back a second? Can you go back a second?

**Diogo Almeida \[01:33:58\]:** Rumors claim Anthropic engineers worth- worship Claude as God. Wow. Wow.

**Diogo Almeida \[01:34:03\]:** Dang. That’s pretty funny.

**Swyx \[01:34:06\]:** And here you are building prod.

**Diogo Almeida \[01:34:07\]:** Abso-- Wow, this is sick.

**Swyx \[01:34:12\]:** Yeah. So clearly it can operate the whole computer with voice, with Jev as the decision model.

**Diogo Almeida \[01:34:17\]:** So Just like I’m anti-benchmarking, I’m also anti-demos. I want to make sure that it works reliably. I love people are playing with it. This is super fucking sick, have no doubt. I want to see this. I wanna see it be used. I want our team to play with it. I wanna find the weaknesses, and I wanna solve that.

**Swyx \[01:34:35\]:** Yeah.

**Diogo Almeida \[01:34:35\]:** And I would lo-- Man, that looked really cool.

**Diogo Almeida \[01:34:37\]:** That looked really cool. I want that. I want that. Like, when my, when my wrists are sore, I, like, just whisper flow everything. That would be sick.

**Swyx \[01:34:44\]:** Well, as, well, just to round out the use cases side

**Diogo Almeida \[01:34:47\]:** Yeah

**Swyx \[01:34:47\]:** ‘cause I do have to let you go. Who’s, who are the, who are the bigger companies that have reached out and have surprised you with what they wanna do?

**Swyx \[01:34:56\]:** Just.

**Diogo Almeida \[01:34:57\]:** I am so out of touch for that.

**Swyx \[01:34:59\]:** Okay.

**Diogo Almeida \[01:34:59\]:** People have shown me screenshots of companies, and from what I’ve seen, it’s all of them.

**Swyx \[01:35:05\]:** Yeah. Mostly, like, for those people who work at larger companies and they’re not doing this kind of work, I just wanna give people examples of, like, you should go look that up, look that up, look that up.

**Diogo Almeida \[01:35:13\]:** Oh. So like, I think demos are super-duper sick. Obviously, the coding agents are, like, gigantic use cases.

**Diogo Almeida \[01:35:21\]:** Like, they are, like, also super sick.

**Swyx \[01:35:23\]:** Oh, Cog is all about Jev right now.

**Diogo Almeida \[01:35:25\]:** Oh, hell yeah. Oh, can I, can I give a little bit of a tangent about coding agents, if that’s or

**Swyx \[01:35:29\]:** Yes, please.

**Diogo Almeida \[01:35:30\]:** Oh, let me

**Swyx \[01:35:31\]:** We love coding agents here.

**Diogo Almeida \[01:35:32\]:** Give me a second. Give me a second. Okay, actually, I’ll come back to coding agents. Let me describe, like, the big families of use cases.

**Swyx \[01:35:37\]:** Yes.

**Diogo Almeida \[01:35:38\]:** Like, we’ve mapped this out from first principles, like, long before release. They are what we call dark data. Like, people hoarded big data, but they would not throw a LM at it ‘cause it was too expensive. So large companies adore this. They have, like, piles of data that they wish they could analyze, and this is like a data scientist’s wet dream. So this is like. This is a giant one. Like, I think this plus, coding agents are the m- big moneymakers because that’s what they’re all. Where all the volume is, right? There’s the real-time stuff. Like people who need, like, intelligence in the loop. They. Like, I would guess that every CEO, if not CTO, at those companies, knows how much better their product gets with every, like, 10 milliseconds shaved.

**Swyx \[01:36:23\]:** Yes.

**Diogo Almeida \[01:36:23\]:** And like

**Swyx \[01:36:24\]:** Especially e-commerce, yeah.

**Diogo Almeida \[01:36:25\]:** Yeah. Oh, or, like, assistant-y things. There’s many AI assistant-y things. And like, as far as I can tell, they really love it. Again, I’m not in the front lines of customers right now, so I just get. Know what my team tells me. But like, this. I’m so excited for this. I’m really excited for this for games. I really wanna play, like, sick-ass auto-battlers where you’re, like, commanding your team, or, like, semi-auto battlers. I think that’d be so cool. But don’t make it too good while I still have a job. And the. Like, there’s the. What we call, like, verify everything. Like, verifying all LLM calls, kind of like observability. I think, actually on the note of docs, what people should be doing is, like, the parallel questions are very cheap. So if you have, like, big states you wanna ask many questions on

**Swyx \[01:37:08\]:** This right here, yeah.

**Diogo Almeida \[01:37:09\]:** Put IDs on every, like, message, and then ask a question about each ID. So like, when you have, like, a long state.

**Swyx \[01:37:16\]:** Huh.

**Diogo Almeida \[01:37:16\]:** So that way you can, like, pay for the state once and ask lots and lots of questions about each message within it. I think that is, like, a, like, a great way that, like, saves money and is.

**Swyx \[01:37:25\]:** Which, by the way, I always think, like, it’s interesting framing System 1 and System 2 because it basically makes the case that you should always make one or 10 or 100 Jev calls for every one reasoning call that you make.

**Diogo Almeida \[01:37:37\]:** Well, maybe. May

**Swyx \[01:37:38\]:** Right.

**Diogo Almeida \[01:37:38\]:** Well, I don’t. I would like people to spend less.

**Swyx \[01:37:42\]:** Yeah.

**Diogo Almeida \[01:37:42\]:** Maybe you do, like, one half the reasoning calls and like, 10 Jev calls each or something like that, or whatever solves the problem that, like, couldn’t have existed otherwise. Wait, number four use case was what I described as, like, smart software. Like, software that’s intrinsically composable and like, does, like, weird, fun stuff that could never happen before. Like the programming language as Jev thing. I don’t know if you’ve seen that. That is so cool. Man, if we knew how to give out credits, because, like, we’re really early in our infra days, I would wanna give all these projects credits.

**Diogo Almeida \[01:38:16\]:** And I think that those are, like, how we’ve mapped out, like, the main use cases. Computer use has also come in kind of like the real-time direction as well, and like, that’s really cool. If it is reliable, I am super jazzed about that. I suspect we can make the model a lot better at these use cases ‘cause, like, that came out of left field a little bit, so that’s, that’s really cool. On the coding agent thing, and this is, like, a really surprising thing that is happening right now.

**Swyx \[01:38:47\]:** Okay.

**Diogo Almeida \[01:38:49\]:** Claude Code and Codex are, I believe, the winner, like, the number one and two. I’m not entirely sure. I don’t follow closely

**Swyx \[01:38:56\]:** Roughly

**Diogo Almeida \[01:38:56\]:** But like, it’s roughly that. But they’re built around a single model world? Like, and that makes a lot of sense for them, right? Because, like, it has been a one-model game where it’s, like, kind of like the same model but different intelligence that you’re shopping.

**Diogo Almeida \[01:39:09\]:** But all the open coding agents are, like, fucking jazzed right now because they’re, like, getting their Jev on. And Like, the thing is, there’s-- I’m sure they’re trying a lot of weird stuff But all the coding agents are kind of roughly at, like, approximate parity, right? Because, like, there’s not so much you can do with a while loop. But the moment one person finds one killer use case that, you can only do with that coding agent, everyone will flock to it because they have, like, a monopoly on that thing. But all the open coding agents will be able to copy that, right? The end. But I don’t know what the Claude Codes and Codexes will do because they are built around that one model world.

**Swyx \[01:39:48\]:** Single model, yeah.

**Diogo Almeida \[01:39:48\]:** And like, I think that’s gonna be, like, a really interesting thing. Like, I would love to be able to integrate with them personally. Like, I want to integrate with everyone. Like, I-- they might make competitors eventually. I don’t know. But like, it is not me-- my job as Songfire Infrastructure to be opinionated on that, right? Like, I want to just serve the world. But I don’t know if they would do that. And like, I think it’ll make the coding agent game super weird. Like, I’m so excited for that. And like, I’m sure. I’m getting my team to review right now an internal document I made on design patterns I suspect will be useful for coding agents. So hopefully I can share it, like, right after I walk home. But like, I think that there’s just, like, such ripe area for exploration out in the world. And like, it’s, it’s. Man, if I did not have this, I would love to experiment with coding agents right now.

**Swyx \[01:40:41\]:** Yeah. And I’m sure the coding agent companies would love to work with you as well to figure that out. Yeah, I do think that there’s still use cases for Claude Code and Codex with you guys

**Diogo Almeida \[01:40:49\]:** Of course

**Swyx \[01:40:49\]:** Which, it’s, it’s easy to explore there. Okay. We’ve-- you’ve, you’ve been very, obliging in the sort of indulging in all these, all these things. I just wanna take you out of TypeSafe

**Diogo Almeida \[01:41:00\]:** Oh, yeah

**Swyx \[01:41:00\]:** Just generally about. And you’ve, you’ve made very clear your position on the state of AI. Give you more room on the alignment safety side of things.

**Diogo Almeida \[01:41:08\]:** Oh, did I not talk about safety alignment at all?

**Swyx \[01:41:11\]:** Oh. Oh, you did, you did.

**Diogo Almeida \[01:41:12\]:** I think I didn’t. I think maybe I didn’t.

**Swyx \[01:41:13\]:** You did.

**Diogo Almeida \[01:41:14\]:** Oh.

**Swyx \[01:41:14\]:** I just, like, I think that there’s, there’s a lot of, You have a lot of researcher discussions. We have this every

**Diogo Almeida \[01:41:20\]:** Of course

**Swyx \[01:41:21\]:** Every NeurIPS.

**Diogo Almeida \[01:41:22\]:** Yeah.

**Swyx \[01:41:22\]:** What are people talking about? Like, I. So for example, I, recently was at, one of these researcher gatherings, and people are genuinely worried about the pacing, right? Like, this whole topic about, like, we should slow down because The public is, like, clearly not ready. And I’m sure you have strong feelings.

**Diogo Almeida \[01:41:47\]:** I feel like this is the kind of thing that is a dangerous

**Swyx \[01:41:51\]:** Okay

**Diogo Almeida \[01:41:51\]:** Topic to talk about. I’m happy to talk about it. I live for danger.

**Swyx \[01:41:55\]:** All right.

**Diogo Almeida \[01:41:56\]:** Our company brand is chaos. It’s not Jev. It is irreverence and chaos.

**Swyx \[01:42:00\]:** And yeah. And like, you were at OpenAI during, like, the. One of the very first, like, very visible incidents, which is the blip, right? Like, which

**Diogo Almeida \[01:42:09\]:** Oh, the

**Swyx \[01:42:09\]:** Which, like. And like, you. The dominoes have gone down now To now every Frontier lab has co-signed a document saying that they wanna pace.

**Diogo Almeida \[01:42:18\]:** Interesting. I. So comp-- it’s a very complicated, nuanced thing. I actually do want to write a response to this more formally. I do have, like, a little bit of a short version of my response

**Swyx \[01:42:31\]:** Yeah

**Diogo Almeida \[01:42:31\]:** Which is that, as you RLVR more, like, RLVR is like.

**Diogo Almeida \[01:42:38\]:** So RLVR is not actually about verifiable rewards. Like, that has been failing since before the reasoning revolution. Like. And that’s the weird part about tasks, right? Like, back when. Oh, fun history. Back when RLHF was becoming a thing, there were three different things that, like, are now called post-training, different efforts. And instruction following was by far the, like, the vaster child. Like, people didn’t like it. They didn’t want to take it into account. It was annoying. Like, I talked to the pre-training team. I’m like, “Guys, this is the magic.” And they’re like, “We run so many model sweeps. You want us to wait for human evals to figure out which models to use?” And like, everyone is, like, giving tons of, like, resources to, like, the code gen team, which, like, they did s- have some successes, but they were trying really hard to do RL on co- like, unit tests. And it didn’t work, obviously, right? Like, you needed reasoning for that. So re- so just to be clear, RLVR is not purely about the reward. It’s about, like, the shape of everything too. And part of it is that reasoning is included in here, like this latent variable that you’re doing things. And when you’re doing things, you’re just letting the models do whatever they want in order to make them be as powerful as you can to answer the hardest problems. And this whole pace the frontier discussion, I think is, like, a very narrow focus because it assumes that everyone needs to do more RLVR, right? Which, like, I obviously don’t think I need to do more RLVR on our models.

**Diogo Almeida \[01:44:10\]:** I think zero is the optimal amount for our shape. Hey, right? Like, come on.

**Swyx \[01:44:14\]:** Yeah.

**Diogo Almeida \[01:44:14\]:** . So It’s really, I think, a bit of a sleight of hand where they are saying that we actually want to keep doing the thing that looks dangerous because it does dangerous things. Like people say, like, “Oh, maybe the sandboxing was a problem,” or whatever else. Yeah, obviously it is, and they could have easily solved that, right? But they chose not to because the more things you let the models do in this do anything category, the more powerful it is, right? So like, there-- I think there’s some, like, disillusion of responsibility there on, like, things that by design or non-design they’re trying to make is just an assumption. We must do RLVR, and not just we must do it, we must do more and more and more, with giving the models, like, the power to do powerful-- do anything they want in the middle ‘cause that teaches them to be powerful outside of it. And we don’t want to limit those things well because it’ll make it slightly less powerful on those things.

**Diogo Almeida \[01:45:24\]:** So like, if you assume all of that, they’re like, “Oh, yeah

**Swyx \[01:45:28\]:** That’s a logical conclusion

**Diogo Almeida \[01:45:29\]:** We’re heading to a dangerous world, guys.”

**Swyx \[01:45:31\]:** Right.

**Diogo Almeida \[01:45:31\]:** Like, “Everyone is gonna be doing this, and this is the only way to make AI sick.” So **Swyx \[01:45:37\]:** So basically it’s like, it’s like, it-- these are all internally consistent, but actually starts from a premise that has alternatives if you

**Diogo Almeida \[01:45:45\]:** Of course

**Swyx \[01:45:45\]:** Think about it.

**Diogo Almeida \[01:45:45\]:** Of course. I think there’s-- Like, on the bittersweet lesson direction, I think that there’s very few people who’ve, like, made right tasks. Like new directions of AI. That is-- Or new North Stars. That is rare. Again, like, I think 2.2 times or something for LLMs itself, like RLHF and then RLCD.

**Swyx \[01:46:04\]:** Oh.

**Diogo Almeida \[01:46:04\]:** RLVR is like a 0.2, in my opinion, and I think that’s generous.

**Diogo Almeida \[01:46:09\]:** But or 0.5 or, like, it could be one whole one. I don’t really care. But I do think that people are thinking very close-mindedly about this type of thing. And this-- the only people who are at fault here are the researchers because it’s definitely not the populace. Like, they just assume that OpenAI and Anthropic are just doing the best they can, and they are not the experts who are aware of the true optionality available.

**Swyx \[01:46:34\]:** Yeah. And that’s fair. And like

**Diogo Almeida \[01:46:36\]:** Yeah

**Swyx \[01:46:36\]:** You’re, you’re also doing your part in waking them up.

**Diogo Almeida \[01:46:38\]:** Yes. Okay. Well, I’m doing my best, but like, my goal is not, like, convince labs that there’s, like, other directions

**Swyx \[01:46:43\]:** Yeah

**Diogo Almeida \[01:46:44\]:** To go down. My goal is have-- it’s like spark hope in software engineers to start, like, actually automating things they’ve always wanted automated. I had this, like, article that I wrote that my team didn’t let me write, that didn’t let me publish about, like, the future I want of AI. And like, there’s, like, a lot of, like, little things. Like, remember do what? Imagine if everything could do what ‘cause, like, that demo was do what.

**Diogo Almeida \[01:47:10\]:** Like, you could. Like, there’s levels

**Swyx \[01:47:11\]:** Yeah, don’t do what I say.

**Diogo Almeida \[01:47:12\]:** ?

**Swyx \[01:47:12\]:** Yeah. Don’t do what I say, do what.

**Diogo Almeida \[01:47:14\]:** Yeah. And like, we couldn’t do what yet because, like, computers are so basic and literal, but that computer use one was just that. And I think that there’s, like, levels of smoothness that’ll happen in the world that people just don’t understand. And like, the promise of, like, smarts all around are. It’s, it’s, it’s-- I don’t wanna overpromise. I don’t think it’s going to happen right now, but like, we are gonna do whatever the fuck we can to make that happen.

**Swyx \[01:47:40\]:** Yeah. Any other things on the sort of general shape of post-training? You obviously you have been very intimately involved. Mid-training, is that, something that you do have comments on? I don’t think we’ve ever talked about it.

**Diogo Almeida \[01:47:53\]:** Mid-training. It’s all a spectrum.

**Swyx \[01:47:57\]:** Yeah.

**Diogo Almeida \[01:47:57\]:** Right? Like, am I

**Swyx \[01:48:00\]:** This is like curriculum, but like, fancier.

**Diogo Almeida \[01:48:02\]:** Yeah. Like, it’s, it’s, it’s like, it’s a cost-saving thing.

**Swyx \[01:48:06\]:** Yeah.

**Diogo Almeida \[01:48:06\]:** Instead of, like, having to pre-train again. Like, there’s intriguing stuff. I actually think that, like, intelligence has a je ne sais quoi at every single level, and it’s always super-duper fascinating. Like, I’m a shape rotator, so I don’t like finding that, but I love it when people find it and teach me about it. But looking at the data, this thing that, our data team is so good at that I’m not.

**Diogo Almeida \[01:48:31\]:** It’s-- I find it really fascinating. I love actually thinking about, like, how capabilities are, like, put into the model, like, over, like, the short term. Like, there’s, like, the really rapid alignment of fine-tuning and over the long term. After seeing it over and over and over again, like, this stuff gets baked deeper and deeper and deeper and deeper into the model until it gets robust. And that is, like, the North Star to surface, and like, the System 1 stuff is the stuff that ends up getting robust. So I find mid-training to be, like, a fascinating thing. I’m a fan of all forms of training. I’m a fan of all forms of, like, surfacing new types of intelligence. I wouldn’t do it all myself because it’s expensive. And I have said privately, and also.

**Diogo Almeida \[01:49:20\]:** Should I say this? Huh. Huh. Like, my philosophy is anything I sh- I should say in, like, private with, like, an investor, I should say in public with the people because that is, like

**Swyx \[01:49:32\]:** Power to the people

**Diogo Almeida \[01:49:32\]:** My thing.

**Swyx \[01:49:33\]:** Yeah.

**Diogo Almeida \[01:49:33\]:** Yes. So m- the thing I’ve said i- before is if you gave me a billion dollars, I wouldn’t pre-train. I still believe that to be true. It is a very expensive thing when. If you are, like. Like, if you’re an AI engineer, you can, like, slice and dice and do all sorts of stuff. Like, Frankensteining is not the most elegant, beautiful thing, but it solves problems, baby.

**Diogo Almeida \[01:49:57\]:** So - Anything except pre-training.

**Swyx \[01:50:00\]:** Yeah. Amazing. I think one direction that I do think that is interesting, just, like, synthesizing all your, all your commentary about these model things is, like, do we have a super model, that has all these capabilities involved, or do we break them out, in further? I guess sort of, like, one way to put this is that OpenAI was trending in the direction of the omni model Right? 4o was one of those. Then for a brief period of time, there was always, like, there was, like, a kind of a main branch of the-- this is the chat-tuned model and this is the coding-tuned model.

**Diogo Almeida \[01:50:35\]:** Those are completely different things. Those are extremely different concepts. I will, like, break that down a little bit. So multimodality is a little bit different

**Diogo Almeida \[01:50:44\]:** Because sometimes the other modalities help, sometimes they hurt.

**Swyx \[01:50:47\]:** Yes.

**Diogo Almeida \[01:50:48\]:** Like, people are moving. They seem to be moving away from speech, which is different than audio, because it seems to not generalize well to the other stuff.

**Diogo Almeida \[01:50:57\]:** This might get solved. I’m a fan of all of this, but these are, like, empirical, real questions. Like, scaling laws are not about just throw money at it and it gets good. Scaling laws are pragmatically how good is a thing? Like, there are worlds where, like, no matter what you scale, it may not be good enough. So Y- like, computer use is not currently solved is my understanding. Like, I’m hoping that we can be a. Like, play a part in solving that. But like, it. There might be no amount of data we collect that will solve that. We might need better methods or something else like that. So **Diogo Almeida \[01:51:33\]:** Like, we. You need to be, like, really practical in all of this. Am I a fan of omni models? I’m a fan of all forms of intelligence, but I will go straight into one thing you talked about, which is different from pre-training, which is post-training ‘cause I hate fracturing intelligence. That is, like, the bad thing to me. And this whole, like, chat-first reasoning mode is because, it forces the intelligence to be fractured. Like, when you’re optimizing for chat, this tends to be, like, pure RLHF, and it’s quite intrinsic in RLHF to do the stuff people, like, naturally complain about, right? Like, oh, I’m gonna

**Swyx \[01:52:07\]:** You’re absolutely right. And

**Diogo Almeida \[01:52:08\]:** Yeah

**Swyx \[01:52:08\]:** .

**Diogo Almeida \[01:52:09\]:** Sycophancy, psychophancy

**Swyx \[01:52:10\]:** Yeah

**Diogo Almeida \[01:52:11\]:** I d- whatever word

**Swyx \[01:52:11\]:** Yeah

**Diogo Almeida \[01:52:11\]:** How- or how to pronounce that. Overconfidence, hallucination. Like, even the kind of style that excels in LM Arena, bold, italicized, emojis? Like, it doesn’t answer the question simply. It gives, like, a long write-up, and then it asks you a follow-up question so it feels more like a human talking to you. All of these things, come because strings are super weird? They are, like, weird-ass things, and you need to be miscalibrated. You need to, like, mode drop. You need to be hyper-confident in order to not go off the rails ‘cause the reward model will punish you so hard when that happens ‘cause it’s obvious. Y- and then this, like, warps the probability space entirely, and it interacts with that of the reasoning models, right? Because, like, it. The models are, like, these simple linear things that tend to cheat a bit. So I think that’s very different than exposing intelligence is my guess. And a lot of the art to intelligence is studying this subtlety that I think that, at least when I was in OpenAI, people were not really studying that because, like, they were just like, “Chat,” just like people are on with Jev right now.

**Swyx \[01:53:19\]:** Yeah, you give me an ultimate. You give me an objective, I will just all go optimize for that, right? Like, and it

**Diogo Almeida \[01:53:23\]:** Yes. But if you try in there. And like, the saying is, like, you could have, like, two objectives and you could just, like, optimize for both, but then that is literally the act of fracturing, right? So yeah.

**Swyx \[01:53:33\]:** So in some ways, you ha- you are also fracturing intelligence into System 1, System 2, but you just don’t agree with the other people’s fractur- fracturing, which is fine.

**Diogo Almeida \[01:53:41\]:** Oh, it

**Swyx \[01:53:41\]:** Which is fine.

**Diogo Almeida \[01:53:42\]:** It’s a little different. No. If I could, if I could add, if I could defend

**Swyx \[01:53:46\]:** Yeah

**Diogo Almeida \[01:53:46\]:** The System 2 tasks, number one, like, we don’t toss out the System 2 tasks, right? Like, you can try to make Jev work on it, and there actually is an intelligent answer for that, which is unknown. Like, my. Like, there. Like, there is better and worse behavior in the System 2 tasks, which should be, like, really low confidence, lots of uncertainty. Maybe some heuristics can, like, move the needle here and there, but we care about them too, just to be clear. I just think that is not what the. What is. The intelligence is native to. So we’re not trying to fracture anything like that. And all fracturing makes the model dumb. Like, if people, like, get the model to say, like, it is OpenAI or Qwen or, d- like, Claude or whatever else, I don’t really know what it says this d- these days. I am not going to put into the models that you are Jev from TypeSafe. That fractures it, right? Like, it. L- like, I don’t want that. Like, represent what do the internet thinks, right? Like, be correct. That is what I want because that’s how you get the smooth, predictable intelligence.

**Swyx \[01:54:50\]:** I, identity is a thing, I guess, that is

**Diogo Almeida \[01:54:53\]:** I- for, it- for

**Swyx \[01:54:54\]:** A somewhat of a special

**Diogo Almeida \[01:54:55\]:** For a first-party product, yes.

**Swyx \[01:54:56\]:** Yeah.

**Diogo Almeida \[01:54:56\]:** But like, for an API, I don’t think so.

**Swyx \[01:54:58\]:** Yeah. Okay.

**Diogo Almeida \[01:54:59\]:** ?

**Swyx \[01:54:59\]:** Yeah, that’s good.

**Diogo Almeida \[01:54:59\]:** Like, I don’t. I. Like, people don’t want. If they’re making a chatbot with, ChatGPT, they don’t want it to say it’s ChatGPT. They wanna say it’s, like, Chipout AI or whatever, right?

**Swyx \[01:55:09\]:** Well, so the way that you also have to make up for it is you have the skill, right? The

**Diogo Almeida \[01:55:13\]:** Yeah

**Swyx \[01:55:13\]:** The Jev skill, which is for coding agents to work with Jev.

**Diogo Almeida \[01:55:16\]:** Yeah.

**Swyx \[01:55:16\]:** Okay, a couple closing questions

**Diogo Almeida \[01:55:19\]:** Hell yeah

**Swyx \[01:55:19\]:** Because I do want to, get you out. One is, like, is just reflecting on your two-year journey. It’s roughly two years? Two point something?

**Diogo Almeida \[01:55:25\]:** With the company

**Swyx \[01:55:26\]:** Yeah

**Diogo Almeida \[01:55:26\]:** I think that this is, like, more like a four-year journey.

**Swyx \[01:55:29\]:** Yeah.

**Diogo Almeida \[01:55:29\]:** But

**Swyx \[01:55:29\]:** Well, yeah. Actually, like, I was thinking, remembering that, like, you had this, like, hero run around Thanksgiving. You were like. You were canceling everything because, you were like, “Guys, like, everyone’s on holiday. I’m gonna take all the open edge GPUs and go do this thing.”

**Diogo Almeida \[01:55:42\]:** Yeah. That was a good time.

**Swyx \[01:55:44\]:** And that was, like, the pre-TypeSafe

**Diogo Almeida \[01:55:46\]:** Yeah

**Swyx \[01:55:46\]:** Moment, right?

**Diogo Almeida \[01:55:47\]:** I. That might have been. Was that when the coup was happening? I don’t really know.

**Swyx \[01:55:50\]:** Yes, actually.

**Diogo Almeida \[01:55:51\]:** Yeah. That sounds right. Yeah. I remember. Oh my God, I don’t wanna. I’m not. I don’t think I have the time to spill the tea about the coup right now, but That was really annoying.

**Swyx \[01:56:04\]:** The coup was annoying or the run was annoying?

**Diogo Almeida \[01:56:06\]:** The coup was annoying.

**Swyx \[01:56:07\]:** The coup. Okay.

**Diogo Almeida \[01:56:07\]:** Yeah.

**Swyx \[01:56:08\]:** Yeah.

**Diogo Almeida \[01:56:09\]:** It. I will

**Swyx \[01:56:10\]:** Safia’s took over the company. Yeah, anyway.

**Diogo Almeida \[01:56:14\]:** Maybe next time we chat

**Swyx \[01:56:16\]:** Okay. All right, all right

**Diogo Almeida \[01:56:16\]:** I’ll, I’ll dump tea about. A tea about the coup. Yeah. It actually, this problem was one that, like, was in my mind since before ChatGPT even launched. I was like, “Holy shit, the ChatGPT team is cooking. They are doing the right task.” They are doing the thing that AI researchers are bad at, but successful product people are good at, which is giving a lot of fucks about the experience. It’s, it’s, it’s very rare. They. Like, there’s very few people like that at OpenAI. And those guys were cooking on it really well.

**Swyx \[01:56:50\]:** And to be clear, this is the whole journey from GPT-3 to 3.5, which included AI Dungeon, which you’ve talked about

**Diogo Almeida \[01:56:55\]:** Yeah

**Swyx \[01:56:55\]:** As like. Yeah. Well, that’s, that’s an example of a use case that we never predicted.

**Diogo Almeida \[01:56:59\]:** Yes, exactly.

**Swyx \[01:56:59\]:** That’s right.

**Diogo Almeida \[01:57:00\]:** Well, Oh, yeah, that is a. Also, I had fought very hard to deploy InstructGPT.

**Diogo Almeida \[01:57:07\]:** Like, actually the early versions of it were even trained with, like, an algorithm we didn’t publish that I made myself because it was too slow to clean the PPO data. And I was like, “Fuck it. This is so fucking good. We need to get it in the hands of users.”

**Diogo Almeida \[01:57:21\]:** And like, basically immediately it took 50% of the market share of LLMs at the time. And but. And we thought it. I made. I went through great effort to make sure everything in our launch video is true. We. I truly was thinking like, “Is this AGI because it’s superhuman at instruction, in instruction out?” You. Obviously, it’s not, but like, everyone I think should have an answer to why that was not AGI, ‘cause it looks very smart. And my answer to that ended up, like, ended up only being used for copywriting. Jasper AI, Copy.ai, like writing, like, what is now called slop on web pages. And we were worried we made the internet a worse place, right? And I went back to the drawing board, and I was like, “What’s missing? We are smart, clearly. Something is missing from it, like, creating value. What is it?” Like, I actually was doing more philosophy at the time of like, “What is going on?” And the answer was, “Oh, machines.” the question I asked myself is like, “Let’s work backwards from an AI-based economic revolution. When that happens, what will c- be. What’ll be calling the AI if AI is an API? Will it be humans or it’ll be code?” And I figured it was many nines of code. And but like, all the optimization was going into the humans part. And then it clicked for me. I’m like, “Holy shit, this is the North Star.” I think, like, I wrote a document. I was, like, talking to Sam about this. Sam was like, “This is so fucking good. You should go work on it.” And we’re like, “Yeah, Sam, I have a job.” like, it. I was working on

**Swyx \[01:58:51\]:** Sam just told you to do it. Dude, go do it.

**Diogo Almeida \[01:58:54\]:** But like, my guess at the time is like, this is super obvious. Like, it’s so unbelievably obvious. Anthropic must be working on this already? And like, we’re already cooked and like, actually OpenAI does better at, like, catching up than it does at, like, actually innovating. So like, ChatGPT was a copy of Claude, right? Like, they had an internal thing. They just didn’t ship it.

**Swyx \[01:59:13\]:** Yes. Yeah. Claude and Slack. But reasoning, I would say first-ish.

**Diogo Almeida \[01:59:17\]:** Yeah.

**Swyx \[01:59:18\]:** Yeah.

**Diogo Almeida \[01:59:18\]:** But debatable how good of a product that is.

**Swyx \[01:59:21\]:** Yeah.

**Diogo Almeida \[01:59:21\]:** Great research though. Super great research. I’m just not sure if people had that product need. And Claude did the coding agent stuff too. So Sam says that, and I just go back to my job for a while. Eventually, like, the instruction following team just says, “We won. We’ve solved instruction following. We don’t need to do stuff anymore.” I’m, like, trying to think about what I do next. I was like, “ maybe I’ll just, like, start playing around with this.” I, do more philosophy and design and thinking. I thought it would end up taking a week, when I started training models. It ended up taking,

**Diogo Almeida \[01:59:58\]:** Many years. At some point I was like, “Holy shit, there’s signs of life here.” This. It obviously didn’t work, right? Otherwise, we would have deployed it. But like, I wanna explore what it would be like research-wise to go all in on this. Like, I wanna really see, like, what it would be like if you went, like, absolutely insanely all in this direction. And because of what I said, like, if an AI winter happened, would I. How would I feel? I would consider myself personally responsible. I talked to other companies at the time, and I was like, “Hey, I want to start a lab on this direction.” And like, there was interest, and I just talked to them like, “How fast. What would be faster? This or a startup?” And they’re like, “Startup.” And I’m like, “Fuck it, man. We ball.”

**Swyx \[02:00:44\]:** Yeah.

**Diogo Almeida \[02:00:44\]:** “I guess we’re doing some crazy shit.” And

**Swyx \[02:00:47\]:** And you called Eric and Sasha and

**Diogo Almeida \[02:00:48\]:** Yeah. Well, I call Eric first. With Sasha, I actually didn’t try to recruit her. I tried to be good, and I was just like, “Hey, am I crazy? Is something missing here? Isn’t there, like, am I too much in the OpenAI bubble that I didn’t realize there must be a solution to this?” And then Sasha was like, “I’m in.” And I’m like, “Sasha, you’re working at a startup.” And she’s like, “I’m folding it right now.” And I’m like, “Do you wanna think about that?” She’s like, “Oh, yeah. Good point. Let me think about it.” And then she joined.

**Swyx \[02:01:19\]:** Yeah.

**Diogo Almeida \[02:01:19\]:** And then, within two weeks we had funding. We di- we had, like, people move into my apartment. It was the worst ‘cause I’m a neat freak. And we just kept on cooking, and eventually we got the research that,

**Diogo Almeida \[02:01:34\]:** That showed the signs of life?

**Swyx \[02:01:37\]:** Yeah.

**Diogo Almeida \[02:01:37\]:** It was, it was a crazy time.

**Swyx \[02:01:38\]:** So the qu- the question is. That was all long context.

**Diogo Almeida \[02:01:41\]:** Oh, yeah.

**Swyx \[02:01:41\]:** And then now the question is, someone like you

**Diogo Almeida \[02:01:43\]:** Yeah

**Swyx \[02:01:43\]:** Is in the Frontier lab right now who is frustrated not getting the funding or the resources, whatever, the attention. What’s your advice to them? Do. Should they do what you did?

**Diogo Almeida \[02:01:54\]:** Should they do it. Ooh, that’s a fascinating question.

**Diogo Almeida \[02:02:03\]:** Ooh, man. How do I do this without burning bridges?

**Diogo Almeida \[02:02:08\]:** I-- My sense is that most n-- unless there’s some level of economics I don’t really understand, I think most neo labs are crap. I don’t want to see myself with that as peers. Like, I don’t really understand what’s going on there. Like, is it becau-- Like, number one, I don’t really value researchers. I value people who. Like, look at my bitterness lesson, right?

**Swyx \[02:02:33\]:** The data, the task.

**Diogo Almeida \[02:02:34\]:** I want. Well, not just that.

**Swyx \[02:02:35\]:** Yeah.

**Diogo Almeida \[02:02:35\]:** I, like, we need researchers, but we need them to give a lot of fucks about the right task, and that’s the important thing, right? So it’s actually, like, the. It’s, it’s kind of backwards when people value pure research pedigree ‘cause that generally doesn’t create value. So it. Like, number one, I believe in North Star tasks and doing cool, really useful stuff. Number two, because I don’t value researchers, I don’t, I don’t recommend going the. Well, it clearly is profitable for someone, or it might be in this environment. So like, from a purely pragmatic perspective, I don’t see creating neo labs as want- something that creates value. It seems to destroy value because, like, they are, like, redoing work from scratch with, like, low probability of actually moving the frontier. And as far as I’ve talked to most neo labs, they don’t really have a direction. They tend to want money to play around with their experiments. If they have a direction, I’m super in favor of it, to be clear. So my advice for someone is it really depends on why you’re doing it? If you are a researcher who wants to play around with research, probably the labs are the best place to do that, TBH. Like, there might be other places. I don’t really keep track of that politics, but I would just recommend not being that way, personally? Like, I think it’s better for the world with people being driven to solve real problems. And those problems may be exploratory. That’s fine. But like, ideally have principles that you stand behind. But if you think that you wanna do the right task, like, abso-fucking-lutely. Like, please do. Like, please break this, like, uni-mind, unimodal, like

**Swyx \[02:04:21\]:** Hive mind.

**Diogo Almeida \[02:04:21\]:** Yeah, exactly. Like, ev- like, again, this pacing the frontier is coming from, like, this one view of AI that looks like, AI super genius that is incredibly jagged, and that is,

**Swyx \[02:04:36\]:** Solvable.

**Diogo Almeida \[02:04:37\]:** It’s solvable, and it’s weird, and it’s, like, not matching reality. And It’s like. It’s tragic, right? Like, I think, like, all of these. Like, the. Like, really unearthing technology I think is, like, just good.

**Swyx \[02:04:51\]:** Yeah. For what it’s worth, again, I’m trying to repre- accurately represent the position of the, Anthropic OpenAI folks I was talking to, SpaceX as well, by the way, is that, it is. This is a political thing much more so than a pure Xris thing.

**Diogo Almeida \[02:05:05\]:** Yep.

**Swyx \[02:05:06\]:** So yeah. Political positioning is

**Diogo Almeida \[02:05:08\]:** And that. And that’s beyond my pay grade.

**Swyx \[02:05:10\]:** Exactly, yeah.

**Diogo Almeida \[02:05:10\]:** That’s well beyond my pay grade.

**Swyx \[02:05:12\]:** Once they, once they told me that, I was like, “I get it. This is about the 2028, election.”

**Diogo Almeida \[02:05:18\]:** Oh, no.

**Swyx \[02:05:19\]:** Yeah.

**Diogo Almeida \[02:05:19\]:** Oh, I wish I didn’t hear that. That’s such a bad vibe.

**Diogo Almeida \[02:05:22\]:** And so

**Swyx \[02:05:23\]:** No. This is not the whole company.

**Diogo Almeida \[02:05:24\]:** Yeah.

**Swyx \[02:05:24\]:** This is just that room’s discussion.

**Diogo Almeida \[02:05:26\]:** No. That makes sense.

**Swyx \[02:05:28\]:** Yeah. Yeah.

**Diogo Almeida \[02:05:28\]:** That makes me lose faith in humanity a bit, but maybe I’m just a naive technologist.

**Swyx \[02:05:33\]:** It’s really starting to matter

**Diogo Almeida \[02:05:35\]:** Yeah

**Swyx \[02:05:35\]:** Who’s, who’s in charge of the governments, that will help to regulate, these things as they emerge. And like, as a lab

**Diogo Almeida \[02:05:41\]:** I tot

**Swyx \[02:05:41\]:** You should probably think that through.

**Diogo Almeida \[02:05:43\]:** No. No. I totally agree with that, to be clear. Like, I think being opinionated on that matters a lot. I personally am afraid of trying to mislead people because I think that bites people in the ass a lot? Like, I think that, like, people trying to be overconfident, like, I obviously just. I’m not actually gonna talk about politics. I think what happened in COVID is, like, people leaned too much in, like, appeals to authority and being overconfident to try to get people to behave in certain ways. And like, obviously our response was extremely suboptimal, and that had, like, ripples of downstream ramifications that are now, I think, extremely bad for the world. Like, maybe I’m naive. I think that misleading people, even for the greater good or what they think is the greater good, is just, it’s just. I’m not a fan.

**Swyx \[02:06:41\]:** Yeah.

**Diogo Almeida \[02:06:41\]:** I’d r- I’d rather not do it.

**Swyx \[02:06:42\]:** For what it’s worth, I. It’s not a. I don’t think it’s misleading. It is just like, this is why now.

**Diogo Almeida \[02:06:46\]:** Yeah.

**Swyx \[02:06:46\]:** Why. Yeah. W- like, w-?

**Diogo Almeida \[02:06:49\]:** The. I think that the thing

**Swyx \[02:06:50\]:** Like, Dario Rodas said in May, like, “Fuck are we doing now?”

**Diogo Almeida \[02:06:52\]:** I think that is why now that is a little bit, misleading about, like, the risks versus, like, the objective. It. There is, like

**Swyx \[02:06:58\]:** Yeah

**Diogo Almeida \[02:06:59\]:** Some level of, like, sneakiness latent in it

**Swyx \[02:07:02\]:** Yeah

**Diogo Almeida \[02:07:02\]:** That, is worth calling out and I think owning up to. Well, obviously they want to. If they want to manipulate, then they shouldn’t own up to that. That seems like a bad strategy.

**Swyx \[02:07:11\]:** No.

**Diogo Almeida \[02:07:11\]:** But like, that to me is just sad for the world.

**Swyx \[02:07:14\]:** Yeah.

**Diogo Almeida \[02:07:15\]:** Hopefully I’m never. I’ve. Yeah. Hopefully, like, we are never involved in anything

**Swyx \[02:07:21\]:** Yeah

**Diogo Almeida \[02:07:21\]:** Like that. It might be inevitable as we get big, but I want to. I wanna stay, like, pure technologist to my roots as much as I can.

**Swyx \[02:07:29\]:** Jev for president. Why not? I can. I. I would trust Jev’s decisions over, my own. Okay, so less shitposting, more about

**Diogo Almeida \[02:07:39\]:** Less shitposting.

**Swyx \[02:07:40\]:** No. For me.

**Diogo Almeida \[02:07:41\]:** You’re just kind

**Swyx \[02:07:42\]:** I’m shit- I’m shitposting.

**Diogo Almeida \[02:07:42\]:** Oh, you’re just crushing my hopes

**Swyx \[02:07:43\]:** No. I’m not gonna be shitposting

**Diogo Almeida \[02:07:44\]:** About, like, American in the world right now.

**Diogo Almeida \[02:07:46\]:** Oh my lord.

**Swyx \[02:07:47\]:** Yeah. Like, there’s. I kind of. I think I watch too much TV about, like, conspiracies to think about the presidency.

**Diogo Almeida \[02:07:52\]:** Oh, no.

**Swyx \[02:07:52\]:** The, You have chosen your North Star. You have chosen reliability. You’re in a programmable and composable AI.

**Diogo Almeida \[02:07:59\]:** And cheap.

**Swyx \[02:07:59\]:** And cheap.

**Diogo Almeida \[02:08:00\]:** Yeah.

**Swyx \[02:08:00\]:** What is a second or third one that you wanna throw as a bone to someone else that you’re not. That you want someone else to work on that you’re not gonna work on?

**Diogo Almeida \[02:08:06\]:** Ooh.

**Swyx \[02:08:07\]:** Like, just basically give people tasks.

**Diogo Almeida \[02:08:10\]:** Give people tasks?

**Swyx \[02:08:11\]:** Yeah, like, that your task

**Diogo Almeida \[02:08:12\]:** Oh, there’s so many I want. Oh, what?

**Swyx \[02:08:13\]:** You have picked your tasks, right? What?

**Diogo Almeida \[02:08:15\]:** What? Wait, I. That’s such a good question. Holy crap. Oh, man, I’m so excited by that.

**Swyx \[02:08:19\]:** ‘Cause you’re, you’re gonna be, you’ll be for the next, like, 50 years, you’re gonna be busy doing your thing.

**Diogo Almeida \[02:08:23\]:** Hell yeah. Okay, so let me give, like, a fun one and a not fun. L- and like, maybe a valuable one that’s also fun.

**Diogo Almeida \[02:08:32\]:** My fun one is I think games could be so freaking cool if they were intelligent. Like, when I see people play around with, like, Ali’s Doom demo, where, like, you can, like, get NPCs to control stuff, like, Like, that was just really, like, the. Like, a proof of concept. I think really cool stuff could be made. It looks really cool. Like, I’m a big Stardew Valley fan? And like, it’s, it’s really static, and it’s still compelling. Like, I feel like there’s a lot of cool story that could happen. You don’t need to call, like, Jev in the game loop. It’s probably too expensive for that. But even, like, simple, like, state machines for NPCs, I think you could make, like, such a compelling world. Oh, man.

**Diogo Almeida \[02:09:13\]:** And man, a little sad that I can’t work on these types of things.

**Swyx \[02:09:17\]:** Yeah.

**Diogo Almeida \[02:09:18\]:** My life path is a little bit set right now, and I’m,

**Swyx \[02:09:22\]:** Yeah, but you can call someone else to work on it

**Diogo Almeida \[02:09:23\]:** Yeah. That’s cool

**Swyx \[02:09:23\]:** And then you can, like, feedback on it.

**Diogo Almeida \[02:09:25\]:** And the thing that I would really like to explore is, like, coding agents free from the tyranny of the KV cache. Like, it might not be as good as true coding agents are, but I think there’s just so many weird things to think about. Th- that’s why I wrote the article KV cache Rules Everything Around Me.

**Diogo Almeida \[02:09:46\]:** Believe it or not, I don’t think anyone has used the phrase on the internet “cache rules everything around me,” C-A-C-H-E, when I, when I Googled it.

**Swyx \[02:09:57\]:** Okay

**Diogo Almeida \[02:09:57\]:** So like, I wrote this ‘cause I wanted to tell people about, like, this is how coding agents w- agents work and how the KV cache works and everything. And I think. I don’t know. Yeah.

**Diogo Almeida \[02:10:13\]:** Like, it explains a lot of stuff, like why routing is really hard, why sub-agents don’t seem to work, like, why compaction is such a hard problem. And I’m going to try to release a document. My team might veto me because, believe it or not, I’m not in charge.

**Diogo Almeida \[02:10:29\]:** But I wish. But I want to release a document of like, “Here are my thoughts. Please play with it, and please figure out all the ways that we can do things with coding agents, like, once you’re freed from that KV cache tyranny.”

**Swyx \[02:10:46\]:** Which is it locks you in and

**Diogo Almeida \[02:10:48\]:** Well, not. It lo- it locks you in into one model, right? And in order to do it efficiently, you need to, like, keep on appending to it. So now you’re not doing best software practices, like state management, abstraction, decomposition. Why can’t you give an easier task some. Yeah, why can’t you give a sub-agent an easier task? Because of the state that you’re passing around. Oh, I touched this. Because of the state you’re passing around, you nee- would need intelligence that is way cheaper than the intelligence using to read this in order to pass this state around. Why can’t you be smart about it, right? And I think there’s just, like, c- tons of really cool, fun research to be had there on, like, different programming patterns. Kind of like how people are playing around, like, with, like, recursive language models. Like, I feel like there’s, like, just lots of cool stuff in here when you think about, like, “Oh, I want to explicitly label the state of everything.” Or imagine you have, like, a sub-task. Like, coding agents, I think it’s fair to say they work on sub-tasks at a time, as from a decomposition perspective. Why do you need to pass all of that state back into the parent task?

**Swyx \[02:11:49\]:** Yeah.

**Diogo Almeida \[02:11:50\]:** Why couldn’t you do smart things about it? And also, if you had a hierarchy of labeled sub-tasks, why can’t you do a search through that sub-task tree for the relevant context when you need it in, right? And then, another thing that you can do. Oh, man, I forgot to write something about this. I have, like, some cooks in here that are really cool. Hope to publish it. I’m down to jam about it, but like, it’s gonna be a long document. And like, if that becomes the case where context becomes cheap, like, why can’t you do cool patterns, like looking at your historical context very cheaply? Is it kind of weird that you start from scratch every time and you need to solve a problem called continuous learning? That’s a, that’s actually like a memory management problem because you don’t have a smart way of looking up the memory, right? But what if you could? What if you could do that all the time? Or what if when you have parallel sub-agents, they can, like, read each other’s states because you have all of that in, like, your computer memory, and you can be smart about what’s reading and writing at the same time, and your coding agent swarm or whatever has, like, locks around things and can coordinate intelligently, not with, like, basic-ass locks. Like, “What are you doing? What am I doing?” “Jev, who should write first?” Blah. And like, I feel like the future there is

**Swyx \[02:13:04\]:** Oh my God

**Diogo Almeida \[02:13:05\]:** Nuts. Yeah.

**Swyx \[02:13:06\]:** Jev to solve locks.

**Diogo Almeida \[02:13:07\]:** It could be so cool for, like, multiple agents working together. Or, like, if you think about state

**Swyx \[02:13:12\]:** Yeah

**Diogo Almeida \[02:13:12\]:** Like, you have

**Swyx \[02:13:12\]:** Agent swarm and stuff.

**Diogo Almeida \[02:13:13\]:** Yeah.

**Swyx \[02:13:13\]:** Yeah.

**Diogo Almeida \[02:13:13\]:** And some things, for example, are read-only processes. Some people like getting, like, summaries of what the agents are doing.

**Swyx \[02:13:20\]:** Yeah.

**Diogo Almeida \[02:13:21\]:** Why can’t they share state easily? Because, like, a read-only agent needs to, like, read parts of the context and figure out what’s relevant to say, well, like, what’s actually being written ‘cause the exploration is not super important, or here is the tree of sub-tasks. I feel like there’s so many different fun things that could be done if, like, a really smart person, like, dedicated, like, a whole lot of time to rethink, like, the coding agent experience, and that would be super-duper sick.

**Swyx \[02:13:46\]:** Yeah.

**Diogo Almeida \[02:13:47\]:** Man, I. That would be my dream.

**Swyx \[02:13:48\]:** I would point you towards PrimeAgent if you haven’t looked at it. So this, works together with the RLM work. We just, talked to Alex, who is a buddy of Ellen’s, in the chair before you.

**Diogo Almeida \[02:13:59\]:** Oh, cool.

**Swyx \[02:14:00\]:** And like, yeah, it is being worked on, but it’s not super popular yet.

**Diogo Almeida \[02:14:04\]:** Yep.

**Swyx \[02:14:04\]:** And if, like, yeah

**Diogo Almeida \[02:14:05\]:** Well, yeah. But the hope. Yeah, I would want everyone to, like, just play around

**Swyx \[02:14:08\]:** Yeah

**Diogo Almeida \[02:14:09\]:** With, like, weird things. I have no guarantees that it’ll work, but it seems really interesting from, like, a technical perspective. So yeah, that seems cool and cool.

**Swyx \[02:14:18\]:** Seems cool.

**Diogo Almeida \[02:14:18\]:** Like, I. Like, once we figure out how to give credits out, I would love to, like, give credits out to people like this.

**Swyx \[02:14:23\]:** Yeah. You will be in a position to fund research, for sure.

**Diogo Almeida \[02:14:26\]:** Yeah.

**Swyx \[02:14:26\]:** No. Anyway, congrats on all your success. You’ve, like, come s- come such a long way since I first met you, like, and the whole team as well.

**Diogo Almeida \[02:14:32\]:** I’d like to think I’m the same person as well.

**Swyx \[02:14:34\]:** Yeah. Yeah. I think. But I think, like, you are energized in a way that I have never seen you before because you found your mission.

**Diogo Almeida \[02:14:40\]:** No. That’s true. That’s definitely true.

**Swyx \[02:14:41\]:** And

**Diogo Almeida \[02:14:42\]:** I was

**Swyx \[02:14:42\]:** You are articulating your mission, because you, for many years you complained about the problems, but you didn’t have a solution yet, right? And you, like, you had, you had the rough shape and that, then you had to do it, put in the work.

**Diogo Almeida \[02:14:55\]:** I will say that is partially because I describe myself as 0% entrepreneurial.

**Diogo Almeida \[02:15:03\]:** I don’t like startups. I never wanted to be a CEO in my life. I can’t imagine anyone doing this twice. It seems horrible. Honestly, doing it once is pretty bad. When we first were fundraising, an investor asked me, like, “Which CEOs do you look up to?” And I was like, “Ew, why would I look up to those people?”

**Diogo Almeida \[02:15:22\]:** No offense to anyone. I’m trying to be, like, I’m trying to be genuine and good. I’ve met, like, a lot of really good people, but like, the famous ones have, like, a lot of, like, skeletons in their closet it seems. And I think I just really did feel disempowered when I was at OpenAI. Like, I felt, Yeah. Like n- it’s, it’s a little bit easier to be truthful now because, like, I have at least some proof that the direction has legs. Like, I just felt like in the insane house where everyone is just like, “ChatGPT, yeah. Like, where do we put ChatGPT in everything? How do we make ChatGPT good for, like, developers and stuff?” And I’m like, “What are you talking about? Like, the function calling interface is insane. Why would you deploy this?” like, this is, this is so anti-developer.

**Swyx \[02:16:04\]:** It’s sort of a hacky way on top of hacks on top of hacks.

**Diogo Almeida \[02:16:07\]:** Well

**Swyx \[02:16:07\]:** Yeah

**Diogo Almeida \[02:16:07\]:** Not just that. Like, the thing I often said was if there was like a, y- l This is also probably tea I don’t have time for right now, but I always used to say, like, “I want to be removed from any project involving, like, function calling if you did not get a logit bias for each function.” Like, so very Very simple ask in my part. Because

**Swyx \[02:16:32\]:** Which is something like a confidence, but not calibrated.

**Diogo Almeida \[02:16:34\]:** Oh, or a probability for it, right?

**Swyx \[02:16:36\]:** Yeah.

**Diogo Almeida \[02:16:36\]:** Like, we need to give users the ability to control, like, let’s say they have actions

**Swyx \[02:16:42\]:** Oh, yeah

**Diogo Almeida \[02:16:42\]:** Or refuse or allow. Yeah, Disney needs to set a different refusal threshold than AI dungeon. The only way to control that with function calling right now is to say, like, “Pretty please.”? That’s nuts. That’s a nuts interface for developers and like, people have been, like, dealing with this for years now, right? Like, they still have that with skills. Like, the existing coding agents are, like, highly overfit to their existing harness ‘cause they’re jagged. They don’t tend to use, like, external, like, tools and MCPs super well because of overfitting, of course. And like, why can’t, like, big companies allow for, like, these slight nudges to be like, “Call this more. It’s really useful.”

**Diogo Almeida \[02:17:22\]:** Right? And like, the solution is begging in a system message. That’s nuts.

**Swyx \[02:17:29\]:** But no, okay. I think I think I get you. And like, man, it is so exciting to talk about all this stuff.

**Diogo Almeida \[02:17:35\]:** Thank you.

**Swyx \[02:17:35\]:** It’s, it’s really cool to get you on the podcast.

**Diogo Almeida \[02:17:37\]:** Yay.

**Swyx \[02:17:38\]:** You’re gonna go, do amazing things, man. Like, I’m excited for your next, big launches, whatever it is.

**Diogo Almeida \[02:17:43\]:** Oh, hell yeah.

**Swyx \[02:17:44\]:** Yeah.

**Diogo Almeida \[02:17:44\]:** Just you wait.

**Swyx \[02:17:45\]:** Yeah.

**Diogo Almeida \[02:17:46\]:** Just you wait. It might be sooner than you think.

**Swyx \[02:17:48\]:** So hiring data people, infra people, I assume, marketer.

**Diogo Almeida \[02:17:51\]:** 100 feel. Depends on who you ask.

**Swyx \[02:17:53\]:** Community person.

**Diogo Almeida \[02:17:54\]:** If you ask me

**Swyx \[02:17:55\]:** Yeah

**Diogo Almeida \[02:17:55\]:** I feel like I’m a pretty good founding marketer. But if you ask anyone on my team, they say, “Shut the fuck up, Diego. You need to do CEO stuff.” So yes, founding marketer

**Swyx \[02:18:03\]:** And it’s not just about spice. Like, I think you’re very spice-oriented, which, like, you, like, that’s Your unique talent. But sometimes you just need to say

**Diogo Almeida \[02:18:10\]:** I know, I know

**Swyx \[02:18:10\]:** Like, yeah.

**Diogo Almeida \[02:18:11\]:** I would really love

**Swyx \[02:18:11\]:** Do team, multi-team things. Yeah.

**Diogo Almeida \[02:18:12\]:** Yes, I. Nothing teaches you delegation like having a tidal wave of stuff to do. Hiring data people, or we call them model capabilities, like, but they are data people, bo- like, data’s kind of a slur in the industry. And like, I want to make sure they

**Swyx \[02:18:28\]:** I don’t think so. We’re very pro-data here.

**Diogo Almeida \[02:18:29\]:** Yeah, but I want them to be the highest status of, like, the people actually working on the model that actually sounds a little weird. I want everyone to have equal status, but like, I want to even that out And I want to know that’s really valuable.

**Swyx \[02:18:40\]:** These are more equal than others.

**Diogo Almeida \[02:18:42\]:** Well. I don’t like weird hierarchies and I think one of the things I’m most proud about in the company is that they don’t respect me that much or they don’t show that. They troll me and like, joke with me and they treat me poorly sometimes and all of that. And I think that’s a good sign of a culture. We’re hiring, like, platform people, like people to, like, build out Jev everywhere. Like, we are so much more sensitive to location because speed of light is more of a bottleneck.

**Diogo Almeida \[02:19:09\]:** Right? Like, I’m so sad for the European users that we were only, like, three times as fast instead of, like, 100 times as fast because, like, we don’t have servers there right now. And like, that’s insane, right? But like

**Swyx \[02:19:20\]:** It’s okay. Life in Europe goes a bit slower as well. It’s okay.

**Diogo Almeida \[02:19:24\]:** Wow, I can’t believe you. You said it, not me. Or everywhere.

**Swyx \[02:19:30\]:** Yeah.

**Diogo Almeida \[02:19:30\]:** Like, if intelligence per second is a metric that matters, like, we’ll launch this all over the place. Like, we care about. Like, if they’re a developer building on top of us, I care a lot about you. And we are hiring for people to keep building more s- l- like, not just. Like, the goal is not to just be, like, Jev as a company. The goal is to, like, ship more shapes of intelligence beyond that. So we are hiring people to, like, build those things too. Like, we want to not just be, like, yeah, like, the one-trick pony of, like, the simple model. But like, I think that there’s gonna be, like, an AWS of, like, intelligence? And

**Swyx \[02:20:07\]:** Which is gonna be you, by the way, right? Yes.

**Diogo Almeida \[02:20:09\]:** Like, that’s a direction I want to go down.

**Swyx \[02:20:11\]:** Yes. Okay.

**Diogo Almeida \[02:20:11\]:** It’d be arrogant to say it will be me.

**Swyx \[02:20:13\]:** Yeah.

**Diogo Almeida \[02:20:13\]:** Like, we. Like, I’m going to do anything I can to make sure that happens.

**Swyx \[02:20:18\]:** Yeah.

**Diogo Almeida \[02:20:18\]:** Like, I think that’s gonna be so cool. Like, we are playing with, like, System 1 intelligence right now. Imagine the layers? Like, this is like the TCP of it.

**Swyx \[02:20:30\]:** Yeah. Several more layers to go.

**Diogo Almeida \[02:20:33\]:** Yeah.

**Swyx \[02:20:33\]:** And who knows what else? I’ve also pitched Temporal, by the way. I don’t know. We need to talk about Temporal as layer eight

**Diogo Almeida \[02:20:38\]:** Ooh

**Swyx \[02:20:39\]:** Out of the seven layers.

**Diogo Almeida \[02:20:40\]:** Ooh.

**Swyx \[02:20:41\]:** But anyway, we can talk forever.

**Diogo Almeida \[02:20:43\]:** Hell yeah.

**Swyx \[02:20:43\]:** You gotta get back to work or sleep.

**Diogo Almeida \[02:20:44\]:** Yep.

**Swyx \[02:20:45\]:** Thank you for coming.

**Diogo Almeida \[02:20:45\]:** Oh, boy. Yeah. Cool. You’re most welcome. It was a pleasure, man.

**Swyx \[02:20:48\]:** Yeah.

**Diogo Almeida \[02:20:49\]:** So excited.

**Swyx \[02:20:50\]:** Yeah.

**Diogo Almeida \[02:20:50\]:** So excited.

**Swyx \[02:20:50\]:** Not the last time.

**Diogo Almeida \[02:20:50\]:** You came the first time. It

**Swyx \[02:20:51\]:** Not the last time.
