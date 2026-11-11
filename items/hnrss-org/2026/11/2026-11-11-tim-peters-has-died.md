---
title: Tim Peters has died
link: https://discuss.python.org/t/in-memoriam-tim-peters-1952-2026/110028
source: hnrss-org
published: 2026-11-11T07:03:17Z
updated: 2026-11-11T07:03:17Z
first_seen: 2026-11-11T21:51:32.649893059Z
authors:
- ayhanfuat
summary: 'https://www.reddit.com/r/Python/s/2Cm5dBcvQw Comments URL: https://news.ycombinator.com/item?id=50450488 Points: 628 # Comments: 61'
content: extracted
html: 2026-11-11-tim-peters-has-died.html
remote_preview:
  url: https://us1.discourse-cdn.com/flex002/uploads/python1/original/1X/f93ff97c4f381b5e8add5a0c163b4ded29f20ed7.png
---

November 11, 2026, 5:36am 1

I am immensely sad to announce that [Tim Peters](https://en.wikipedia.org/wiki/Tim_Peters_\(software_engineer\)) has [died](https://www.bratleyfamilyfuneralhomes.com/obituary/timothy-peters) earlier this year on July 15.

It’s hard to overestimate Tim’s influence on Python. He’s probably best known for writing the Zen of Python ([PEP 20](https://peps.python.org/pep-0020/)), but to me he was a mentor and language co-designer long before that.

Tim found Python not long after its initial release. His first deed was to convince me that Python should support mixed arithmetic between `int` and `float`, because most users would expect them to work that way. And he never stopped contributing.

Tim quickly became a major phenomenon on the Python mailing list (including later incarnations and spinoffs), where he earned the nickname “Timbot” by his innumerable, careful and patient answers to user questions. These he filled with mild humor (always marked by `<wink>` or `<0.5 wink>`) and often referred to as “channeling Guido”.

At the same time Tim participated in numerous in-depth design discussions, saving me or others from embarrassing design mistakes or solving deep problems, guided by his vast mathematical knowledge and compiler-building experience. In fact, Tim really set the tone for early core development, never outright dismissing people and explaining in detail why things could or couldn’t work.

In 2002, Tim gave us [Timsort](https://en.wikipedia.org/wiki/Timsort): a new sorting algorithm he designed for Python lists, which was later adopted by Java and Android.

Python-would-not-be-what-it-is-today-without-Tim-ly y’rs,

–Guido

149 Likes

 [guido](https://discuss.python.org/u/guido) (Guido van Rossum) November 11, 2026, 5:41am 2

Feel free to respond with your own Tim anecdotes. I could not cover everything he did.

4 Likes

 [Rosuav](https://discuss.python.org/u/Rosuav) (Chris Angelico) November 11, 2026, 6:04am 3

Very sad news. I have had the privilege and pleasure of discussing MANY topics with Tim, and his input into technical discussions has always been something I value very highly. He never treated you like an idiot, never dismissed your concerns out of hand, always answered and explained. I exchanged emails with him many times, but geography meant that sadly, we never met in person.

12 Likes

 [hugovk](https://discuss.python.org/u/hugovk) (Hugo van Kemenade) November 11, 2026, 6:13am 4

From the start to the end. His [last post](https://discuss.python.org/t/sorted-data-structure-in-the-stdlib/107994/30) concluded:

> Which is all a long-winded say of saying the real world is more complicated than our clean abstractions hope for ![:wink:](https://emoji.discourse-cdn.com/apple/wink.png?v=16 ":wink:").

36 Likes

 [mcdonc](https://discuss.python.org/u/mcdonc) (Chris McDonough) November 11, 2026, 6:41am 5

I wrote this some time ago about Tim:

“”"

I worked with Tim and the rest of Python Labs for a couple of years in the early 2000s. Tim was a hugely positive influence on me throughout my Python career.

Before I had ever spoken to Tim, he was already legendary. He had worked for Cray Research writing compilers for supercomputers, and for Dragon Systems, writing speech-to-text software at a time when such a task was considered largely impossible. He took up Python on a lark in its very early days, becoming something like Python’s second real user after Guido.

Guido came to rely on Tim heavily. Tim will demur about this, but as far as I can tell, Tim and Guido were largely partners in the design evolution of very early Python, and before most important features made it in to Python, you could be sure that they had been examined from every angle, shaken, stirred, and thunk about very hard by both of them.

Tim often played the role of gatekeeper and polisher. You have Tim to thank at least partially if you were a refugee from Java or Microsoft languages, which made excuses for their sprawling verbosity, complexity, and poor readability. Python, at the time, made none of those and had none of that (it now does). Tim was crucial in ensuring it stayed that way during his most active years. He will claim differently, but, by my observation, it is no accident that Python ballooned in complexity after his exit as day-to-day core developer. Perhaps this was more due to the resignation of Guido as BDFL and the new Steering Council model, which was also coincident, but not having Tim around didn’t help either.

It was pure joy to read Tim’s Usenet and mail list posts, which were full of humor and good nature and measured positivity and not a little winking darkness. They somehow resonated with me on my journey from becoming a new programmer to a not-so-new programmer and they lit a path to becoming one on which you could be dramatically competent without becoming cryptic and insufferable. He was intensely knowledgeable, and unfailingly precise when arguing for or against some design change. But he was always gentle and patient and matter-of-fact, preferring to use humor and warmth rather than bile and authority as the currency in which he dealt.

Tim penned “the Zen of Python” (“beautiful is better than ugly…”) which can be seen via “import this” to this day. You can see a bit of his wonderful humor and style in the [prelude to his introduction of the poem](https://mail.python.org/pipermail/python-list/1999-June/001951.html) . His enthusiasm was infectious, and in the days when he was most involved, [most everyone could be witnessed](https://mail.python.org/archives/list/python-dev@python.org/thread/ND2WKGJME4FCFM3IDZMLG5EJW76DWOGQ/#YXWNISHMQYFCD6HAR4GLFZATDESWB2VR) spouting Tim-isms (winks and “'lys” and whatnot).

When I did get the chance to work with Tim, he was just as influential. He had absolutely no pretensions. If I hadn’t already known him to be a computer savant, I would have assumed he was a biker or mechanic due to his long grey locks, full mustache and and working-class fashion sense. I would later find out that he had indeed worked at Harley-Davidson. I think he’s originally from Wisconsin, but an East- or West-Coaster not fluent in Midwest tongues could be excused for believing [his disarming accent](https://www.youtube.com/watch?v=1wAOy88WxmY) is Chicagoan. He seemed just like a regular-looking neighborhood punch-the-clock guy, not unlike all the mechanics and deliverymen and cooks and dishwashers and event staff I’d worked around as a kid and in my 20s. Except he was pretty much the smartest guy in every room he entered.

Tim’s demeanor and lack of pretense inspired me to think that despite my own lack of education and pedigree, with enough effort and grace, that I might become a half-competent programmer. And eventualy I did. But before meeting Tim, it was not a given. I was alienated by the pretensions of my community peers, and I was often reminded that they were not necessary only by Tim.

He was somewhat private. He could often be found with a pen and paper outside the building just standing alone, staring blankly off into space, presumably thinking about some mind-boggling algorithmic topic with which he had just been charged by our CTO, or perhaps mentally designing some aspect of a pet project, scribbling something every few minutes. He applied himself to our code, which included a custom database written in C and Python. He diagnosed and fixed a data-destroying bug in that byzantine codebase for our largest customer. This saved the account, and likely gave us enough runway for another year or so.

Everyone who worked there was in awe of his programming and debugging skills, and while we tried our best to let the poor man be, we often asked him questions, many of which were pedestrian and dull. I remember one of our teammates trying to engage him into a debugging session, claiming that some fault seemed to happen every six runs. He smiled winkingly and asked “have you grepped the source for six?” It was a minor jab but given the sincerity of the question, and the person from which it had come, it occurred to me at the time that it might not be the worst theory of debugging from which to start.

Tim was a Director of the Python Software Foundation from 2001 to 2014, bringing Python across an uncanny valley from a time where it was “interesting but ignorable” to “the planet’s default language” along with a cast of other incredible people. If you use Python, it’s not in small part due to Tim.

When I started my own open source projects, it was Tim whom I channeled during design discussions and sprints. I quite consciously used tricks I had learned from him to make the process fun and welcoming. Rather than become prickly about some technical conflict, I would try to use low-brow humor to defuse things and I would try very hard to learn everything I could about the topic before responding such that I could gain and rely on the respect of other contributors. I often failed, of course, because I’m not Tim, but I was at least a twentieth a Tim, and at the time, I think my co-contributors would have agreed that the projects I led were fun and worthwhile to work on.

It is in this way that Tim’s influence is likely most obvious to people today, because I’m not the only one who channeled him; any Python project started between 1995 and 2010 has a bit of Tim culture in it.

“”"

In sadness,

. C

25 Likes

 [duncathan](https://discuss.python.org/u/duncathan) (Dunc Graham) November 11, 2026, 6:46am 6

As a young child, only about 15, I first learned timsort. Some dozen years later, I had the briefest opportunity to exchange words with Tim himself on this forum. What a treat to have had that chance, just in time. RIP

9 Likes

 [encukou](https://discuss.python.org/u/encukou) (Petr Viktorin) November 11, 2026, 8:32am 7

When I was finishing university, head full of textbook algorithms, I read [`Objects/listsort.txt`](https://github.com/python/cpython/blob/2fc68e180ffdb31886938203e89a75b220a58cec/Objects/listsort.txt) and was overwhelmed by how much depth, rigor, common sense and *humanity* can fit in a technical note. That was when I picked a programming language.

22 Likes

 [pitrou](https://discuss.python.org/u/pitrou) (Antoine Pitrou) November 11, 2026, 9:53am 8

Tim’s main legacy for me has been the Zen of Python, which - despite being less cited nowadays - has been so useful and influential as a framework for elegance and clarity, and has informed so many people in their daily practice (and I cannot find a better way to describe him than as an *excellent practitioner*).

He was already less active when I started contributing around 2007; in fact my most frequent interactions with him have been in the recent years while he grew a slightly more confrontational attitude in some discussions, including here on d.p.o. I believe [^1] he was acting with the best of intentions and was still deeply caring for the community he had been such a pivotal member of; acting with the same objectives as others, but having trouble to understand that different means might be used towards the same ends.

* * *

14 Likes

 [malemburg](https://discuss.python.org/u/malemburg) (Marc-André Lemburg) November 11, 2026, 9:58am 9

This is so sad and such a big loss for Python. Tim has given us a lot and we owe him.

He was one of the extremely smart people who drew me into PythonLand in the late 1990s. Coming out of university, I was amazed how approachable, down to earth and humurous really great folks can be.

Rest in Peace, Tim, and thanks-for-being-one-of-those-great-people-ly, Marc

14 Likes

 [pf\_moore](https://discuss.python.org/u/pf_moore) (Paul Moore) November 11, 2026, 10:51am 10

So sad. I never had the chance to meet Tim in person, but his posts were always a delight to read, full of knowledge, insight and humour. I learned more than I can express from him. He will be missed.

12 Likes

 [hodgestar](https://discuss.python.org/u/hodgestar) (Simon Cross) November 11, 2026, 10:55am 11

One of the great souls, and roughly treated here near the end.

12 Likes

 [nas](https://discuss.python.org/u/nas) (Neil Schemenauer) November 11, 2026, 12:31pm 12

A +1 to what Chris wrote.

Tim was always pragmatic, preferring simple solutions over complex ones. In the early days of syntax highlighting in text editors, like Emacs and Vim, I remember that Python’s triple-quoted strings were tricky to get right. They span multiple lines and opening and closing tokens are identical, so it’s easy to get out of sync. Doctests make things especially tricky.

I was telling Tim about some techniques I was using in the Vim syntax file to make it work more reliably. I don’t recall the editor Tim used (some commercial program on Windows) but he explained how he did it in his editor. Open the syntax highlighting config window, for open string token, type three quotes, for close string, type three quotes, close config window. Job done. That tickled him.

He primarily used Windows 95 or 98 at a time when many Python core devs used NT, Unix, or Linux. He said if you installed *all* of Microsoft’s updates, it was quite reliable. Maybe he preferred it because it worked for him and there was no reason to change. Or, he found it useful when supporting people, since Windows was the most common platform Python was used on. Tim was about the furthest thing from an elitist you could be.

This Bible quote comes to mind for me: “Ye shall know them by their fruits.”. Tim gave us a lot of fruit.

I poked around at Python 2.4, since that seems a good version to show off the things he worked on. His contributions were all over the codebase but I’ll point out a few that stand out to me. [pymalloc](https://github.com/python/cpython/blob/v2.4/Objects/obmalloc.c) was contributed by Vladimir Marangozov but it was Tim who polished it to diamond-like form. A memory allocator is one of those things where it is impossible to have something completely optimal; you always make tradeoffs. Tim’s special contribution in that unit is `Py_ADDRESS_IN_RANGE`. It is well commented, if you want to understand how it works. Technically, it relies on undefined behavior but Tim figured it should work in practice. It did so for many years.

At some point I wanted to increase the pymalloc pool size, since typical RAM sizes had increased so much since 2000. Tim [helped](https://gist.github.com/nascheme/bf32c1c018fafeb18a050b1cc9dc7d2b) me optimize the `address_in_range()` check.

TimSort is a good example of Tim’s approach to software problems. Someone must have “nerd sniped” him on the problem of sorting. He already had an encyclopedic knowledge of the classic algorithms, like you’d find in Knuth. He spent a few days reading the recent research papers on sorting. With that knowledge and throwing in a few twists of his own, he made TimSort. He was always thinking of the common case, the favorable case as well as the worst-case scenarios. TimSort does all three well (it exploits already sorted runs, and uses galloping during merging to copy long stretches efficiently).

Speaking of Knuth, the [dict object](https://github.com/python/cpython/blob/v2.4/Objects/dictobject.c) algorithms were heavily optimized by Tim. The C file contains lots of comments. He also wrote [dictnotes](https://github.com/python/cpython/blob/v2.4/Objects/dictnotes.txt), which is a good example of his technical writing. Another example is [weakref.txt](https://github.com/python/cpython/blob/v2.4/Modules/gc_weakref.txt), which I think Tim wrote after fixing a particularly difficult GC bug revealed by ZODB (while working at Zope Corp).

On the topic of GC, the initial version of the cyclic garbage collector was written by me, merged before the 2.0 release. It was Tim who polished it into robust and well-performing shape. Likewise, I wrote the initial version of “simple generators” but it was Tim who put them on solid foundation, both the PEP and the implementation.

Tim was skilled with numerical algorithms and error analysis, and Python benefited from his expertise. The float object and math module got special attention from him. He also worked on the Karatsuba multiplication algorithms in [longobject](https://github.com/python/cpython/blob/v2.4/Objects/longobject.c).

In addition to the C code, Tim also wrote beautiful Python code. As an example, see the N-Queens and Knight’s tour solvers in [test\_generators.py](https://github.com/python/cpython/blob/v2.4/Lib/test/test_generators.py). He wrote the initial version of [doctest](https://github.com/python/cpython/blob/v2.2/Lib/doctest.py). Some more examples of his work: [difflib](https://github.com/python/cpython/blob/v2.2/Lib/difflib.py), [pickletools](https://github.com/python/cpython/blob/v2.4/Lib/pickletools.py), [tabnanny](https://github.com/python/cpython/blob/v2.4/Lib/tabnanny.py) (she shows no mercy!).

It’s clear that Tim enjoyed writing code and solving problems. It’s also the case that he spent many thousands of hours of his time doing things primarily to help his fellow humans, without expectation of financial gain or of glory. Thank you, friend, for that and thank you for setting such an example.

27 Likes

 [vinitkme](https://discuss.python.org/u/vinitkme) (Vinit Kumar) November 11, 2026, 12:58pm 13

Thanks for everything Tim. Zen of Python has always been and will be my programming " North Star" and I strive to use it in every language I work with. Your impact has been on many of us Python Programmers and our families lives. Thank you and rest in peace! ![:heart:](https://emoji.discourse-cdn.com/apple/heart.png?v=16 ":heart:")

5 Likes

 [Stefan2](https://discuss.python.org/u/Stefan2) (Stefan Pochmann) November 11, 2026, 1:09pm 14

I’ve been fearing this since I noticed his absence end of July, dreading the thought whenever I saw his name but still no new activity. I’ve had quite a few interactions with Tim, he was one of my main contact points to Python, and he was the one who added me to [Misc/ACKS](https://github.com/python/cpython/blob/main/Misc/ACKS). Thank you for everything, Tim.

5 Likes

 [Paddy3118](https://discuss.python.org/u/Paddy3118) (Paddy3118) November 11, 2026, 1:30pm 16

2 Likes

 [diegor](https://discuss.python.org/u/diegor) (Diego Russo) November 11, 2026, 2:39pm 17

I wish I’d had the chance to meet Tim in person. We had just a few exchanges on social media, but I always enjoyed reading his messages. There was always something to learn from his experience, his perspective and his way of thinking. Reading the memories shared here by those who knew and worked with him makes me regret even more that I never had that opportunity.

I remember when Timsort was created. I was at university, studying algorithms, and it was a big deal at the time.

But it’s the Zen of Python that influenced me most personally.

**“Now is better than never.”**

That sentence has stayed with me since I started using Python in the early 2000s. Over the years, it has become one of my personal mottos, encouraging me to keep trying, strive to do better and step outside my comfort zone, both in life and at work.

We’ll probably never know what the 20th aphorism of the Zen of Python would have been. But perhaps that’s OK. I like to think that Tim left us the freedom to fill that space with something meaningful to each of us.

Thank you, Tim, for everything you gave to Python and for the impact your work and words had on people.

Rest in peace.

9 Likes

 [holdenweb](https://discuss.python.org/u/holdenweb) (Steve Holden) November 11, 2026, 3:09pm 18

Tim and I originally thought we first met fairly shortly after we both moved to Virginia, and became friends over Chinese lunches and discussions with Guido about starting a new Python conference. As we got to know each other better we realised that we had seen each other once before, when I visited a fellow academic’s office in Manchester, England, and Tim was there representing Cray Research back in 1983. Small world. After PythonLabs dissolved on Guido’s departure for the West coast the lunches eventually became a regular date at our local PF Chang, where Tim would invariably end with their huge “Mountain of Chocolate” dessert. The resulting sugar rush was often amusing.

Tim was well-known long before I arrived in the Python world, and remained so after I left it. His technical contributions to Python, now the most popular programming language in the world, are legion. Just as significant, if not more so, was the way he communicated the joy, both of being a programmer and a human being. He was deeply affected by the ban imposed on him for ideological reasons by the Python Steering Committee but did not let that affect his desire to help others, which was on display until the end.

I respected Tim above anyone else in the Python world for his wisdom, his witty humour and his deep humanity. There won’t be another one like him. Goodbye, old friend.

13 Likes

 [sdeibel](https://discuss.python.org/u/sdeibel) (Stephan Deibel) November 11, 2026, 3:41pm 19

This is very sad. I worked with Tim for quite a few years in context of the PSF board and of course also knew him from IPC and PyCons, starting in the relatively early days. He stood out to me almost immediately because so obviously utterly brilliant and yet with such a kind, helpful, respectful, good-humored way about him. One of the best people I’ve known, he was an inspiration to me and I think clearly also to very many others. Thanks, Tim, you will be missed.

3 Likes

 [mwichmann](https://discuss.python.org/u/mwichmann) (Mats Wichmann) November 11, 2026, 3:42pm 20

I only met Tim once, along with Guido, in 2001. I was writing a Python training course, and the training company wanted to arrange for “the experts” to get a look at it, so on a week I was in Reston working on it, we sat down over a meal. It wasn’t a very good course (“if I knew then what I know now”, a quarter century later…), but they generously gave of their time to critique, including Tim’s trademark humor, and I so appreciated it. He will be missed!

3 Likes

 [mdickinson](https://discuss.python.org/u/mdickinson) (Mark Dickinson) November 11, 2026, 4:00pm 22

Tim was hugely influential on my own CPython core development journey. Initially that influence was through his voluminous writings, mailing list discussions and his code itself: when I joined the core dev team in 2008, Tim himself was absent from the scene in person, but very much present through his many many contributions. Later on, after his return to core development, I was privileged to interact more directly with Tim on many topics. His gentle erudition, his humour and his patience were endless.

Rest in peace, Tim. You will be sorely missed.

9 Likes

[^1]: and I might be full of myself here
