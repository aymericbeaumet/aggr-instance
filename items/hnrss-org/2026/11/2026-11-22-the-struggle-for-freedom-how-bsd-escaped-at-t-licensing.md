---
title: 'The struggle for freedom: How BSD escaped AT&T licensing'
link: https://fosscomics.com/posts/16.%20BSD%20Unix%20Part%206%20-%20The%20Struggle%20for%20Freedom/
source: hnrss-org
published: 2026-11-22T21:33:12Z
updated: 2026-11-22T21:33:12Z
first_seen: 2026-11-23T09:43:22.615423405Z
authors:
- joonehur
labels:
- at&t
- bsd
- csrg
- unix
content: extracted
html: 2026-11-22-the-struggle-for-freedom-how-bsd-escaped-at-t-licensing.html
remote_preview:
  url: https://fosscomics.com/posts/16.%20BSD%20Unix%20Part%206%20-%20The%20Struggle%20for%20Freedom/images/feature-freedom.png
---

Display dialogue text

- [BSD Unix, Part 1 - Unix Code Goes to UC Berkeley](https://fosscomics.com/posts/11.%20BSD%20Unix%20Part%201%20-%20Unix%20Code%20Goes%20to%20UC%20Berkeley/)
- [BSD Unix, Part 2 - The Birth of the vi Editor](https://fosscomics.com/posts/12.%20BSD%20Unix%20Part%202%20-%20The%20Birth%20of%20the%20vi%20Editor/)
- [BSD Unix, Part 3 - The Beginning of BSD Unix](https://fosscomics.com/posts/13.%20BSD%20Unix%20Part%203%20-%20The%20Beginning%20of%20BSD%20Unix/)
- [BSD Unix, Part 4 - The Formation of CSRG](https://fosscomics.com/posts/14.%20BSD%20Unix%20Part%204%20-%20The%20Formation%20of%20CSRG/)
- [BSD Unix, Part 5 - Bringing TCP/IP to BSD](https://fosscomics.com/posts/15.%20BSD%20Unix%20Part%205%20-%20The%20Development%20of%20TCP-IP/)

## BSD Unix, Part 6

## The Struggle for Freedom

![A helmeted figure and a Unix developer cross red and blue lightsabers.](https://fosscomics.com/posts/16.%20BSD%20Unix%20Part%206%20-%20The%20Struggle%20for%20Freedom/images/struggle-for-freedom.png)

Early Unix was available through inexpensive source-code licenses because a 1956 antitrust settlement kept AT&T out of the commercial computer business \[6\]. Some early licenses cost just $99 \[1\].

> Too cheap.

> One tape,\
> please.

> Schools\
> too?

> Companies\
> too?

> Research\
> labs too?

[Defense Advanced Research Projects Agency (DARPA)](<https://en.wikipedia.org/wiki/DARPA >) also valued Unix's source-code availability. That was why it chose [BSD](<https://en.wikipedia.org/wiki/Berkeley_Software_Distribution >) Unix over [DEC](<https://en.wikipedia.org/wiki/Digital_Equipment_Corporation >)'s [VMS](<https://en.wikipedia.org/wiki/OpenVMS >) for building the Internet \[1\].

![A DARPA representative folds his arms over three choices: a DEC VMS installation tape, a BSD installation tape and BSD source code.](https://fosscomics.com/posts/16.%20BSD%20Unix%20Part%206%20-%20The%20Struggle%20for%20Freedom/images/choosing-bsd.png)

But AT&T's breakup in 1984 opened the way for it to sell Unix commercially. AT&T abruptly raised the price from $99 to $250,000 \[1\].

> AT&T Unix fees

> Khh...

BSD still required an AT&T license. When fees rose, some companies wanted its networking code separately.

> How can I get just BSD's\
> TCP/IP and vi source code?

> AT&T's raised the fees.\
> Users want just our\
> networking code and tools.

> Let's release our\
> networking code\
> and tools separately.

> How's the release\
> coming?

> Fine. What about\
> the license?

> Let's write our own BSD\
> license. BSD users can use\
> our code freely. They just\
> need to put the copyright\
> notice in the manual if\
> they make a product or\
> redistribute the source.\
> That'll throw AT&T off.

> Anything goes, huh? Well, we don't want to put\
> restrictions on people like AT&T does.

[CSRG](<https://en.wikipedia.org/wiki/Computer_Systems_Research_Group >) released its networking code and utilities as Networking Release 1 (Net/1) in June 1989. Its new BSD license allowed use without an AT&T source license. People could modify the code or redistribute it as source or binaries without paying Berkeley royalties. They had to retain the source copyright notices and credit UC Berkeley and its contributors in product documentation.

![Large red letters spell BSD against a white background.](https://fosscomics.com/posts/16.%20BSD%20Unix%20Part%206%20-%20The%20Struggle%20for%20Freedom/images/releasing-net1.png)

> BSD's networking release\
> is finally here.

> How\
> much?

> They'll mail you a tape\
> for $1,000. Or download it\
> free by FTP. We can even\
> change and redistribute\
> the code without royalties.

## Meanwhile, at CSRG

> Professor, the networking\
> release is selling well.

> Grants, tape sales...\
> Development's funded\
> for a while.

> Time for the weekly meeting.

> Professor, why don't we make a version of BSD\
> that doesn't require an AT&T Unix license?

> We can't keep depending on AT&T.\
> We've already changed much of the kernel.\
> It's the tools and C library we need to rewrite.\
> Can we do it quickly?

> Let's try. With help from\
> outside contributors, we can\
> rewrite the AT&T code.

CSRG decided to rewrite AT&T's tools and C library. With help from outside contributors, most of the code was rewritten in eighteen months.

![Contributors work at terminals in four separate locations.](https://fosscomics.com/posts/16.%20BSD%20Unix%20Part%206%20-%20The%20Struggle%20for%20Freedom/images/rewriting-unix-tools.png)

With most of AT&T's Unix code removed, a completely free BSD Unix was finally released in June 1991. The source was still available on tape for the $1,000 copying fee, or as a free download by FTP.

> Now we can sell\
> BSD Unix too.

[CSRG](<https://en.wikipedia.org/wiki/Computer_Systems_Research_Group >) founded a company called [BSDi](<https://en.wikipedia.org/wiki/Berkeley_Software_Design >) to sell BSD Unix commercially. The University of California also began selling BSD Unix in the early 1990s. An advertisement claimed it was 95% cheaper than AT&T Unix. That did not go down well with AT&T.

![A cloaked figure sits in a high-backed chair, reading an advertisement that says BSD Unix is 95% cheaper.](https://fosscomics.com/posts/16.%20BSD%20Unix%20Part%206%20-%20The%20Struggle%20for%20Freedom/images/advertising-bsd.png)

## The Empire Strikes Back

In April 1992, AT&T subsidiary USL sued BSDi. UC Berkeley was added to the lawsuit later.

> I thought we'd removed\
> all the AT&T code...\
> What's this?

But AT&T's [Unix System V](<https://en.wikipedia.org/wiki/UNIX_System_V >) also included plenty of BSD code, such as virtual memory, TCP/IP and vi. The BSD license allowed anyone to use the code if they kept the copyright notices. AT&T removed those notices from the source and sold Unix as though it had written all the code itself.

![A slice labeled TCP/IP, Virtual Memory and VI is marked BSD code within a diagram of AT&T Unix source code.](https://fosscomics.com/posts/16.%20BSD%20Unix%20Part%206%20-%20The%20Struggle%20for%20Freedom/images/bsd-in-system-v.png)

> AT&T's suing us for distributing\
> Unix code without permission.

> That's absurd. System V includes\
> our virtual memory, TCP/IP\
> and vi code.

> Exactly. We should\
> countersue.

The BSD side filed a countersuit against AT&T.

![The cloaked figure in the high-backed chair reads a document headed \"Complaint\".](https://fosscomics.com/posts/16.%20BSD%20Unix%20Part%206%20-%20The%20Struggle%20for%20Freedom/images/receiving-countersuit-en.png)

In February 1994, the two sides agreed that each could sell Unix, provided CSRG removed some remaining AT&T Unix code.

![A judge compares documents between piles of BSD Unix and AT&T Unix source code while a man on the right comments on his work.](https://fosscomics.com/posts/16.%20BSD%20Unix%20Part%206%20-%20The%20Struggle%20for%20Freedom/images/settling-lawsuit-en.png)

> It's the same code...\
> Wouldn't want his job.

Although BSD Unix began with AT&T's code, much of it had been rewritten, and evidence had emerged that AT&T had incorporated BSD's TCP/IP, virtual memory and vi code while removing copyright notices. But legal uncertainty hung over BSD distribution during the lawsuit, and the two years lost before a completely free BSD Unix appeared gave Linux room to grow.

> If I could have installed BSD Unix\
> on my Intel 386 PC,\
> I wouldn't have needed to write Linux.

**That wraps up our six-part story of BSD Unix!**

## References

1. Andrew Leonard, [BSD Unix: Power to the people, from the code](<https://www.salon.com/2000/05/16/chapter_2_part_one/ >), 2000
2. Marshall Kirk McKusick, [Twenty Years of Berkeley Unix: From AT&T-Owned to Freely Redistributable](<https://www.oreilly.com/openbook/opensources/book/kirkmck.html_original >), in Open Sources: Voices from the Open Source Revolution, O'Reilly, 1999
3. [Computer Systems Research Group](<https://en.wikipedia.org/wiki/Computer_Systems_Research_Group >)
4. [The Design and Implementation of the 4.4BSD Operating System: Network Implementation](<https://www.freebsd.org/doc/en/books/design-44bsd/overview-network-implementation.html >)
5. Mike Linksvayer, [Interview with Linus Torvalds](<https://gondwanaland.com/meta/history/interview.html >), Meta Magazine, 1993
6. Federal Judicial Center, [The Breakup of "Ma Bell": United States v. AT&T](<https://www.fjc.gov/history/spotlight-judicial-history/breakup-ma-bell >)
7. Dennis Ritchie, [Old Unix Licenses and Price Lists](<https://www.nokia.com/bell-labs/about/dennis-m-ritchie/licenses.html >)
