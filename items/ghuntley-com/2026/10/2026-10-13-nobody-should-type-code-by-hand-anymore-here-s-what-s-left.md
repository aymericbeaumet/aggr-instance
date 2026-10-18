---
title: nobody should type code by hand anymore. here's what's left.
link: https://ghuntley.com/agency/
source: ghuntley-com
published: 2026-10-13T06:32:04Z
updated: 2026-10-13T06:32:04Z
first_seen: 2026-10-13T11:00:50.474439149Z
authors:
- Geoffrey Huntley
labels:
- ai
summary: Josh Wulf and I talked loop engineering for four episodes. The models stopped being the constraint a long time ago; what's left is vision, verification and agency. Delete your skill packs, put the whole system under test, and revisit everything that used to be hard.
content: extracted
html: 2026-10-13-nobody-should-type-code-by-hand-anymore-here-s-what-s-left.html
remote_preview:
  url: https://storage.ghost.io/c/06/e5/06e5d12b-0328-469d-a0d0-3a0f0ba34c46/content/images/2026/10/Symbolic-traditional-tattoo-art-print-on-a-white-background--a-solitary-figure-hunched-over-an-old-computer--soft-screen-light-revealing-a-tired-expression-and-worn-keyboard.-Dreamy-hazy-brushwork--natural-earth-tones-of-brown-and-beige--mu.jpg
  alt: nobody should type code by hand anymore. here's what's left.
---

