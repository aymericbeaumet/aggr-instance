---
title: 'AI Engineer Code Summit: OpenAI on New Ways of Working With Codex'
link: https://latent.space/p/codex-work/
source: latent-space
published: 2026-11-11T04:49:48Z
updated: 2026-11-11T04:49:48Z
first_seen: 2026-11-11T07:00:45.879812165Z
authors:
- Richard MacManus
summary: On the first day of AIE Code Summit, OpenAI showed how developers can steer Codex by voice, hand off work to the cloud, and make use of dots.
content: extracted
html: 2026-11-11-ai-engineer-code-summit-openai-on-new-ways-of-working-with.html
remote_preview:
  url: https://beehiiv-images-production.s3.amazonaws.com/uploads/asset/file/27ce2150-3717-4122-ac3a-37dd0e8ce18f/openai-aiecode-feature.png?t=1794370546
  alt: 'AI Engineer Code Summit: OpenAI on New Ways of Working With Codex'
extra:
  thumbnail: https://beehiiv-images-production.s3.amazonaws.com/uploads/asset/file/27ce2150-3717-4122-ac3a-37dd0e8ce18f/openai-aiecode-feature.png?t=1794370546
---

On the first day of AIE Code Summit, OpenAI showed how developers can steer Codex by voice, hand off work to the cloud, and make use of dots.

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/27ce2150-3717-4122-ac3a-37dd0e8ce18f/openai-aiecode-feature.png?t=1794370481)

[AI Engineer Code Summit](https://ai.engineer/code/2026/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) kicked off today with a series of workshops, including from leading frontier labs OpenAI, Anthropic and Google DeepMind. I attended the OpenAI workshop, intriguingly titled **“New Ways of Working with Codex”** and hosted by two of its Developer Experience engineers, **Dominik Kundel** and **Charlie Guo**.

The past year in coding agents has been one of constant product upgrades by all the frontier labs. But not all of these changes have gone smoothly. The **shift to cloud**, for example, is still confusing to many users of Codex, Claude and Antigravity.

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/0206e434-836e-44fe-943e-53b4d5f846ad/codex-evolving.jpg?t=1794370871)

An illustration of the evolution of Codex, shown at the OpenAI workshop today.

We’ve also seen new ways to work with agents. **Personal agents** like OpenAI’s dots and Anthropic’s Claude Duet can assist between sessions, while **voice mode** gives developers another way to steer coding tasks.

OpenAI’s workshop addressed all three, and I also had some followup questions on these topics for Kundel, after the session.

Guo began the workshop by listing a selection of things they’ve shipped in Codex since the AI Engineer World’s Fair in June 2026. “I had to cull this list in order to make it fit on the screen because **there were too many things to put in here**,” he said. “I think the team has been shipping at an unbelievable rate.”

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/2525f115-d0c8-465c-a3b0-fc3e4ec43db5/image.jpeg?t=1794368819)

## The awkward transition to cloud

