---
title: Writing as a thinking tool
link: http://muratbuffalo.blogspot.com/2026/11/writing-as-thinking-tool.html
source: hnrss-org
published: 2026-11-08T10:41:52Z
updated: 2026-11-08T10:41:52Z
first_seen: 2026-11-08T18:46:19.638047103Z
authors:
- jruohonen
labels:
- my advice
- research-advice
- writing
content: extracted
html: 2026-11-08-writing-as-a-thinking-tool.html
extra:
  comments_url: https://news.ycombinator.com/item?id=50412067
  num_comments: 42
  points: 144
---

In middle school I could not get through long passages no matter how hard I tried. I would read an entire page and find that nothing had registered, so I started taking notes out of desperation. This was the late 80s, and Tony Buzan's mind maps had just reached Turkey. This led me to produce pages and pages of notes that looked like spiderwebs. Creating those notes have helped me tremendously. I made sense of what I was reading by figuring out which branch they would hang off in my mind maps. I realized my pages were not for storing the information as much as they were for processing it.

It took me another ten years to understand why this works so well. During a stretch of PhD procrastination, I read [Manuel Blum's advice to graduate students](https://www.cs.cmu.edu/~mblum/research/pdf/grad.html):

> You are all computer scientists.
>
> You know what FINITE AUTOMATA can do.
>
> You know what TURING MACHINES can do.
>
> For example, Finite Automata can add but not multiply.
>
> Turing Machines can compute any computable function.
>
> Turing machines are **incredibly more powerful** than Finite Automata.
>
> Yet the only difference between a FA and a TM is that
>
> the TM, unlike the FA, has paper and pencil.
>
> Think about it.
>
> It tells you something about the power of writing.
>
> Without writing, you are reduced to a finite automaton.
>
> With writing you have the extraordinary power of a Turing machine.

This is a crisp technical claim about which problems you can solve with writing. If you take the paper away, you downgrade yourself by an entire complexity class!

I think this explains a lot about human history as well. Language let us externalize some of our thinking, and our species got smarter. Writing let us externalize even more of it, and we got smarter again. I suspect this is also why LLMs are as capable as they are: so much of our intelligence is already deposited in language that a model trained on it inherits a good deal of intelligence for free.

## Out-of-core execution

When we write, we are externalizing our thinking onto paper, and that is what raises our capability to think bigger thoughts. I liken this to [out-of-core execution, or external memory algorithms](https://en.wikipedia.org/wiki/External_memory_algorithm).

A good example of this for me is [reviewing a paper](https://muratbuffalo.blogspot.com/2013/07/how-i-read-research-paper.html). When I read a paper, I think I understand it, and then I sit down to write the review and find out I understood maybe 60% of it. While writing the review, I end up contextualizing the paper better, and get to understand it fully. And sometimes I even realize the ideas are bigger than the authors noticed, and they connect to many other things.

Sometimes the unearthing goes further than understanding one paper. In 2018, [I wrote a quick post about the anatomical similarities between Paxos and Nakamoto consensus](https://muratbuffalo.blogspot.com/2018/03/anatomical-similarities-and-differences.html). I had not started with that thesis originally. I started writing, and the connections got clearer as I went, and that post became the seed for one of our research papers. This is not uncommon, [this is called freewriting](https://muratbuffalo.blogspot.com/2018/05/book-review-accidental-genius-using.html). This is what happens when you give the machine a tape.

Writing is also the reason I remember anything. I refer back to my own blog constantly. Because I strained my brain writing those posts, rereading them re-fires the same neurons, and I reconstruct the understanding quickly. In this sense, the blog became my external memory. The thoughts I never wrote down do not stick well, because they were never fully thought.

> If you think without writing, you only think you're thinking.
>
> -- Leslie Lamport

## Writing in the age of LLMs

Is writing for thinking getting obsolete now since you can now argue anything with a model that has read everything?

Well, the model has read everything except what is in your brain! It knows the literature, the standard framing, and the consensus. But it does not know your half-formed intuition about why the standard framing may be wrong for this particular case. AI made a lot of progress, but I don't trust an LLM to take the creative leap and make a new connection. I think that would require another AI breakthrough. So, if you open the chat immediately on anything, you lose the opportunity to write/think for yourself.

This is a real problem, and [it seems to be spreading quickly.](https://alifeengineered.substack.com/p/on-the-death-of-amazons-writing-culture) We have been celebrating Amazon's six-pager as a great example of writing-for-thinking. Bezos had said: "There is no way to write a six-page, narratively structured memo and not have clear thinking." Unfortunately, with LLMs, that sentence has stopped being true. You can now produce six pages with no thinking at all, and the six pages still look like evidence that the thinking happened.

A model optimizing next-token probability cannot do your thinking. If you trust it to hold the pen, no thinking would get done. As everyone turns their thinking down and becomes a meat-proxy for generated text, if you still keep writing yourself, you would still be thinking for yourself. So [writing is now a comparative advantage](https://muratbuffalo.blogspot.com/2026/08/the-safest-job-from-ai-may-be-writing.html), and a widening one!

Anyways, enough about why you should write. Here is my advice on how you should write.

## Rule 1: Mess up

The first rule of writing club is to produce in bulk with no restraints at all. Don't criticize, don't fix the typos, don't write in full sentences if you don't feel like it. The goal in this phase is to just accumulate text and get to a zeroth draft you can work with.

[McPhee argues best](https://muratbuffalo.blogspot.com/2019/03/book-review-draft-no-4-by-john-mcphee.html) for why getting anything down matters so much. Even if you had a bad writing session, it gave your mind something to work with. After you get something down, you can spot problems with it and think of better ways of phrasing things. But none of that happens without the drafted version to react against.

This is also how research papers get written. Open a notes.org file and treat it as a lab notebook: write while you do the work, explore every tangent, produce without any pressure to be presentable. [In the Köroğlu post](https://muratbuffalo.blogspot.com/2014/03/how-to-write-your-research-paper.html) I compared this to the foal fattened in a pitch-black stable before any training begins. The key part of that frame is the stable. During this phase you guard the idea from critics, including and especially yourself. Ideas start out fragile and easily squished. So suppress judgment for a while and see what the thing becomes.

## Rule 2: Tidy up

Now you must become the critic, the editor. Forget that you wrote this. Imagine some other guy wrote it, and do a harsh review. Cut ruthlessly. Take delight in deleting text the way you would in deleting code. If you cannot bear to delete a good sentence, move it to a junk buffer at the bottom of the file so you can tell yourself you might bring it back. Spoiler: you won't.

Don't be discouraged by how bad the first draft was. [McPhee says](https://muratbuffalo.blogspot.com/2019/03/book-review-draft-no-4-by-john-mcphee.html) the way to do a piece of writing is three or four times over, never once. This is good news. Thinking in multiple drafts takes the pressure off: you don't have to be spontaneous, brilliant on the first pass, or finished today.

I think the reason writing works so well for thinking is that it slows you down while keeping you on the problem. In that extra time, the insights and connections get room to surface. [Chunking](https://en.wikipedia.org/wiki/Chunking_\(psychology\))/[abstracting](https://muratbuffalo.blogspot.com/2023/09/beyond-code-tla-and-art-of-abstraction.html) happens in the background. Editing is then the magic trick where you pretend you had an organized thought all along. It is the packaging of the product.

## Rule 3: Keep it simple, plain, and succinct

I am an Elements of Style guy. [Strunk and White got it right](https://muratbuffalo.blogspot.com/2010/09/writing-advice.html), and their writing still reads fresh decades later. I adore Vonnegut too. His writing is simple as fuck and completely eloquent. It took a lot of balls to write that plainly. Complexity in prose is usually a place to hide sloppy thinking, or a way to weasel out of taking a stance. If you can't say it plainly, you probably haven't thought it through yet. And the way you think it through is by writing/editing.

## Rule 4: Write without expectations

[Write for yourself.](https://muratbuffalo.blogspot.com/2024/03/why-i-blog.html) The audience I have in mind is me. If others read it, that is a bonus, but the motivation has to be intrinsic, because the external rewards are far too delayed and too random to steer on. Writing like no one is watching also frees you up, and you can decide afterwards how much to share. Get your dopamine hit from the writing itself, not from people reading or sharing what you wrote, as that is very unreliable.

I have been writing and sharing on this blog since 2011, and around 2016 I started running into people at conferences who read it. I assumed it was a fluke, but it kept happening. Writing with a human voice turns out to get a lot of attention, and I am still surprised by how much. Paradoxically, you seem to get an audience only if you weren't doing it for the audience.

## Rule 5: Put it on the schedule

You should stop trying to get inspiration to write and [start scheduling it.](https://muratbuffalo.blogspot.com/2018/04/book-review-how-to-write-lot-practical.html) Waiting for inspiration doesn't work, instead, putting your butt on seat and writing breeds good ideas for writing.

I find that keeping a posting schedule for the blog keeps me in "a writing frame of mind", which is really "a noticing frame of mind". Many weeks I tell myself a topic isn't interesting, but I start writing about it anyway, and then, what do you know, I am startled by the fount of interesting things that surfaces.

I also feel relieved after writing something as the thought is out of my system, I have closure on it, and I have room to think new thoughts. If a week goes by without a post, I feel clogged. So put writing on the calendar and keep it there until it no longer needs to be on the calendar.