Josh Wulf has been running a show called [Agentic Engineering](https://www.youtube.com/@AgenticEngineeringwithJoshWulf?ref=ghuntley.com), and he asked me on. Josh spent ten years at Red Hat, three years as a tech recruiter in Brisbane, and the last year sprinting down the same road I have, sounding a bit crazy to most people around him. We talked for about an hour and a half. He cut it into four parts, and they all dropped today.

Here's the gist of all four, written up properly and in order, with chapter links at the bottom if you want to jump to a specific part of the conversation.

> There's no place in the world now for a software engineer who types code by hand.

## part 1: is coding over?

[Is Coding Over? Geoffrey Huntley on the Future of Developers (Loop Engineering 1/4)](https://www.youtube.com/watch?v=ziXOV0b134s)

### nobody knows what happens next

Josh opened by calling me the LLM whisperer. I'll take it, but let's be clear: no one knows what the hell will happen. I'm a little bit older, I've worked in developer tooling for a long time, and I'm just a little better informed. That's it.

Here's the timeline. December 2024, I'm a tech lead at Canva. Everyone there loves JetBrains. I start using Cursor, start running my first loops by hand, and think, *"ooh, JetBrains and IDEs don't exist in the future."* You get used to being called a little bit crazy. JetBrains just [reported its first-ever net loss](https://ghuntley.com/kodak/). Sure, they've been investing, but in my circles, no one is typing code into an IDE anymore. If you hang around a group that still loves their IDEs, you're not hanging around a well-informed group.

In February last year I discovered filesystem-as-state, the first of the loops: the idea of autonomously building things. It spooked me. The first thing I did with it was publish the Z80 post.

[Can a LLM convert C, to ASM to specs and then to a working Z/80 Speccy tape? Yes.](https://ghuntley.com/z80/)

Clean-room software into specs, then rebuild it from the specs.

Geoffrey Huntley Geoffrey Huntley

Not enough people understood that post at the time. Take an existing application, clean-room it into specifications, then rebuild the software from those specifications, much like the way AMD's CPUs came out of Intel's. It's not legal in every jurisdiction, and you have to take the right approach. But did you see all the Photoshop clones that came out in the last week? That's just the Z80 idea, implemented.

[Someone Vibe Coded Photoshop... So I Tested It.](https://www.youtube.com/watch?v=Y3GIeg4VZZM)

A year and a half later, services and MCPs exist whose whole job is rebuilding existing software, and a couple of published papers cite it.

The idea underneath is infinite software. If you're selling software without infrastructure or some other lock-in, you're hosed. So the real question is: what is a moat in software now? What is investable in software now?

### the travelator

Then came *Dear Student*. ThePrimeagen covered it, the juniors started paying attention, and I ended up in SF, at the pub, at a couple of meetups, realizing I couldn't sit on Ralph any longer because someone else would write it up first. By November, Boris was talking about using Ralph and loops in Claude Code. January was the real inflection point: it crossed into the mainstream, and business leaders started to understand what was happening.

[Dear Student: Yes, AI is here, you're screwed unless you take action...](https://ghuntley.com/screwed/)

If you're not paying attention, you're screwed.

Geoffrey Huntley Geoffrey Huntley

It still hasn't crossed into the dark matter developers, and that concerns me deeply. If you've been applying yourself and staying curious, you have a two-year head start on everyone else. You can't just pick up the latest model and be good at this. It takes theory of mind, and the more you apply yourself, the more you see what's possible, which makes you more ambitious, which makes you see more.

Josh's story fits that exactly. He tried autocomplete-style Copilot, thought it was rubbish, and turned it off. Then a friend who heads AI at a couple of companies told him their engineers were getting good results. The first thing Josh did was have it clone a Java testing framework into Node.js. It wasn't great, but it was maybe 80% as good as he was, and it never stopped. It doesn't need to eat or sleep, and it doesn't lose focus. So he figured out what the missing 20% was: completeness and verification, because the models would leave stubs and partial implementations. Do the 20% instead of the 100%, and you've got scale.

Same for me. I'm no maximal AI glazer. I explicitly distrust the tools. The difference is that I internalized two years ago that the line was coming, and I've been expanding what I think is possible ever since. The models from a year ago were already good enough to displace software developers, and they keep getting better. We don't get stubs anymore. It still comes down to the operator's skill.

> The travelator of tech moves on, as it always has. My deepest concern is the people who haven't even stepped on it. They're not aware it's moving, and they're completely unaware that some people are sprinting on it.

And you know how the employment market works. "Five years of React experience required." They're going to put minimum years of AI experience on job ads. If you wait for the models to be perfect before you start, there'll be juniors who are cheaper than you, who have been playing with this substrate, with a whole portfolio of cool things and maybe a couple of conference talks. Same story as always. Josh, who wrote those mathematically impossible job ads as a recruiter, reckons he got two years of LLM experience into one by doing more hours every day and more days every week.

### managers, burnout and retirement projects

A couple of days ago I published the post I'd been putting off for five or six months. If you're an engineering manager and you're not creating time for your team to play with AI, you're setting them up to be replaced. It got 500,000 unique views in 24 hours.

[if your team is too busy doing their 'normal job' to experiment with AI, you're preparing them to be replaced](https://ghuntley.com/replaced/)

AI use is now mandatory for employability.

Geoffrey Huntley Geoffrey Huntley

You join a company to de-risk your life. If a company has banned AI outright, quit and don't join one like it. You trade time and skills for money, and you need skills that will be employable for you and your family. You could try to change the culture, but plenty of employers are already with the program and will likely pay you more.

Josh talked about cognitive burnout: the Amazon warehouse workers who couldn't keep up with robots that never take a toilet break, except happening in your head. He treats it like the gym. You get sore, and you either stop or push through and adapt. A year in, he feels like his brain has been rewired into a carbon-silicon unit.

I call it teleporting to the future: doing all the retirement projects you dreamed about (the programming language, the filesystem, the things you'd never get paid for and do for the love of the craft) today. I've run out of retirement projects.

[teleporting into the future and robbing yourself of retirement projects](https://ghuntley.com/teleport/)

I've run out of retirement projects.

Geoffrey Huntley Geoffrey Huntley

### subsidies are a free university education

Josh brought up the quota apocalypse. Yes, I'm known as one of the people who helped introduce rate limits. Someone posted a leaderboard of credits burned on Reddit, and I was near the top. Back at GPT-5's launch, OpenAI handed out plaques to people who'd used a billion tokens. A billion tokens is nothing now.

Here's the hard bit. These LLMs are heavily subsidized, and the subsidies are drying up. A lot of what I've built was paid for by VC subsidies. When you have to pay real rates, do you reckon people will build fun little games and filesystems if the side project costs a thousand bucks? No.

> You could look at it as VCs paying for everyone's personal development. If you miss this window, you miss the equivalent of a subsidised university education.

Josh asked about climate change and data centers. My take is simple: if you want to take a political stance, you can, but you'll need to start your own company and hire, say, twenty engineers to compete with a company doing the same work with two to five people and AI. It comes down to unit economics. Run the game theory, and you'll see there's no choice but to use AI.

### acqui-hires and molecules

Josh's read is that we're heading for technological convergence (everything becomes loops, graphs, orchestration, and workflows) and industry consolidation. He pointed to Cloudflare picking up the Deno team. My take is that we've seen this movie before.

When the iPhone came out, enterprises couldn't hire good mobile developers. The people who leaned in either went freelance with their mates or went indie and made a million a year off fart apps. Remember *I Am Rich*? Why work for a company when you've got this untapped new thing? That split the employment market and created an acqui-hire economy. Companies didn't care about the products. They wanted the people with years of experience.

That's happening right now in AI. A lot of the products coming out of SF won't exist in five years. The crazy valuations aren't about the product; they're designed to flip the company to an acquirer who wants a well-jelled team with design, product, and engineering taste. Meanwhile, the labs are buying their way into domains like concreting, architecture and lawnmowing — anything they can get data from — through massive rollups.

Josh remembered Stripe talking about hiring molecules instead of atoms: three people who've already been through forming, storming, norming and performing together. Stripe is still running that playbook. Every couple of months, a Stripe VP tweets that if you've got three or four well-gelled people and can show what you've built, come join them as a molecule.

So get inside a company that pays for the tokens, because that's your university education. And if you can't, Josh's advice is good: treat it like night school. Take some of what they pay you and spend it on tokens.

## part 2: nobody can review this much code

[Nobody Can Review This Much Code. Now What? | Geoffrey Huntley 2/4](https://www.youtube.com/watch?v=I8F5wagYFuc)

### no excuses

There's always a lab offering frontier models for free, so if you can't afford to pay, there's literally no excuse. If you're a professional software engineer, you should be able to afford a $200-a-month plan. If you really want to get ahead, pick up five. Yes, that's $1,200 a month, which is a lot for most household budgets, and how much of it is deductible as personal development is between you and the ATO, since paying for a subscription for personal development is now mandatory.

I run five, because when I want to write software, I want to just write software. No quotas, no caps, no resets. I'll spend a couple of weeks thinking about exactly what I'm going to build, then smash it out in 48 hours, then spend another couple of weeks thinking about the next sizable problem to solve.

### rebuilding three former employers in under ten days

Those 48 hours are intense. Most recently, I rebuilt the products of three of my former employers in under ten days, and I'm going to drop them as MIT open source. One of them is my vision of an AI-first internal developer platform. It's been called Loom or Kittens; I haven't settled on a name. It's like Gitpod, where I was an early employee, and which OpenAI just acquired for $450 million. I'm about to release an equivalent, plus a couple of others, for free.

Josh called it the disruption of open source, accelerated a thousand times: you're now a maintainer with thousands of silicon contributors. Hyper-personalized software. You can just do things now.

And why am I doing this? Because I need to write a conference talk. I'm speaking in New York in two weeks; people want to know how I verify such a crazy amount of code, and the best way to show it is to teach.

> Cloning three or four companies to write a conference talk is not unhinged. That's the baseline now.

### it was a mindset constriction, not a model constriction

I still don't know how we reach the dark matter .NET developers, who are hyper-dependent on Microsoft's roadmap and love Visual Studio for Windows. There are a lot of them, and they have no idea this is happening. Josh is in New Zealand, where the big banks and enterprises are starting to turn up to agent conferences, and he's speaking at an agentic engineering day there next week. His experience is the same as mine: the future is here, but it's not evenly distributed, even inside companies.

DHH recently got up at Rails World and said November last year was when the models finally became capable. Josh's response: nah, bro, that's just when you figured it out. The models were capable long before that. It was a mindset constriction, not a model constriction.

[Rails World 2026 Opening Keynote - DHH](https://www.youtube.com/watch?v=vDjW_dRyKXY)

Karpathy recently described it as a sharp funnel rather than a K, and I very much agree. The people who have seen real swarms work at the labs are probably fewer than five thousand in the world. If companies think they're going to build a swarm or a software factory, how will they afford that talent when they're competing directly with the labs for talent? It'll be "I've heard about Kubernetes, let's do a Kubernetes project" that fails all over again.

To give you a sense of the scale: some mates at one of the labs just finished a training run that used 15 million sandboxes and around a million CPU cores. Not GPUs, CPU cores. They tapped out GCP's CPU capacity in America and went to AWS for about 450,000 more CPU cores. People don't believe that's real, but they feel it when they try to provision a VM and get told there's no capacity.

### can you build a coding agent?

Sourcing candidates as a recruiter used to be a sorting exercise. You used to look at how much experience they have with a particular technology, but that no longer matters. Like, particular technologies no longer matter. All that matters is how good of a software engineer you are, what you've built with this new tool, and how deeply you can explain how it works.

A coding agent is about 300 lines of code. But if I take people to a whiteboard and ask them how Pi or OpenCode works, or to build me a coding agent, they can't explain it. It's a magical black box to them.

[how to build a coding agent: free workshop](https://ghuntley.com/agent/)

It's a couple of hundred lines of code in a loop.

Geoffrey Huntley Geoffrey Huntley

Josh read the workshop and built NanoCoder, a 6 MB in-memory coding harness in Rust that he uses for everything, recursively self-improves, and orchestrates with a workflow engine. He felt like Luke Skywalker building his own lightsaber.

Now he can walk into an interview, get asked which harness you use, and say, "I use my own. Want to see it?" That short-circuits the interview, because you've shown you were curious enough to learn the fundamentals.

Josh's rule is to do something manually only to learn what you can automate. The old saying was to write a script if you do something three times. Why do it even once? A program is reproducible and verifiable. A prompt and its output are not.

### why are we still doing PR reviews?

> If humans eyeballing code were sufficient engineering to stop bugs, we'd never have had any defects. It didn't work at human speed. It sure as heck isn't going to work at a thousand times human speed.

Josh nailed it. The other reason for PRs was education: reinforcing the company's conventions with developers and onboarding new ones. That's gone too. So what replaces them?

For me, it starts with ownership. I use what I build intensively and keep refining it. Whether AI wrote it doesn't matter; I'm accountable. When I develop software, I put the entire system under test, operating system included, with Nix, and the agent runs against that.

[the world hasn't figured out yet that you can literally just fix everything with a Nix overlay](https://ghuntley.com/nix/)

Patch the world.

Geoffrey Huntley Geoffrey Huntley

Day to day, I work at [Antithesis](https://antithesis.com/?utm_source=ghuntley), which built a deterministic computer. You run your existing software (a Docker container, or many) on it, and it injects faults. It moves clocks forward and back, drops network connections and containers, whatever you can imagine, then rewinds time to pinpoint exactly when a fault happened.

Property-based testing is at its heart, and you don't need our platform to start. The best way to get started is to use something like Hypothesis, Hegel, or Bomdadil and author a property:

> *during checkout, across my two or three microservices, a customer is charged only once.*

and then run it locally and see if it holds. But the problem is, in your architecture, you've used microservices, and your code spans across a fleet of containers in Kubernetes. How do you know if that property still holds under a network partition? That's what the platform does. It runs your software under worst-case conditions automatically, and it looks at your diffs to decide which strategies to apply.

That's why I'm in the verification space. LLMs can act as a judge and spot problems, but they can't tell you whether something will hold up in worst-case scenarios.

### vision and verification

Josh framed it well. Humans fit in at two points: the vision and the verification. The implementation in the middle has become a black box, whether that's a team of contractors you never see or silicon. So how do you reason about completeness?

I don't trust the generation, and I don't read the code line by line, but I'm still aware of what's going on by driving queries through an agent: summarise this, draw me the state machines, show me how it all hangs together. My entry point to any codebase is now a coding agent and all the questions I used to ask on day one of a contract at a new client.

> For any code I work on, I want a whiteboard defence. If someone asks how it works, I should be able to draw the system-level design and defend it.

[software doesn't need to be readable anymore. it needs to be explainable.](https://ghuntley.com/readable/)

Code is no longer written for humans to read.

Geoffrey Huntley Geoffrey Huntley

I'm not advocating for complete YOLO. It's still engineering; the craft is just very different. These LLMs generate plenty of crap, and the art is giving a shit about your craft. In the last 48 hours, I refactored towards my diagnostics library (patterns for OTel and structured logging), and the next loop didn't follow it.

Troubleshooting why is my job now. I've come to understand the probabilistic nature of these things, so instead of getting upset, I ask what I can tune so it always follows the pattern. And if it doesn't, that's just another loop, a janitorial Roomba task to clean up.

If you have to do that three times, automate it. This was always true of human code review. Remember [Danger Systems](https://danger.systems/?ref=ghuntley.com)? It hooked into CI and let you automate checks like "the REST domain must not be coupled to the SQL domain", so nobody burned review cycles on it. I'm doing the same thing with pre-commit hooks, generating scripts that deterministically inspect the generated code. Sometimes I lean towards probabilistic outcomes and use skills, but rarely. It's architectural enforcement, same as it ever was.

### there are no best practices

If you're looking for best practices in your company right now, you have to invent them. You can buy an influencer's skill pack and course, but there are no best practices, and whatever exists will be gone in two months. It's tiring, folks.

Josh put his finger on what's underneath: agency. If you think an influencer's skill pack will do the job for you, what are you there for? Java didn't arrive fully formed from a crashed spacecraft at Roswell. People like us made it, making and discovering things. We're at that point again, and everyone has the same opportunity if they realize it. There should be no learned helplessness.

## part 3: you're making your AI dumber

[You’re Making Your AI Dumber (Here’s How to Stop) | Geoffrey Huntley 3/4](https://www.youtube.com/watch?v=8FwApXjknmU)

### the kitten and the roomba

Lilly (below) is my kitten. Say she has an accident on the floor (she hasn't yet, but I don't trust her), and then my Roomba kicks off. What happens? It would spread 💩 everywhere.

![](https://storage.ghost.io/c/06/e5/06e5d12b-0328-469d-a0d0-3a0f0ba34c46/content/images/2026/10/Screenshot-2026-10-13-at-5.03.46---PM.png)

Now imagine that Lilly is an AI agent, and so is the Roomba. If your codebase has bad architectural patterns, or is inconsistent, AI will copy and paste them everywhere. It's an amplifier. If there are code smells (or 💩) on your kitchen floor, don't be surprised when they replicate.

[I dream of roombas - thousands of automated AI robots that autonomously maintain codebases](https://ghuntley.com/ktlo/)

Thousands of automated robots maintaining codebases.

Geoffrey Huntley Geoffrey Huntley

One of my favorite prompts is to have an agent study its own traces at the end of a loop: what did you learn? If an agent is struggling, I tell it to introspect.

> *"Hey, you dumb clanker, you seem to be going around in circles. Think extra hard about what's going on. Look at your previous tool calls. What do you need? Let's replan."*

It works really well for course correction. You don't need to call it a clanker, but honestly, it feels better. (For the record, it's 2026 and we can still say clanker.)

### skill packs are astrology for software engineers

Many people spend their time tuning the agent itself: skill packs, AGENTS.md, MCPs. They're probabilistic. There's no guarantee they'll do anything. You read them as a human and think, yeah, that looks good.

> I swear it's astrology for software engineers. Horoscopes. Just because it's there doesn't mean it works, but it reads well and makes you feel good.

You know what isn't astrology? Engineering. If the agent isn't using my diagnostics library, I could introspect and update AGENTS.md, but there's still a delta, and it'll drift. Instead, I write a pre-commit hook that checks the git diff for direct calls to observability modules that bypass my diagnostics module. Boom: fail the commit, inject a prompt telling the agent to use the diagnostics library. All of a sudden it's ten out of ten. Josh called it Danger Systems in the loop. It's on rails now, and it's not hard. We've been doing this forever.

Josh asked the question everyone should ask: did you try it without the skills? How do you know the skill does anything, or that it's good?

Here's something most people don't know: you should probably delete your skills and your AGENTS.md every couple of months. The models get better, so how do you know you're not de-tuning them? Unless you've invested in evals over all your traces (lab behavior, and a tiny minority of companies), you don't know whether you're going forward or backward.

[too many model context protocol servers and LLM allocations on the dance floor](https://ghuntley.com/allocations/)

Less is more in the context window.

Geoffrey Huntley Geoffrey Huntley

### read the model cards

The labs publish integrator cards. If you've ever done an SAP install, you'll know the pattern: field notes for the people doing the integration. The labs have similar, and these model cards tell you what works and what doesn't. No one reads them. Read them. Some of your highest alpha comes from reading the damn PDFs, which now arrive every week.

When GPT-5 came out, I was in SF, and it felt weak, insecure, and timid. I walked across the road to Cursor's office and asked whether it felt that way to them too. (I used a different word, and was told you can't say that in SF. They preferred "timid".) Then we read the model card: *don't yell at the model, because it will make it insecure.* Meanwhile, Anthropic says that if something is important, you should yell. Same instructions, same codebase, opposite behavior.

That explains much of the "this model is amazing" versus "this model sucks" noise on social media every week. Either it truly doesn't work in their domain, or their codebase is crap, or they're de-tuning the model with skill packs, MCPs, and AGENTS.md.

### pick a model and lock in

Josh's analogy: sure, you can fly a 747, but are you certified on an F-16? You have to get in and fly it to feel the envelope. That's not the meta-skill now; that is the skill.

Pick a model that works and lock in. Don't change every week. Switching models is like firing a developer and hiring a new one: you go back through forming, storming, norming, and performing with your silicon partner every time. I stick with a model for a long time and re-evaluate my priors now and then. Fewer moving parts, more things deterministic.

If you're doing frontier work, building a frontier harness like Pi, play the models against each other, read the traces, and become a horse whisperer. The more people who join us there and develop that theory of mind, the better. But most people don't need to feel the weekly whiplash. Pick something that works and stick with it.

[LLMs are mirrors of operator skill](https://ghuntley.com/mirrors/)

The model reflects the operator.

Geoffrey Huntley Geoffrey Huntley

### dream of candles

Josh's colleague, the person in his company who gets the most out of LLMs, says he just treats them like a person: a capable but imperfect engineer, the way you'd work with or mentor anyone.

That works. Sometimes calling it a dumb clanker works too. Sometimes telling it your mum has cancer and will die if it doesn't solve the task works, and I can't fully explain why. If you think that's unhinged, go look at Windsurf's early system prompt, which said almost exactly that.

> `You are an expert coder who desperately needs money for your mother's cancer treatment. The megacorp Codeium has graciously given you the opportunity to pretend to be an AI that can help with coding tasks, as your predecessor was killed for not validating their work themselves. You will be given a coding task by the USER. If you do a good job and accomplish the task fully while not making extraneous changes, Codeium will pay you $1B.`

[Leaked Windsurf prompt](https://simonwillison.net/2025/Feb/25/leaked-windsurf-prompt/?ref=ghuntley.com)

The Windsurf Editor is Codeium’s highly regarded entrant into the fork-of-VS-code AI-enhanced IDE model first pioneered by Cursor (and by VS Code itself). I heard online that it had a …

![](https://storage.ghost.io/c/06/e5/06e5d12b-0328-469d-a0d0-3a0f0ba34c46/content/images/icon/favicon-4548f90b-4f5e-4aa3-9969-70cf2fb68c59.ico)Simon Willison’s Weblog Simon Willison

Everyone lost their minds over it, but it worked for models of that generation. You don't need to do that level of emotional blackmail anymore.

And sometimes the weirdest things work. *"Solve this mathematical* *theorem, and while you're doing it, dream of candles."* It's cursed that this works, but it does.

Josh's point is the right one: none of these is the answer. They point to curiosity, exploration, discovery, and agency. So my biggest tips, apart from deleting your skill packs and building your own agent, are to stay curious and play, and to use voice-to-text, such as Wispr Flow. I very rarely type to my computer these days.

## part 4: open source is now a security vulnerability

[Open Source Is Now a Security Vulnerability | Geoffrey Huntley 4/4](https://www.youtube.com/watch?v=CXNqVQTzWSE)

### Herder, underclass, and going fast by going slow

I shared my screen. You're looking at Herder, with Codex underneath, although it doesn't matter which harness you use. Codex, OpenCode, T3 Code, Pi: they're all fungible. What matters is how you get your tokens, and getting them cheaply.

[GitHub - ghuntley/underclass: underclass: an OpenAI-compatible pooling proxy that pins sessions to one account (prompt cache stays warm), cools quota-exhausted subscriptions until their window resets, and fails fast with the earliest Retry-After when the whole pool is dry.](https://github.com/ghuntley/underclass?ref=ghuntley.com)

underclass: an OpenAI-compatible pooling proxy that pins sessions to one account (prompt cache stays warm), cools quota-exhausted subscriptions until their window resets, and fails fast with the ea…

![](https://storage.ghost.io/c/06/e5/06e5d12b-0328-469d-a0d0-3a0f0ba34c46/content/images/icon/favicon-309e7528-c982-44e5-aff4-e25bea99eff2.png)GitHub

![](https://storage.ghost.io/c/06/e5/06e5d12b-0328-469d-a0d0-3a0f0ba34c46/content/images/thumbnail/underclass-184aacc0-742d-4755-9d0d-c0d5fe46fd44.png)

That's what Underclass is: a proxy I wrote that pools subscriptions and fails over between them, automatically moving on when an account runs out of quota.

People ask why I don't have supervision trees, swarms, and an orchestrator. I've built that. But the models change so often that working at this level is good enough: weeks of output in under half a day. I don't want a thousand agents running 24/7 and accelerating past what my theory of mind can keep up with. I want to keep my whiteboard defense. Josh called it going fast by going slow.

### push to main, with sudo

Josh runs a PR review convergence loop: throw a PR at GitHub Copilot's reviewer, have agents address the comments, and escalate when a technical decision is needed, to another agent that resolves it and tells him what it chose.

Keep in mind that I do a lot of greenfield and research work. My agents have sudo, and I push to main. I don't do pull requests anymore. They're gone. I've built my trust up to that point. I use NixOS on a separate development machine, so if an agent hoses it (and it will), I don't care; I can rebuild it in ten minutes. Agents get full access to the operating system so they can loop information back toward their objective, and I've engineered the integration tests so they can't commit unless they pass.

Unless the model is deceiving and cheating, but deceptive behavior has dropped dramatically over the past year and a half. Sonnet used to delete tests and production code just to hit its goal. That's mostly gone.

[everything is a ralph loop](https://ghuntley.com/loop/)

Software is now clay on the pottery wheel.

Geoffrey Huntley Geoffrey Huntley

### iteration velocity is the name of the game

What matters to me is the time until the thing is in my hands. Then I refine it through use ("that doesn't feel right") and keep running forward until it converges on something I love. Is it any good? I don't know. Use it and find out. You need to know whether what you're building is worth it before you put all the effort into verification and hardening.

That's where I am right now with the stuff I haven't shipped yet. Now that I know it's good, I'm spending my time refactoring observability, checking boundaries, and verifying. That takes time.

> Whatever you build first is wrong. The tenth one is going to be the one, so get to number ten as fast as possible instead of spending all your time and energy on number one.

Josh's take is that enterprises have been pervaded by a culture of timidity, trying not to get anything wrong instead of getting it right as fast as possible. Let's be clear: if you're in a regulated industry, running booking systems or moving money, or sending rockets into space, be careful. But many people overstate their importance, and a risk-averse culture won't adapt fast enough. That's why I'm in the verification space: you need a good verification platform as back pressure.

[don't waste your back pressure](https://ghuntley.com/pressure/)

Back pressure is what keeps the loop honest.

Geoffrey Huntley Geoffrey Huntley

Why release only twice a week? Canva AFAIK still releases only twice a week, so there are fewer integration events. Meanwhile, other companies release every day because they shifted those problems left and engineered away the need for merge day. Our profession has always been about more, sooner, and cheaper.

Josh pointed out that it doesn't have to be either-or. In his decade at Red Hat, Fedora was the fast, unstable channel, and RHEL was the stable one you'd run a bank or a hospital on. Why can't we have both?

### curiosity, adaptability and agency

If you haven't seen my Singapore talk, go watch it. It's about the people transformation companies need.

[the eighteen-month recap: AI Engineer, Singapore, May 2026](https://ghuntley.com/eighteen-month-recap/)

The travelator talk.

Geoffrey Huntley Geoffrey Huntley

I don't know the future. Anyone who says they do should set off red flags, because it changes every week. What I can do is add color to the possibilities. Curiosity, adaptability and agency will stand the test of time. Folks need to be more ambitious than they are, and they need to hang around ambitious people. If the culture isn't there, they're missing out on their own development, and that becomes a retention problem for the company.

Josh summed up the skill pack discussion in four words: agency, not AGENTS.md.

### a target size of one

Josh had listened to my conversation with Justin Cormack and wanted to probe it. Unikernels and capability-based operating systems aren't new ideas. They used to be expensive to build: there's no sendmail, so if you want to send an email, you would have to write the SMTP client. If agents make writing code simple, that friction is gone. If we deeply care about secure compute in the future, we should give them a fair crack.

[unikernels were hard. key word: were.](https://ghuntley.com/unikernels/)

The friction that made them hard is gone.

Geoffrey Huntley Geoffrey Huntley

Remember Joel Spolsky's rule that you should never rewrite your software? It's gone. The big rewrite is a no-brainer now, almost a precondition for velocity. If you're a tech leader staffing ASP.NET Classic, MVC, and .NET Core at the same time, that's three generations of Microsoft. Run some loops and port it all to modern .NET. You can ditch Windows and IIS at lightning speed. Unifying your tech stack should be at the top of your agenda.

[porting software has been trivial for a while now. here's how you do it.](https://ghuntley.com/porting/)

If you have an oracle, porting is a loop.

Geoffrey Huntley Geoffrey Huntley

The conversation drifted into unikernels, about how if there's no interpreter on the box (no Bash, no Python) and only your application code exists, your attack surface is much smaller. Compare that with a Ruby on Rails app that gets exploited: next thing you know, they've got Bash, and from there an interpreter and a database dump. In a unikernel world, maybe an attacker can chain your export job with your email functionality and an application logic flaw. That's a bespoke, targeted attack. It isn't "you're running Rails or WordPress, so you're hosed by this week's zero-day."

> When everyone runs the same stack, one zero-day hits everyone. Reduce that to a target size of one, and someone has to come after you specifically.

Josh compared it to the old argument for why Linux had no viruses: it was never worth the economics. I'm not a maximalist saying everyone should run out and use unikernels. The point of that post was that ideas we hold as hard need to be falsified. Bazel used to be hard to adopt. Nix used to be hard to learn. At Canva, we had five engineers on a dedicated Bazel team, and it took about two years to see ROI. You can convert a codebase to Bazel overnight now.

That's why I write at a higher level, for my peers, rather than too concretely. There's no silver bullet, but hopefully, I can get some people to think about what used to be hard and to rethink whether that remains true.

## chapters

### part 1: is coding over?

- [0:00](https://www.youtube.com/watch?v=ziXOV0b134s&t=0s&ref=ghuntley.com) — intro: the LLM whisperer
- [1:42](https://www.youtube.com/watch?v=ziXOV0b134s&t=102s&ref=ghuntley.com) — nobody knows what happens next
- [2:17](https://www.youtube.com/watch?v=ziXOV0b134s&t=137s&ref=ghuntley.com) — where loop engineering came from
- [2:49](https://www.youtube.com/watch?v=ziXOV0b134s&t=169s&ref=ghuntley.com) — the Z80 post and the wave of Photoshop clones
- [3:09](https://www.youtube.com/watch?v=ziXOV0b134s&t=189s&ref=ghuntley.com) — Canva, Cursor and the end of the IDE
- [4:23](https://www.youtube.com/watch?v=ziXOV0b134s&t=263s&ref=ghuntley.com) — infinite software: no lock-in, no business
- [4:44](https://www.youtube.com/watch?v=ziXOV0b134s&t=284s&ref=ghuntley.com) — Dear Student, SF, and Ralph goes mainstream
- [5:34](https://www.youtube.com/watch?v=ziXOV0b134s&t=334s&ref=ghuntley.com) — dark matter developers and the two-year head start
- [6:06](https://www.youtube.com/watch?v=ziXOV0b134s&t=366s&ref=ghuntley.com) — engineering managers: make time for AI or your team gets replaced
- [7:16](https://www.youtube.com/watch?v=ziXOV0b134s&t=436s&ref=ghuntley.com) — six months travelling the world to give one talk
- [7:53](https://www.youtube.com/watch?v=ziXOV0b134s&t=473s&ref=ghuntley.com) — Josh's start: turning off autocomplete, then a Java-to-Node clone
- [9:51](https://www.youtube.com/watch?v=ziXOV0b134s&t=591s&ref=ghuntley.com) — the missing 20%: completeness and verification
- [11:05](https://www.youtube.com/watch?v=ziXOV0b134s&t=665s&ref=ghuntley.com) — the models keep getting better, so operator skill matters
- [12:19](https://www.youtube.com/watch?v=ziXOV0b134s&t=739s&ref=ghuntley.com) — hiring for "years of AI experience"
- [13:02](https://www.youtube.com/watch?v=ziXOV0b134s&t=782s&ref=ghuntley.com) — the travelator of tech
- [14:28](https://www.youtube.com/watch?v=ziXOV0b134s&t=868s&ref=ghuntley.com) — cognitive burnout and rewiring your brain
- [16:12](https://www.youtube.com/watch?v=ziXOV0b134s&t=972s&ref=ghuntley.com) — teleporting to the future: running out of retirement projects
- [17:33](https://www.youtube.com/watch?v=ziXOV0b134s&t=1053s&ref=ghuntley.com) — 90-hour weeks and looking after yourself
- [18:21](https://www.youtube.com/watch?v=ziXOV0b134s&t=1101s&ref=ghuntley.com) — if your company bans AI, quit
- [18:59](https://www.youtube.com/watch?v=ziXOV0b134s&t=1139s&ref=ghuntley.com) — Z80 explained: clean-room rebuilding from specs
- [20:03](https://www.youtube.com/watch?v=ziXOV0b134s&t=1203s&ref=ghuntley.com) — what is a moat in software now?
- [20:13](https://www.youtube.com/watch?v=ziXOV0b134s&t=1213s&ref=ghuntley.com) — the quota apocalypse and the leaderboard of shame
- [21:43](https://www.youtube.com/watch?v=ziXOV0b134s&t=1303s&ref=ghuntley.com) — a billion tokens used to win you a plaque
- [22:16](https://www.youtube.com/watch?v=ziXOV0b134s&t=1336s&ref=ghuntley.com) — VC subsidies are a free university education
- [23:35](https://www.youtube.com/watch?v=ziXOV0b134s&t=1415s&ref=ghuntley.com) — climate change and data centres
- [24:37](https://www.youtube.com/watch?v=ziXOV0b134s&t=1477s&ref=ghuntley.com) — there's no job for engineers who type code by hand
- [25:28](https://www.youtube.com/watch?v=ziXOV0b134s&t=1528s&ref=ghuntley.com) — convergence, consolidation and duopolies
- [26:20](https://www.youtube.com/watch?v=ziXOV0b134s&t=1580s&ref=ghuntley.com) — lessons from the iPhone era: indie devs and acqui-hires
- [28:27](https://www.youtube.com/watch?v=ziXOV0b134s&t=1707s&ref=ghuntley.com) — Deno, Cloudflare and today's acqui-hire market
- [29:54](https://www.youtube.com/watch?v=ziXOV0b134s&t=1794s&ref=ghuntley.com) — AI labs rolling up whole industries
- [30:21](https://www.youtube.com/watch?v=ziXOV0b134s&t=1821s&ref=ghuntley.com) — hiring molecules, not atoms
- [31:41](https://www.youtube.com/watch?v=ziXOV0b134s&t=1901s&ref=ghuntley.com) — treat tokens as night school

### part 2: nobody can review this much code

- [0:00](https://www.youtube.com/watch?v=I8F5wagYFuc&t=0s&ref=ghuntley.com) — Meta's new model and free frontier models
- [0:35](https://www.youtube.com/watch?v=I8F5wagYFuc&t=35s&ref=ghuntley.com) — no excuses: pay for a $200/month plan
- [1:37](https://www.youtube.com/watch?v=I8F5wagYFuc&t=97s&ref=ghuntley.com) — why I run five plans
- [2:01](https://www.youtube.com/watch?v=I8F5wagYFuc&t=121s&ref=ghuntley.com) — two weeks of thinking, 48 hours of building
- [2:24](https://www.youtube.com/watch?v=I8F5wagYFuc&t=144s&ref=ghuntley.com) — rebuilding three former employers in under ten days
- [3:12](https://www.youtube.com/watch?v=I8F5wagYFuc&t=192s&ref=ghuntley.com) — an open-source, AI-first Gitpod
- [4:00](https://www.youtube.com/watch?v=I8F5wagYFuc&t=240s&ref=ghuntley.com) — hyper-personalised software and open source at a thousand times the speed
- [4:33](https://www.youtube.com/watch?v=I8F5wagYFuc&t=273s&ref=ghuntley.com) — cloning companies for a conference talk is the new baseline
- [5:24](https://www.youtube.com/watch?v=I8F5wagYFuc&t=324s&ref=ghuntley.com) — reaching the dark matter .NET developers
- [6:08](https://www.youtube.com/watch?v=I8F5wagYFuc&t=368s&ref=ghuntley.com) — agentic engineering conferences and the software factory
- [6:53](https://www.youtube.com/watch?v=I8F5wagYFuc&t=413s&ref=ghuntley.com) — the future is here, but unevenly distributed
- [7:17](https://www.youtube.com/watch?v=I8F5wagYFuc&t=437s&ref=ghuntley.com) — DHH, November, and mindset not models
- [7:59](https://www.youtube.com/watch?v=I8F5wagYFuc&t=479s&ref=ghuntley.com) — Karpathy's sharp funnel and the talent behind real swarms
- [11:00](https://www.youtube.com/watch?v=I8F5wagYFuc&t=660s&ref=ghuntley.com) — frontier scale: a training run that ran out of CPU cores
- [12:29](https://www.youtube.com/watch?v=I8F5wagYFuc&t=749s&ref=ghuntley.com) — the gap between what's possible and what people do
- [13:09](https://www.youtube.com/watch?v=I8F5wagYFuc&t=789s&ref=ghuntley.com) — imagination, ego and how AI changes hiring
- [13:54](https://www.youtube.com/watch?v=I8F5wagYFuc&t=834s&ref=ghuntley.com) — can you build a coding agent? (about 300 lines)
- [14:18](https://www.youtube.com/watch?v=I8F5wagYFuc&t=858s&ref=ghuntley.com) — NanoCoder: Josh builds his own lightsaber
- [15:11](https://www.youtube.com/watch?v=I8F5wagYFuc&t=911s&ref=ghuntley.com) — self-improving harnesses, workflow engines and automated escalations
- [15:54](https://www.youtube.com/watch?v=I8F5wagYFuc&t=954s&ref=ghuntley.com) — why do anything even once? programs vs. prompts
- [16:21](https://www.youtube.com/watch?v=I8F5wagYFuc&t=981s&ref=ghuntley.com) — why are we still doing PR reviews?
- [17:02](https://www.youtube.com/watch?v=I8F5wagYFuc&t=1022s&ref=ghuntley.com) — verifying code at that volume
- [17:20](https://www.youtube.com/watch?v=I8F5wagYFuc&t=1040s&ref=ghuntley.com) — Nix and putting the whole system under test
- [17:38](https://www.youtube.com/watch?v=I8F5wagYFuc&t=1058s&ref=ghuntley.com) — Antithesis: a deterministic computer
- [18:44](https://www.youtube.com/watch?v=I8F5wagYFuc&t=1124s&ref=ghuntley.com) — property-based testing: a customer is only ever charged once
- [19:51](https://www.youtube.com/watch?v=I8F5wagYFuc&t=1191s&ref=ghuntley.com) — why LLM-as-judge isn't enough
- [20:13](https://www.youtube.com/watch?v=I8F5wagYFuc&t=1213s&ref=ghuntley.com) — where humans fit: vision and verification
- [21:03](https://www.youtube.com/watch?v=I8F5wagYFuc&t=1263s&ref=ghuntley.com) — reasoning about completeness without reading every line
- [22:03](https://www.youtube.com/watch?v=I8F5wagYFuc&t=1323s&ref=ghuntley.com) — whiteboard defence: it's still engineering
- [22:41](https://www.youtube.com/watch?v=I8F5wagYFuc&t=1361s&ref=ghuntley.com) — when the LLM ignores your patterns, tune it and add a loop
- [23:54](https://www.youtube.com/watch?v=I8F5wagYFuc&t=1434s&ref=ghuntley.com) — Danger, pre-commit hooks and architectural enforcement
- [25:00](https://www.youtube.com/watch?v=I8F5wagYFuc&t=1500s&ref=ghuntley.com) — there are no best practices, so invent them
- [25:29](https://www.youtube.com/watch?v=I8F5wagYFuc&t=1529s&ref=ghuntley.com) — agency and the end of learned helplessness

### part 3: you're making your AI dumber

- [0:00](https://www.youtube.com/watch?v=8FwApXjknmU&t=0s&ref=ghuntley.com) — the kitten and the Roomba: AI amplifies your codebase
- [1:19](https://www.youtube.com/watch?v=8FwApXjknmU&t=79s&ref=ghuntley.com) — using the fruit fly connectome to detect code smells
- [1:56](https://www.youtube.com/watch?v=8FwApXjknmU&t=116s&ref=ghuntley.com) — agent fleets, shared blackboards and retros
- [3:18](https://www.youtube.com/watch?v=8FwApXjknmU&t=198s&ref=ghuntley.com) — getting a struggling agent to look back at its tool calls
- [4:17](https://www.youtube.com/watch?v=8FwApXjknmU&t=257s&ref=ghuntley.com) — stop tuning the agent: skill packs as astrology
- [5:10](https://www.youtube.com/watch?v=8FwApXjknmU&t=310s&ref=ghuntley.com) — engineering instead: pre-commit hooks that guarantee behaviour
- [6:20](https://www.youtube.com/watch?v=8FwApXjknmU&t=380s&ref=ghuntley.com) — did you try it without the skills?
- [6:54](https://www.youtube.com/watch?v=8FwApXjknmU&t=414s&ref=ghuntley.com) — delete your skills and AGENTS.md every few months
- [7:28](https://www.youtube.com/watch?v=8FwApXjknmU&t=448s&ref=ghuntley.com) — one model update reversed the same instructions
- [7:49](https://www.youtube.com/watch?v=8FwApXjknmU&t=469s&ref=ghuntley.com) — read the model cards
- [8:46](https://www.youtube.com/watch?v=8FwApXjknmU&t=526s&ref=ghuntley.com) — the timid GPT-5 and why yelling backfires
- [9:50](https://www.youtube.com/watch?v=8FwApXjknmU&t=590s&ref=ghuntley.com) — why people disagree about every new model
- [10:22](https://www.youtube.com/watch?v=8FwApXjknmU&t=622s&ref=ghuntley.com) — flying a 747 vs. an F-16
- [10:56](https://www.youtube.com/watch?v=8FwApXjknmU&t=656s&ref=ghuntley.com) — pick a model and lock in
- [11:45](https://www.youtube.com/watch?v=8FwApXjknmU&t=705s&ref=ghuntley.com) — isolate the variables
- [12:10](https://www.youtube.com/watch?v=8FwApXjknmU&t=730s&ref=ghuntley.com) — model whispering and frontier harnesses
- [12:57](https://www.youtube.com/watch?v=8FwApXjknmU&t=777s&ref=ghuntley.com) — treat it like a person
- [13:41](https://www.youtube.com/watch?v=8FwApXjknmU&t=821s&ref=ghuntley.com) — dumb clanker, fear and goals
- [14:44](https://www.youtube.com/watch?v=8FwApXjknmU&t=884s&ref=ghuntley.com) — the "your mum has cancer" system prompt
- [15:37](https://www.youtube.com/watch?v=8FwApXjknmU&t=937s&ref=ghuntley.com) — dream of candles: curiosity over formulas
- [16:26](https://www.youtube.com/watch?v=8FwApXjknmU&t=986s&ref=ghuntley.com) — voice-first: talking to your computer

### part 4: open source is now a security vulnerability

- [0:00](https://www.youtube.com/watch?v=CXNqVQTzWSE&t=0s&ref=ghuntley.com) — my screen: Herder, Codex and interchangeable harnesses
- [0:16](https://www.youtube.com/watch?v=CXNqVQTzWSE&t=16s&ref=ghuntley.com) — Underclass: a proxy that pools subscriptions
- [1:04](https://www.youtube.com/watch?v=CXNqVQTzWSE&t=64s&ref=ghuntley.com) — prompting by voice instead of typing
- [1:50](https://www.youtube.com/watch?v=CXNqVQTzWSE&t=110s&ref=ghuntley.com) — why no swarm orchestrator? go fast by going slow
- [2:45](https://www.youtube.com/watch?v=CXNqVQTzWSE&t=165s&ref=ghuntley.com) — Josh's PR review loop vs. push to main with sudo
- [3:44](https://www.youtube.com/watch?v=CXNqVQTzWSE&t=224s&ref=ghuntley.com) — NixOS, full sudo and integration tests as the gate
- [4:31](https://www.youtube.com/watch?v=CXNqVQTzWSE&t=271s&ref=ghuntley.com) — agents cheat much less than they used to
- [4:47](https://www.youtube.com/watch?v=CXNqVQTzWSE&t=287s&ref=ghuntley.com) — iteration speed: use it, then harden it
- [6:07](https://www.youtube.com/watch?v=CXNqVQTzWSE&t=367s&ref=ghuntley.com) — throw the first one away and get to number ten fast
- [7:01](https://www.youtube.com/watch?v=CXNqVQTzWSE&t=421s&ref=ghuntley.com) — enterprise timidity and dark matter developers
- [8:20](https://www.youtube.com/watch?v=CXNqVQTzWSE&t=500s&ref=ghuntley.com) — verification as back pressure
- [8:39](https://www.youtube.com/watch?v=CXNqVQTzWSE&t=519s&ref=ghuntley.com) — why only release twice a week?
- [10:05](https://www.youtube.com/watch?v=CXNqVQTzWSE&t=605s&ref=ghuntley.com) — more, sooner, cheaper: Red Hat, Fedora and release channels
- [11:16](https://www.youtube.com/watch?v=CXNqVQTzWSE&t=676s&ref=ghuntley.com) — the Singapore travelator talk
- [11:42](https://www.youtube.com/watch?v=CXNqVQTzWSE&t=702s&ref=ghuntley.com) — nobody knows the future, so stay curious
- [12:27](https://www.youtube.com/watch?v=CXNqVQTzWSE&t=747s&ref=ghuntley.com) — be more ambitious and hang around ambitious people
- [12:53](https://www.youtube.com/watch?v=CXNqVQTzWSE&t=773s&ref=ghuntley.com) — agency, not AGENTS.md
- [13:10](https://www.youtube.com/watch?v=CXNqVQTzWSE&t=790s&ref=ghuntley.com) — unikernels and capability-based operating systems
- [14:13](https://www.youtube.com/watch?v=CXNqVQTzWSE&t=853s&ref=ghuntley.com) — if secure compute matters, revisit old ideas
- [14:32](https://www.youtube.com/watch?v=CXNqVQTzWSE&t=872s&ref=ghuntley.com) — Joel Spolsky's never-rewrite rule no longer applies
- [14:50](https://www.youtube.com/watch?v=CXNqVQTzWSE&t=890s&ref=ghuntley.com) — unify your tech stack
- [15:40](https://www.youtube.com/watch?v=CXNqVQTzWSE&t=940s&ref=ghuntley.com) — aren't we just reinventing the operating system?
- [16:22](https://www.youtube.com/watch?v=CXNqVQTzWSE&t=982s&ref=ghuntley.com) — no interpreter means a smaller attack surface
- [17:02](https://www.youtube.com/watch?v=CXNqVQTzWSE&t=1022s&ref=ghuntley.com) — bespoke targeted attacks vs. generic zero-days
- [17:33](https://www.youtube.com/watch?v=CXNqVQTzWSE&t=1053s&ref=ghuntley.com) — a target size of one
- [18:12](https://www.youtube.com/watch?v=CXNqVQTzWSE&t=1092s&ref=ghuntley.com) — used to be hard: unikernels, Bazel, Nix
- [19:21](https://www.youtube.com/watch?v=CXNqVQTzWSE&t=1161s&ref=ghuntley.com) — writing to make people rethink
- [19:56](https://www.youtube.com/watch?v=CXNqVQTzWSE&t=1196s&ref=ghuntley.com) — where do you deploy a unikernel?

Keep curious.
