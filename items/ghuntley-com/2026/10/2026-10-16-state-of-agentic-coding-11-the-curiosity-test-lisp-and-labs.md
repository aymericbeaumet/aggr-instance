---
title: 'state of agentic coding #11: the curiosity test, lisp, and labs that can''t fail'
link: https://ghuntley.com/toronto/
source: ghuntley-com
published: 2026-10-16T11:41:01Z
updated: 2026-10-16T11:41:01Z
first_seen: 2026-10-16T14:45:31.206858038Z
authors:
- Geoffrey Huntley
labels:
- ai
summary: Armin Ronacher and Ben Vinegar had me on State of Agentic Coding to talk about the curiosity test, DHH coming around, durable agents, a Lisp you program by talking to it, and why the labs cannot afford to fail.
content: feed
html: 2026-10-16-state-of-agentic-coding-11-the-curiosity-test-lisp-and-labs.html
remote_preview:
  url: https://storage.ghost.io/c/06/e5/06e5d12b-0328-469d-a0d0-3a0f0ba34c46/content/images/2026/10/state-of-agentic-coding-11.jpg
  alt: 'state of agentic coding #11: the curiosity test, lisp, and labs that can''t fail'
---

![state of agentic coding #11: the curiosity test, lisp, and labs that can't fail](https://storage.ghost.io/c/06/e5/06e5d12b-0328-469d-a0d0-3a0f0ba34c46/content/images/2026/10/state-of-agentic-coding-11.jpg)

people are failing a curiosity test right now. Armin Ronacher and Ben Vinegar had me on State of Agentic Coding to talk about it, along with DHH coming around, durable agents, a Lisp you program by talking to it, and why the labs cannot afford to fail.

[State of Agentic Coding #11 with Geoff, Armin, and Ben](https://www.youtube.com/watch?v=HDdbU10r7Js)

Ben Vinegar (co-founder of [Modem](https://modem.dev/?ref=ghuntley.com)) and Armin Ronacher ([Earendil](https://earendil.com/?ref=ghuntley.com), and the person most people know from Pi) run a podcast called State of Agentic Coding, and I was the guest for episode 11. Ben and I were in the same room in Toronto. I'd landed the day before after 22 hours of flying, I was there for two days, and then I was flying back to Sydney. Armin stayed in Vienna, sensibly.

We covered a lot of ground in an hour and a half. Here's the gist, written up properly, with chapter links at the bottom if you want to jump to a specific part of the conversation.

> DHH did the funeral last week and he's right.

## the curiosity test

Ben opened by asking what my contribution to the GDP of AI development has been from Ralph loops. The OpenRouter folks personally came up to me to say thanks for the uptick, so I know it has burned a lot of tokens. I published the research in July last year. When I first showed it to Cursor they thought it was garbage. Nine months later they'd implemented it, then Anthropic did it, then OpenAI did it, and now everyone's talking about loop engineering. Dex calculated it at about $10.42 an hour when you run it in the loop and it builds AFK overnight.

That weighed a lot on my shoulders when it first came out.

[Ralph Wiggum as a "software engineer"](https://ghuntley.com/ralph/)

How Ralph Wiggum went from 'The Simpsons' to the biggest name in AI right now - Venture Beat 😎Here's a cool little field report from a Y Combinator hackathon event where they put Ralph Wiggum to the test. "We Put a Coding Agent in a While Loop and It Shipped

I've spent this year traveling the world giving talks, pleading with people to invest in themselves. My shortest take is this: software developers are failing a curiosity test. If you're displaced by AI as a software developer, it's self-inflicted, because you haven't been curious in a time of rapid transformation. As an employee, you trade time and skill for money. Employers' tastes are changing rapidly, but there's no reason you can't keep up with what's going on.

Just harshing everybody's vibe right out of the gate.

Ben pushed back here, and fairly. When people come into the industry they're young and have infinite time on their hands. Now many of them are in their thirties or forties and have racked up responsibilities. It's the same reason video games get harder to stay into as you age. It's hard to retool at this rate of change, and it's hard to feel optimistic when the thing at the centre of your income and your self-worth is in flux.

I concur. Dex Horthy has described my writing over the last two years as the depression era of Geoff: mourning the profession while pleading with people to be curious and to put themselves and their family unit first. This change has been forced on the world non-consensually. It displaces your identity. Are you a .NET developer? A Golang developer? A Python developer? It doesn't matter anymore. What matters is how good a developer you are, and whether you're using the thing employers now expect as the bare minimum.

Armin made a sharp observation: a lot of people in AI talk a very confident game externally, and that probably applies to us too. I'm straight up about this. I don't bloody know. Every time I discover something new, it strips me raw. You discover a new technique or capability, you thought you had a mental model of how it would play out, and then the game changes.

## boom and bust

Ben, playing tour guide, pointed out that the closest US cities to Toronto are Buffalo and Detroit. Buffalo was once the third most populous city in the US, prosperous because of its rail access to the Great Lakes. It's now one of the most rapidly declining. Detroit is a later, similar story, tied to the auto industry. His point: there's a long history of people with hubris believing the good times will never end.

I've been through a few of these cycles, going back to the original dot-com boom and bust. Senior compensation went through the roof during the ZIRP era because, in the previous bust, we didn't develop enough talent. That created a scarcity of knowledge. Hold on, folks: the same cycle will repeat. CS enrolments are down; I remember seeing something like 22% in Singapore this year.

[Dear Student: Yes, AI is here, you're screwed unless you take action...](https://ghuntley.com/screwed/)

Two weeks ago a student anonymously emailed me asking for advice. This is the reply and if I was in your shoes this is what I'd do. So, I read your blog post "An oh f\*\*\* moment in time" alongside "The future belongs to idea guys that can just

If you really love computers, you should be all in on loving computers right now. Everyone's having the time of their damn life. If you've stayed curious and kept your skills up to date, you can get placed anywhere right now. That means being able to stand at a whiteboard and explain how to build a coding agent as a sequence diagram, how inferencing works, and what a prompt cache is.

Armin had a slightly different read. Detroit not growing doesn't mean cars stopped being a thing; the industry went worldwide and commoditized. Software engineers got paid really well because we were scarce and our only competition was other software engineers. Automation didn't play a role. That's changing. He's absolutely convinced the exceptional role our profession has had over the last twenty years won't return in the same form. There will probably be a period with more demand for software engineers, but it will commoditize quickly. He's also curious what happens to San Francisco, a city that hasn't grown its housing stock, when the Anthropic IPO mints a pile of multi-millionaires competing for it.

I'm hoping for another PayPal-mafia-type thing. When people get minted, they invest in what they know.

## dhh did the funeral

If you're in love with typing on a keyboard in an IDE, I'm sorry to say it, but DHH did the funeral last week, and he's right. You can still type code; you just do it in your personal workshop. Employers now want you to use the CNC machine and be very good with the CNC machine. They don't want artisanal woodworking anymore.

[Rails World 2026 Opening Keynote - DHH](https://www.youtube.com/watch?v=vDjW_dRyKXY)

Ben summarised the Rails World keynote nicely. DHH walked through three generations of family pictures: a single drawing from his grandfather, a handful of photos from his childhood, and his kids with every second of their lives photographed. Portrait painting was an economic outcome. When photography made drawing reality uneconomic, painting went to impressionism and cubism: let's draw what isn't reality.

Armin had been on a panel at a Ruby conference in Austria earlier this year with Chad Fowler, who already said that the agent never writes Ruby for him anymore. When you sample which languages agents actually use, Ruby doesn't show up meaningfully. The online reaction to the keynote was largely dismissive, but the in-person reaction was much better received. Armin's point is that communities are disintegrating and reforming around the ideas that brought them together in the first place, even as the technology underneath changes. The Rails community is really a descendant of how DHH thinks software should be built; Ruby was just the tool.

So what's the point of a programming language or framework full of sugar designed for humans if humans aren't writing the code anymore? I've been chatting with the creator of Elixir, and he gets it. Language designers have dumbed down their languages to make them easy to learn for the human operator. All of that is falsified now that the agent writes the code. What does an agent need? What's the point of LSPs? Will we get new forms of interpretability, like an agent driving GDB, on a per-language basis? The languages won't go away, but the communities that swallow this tough pill and evolve will be the ones that survive.

Developers mostly speak to other developers. It's like a couple of cows in a field going moo to each other, completely missing that the cows are headed for the abattoir. Grim, I know. Stop being a cow. Learn a bit about business. Speak with founders and investors. Learn to be a product engineer. If you're working at a company that has banned AI outright, you should quit that company.

[if your team is too busy doing their 'normal job' to experiment with AI, you're preparing them to be replaced](https://ghuntley.com/replaced/)

When Ralph was published last year, I sat on it. People who have been reading this blog for a while know this. I first came across the idea of loop engineering in early January last year, sat on it for four months, and then started writing a lot, begging people

Armin said the strongest signal for him isn't his own belief; it's that the experienced, vocal holdouts are turning. DHH was dismissive early on. Notch was dismissive and recently turned. Ben noticed that DHH was transparent about having tried AI for design, pulled it back, and now landed firmly on the other side. It felt like an invitation: come join me. Armin's worry is that because DHH is polarising, the message may not land as widely as it should.

On where to find the signal: if you've been on X for the last two years, you're ahead of the game, because that's where all of this has happened. I don't love the platform; the number of AI reply bots is through the roof. But if you want the alpha, you have to push through. Go to the fediverse and talk about AI and you'll get your whole Mastodon instance banned. Bluesky has some high-alpha accounts. Unfortunately, the tech community has fractured along political lines, which means a lot of this isn't getting out.

[Multi Boxing LLMs](https://ghuntley.com/multi-boxing/)

Been doing heaps of thinking about how software is made after https://ghuntley.com/oh-fuck and the current design/UX approach by vendors of software assistants. IDEs, since 1983, have been designed around an experience of a single plane of glass. Restricted by what an engineer can see on

Back in December 2024 I was writing that IDEs are dead as a product, and everyone called me crazy. I've taken a lot of unpaid time off work this year to give talks. You can't carry all that water yourself. The best you can do is this: if someone gets curious, help them get to their own moment. Find the thing that makes their mind explode, then ask them to show the next person. For me, that moment was seeing how easy it is to build a coding agent and realizing the entire game now is to build product recursively.

[how to build a coding agent: free workshop](https://ghuntley.com/agent/)

It's not that hard to build a coding agent. 300 lines of code running in a loop with LLM tokens. You just keep throwing tokens at the loop, and then you've got yourself an agent.

Ben brought up the Bob Dylan biopic with Chalamet. Dylan is folk royalty, goes electric, and the old caretakers practically call the police to shut it down while the younger audience wants the new stuff. Folk was one of the most popular genres of its time. The reason you know Dylan's name and not most of the folk scene's is that he evolved. Boy, we don't like change.

The one thing I wrote that really landed with juniors was "dear student, you're screwed". ThePrimeagen narrated it (thank you, Prime), and I wrote it as if I were giving career advice to my own kid at twenty. Now, at the AI Engineer World's Fair and in my travels, people come up and tell me they got a promotion because they turned up with the attitude and the skills. Don't thank me; you did the work. You were curious and you invested in yourself.

[Dear Student: Yes, AI is here, you're screwed unless you take action...](https://ghuntley.com/screwed/)

Career advice for students and juniors on what's about to happen, and what to do about it.

## four months in 2026

Armin and the Earendil crew shipped Pi 1, and they also shipped something that's around 10,000 lines of code but took four months to build. Four months, in 2026. That's like a five-year project.

Armin's explanation is the bit I want everyone to hear. LLMs suck at the shape of a contract and the human feel of an API. You can express an idea really fast, but getting the right feel takes a lot of knowledge and experimentation. They solved the problem something like twenty times, each in about two hours, and none of those solutions felt right. The agent solves every problem in record time, but that doesn't give you a system with the right trade-offs. Finding the right trade-offs is what took four months.

The result is [Pi Durable](https://earendil.com/posts/pi-durable/?ref=ghuntley.com), which is how they now think an agent should be structured. You can shut down the harness, restart it, and it picks up exactly where it left off. That holds over the network, and through complex interactions such as sub-agents spawning and talking to each other.

[Pi Durable | Earendil](https://earendil.com/posts/pi-durable/?ref=ghuntley.com)

Pi Durable is a new experimental package for long-running, durable, and malleable agents that can run anywhere. A tour of what we built and why.

Fourteen or fifteen months ago, Armin, Mario, Peter and others had a strong conviction that file systems and bash were the way to go, but nobody really knew how to build a harness around that. So everyone copied what Claude Code was doing, which is a very simple idea of a session: a transcript, a tool call, a tool result, in a loop. That model has stayed at the core of more or less every agent. At its simplest, all it does is automate copying and pasting.

But demands have changed. Background compaction means doing two things at once. A Slack experience means five people talking to the same agent, some in the same sub-thread and some branching off. That mapped incredibly poorly onto Pi's original design. Armin called it a little bit of cargo culting that we're all suffering from: we keep building on a basic primitive that causes pain everywhere. We don't notice, because an agent will happily build you a decent-looking agent, but under the hood it's messy, horrifying and not reliable.

> this technology is moving really, really quickly and is held back by ideas about how it should work that are a year plus old.

Can I get that louder and printed on a shirt? Here's my hot take: your AGENTS.md and many of your skills should be deleted after every model release, or at least every couple of months, and redone. We carry so much that we think of as best practice, and we perpetuate it. Steering messages versus follow-ups were a UX paradigm; the loop underneath stayed the same. Async tasks for bash were one of the last big advancements. Go further back: why do we even have a shell? Because we used to have a human operator. If that need had never existed, would we have an operating system shell at all? That's the level at which we need to rethink the substrate.

## a lisp you grow by talking to it

Ben asked what we'd actually thrown out lately, so I showed the thing I'd published that morning. We've raised two generations of software developers who have probably never seen Lisp and what it can do. Armin was very curious, because three months earlier he'd given a talk saying Lisp was the worst language for agents since they're terrible at counting closing parentheses. That's the old joke.

[an application in lisp you grow by talking to it](https://ghuntley.com/lisp/)

It's very clear that programming languages will converge toward what agents need, not humans.

The usual mental model of a coding tool goes like this: you prompt it, it runs bash to look at your project, finds the file and uses the edit tool to change it. Then comes compilation, then CI/CD, then code review. What I showed throws all of that out.

I've connected an LLM to a running Lisp. I asked what functions were defined, and the LLM asked the running image. I asked for a string reverse function, typos and all, and it defined one directly into the image. Then string uppercase. Then a function to fetch a URL and parse the JSON. I can undo a change and roll back to the previous generation. There are only two tool calls in the inferencing loop: evaluate an expression, or define one as a function. I'm editing the Lisp image and its runtime environment through a chat. Once a function is defined, it's defined.

Now don't think of this as a chat interface. Imagine you're logged into your website and there's a secret little Hackers-style menu that only shows for staff. You don't load up an IDE. You pop a prompt and evolve the running program by chatting with it. What if that's how a user feedback request gets actioned?

Ben mentioned that the opencode folks had just written about how much work it took to make individual plugins hot reload, never mind be programmed live. Armin said that on a single-player level this is very much what made Pi interesting, since Pi can rebuild and reload itself. The challenge is that JavaScript is not a language built for hot reloading.

## why lisp died

Ben's question was the right one: if this is so fantastic, why does it feel like you're digging up a grave?

Lisp has always been about these things. But you had to be a Lisp programmer to do Lisp. Now there's a natural-language interface that lets anyone do Lisp-type stuff. Armin gave the more precise answer. The problem with software is effects. That's partly why Pi Durable exists: if you shut down Pi and reload it, it doesn't come back exactly as it was, because not everything is serialisable. Lisp can serialise its entire state out more than most languages can, and functions are defined much like data.

Why did it go away? Lisp was slow unless you had a machine that really suited it, and it didn't survive the shift to the personal computer, where people wrote assembly. Armin's bluntest point: it's a horrible language to look at. It's awful for humans. Maybe it's great for LLMs. Many of our trade-offs as engineers exist because we're lazy and pick the things that let us stay lazy.

That's the Ruby thing again: designed to be beautiful and easy for humans, but less powerful. Paul Graham's [Beating the Averages](https://paulgraham.com/avg.html?ref=ghuntley.com) covers this with the Blub paradox.

I also want to give credit to [Krzysztof Cieślak](https://github.com/krzysztof-cieslak?ref=ghuntley.com). He doesn't get credit for it, but he's the person behind what everyone now calls spec-driven development. He's at GitHub Next, he's one of the people behind Ionide for F#, and he published research back in the GPT -3 era asking whether Markdown could be the future of how we publish software. When I was doing my Ralph loops, I remembered his research and thought: the models got better, let's redo it. The rest is history. For the record, though: code is the spec, folks. Markdown is not a spec. As Armin put it, there are specifications and there are vibe specifications.

Someone took the Lisp idea and vibed up a web app version in about an hour. It's not Lisp; Lisp is just an easy way to express the idea, nerd snipe people and teach a new generation that powerful languages exist even when their developer experience is terrible. Who cares about the developer experience now that agents write the code? He clicks a little developer button, prompts the functionality in, and the app saves a generation. Think about what that means for someone in sales, marketing, finance or legal: building the product by chatting with the product. This is a completely different way of designing a coding harness. The programming language is LLM-enabled, rather than a harness producing code for a programming language. If you want to be a nerd, go and do this with Smalltalk. You'll be on the front page of Hacker News tomorrow, guaranteed.

Ben compared it to interviews with musicians like Beck, who pull records off the shelf in their office, or hip-hop producers digging through forgotten lore. I'm pulling out my old Smalltalk and Lisp records. He called me old. It's worth going back to the classics and doing some sampling.

## code mode and code golf

Armin has noticed the models changing. Harnesses are increasingly having the model write code to make tool calls. I've seen the same thing: the models aren't going for the edit tool anymore. They're doing a kind of program synthesis, writing one-liner Python to edit files.

Armin said Pi supplies an edit tool, and in something like 80% of cases with the latest models it's no longer used. His theory has two parts. First, there's very little punishment in training runs for anything other than succeeding, so the models use whatever means they have. Second, that code behaviour is being trained into the models. Pi offers code mode as a tool and will probably make it the default: run some JavaScript instead of bash. Bash runs where your files are; code mode runs in the harness loop, which is typically the same computer but not necessarily.

The side effect is fascinating and a bit grim. Remember code golf, where you win by writing the shortest possible solution? That's what some of these models feel like. Single-character variables, no indentation, unreadable. Ben's read was that there's a real tension here: the tightest possible code is optimal for a tool-calling loop, but it isn't what you want in your application code. Armin's read was that it might not get punished much anymore, because the models are mostly rewarded for completion on synthetic benchmarks, and they're releasing faster than ever.

The last time it was like this was the Sonnet 3.5/3.7 era, which spewed Markdown everywhere and wrote those one-liners even though it had an edit tool. Then it went away. Now it's back. I was recently given access to a couple of racks of GB300s and have been running Kimi on them. You can feel the models diverging: Kimi reaches for the edit-file tool, while the frontier American models program their way around it over bash.

## black-box agents

For our profession, we've been post-AGI for over a year now. By that I mean the models have been good enough to cause displacement for over a year. I'd rather focus on how beautiful it is that you don't need frontier intelligence for every task.

As much as you might want local models, they aren't really a thing yet unless you have a couple of million dollars of data centre hardware. That won't always be the case. I love open-weight models. Kimi is not as good output-wise, it's chattier and slower in its chain of thought, but I get to see the thinking traces. I'm permanently upset that the American frontier labs keep locking more of my stuff away.

Ben has noticed the desktop apps showing less and less of what's happening. You ask for a big piece of software and get three lines of output. Is that product design or technology?

Armin split it in two. Hiding thinking traces is the labs hating distillation, and we should call that out for the nonsense it is. Less tool output is the same reason Pi will probably soon hide tool output: when the model invoked harness-provided tools, you could build a UX around it ("now you're reading a file"). Now that it's running unreadable code, you'd need a second LLM just to explain what it's doing.

He thinks that's a regression for the user, the model and the lab. Approvals are mostly gone. But if you're using an agent for something truly critical, like medical research or fully verified programs, you can't just take its word at the end.

You want verification along the way. One possible future is two streams: the code being generated, and a commentary stream from another model explaining what's going on. We already run smaller models alongside larger ones for speculative decoding. But do you really want to pay extra tokens to find out what's happening?

I backed him on that. When we built Amp, a lot of it was UX design: animations and components keyed to the type of tool, whether it was an edit or a git patch. When everything is just bash, there's no way to know what it's doing without running a classifier over the tool calls. That's what the big harnesses are doing now: classifiers and parallel things in the background, chewing more tokens.

Here's a brainworm. How much of agentic design is constrained by not busting the prompt cache, because everyone is financially sensitive to it? If you removed that restriction, what would you build? I think it would allow some really cool stuff.

Armin is excited about fast classifier models for LLM-sized problems at much higher speeds; every morning he uses one to sort open issues and compare them to yesterday. But he called the KV cache an innovation killer. It locks you into an ecosystem, at least for the duration of a session, so you can't switch models mid-session. It's got worse: on some frontier models, a cache miss from a prefix change now loses all your reasoning. That's driven by distillation fear, and it's one more reason open-weight models have a good future. They allow experimentation.

I'm very long on open-weight models. They've been good enough for the last year and lag the frontier by about three months; the hardware is what's missing. American open-weight efforts like Reflection are starting to show up too.

## tech demo land

Look at the frontier labs' go-to-market motion. It's an AWS play: commit now to reserved capacity and pre-commit, and you get a discount. They're asking people to lock into an ecosystem before a winner has been declared. Two weeks ago OpenAI was king; now we're back to loving Claude.

Ben had a conspiracy-brain theory. All the models write decent code now, so that's no longer the differentiator. Are capabilities trained in deliberately and then seeded through influencers? When one model came out, everyone was suddenly making games and 3D models. When Opus 5.5 came out, videos of it making EDM-pop music videos appeared out of nowhere.

Armin doesn't think every release will be punctuated like that, and his reason is the important one. Forcing himself to pay API prices changes how you look at all of it.

> I am burning more virtual money than I'm earning.

That's ridiculous. This is tech-demo land more than sustainable-business land, and eventually someone will want a return. His favourite example: a $20 CarPlay adapter from China with a system on a chip. You can hack it, so now it runs his little agent and his car dashboard shows a crappy version of Pi. It was $20 for the hardware and $15,000 in tokens. He would never have paid $15,000 for a side project. At $200 he still wouldn't have bothered. He does it because he rotates subscriptions.

Ben has been doing the same with Blender scenes, a Toy-Story-esque ad and 3D-printed replacement parts. Any rational pricing would say just order the part on Amazon.

Two things can be true. Either a random account making a magical discovery was genuinely curious, and curiosity is the limiting factor in society right now. Or that person is part of a campaign, like a game developer planting an Easter egg nobody finds and then going to a games magazine to do an exposé. It's happened before in our industry. (Ben has a bunch of Modem Easter eggs on his website that nobody has found. ATDT.)

People call me the token guzzler, the subscription killer. A year ago I went viral on Reddit as one of the top users; I'm the clown that blew through the quotas. If you're using OpenAI models, there's a thing called underclass on my GitHub that lets you pool multiple subscriptions.

[GitHub - ghuntley/underclass: underclass: an OpenAI-compatible pooling proxy that pins sessions to one account (prompt cache stays warm), cools quota-exhausted subscriptions until their window resets, and fails fast with the earliest Retry-After when the whole pool is dry.](https://github.com/ghuntley/underclass?ref=ghuntley.com)

underclass: an OpenAI-compatible pooling proxy that pins sessions to one account (prompt cache stays warm), cools quota-exhausted subscriptions until their window resets, and fails fast with the ea…

It still makes sense to have ten of them, because they're so heavily subsidised. But you don't need frontier intelligence for everything. Maybe just get a GLM coding plan, which used to be about 300 bucks for the entire year.

They're subsidised for a reason. The labs want to build addictive behaviour at home, so that when you go to work you think: if I can build my wildest dreams at home, why can't that magic happen here? No one can pay the real cost of these tokens. I'd be up past US$200,000 a month at real prices. I hit these slot machines hard for about ten days, watching the inferencing and the behaviours. Then I throw my computer away and don't look at it for a week or two, because you've done six months of work in a week and you need to self-care. I've seen what happens when you don't. One of the first people I showed the Ralph loop to fully cooked himself: he teleported into the future, built everything he'd planned for retirement, didn't sleep, and was unable to work for nine months.

On a lighter note, my twelve-year-old has been playing Magic the Gathering and making friends, which is great. He's also been 3D printing custom card boxes. I'm putting this on tape so I can show him when he's eighteen: I've noticed that every box, every week, is hyper-personalised with the name of a different girl in his class.

Ben suggested I send him the bill at the real token price.

Ben's sharper point: if a general model can write music, generate those videos and drive Blender, do you still need the vertical model companies? Maybe you just need a Claude subscription.

## the labs cannot fail

Put your financial accounting hat on. The labs have raised so, so much money, and we're entering a higher-interest-rate environment. The bond market is changing. If you borrow money, you need an ROI above the bond rate. They very much cannot fail, which means everything is on the table to be absorbed into your Claude subscription. I could see Runway inside Claude, maybe even through an acquisition. At this level of capitalisation, nothing is off the table.

Armin was more measured: some people win, some lose. He thinks it will most likely work out, but there are no guarantees. I think the financials are cooked. The ROI over the new bond rate needs an adoption curve far steeper than the human adoption curve, and that gap is going to be very hard for the labs.

Both things can be true at once. I haven't written code by hand in about two years, and I'm never going back. This is transformational. If it all blows up, I'll run GLM or Kimi on some data centre hardware myself and be fine. My profession has been transformed, and I'm not going back to how it was, but I'm in no way hyper-dependent on any one lab.

## who wants this?

Ben brought it back to my demo, and fairly. He doesn't know that many people actually want to change their website or app by talking to it. Technologists look at all the incredible stuff we can do through the lens of software malleability, but that's not society at large. Most people want to use software, not change it every session. He's been running Omarchy for about a year and only now let himself customise it, for about an hour, to make it nicer for his family. That's probably the interaction.

I'll play with that. Society at large does not want an assistant, folks. Only type-A people who already have an executive assistant want one. When I get automated emails that are obviously AI, I block them. I'm actually seeking out humanity as everyone else rushes to automation. Search Instagram and TikTok for AI data centres and you'll find people who want to burn them down. In business, though, people do want to change their software without having to talk to someone and switch between tools.

Armin's ending thought: while we were recording, someone proposed two potential new superconducting materials that Opus 5.5 found. It gave him LK-99 flashbacks. He doesn't think it matters that we're all obsessed with AI for software engineering. Where this really pays out is if we get room-temperature superconductors, or whatever else we might find.

> if it's just going to be a bunch of programmers who are now producing slop, I'm pretty sure society will just burn the data centers down.

We'll know the labs are ready to IPO when they've found a cure for cancer. That's what changes society's heart. That's always been the narrative.

Then again, the average person doesn't really care. TikTok has been very anti-AI lately: people see something is AI and won't like or reshare it. But there's an AI-made song doing the rounds about an Irish girl that I can't describe on a family-friendly podcast. Young women are showing it to their mothers to film the shocked reaction, and for the first time people are saying, "I don't care if this is AI, it slaps."

content warning: not safe for work audio

We call things slop because we're playing with a new creative medium and don't yet know what isn't slop. We're all just trying to figure out what's changing.

Yesterday's slop is today's content.

## chapters

- [0:00](https://www.youtube.com/watch?v=HDdbU10r7Js&t=0s&ref=ghuntley.com) — cold open
- [0:22](https://www.youtube.com/watch?v=HDdbU10r7Js&t=22s&ref=ghuntley.com) — intro, and the GDP of Ralph loops
- [4:43](https://www.youtube.com/watch?v=HDdbU10r7Js&t=283s&ref=ghuntley.com) — the curiosity test
- [9:57](https://www.youtube.com/watch?v=HDdbU10r7Js&t=597s&ref=ghuntley.com) — boom and bust: Buffalo, Detroit and the next cycle
- [17:14](https://www.youtube.com/watch?v=HDdbU10r7Js&t=1034s&ref=ghuntley.com) — DHH comes around
- [34:50](https://www.youtube.com/watch?v=HDdbU10r7Js&t=2090s&ref=ghuntley.com) — Pi Durable, and deleting your AGENTS.md
- [42:31](https://www.youtube.com/watch?v=HDdbU10r7Js&t=2551s&ref=ghuntley.com) — live Lisp demo
- [50:37](https://www.youtube.com/watch?v=HDdbU10r7Js&t=3037s&ref=ghuntley.com) — why Lisp died
- [59:37](https://www.youtube.com/watch?v=HDdbU10r7Js&t=3577s&ref=ghuntley.com) — code mode
- [1:05:17](https://www.youtube.com/watch?v=HDdbU10r7Js&t=3917s&ref=ghuntley.com) — black-box agents, subsidised tokens and open weights
- [1:29:44](https://www.youtube.com/watch?v=HDdbU10r7Js&t=5384s&ref=ghuntley.com) — labs can't fail
- [1:31:38](https://www.youtube.com/watch?v=HDdbU10r7Js&t=5498s&ref=ghuntley.com) — who wants this?

Keep curious.
