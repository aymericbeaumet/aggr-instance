---
title: Why I wrote the BEAM book (2025)
link: https://happihacking.com/blog/posts/2025/why_I_wrote_theBEAMBook/
source: hnrss-org
published: 2026-11-09T15:41:35Z
updated: 2026-11-09T15:41:35Z
first_seen: 2026-11-11T15:04:08.373515189Z
authors:
- zenincognito
content: extracted
html: 2026-11-09-why-i-wrote-the-beam-book-2025.html
remote_preview:
  url: https://happihacking.com/images/og/thebeambooks.jpg
---

Post-mortems, coffee, and a decade of stubborn curiosity

## Why I wrote the Beam Book

After ten years of keeping Klarna’s core system upright I know this: a 15 millisecond pause in the BEAM can stall millions of peak-shopping payments, trigger a 3 a.m. Christmas-Eve post-mortem, and earn you a very awake call from the CEO. I wrote *The BEAM Book* so the next engineer fixes that pause before the coffee cools.

![A picture of two printed BEAM Books.](https://happihacking.com/images/thebeambooks.jpg "A picture of two printed BEAM Books.")

### Origins

I opened the project on 12 October 2012 with a lone DocBook file with four lines of text and an oversized sense of optimism. After two weeks, the commit log is mostly me adding structure, moving headings, and updating metadata. Most of it is scaffolding. The actual content is still just a few hopeful lines.

By November I had abandoned DocBook for AsciiDoc, written a custom build script, and convinced myself the book could be wrapped up in six months. Those early commits glow with energy: adds, rewrites, then more rewrites to fix the rewrites. Delusion is underrated.

In 2013 I managed to convince O’Reilly to publish. Moving the repo to their Atlas system sounded simple until Atlas began hiding my main file and overwriting half-finished chapters.

The Git history reads like a diary of frustration: “Moving files to top level to cope with Atlas,” “Atlas seems to be overwriting book.asciidoc”. Word count shot past 120 000 while actual progress crawled. On 10 March 2015 I was literally “Smashing chapters into sections” just to keep the build green.

The quiet cancellation came two months later. No drama, just a polite call and a line through the contract. Relief mingled with embarrassment, I had spent two years rearranging files rather than finishing sentences.

Pragmatic Bookshelf took over that same year. I kept working in CVS for their production system, but progress was slow. Eventually, they cancelled too. On 20 January 2017, I imported everything into a new repo in one massive commit: 6,622 files, over a million lines. The rewrite stalled, and so did the project.

On 23 March 2017 I started fresh with Asciidoctor in a private GitHub repo, copy-pasting only the parts that still made sense. Two weeks later, on April 7, minutes before a lecture at Chalmers, I flipped the repository public. Within twenty-four hours strangers fixed typos, added diagrams, and merged a Creative Commons BY-4.0 license.

### What Kept Me Going

![A picture of the stars on GitHub passing 3000.](https://api.star-history.com/svg?repos=happi/theBeamBook&type=Date "A picture of the stars on GitHub passing 3000.")

I kept going because I wanted to understand the BEAM properly. There’s value in following the real logic, not just the surface explanations.

Community feedback made a difference. As soon as the repo was public, people began sending corrections, examples, and improvements.

Seeing the numbers of people starring the repo on GitHub kept me going. One highlight: **Issue #113 - "Please continue being awesome."** (I wrote [a whole post about that issue](https://happihacking.com/blog/posts/2025/an_issue/).) That emoji-laced drive-by encouragement (August 2018) still pops into my head whenever motivation dips.

![Issue 113: This book
is ridiculously good. I have only read a few bits of it so far and have
learned a lot already. Please continue being awesome!](https://happihacking.com/images/issue113.png "Issue 113: This book
is ridiculously good. I have only read a few bits of it so far and have
learned a lot already. Please continue being awesome!")

The book started showing up as a reference in Erlang and BEAM conference talks, sometimes several times in the same event. That was a clear signal that others needed this as much as I did.

Even Twitter (in the good old days of Twitter) played a role. Whenever someone mentioned the book or shared a link, it was an extra nudge to keep at it.

Mostly, I just wanted a manual I could trust myself, a reference for the parts of the VM that matter when things go wrong. That’s reason enough to keep writing, even after the third rewrite.

### What’s Inside the Book & Who It Helps

The book covers what I wish I’d had when building and operating large Erlang systems:

- Schedulers and process management: How the BEAM schedules, prioritizes, and balances processes under real load.
- Processes and their memory: How process heaps, stack, messages, and binaries are managed and why these details matter in production.
- Garbage collection and memory: What actually happens with per-process and global garbage collectors, binary references, and memory leaks.
- Tagging schemes and terms: How the BEAM represents data-integers, floats, tuples, binaries, references-down to the tagging bits.
- The compiler and the VM: How code is turned into instructions, what the compiler does (and doesn’t do), and how the emulator executes it.
- Tracing and debugging: Practical use of dbg, erlang:trace, and other tools to follow messages, events, and identify bottlenecks.
- Performance tuning: What matters when profiling real code, understanding reductions, and tracking down real-world latency problems.
- System architecture: How ERTS, the BEAM VM, and their subsystems actually work together in a running node.

If you build or operate Erlang or Elixir systems, especially under any kind of scale-this book is for you. It saves you from hunting through mailing lists, scattered docs, and code comments just to answer, “Why is the VM behaving like this?”

### Lessons Learned

Persistence beats perfection. Two cancelled publishing deals look bad on a résumé, but an unfinished idea looks worse.

Boundaries matter. I made progress by blocking time for writing, turning off notifications, and treating focus like a real deadline. Fika at 14:30 is non-negotiable.

The crowd helps. Making the repo public brought in corrections, encouragement, and the occasional nudge when motivation was low.

Scope is everything. I cut the details on dirty schedulers, the new JIT, and the debugger. Maybe those will end up in an appendix, but not in the core. I wrote more about this scoping process in [The BEAM Book Is Almost Done](https://happihacking.com/blog/posts/2025/the_beam_book_lessons/).

Ship, then iterate. The BEAM changes every year. A living Git repo keeps up.

A real deadline helps. This January, during my yearly review, I decided to print the book in time for Code Beam Stockholm. I thought I had until autumn, turns out the conference was June 2. That’s how you find out what’s truly essential.

### Definition of Done

Holding the print in my hands, it finally feels finished, at least for now. Years of scattered commits are bound into something real, so I’m calling it done.

### Get Involved

You can now get the paperback-The BEAM Book 1.0 is live on Amazon. Buy it here. [Amazon](https://www.amazon.com/dp/9153142535)

If you spot an error, want to improve something, or just want to see how it works under the hood, star or fork the repo. File an issue or, even better, submit a pull request. Contributors are credited in the acknowledgments. [GitHub: theBeamBook](https://github.com/happi/theBeamBook)

If you read the book, please leave an honest review. Algorithms notice real feedback more than marketing copy.

If your team wants a deep dive, I run hands-on BEAM internals workshops, tailored for real systems, not just hello world. Email me if that’s what you need. [happi@happihacking.com](mailto:happi@happihacking.com)
