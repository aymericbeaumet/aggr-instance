---
title: The Root of the Root of All Evil
link: https://www.computerenhance.com/p/theroot
source: computerenhance-com
published: 2026-08-26T03:00:49Z
updated: 2026-08-26T03:00:49Z
first_seen: 2026-09-19T21:30:23.395188495Z
authors:
- Casey Muratori
summary: A Brief History of Programming's Most Ambiguous Catchphrase
content: extracted
html: 2026-08-26-the-root-of-the-root-of-all-evil.html
preview:
  file: 2026-08-26-the-root-of-the-root-of-all-evil.preview-48802caa1474.webp
  width: 256
  height: 144
  color: '#867f9e'
images:
- source: https://substack-post-media.s3.amazonaws.com/public/images/9c89aefc-e275-45b1-a760-8be23ead7d7b_1920x1080.webp
  original:
    file: 2026-08-26-the-root-of-the-root-of-all-evil.image-63f875401324.webp
    width: 1920
    height: 1080
  variants:
  - file: 2026-08-26-the-root-of-the-root-of-all-evil.image-e4a2990fd5ca.webp
    width: 320
    height: 180
  - file: 2026-08-26-the-root-of-the-root-of-all-evil.image-de3c8bf0e433.webp
    width: 640
    height: 360
  - file: 2026-08-26-the-root-of-the-root-of-all-evil.image-308087ea3db5.webp
    width: 960
    height: 540
  - file: 2026-08-26-the-root-of-the-root-of-all-evil.image-a2261acb1e7b.webp
    width: 1280
    height: 720
  color: '#7689c3'
- source: https://substackcdn.com/image/fetch/$s_!feZW!,w_720,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4e16d08c-d60a-4774-ad1c-5b587196be06_1174x1666.png
  original:
    file: 2026-08-26-the-root-of-the-root-of-all-evil.image-a9cba4eb067a.jpg
    width: 720
    height: 1022
  color: '#fdfdfd'
- source: https://substackcdn.com/image/fetch/$s_!1hI5!,w_720,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb20adefd-93e4-4a41-a0ee-e2021a966ac6_1182x1678.png
  original:
    file: 2026-08-26-the-root-of-the-root-of-all-evil.image-ae6fbcc32afe.jpg
    width: 720
    height: 1022
  color: '#fefefe'
---

