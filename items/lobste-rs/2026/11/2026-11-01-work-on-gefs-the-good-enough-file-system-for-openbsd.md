---
title: Work on GEFS (the Good Enough File System) for OpenBSD progresses
link: https://www.undeadly.org/cgi?action=article;sid=20261101115637
source: lobste-rs
published: 2026-11-01T20:13:29Z
updated: 2026-11-01T20:13:29Z
first_seen: 2026-11-02T16:59:20.971254334Z
authors:
- undeadly.org via jbauer
labels:
- openbsd
summary: Comments
content: extracted
html: 2026-11-01-work-on-gefs-the-good-enough-file-system-for-openbsd.html
remote_preview:
  url: https://www.undeadly.org/images/MangaRamblo_25_x.png
  alt: Front page
---

Contributed by [Peter N. M. Hansteen](https://nxdomain.no/~peter/blogposts/) on 2026-11-01 from the most excellent file shredder dept.

As *[previously](https://www.undeadly.org/cgi?action=article;sid=20260918115729)* reported, Ori Bernstein is working on GEFS, the "Good Enough File System" imported to *[OpenBSD](https://www.openbsd.org/)*, and his *[EuroBSDCon 2026](https://2026.eurobsdcon.org/) [presentation](https://events.eurobsdcon.org/2026/talk/NVMSCJ/)* was very well received. In fact, the video is among the most frequently played from the conference.

Now Ori has followed up with more evolved code that includes several important steps towards makin the new file system fit for general use. In a [message](https://marc.info/?l=openbsd-tech&m=179346643071941&w=2) to `tech@`, Ori writes:

> ```
> List:       openbsd-tech
> Subject:    Re: GEFS on OpenBSD: A very early preview
> From:       ori () eigenstate ! org
> Date:       2026-10-31 17:08:42
> 
> So, it's probably time for a few updates. First
> off, thanks to Martijn, and everyone else who
> tried it out and sent patches.
> 
> Everything is updated and rebased, and the diff
> has been regenerated with all of the lastest.
> ```

> ```
> I've gone through a the error handling code. In
> theory the filesystem will handle errors mostly
> correctly. There are probably still places where
> theory diverges from reality. I've also forward
> ported most of the important bugfixes from 9front.
> 
> The file system now leaves the first FS block
> unused by default -- we can put disklabels there,
> which makes it hard to put the superblock there as
> well.
> 
> Some userspace tools have been done (you can now
> mkfs on openbsd!), but snapshot control hasn't.
> I'm thinking we have a userspace 'gefs' program
> that lets you handle this, trying to keep the
> interface similar to the 9front ctl file. The
> commands below should suffice:
> 
>   gefs debug [0|1]
>   gefs usereserve
>   gefs retain snap timespec
>   gefs snap name newname
>   gefs fork name newname
> 
> The consistency protocol is still the biggest
> missing piece; I've been reading the old code
> for softupdates, since it had the same flavor
> of problem.
> 
> what gefs wants, in the ideal case, is some way
> to put ordering barriers in the commit phases,
> while still accepting new block i/o, something
> like the following sequence of steps:
> 
> 	0) normal use, generate dirty blocks and
> 	  write back in any order.
>       holding mutlk {
> 		1) barrier(), ensure that all blocks
> 		 written before the commit will be
> 		 on disk before the commit finishes
> 		2) enqueue free list/deadlist data
> 		3) prepare arena header/footer + superblock
> 	 }
> 	4) enqueue arena headers
> 	5) barrier()
> 	6) enqueue superblock
> 	7) enqueue arena footers
> 	8) wait for enqueued blocks to hit disk
> 	9) free blocks no longer used by previous gen
> 
> on 9front, I have a priority queue of blocks that
> are sorted by (gen, addr) tuples, which means that
> writes within a generation are flushed to disk as
> sequentially as they can be, while not crossing
> generation boundaries.
> 
> I'm currently thinking I'll implement the same in
> OpenBSD -- launch syncer kprocs that do synchronous
> I/O to the block device. If we kick things out of
> cache and write it back to disk early, that should
> generally be safe -- The only blocks the buffer
> cache must not reorder are the arena headers and
> superblocks, so a flag to pin them unwritten may
> be all that's needed here.
> 
> I'm open to other ideas, though.
> 
> Quoth ori@eigenstate.org:
> ```

Followed by a quoted part of the [previous message](https://marc.info/?l=openbsd-tech&m=178948744271633&w=2) that has the links to the code and documentation.

If you are as excited as we are about this new file system development and are in a position to test, please do.
