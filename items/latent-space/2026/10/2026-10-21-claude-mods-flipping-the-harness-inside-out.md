---
title: 'Claude Mods: Flipping the Harness Inside Out'
link: https://www.latent.space/p/claude-mods
source: latent-space
published: 2026-10-21T13:04:19Z
updated: 2026-10-21T13:04:19Z
first_seen: 2026-10-21T14:37:58.044590695Z
authors:
- Dan McAteer
summary: Claude Mods is an early form of the Attention Interface. It flips the agent harness paradigm from the harness facing the machine, to the harness facing the human.
content: extracted
html: 2026-10-21-claude-mods-flipping-the-harness-inside-out.html
remote_preview:
  url: https://substackcdn.com/image/fetch/$s_!Kjjn!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc61f4d99-31a9-4e4c-aa02-65e5b5db1799_1280x720.png
---

Claude Mods is an early form of the Attention Interface. It flips the agent harness paradigm from the harness facing the machine, to the harness facing the human.

[![](https://substackcdn.com/image/fetch/$s_!Kjjn!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc61f4d99-31a9-4e4c-aa02-65e5b5db1799_1280x720.png)](https://substackcdn.com/image/fetch/$s_!Kjjn!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc61f4d99-31a9-4e4c-aa02-65e5b5db1799_1280x720.png)

For the last few weeks I’ve used Claude Mods to mold Claude Code to my AI engineering workflow. Claude Code is great out of the box, but every engineering workflow is different. [Claude Mods](https://code.claude.com/docs/en/plugins/mods/overview) let you make Claude Code yours, in the way you want to pay attention.

**The “paying attention” part is now the most important thing.**

In a recent Latent Space post, [The Evolution of the Agent Harness](https://www.latent.space/p/attention-interface), I argued that as models absorb more of the harness into their weights, **the part that’s left faces the human.** It becomes the **Attention Interface**: the layer that decides when the agent interrupts you, what it shows you, and which decisions it needs you to make. Claude Mods are an early version of the Attention Interface, and one you can shape yourself.

Here’s my bet: ultimately, **the harness becomes a platform that other companies build on**, with models as the engine. Just as the browser became a platform for web apps, the harness could become a platform for the interfaces between people and agents.

A mod is **a Claude Code plugin** that consists of **TypeScript or JavaScript functions** that run inside Claude Code. These functions customize the behavior and appearance of the interface in the CLI and Desktop. **Mods respond to events**, like a prompt, tool call or interface render. They can observe the event, modify how it’s rendered, and handle it in place of the default behavior.

[![](https://substackcdn.com/image/fetch/$s_!qzAy!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F00e9700e-b830-4845-ae84-bfc317d8f057_1600x960.png)](https://substackcdn.com/image/fetch/$s_!qzAy!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F00e9700e-b830-4845-ae84-bfc317d8f057_1600x960.png)

Claude Mods are **middleware for the agent harness**. They inject code that changes Claude Code’s execution and rendering flow, changing how the application behaves at runtime.

Claude Mods let the human and the agent build the human-facing part of the harness together. You can now work with Claude to design how Claude works with you. That is the essence of the “Attention Interface.”

The agent harness started as an interface between the agent and the machine it runs on. It lets you prompt the agent and the agent then sends commands, edits files and calls tools. Now it’s gradually becoming a way for the agent to interface with the human.

**Claude Mods let you reverse the direction of the harness.** Rather than the harness pointing from the agent to the machine, you point it from the agent to the human operator, and the harness becomes a way to interface with human attention.

[![](https://substackcdn.com/image/fetch/$s_!xzEx!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3183f682-cc38-425b-9747-42aa40878b29_2000x1220.png)](https://substackcdn.com/image/fetch/$s_!xzEx!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3183f682-cc38-425b-9747-42aa40878b29_2000x1220.png)

Claude Mods make the human layer programmable, by the human who uses Claude.

**Claude can also build mods for you.** Claude Code has a [built-in skill](https://code.claude.com/docs/en/plugins/mods/create#ask-claude-for-a-mod) that teaches it how to build mods, so you can ask for the mod you want in plain language and Claude will build it.

Some of Claude Code’s own features are already mods. Anthropic has shipped [six of them built-in](https://code.claude.com/docs/en/plugins/mods/overview#mods-built-into-claude-code). You can see these by running /plugin and opening *Installed*, where they’re listed under Built-in:

- **agents-md:** loads AGENTS.md as project instructions.

- **diff:** takes over /diff and draws its pane.

- **plugin-authoring:** gives Claude the skill it uses to write mods for you.

- **sec-default:** guards what your organization manages from the mods a user installs.

- **telemetry:** sends the analytics records that Claude Code and its built-in mods log.

- **you-should-know:** a side agent that shows a note above the prompt when it finds something you might have missed. It’s the only one that’s off by default.

Most of these work in the background, which is interesting because it means Anthropic is building parts of Claude Code as mods — even parts you may not notice. The one I’ve turned on myself is *You Should Know*.

Here are the two mods I’ve found most useful in practice: one a built-in, and one I built myself.

This is a native Claude Code plugin built by Anthropic. **It runs a side-agent that observes the main agent while it works** and flags important information you’re likely to miss. Think about it like: tool calls and decisions that have a material impact on the output, but aren’t necessarily easily visible because they’re buried in the agent trace. It comes built into Claude Code, but it’s off until you [turn it on](https://code.claude.com/docs/en/interactive-mode#you-should-know-suggestions).

Here’s a real example. I asked Claude Code to simplify the noisy UI text in one of my personal projects. The main agent trimmed the copy on the Compare screen, installed the new build and took before-and-after screenshots. Everything looked done.

Then *You Should Know* flagged something for me. CompareView.swift, the file the agent was editing, already had uncommitted work in it — and so did about 45 other files. If I later discarded the demo edits, I’d throw away that earlier unfinished work too. Its suggestion was to commit or stash the earlier work first.

In this case, the *You Should Know* mod saved me from discarding work unintentionally.

[![](https://substackcdn.com/image/fetch/$s_!t5Pz!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F021b43c4-0ad5-4f18-8345-f655e711bc58_2048x1150.png)](https://substackcdn.com/image/fetch/$s_!t5Pz!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F021b43c4-0ad5-4f18-8345-f655e711bc58_2048x1150.png)

This mod only runs **after a turn that edited files, ran commands or made 8 or more tool calls.** In the screenshot below, you’ll see six different pieces of information flagged for your attention. *Next Steps Supervisor* then suggests a next step to take, as a clickable button which sends the instruction as a message to Claude Code. This mod is available as part of **[ModSmith](https://github.com/DannyMac180/skills/tree/main/modsmith)** — an open-source Claude Code plugin I built with several different built-in mods, as well as a template Claude can use to build new mods that work together.

[![](https://substackcdn.com/image/fetch/$s_!mm-W!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F514aa5de-64f1-4721-b596-24c528448bfa_2048x1308.png)](https://substackcdn.com/image/fetch/$s_!mm-W!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F514aa5de-64f1-4721-b596-24c528448bfa_2048x1308.png)

Anthropic isn’t the only company that’s building an Attention Interface. Two recent products, one from OpenAI and one from Cursor, point in the same direction. Using the new **ChatGPT Intelligent UI** and **Cursor’s PR Routing & Approval** as a frame of reference shows what’s unique about Claude Mods.

On October 7, OpenAI shipped GPT-6 inside ChatGPT alongside a new ChatGPT interface it calls [Intelligent UI](https://openai.com/index/gpt-6-for-everyone/). Rather than answering only in text, **ChatGPT now can generate an interface as it answers**: charts, maps, checklists, forms, and small tools like a bill splitter or a savings calculator. All generated on the fly by the model as a type of **generative UI**.

In one of OpenAI’s examples, ChatGPT replies with multiple-choice follow-up questions to narrow down what you’re looking for. Think about it like: the model decides when it needs your judgment, then builds the interface for you to give it.

That’s an Attention Interface. The agent designs how you interact with its response.

But notice the difference between ChatGPT’s Intelligent UI and Claude Mods. For Intelligent UI, the model picks the interface, answer by answer, from a component library that OpenAI built. The one control mentioned at launch is turning the visuals down.

Cursor is working on the same problem from the other end: not one person and one answer, but **a team and a stream of pull requests**, more and more of them written by agents. Someone has to decide which changes deserve the attention of the human-in-the-loop.

[PR Routing & Approval](https://cursor.com/docs/approval-agents) makes that decision. It assigns reviewers based on code ownership and commit history, and it can auto-approve low-risk PRs. It weighs a risk score against a threshold you set, findings from Cursor’s Bugbot and Security reviewers, and policy files that live in the repo.

One rule caught my attention during research. If a PR edits an approval policy, the agent won’t use the edited version to relax review on that same PR. It reads the policy from the base branch instead. That means an agent can’t change the rules to get its own PR approved.

Cursor is also clear about the limits. [Its docs](https://cursor.com/docs/approval-agents) say the feature “does not replace a full code review.” The point is to spend human review where it matters, not to remove it.

All three systems **shape what gets your attention and how.** The main difference is who gets to design the Attention Interface.

- **ChatGPT Intelligent UI:** *the model* designs the interface for each answer, inside a frame the lab built. Zero setup, but little control. (I don’t believe the target user is engineers.)

- **Cursor PR Routing & Approval:** *the team* writes policy, and a fixed review workflow enforces it. Configurable, but the flow of the interaction is Cursor’s. You decide the rules, not the interface.

- **Claude Mods:** *you and the agent* build the interface itself, in code. A mod can hook into any point in the loop — from the prompt you submit to the tool call the agent makes to the moment a turn ends — and decide what to show you there and what you can do about it.

The supervisor I showed earlier is a good example. Cursor could easily route a PR to me because of the files it touched. ChatGPT could just as easily render the verdict as a nice card. But neither would let me decide that every turn which edits a file gets a second opinion, that the opinion lists shortcuts taken, and that each suggested next step is one keypress away. That interface exists because **I prefer to pay attention that way, and Claude built it with me.**

ChatGPT and Cursor give you an Attention Interface that someone else designed. Claude Mods let you design your own with Claude, but the tradeoff is you’re responsible for making it useful.

Mods aren’t free, and I’ve run into some downsides.

**The first is noise.** *Next Steps Supervisor* runs after any turn that edits a file, runs a command or makes 8 or more tool calls. **In practice, that’s nearly every turn.** Since I installed it on October 15, 99 of my 111 turns in Claude Code met that bar. Some of those verdicts are useful, others are just noise. So when a mod speaks up on almost every turn, I start skimming past it — which defeats the point of an Attention Interface.

**The second is cost.** Each supervisor check forks the conversation and re-reads it. Most of that is served from the prompt cache, but [the mod’s own README](https://github.com/DannyMac180/skills/tree/main/modsmith) estimates about 5–6k input-equivalent tokens per check on a 50k-token session, and more when the cache has expired.

**The third is trust.** A mod is code that runs with your permissions. [Anthropic’s docs](https://code.claude.com/docs/en/plugins/mods/overview#what-a-mod-can-reach) are clear about this: once it loads, a mod can read and write your files, see every prompt you send, and spend your usage. Plus, **“mods aren’t sandboxed.”** Built-in mods also keep running when you start Claude Code with *--safe-mode*. I’m comfortable with mods I wrote or that ship with Claude Code, but I’d read anyone else’s code before installing it.

**The upside is that mods are fixable in a way other tools aren’t.** When the supervisor is noise for a session, */supervisor off* turns it off. Because it’s my mod, I can change when it runs.

Customizing the harness isn’t guaranteed to be a win for every individual engineer. **Every mod I add is another piece of information I need to give my attention to.** In one of my own sessions, my *Next Steps Supervisor* marked a task as “Solved” yet also listed two gaps and three shortcuts. Agents are intelligent but far from perfect, and my attention is far from perfect too.

**So the test I use to gauge the usefulness of my mods** is simple: did it surface something I would have missed, make a decision easier, or let me act without getting distracted by more output? If not, it’s just window-dressing rather than a useful tool.

Right now, building a mod works like this: I notice friction in how I work with Claude, describe the mod I want, and Claude builds it with me. My personal attention is still needed to notice the friction.

**The next step is for the harness to notice the friction.** Claude Code already keeps my session transcripts. Think about it like: a background process reads through them, finds the places where I keep correcting the agent or checking the same thing by hand, and suggests a mod for it. Then it helps me test that mod and refines it from my feedback.

That’s the best case for AI engineers: **a malleable harness that suggests ways to improve itself from real-world use.** Claude Mods are a step in that direction.

If the bet I made at the start is right, in a few years we will talk about the startups that were built on Claude Code, or Codex, or a harness that doesn’t exist yet.

I’ll leave you with a question, where the answer hopefully provides you with practical value: what friction have you noticed working with Claude Code that this article can inspire you to solve with a mod today?
