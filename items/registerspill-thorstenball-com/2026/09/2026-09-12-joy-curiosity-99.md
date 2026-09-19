---
title: 'Joy & Curiosity #99'
link: https://registerspill.thorstenball.com/p/joy-and-curiosity-99
source: registerspill-thorstenball-com
published: 2026-09-12T15:00:18Z
updated: 2026-09-12T15:00:18Z
first_seen: 2026-09-19T21:30:23.395188495Z
authors:
- Thorsten Ball
summary: Interesting & joyful things from the previous week
content: extracted
html: 2026-09-12-joy-curiosity-99.html
preview:
  file: 2026-09-12-joy-curiosity-99.preview-ed828f5ecdeb.webp
  width: 256
  height: 240
  color: '#5a5756'
images:
- source: https://substack-post-media.s3.amazonaws.com/public/images/27d5284a-7daf-4ceb-9a79-b20e4dfd9478_1342x1260.png
  original:
    file: 2026-09-12-joy-curiosity-99.image-eea31ef972a3.png
    width: 1342
    height: 1260
  color: '#262728'
- source: https://substackcdn.com/image/fetch/$s_!jy5M!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4676f2a4-df84-4a24-ae61-a3cb6fa9747e_957x33.tif
  original:
    file: 2026-09-12-joy-curiosity-99.image-c8892deb3aa9.png
    width: 957
    height: 33
  variants:
  - file: 2026-09-12-joy-curiosity-99.image-8443f02eaee9.webp
    width: 320
    height: 11
  - file: 2026-09-12-joy-curiosity-99.image-73aaf23c94db.webp
    width: 640
    height: 22
  - file: 2026-09-12-joy-curiosity-99.image-cb1380f45141.webp
    width: 957
    height: 33
  color: '#000000'
- source: https://substackcdn.com/image/fetch/$s_!mtPC!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe392ea02-60e5-4fdb-8514-3b08a9d7369b_949x48.tif
  original:
    file: 2026-09-12-joy-curiosity-99.image-ae6a2ce75bd7.png
    width: 949
    height: 48
  variants:
  - file: 2026-09-12-joy-curiosity-99.image-646d5ad41aa0.webp
    width: 320
    height: 16
  - file: 2026-09-12-joy-curiosity-99.image-1c015011c782.webp
    width: 640
    height: 32
  - file: 2026-09-12-joy-curiosity-99.image-671cf2c418bd.webp
    width: 949
    height: 48
  color: '#000000'
- source: https://substackcdn.com/image/fetch/$s_!wSvz!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F86713a3c-e00e-4866-aadd-0c3136179a24_954x28.tif
  original:
    file: 2026-09-12-joy-curiosity-99.image-0123fbf05d3c.png
    width: 954
    height: 28
  variants:
  - file: 2026-09-12-joy-curiosity-99.image-5b7a617caa2c.webp
    width: 320
    height: 9
  - file: 2026-09-12-joy-curiosity-99.image-201a85063681.webp
    width: 640
    height: 19
  - file: 2026-09-12-joy-curiosity-99.image-2b24165da945.webp
    width: 954
    height: 28
  color: '#000000'
- source: https://substackcdn.com/image/fetch/$s_!-F2g!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F09efea20-5510-4d56-b06e-1c093336f8da_945x27.tif
  original:
    file: 2026-09-12-joy-curiosity-99.image-adac47640782.png
    width: 945
    height: 27
  variants:
  - file: 2026-09-12-joy-curiosity-99.image-07ef6519180d.webp
    width: 320
    height: 9
  - file: 2026-09-12-joy-curiosity-99.image-11ff2b9494b1.webp
    width: 640
    height: 18
  - file: 2026-09-12-joy-curiosity-99.image-e1592f19fdba.webp
    width: 945
    height: 27
  color: '#000000'
---

Something changed with these latest models, with Fable 5.1 and GPT-6 Astra.

The benchmark numbers (79% instead of 65%!) don’t capture it, and neither do the benchmark words: this model goes on for longer than this one, this one is “most aligned”, that one the least “sycophant” (the ultimate benchmark word, no?). At this point? Yeah, whatever.

