---
title: Learning Programming in an Age of LLMs
link: https://blog.ploeh.dk/2026/09/16/on-learning-programming-in-an-age-of-llms/
source: hnrss-org-frontpage
published: 2026-09-16T09:12:34Z
updated: 2026-09-16T09:12:34Z
first_seen: 2026-09-16T20:24:49.200228376Z
authors:
- moneroloop2018
summary: 'Article URL: https://blog.ploeh.dk/2026/09/16/on-learning-programming-in-an-age-of-llms/ Comments URL: https://news.ycombinator.com/item?id=49723873 Points: 206 # Comments: 156'
content: extracted
html: 2026-09-16-learning-programming-in-an-age-of-llms.html
preview:
  file: 2026-09-16-learning-programming-in-an-age-of-llms.preview-ab417ed8d6fb.webp
  width: 196
  height: 196
  color: '#446f94'
images:
- source: https://blog.ploeh.dk/assets/themes/ploeh/images/favicons/favicon.png
  original:
    file: 2026-09-16-learning-programming-in-an-age-of-llms.image-7ffc9eab23f6.png
    width: 196
    height: 196
  color: '#418ac9'
---

*Open answers to a reader's letter.*

A reader recently wrote me a long letter with lots of questions about learning programming in this age of LLMs. After a bit of back-and-forth, I got permission to quote extensively from the letter in order to attempt some answers in public.

None of my answers I consider particularly rigorous; the situation is so uncertain that I can only answer to the best of my abilities, but I don't claim them to hold any kind of immutable truth.

> "I'm trying to understand how people who deeply understand software think about learning and competence in the age of AI. I'm approaching it almost as a historian would: asking people directly how they make sense of a technological transition while actually living through it.
>
> "About a year ago I became fascinated by AI-assisted programming. Despite having no formal CS background, with LLMs I managed to build a fairly large TypeScript/JavaScript system involving APIs, PostgreSQL, LLM pipelines, research automation and multi-model workflows. At first it felt almost magical: AI seemed to collapse the distance between having an idea and being able to build it.
>
> "But now I'm trying to turn that system into a real production product, and I'm struggling. I fix one error with AI, then another appears, then another part behaves in a way I don't fully understand. After months of refactoring I had an uncomfortable realization: I may have built a system that is above my own level of understanding. When everything works, that gap is almost invisible. When it doesn't, it becomes very real.
>
> "Sometimes I genuinely don't know what to do next without asking another model. That made me wonder whether I spent a year building a product, or partly building the appearance of one: something sophisticated enough to work, but which I don't yet understand deeply enough to truly own.
>
> "I'm not anti-AI at all. I'm fascinated by these systems and want to work with them professionally. But I'm unsure what the right relationship with them should be."

