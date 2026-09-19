---
title: 'Joy & Curiosity #82'
link: https://registerspill.thorstenball.com/p/joy-and-curiosity-82
source: registerspill-thorstenball-com
published: 2026-04-19T05:23:09Z
updated: 2026-04-19T05:23:09Z
first_seen: 2026-09-19T21:30:23.395188495Z
authors:
- Thorsten Ball
summary: Interesting & joyful things from the previous week
content: extracted
html: 2026-04-19-joy-curiosity-82.html
preview:
  file: 2026-04-19-joy-curiosity-82.preview-6c6ac77a9a8e.webp
  width: 256
  height: 182
  color: '#f3f2f1'
images:
- source: https://substack-post-media.s3.amazonaws.com/public/images/1e0de128-39ef-469b-aac3-1cfd91b46a60_1982x1412.png
  original:
    file: 2026-04-19-joy-curiosity-82.image-7d0630345d00.png
    width: 1982
    height: 1412
  color: '#f4f4f4'
- source: https://substackcdn.com/image/fetch/$s_!nXIP!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fbd264ddb-cf7d-48e6-ba62-50643a85feef.tif
  original:
    file: 2026-04-19-joy-curiosity-82.image-5e7a5d2fef24.png
    width: 944
    height: 37
  variants:
  - file: 2026-04-19-joy-curiosity-82.image-3ef3382f3078.webp
    width: 320
    height: 13
  - file: 2026-04-19-joy-curiosity-82.image-76e565496d56.webp
    width: 640
    height: 25
  - file: 2026-04-19-joy-curiosity-82.image-b3c60dc11c56.webp
    width: 944
    height: 37
  color: '#000000'
- source: https://substackcdn.com/image/fetch/$s_!uTg2!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3d5c05ef-f280-49df-8c2e-ac2c88bac210.tif
  original:
    file: 2026-04-19-joy-curiosity-82.image-c521ab8f2da7.png
    width: 966
    height: 43
  variants:
  - file: 2026-04-19-joy-curiosity-82.image-9f8e540dcfce.webp
    width: 320
    height: 14
  - file: 2026-04-19-joy-curiosity-82.image-56d86d0cb8c9.webp
    width: 640
    height: 28
  - file: 2026-04-19-joy-curiosity-82.image-0a56f46badb6.webp
    width: 960
    height: 43
  - file: 2026-04-19-joy-curiosity-82.image-248dd78b96c6.webp
    width: 966
    height: 43
  color: '#000000'
---

This one’s short, because it’s been a week full of programming and building, less reading. And this weekend’s equally busy, so here’s a question I’ve been flipping around in my head for months now:

What are we learning about working with these models that will be valuable in the future?