[Codex launched in May 2025](https://openai.com/index/introducing-codex/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) as “a cloud-based software engineering agent that can work on many tasks in parallel.” But Kundel admitted that “it took a while for models and harnesses to equalize into something where we can build a really engaging experience.” In other words, **the cloud part of Codex has often been an awkward user experience.**

That began to change after DevDay in late-September, when [a revamped version 2 of Codex Cloud](https://community.openai.com/t/meet-the-all-new-codex-cloud/1402399?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) was announced. Suddenly, cloud usage seemed more viable. As the DevDay announcement put it, you can “finally close your laptop now and your agents will keep working.”

As explained in [the official workshop writeup](https://aie-codex-workshop.openai.chatgpt.site/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) — hosted on ChatGPT Sites — the new version of Codex Cloud lets you “**publish an environment once** and run multiple tasks in isolated instances of that setup.”

“These can be shared within a company if you're working in a company, or you can just create your own,” said Kundel about these **cloud environments**. “You start off by picking the GitHub repositories that you want to have access to.”

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/89edeca1-f8a5-47f1-bd37-c3a927b0c266/codex-cloud.jpg?t=1794370885)

After the workshop, I admitted to Kundel that I still find Codex’s cloud functionality confusing — for instance, when I start a task locally and later want to move it to the cloud.

“We definitely want to have it be easier,” Kundel replied. “Some of it is **going to depend on managing the context differences**. For example, yesterday what I ended up doing is I was working on something locally, and I just asked Codex to go and hand it off to the cloud.”

That **handoff to the cloud** is something OpenAI is trying to improve for users.

To demonstrate, Kundel reopened an existing cloud task on the web and showed me that its Appshot (a new feature enabling you to save an app experience to ChatGPT) was still there. For a task that started locally, he described asking Codex to push a branch and start a new cloud thread.

**Codex Cloud is now as “just a background**, like a different executor of where things are happening,” he continued. “And so in a similar vein, you can actually ask your local Codex to — for example — spin up a task on a cloud Codex. So they're all sort of **merging together**.”

## Voice as user interface

One of the demos Kundel showed during the workshop was a new website for a fictional record company called Blossom Music. Kundel demonstrated live edits to the site and said he sometimes **uses voice dictation when iterating this way**.

“You can really quickly iterate on these things and get to a world that you're actually envisioning with your stuff, especially if you're unsure what you actually want to get to,” he said.

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/f0184a16-db57-4df2-a65f-003379fe085a/codex-vinyl.jpg?t=1794371317)

Later in the workshop, Guo explained that voice is an effective mode to use when **delegating work to different agents**.

“Most people I see that use voice with coding essentially treat it as just a different medium on top of their normal work,” he said. But personally, he likes to **use voice as a control plane**, which he says allows him to control everything he’s doing in a code base.

“I don't think about it \[voice mode\] as saying, here's the one lane of work that I'm doing right now with voice. I think about it as saying, **how do I manage my threads? How do I check in on stuff?** How do I pull data from other apps on the computer? How do I create new threads in the application? Just using my voice.”

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/a4bdeec4-e3c1-4bce-be03-c5f2ff8e5ac9/codex-voice.jpg?t=1794370987)

Guo used a telephone as a prop in this demonstration of voice mode.

After the workshop, I asked Kundel what are some of the ways he is using voice in his work?

He replied that dictation is one way he uses it, but another use case is **to talk to his dot**.

“Since my dot has access to all of my dev boxes and my laptop and my cloud environments, **it also has all the context on what I work on**, what I do,” he told me. “And so I often talk to it on my way to work \[...\] to work through stuff, start up some PRs while I'm driving, so that when I come home or when I come to the office, I can go and review them. So, voice is incredibly powerful.”

## Dots

Speaking of dots, personal agents were only briefly discussed in the workshop. Perhaps that’s because, as Guo admitted, they’re not entirely intuitive to developers.

**“As a Codex power user, dots feel a little bit awkward to work with,”** he said. “I think because they're a new paradigm and because they're good at different things than the main Codex.”

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/0178cb5b-90e3-44cf-aa4c-8e2944bdc115/codex-dots.jpg?t=1794371131)

But he said there are three things he’s found useful for dots, as a power user. The first is as a **safety net** — your dot can flag things you may have missed. The second is as **“a delegation sink”**; Guo said he uses it “a ton in Slack” for this reason. And the third is as a **ubiquitous presence**.

“I think the third way that dots are a little bit different is we're aiming to **make them truly everywhere that you work**,” Guo said.

For his part, Kundel told me that he starts off a lot of his tasks from his dot, rather than by prompting Codex itself.

“And that's because it often already has a lot of the context and understands how to manage things, and it can push these PRs forward for me.”

## Conclusion

Kundel ended the workshop with a list of tips to use on Codex, including “turn it down” (meaning the effort level) and to “ask your agents for help.”

But ultimately, he said, the key is to not get too attached to your current way of working in Codex — **“things are constantly changing.”**