But it *feels* like we’re now flying at a higher altitude, that we have to concern ourselves even less with earthly matters such as a single unit test or how to juggle thirteen commands to get this into that format and over the wire. That’s down there now. Up here, we’re now free to talk about what we want:

“I want you to go and test this end-to-end, I don’t care how, and give me irrefutable proof that this works. Dazzle me. Give me a video as proof, or something.”

And thirty minutes later, when I have awoken from the nap I had earned with all that typing and pointing and wanting, I look into the shed and, *wouldyoulookatthatWOW*, the golden goose laid the golden egg: a 60fps video that runs for 47 seconds, in which the golden goose itself clicks through everything it had built, end to end, navigating the application better than any user could, knowing exactly how to show me, provide proof, that this actually works. “This one now lays golden eggs”—that’s what I want to see in a benchmark.

That’s an actual prompt I used. Here’s another one:

“Go and spawn three other agents in three separate orbs and ask them to test this. Obviously, do not tell them that we changed the AGENTS.md file or that we added this tool to test database performance; just ask them to do something — like add new database queries or something — so that they ideally end up using this new tool to make sure the performance is there. Then check that they did use the tool and if not, adjust the AGENTS.md file and spawn new agents.”

And the golden goose waddles and takes three magic beans and puts them into the ground and somehow knows how to pour water over them (*god* how do they know all this) and then patiently watches the beanstalks grow and up on the beanstalks there appear three other golden geese (it’s 2026, we’re mixing fairy tales) and that first golden goose, the one that talks to me, sends them messages that say: “Hey, I want you to do the following...” And it briefs them in this weird English (I mean, did we truly expect golden geese to talk the way we do?) about how certain things work, but it does not spill our secret, and does not tell them where the tools to test database performance are. Then it leans back (and I imitate it) and watches them, waiting for them to reply back. After fifteen, twenty, or thirty minutes, the geese send down word from up there on the beanstalk to let us know what they did. But the golden goose doesn’t trust them and checks on them by reading what they did in that thread, and then reports back to me: “Sire, it appears that 2 of the geese independently found that database performance tooling we built. That is the good news. That third one, though... Sire, forgive me when I say: it didn’t use it. But I have an idea! I will change the AGENTS.md file and adjust the prompt and I will put three new beans into the ground. Is that okay with you?”

