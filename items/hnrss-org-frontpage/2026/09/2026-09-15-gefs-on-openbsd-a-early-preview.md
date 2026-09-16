---
title: 'GEFS on OpenBSD: A Early Preview'
link: https://marc.info/?l=openbsd-tech&m=178948744271633&w=2
source: hnrss-org-frontpage
published: 2026-09-15T17:12:19Z
updated: 2026-09-15T17:12:19Z
first_seen: 2026-09-16T01:36:55.400042967Z
authors:
- sippingabonedry
summary: 'Article URL: https://marc.info/?l=openbsd-tech&m=178948744271633&w=2 Comments URL: https://news.ycombinator.com/item?id=49715590 Points: 108 # Comments: 61'
content: extracted
html: 2026-09-15-gefs-on-openbsd-a-early-preview.html
---

```
[prev in list] [next in list] [prev in thread] [next in thread] 

List:       openbsd-tech
Subject:    GEFS on OpenBSD: A very early preview
From:       ori () eigenstate ! org
Date:       2026-09-15 15:45:05
Message-ID: EBB049B251DCF60A8C9A0575F6FC7DC9 () eigenstate ! org
[Download RAW message or body]

Before anyone asks: I am *not* proposing to put
this in tree for a while yet.

What is it
----------
So, as people may have seen at EuroBSD, I've got a
rough, buggy, issue-filled preview of GEFS running
on OpenBSD. The port is NOT ready for production,
and data loss is currently expected, especially on
error.

However, the port has reached the state where (with
some exceptions) the remaining problems are reasonably
understood, and people can poke at them.

For those who haven't watched my talk, GEFS is a
new, crash-safe, snapshotting, copy on write FS
that I wrote for 9front, and which I am in the
process of moving to OpenBSD. The file system is
described in full here:

	https://orib.dev/gefs.pdf

It's under 9,000 lines of code in kernel at the
moment, though I expect it'll grow a bit over time,
since there's a good deal missing.

Porting
-------
I'm approaching this as a proper, in-kernel file
system that keeps the data structure and fiddly
code fairly in sync between Plan 9 and OpenBSD,
but the code itself is copied and pasted. My goal
is that it will continue to rhyme, so that fixes
can be shared, but the requirements of the two
environments means that the code probably won't
be ported, and trying to shove in some abstraction
layer seems like a bad idea.

Problems
--------
So, IMO, the biggest of the remaining problems:

Consistency Protocol:
  This is the big unknown for me; A write to the
  superblock needs to come after all writes in
  the snapshot, but as long as it is written after
  the blocks have hit disk, it's safe. There are
  also a couple of fixes to backport from 9Front
  on the write ordering.

Error handling and porting brain damage:
  Error handling is largely commented out. I'm
  going to have to go through the error handling
  paths bit by bit and convert it with care; the
  approach to error handling I took on 9front is
  not acceptable for OpenBSD. There's a few other
  things, like some globals being used that would
  prevent more than one mount at a time.

Userspace tools:
  At least, I assume that telling people to boot
  9front in a VM to create or fsck is not a good
  long term solution. Snapshot management and the
  associated ioctls will also go here.

Posix persnicketies:
  The system that this was ported from was not
  a posix system, so there are likely to be many
  places where we don't quite get the constraints
  right.

Regress:
  There's a small test suite for Plan 9. There's
  none for OpenBSD.

Hardlinks:
  Easy to implement, but needs a refcount file
  by file.

Kqueue:
  Easy to implement, but needs a refcount file
  by file.

NFS:
  Has some ugly hooks, not sure what would be
  needed to add them.

Other missing pieces:
  Things like bootloader support, adding boot
  environments, quotas, and other niceties; this
  is well  down the list. Quotas, in particular,
  would allow LLVM to continue in its attempt to
  grow to the size of the known universe without
  us preallocating a /usr/obj of that size.

I'm sure I've forgotten some.

Right now: I'm not looking to fix KNF; as things
get touched, I'll start to move them over, but I
would prefer to keep the code closer with the
original until it's closer to ready for upstreaming.

There are also a few non-fatal issues independent
of 9front and OpenBSD, such as slow deletions of
large files; I've got ideas to improve them.

Getting it
----------

as one patch:

  https;//orib.dev/gefs.diff

The git repo is hidden on shithub, and the
server is tiny and will get overloaded if
people clone fresh repos from it. Instead,
get a copy of the initial repo from github,
and pull only the updates.

  # first, get a copy of the upstream from
  # somewhere else
  git clone https://github.com/openbsd/src
  cd src

  # now, add my server, and get just the diffs
  git remote add gefs git://shithub.us/ori/openbsd
  git fetch gefs
  git checkout -b gefs gefs/gefs

Note, I will be rebasing and force-pushing
the branch occasionally in order to keep up
with openbsd current; commit ids will change.

[prev in list] [next in list] [prev in thread] [next in thread] 
```

\

[Configure](https://marc.info/?q=configure) | [About](https://marc.info/?q=about) | [News](https://marc.info/?q=news) | [Add a list](mailto:webguy@marc.info?subject=Add%20a%20list%20to%20MARC) | Sponsored by [KoreLogic](http://www.korelogic.com/)