In his Lex Fridman interview, [ThePrimeagen said something](https://lexfridman.com/theprimeagen-transcript/#chapter32_programming_with_ai) that stuck with me: “Is anyone actually falling behind for not using AI then? Because if the interface is going to change so greatly that all of your habits need to fundamentally change \[…\], have I actually fallen behind at all? Or will the next gen actually just be so different from the current one that it’s like, yeah, you’re over there actually doing punch card AI right now. I’m going to come in at compiler time AI, so different that it’s like what’s a punch card?”

There’s something to this. The frontier models are now much more forgiving when it comes to prompts. We no longer have to write “you are a senior engineer” in our prompts. “Don’t make mistakes” is more a prayer than a helpful trick. The days of the Prompt Engineer won’t be visible on the timeline if we zoom out to even five years.

Nowadays, I’m even convinced that a lot of what we considered important for manual context management is now no longer needed. (Yes, we’re shipping soon.) We’re close to the point where you no longer have to care whether you’re at 30% or 70% of the context window.

And I’m also convinced that the models will get even better.

Now, maybe it is a form of sunk cost fallacy, a bias talking, but still: I do think that I got better at working with these models over the past two years. It might not be relevant anymore whether I write down my task before or after I include a file in a prompt, but I think I’ve gained some meta-abilities that made me better at solving problems through the use of agents: chopping up problems into engineering tasks and sequencing them, figuring out what the pitfalls (that wouldn’t be pitfalls for humans) are, knowing what’s poison in the codebase and what isn’t. Stuff like that.

In the most general sense, I think I’ve learned how to work with artificial intelligence. And if prompt engineering tricks are punch cards, then that might be seen as learning about computation.

[![](https://substackcdn.com/image/fetch/$s_!nXIP!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fbd264ddb-cf7d-48e6-ba62-50643a85feef.tif)](https://substackcdn.com/image/fetch/$s_!nXIP!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fbd264ddb-cf7d-48e6-ba62-50643a85feef.tif)

- This is, at least for me, already [a Hall of Fame comment](https://news.ycombinator.com/item?id=47748064) : “For reasons which it would take a while to unpack, if is often the case that the best (or sometimes only) way to find out what programming actually needs to be done, is to program something that’s not it, and then replace it. This may need to be done multiple times. Programming is only occasionally the final product, it is much more often the means of working through what it is that is actually needed. This is very difficult for the people who ask for the software, to understand, and it is quite often very difficult for the people doing the programming to understand. Most of what is being done, during programming, is working through the problem space in a way which will make it more obvious what your mistakes are, in your understanding of the problem and what a solution would look like. Once you have arrived at that understanding, then there are a variety of ways to make what you need, but that is not the rate-limiting step.” So, so, so good. This is what software development is: learning.

- [Fractal Paris](https://panoramic.city/fractalparis/) and [Fractal Istanbul](https://www.fractalistanbul.com/) . Lovely!

- Rands: [The Complicators, The Drama Aggregators, and The Avoiders](https://randsinrepose.com/archives/the-complicators-the-drama-aggregators-and-the-avoiders) . Read and recognize people you’ve worked with.

- Brian Cantrill on [the peril of laziness lost](https://bcantrill.dtrace.org/2026/04/12/the-peril-of-laziness-lost/) : “The problem is that LLMs inherently lack the virtue of laziness. Work costs nothing to an LLM. LLMs do not feel a need to optimize for their own (or anyone’s) future time, and will happily dump more and more onto a layercake of garbage. Left unchecked, LLMs will make systems larger, not better.” Read this at the start of the week and then constantly thought of it whenever I asked my agent whether this is “truly the simplest, most minimal, as-little-as-possible and as-much-as-needed solution?”

- Vicki Boykis, in some sense in harmony with Brian Cantrill’s thoughts, on [Mechanical Sympathy](https://vickiboykis.com/2026/04/13/mechanical-sympathy/) : “Mechanical sympathy for both developers and end-users means understanding when asyncio is and is not helpful. It means using the right language, the right build system, the right font. It means using the least amount of tooling possible. Allowing for local development. It means reading code inside out rather than top to bottom. Using uv. Removing code where not necessary. Respecting boundaries.”

- stevey wrote [a tweet about AI adoption at Google](https://x.com/Steve_Yegge/status/2043747998740689171) and [got pushback from Demis Hassabis](https://x.com/demishassabis/status/2043867486320222333) and others and, well, I actually don’t care *that much* about AI adoption at Google, but I find this one thought in there very fascinating: “There has been an industry-wide hiring freeze for 18+ months, during which time nobody has been moving jobs. So there are no clued-in people coming in from the outside to tell Google how far behind they are, how utterly mediocre they have become as an eng org.” I know that people aren’t sure whether there are more or less software jobs right now, but from where I’m sitting it does look like hiring has slowed and I find it fascinating to think about the second-order effects of that: is there less industry-wide diffusion of frontier knowledge because hiring has slowed?

- Shifted something in my brain:[Nucleus Nouns](https://ben-mini.com/2026/nucleus-nouns) . Very good and much more thought-inspiring than the usual “focus! focus! focus!” chants.

- [The Closing of the Frontier](https://tanyaverma.sh/2026/04/10/closing-of-the-frontier.html) : “There is something special about training a model on all of humanity’s data and then locking it up for the benefit of a few well-connected organizations that you have relationships with. Maybe you’ll notice another historical pattern here. Extract value from a population that can’t meaningfully consent, concentrate the returns within a small inner circle, and then offer some version of charity to the people you extracted from as moral cover for the arrangement.”

- [Andy Matuschak has the Practice Guide for Computer printed out](https://x.com/andy_matuschak/status/2043066611905761588?s=46&ct=rw-null) and hanging above his desk.

- Apparently I’m the last person to learn about this idea, but who cares, it’s great and I think I want to try this: [The Spark File](https://medium.com/the-writers-room/the-spark-file-8d6e7df7ae58) .

- Sometimes I read things online and it makes me really happy that we have the Internet and that smart, beautiful minds share their thoughts online. Here’s James Somers with his idea of [the Paper Computer](https://jsomers.net/blog/the-paper-computer) : “Now that we have actually good AI, I have this vision of a form of computing that doesn’t involve me using a computer so much. Imagine you had the day’s emails to go through. It would be nice if the ones that required a simple decision could be dispatched with a few pen-strokes: I could write down a date that would work for that meeting; check a box to accept that invitation; etc. If an email required me to review a draft, I’d love to mark up a print version on my couch, sans screen, and have those notes scanned and sent off as if I’d done the whole thing on Google Docs.”

- Tim Zaman, who worked at NVIDIA, Tesla, X, Google DeepMind and now at OpenAI on AI infrastructure on [Getting Into AI Infra](https://timzaman.com/getting-into-ai-infra) . I’m *convinced* that posts like these create and change entire lifes. I love it. Also: nearly made me want to build a cluster.

- It’s been a while since I’ve thought about people who have *not* yet walked through the one-way door that makes you say “holy shit, AI is going to change everything”, but Armin shared his thoughts after encountering people still being skeptical: [The Center Has a Bias](https://lucumr.pocoo.org/2026/4/11/the-center-has-a-bias/) . Well worth reading.

- Dwarkesh Patel shared what [he learned this week](https://www.dwarkesh.com/p/what-i-learned-april-15) and note how interesting that is and how enjoyable it is to read, even though (or is it because of?) it’s not polished at all.

- Drew Breunig, following the Anthropic Mythos frenzy and some companies closing their open-source projects down for fear of security vulnerabilities being discovered, says [Cybersecurity Looks Like Proof of Work Now](https://www.dbreunig.com/2026/04/14/cybersecurity-is-proof-of-work-now.html) : “If Mythos continues to find exploits so long as you keep throwing money at it, security is reduced to a brutally simple equation: to harden a system you need to spend more tokens discovering exploits than attackers will spend exploiting them.”

- But antirez disagrees: [AI cybersecurity is not proof of work](https://antirez.com/news/163) . Both posts are very interesting and I recommend reading through them.

- I’m in [the process of setting up my 2013 MacBook Pro for my 4-year-old daughter](https://x.com/thorstenball/status/2045412037157675296) and [Peter recommended](https://x.com/PeterJThomson/status/2045488461810209196) this lovely page to let her type on: [tiny-terminal.com](https://tiny-terminal.com/) .

- So, of course, I had to [fork it](https://github.com/meimakes/tiny-terminal) , bought a domain, and let Amp translate it to German so my kids can type words they already know in there: [kleines-terminal.de](https://kleines-terminal.de/) .

- Turing Award winner [Michael Rabin](https://en.wikipedia.org/wiki/Michael_O._Rabin) has passed away. Here’s “an assorted collection of quotations due to Professor Michael Rabin, produced at Harvard University during the Fall 1997 incarnation of the course Computer Science 226r”: [Rabinism Collection](https://web.archive.org/web/20210509160248/http://www.eecs.harvard.edu/~cat/rabinisms.html) .

- [Thoughts and Feelings around Claude Design](https://samhenri.gold/blog/20260418-claude-design/) .

- Another way to think about the question of whether AI will create more jobs or not, [by Aaron Levie](https://x.com/levie/status/2044621545348481285) : “Why will AI create more jobs in plenty of industries? It’s because we’re going to use AI to accelerate output in one area, and then eventually you run into a new bottleneck somewhere else in the process that still requires humans.” This sounds very likely to me. But, of course, “more jobs” doesn’t mean it’ll be the same jobs and then some. Everything’s changing.

- Related, [Gary Bernhardt](https://x.com/garybernhardt/status/2045396869074088291) : “This might be a Mel moment. It’s not immediately obvious that [Mel is a tragic story](http://www.catb.org/jargon/html/story-of-mel.html) . He clearly loved the work. Then the work changed and, presumably, he was left behind. The thing he perfected no longer mattered. There might be millions of Mels right now.”

- Wow, look at just the table of contents here: “I have for years been interested in sleep research due to my professional involvement in memory and learning. [This article attempts to produce a synthesis of what is known about sleep](https://super-memory.com/articles/sleep.htm) with a view to practical applications, esp. in people who need top-quality sleep for their learning or creative achievements.”

[![](https://substackcdn.com/image/fetch/$s_!uTg2!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3d5c05ef-f280-49df-8c2e-ac2c88bac210.tif)](https://substackcdn.com/image/fetch/$s_!uTg2!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3d5c05ef-f280-49df-8c2e-ac2c88bac210.tif)

No posts