Following in the footsteps of last year’s [The Big OOPs](https://www.youtube.com/watch?v=wo84LFzx5nI) , I once again decided to do a research-intensive talk for this year’s [Better Software Conference](https://bettersoftwareconference.com/) . Last time, my starting point was Ivan Sutherland’s *Sketchpad*. This time, it’s the phrase, “premature optimization is the root of all evil.”

When people bring up this phrase, they typically want to debate its proper application, use it to argue a point, or divine which legendary computer science figure originated it. In this new talk, *The Root of the Root of All Evil*, I do none of those things.

Instead, I try to give as complete a picture as I can of the specific circumstances that lead to the phrase being coined [1](https://www.computerenhance.com/p/theroot#footnote-1) . The more research I did on the historical figures involved, the more I came to believe that you can’t really grasp all the subtleties of the original meaning without knowing each of their stories: you’ll either read too much into the phrase, or too little, and miss the subtle understanding necessary to apply it properly.

So, if you’ve ever wondered what “premature optimization is the root of all evil” *really* means - or wondered why a prominent programmer would have said it - I hope you’ll join me for one more journey back into computer science history to search for *The Root of the Root of All Evil!*

Nearly all of the photographs used in the talk came from [Brian Randell’s fantastic NATO conference history pages](https://homepages.cs.ncl.ac.uk/brian.randell/NATO/index.html) . They are credited to both Randell and his contemporary, Robert McClure. Randell actually appears briefly in the talk proper - he is Dijkstra’s primary interlocutor in the inciting incident for *GOTO Considered Harmful*.

Unfortunately, at the time I am writing these credits, the server hosting Brian’s page seems to be experience SSL certificate issues. It is a wonderful page, with rare historical photographs, so hopefully this will be resolved soon! If it isn’t, you can try accessing a backup version of the page [at the internet archive](https://web.archive.org/web/20251213170245/http://homepages.cs.ncl.ac.uk/brian.randell/NATO/index.html) .

For the remaining photos: The pictures of Donald Knuth’s books were taken by [Héctor García-Molina](https://en.wikipedia.org/wiki/H%C3%A9ctor_Garc%C3%ADa-Molina) , and are available to [download directly from Donald Knuth’s web site](https://cs.stanford.edu/~knuth/graphics.html) . The picture of Donald Knuth himself, as well as the photograph of his long-time collaborator Robert Floyd, appear in many places on the internet, and in books, but never with attribution. If anyone knows the original sources of these two photographs, I would be grateful if you could let me know so I can properly credit the photographer(s) here!

As you might imagine, to prepare a talk like this I read through a large volume of historical material. My research directory for *The Root* has over 200 documents in it! From those sprawling thousands of pages, only a select few excerpts make it onto actual slides.

Each quotation slide bears a footer saying where it came from. If you’d like to read the document behind a slide in more detail, I’ve included a hyperlinked list below to help you find them. Where possible, I have linked to a publicly available version of the material if it appears to have been legitimately posted (ie., not pirated).

In the order in which each appears in the talk, the excerpts are from:

- [ACM Computing Surveys, Volume 6, No. 4](https://archive.org/details/sim_acm-computing-surveys_1974_6_contents/mode/2up)

- [Structured Programming with go to Statements](https://dl.acm.org/doi/10.1145/356635.356640)

- [The Errors of TeX](https://onlinelibrary.wiley.com/doi/abs/10.1002/spe.4380190702) *(paid access only)*

- [EWD196 - The Structure of the “THE”-Multiprogramming System](https://dl.acm.org/doi/10.1145/800001.811672)

- [Edsger Wybe Dijkstra - His Life, Work and Legacy](https://dl.acm.org/doi/book/10.1145/3544585) *(paid access only)*

- [An Interview with Edsger W. Dijkstra](https://conservancy.umn.edu/server/api/core/bitstreams/7a97387c-e045-4647-925c-d6d94a312f6e/content)

- [EWD215 - A Case against the GO TO Statement](https://www.cs.utexas.edu/~EWD/ewd02xx/EWD215.PDF)

- [Go To Statement Considered Harmful](https://dl.acm.org/doi/epdf/10.1145/362929.362947)

- [EWD1308 - What led to “Notes on Structured Programming”](https://www.cs.utexas.edu/~EWD/ewd13xx/EWD1308.PDF)

- [EWD245 - On Useful Structuring](https://www.cs.utexas.edu/~EWD/ewd02xx/EWD245.PDF)

- [EWD340 - The Humble Programmer](https://www.cs.utexas.edu/~EWD/ewd03xx/EWD340.PDF)

- [Datamation - October 1968](https://bitsavers.trailing-edge.com/magazines/Datamation/196810.pdf) *(original scan preserved thanks to [Bitsavers](https://www.patreon.com/cw/bitsavers) )*

- [Datamation - December 1968](https://bitsavers.trailing-edge.com/magazines/Datamation/196812.pdf) *(original scan preserved thanks to [Bitsavers](https://www.patreon.com/cw/bitsavers) )*

- [EWD227 - Stepwise Program Construction](https://www.cs.utexas.edu/~EWD/ewd02xx/EWD227.PDF)

- [A Review of “Structured Programming”](http://infolab.stanford.edu/pub/cstr/reports/cs/tr/73/371/CS-TR-73-371.pdf)

- [NATO Software Engineering Conference 1968 Report](https://web.archive.org/web/20251213165653/http://homepages.cs.ncl.ac.uk/brian.randell/NATO/nato1968.PDF) *(at the Internet Archive, since Brian Randell’s page is currently down)*

- [EWD249 - Notes on Structured Programming](https://www.cs.utexas.edu/~EWD/ewd02xx/EWD249.PDF)

- [EWD209 - A Constructive Approach to the Problem of Program Correctness](https://www.cs.utexas.edu/~EWD/ewd02xx/EWD209.PDF)

- [The Emperor’s Old Clothes](https://dl.acm.org/doi/10.1145/358549.358561)

- [Efficient Production of Large Programs](https://www.cs.ox.ac.uk/publications/publication8100-abstract.html)

- [A Contribution to the Development of ALGOL](https://dl.acm.org/doi/10.1145/365696.365702)

- [Record Handling](https://archive.computerhistory.org/resources/text/Knuth_Don_X4100/PDF_index/k-9-pdf/k-9-u2293-Record-Handling-Hoare.pdf)

- [Oral History of Sir Antony Hoare](https://www.computerhistory.org/collections/catalog/102658017/?media=131887)

- [Structured Programming](https://archive.org/details/Structured_Programming__Dahl_Dijkstra_Hoare)

- [Oral History of Donald Knuth](https://archive.computerhistory.org/resources/text/Oral_History/Knuth_Don_1/Knuth_Don.oral_history.2007.102658053_all.pdf)

- [Research in the Computer Science Department (1969)](https://exhibits.stanford.edu/stanford-pubs/catalog/ck700wt5969)

- [Notes on Avoiding “go to” Statements](https://www.sciencedirect.com/science/article/abs/pii/0020019071900184) *(paid access only)*

- [The IBM System/360 Model 91](https://ieeexplore.ieee.org/document/5392015) *(paid access only)*

- [Optimal Measurement Points for Program Frequency Counts](https://link.springer.com/article/10.1007/BF01951942) *(paid access only)*

- [An Empirical Study of FORTRAN Programs](https://onlinelibrary.wiley.com/doi/abs/10.1002/spe.4380010203?msockid=116fcbc826066c661f1bdc7727fe6d05) *(paid access only)*

- [The Execution Time Profile As A Programming Tool](https://archive.org/details/designoptimizati0000vari/mode/2up) *(a few months after I bought mine on eBay, somebody uploaded it to the Internet Archive! But it wasn’t me, I swear.)*

- [An Interview with Charles Antony Richard Hoare](https://commons.lib.jmu.edu/cgi/viewcontent.cgi?article=1206&context=selectedworks)

- [The Debugging of Computer Programs](https://www.proquest.com/openview/a7882c93c36fa41d1e1c2d305abbded8/1?pq-origsite=gscholar&cbl=18750&diss=y) *(paid access only - only appears in the Q&A)*

I realize these days it’s a hard sell to convince people to spend time reading historical documents. Why not just have an AI do it for you?

Personally, I find the literal act of going through the documents to be the most valuable part of the experience. The primary reason for this is that an overall sense of the material is what matters more than any particular fact, and it’s not possible to get that without spending a considerable amount of time immersed in the historical record.

But a secondary reason is that it’s impossible to know beforehand all the facts you might want to look for. Every time I go spelunking in computer science history, I always happen upon dozens of fascinating artifacts that I was never specifically looking for. It might be Doug Ross’s invention (and use) of fat structs in the 1950s, Bjarne Stroustrup doing “Dependency Injection” in 1979 (more on that later), or, this latest time, finding Sir Charles Antony Richard Hoare proposing what appears to be static single-assignment form… in 1965!

[SSA](https://en.wikipedia.org/wiki/Static_single-assignment_form) - a mainstay of compiler construction still used in today’s most popular backends - was supposedly developed in the 1980s at IBM. Though I’ve never done any research into that claim (or its history), while doing research for *The Root*, I stumbled upon the following passage in [an unpublished IFIP proposal of Hoare’s](https://www.cs.ox.ac.uk/files/6069/H65%20-%20Programming.pdf) :

![](https://substackcdn.com/image/fetch/$s_!feZW!,w_720,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4e16d08c-d60a-4774-ad1c-5b587196be06_1174x1666.png)![](https://substackcdn.com/image/fetch/$s_!1hI5!,w_720,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb20adefd-93e4-4a41-a0ee-e2021a966ac6_1182x1678.png)

The coversheet and most relevant page from Hoare's IFIP 1965 proposal

> Firstly, the program is transformed into an identically equivalent program from which all inner blocks have been removed, and in which no identifier occurs more than once on the left hand side of an assignment statement. The satisfaction of these conditions is made possible by the absence of go to statements, conditional statements, and for statements from the language of the translation routines. The next stage is to rearrange \[the\] sequence of the statements of the program in such a way that every variable occurs on the left hand side of an assignment before its first occurrence on the right hand side. This can be done by the same topological sorting algorithm that is widely used in PERT programs.

Hoare proposes the SSA-like transformation for the exact same reason as it was eventually developed (and for which it is still used to this day): as an internal representation structure for multi-pass compilers.

So why did the world have to wait another 15 years for compiler authors to develop SSA proper? According to the handwritten note added to the proposal’s coversheet by Hoare in 1998:

> As a result of negative comments of Naur at the congress, I never wrote this up for publication. He said writing a nine-pass compiler was easy.

The “Naur”, in this case, is [Peter Naur](https://en.wikipedia.org/wiki/Peter_Naur) , whose name you may recognize from the term “ [Backus-Naur Form](https://en.wikipedia.org/wiki/Backus%E2%80%93Naur_form) ”, which is used ubiquitously to refer to a notation style for defining programming language syntax.

I hope you enjoy *The Root of the Root of All Evil*. As with *The Big OOPs*, I once again learned a tremendous amount about the history of computing while preparing it. But, also like *The Big OOPs*, I came away with the inescapable feeling that I’d only just scratched the surface.

I’m not sure if I’ll do another one of these talks in the future. They’re extremely stressful to prepare, because the volume of information you’re trying to streamline is immense - far larger than any other kind of talk I’ve ever given. There is always more research you could do, and nagging questions whose answers might exist in some obscure document you just haven’t yet found. I am typically making slides right up until the moment I give the talk, and I never get the chance to do a proper rehearsal beforehand.

But I doubt I’ll ever stop nosing around in computer science history. There’s so much great stuff, I’m sure I’ll never exhaust it. As an industry, sadly, it feels like we’ve forgotten far more than we’ve retained.

On that note, since this is the one-year anniversary of *The Big OOPs*, I’ve also prepared a special hour-long, members-only video where I do my best to show what it’s like to read through the historical materials used to prepare these two unusual talks. It attempts to condense an over-500-page slice of *Big OOPs* research into one hour, which of course requires my best impression of the latest TikTok “fast talking” style. For all you OOP fans out there, it also includes the aforementioned finding that Stroustrup proposed dependency injection in the late seventies.

I will be posting that video here as a follow-up, so if you’d like to be notified when it’s live, please check out our subscription options:

Until next time, have fun programming everyone, and I’ll see you on the Internet.

— Casey

[1](https://www.computerenhance.com/p/theroot#footnote-anchor-1)

Regardless of who coined it, since some - including at times its most likely originator, Donald Knuth - have attributed it to others.