Indeed, I'm not sure either, but before proceeding, I find it most transparent to reveal my position. I haven't yet decided on AI, but [I lean toward disliking it](https://blog.ploeh.dk/2026/05/04/motivated-reasoning), knowing full well that it may be unstoppable.

I do work and experiment with it, and it often impresses me. At other times, it frustrates me. It's usually when it impresses me the most that I resent it maximally.

When it's bad, it can be frustrating, but then at least I can absorb an ember of warmth in the illusion that what I've spent more than thirty years learning is still relevant. When it's at its best, I sometimes think: *Where do I sign up for [the Butlerian jihad](https://dune.fandom.com/wiki/Butlerian_Jihad)?*

My position on LLMs is only partly based on my own socio-economic status. I'm old enough, and have had enough success already, that all other things being equal, I can survive unemployment. I'm not sure, on the other hand, than any knowledge-based society can.

It may be that LLMs will take programmer jobs before they take other white-collar jobs. After all, programming *may* be a discipline where verification is easier than, say, insurance claims management. Still, if we reach a point of mass unemployment among knowledge workers, I'm not sure society as we know it will survive.

I usually don't talk much about my background as an economist, but in this context I find it relevant to mention. As an economist, I can't imagine that mass unemployment of 30-40% will not have a significant impact on the economy.

I'm painfully aware of the arguments that this has happened before: There may be job loss, but the advance of technology leads to new jobs we can't even imagine today. It was like that with the introduction of the [stocking frame](https://en.wikipedia.org/wiki/Stocking_frame), the steam engine, the internal combustion engine, computers, etc. This is only partly true: Yes, new jobs were created, but often not for those people who lost their jobs. Coal miners didn't just become programmers overnight.

The same kind of argument was used when [China was admitted to the World Trade Organization](https://en.wikipedia.org/wiki/China_shock). And indeed, lots of new jobs were created, just not in the Western world.

So, based on lived and historical experience, I'm sceptical of arguments that all will be fine.

But I sincerely hope that I'm wrong. I love to program, and wouldn't mind doing it for another ten years. Perhaps more importantly, I have young adult children. I hope that there's a world for them, too.

> "So I'd really like to know how you think about this. Are you glad you learned programming fundamentals before LLMs existed? If you were starting today, would you still seriously study languages, data structures, databases, networking, operating systems, debugging and architecture? Do you think AI can let people become capable of building much faster than they become capable of understanding?"

Am I glad that I learned programming before LLMs? Yes, of course. Those skills served me well for thirty years.

If I was starting today, I'd seriously consider learning carpentry, metalworking, gun-smithing, or something else that requires hand-eye coordination. I know that advances are made in robotics, too, but replacement of manual labour seems to lie farther in the future.

But to address the question: I am, personally, currently learning data structures, language semantics, etc. as part of a university programme. I do that because I'm curious, however, and not because I expect to get much monetary reward out of it.

Do I think that AI enables people to develop faster than they can keep up? This remains to be seen. Software developers have already, for decades, been working on top of abstractions they didn't understand. If you were a web developer, you didn't know much about compiler programming. If you were a compiler programmer, you didn't know much about integrated circuit design. And if your job was to engineer integrated circuits, you wouldn't know much about the levels of abstraction *above* you.

A good rule of thumb was: Understand the level of abstractions directly below the one you work in, as well as the one above. That would enable you to troubleshoot *most* problems.

> "And how do you personally deal with that? When AI can solve something immediately, how do you decide when to use it and when to work through the problem yourself? If you were in my position, with a substantial AI-built project but weak foundations underneath it, would you step back and systematically learn those foundations, keep building and learn as problems appear, or combine the two?"

That's two radically different questions, because I no longer have a weak foundation in software development. Even if I were dealing with something far from what I usually do, I can ramp up leveraging what I already know. Let's imagine that someone tasked me with maintaining an application written exclusively in [RISC-V](https://riscv.org/) assembly code. That's the most alien software environment I can imagine for myself. Adapting to such a development environment would be difficult for me, but still not as difficult as it would be for someone new to programming in general. Believe it or not, I *have* written small exercise programs in RISC-V, as well as an exercise compiler that compiled to RISC-V.

But what if I had virtually no software background?

Well, once upon a time, I was in exactly that situation. When I started my career, for years I balanced a knife's edge of getting things done while learning on the job. Beginning in 1999, I wrote COM components in [C++](https://en.wikipedia.org/wiki/C%2B%2B), not understanding much of what I was doing. Somehow, I still made it work, even to a degree that I managed to eliminate any obvious memory leaks.

I was, however, never happy just slapping things together without understanding how they worked. So I did, as suggested by the question, step back to systematically learn [fundamentals](https://blog.ploeh.dk/2024/05/20/fundamentals). This worked well for a career launched in the mid 1990s. Will it work well today?

I'm not so sure: Reaching a level of competency high enough to recognize your past confidence as clearly lying on the too-ignorant-to-realize-it portion of the [Dunning-Kruger](https://en.wikipedia.org/wiki/Dunning%E2%80%93Kruger_effect) curve took decades. Do you have that much time today?

Granted, with LLMs, you can learn faster, because you can ask more directed questions. Thirty years ago, I would buy books in the hope that they would contain *some* helpful material. This still meant slogging through a lot of learning material not immediately relevant to the task at hand.

Still, I doubt that it's possible to significantly speed up human learning. The bottleneck is hardly the teachers nor the materials, but how fast a human brain can absorb new knowledge.

> "One last thing I would be especially grateful to hear about is how you learned programming yourself, and how you learn new technical things today. How did you approach learning a new language earlier in your career? Books, projects, reading other people's code, exercises, debugging, something else? And if you had to learn a completely new programming language today, with AI available, how would you do it?"

The short answer to the first question: Slowly, based on much trial and error, occasionally backed by a book.

Apart from a very early false start with [COMAL 80](https://en.wikipedia.org/wiki/COMAL), my first programming projects was to (re)calculate [bifurcation diagrams](https://en.wikipedia.org/wiki/Bifurcation_diagram) and the [Lorenz attractor](https://en.wikipedia.org/wiki/Lorenz_system) for my master's thesis in economics. Reaching for what I had, I wrote them in [QBasic](https://en.wikipedia.org/wiki/QBasic), learning from the samples that shipped with it, as well as occasionally asking a friend.

While I'm glossing over many details, in the 1990s and 2000s, I mostly learned from examples and documentation. While I did buy a book about C++, I don't think I ever finished it, and I picked up various Basic dialects as well as C# exclusively from documentation and example code.

That said, although I never read a book to learn C#, books were instrumental in teaching me both [F#](https://fsharp.org/) and [Haskell](https://www.haskell.org/). I have, over the years, relied heavily on books to educate myself, but as [my Goodreads profile](https://www.goodreads.com/author/show/4383188.Mark_Seemann) reveals, I love books in general.

How do I learn a completely new programming language today? Again, my experience is useless to someone new to programming in 2026: I've now seen so many programming languages that if I run into a new one, I can usually pick it up from perusing existing code and looking up the few things that aren't immediately clear.

But that's presupposing that the language in question is 'normal'. If I had to get back into [APL](https://en.wikipedia.org/wiki/APL_\(programming_language\)), I'd at least have to find a tutorial.

You may have noticed that I don't much use LLMs for learning. [LLMs don't hallucinate; they bullshit](https://blog.ploeh.dk/2026/04/06/the-hailo-effect), and I'm deeply distrustful of anything they tell me. This is not to say that I don't use LLMs, but I tend to ask them questions that yield verifiable answers. *Can I make this Haskell expression more succinct?* Any useful answer to such a question is a code suggestion that either works, or doesn't work; is shorter, or isn't. That's easy to verify.

*What should I learn next?* does, on the other hand, not yield a verifiable answer. I tend to not to ask such questions of LLMs.

In conclusion, you could say that I prefer asking LLMs [falsifiable](https://en.wikipedia.org/wiki/Falsifiability) questions.
