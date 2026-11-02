---
title: 'F/OSS Comics #13: BSD Unix, Part 3 – The Beginning of BSD Unix'
link: https://fosscomics.com/posts/13.%20BSD%20Unix%20Part%203%20-%20The%20Beginning%20of%20BSD%20Unix/
source: hnrss-org
published: 2026-11-02T01:59:38Z
updated: 2026-11-02T01:59:38Z
first_seen: 2026-11-02T16:59:20.971254334Z
authors:
- joonehur
labels:
- bsd
- unix
content: extracted
html: 2026-11-02-f-oss-comics-13-bsd-unix-part-3-the-beginning-of-bsd-unix.html
remote_preview:
  url: https://fosscomics.com/posts/13.%20BSD%20Unix%20Part%203%20-%20The%20Beginning%20of%20BSD%20Unix/images/feature.png
---

- [BSD Unix, Part 1 - Unix Code Goes to UC Berkeley](https://fosscomics.com/posts/11.%20BSD%20Unix%20Part%201%20-%20Unix%20Code%20Goes%20to%20UC%20Berkeley/)
- [BSD Unix, Part 2 - The Birth of the vi Editor](https://fosscomics.com/posts/12.%20BSD%20Unix%20Part%202%20-%20The%20Birth%20of%20the%20vi%20Editor/)

**The beginning of BSD Unix**

When Ken Thompson returned to Bell Labs in 1976, Bill Joy and Chuck Haley began exploring the Unix kernel themselves.

> Hmm... we could speed things up here in the kernel.

> Let's try it.

> Other universities want copies of our Pascal compiler.

> Let's bundle it with ex and send them a tape.

In early 1977, Bill Joy put together the Berkeley Software Distribution, or BSD, bundling the Pascal system with ex, a line editor developed from em. Early BSD was not a standalone operating system: it was a collection of software to install on an existing Unix system. About thirty copies were sent out over the following year.[\[1\]](<https://www.oreilly.com/openbook/opensources/book/kirkmck.html >)

> Thirty tapes to copy...

At the time, an AT&T Unix source license was needed to get BSD. Berkeley charged only for the tape and distribution.[\[2\]](<https://www.salon.com/2000/05/16/chapter_2_part_one/ >)

> If you already have a Unix source license, it's just the tape and postage.

Berkeley already had a tradition of sharing research software. BSD carried it on. Berkeley's Free Speech Movement, hippie culture, and antiwar activism are also said to have influenced Joy's decision to charge so little.[\[2\]](<https://www.salon.com/2000/05/16/chapter_2_part_one/ >)

![People march together carrying a sign that reads FREE SPEECH.](https://fosscomics.com/posts/13.%20BSD%20Unix%20Part%203%20-%20The%20Beginning%20of%20BSD%20Unix/images/free-speech.png)

Universities and research institutions could obtain Unix, including its source code, for a modest licensing fee. This also helped the system spread.[\[2\]](<https://www.salon.com/2000/05/16/chapter_2_part_one/ >)\
[\[6\]](<https://spectrum.ieee.org/the-strange-birth-and-long-life-of-unix >)

> We have Unix, but the antitrust agreement bars us from the computer business... We can't sell it as a product, either.

**The second distribution: 2BSD**

After the first BSD distribution, Joy improved the Pascal system based on user feedback. He bundled it with vi (ex's visual mode), the C shell, and other tools to form the Second Berkeley Software Distribution, or 2BSD. Nearly seventy-five tapes were shipped over the following year.[\[1\]](<https://www.oreilly.com/openbook/opensources/book/kirkmck.html >)\
[\[5\]](<https://www.tuhs.org/Archive/Distributions/UCB/2BSD/ >)

Joy did more than fix bugs. He handled every distribution request, copied BSD onto tapes, and mailed them out himself.

> Oh, you need a copy of BSD?

> Another university wants a copy.

> Tune the kernel. Fix the bugs.

> More copying.

**A VAX arrives at Berkeley, 1978**

> Professor Fateman's group is getting a VAX. It's 32-bit, with a whole megabyte of memory.

> Really? We should put Unix on it.

> They've already got VAX Unix from Bell Labs. It's called 32/V.

> Hi, I'm Ozalp Babaoglu.

> My adviser asked me to add virtual memory to VAX Unix. Could you look over the code?

> Sure. Let's take a look.

Babaoglu was a Berkeley graduate student working with Professor Domenico Ferrari. Joy helped him integrate the virtual-memory code into 32/V and debug it.[\[1\]](<https://www.oreilly.com/openbook/opensources/book/kirkmck.html >)\
[\[4\]](<https://www.cs.unibo.it/~babaoglu/ >)

At the time, 32/V did not use the VAX hardware's virtual-memory capabilities. A program's address space was limited by physical memory, but Professor Richard Fateman's work on the computer algebra system [Macsyma](<https://en.wikipedia.org/wiki/Macsyma >) needed more. Virtual memory uses disk storage to give programs an address space larger than physical memory.[\[1\]](<https://www.oreilly.com/openbook/opensources/book/kirkmck.html >)

Macsyma, developed at MIT beginning in the late 1960s, worked with symbolic expressions. It could factor expressions, compute derivatives, and perform integration.

Debugging continued through the holidays at the end of 1978.

> Not going away for the holidays?

> Might as well debug this while fewer people are using the VAX.

By January 1979, most of the bugs had been fixed.[\[1\]](<https://www.oreilly.com/openbook/opensources/book/kirkmck.html >)

> Let's try Unix with virtual memory.

> Finally. It runs without crashing.

Joy ported ex, vi, the C shell, and other 2BSD tools to the VAX, while Peter Kessler and Marshall Kirk McKusick ported the Pascal system. These programs were bundled with the standard 32/V utilities and the virtual-memory kernel that Babaoglu and Joy had integrated and debugged. The result was 3BSD, which began shipping in December 1979.[\[1\]](<https://www.oreilly.com/openbook/opensources/book/kirkmck.html >)

> At last, we can send these out.

Since 1969, ARPANET had linked research institutions funded by DARPA, the Defense Advanced Research Projects Agency.[\[3\]](<https://www.sri.com/hoi/arpanet/ >) But these institutions used different computers and operating systems, so they often couldn't share software. Porting the existing software to new machines was also costly. DARPA became interested in Unix's portability, and took notice of Berkeley's successful distribution of 3BSD to research institutions.[\[1\]](<https://www.oreilly.com/openbook/opensources/book/kirkmck.html >)

> BSD runs pretty well on this VAX.

**References**

1. Marshall Kirk McKusick, [Twenty Years of Berkeley Unix](<https://www.oreilly.com/openbook/opensources/book/kirkmck.html >), in *Open Sources: Voices from the Open Source Revolution*, 1999. [Korean edition, Hanbit Media](<http://www.hanbit.co.kr/store/books/look.php?p_code=E5329835060 >), 2013.
2. Andrew Leonard, [BSD Unix: Power to the people, from the code](<https://www.salon.com/2000/05/16/chapter_2_part_one/ >), Salon, 2000.
3. SRI International, [ARPANET](<https://www.sri.com/hoi/arpanet/ >). Its origins and the first transmission in 1969.
4. Ozalp Babaoglu, [Biographical note](<https://www.cs.unibo.it/~babaoglu/ >). His Unix virtual-memory research as a Berkeley graduate student.
5. The Unix Heritage Society (TUHS), [Original 2BSD distribution archive](<https://www.tuhs.org/Archive/Distributions/UCB/2BSD/ >). The installation document `install.ms` and source code in `2bsd.tar.gz`.
6. Warren Toomey, [The Strange Birth and Long Life of Unix](<https://spectrum.ieee.org/the-strange-birth-and-long-life-of-unix >), IEEE Spectrum, 2011.
