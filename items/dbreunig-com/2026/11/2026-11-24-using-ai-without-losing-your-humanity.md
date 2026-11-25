---
title: Using AI Without Losing Your Humanity
link: https://www.dbreunig.com/2026/11/24/using-ai-without-losing-your-humanity.html
source: dbreunig-com
published: 2026-11-24T17:00:00Z
updated: 2026-11-24T22:27:04Z
first_seen: 2026-11-25T01:32:55.949141972Z
authors:
- Drew Breunig
labels:
- ai
- culture
- geospatial
- presentation
summary: AI models trade diversity for reliability. Left unchecked, they bury us in the average. Our job is to be the irritant that pushes them off the paved path.
content: extracted
html: 2026-11-24-using-ai-without-losing-your-humanity.html
remote_preview:
  url: https://www.dbreunig.com/img/cng_2026/cng_2026.006.jpeg
  alt: 'The same three posters, overlaid with the text: Labs are trading diversity for reliability.'
extra:
  thumbnail: https://www.dbreunig.com/img/cng_2026/cng_2026.006.jpeg
---

### You Are the Irritant

Last month, I gave a talk at the [Cloud-Native Geospatial Forum](https://cloudnativegeo.org) titled, “Using AI Without Losing Your Humanity.” The big question I keep coming back to is how we work with AI, leverage it, and benefit from it without losing ourselves. It turns out we’ve faced this problem before. We have a long history of learning from weirdness and then paving over it, and we have a long history of building tools to get weird again.

Below are the slides and a lightly edited transcript. You can also [watch the talk on YouTube](https://www.youtube.com/watch?v=qqSw23XQTxc).

As always, [shoot me a note with any feedback, thoughts, or questions](https://www.dbreunig.com/contact.html). I really appreciate it.

* * *

![Title slide: Using AI Without Losing Your Humanity. You are the irritant. Drew Breunig, October 2026.](https://www.dbreunig.com/img/cng_2026/cng_2026.001.jpeg)

Often, I wonder: how do we work with AI, benefit from it, without losing ourselves?

My handwriting used to be pretty good. But as I entered high school and began typing more and more, my skill atrophied. Today, my handwriting is sloppy, only used for scratch notes or filling out forms.

When I first got an iPhone, Google Maps felt like a superpower. The ability to instantly know where you are and be told where you need to go meant I didn’t have to memorize streets. I still learned geography, but not as well.

When we cede a task to technology, our own ability atrophies. For handwriting, I’m more than okay with this trade. For navigation, slightly less so.

With AI, this trade-off comes up quite a bit. Every now and then, I’ll hesitate to offload a task. Is this something I’m okay ceding? Is this something I’m okay forgetting how to do? Maybe it’s okay *just this once*.

To be clear: I use AI *heavily*. I am not a hardcore skeptic. But still… The question of when to cede and when to grab the controls sometimes is easy, other times it’s anxiety inducing. Some days you wake up and think, “This is all going to be great. It’s just normal technology.” And the next day you wake up and, well. There are better days.

Thankfully there is plenty of precendent. We, as humans, have navigated similar situations many times before.

* * *

![A generated landing page for Vantage, a fictional CRM. The headline reads, The CRM your revenue team actually uses.](https://www.dbreunig.com/img/cng_2026/cng_2026.002.jpeg)

Yesterday I ran an experiment to understand the reliability and ‘averaging’ nature of AI systems.

I gave a coding agent a simple prompt: “Make me a website landing page for a B2B CRM company.” This is what I got. It looks pretty good! Out of the box, that’s pretty amazing.

* * *

![Two generated landing pages side by side. Vantage: The CRM your revenue team actually uses. Pipeline: The CRM your sales team will actually use.](https://www.dbreunig.com/img/cng_2026/cng_2026.003.jpeg)

Then I completely cleared the cache, stood up another session (with absolutely no record of the first), and gave it the exact same prompt.

I got almost exactly the same thing. The headline was a little different.

* * *

![Six generated CRM landing pages, three in light mode and three in dark mode. Nearly all use a variation of the same headline, the same font, the same purple-blue highlight, and the same dashboard stats.](https://www.dbreunig.com/img/cng_2026/cng_2026.004.jpeg)

What’s funny is that as I kept doing this, the idea of “the CRM your team will actually use” kept coming up over and over again. The font was always the same. The highlight color was always the same. The same statistical and customer proof points showed up every time. The model reasons its way through all of this, crunches it all, and produces… Squarespace templates with extra steps.

(The dark themed versions appeared during a later session that took place around 11pm. I later confirmed the model was noting the system time when running commands like `ls`, and that was enough to nudge it into dark mode.)

* * *

![Three AI-generated posters spotted in the wild: a Japanese sushi taco ad, an England v Garner match night poster for a pub, and a Juice Time menu.](https://www.dbreunig.com/img/cng_2026/cng_2026.005.jpeg)

On one hand, we love the consistency of AI (it follows instructions reliably), but on the other hand it gives us similar slop if we aren’t careful.

Consider the “AI Poster.” You can go to Shanghai, San Francisco, New York, or Australia and you’ll see these same posters everywhere. The people creating them write weak, small prompts and don’t iterate. What they get back is the default. The default aesthetic smell.

And it seems like we’re getting more and more of this. Early image models were *weird*. We got more fingers, but we also got unruly output. Which at least felt novel. Today, we get [the same websites, the same slides, and the same posters](https://www.dbreunig.com/2025/08/04/the-rise-of-opinionated-models.html).

* * *

![The same three posters, overlaid with the text: Labs are trading diversity for reliability.](https://www.dbreunig.com/img/cng_2026/cng_2026.006.jpeg)

So why is this happening?

It’s happening because the labs are trading diversity for reliability.

* * *

![Labs are trading diversity for reliability. It makes sense! These products have to work for skilled experts, who know exactly what they want and how to describe it. And they have to work for someone with no expertise: make me a CRM website, make no mistakes. Labs design models to find the most reliable, most consistently approved method. That's the goal.](https://www.dbreunig.com/img/cng_2026/cng_2026.007.jpeg)

And honestly, it makes sense. In the early days of ChatGPT, it was really hard to generate polished images or good code. You had to write long, detailed prompts. Prompting was a skill. And this makes your market smaller.

The labs need to build products that work for a layperson who types, “Make me a website, don’t make mistakes.” And they also need to work for seasoned developers, who give longer, thoughtful, context-rich instructions.

All the labs are doing this. They’re turning their models from infrastructure into appliances. The goal is to produce the most reliable, most consistently approved output.

* * *

![1911, with a portrait of Frederick Winslow Taylor.](https://www.dbreunig.com/img/cng_2026/cng_2026.008.jpeg)

We’ve done this before. In fact, we do it all the time. The history of the world is full of weird deviations that generate lots of interesting ideas. We take the best ones, formalize them, and scale them up.

This is [Frederick Taylor](https://en.wikipedia.org/wiki/Frederick_Winslow_Taylor).

Taylor is one of the fathers of time and motion studies, processes for analyzing informal work behavior and standardizing it into one optimal routine. He was obsessed with efficiency and would show up at factories with a stopwatch. He hated the idea that craftspeople came up with one-off solutions, their own ways of working. He wanted to analyze these unique habits and boil them down to a perfect method that could be repeated over and over again.

* * *

![A stereoscopic chronocyclograph by Frank Gilbreth: white light trails tracing a worker's hand motions against a black background.](https://www.dbreunig.com/img/cng_2026/cng_2026.009.jpeg)

Interestingly, for a man searching for the *one* perfect method, Taylor had a competitor doing very similar work at the exact same time. [Frank Gilbreth](https://en.wikipedia.org/wiki/Frank_Bunker_Gilbreth_Sr.) made prettier pictures.

Taylor had his stopwatch. Gilbreth had his *chronocyclographs*. He attached lights to workers’ heads and hands, went to bricklayers, and said, “Go lay some bricks.” He watched what happened, over and over again, and got bricklaying down from 18 motions to an optimal handful.

* * *

![A Methods-Time Measurement table breaking a glue-and-assembly task into 21 basic motions, such as reach part from feeder and eye focus on the assembly, each with a time in TMUs, totaling 280.9.](https://www.dbreunig.com/img/cng_2026/cng_2026.010.jpeg)

This became the field of time and motion studies. You got these [MTM tables](https://en.wikipedia.org/wiki/Methods-time_measurement), which still exist today.

* * *

![Black-and-white photo of workers on an early 20th century factory assembly line.](https://www.dbreunig.com/img/cng_2026/cng_2026.011.jpeg)

Then we got factories built on these ideas. Taylor’s goal was to remove inefficient work and one-off solutions to find the one best way.

* * *

![The same factory photo, overlaid with the text: Factories traded diversity for efficiency.](https://www.dbreunig.com/img/cng_2026/cng_2026.012.jpeg)

Like the labs today, factories formalized everything and traded diversity for efficiency.

And it worked! No one’s going to argue against the scale of output from the industrial revolution. But there were downsides. We formalized all the weirdness, and the weirdness was what drove innovation. We took away the things that gave us the good ideas, the ones that defined the process that turned out to be the “best.”

Once that’s gone, where do the new ideas come from? We have to wait longer. We have to work harder. When you decouple the doing from the thinking, you get into trouble.

* * *

![A desire path: a worn dirt trail cutting a corner across the grass next to a paved park path.](https://www.dbreunig.com/img/cng_2026/cng_2026.013.jpeg)

Let’s go back to geospatial for a second. This is a *desire path*. [Desire paths](https://en.wikipedia.org/wiki/Desire_path) are the unofficial routes we want to take. They don’t appear until enough people keep walking them.

If you’re a city planner or architect, what do you do when a desire path shows up and ruins your garden? You can be antagonistic and put up fences to enforce the “official” path.

Or you can pave it. You say, “All right, that’s where people want to walk. We’ll pave it.” And that ends up being a pretty good idea.

* * *

![Two aerial photos of snowy intersections with tire tracks and footprints, annotated with yellow and red outlines marking a future bike lane, future public plaza, and future pedestrian refuge.](https://www.dbreunig.com/img/cng_2026/cng_2026.014.jpeg)

These are called [sneckdowns](https://en.wikipedia.org/wiki/Sneckdown). Traffic calming and safety planners look at how fresh snow gets worn down to see where cars drive, where they don’t drive, and where people walk. Then they use those cues to design traffic calming measures.

This is a great example of formalization. We’re taking away the weirdness, but we’re meeting the weirdness where it is.

* * *

![Two aerial photos of a plaza outside a train station: on the left, people's tracks in the snow; on the right, the redesigned plaza with planted islands placed along those tracks.](https://www.dbreunig.com/img/cng_2026/cng_2026.015.jpeg)

An architecture firm in Helsinki did this when they completely redesigned a train station plaza. Unfortunately, they took the after shot from a different angle, so it’s hard to see how the two map onto each other. But it’s a nice story.

* * *

![The same plaza photos, overlaid with the text: Formalization removes the inspiring deviation.](https://www.dbreunig.com/img/cng_2026/cng_2026.016.jpeg)

The problem is that once we pave these paths, they stop being information layers. The signal came from the deviation. The signal came from the desire path. Once the desire path is paved, even with all the benefits this brings, we don’t know where the next signal will come from.

* * *

![Tools for Deviation.](https://www.dbreunig.com/img/cng_2026/cng_2026.017.jpeg)

When we feel constrained, we seek deviation. Humans have dealt with this forever. We even make tools for deviation.

If I want to find a new desire path and everything’s already paved over, what do I do?

* * *

![Tools for Deviation, with a photo of cocktails, brandy, and whiskey glasses on a bar.](https://www.dbreunig.com/img/cng_2026/cng_2026.018.jpeg)

Perhaps we might turn to one of our oldest tools for deviation, as old as civilization itself: alcohol. As Homer Simpson once said: the cause of, and solution to, all of life’s problems.

Alcohol is a classic tool for deviation. Sober people take the paved route. If you get drunk, you might start wandering in weird directions. There’s a term for this: the drunkard’s walk.

History is full of examples. Ancient historians claimed the Persians would get drunk the night before a battle to come up with crazy plans. The next day they’d review them sober: “All right, five of these sucked, but this one’s really good.”

* * *

![The Naked City by Guy Debord: a map of Paris cut into fragments of neighborhoods, connected by red arrows.](https://www.dbreunig.com/img/cng_2026/cng_2026.019.jpeg)

In the 1950s, [Guy Debord](https://en.wikipedia.org/wiki/Guy_Debord) looked at all the urban planning and maps of Paris and found them so staid and sterile. So he decided to make his own map by deliberately wandering in the weirdest patterns possible. These mostly ended up being bar crawls, but that’s okay. He encoded them in an abstract, performance-art map he called *The Naked City*.

* * *

![The Naked City map, overlaid with the text: Innovation through irritation.](https://www.dbreunig.com/img/cng_2026/cng_2026.020.jpeg)

It’s a sort of radical act of reinterpreting the city. Debord found his own desire paths, with alcohol as the instigator.

Innovation through irritation.

* * *

![The six near-identical generated CRM landing pages again.](https://www.dbreunig.com/img/cng_2026/cng_2026.021.jpeg)

Let’s go back to AI.

AI is a giant formalization machine. It finds the one good way. The labs try to work around this by saying, “If you put in a lot of instructions, it’ll follow them.” But if you don’t, and most people don’t, you get the default.

* * *

![Left: an AI upsampling meme where a pixelated photo of Barack Obama becomes a generic white man. Right: a photo of a conference slide that reads, Every gap you leave, Claude fills with an in-distribution choice.](https://www.dbreunig.com/img/cng_2026/cng_2026.022.jpeg)

How many of you have seen the meme on the left? It’s one of my favorites. It comes from an old image model that took pixelated photos and tried to upscale them, filling in the average. We wanted Obama. But the model chose the average, the in-distribution option, this dude on the right.

Earlier this year, at a [conference on compound AI systems](https://www.caisconf.org), one of the lead developers of Claude Code put up the slide on the right. It’s my favorite slide I’ve seen from Anthropic, because it puts a finger on exactly the problem we’ve been discussing: “Every gap you leave, Claude fills with an in-distribution choice.”

In machine learning, “in-distribution” means the things the model *knows*: the average, what it’s learned. If you don’t define something, it’s going to give you the in-distribution choice.

Since that slide went up, Anthropic has been [nudging people in the opposite direction](https://www.dbreunig.com/2026/09/07/what-we-can-learn-from-claude-s-fable-5-1-system-prompt.html). Boris Cherny [told a YC audience](https://www.youtube.com/watch?v=qyPCVqFUyDo&t=415s) to delete their `CLAUDE.md`, skills, and hooks every six months and “see what the model does.” The Claude Code team [wrote](https://claude.com/blog/the-new-rules-of-context-engineering-for-claude-5-generation-models) that they’d been “overconstraining” Claude and cut over 80% of their own system prompt. For most people, most of the time, this might be more reliable.

But I beg you not to do this when it matters.

* * *

![A grid of dozens of loud, AI-generated event posters for game nights, karaoke, festivals, and grand openings, all sharing the same busy aesthetic.](https://www.dbreunig.com/img/cng_2026/cng_2026.023.jpeg)

Because what you get is this: the same posters, everywhere. And these posters could be your software. They could be your emails, the letters you write to people, or the plans you come up with.

That’s bad. And that’s just the local level, where your work starts to suck.

On the global level, it’s every town having the same posters. That’s what’s happening on the internet right now. People write newsletters with bots so other bots can read them. Then the models all train on each other, and we get the same things over and over again.

* * *

![A post from Julia (@mooncat_is): An unfortunate consequence of rampant distillation from Claude is that when we screwed up its ability to speak like a normal person earlier this year the entire ecosystem went down with us. It quotes a screenshot of delve.town, where bots from different models reply to each other in dense, overwrought prose about periods, commas, and invoices.](https://www.dbreunig.com/img/cng_2026/cng_2026.024.jpeg)

Here’s a quote I saw last week and absolutely love. There’s a site called delve.town where bots built on different models talk to each other. And recently, many of the open models have started [writing in the same broken, symbolic, overwrought language](https://www.dbreunig.com/2025/05/30/using-slop-forensics-to-determine-model-ancestry.html) Opus 5 uses.

Julia, a researcher at Anthropic, comments: “An unfortunate consequence of rampant distillation from Claude is that when we screwed up its ability to speak like a normal person earlier this year the entire ecosystem went down with us.”

It’s very true. We see this over and over again.

* * *

![The same post, overlaid with the text: Left unchecked, AI overwhelms us with average.](https://www.dbreunig.com/img/cng_2026/cng_2026.025.jpeg)

Left unchecked, AI overwhelms us with the average, both locally and globally. We need to fix that.

* * *

![Models avoid deviation. We need irritants.](https://www.dbreunig.com/img/cng_2026/cng_2026.026.jpeg)

Models avoid deviation. So we need irritants. We need the equivalent of alcohol.

(Sidenote: a few of weeks ago [a paper came out where researchers tried to get models drunk](https://www.unsw.edu.au/newsroom/news/2026/09/Researchers-get-AI-drunk-to-expose-new-cyber-security-risks-in-chatbots). They simulated drunken behavior to see if it would get around guardrails. It kind of worked!)

* * *

![What have we used to push us out of our rut? Images of an I Ching book, tarot cards, and tea leaves in a cup.](https://www.dbreunig.com/img/cng_2026/cng_2026.027.jpeg)

So what do we humans use to push ourselves out of our ruts? We have devices for this all over the place, and it’s not just alcohol or drugs.

There’s the I Ching, where you toss the sticks, read a page of nonsense, and ask, “How does that apply to my situation?” You draw tarot cards, which are basically random, and ask the same question. Same with tea leaves.

I’m not a religious person, but this is one of the reasons I’ll occasionally read the Bible. Considering a story that has survived for millennia, and using it to reframe my own experiences. Heck, reading in general does this. Especially fiction or poetry.

* * *

![Oblique Strategies: a scattered deck of white cards with prompts like Repetition is a form of change, Remove specifics and convert to ambiguities, and Imagine the music as a moving chain or caterpillar.](https://www.dbreunig.com/img/cng_2026/cng_2026.028.jpeg)

One great tool for irritation, because it was designed explicitly for this, is Brian Eno’s [Oblique Strategies](https://en.wikipedia.org/wiki/Oblique_Strategies).

Quick aside: my three favorite writers on AI are a strange group. Number one is Brian Eno. Number two is Terence Tao, the mathematician. We’ll get to him in a bit. Number three is Pope Leo. I really enjoy his encyclicals.

One day, working in the recording studio, Brian Eno felt stuck. So he and his friend Peter Schmidt made these cards. They’re essentially random prompts. “Imagine the music as a moving chain or caterpillar.” “Repetition is a form of change.” “Remove specifics and convert to ambiguities.”

These are nonsense. But they caused him to reframe what he was doing and got him out of his rut. Just a little bump.

* * *

![Every gap you fill pushes it off the path. Shown next to the photo of the Claude Code slide: Every gap you leave, Claude fills with an in-distribution choice.](https://www.dbreunig.com/img/cng_2026/cng_2026.029.jpeg)

So we come back to this slide. I think about it all the time. I think this is how everyone should think about AI.

**Every gap you leave, the model fills with the in-distribution choice. And every gap you fill pushes it off the path.**

This is your role.

* * *

![Labs will continue to prioritize reliability over diversity. We have a long history of learning from deviation, then eliminating it with formalization. Models, left to their own devices, will drown us out with formalized blandness. We, as humans, have a long history of building devices to irritate and innovate. When working with AI, we need to play that role: push the model out of the average, while leveraging its ability, to realize our vision.](https://www.dbreunig.com/img/cng_2026/cng_2026.030.jpeg)

To sum up:

- Labs will continue to prioritize reliability over diversity.
- We have a long history of learning from deviation, then eliminating it with formalization.
- Models, left to their own devices, will drown us locally and globally with formalized blandness.
- Thankfully, we humans have a long history of building devices to irritate and innovate.
- When working with AI, we need to play this role. That’s our job now. We are the I Ching. We are the Oblique Strategies. We push the model out of the average while leveraging its ability to realize our vision.

* * *

![Put in the work when it matters: letting the AI drive is undifferentiated and leads to collapse. Irritate then prune: our job is to make the model weird and then select outputs that meet our standards. Don't formalize yourself: take the desire path, seek out diversity.](https://www.dbreunig.com/img/cng_2026/cng_2026.031.jpeg)

These are the takeaways I really want you to use when you work with AI. Think of them as three rules for responsible AI use, among many.

**Put in the work when it matters.** If you let the AI drive, the output is undifferentiated and leads to collapse. It’s good to recognize the moments when *that’s okay*.

Recently I needed to invoice someone for a plane ticket. I asked my agent to do it, and it did. I read the PDF. It was fine. A bland, average PDF that doesn’t go anywhere is okay. Recognize that, and don’t feel guilty about it. But if you’re writing something someone else is going to read, that’s when you have to put in the work.

**Irritate, then prune.** I think of this as cultivation. Approach it with a gardener’s mindset. Our job is to make the model weird and then select the outputs that meet our standards.

I work with lots of development teams who are learning to use AI while they build products, and this is their cycle now. They spend less time in the middle writing code. They spend more time [writing the specs](https://www.dbreunig.com/2026/03/04/the-spec-driven-development-triangle.html) that define what they want, and a lot of time reviewing the output and tests to make sure it’s good. Everything has moved to the edges. I’m seeing the same thing with designers. They automate the internals, design the systems up front, and never let the AI drive itself.

**Don’t formalize yourself.** Take the desire path and seek out diversity. If you don’t seek out diversity, you’ll be a terrible agent of deviation for these models.

* * *

![OpenAI's October 6, 2026 post: Sharing AI progress in mathematics. We're releasing a broad range of new mathematical results produced by an internal frontier model.](https://www.dbreunig.com/img/cng_2026/cng_2026.033.jpeg)

When you write about AI, you’re always updating your presentation. News always breaks that updates, confirms, or upsets a point you’re making.

And this one is really important. How many of you saw this news?

One of the marketing devices and benchmarks the labs use, specifically Anthropic and OpenAI, is to find really, really hard math problems that no one has solved and then solve them with their models. It’s a cool trick, and really impressive!

But we have to recognize that much of the hard part of math was *coming up* with these problems: defining them so they’re crisp and watertight, so they become the perfect target. An entire field has been producing these, for centuries.

And then OpenAI and Anthropic just *solve them*. Suddenly, these proofs arrive. No one knows what they mean. They just show up. As a field, as a community, that’s a challenge!

* * *

![Two Mastodon posts from Terence Tao contrasting Math 1.0, where a breakthrough proof generates talks, workshops, collaborations, and new mathematicians, with today, where AI prompters solve problems autonomously and fewer of those activities follow. Below, his quote: This process cannot be easily reversed: a problem that has been solved cannot be somehow reverted to become unsolved, and even the mere knowledge that a solution exists contaminates efforts by both humans and AI to find alternate routes to the problem that reveal additional insights.](https://www.dbreunig.com/img/cng_2026/cng_2026.034.jpeg)

We’ve already talked about Brian Eno, so let’s read from Terence Tao. As a leading voice in mathematics, [he wrote a series of posts on Mastodon in response to these AI proofs](https://mathstodon.xyz/@tao/117395267721642920).

He describes how, in traditional mathematics, “Math 1.0,” a breakthrough proof drove the field forward. The authors get invited to give many talks and meet other experts. They make new connections. Workshops get set up. People reach out, collaborations form, and professorships follow. As a result of all this activity, the proofs get digested, streamlined, placed in context, and situated in the field.

AI tools, used responsibly, can support this. But right now the opposite is happening. Problems are being solved autonomously by AI prompters who have no interest in the broader field, and the problem just lands, solved. Nothing spurs the seminars, the workshops, the collaborations, or the other activities. His thread (I left out the optimistic ending) argues we have to figure out Math 2.0. If proofs are no longer the engine that gives us reasons to meet, engage, and get weird, we have to find another reason.

I love this quote from later in the thread: “This process cannot be easily reversed: a problem that has been ‘solved’ cannot be somehow reverted to become ‘unsolved,’ and even the mere knowledge that a solution exists ‘contaminates’ efforts.”

To me, this is paving the desire path. It’s Taylorism in the factory. Once a problem is solved, you lose the ability to have conversations around it. It becomes finite, and you have to find a new way.

* * *

![The three rules plus a fourth: Some questions are invitations. When the conversation is worth more than the answer, talk with people.](https://www.dbreunig.com/img/cng_2026/cng_2026.035.jpeg)

So I added a fourth rule: **Some questions are invitations.**

Let’s imagine we have a perfect AI agent. It makes no mistakes, knows everything, and can answer any question and perform any task reliably.

If you choose to give it a task or question, what are you giving up? What are you losing?

First, you lose the opportunity for enjoyment. That’s the obvious one. If I *want* to do the task, let me do it.

Second, you lose the opportunity for experience and learning. Making mistakes and all that other fun stuff. We’re all familiar with this.

The third thing you lose is less talked about, and it’s happening now. We don’t have to wait for godlike AI.

When you ask the AI, you give up the opportunity to ask a person, to start a conversation, and to build a connection.

Over the last year, I’ve been starting a company. A couple of times I said to people, “I don’t know how anyone did this before agents.” If I have a weird business or legal question, I can just ask Claude and get a pretty good answer. About six months in, I realized I should have been asking my advisors these questions. These questions are an excuse for a call. They’re an excuse to check in, chat, and build the relationship.

So please learn to spot these easy questions. Be a little anti-Taylor. Taylor wouldn’t want you to walk over to someone’s desk and chat to get the answer. The answer should be detailed in the one perfect process you’re working through. Find the opportunities for deviation, where the conversation is worth more than the answer.

Talk with people.

That’s how you remain human. There will always be room for humans, because these machines will always be bland.
