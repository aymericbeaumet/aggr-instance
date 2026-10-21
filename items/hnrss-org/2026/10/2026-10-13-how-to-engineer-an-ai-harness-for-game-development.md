---
title: How to engineer an AI harness for game development
link: https://kylekukshtel.com/ai-harness-engineering-game-development-claude-code
source: hnrss-org
published: 2026-10-13T14:14:23Z
updated: 2026-10-13T14:14:23Z
first_seen: 2026-10-13T23:05:17.612288367Z
authors:
- kkukshtel
content: extracted
html: 2026-10-13-how-to-engineer-an-ai-harness-for-game-development.html
extra:
  comments_url: https://news.ycombinator.com/item?id=50072120
  num_comments: 49
  points: 112
---

![harness](https://cdn.blot.im/blog_bceb40b92fea4cb981fda87bd5169c58/_image_cache/ea931f86-2331-475a-8fcb-6a69f7a1ffd3/harness.jpg)

I gave a 45 minute presentation entitled “Your Game Is A Harness” as part of [Indie Rodeo in Calgary](https://indierodeo.com/) on 9/12/26. After speaking with a lot of people after my talk, I thought it would be good to more broadly share the ideas I presented there in a form that could be enjoyed by people beyond just those who attended the conference.

Also, because I talk about AI a lot on this blog, people assume I use AI to write. This is not true! Everything below is 100% human authored, with me writing the post with basically only the slides I presented as a reference (this is not a transcription). As much as I enjoy using AI to write code and tools, I get a similar enjoyment out of writing “the old way”, especially as I have no reason to try and make any part of this process more efficient.

So with that all out of the way, I present to you my first ever blogification of a talk I gave. Enjoy!

* * *

## Introduction

\[I give a small introduction to myself and my own work, that I’m omitting here, as you’re on my website\]

I want to talk about using LLMs in game development. Not one-shot type demos that look cool on Twitter, but like *real* development. Where you’re working eight hours a day, 5 days a week, etc.

And before I get into it, I think it’s useful to level set about LLMs. Or at least get into some basics so the rest of what I talk about here makes sense. I recognize that for a lot of this people this will be review, so if you just want to get into the dev-loop stuff, scroll down to the next section.

My journey into using LLMs for dev work started with a moodboarding site I made called [mood.site](https://www.mood.site). This was pre “agentic” anything, but I wanted to test and see if ChatGPT at the time could actually code anything. The process was a lot of me just blindly copy/pasting code into ChatGPT’s web interface. The process sucked, but I was surprised at the results. [I built the site! It worked!](https://kylekukshtel.com/mood-site-moodboards-spite-driven-development) I still needed to verify a fair amount of stuff myself, but it was enough to convert me. If I didn’t have access to an LLM I wouldn’t have been able to make that site happen in near the amount of time it ended up taking me (~2-3 weeks of nights, probably like 20 hours total). I’d probably still be working on building the first version today.

Since then, mood has become something of a testbed for me moving through LLM stuff (and a mildly successful project on its own!), and has inspired me to kick up lots of other projects that use similar stacks. Additionally, LLMs have gotten better, especially with things like Claude Code and later Claude models. From my own experience, I feel like now there is very little, if anything, I think an LLM *can’t* do when it comes to a web-based stack.

It could be easy to then say LLMs can do *everything*, as so much of software is web software. But whenever I tried to use an LLM on building a non-web game *thing*, it was this resounding takeaway:

![Screenshot 2026-09-19 at 2.00.12 PM](https://cdn.blot.im/blog_bceb40b92fea4cb981fda87bd5169c58/_image_cache/49535a95-d741-4768-88f1-cb1c5715491f/screenshot-percent202026-09-19-percent20at-percent.png)

## “LLMs are Bad At Games”

Any time I tried to use an LLM for a game-y thing, it would fall over. Maybe this is my own bias. I’ve been a game programmer for nearly 15 years, I think I know how things work. I can sniff out “bad” game code a mile away. But when it comes to Svelte web-apps, I’m decidedly less well versed and am inclined to think that whatever code the LLM produces is “good”. Maybe it’s *actually* garbage. But there *is* reason to believe the Svelte and web-stack stuff *is* quality, in part due to the fact that there are LOTS of high quality web projects with their source available online. Comparatively for games, I imagine there are orders of magnitude less data to train on.

Not only this, but games have a lot of *stuff* in them that isn’t code. Games aren’t just about putting static images and text in a layout. There are *tons* of things in games that aren’t easily representable by code. *AND* then on top of that there is all the same text layout stuff of web, but only as a subset of the 100 other systems a game needs to handle.

This is part of the reason why a lot of people have been making games in Three.js — they can easily represent assets as plain boxy primitives and skip a lot of the asset generation/conforming work and processing normally required for games. As a result, a lot of that stuff is slop because it lacks any real authorial voice outside of whatever the LLM median is.

However, maybe they have a point.

![Screenshot 2026-09-19 at 2.01.19 PM](https://cdn.blot.im/blog_bceb40b92fea4cb981fda87bd5169c58/_image_cache/cb603342-3977-46f7-95a0-c915d9ba9985/screenshot-percent202026-09-19-percent20at-percent.png)

A key idea of the one-shot Three.JS workflow (as well as LLMs proficiency at building web apps) is that *everything* is code. By having *stuff* as code, it’s very easy for an LLM to be able to reason about it, make changes, etc.

![Screenshot 2026-09-19 at 2.02.41 PM](https://cdn.blot.im/blog_bceb40b92fea4cb981fda87bd5169c58/_image_cache/bcc1482a-1271-4913-88c7-ccb5daf20d76/screenshot-percent202026-09-19-percent20at-percent.png)

However for traditional games, this is not the case. Or at least the full story. Games obviously have code, but there is a lot of other stuff that is *not* code. This means that there are key components of games that are effectively opaque to an LLM. If your game then relies on the effective puppeting and transforming of that opaque stuff, then an LLM is going to fall over when trying to work on your game.

![Screenshot 2026-09-19 at 2.09.12 PM](https://cdn.blot.im/blog_bceb40b92fea4cb981fda87bd5169c58/_image_cache/44565332-1805-4b62-8cb4-b3db1fe20774/screenshot-percent202026-09-19-percent20at-percent.png)

I think a lot of the gamedev community maybe gave LLMs a go a bit ago and ran directly into this. The “LLMs are bad/not useful/etc.” sentiment (massive caveat here, just talking about capability specifically and not potential society-ending implications) came from running headfirst into these limitations.

![Screenshot 2026-09-19 at 2.11.28 PM](https://cdn.blot.im/blog_bceb40b92fea4cb981fda87bd5169c58/_image_cache/a439c8bd-6769-4cb3-9a97-1f8b684b6bc5/screenshot-percent202026-09-19-percent20at-percent.png)

This is to say that, structurally, LLMs are bad at reading games. At least in their default state.

## LLM Basics

Let’s talk more about why, and what we can do about it.

Any call/prompt to any LLM service breaks down into three basic components. A system prompt, an assistant prompt, and a user prompt:

**System prompt:** Generic text shared across all users of a service. This usually describes the basics of the LLM itself, with something like “You are Opus 5, an intelligent Large Language Model created by Anthropic…”. This isn’t to say these aren’t important (they are), but more that they aren’t super-relevant to our use case. [You can see some examples of posted (and exfiltrated) system prompts for models here](https://github.com/asgeirtj/system_prompts_leaks).

**Assistant prompt:** This is kind of a sidecar prompt section meant to help guide the AI in terms of what it is responding to. You can think of it basically as the model’s basic “interaction” vector with your prompt, and represents something like the model’s “response” to the user based on some input. You usually do not have to think about this.

**User Prompt**: This is really The Thing. Most any input you put into any AI tool goes here. Any submitted chat, code, etc. gets dumped into this section. When people talk about “context”, they are largely talking about “how many tokens are stored in the user (and assistant) portion of a prompt”.

It may be easy to think that each chat is a “fresh” version of the above, but what actually happens is that, as you work with a model in a given session (the bounds of “session” vary based on what service you are using — this can be a single Claude Code session before closing it, a “chat” accessible in the side panel of a web hosted model, etc.), every prior message you sent to the model is re-submitted *every* time you send a message. It’s kind of like playing the old playground game where, on your turn, you have to say everyone else’s thing *again* before then adding your own thing for the next person to have to add on.

![Screenshot 2026-09-24 at 8.35.14 AM](https://cdn.blot.im/blog_bceb40b92fea4cb981fda87bd5169c58/_image_cache/c2334c7c-25e3-4db6-b41d-8f11ce15b73b/screenshot-percent202026-09-24-percent20at-percent.png)

## Context and Context Pollution

As you do this over and over, you build up the “context” of the session you’re working in.

![Screenshot 2026-09-24 at 8.35.51 AM](https://cdn.blot.im/blog_bceb40b92fea4cb981fda87bd5169c58/_image_cache/c6f18dec-e613-4de5-995e-323d77195ba5/screenshot-percent202026-09-24-percent20at-percent.png)

This is all well and good, but a key thing to notice here is that you only really are able to address the “latest” thing in your context (i.e., you submit a new message). Things from previous messages get submitted over and over again to the model. Hence, you want to make sure what’s in your context is “good”, otherwise you can find yourself in degenerate scenarios where your context becomes polluted due to “bad” things earlier continually informing new work.

![Screenshot 2026-09-24 at 8.47.56 AM](https://cdn.blot.im/blog_bceb40b92fea4cb981fda87bd5169c58/_image_cache/4bf2056d-6265-44e2-8956-b1e6fe1b35c9/screenshot-percent202026-09-24-percent20at-percent.png)

Managing context then is a key thing in working with LLMs, and a new engineering practice called “context engineering” has emerged in light of these constraints. Context engineering asks “how can we get the *best* information into the context, with as little pollution as possible”.

There [is a great blog post here on “ways long contexts fail”](https://www.dbreunig.com/2025/06/22/how-contexts-fail-and-how-to-fix-them.html) that goes a bit more into this, but I’ll highlight the key part here:

![Screenshot 2026-09-24 at 8.50.21 AM](https://cdn.blot.im/blog_bceb40b92fea4cb981fda87bd5169c58/_image_cache/0dacdcdf-314b-4b09-8ea0-cade91cde605/screenshot-percent202026-09-24-percent20at-percent.png)

Context Engineering itself is a really big topic that is outside the scope of this talk/blog, but we will be using some of the tenets of it here to explore how to help LLMs do a better job working with games.

Said differently, getting LLMs to work well with games is a *context* problem, not something that is intrinsically linked to how games or LLMs “are” (or “aren’t”!).

![Screenshot 2026-09-24 at 8.52.19 AM](https://cdn.blot.im/blog_bceb40b92fea4cb981fda87bd5169c58/_image_cache/d73b4f8b-6c5f-42b7-ae01-f0614af6e479/screenshot-percent202026-09-24-percent20at-percent.png)

The complicating factor here though is, games are *weird*. Or at least: we have no real template to build a game. We have game engines, but there is effectively 0 standardized tooling for anything, in part because every game manifests its own needs differently from one another.

![Screenshot 2026-09-24 at 8.56.06 AM](https://cdn.blot.im/blog_bceb40b92fea4cb981fda87bd5169c58/_image_cache/9311b154-4788-417a-8a24-728868453a7b/screenshot-percent202026-09-24-percent20at-percent.png)

This is acutely unique to games (imo), but it manifests enough in other technological domains that a new concept has emerged in order to get agents to effectively “work” with whatever your work product is: Harnesses.

## Some Description of the Idea of a Harness

The general idea of a Harness is that it’s some set patterns of actions an agent is meant to take in response to some well (or loosely) defined expected input. You’re probably already using a harness like Claude Code, but these can be augmented, extended, distilled, abstracted, etc. to better support the *specific* work you are trying to do. There’s also been a LOT of action in this space recently, with many harnesses being built for developing all sorts of things:

![Screenshot 2026-09-24 at 8.59.31 AM](https://cdn.blot.im/blog_bceb40b92fea4cb981fda87bd5169c58/_image_cache/d91d1fe7-70a9-4264-adf9-8162c1716d73/screenshot-percent202026-09-24-percent20at-percent.png)

The key things these are all pointing to is: “how can I better understand *what* I’m building, and guide that process as it is developed”. Harnesses can tie in things like iterating with a browser, telemetry, profiling, and even full SDLC work like making tickets, documents, etc.

However, the thing *none* of these do well is actually talk *to* your game. Your game is a bit of a black box, and most of what’s visible to an LLM is only the code-as-written, which as we talked about before, isn’t really enough. We know LLMs are already good at working with code, so it’s not like some harness itself solves that “games aren’t just code” issue we ran into in the first place.

What really needs to happen here is that we can take all the *stuff* in games, and try to turn it *into* something the LLM is able to work with. We need to crack open the black box of your game and make more of it addressable directly by an LLM.

![Screenshot 2026-09-24 at 9.06.06 AM](https://cdn.blot.im/blog_bceb40b92fea4cb981fda87bd5169c58/_image_cache/0b5defe5-549b-4e8d-a154-871f207a035f/screenshot-percent202026-09-24-percent20at-percent.png)

![Screenshot 2026-09-24 at 9.06.24 AM](https://cdn.blot.im/blog_bceb40b92fea4cb981fda87bd5169c58/_image_cache/909ab81a-c69d-44b4-805f-fb91233f2cd9/screenshot-percent202026-09-24-percent20at-percent.png)

During the talk version of this I had this Very Funny Slide that said that the arrow here was the actual content of this talk. The joke hits nowhere near as hard in blog form but I’m leaving it here for posterity:

![Screenshot 2026-09-24 at 9.06.38 AM](https://cdn.blot.im/blog_bceb40b92fea4cb981fda87bd5169c58/_image_cache/9af445d1-ef54-4854-8f83-25e5d133c5c9/screenshot-percent202026-09-24-percent20at-percent.png)

![Screenshot 2026-09-24 at 9.07.28 AM](https://cdn.blot.im/blog_bceb40b92fea4cb981fda87bd5169c58/_image_cache/4fef54cb-36b7-463f-95e9-f386be35145f/screenshot-percent202026-09-24-percent20at-percent.png)

So let’s talk about what we can do.

## Let’s Make a Game Legible To AI Agents

Here’s some very tangible, actionable things you can do for your game to make it more legible and *workable* by an LLM.

As a brief aside before getting started, it’s also worth noting here that a lot of these tips are good for game development *in general*. They get supercharged a bit with AI, but doing any of this will likely make working on your own games generally just *better*.

## Fixed Randomness

A running theme through this whole talk/post is the concept of *determinism*. Basically, how accurately can you reproduce a very specific state in your game. The general goal is that it should be very easy, and very exact. Doing this at the very end of your production will be VERY hard to do, and would likely require fundamental changes to your game in the 11th hour.

Instead, if you assume this constraint before you write your first line of code, and stick to it as a goal, you’ll be paving the path to a much more productive end game and not find yourself fighting off entropy and spaghetti code.

Starting off here is pretty easy. At the start of your game, create a single source of *seeded* randomness. Make sure any call to anything non-deterministic uses this source of randomness. This means that your game will be able to deterministically recreate almost any state in the game. If your code calls Random.Float or whatever in 30 places to determine some gameplay behavior, you know that the 30th call will be the same every time. Easy.

## Screenshot-ability

Why have reproducible states? Many reasons, but for an agent, imagine you’re iterating many times on trying to correct something. It’s important for the agent to be able to get to the same state from scratch over and over again without needing to churn through state replication failures.

And once you get there, how do you know? How can you evaluate those changes?

Well for one, screenshots! Most engines have some built in screenshot API or functionality:

![Screenshot 2026-10-01 at 12.22.01 PM](https://cdn.blot.im/blog_bceb40b92fea4cb981fda87bd5169c58/_image_cache/5da4a53a-b8a7-4d55-9619-2f8813bb87a6/screenshot-percent202026-10-01-percent20at-percent.png)

I also recommended adding in the ability to easily toggle UI layers, gameplay layers, etc. on/off that way you can reduce visual clutter that distracts the LLM from the layer you’re trying to address (ie, dont have gameplay under a UI if you’re trying to fix some UI style thing).

Taking a before and after picture of your game between agent changes is a nice way for an agent to be able to understand what changed.

…Or is it?

![Screenshot 2026-10-01 at 12.15.58 PM](https://cdn.blot.im/blog_bceb40b92fea4cb981fda87bd5169c58/_image_cache/2b1f1d96-c14c-4479-b09c-5394c19cc375/screenshot-percent202026-10-01-percent20at-percent.png)

When you take two screenshots (Before/After) a change, the LLM, even if it’s multi-modal (ie, has vision capabilities), will likely have a VERY hard time not only understanding what the change is, but understanding if the change it made moved it closer to its own end goal.

This noise from image data is its own form of context rot: naiively throwing screenshots at models can muddy their own sense of progress, leaving both them and you confused.

A visual example can help here: Imagine you take a screenshot of the home page of this conference:

![Screenshot 2026-10-01 at 12.18.48 PM](https://cdn.blot.im/blog_bceb40b92fea4cb981fda87bd5169c58/_image_cache/7173be38-858e-40c0-8e35-14ad26b1299b/screenshot-percent202026-10-01-percent20at-percent.png)

You ask the model to “Make the skeleton a pirate”. Assuming if (and with text LLMs, unlikely) this can be applied as an annotation to the image ([Google Stitch](https://stitch.withgoogle.com/home) does this), your image will be passed to the model provider with your tag, but then also all the *other* stuff you aren’t trying to address (in green below).

![Screenshot 2026-10-01 at 12.20.46 PM](https://cdn.blot.im/blog_bceb40b92fea4cb981fda87bd5169c58/_image_cache/6a078c0b-82ac-48ee-abdc-bcbbdaba4d48/screenshot-percent202026-10-01-percent20at-percent.png)

This means that everything in green is something akin to “context pollution”. It is data the LLM is processing as well as your requested change, but it’s not just thinking about what you requested, but is thinking about the *whole* image.

You need to get the LLM to think about *only* what needs to change. How do you judge what has changed?

Image diffing!

![Screenshot 2026-10-02 at 9.32.48 AM](https://cdn.blot.im/blog_bceb40b92fea4cb981fda87bd5169c58/_image_cache/811c2ad5-8a36-468f-b06d-e19b61008b9e/screenshot-percent202026-10-02-percent20at-percent.png)

If you take two images and “diff” them, like you do with code changes, you can easily see what has changed. Combined with clever uses of cropping, you can have an LLM understand, with a quantitative measure, how closely some output post-change is close to your provided input/target image.

Luckily, you don’t have to do this yourself. There is a *very* good free, [command-line image-diffing tool called odiff that does exactly this](https://github.com/dmtrKovalenko/odiff).

![Screenshot 2026-10-02 at 9.35.12 AM](https://cdn.blot.im/blog_bceb40b92fea4cb981fda87bd5169c58/_image_cache/198df64d-a1f7-4981-b5f1-2497faa179d7/screenshot-percent202026-10-02-percent20at-percent.png)

Given two different images, odiff finds the “difference” between the two, and can output that difference as a “diff” image:

![Screenshot 2026-10-02 at 9.36.25 AM](https://cdn.blot.im/blog_bceb40b92fea4cb981fda87bd5169c58/_image_cache/1745b973-13a4-41eb-b0b4-0a5f91e5eee5/screenshot-percent202026-10-02-percent20at-percent.png)

odiff also can give you a quantitative difference value like “85% similar”, that an LLM is very good at reading, and can then use that as a stable baseline of eval across each agent turn as it strives to reduce the difference/increase the similarity.

Said differently, as you are working, these agent screenshot -> progress diff check become the basis for continual agent evaluation towards whatever your task is:

![Screenshot 2026-10-02 at 9.40.29 AM](https://cdn.blot.im/blog_bceb40b92fea4cb981fda87bd5169c58/_image_cache/6e01ed42-5eec-49a9-b93e-b53f649c03bc/screenshot-percent202026-10-02-percent20at-percent.png)

However, this obviously isn’t all. Games are much more than just their visual elements. “Progress” here breaks down into it’s own little inner loop:

![Screenshot 2026-10-02 at 9.47.22 AM](https://cdn.blot.im/blog_bceb40b92fea4cb981fda87bd5169c58/_image_cache/a10394cd-a7e9-4756-b4ee-bc91b04c0344/screenshot-percent202026-10-02-percent20at-percent.png)

This inner-loop is likely the place where an agent will spend most of its time, doing multiple turns on its own before going back to the screenshot comp/diff (assuming that your change has some visual implication).

The goal here is to be able to make every change be the *minimum* amount of change required for a desired outcome, and have those changes able to be quickly verified. You want an incredibly stable, “boring” baseline that can be evaluated in near-isolation to ensure that anything built on top of it doesn’t produce wide ranging technical debt (aka, spaghetti). So let’s talk more about deterministic state.

![Screenshot 2026-10-02 at 9.53.45 AM](https://cdn.blot.im/blog_bceb40b92fea4cb981fda87bd5169c58/_image_cache/0b43f1a3-09d6-4b96-9733-abf23abac67a/screenshot-percent202026-10-02-percent20at-percent.png)

## Deterministic State

The rest of the items in the above graph also deal in some part with deterministic state, but I want to talk about specific strategies and techniques for achieving it, as they will be essential for everything else.

There’s a simple question to ask to understand/know how close you are to deterministic state: how easily can you save and load an exact state in your game? Or said differently, the most important part of state reproduction is having a strategy for efficiently saving and loading your game *from the start*.

Saving/Loading is usually something amateur developers will put off to the end of a game, but this is a grave mistake, as many more seasoned developers will tell you. It’s easy to think of saving/loading as a “feature” that you can add at some point during a game’s development, but saving and loading, when leveraged at the VERY start of a game and programmed in conjunction *with*, can be a development superpower that has a near innumerable list of benefits.

Let’s talk about one strategy for doing this, and why it makes ALL of development easier.

![Screenshot 2026-10-02 at 10.00.47 AM](https://cdn.blot.im/blog_bceb40b92fea4cb981fda87bd5169c58/_image_cache/7d009ffc-4175-4fdc-9084-37b1e2ffb9d2/screenshot-percent202026-10-02-percent20at-percent.png)

We’re going to take a small jaunt here into CS-style programming theory. Most people that spend extensive time with object-oriented type architectures for games will find that, near the end of their project, something like the above statement “A Tree is a Dog” is true. They look at their code, and what things actually do in a game, and realize that, these two seemingly very different objects with separate OOP hierarchies are, at the end of the day, actually more or less the same kind of “thing” from the perspective of the code.

A tree is an object that has a mesh and a collider. A dog is an object that has a mesh, collider, and can move. “Movement” is not an intrinsically “dog” thing (other things in the game may move), and hence a tree and a dog are basically just compositions of components that have some game-representation. From the code’s perspective, they are effectively the same.

This can be counter-intuitive at first, as it *feels* wrong, but there is a chakra-unlocking insight here that, when you hold on to, starts to make game programming much easier and not a constant uphill battle against a wall of abstraction that occurs when you try to maintain a strong OOP-style object model in your game.

## Thing-Driven Development

Instead, if you take the idea I’m proposing and run with it, you can assume from the very start of your game that everything is actually the same thing (maybe even use the type `Thing`). If you were to define Thing in code, you’d have something that looks like this:

![Screenshot 2026-10-02 at 10.17.15 AM](https://cdn.blot.im/blog_bceb40b92fea4cb981fda87bd5169c58/_image_cache/c8b7dfb0-b456-4bad-9731-9b85e35f73db/screenshot-percent202026-10-02-percent20at-percent.png)

You can see a Thing has some stat-type values on it as well as a shared idea of “liveness” from the isAlive flag. Otherwise, the “types” of thing is just an enum value, and based on that type the implementing code can take some action.

And as It turns out, AI REALLY likes this.

![Screenshot 2026-10-02 at 10.18.21 AM](https://cdn.blot.im/blog_bceb40b92fea4cb981fda87bd5169c58/_image_cache/dded6e6e-4347-4376-9bc8-ce4961689de0/screenshot-percent202026-10-02-percent20at-percent.png)

With this type of architecture, there is a *single source of truth* for your object model. If instead you have 100 classes of 100 different types of objects, an AI will need to likely understand *all* of those (and their relationships) to take action. This fills up your context, and makes reasoning about how to fit into that pattern messy — should I (the AI) extend some class? Morph the type of a class to better capture this new thing? Make a new class? It’s hard to say!

With a single object type, there is no ambiguity. AI knows exactly where to make changes, *every* time.

Not only this, but (getting back to saving/loading), having a single object type makes saving and loading FAR easier:

![Screenshot 2026-10-02 at 10.20.44 AM](https://cdn.blot.im/blog_bceb40b92fea4cb981fda87bd5169c58/_image_cache/1dd012fc-0e0c-4c5a-b731-403699e5af01/screenshot-percent202026-10-02-percent20at-percent.png)

When you have one object type, deserializing/serializing is trivial — it’s a single type, so there is one way in/one way out. With multiple different types (and potential trees of inheritance), you will likely find yourself in weird race condition deserialization paths, needing to link up objects and trees of children in strange ways and grappling with concepts like “[polymorphic deserialization](https://learn.microsoft.com/en-us/dotnet/standard/serialization/system-text-json/polymorphism#polymorphic-type-discriminators) ”. It is well and good to know about these things and use them when required, but for games There Be Dragons here.

A game then is just a giant list of Things. Saving is saving all those things through one path, and loading is the same, but in reverse. Any casing you need to do can happen on the deserialized enum type value. It’s nice!

I’m also far from the first person to ever talk about this, so if you want to hear more about this I highly recommend some of the episodes of Wookash podcast where he talks with some of the creators of [Dreams](https://www.playstation.com/en-us/games/dreams/) about a very similar system:

[www.youtube.com](https://www.youtube.com/watch?v=mFBmoCv5EcQ)

By working with a system like this, reproducing state immediately is quite easy and you skirt all the difficulties of saving and loading. There is more complexity here, and I do recommend the above talk for further reading, but the fundamental idea is the same — capture everything as a single object type. State itself is then easily captured by the same system. Nothing is stopping you from have a Turn Number field on every structure as well, and a Type that is GameManager. Make this object do literally everything. And no, [it’s not memory expensive like you may think it is](https://www.computerenhance.com/p/q-and-a-80-2025-10-31).

## Addressable State

Once you have a system like this, it makes other parts of the game easier as well.

![Screenshot 2026-10-02 at 10.31.00 AM](https://cdn.blot.im/blog_bceb40b92fea4cb981fda87bd5169c58/_image_cache/1972299f-5207-49f8-b779-06c0957023dd/screenshot-percent202026-10-02-percent20at-percent.png)

If you have one type of object, it’s very easy to capture changes to state, because *any* change in your game presumably targets *the* single object type. You may want to change these values directly, but with this “fat struct” approach you can very easily implement something called the “[Command Pattern](https://gameprogrammingpatterns.com/command.html) ” for even more benefit:

![Screenshot 2026-10-02 at 10.32.01 AM](https://cdn.blot.im/blog_bceb40b92fea4cb981fda87bd5169c58/_image_cache/9fcaaad2-131b-4359-a576-e8d7bea54635/screenshot-percent202026-10-02-percent20at-percent.png)

The idea of the command pattern is that you capture changes to an object itself not as a loose function call, but instead as an (ideally serializable) object. These commands then get passed through some processing class, which makes the changes to the objects themselves instead of you directly. This level of indirection can again seem strange at first, but bear with me.

Imagine you have some starting state. The player makes some actions that alter state, which manifest as commands. This results in some end state, where a bug occurs.

Now lets say you want to try to reproduce this. One version of this is you trying to communicate with the player to figure out what they did and try to do the same thing in your copy of the game.

OR, you can take the starting state, and “playback” the serialized commands, which capture *exactly* what the player did. And notably, this doesn’t have to be you literally playing the game or writing bespoke function calls to change state: you just playback the commands. Or not even you, but an AI agent can also run them back as it continually tries to assess how its changes affect state.

![Screenshot 2026-10-02 at 10.38.21 AM](https://cdn.blot.im/blog_bceb40b92fea4cb981fda87bd5169c58/_image_cache/2af2dda4-3767-4de6-a84b-f2f26243cef6/screenshot-percent202026-10-02-percent20at-percent.png)

Taking this a step further, you may notice that these commands also don’t actually have to be authored from the game itself. You can easily string together commands in a text editor (or authored by the AI), and then have them played back on a running instance of the game, no compiling required!

![Screenshot 2026-10-02 at 10.40.32 AM](https://cdn.blot.im/blog_bceb40b92fea4cb981fda87bd5169c58/_image_cache/9955ae2a-daa3-437d-be4f-79bdef986188/screenshot-percent202026-10-02-percent20at-percent.png)

Commands give the AI (and you), a typed interface to interact with your game, effectively making it “puppet-able” and not requiring every change or attempt to work with your game need some bespoke processing logic or reflection (and then compiling those changes). AI likes this!

To linger on no-compile changes for a bit: the more you can drive your game to be able to make changes without compiling, the better. Not just as some badge of honor, but especially for agentic loops — the more it can test without needing to wait for *any* compile time means it’s able to act more quickly and make/validate changes in a tighter loop.

## On Data Management

This dovetails nicely into [one of my own pet peeves](https://www.gamedatapodcast.com/): data management. Lets go back to the fat struct example:

![Screenshot 2026-10-03 at 1.15.51 PM](https://cdn.blot.im/blog_bceb40b92fea4cb981fda87bd5169c58/_image_cache/8086c2c9-9457-4c96-87d2-bc97e19fc300/screenshot-percent202026-10-03-percent20at-percent.png)

When I say “data”, I’m talking about the static, operational data your game uses to describe how things *are*. In the example above, I’d consider Type, attackDamage, walkSpeed, and boatSpeed all “data”. These are characteristics of the object that do not change. Things can modify these and work with these, and players may never see literally these numbers, but they describe the baseline shape of some Things existence in the game.

If you were making things for your game, you may write out a lot of your structs directly, setting their properties based on what you know they are. Those changes then get compiled into the game. If you want to change something’s *base* walk speed, you need to either make the change and recompile, or write some function binding that assigns the value and is externally callable (think console commands).

All of these options introduce churn to the AI agent. Especially in the scenarios where you’re compiling, you’re putting testing basic value changes behind a compilation.

The better way to do this is to have all your data *external* to your game instead of represented as code. Assigning this data is just a matter of deserializing the data, applying it to the correct objects, and done! So changing data is just a matter changing in your data source of choice, setting up some basic deserialization in your game, and combine it with object creation. Done! This also allows you (or an agent, or other team member, etc.) to be able to iterate on your game with a *build*! They don’t even necessarily need editor access.

![Screenshot 2026-10-03 at 2.17.15 PM](https://cdn.blot.im/blog_bceb40b92fea4cb981fda87bd5169c58/_image_cache/f5317c60-40f6-4d9b-9f97-0b975da8a591/screenshot-percent202026-10-03-percent20at-percent.png)

For [Cantata](https://store.steampowered.com/app/690370/Cantata/), this was our exact workflow. All the non-programmers on the team (~7 people) were given builds of the game and our custom data editor Depot. This workflow was so effective, [I’m now turning Depot into a full-fledged product](https://depot-editor.com/):

![img](https://cdn.blot.im/blog_bceb40b92fea4cb981fda87bd5169c58/_image_cache/e7fc0f8e-df1a-41be-964e-c5ce5822195f/screenshot.png)

Team members made changes in Depot, and the game build handled deserialized changes so they would work in a loop without needing me to cut new builds. Depot is my tool of choice here, but other teams use things like Google Sheets, or [their own custom tools](https://www.riotgames.com/en/news/content-efficiency-game-data-server).

AI agents can be taught to use your own tools, and then can, like other team members, test out data changes on their own without needing to rebuild the game constantly. The tighter you can make the agentic loops, the better! It’s all about trying to remove cycle bottlenecks.

![Screenshot 2026-10-03 at 2.18.01 PM](https://cdn.blot.im/blog_bceb40b92fea4cb981fda87bd5169c58/_image_cache/9f384d7d-dd15-4ea5-95b8-6ebaed10390e/screenshot-percent202026-10-03-percent20at-percent.png)

This leads into another core idea. It’s a bit of a rabbit hole, so bear with me.

## Standalone Simulation / Blackbox

When developing a game, either in an engine or on your own with some custom tooling, it’s usually the case that the logic of the game itself is intermingled with visual state transitions, UI calls, etc. This is by no fault of anyone, but it makes sense as a default model: some action happens, you update the UI, maybe change some visuals, etc.

However, in putting all of this together, especially in engine, you start to realize that to start trying to test anything or reproduce someone’s bug, you need to have the *whole* game running and doing its thing. Models have to be loaded, sounds setup, scenes loaded, etc. The *simulation* of your game necessitates having “the whole game”. When things start to break, it becomes hard to disambiguate between what is breaking and why: is it the simulation? The models? The scene design?

A better way to work here is that the *simulation* of your game sits *outside* of the actual operating logic of the engine. And by outside, I mean literally as its own DLL/library. The game consumes the simulation and its outcomes, but doesn’t itself “own” the simulation.

Instead, the engine communicates with the simulation through “commands”, not dissimilar from how clients and servers work in web programming.

![Screenshot 2026-10-04 at 8.43.30 AM](https://cdn.blot.im/blog_bceb40b92fea4cb981fda87bd5169c58/_image_cache/c207e383-272e-4e34-82fd-2f0c07dd89d8/screenshot-percent202026-10-04-percent20at-percent.png)

These commands can (and should!) take the form of the previous idea of commands we introduced earlier:

![Screenshot 2026-10-02 at 10.32.01 AM](https://cdn.blot.im/blog_bceb40b92fea4cb981fda87bd5169c58/_image_cache/9fcaaad2-131b-4359-a576-e8d7bea54635/screenshot-percent202026-10-02-percent20at-percent.png)

As the engine pipes commands to the simulation core, the simulation responds with the updated states passed back to the engine. The engine then interprets the updated states (or diff’d state from the last received state) and acts on it.

This sounds a bit abstract, so here’s a more specific example:

Imagine something is moving in your game. That movement is likely a few different things — animation, models, sound effects, and logic around movement start, continuing, stopping, with maybe intermediate updates for things like vision as the object moves. However, what movement fundamentally “is” is something going from point A to point B in response to some player input.

The idea is that the engine sends in a movement request for some Thing (fat structs remember!), and the simulation independently decides if that’s a valid movement target (the engine can also filter for this specifically before submitting the command). The simulation then resolves the movement, and gives back to the engine an updated state object that shows the object has indeed moved. The engine can understand that the movement happened by diffing against the previous state, or other ways that are too complex to fully get into here (recording buffered commands and acting on them if the simulation accepts them, etc.). The key though is that the engine is “dumb” — it just acts on what it gets from the simulation and doesn’t question it. The simulation is the source of truth: if the simulation says something moved, the engine then moves the object, no questions asked.

This level of indirection can seem like too much abstraction, but when you have your simulation core separate from your engine, it means that *anything* can send commands at your core:

![Screenshot 2026-10-04 at 9.18.05 AM](https://cdn.blot.im/blog_bceb40b92fea4cb981fda87bd5169c58/_image_cache/1cd225ac-9377-4cc9-a7bf-9f8cc1e39d03/screenshot-percent202026-10-04-percent20at-percent.png)

The biggest immediate win here is that network programming becomes FAR easier — because your game is setup to just consume states/diffs, where they are actually coming from doesn’t matter. They can come from the network, LLM, whatever!

There are even simpler use cases. Testing in game development is often a bit of a bear to tackle, but I’d argue the reason for a lot of that is because state/logic/data are often coupled so tightly that it’s very hard to test the game because it requires so much scaffolding.

However, leveraging an architecture like this allows you to *easily* test a game’s simulation, without needing an engine at all. You can rip through tests with pure C# projects, using standard C# tooling, and not need any engine running your code. AI likes this!

![Screenshot 2026-10-07 at 9.52.56 PM](https://cdn.blot.im/blog_bceb40b92fea4cb981fda87bd5169c58/_image_cache/f27502ee-9b14-4ec0-b69f-dc8a8da6e40d/screenshot-percent202026-10-07-percent20at-percent.png)

This idea broadly is called “Blackbox Game Architecture”. Versions of it have existed before, but this talk by Brian Cronin as part of Roguelike Celebration a few years ago goes into technical detail about he leveraged the technique for Monster Train:

[www.youtube.com](https://www.youtube.com/watch?v=qO3CIpP62Q0)

I also talked with the team behind [Solium Infernum](https://store.steampowered.com/app/1893810/Solium_Infernum/) as part of [Game Data Podcast](https://www.gamedatapodcast.com/) where they described a similar technique:

[www.youtube.com](https://www.youtube.com/watch?v=lij4Mlc8I4I)

When you leverage something like this, it becomes a gamedev superpower. You can start really working with your game as a technical object that is abstracted from the actual player experience of it. You can run simulations, hypothesis, tests, etc., directly on the simulation itself and can skirt all the questions of state that usually plague related efforts that don’t use some architecture strategy like this.

![Screenshot 2026-10-10 at 12.42.23 PM](https://cdn.blot.im/blog_bceb40b92fea4cb981fda87bd5169c58/_image_cache/36b28bdb-76ef-4aa7-91f0-c5a4cca2aa4d/screenshot-percent202026-10-10-percent20at-percent.png)

Going back to our chart above, we’ve made out way to the near end of our flywheel. We set some deterministic input parameters, have ways to modify that state in structured ways that can avoid compilation, and can then verify our own changes in a vacuum.

## Impact Assessment

However, how do we actually know those changes then affect the actual game how we want? This is the idea of “impact assessment”.

![Screenshot 2026-10-10 at 12.46.40 PM](https://cdn.blot.im/blog_bceb40b92fea4cb981fda87bd5169c58/_image_cache/69f4784a-7407-4947-9c0f-b3196ea89630/screenshot-percent202026-10-10-percent20at-percent.png)

I’m going to focus on a somewhat narrow set of options here, but there are more tools a studio could deploy in this arena that are more specific to their own game. Things like tests fall into general areas here, but also running things like fixed gameplay scenarios to figure out balance impact, etc. These all matter, but are game specific and as such hard to directly extract in the scope of this post.

However, one thing that all games share is a concern for performance. Traditionally, performance profiling has been a relatively manual effort, and required developers to work with profiling tools, with ones built into editors like Unity/Unreal or through IDE tooling like dotMemory:

![Screenshot 2026-10-10 at 12.49.36 PM](https://cdn.blot.im/blog_bceb40b92fea4cb981fda87bd5169c58/_image_cache/a17eddb3-7777-46d8-97cb-36beb3c4befc/screenshot-percent202026-10-10-percent20at-percent.png)

These tools are designed for *human* consumption — they expect you to dig into the call stacks and graphs and such to find whatever bottlenecks you are looking for. Perhaps unsurprisingly then, it turns out AI doesn’t really like using tools like this: they aren’t legible to the agent (though worth saying Unity *is* doing a lot of work here to make the profiler better able to be read by agents!).

What AI really likes it to work with text (and lots of it), so profiling tools that are able to dump out raw captures as text REALLY help to accelerate AI iteration. There are a few options here, but the one I’ve grown to like the most is the (free!) tool [samply](https://github.com/mstange/samply):

![Screenshot 2026-10-10 at 12.53.50 PM](https://cdn.blot.im/blog_bceb40b92fea4cb981fda87bd5169c58/_image_cache/af1c3916-1028-4297-a2af-7a6a7548c818/screenshot-percent202026-10-10-percent20at-percent.png)

Samply can be hooked up to easily run against your own project, and dump capture output to console. An AI agent knows how to read the samply source and docs to help it figure out how to eval its own changes here, so the capture and iterate loop can be performed effectively autonomously. AI likes this!

## Bringing It Home

As we combine all these tools and technique together, something like this comes into view (and this is just one version!):

![Screenshot 2026-10-10 at 12.55.44 PM](https://cdn.blot.im/blog_bceb40b92fea4cb981fda87bd5169c58/_image_cache/202d266f-8279-4e82-b0a5-fd028bc0dce8/screenshot-percent202026-10-10-percent20at-percent.png)

With the data-oriented, binary-isolated, no-compile-needed version of development I’m sketching here, an agentic turn can run through this WHOLE loop in *seconds*. It makes iteration significantly faster than a baseline naiive approach, and at every step helps the AI to better understand your game and what you’re trying to do. Because the whole loop is efficient and fast, it can be run against *every* change, and not saved for only when it “matters”.

Reframing this a big, you can see what a game-driven harness looks like:

![Screenshot 2026-10-10 at 1.03.19 PM](https://cdn.blot.im/blog_bceb40b92fea4cb981fda87bd5169c58/_image_cache/c6bc005e-816c-40a3-af5f-db9971511533/screenshot-percent202026-10-10-percent20at-percent.png)

Spoilers: it’s your game! Any game is capable of all of this, meaning any game can easily be its own harness. Or, as we’ve said at the start:

![Screenshot 2026-10-10 at 1.04.16 PM](https://cdn.blot.im/blog_bceb40b92fea4cb981fda87bd5169c58/_image_cache/7dd5f7b6-ecfa-4172-b9d0-4b7294686402/screenshot-percent202026-10-10-percent20at-percent.png)

Thanks for reading!

## Postscript

It’s been interesting to write this after having presented it. I stand by a lot of what I’ve said, but am in the process of shipping a project where I’ve also used none of this. I think the scale I’m targeting here is really *larger* projects. The thing I’m shipping is very small, so I really didn’t need to do much for it in terms of any harness stuff.

However, as I pick up work on a larger game that I’m cooking, a lot of this is definitely relevant and I’m very thankful for all these tools.

If you apply strategies like the above to your own games, I’d love to hear about your experience and what worked/didn’t work! I’m really thinking about a lot in this space (obviously) as I continue to make games and tools, so am excited to hear what other people are doing.

Again, thanks for reading and talk next time!

Published on October 10, 2026.
