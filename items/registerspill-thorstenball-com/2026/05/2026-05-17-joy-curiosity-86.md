---
title: 'Joy & Curiosity #86'
link: https://registerspill.thorstenball.com/p/joy-and-curiosity-86
source: registerspill-thorstenball-com
published: 2026-05-17T05:22:28Z
updated: 2026-05-17T05:22:28Z
first_seen: 2026-09-19T21:30:23.395188495Z
authors:
- Thorsten Ball
summary: Interesting & joyful things from the previous week
content: extracted
html: 2026-05-17-joy-curiosity-86.html
preview:
  file: 2026-05-17-joy-curiosity-86.preview-181ce2ead8ad.webp
  width: 256
  height: 144
  color: '#7f7e7b'
images:
- source: https://substack-post-media.s3.amazonaws.com/public/images/6b3bad9a-d184-461b-a884-16e8e3fa8d99_5712x4284.jpeg
  original:
    file: 2026-05-17-joy-curiosity-86.image-8872d6edbce6.jpg
    width: 5712
    height: 4284
  color: '#c9c9c8'
- source: https://substackcdn.com/image/fetch/$s_!8l-q!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F579faa66-8729-497c-bc48-43c8a1c95e5f.tif
  original:
    file: 2026-05-17-joy-curiosity-86.image-b27c5de27d1c.png
    width: 964
    height: 42
  variants:
  - file: 2026-05-17-joy-curiosity-86.image-c7aaeb1b2603.webp
    width: 320
    height: 14
  - file: 2026-05-17-joy-curiosity-86.image-3e8af5ee28cf.webp
    width: 640
    height: 28
  - file: 2026-05-17-joy-curiosity-86.image-b5119d73fd99.webp
    width: 960
    height: 42
  - file: 2026-05-17-joy-curiosity-86.image-ce55a6064ae6.webp
    width: 964
    height: 42
  color: '#000000'
- source: https://substackcdn.com/image/fetch/$s_!8KC1!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ffbc5b300-35f4-4a9f-9b4f-5c6e959de07b.tif
  original:
    file: 2026-05-17-joy-curiosity-86.image-01f3830591c5.png
    width: 967
    height: 32
  variants:
  - file: 2026-05-17-joy-curiosity-86.image-000b4789dfd2.webp
    width: 320
    height: 11
  - file: 2026-05-17-joy-curiosity-86.image-a5f6c63f50de.webp
    width: 640
    height: 21
  - file: 2026-05-17-joy-curiosity-86.image-7a572e110e63.webp
    width: 960
    height: 32
  - file: 2026-05-17-joy-curiosity-86.image-9252f866f0d9.webp
    width: 967
    height: 32
  color: '#000000'
---