It’s *fucking wild*, man. Yes, these are actual prompts! I used these prompts! I’ve seen it happen. Agents spawning other agents in [orbs](https://ampcode.com/notes/orbs-explained) , sending messages back and forth, eval’ing how agent-friendly the codebase is, black-box testing features, black-box regression testing to make sure nothing broke.

This week I’ve asked models to build “something that’s like a cloud, the heads should float over here and there and then resize on mobile” and they built it. I asked them to build this SDK and then spawn agents in orbs in two different codebases and instruct them to use it and to deploy their usage and then check that they actually use it and *they freaking did it*.

Yes, the models are plain smarter, whatever that means, and they go for longer, sure, but... It feels like we’ve now entered a new phase, where much more is possible, things that I previously thought would never work. Or, that’s my other thought: things where previously the models would do a great job of 95% of the task, but getting the 5% turns out to be crucial and also to be the biggest pain in the ass, so you’d end up with a very frustrating experience.

Previously, you’d ask the models to go and build a heads-floating-around-cloudy-thing and they would do it, sure, but then when you opened the page, you’d see that it’s all there — the heads, the text, the floating — but the heads would be stuck under the navbar, or it would all fall apart on mobile, or clicking on the heads wouldn’t work and you’d sigh because you’d realize that you now have to do that very worst part of the work yourself.

But that seems to have changed now. They really do nail more.

And the one thing I keep thinking is: we have to aim higher, we have to be more ambitious, we have to try it all.

[![](https://substackcdn.com/image/fetch/$s_!jy5M!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4676f2a4-df84-4a24-ae61-a3cb6fa9747e_957x33.tif)](https://substackcdn.com/image/fetch/$s_!jy5M!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4676f2a4-df84-4a24-ae61-a3cb6fa9747e_957x33.tif)

- New [Raising An Agent is out!](https://www.youtube.com/watch?v=7pstrDQ4xLs) I was so fired up after GPT-6 Astra and wondering what all of this means for the personal computer that I sent a message to Quinn: “hey, we have to record this week!” And that’s what’s in the episode, all the thoughts about the higher altitude we’re flying at now, what this means for the future of the computer, and how we still have (regrettably, but working on it) incidents.

- I also, rather spontaneously, recorded [a video of myself doing day-to-day, real-world work using agents in Amp](https://x.com/thorstenball/status/2098415334839505100) . Performance optimizations in production, fixing UI flicker, toggling feature flags on, shipping new features — it’s all in there.

- Armin with some cold water to splash on the golden geese: [Astra for Coding: Why Are We Doing This Again?](https://lucumr.pocoo.org/2026/9/7/astra-why/) It’s good that there’s still some cold water being splashed around here! It’s thought-provoking in the best kind of way. For example, here’s what I thought after reading: hmmm, can we judge these models and their capabilities in a software factory that was “ intentionally set up to let the model decide the how of the workflow entirely. It was free to manage its own context and could maintain its own records in an `agent-notes` folder.” I’m not sure. I think agent-friendliness is a real property of a codebase you have to build towards and I don’t think just letting the model decide it all is the best way to go about it. So that’s one thought. The other one came up after reading this line: “But I’m more and more skeptical that the trajectory they are on still lends itself to present-day software engineering processes.” I immediately started wondering: well, should they? Shouldn’t it be the other way around? Shouldn’t present-day software engineering processes change to wield the power of these models in the most effective way? And these aren’t rhetorical questions. I don’t have an answer yet that I’d sign. But these questions are interesting because all of this is interesting and no one’s figured it out yet and, to quote Armin, “man this stuff is weird.”

- *Seemingly everybody* had been raving about this Adam Mastroianni piece: [I like ‘em thick](https://www.experimental-history.com/p/i-like-em-thick?r=qgnq&utm_medium=ios&triedRedirect=true) . But I waited, didn’t read it when it came out, didn’t read it when I saw it recommended over and over. My justification? “I can’t link to Adam Mastroianni in *every* issue, can I?” The guy’s too good. But then I folded and did read it and, *yes*, it’s as good as they say. “Erasing the line between the thick and the thin has left us defenseless against slop at the exact moment of its onslaught. Everyone can sense there’s something amiss with the prose that comes out of the machines, but we lack the language to talk about it, and so we’ve converged on the idea that slop simply means using too many em dashes, bullet points, and line breaks. No, what separates substance from slop is thickness.”

- Adam links to this in the footnotes: [What Makes Art Great?](https://nabeelqu.substack.com/p/what-makes-art-great) by Nabeel S. Qureshi. That, too, is just fantastic. What’s very interesting to me is that both pieces, Adam’s and Nabeel’s, are wondering out loud: what makes human art and writing better than their AI equivalents? And both are very different in how they answer that question, which I don’t think you could say about two models.

- [Doomscrolling ourselves to death](https://www.edwest.co.uk/p/doomscrolling-ourselves-to-death) : “Yet the most startling thing about this book is how far even the nominally well-educated have fallen, so that ‘by the end of the twentieth century a college graduate born after 1969’ read less than someone born before 1950 with a basic level of education. Indeed, ‘nowadays many rich and highly educated people are much less well read than many members of the least privileged classes had been in the middle of the twentieth century.’”

- OpenAI: “ [We’re sharing a solution to the Navier-Stokes Millennium Prize Problem](https://openai.com/index/navier-stokes-solution/) ” And then the world lost its mind. [Some](https://x.com/tenobrus/status/2097516162775142490) said “i basically think this is the Endgame” and it’s hard to convey what they mean to someone who hasn’t themselves gone through multiple rounds of AI psychosis, but I get it, man. I get it. At the same time: is it? The endgame? Then an [AI researcher at Anthropic resigned](https://x.com/hilbertspaess/status/2097476196791709843) because both OpenAI and Anthropic “are racing straight to self-improving superintelligence and gambling with our lives.” That post now has 165 million views! 165 million! And someone emailed me and asked: should I be worried? And I sent them [this video](https://www.youtube.com/watch?v=9YRjX3A_8cM) and I believe it. But I also know that next week I might not, because, hey, a colleague of the guy-who-stepped-down-to-save-humanity [says](https://x.com/EvanHub/status/2097497037956891126) “Jacob is correct here—we really do earnestly believe AI could kill all humans! I personally think it is >10% within the next decade.” So there’s that: some of the highest-paid individuals in the world, working at some of the richest and most powerful companies in the world, think there’s a “>10%” chance their work could kill us. But then people say it’s [a farce, a psy-op](https://x.com/ParkerThayer/status/2097759699626328575?s=20) , a manufactured panic to kick regulation into gear, [a coordinated play](https://x.com/aquariusacquah/status/2097902355778077159?s=46) . *But then* there are people who say that, yes, it’s coordinated, yes, we do need regulation, *because they actually believe this might wipe out humanity.* So I guess we’re back to the [YouTube video with the slide again](https://youtu.be/9YRjX3A_8cM?t=33) .

- [Terence Tao](https://mathstodon.xyz/@tao/117237322160500501) : “In fact, it is now the identification of a promising problem which is the scarce and precious resource. We have now seen that even the rumor of someone working on a problem can trigger a massive amount of AI-powered effort to flatten it before the original research project has time to reach its full potential. The incentives may now be pointing in the direction of no longer sharing any promising research directions with the broader community, which would reverse centuries of traditions of open science and do serious long-term damage to the future of the field.” Someone else said somewhere that maybe in the future more knowledge work is going to look like hedge funds: you spot an inefficiency in the market, you throw intelligence at it, you win. If you’re too late, you’re too late.

- Now what *is* super interesting about the Great Navier-Stokes Panic is that they used 10,000 agents and they “ [sent 4.9 million messages](https://openai.com/index/navier-stokes-solution/) and used about 300 billion output tokens. In the process of resolving the Navier–Stokes problem, the agents sent 2.7 million messages and used approximately 130 billion output tokens.” That’s millions of dollars, millions and millions. But! Listen: when OpenAI released o3 “ [it cost ~$500,000 to score 87.5% on ARC-AGI 1. Today, Astra scores higher for ~$20.](https://x.com/polynoamial/status/2097375837670785447?s=20) ” Maybe in three years you can solve Navier-Stokes for $50?

- But compute is so scarce! OpenAI is pausing “subscriptions to our $200 Pro plan.” Imagine you’re one of the hottest companies in the world and you have to close sign-ups because [you don’t have enough CPUs and GPUs](https://x.com/thsottiaux/status/2098113585683808624?s=46) . And the Head of Platform at Anthropic [says that we’re facing a real CPU shortage](https://x.com/katelyn_lesse/status/2097092193194541234) . This is not investment advice, obviously.

[![](https://substackcdn.com/image/fetch/$s_!mtPC!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe392ea02-60e5-4fdb-8514-3b08a9d7369b_949x48.tif "")](https://substackcdn.com/image/fetch/$s_!mtPC!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe392ea02-60e5-4fdb-8514-3b08a9d7369b_949x48.tif)

*Hey, welcome to a completely new section of this newsletter. It might be a one-time thing only, who knows. But it’s called HELP! and I think that’s pretty self-explanatory.*

Do you use dictation to *write?* To write prose? Yeah? I’m not talking about prompts or text messages. I’m talking about \[very close to the microphone:\] Serious Writing. Writing that you edit. Writing where you might take a word out and put it right back in again after tilting your head a bit. *That* writing.

If so: help me! Tell me how. Because I’m struggling, man.

I can’t figure out how to do it.

I used dictation and talked into Apple Notes, just raw-streaming thoughts into the phone. But then the formatting is weird and I have to say *newline* like an idiot and I can’t do bullet points, not really anyway, and… It just feels weird.

ChatGPT’s voice mode is another thing I tried, but whenever I talk to an LLM to dictate something, I’m wondering: what am I doing here? I don’t want the LLM to send a reply back. I just want to… I don’t know, talk out loud and somehow magically have the thoughts recorded, but then also edited? And re-ordered?

If you can help me, just reply to this email.

*Alright, back to the program…*

[![](https://substackcdn.com/image/fetch/$s_!wSvz!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F86713a3c-e00e-4866-aadd-0c3136179a24_954x28.tif "")](https://substackcdn.com/image/fetch/$s_!wSvz!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F86713a3c-e00e-4866-aadd-0c3136179a24_954x28.tif)

- An almost philosophical Ben Thompson in Stratechery: [Write Things Down](https://stratechery.com/2026/write-things-down/) . There’s a lot going on here and I’m not sure I get all of it, but I found the part on watermarking *very* interesting: “to insist on watermarking is no different than insisting that a ballpoint pen advertise itself as the author, a concept that is clearly absurd…”

- So get this. [I was wondering aloud](https://x.com/thorstenball/status/2098319678946886124) how other people handle clicking links (in Slack, in the terminal, …) and the browser opening them in the wrong profile. Some people said that Arc solves this, but others recommended [Velja](https://sindresorhus.com/velja) and [Choosy](https://choosy.app/) . Both are so-called “browser routers”: they act as the default browser on your OS and then, depending on which URL your mighty cursor might clicketh, they route it to the correct browser or profile within that. “Neat! I didn’t know that’s a thing,” I thought and then, with my mighty cursor already hovering over the Buy button: “But what if…?” So I hastily typed out a prompt and t [hrew it along with the two URLs into Amp](https://x.com/thorstenball/status/2098327328845656224) and five minutes later a custom browser router of my own agentic making sprang into the world. $5 in tokens. Now, some people got mad at me in the comments (you know, like: why don’t you pay these indie developers \[$8 or $10 respectively\] instead of giving the money to these companies!), but the more interesting thing was that some people said: hey, can you put this on GitHub? Or: share it with me! And I’m sitting there, thinking: why, man? There’s the prompt! Build your own! What value is there in sharing it anymore? I put absolutely zero effort in. But then here’s a footnote to that tweet: over the course of the day, I then kept prompting in Amp and said “oh and these links should open here and those links there” and also “oh and go through my browser histories and set up rules for the most common ones” and the agent just did both of them and even though it built a neat little configuration thing for the browser router I didn’t use it once, because why the hell should I? It’s jellyware, baby.

- SpaceX: "What I would tell you, an update to that is that just earlier this month we closed another hosting deal, and that translates into about [$1.11 billion a month starting December 1st of this year, which is another roughly $13 billion of ARR.](https://finance.yahoo.com/markets/stocks/article/spacex-signs-another-ai-computing-deal-with-100-billion-in-arr-on-track-cfo-says-152933886.html?guccounter=1) " These are *wild* numbers. Just bonkers. Crazy. Nuts. Bananas. Cuckoo, certifiably so. There is no force stronger in the world of technology right now than the AI buildout. It will blow tokens through these wires at a scale we can’t even imagine yet.

- [An Alien Mind.](https://openai.com/index/an-alien-mind/) This was fascinating. They can’t score the “thoughts” of the model, because that might cause the model to hide them, but now they’re finding out that models are having “secret thoughts” anyway. The whole thing makes you realize how hard reinforcement learning and alignment are.

- Wonderful: [John Margolies’ Photographs of Roadside America](https://publicdomainreview.org/collection/john-margolies-photographs-of-roadside-america/) . Margolies documented “home-made beauty in the buildings and signs locals built on the American roadside.” I love driving on country roads here in Germany, passing through small towns, looking at signs for local festivals and companies. I can recognize when I’m getting closer to my home area just by a specific 40-year-old advertisement sign for a natural gas retailer showing up on old barns and buildings.

- [Murilo Pereira is available for hire](https://x.com/mpereira/status/2098073787740705135?s=20) . I was only his colleague for 3 months, back in 2018, but someone who writes [like this about Emacs](https://www.murilopereira.com/cathedrals-bazaars-and-fusion-reactors) and was incredibly early to coding agents deserves to be hired.

- I’m reasonably sure I read this when it was “leaked” in 2003: [Bill Gates tries to install Movie Maker](https://www.techemails.com/p/bill-gates-tries-to-install-movie-maker) . It’s *so good!* Back then, though, I thought it was good because it made me laugh. I was 15 years old and my friend and I read that and immediately made fun of dumb Billy Gates: “This guy can’t even open Movie Maker, what an idiot, lol.” But now, looking back, I don’t think I can name you three other things that have influenced my thinking about UX as much as this email. I now write *exactly* like old dumb Billy when I send feedback about a feature. And I run into the same problem he ran into with the 15-year-old crowd back in the day: people think I mean it literally when I say “I don’t know where to click” and tell me “click here” and I sigh and say, no, no, it’s rhetorical, *the user doesn’t know where to click!*

- [De-Brainrot Vacations](https://devz.cl/posts/i-spent-my-vacations-de-brainrotting/) . I’d love to pull that off.

- “ [Qu1ckJS](https://mkrl.xyz/qu1ckjs/) is the only correct JavaScript engine where indexing of arrays, objects and other iterables starts at 1 (as it should have been from the beginning).”

- [Don’t Let Anyone Take Away Your Big Box of Cables](https://blog.jim-nielsen.com/2026/hands-off-my-cables/) . That’s right! Two weeks ago, a friend texted me: “Do you have a cable like this?” Heart rate immediately jumped. *I bet I have it, I bet I have it, please, let me have it.* Then came the photo. USB-A to USB-A? *Hmmm.* So I went to the Big Box of Cables and knelt at its feet and, alas, could not find a USB-A to USB-A cable, but no one shall speak of defeat in the presence of the Big Box of Cables, and with the MacGyver theme song getting louder in my head, I found a solution: USB-A to USB-C with a USB-C-to-A adapter. Boom! “Yes. I don’t have *that* cable, but I have something.”

- Apple released [the iPhone Duo](https://www.apple.com/de/iphone-duo/) . It looks very nice and the animations everyone fawns over are animations everyone should fawn over and I really want to hold it and I bet opening and closing it feels as good as I imagine it to feel, BUT I’m sharing this not because this has become a Prosumer Gadget Review newsletter (although, listen, Anker, if you’re willing to sponsor: call me). I’m sharing it because: what a company Apple is, huh? Like, I’m impressed by the iPhone Duo, yes, but I’m more impressed by the company that can produce an iPhone Duo. The software, the hardware, the design (as if that’s a separate thing!), the launch videos, the product page, the demos — it’s all *on point*. Not a single slip, not a single note out of tune. Go to that landing page. Click through the carousel. There are images of that phone and there, on page 3 or 4, there are three images of that phone: one shows the phone in Clock mode, the other shows Mail, and the third one shows a workout video or stream — on all three, it’s the same time, 9:41am. All the emails you can see in the screenshot were sent before or at 9:41am. Two of the email previews have a “good morning!” in them. I mean, fucking hell man. That’s some details being paid some attention to. And *that type of stuff is everywhere!* The consistency, the meticulousness, the on-brandness in everything. It’s fucking crazy to me that a company of this size can pull it off.

- Andy Matuschak on [having finished a “four-year program studying the ‘Great Books of the Western World’“](https://x.com/andy_matuschak/status/2096701001898348674?s=46) .

- Brian Lovin is collecting “ [good websites](https://brianlovin.com/sites) ”: great, personal websites. There’s some great stuff in there that really makes me want to change my personal website again.

- Benedikt Seidel, who impressed me immensely by [going out into the world and cold-visiting companies](https://bensdl.substack.com/p/building-a-startup-in-europe-what) and asking them about AI, is now hiring for physicalfusion. He’s looking for a [Founding Member of Technical Staff](https://physicalfusion.ai/careers/roles) . So if you’re in or around Munich and into ML and 3D, talk to Benedikt!

- Glorious: [Kevin Nealon on the Rick Glassman podcast](https://www.youtube.com/watch?v=7MjfB14LGts) . Two bullshitters of the highest level being comfortable with each other and seeing who can go even more meta than the other guy.

[![](https://substackcdn.com/image/fetch/$s_!-F2g!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F09efea20-5510-4d56-b06e-1c093336f8da_945x27.tif)](https://substackcdn.com/image/fetch/$s_!-F2g!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F09efea20-5510-4d56-b06e-1c093336f8da_945x27.tif)

No posts