There it is: light at the end of the tunnel. After a week of scaling, tuning, and modifying our infrastructure, [Amp Neo](https://ampcode.com/news/neo) is now rolling out to more users. Update your Amp and watch out for a welcome screen that, some people say, “should be marked NSFW” because it’s so sexy.

Boy do I have stories to tell. Here’s a very 2026 one. We were debugging database issues in production but could only see symptoms and not what caused them. Spike on this graph correlates with spike on that graph alright, but why? What causes it?

If you’ve ever done any sort of investigation like this, you know that there are a lot of things you can check: are processes restarting? Is the rate of errors going up? Is a CPU pinned? What’s the memory usage look like? And so on.

But it’s 2026, isn’t it? So what ended up happening is that we’d send runbooks to each other. And runbook here means “prompt”: a long Markdown document, created by an agent, that instructs the reading agent to check a series of metrics and data points. You’d take a prompt, paste it into Amp, Amp would go off and compile a report. It would say something like “this cluster looks healthy, except for the otel collectors which are OOMing, but that’s benign for now, the smoking gun is the log messages that say … because those correlate with … showing up at the same time, which means …” And then you’d take that report and send it to a fresh Amp and that fresh Amp would say that yes, that’s right, let’s fix this, and you’d sit there and think that there’s *no way*, no way that you would’ve dug through the logs and metrics at that speed with that precision, but it’s 2026 so the agent did it for you.

[![](https://substackcdn.com/image/fetch/$s_!8l-q!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F579faa66-8729-497c-bc48-43c8a1c95e5f.tif)](https://substackcdn.com/image/fetch/$s_!8l-q!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F579faa66-8729-497c-bc48-43c8a1c95e5f.tif)

- antirez with a [“short story of a long development”](https://antirez.com/news/164) : “Because I had AI, I took no compromises, and I decided to go the extra mile.” This is a well-grounded, realistic portrayal of what it looks like to use agents in the first half of 2026. “For high quality system programming tasks you have to still be fully involved, but I ventured to a level of complexity that I would have otherwise skipped. AI provided the safety net for two things: certain massive tasks that are very tiring (like the 32 bit support that was added and tested later), and at the same time the virtual work force required to make sure there are no obvious bugs in complicated algorithms.”

- So, the [Bun rewrite from Zig to Rust has been merged](https://github.com/oven-sh/bun/pull/30412) and not only did that add around a million lines of Rust code to the `main` branch, it also created a million hot takes. Most of them are silly (“there’s unsafe {} in there”). But [there are good ones](https://x.com/tekbog/status/2054936125106794895?s=46) : “if after the bun rust rewrite and 1M code merge in a few days you still think that software engineering hasn’t changed massively idk what to tell you” It’s an automated Zig-to-Rust rewrite of a million lines of code and all the tests pass and it took a week. Just as a reminder: this time last year we complained about models leaving “// Deleted this function” comments in code when deleting code.

- “ [Complexity has to live ](https://ferd.ca/complexity-has-to-live-somewhere.html)*[somewhere](https://ferd.ca/complexity-has-to-live-somewhere.html)*. If you are lucky, it lives in well-defined places. In code where you decided a bit of complexity should go, in documentation that supports the code, in training sessions for your engineers. You give it a place without trying to hide all of it. You create ways to manage it. You know where to go to meet it when you need it. If you’re unlucky and you just tried to pretend complexity could be avoided altogether, it has no place to go in this world. But it still doesn’t stop existing.”

- matklad on [Learning Software Architecture](https://matklad.github.io/2026/05/12/software-architecture.html) . Great read. And plus one thousand on the recommendation to read [Pieter Hintjens](http://hintjens.com/) and [the importance of Conway’s Law](https://registerspill.thorstenball.com/p/the-most-powerful-law-in-software) .

- [John Tromp’s Binary Lambda Calculus universal machine.](https://github.com/tromp/AIT/blob/master/uni.c) This is like reading a beautiful poem.

- [Sense of Urgency](https://nbt.substack.com/p/sense-of-urgency) : “This week, I had the privilege of dining at The French Laundry (TFL). \[…\] Since the first Michelin Guide of San Francisco in 2007, TFL has received 3 Michelin stars every single year, and is one of the best restaurants in America. \[…\] Looking around, there were a few inescapable details, none more so than a clock placed in the center of a blank wall facing the kitchen, with a plaque below it that reads Sense of Urgency.” I’ve been [thinking about this clock for years now](https://x.com/thorstenball/status/1555466744482959360) . Yes, I’ve considered getting a plaque like that. A post-it would do it, surely, but imagine having a plaque like that. No longer would people walk into my office, see the camera and the lights and go “are you a streamer?” but instead they’d ask “what does that mean?” and I’d tell them and— okay, now thinking about it, the plaque would be too much.

- [Aperio](https://aperio-lang.github.io/aperio/introduction.html) is a new programming language and here is its pitch: “Every language designed before 2023 was optimized for a single tradeoff: minimize friction between human cognitive capacity and machine execution. Assembly to C to managed runtimes to DSLs were different points on the same line. In an LLM-driven workflow, those languages don’t get cheaper to use — they get more expensive. The cost just hides in the LLM’s token count, its retry rate, and the latency it eats per turn. Pre-LLM languages are a hidden tax in the LLM era.” I find this very interesting, the question of what a language optimized for LLMs would look like. Does it look like Aperio? Does it really introduce new terms and ideas, such as “locus” and “loci” here? Does a language need to be in the training data? Or can raw intelligence master it?

- [en.dev](https://en.dev/) is “a one-person open source software company building mise & friends.” I’m intrigued by this page. It’s a very, very good page and I don’t think I’ve seen a Sponsors section quite like that. I like it.

- Amazing: [ratty-term.org](https://ratty-term.org/) . That landingpage and video are *great*, but make sure to read [the blog post](https://blog.orhun.dev/introducing-ratty/) too. It has some incredible hall-of-fame sentences. This one, for example: “your terminal cursor is a spinning rat”. But fear not: “you can configure the cursor to be your dog”.

- Daniel Lemire: [We see something that works, and then we understand it](https://lemire.me/blog/2025/12/04/we-see-something-that-works-and-then-we-understand-it/) . “Both the linear theory and the waterfall model are forms of thinkism, a term I learned from Kevin Kelly. Thinkism sets aside practice and experience. It is the belief that given a problem, you should just think long and hard about it, and if you spend enough time thinking, you will solve it.”

- [Geography is four-dimensional](https://sive.rs/4d) , by Derek Sivers.

- This was fascinating: [We don’t know why Malawi is poor](https://newsletter.deenamousa.com/p/we-dont-know-why-malawi-is-poor?hide_intro_popup=true) .

- [Googlebook](https://googlebook.google/) looks really interesting. The hardware I don’t care about, but the idea that you can have a self-modifying operating system: *that*’s interesting. It’ll hinge on how much power Gemini will have, though. Right now I have Gemini in Gmail but every time I try to use it it feels like using Siri: sorry, I can’t help you with that. But if they truly unleash Gemini and you can whip up widgets and apps and whatnot and it truly knows how you use the computer? Interesting. Also worth a quick read: [their idea of solving “this and that” with the mouse pointer for the AI era.](https://deepmind.google/blog/ai-pointer/)

- Speaking of self-modifying operating systems: remember Emacs? Thomas Ptacek is wondering whether we’re entering the era of [The Emacsification of Software](https://sockpuppet.org/blog/2026/05/12/emacsification/) . In the Emacs world, it’s common to fork & modify and never contribute back. You clone a bit of code into your emacs.d and then you personalize it there. Now, with agents, you can do that with a lot more software.

- [AI, the Shadow Prince](https://callmemapo.substack.com/p/ai-the-shadow-prince) : “In none of these instances does AI need to be ‘alive’ to wield power. It doesn’t need to stage a violent revolution or hatch a takeover plan or develop desires. It need only be so efficient, so influential, and so deeply integrated into the loop of human decision-making that it effectively runs the show. We cede authority to AI ministers unwittingly, and out of convenience rather than fear.”

- When I was working at UPS I [did a job pretty much close to this one](https://x.com/adcock_brett/status/2054603963996278786) , except you had to sort the packages into three to five lanes, depending on the zip code.

- [I spent my whole career building passive income. Here’s what I got wrong](https://dariusforoux.com/i-spent-my-whole-career-building-passive-income-heres-what-i-got-wrong/) : “The grass had been long for a while and every time I looked at it, I didn’t like it. So I went out and mowed it. It took a couple of hours. It looked great when I was done. And I felt genuinely good afterward. That’s the secret to living a happy life. Write when you don’t feel like it. Work out when it’s easier to skip. Do your taxes instead of postponing again. Go outside when you’d rather stay in.”

- Yes,[I do like the new Spotify logo](https://old.reddit.com/r/mildlyinfuriating/comments/1td4lt4/the_new_spotify_logo_is_absolute_horse_shit/) . Mark my words: Minimalism is dead, flashy is dead, pixel perfection is dead. What’s alive and desired: graininess, details, rawness, honesty. In a world in which everything can be copied, the things that are truly unique will stand out.

- Sean Goedecke provoking thoughts: [Software engineering may no longer be a lifetime career](https://www.seangoedecke.com/software-engineering-may-no-longer-be-a-lifetime-career/) . I mean, who knows? No one, as Sean admits. But incentives have shifted, massively, and it’s unclear what’ll come of that.

- I’m typing this on a new [Lofree Flow 2 keyboard](https://www.lofree.co/products/flow-2-84-low-profile-mechanical-keyboard) that I got yesterday. So far, so good. I like the sounds a lot, but still need to get used to the typing.

- Daniel Stenberg, author of curl, on Anthropic’s Mythos: [Mythos finds a curl vulnerability](https://daniel.haxx.se/blog/2026/05/11/mythos-finds-a-curl-vulnerability/) . Emphasis on “a” — a single one. On one hand: yes, I’m skeptical, too. On the other: curl is a project that has had many eyeballs over the years. Not sure how much there’s left to discover.

- [whatcable](https://github.com/darrylmorley/whatcable) : “A small macOS menu bar app that tells you, in plain English, what each USB-C cable plugged into your Mac can actually do, and why your Mac might be charging slowly.” Makes me want to check every cable I have. I might.

- [A History of IDEs at Google](https://laurent.le-brun.eu/blog/a-history-of-ides-at-google) . Very, very interesting! Reading “Code Intelligence” and “Google” brings back a lot of memories.

- “If you're in the business of building things that run on computers long enough, I think you will eventually acquire a favorite bug story. [This is a short story about mine](https://george.mand.is/2026/05/my-favorite-bugs-invalid-surrogate-pairs/) .”

- “ [This is the cart before the horse, surely](https://youtu.be/6MIaS9uhRAk?t=1284) . Who gives a fuck what people think they want to see at that moment? \[...\] They come to you, because they’re interested in what you want to do. And if they don’t like it, they can go elsewhere.” This is going to sound incredibly naive, but I have to admit that for many, many years now, decades actually, I haven’t considered the possibility of just ignoring the audience and building something that you want and still putting it in front of an audience. I’ve listened to this episode at the start of the week and I still think about that bit.

[![](https://substackcdn.com/image/fetch/$s_!8KC1!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ffbc5b300-35f4-4a9f-9b4f-5c6e959de07b.tif)](https://substackcdn.com/image/fetch/$s_!8KC1!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ffbc5b300-35f4-4a9f-9b4f-5c6e959de07b.tif)

No posts
