---
title: Linus Torvalds on AI-assisted kernel contributions
link: https://lore.kernel.org/lkml/CAHk-=whTs09_ozN+e09GU68N1wPP56MUzDjrg9rnKxQUa0A_4Q@mail.gmail.com/
source: lobste-rs
published: 2026-10-16T02:50:33Z
updated: 2026-10-16T02:50:33Z
first_seen: 2026-10-16T18:46:36.999527378Z
authors:
- lore.kernel.org via thang
labels:
- linux
- vibecoding
summary: Comments
content: extracted
html: 2026-10-16-linus-torvalds-on-ai-assisted-kernel-contributions.html
---

```
linux-kernel.vger.kernel.org archive mirror
 help / color / mirror / Atom feed
```

```
From: Linus Torvalds <torvalds@linux-foundation.org>
To: Laurent Pinchart <laurent.pinchart@ideasonboard.com>
Cc: Kees Cook <kees@kernel.org>, Jori Koolstra <jkoolstra@xs4all.nl>,
	 "Lorenzo Stoakes (ARM)" <ljs@kernel.org>,
	Krzysztof Kozlowski <krzk@kernel.org>,
	Sasha Levin <sashal@kernel.org>,
	corbet@lwn.net,  skhan@linuxfoundation.org,
	rdunlap@infradead.org, federico.vaga@vaga.pv.it,
	 apw@canonical.com, joe@perches.com, workflows@vger.kernel.org,
	 linux-doc@vger.kernel.org, linux-kernel@vger.kernel.org,
	 Greg Kroah-Hartman <gregkh@linuxfoundation.org>,
	Justin Stitt <justinstitt@google.com>,
	 Carlos Maiolino <cem@kernel.org>,
	Jakub Kicinski <kuba@kernel.org>,
	Brian Foster <bfoster@redhat.com>,
	 Christoph Hellwig <hch@infradead.org>,
	David Disseldorp <ddiss@suse.de>, Mark Brown <broonie@kernel.org>,
	 Jani Nikula <jani.nikula@intel.com>,
	Jens Axboe <axboe@kernel.dk>,
	 "David Hildenbrand (Arm)" <david@kernel.org>,
	Vlastimil Babka <vbabka@kernel.org>,
	 "Christian Brauner (Amutable)" <brauner@kernel.org>
Subject: Re: [PATCH] docs: remove Assisted-by references
Date: Tue, 13 Oct 2026 13:45:09 -0700	[thread overview]
Message-ID: <CAHk-=whTs09_ozN+e09GU68N1wPP56MUzDjrg9rnKxQUa0A_4Q@mail.gmail.com> (raw)
In-Reply-To: <20261013201628.GO4214@killaraus.ideasonboard.com>

On Tue, 13 Oct 2026 at 13:16, Laurent Pinchart
<laurent.pinchart@ideasonboard.com> wrote:
>
> What this is about is setting our expectations, telling what we want and
> what we don't want.

No. What you are about is something entirely different. It has nothing
to do with "expectations", and it has everything to do with your
hangups.

I don't understand this whole idiotic concept of "don't use AI for X".

Here's a new rule just for you: don't use compilers for any of your
kernel submissions.

Because if you can't show that you really understand what the machine
is actually doing by writing your code in assembler, then I don't want
to see your code contribution. You clearly aren't competent if you
can't write it that way.

And btw, you had better not use an editor to write that commit message either.

Because I need you to show me that you can actually write a good
commit message without using tools that make it easy to backspace and
hide the mistakes you made while typing.

Ok?

Because otherwise those tool-assisted commits of yours clearly aren't
showing that you know what you are doing.

Do you not see how UTTERLY STUPID those kinds of rules would be?

So no. We're not going to make stupid rules that say "you can't use
tools for X".

The kind of expectations *I* have is that people do their best and I
expect people to understand what they are doing.

But that understanding has very little to do with what tools they use.

And quite honestly, in my experience, the people I respect and call
intelligent are not the ones that shy away from using new and
interesting tools.

                   Linus
```

* * *

```
next prev parent reply	other threads:[~2026-10-13 20:45 UTC|newest]

Thread overview: 66+ messages / expand[flat|nested]  mbox.gz  Atom feed  top
2026-10-09  8:31 [PATCH] docs: remove Assisted-by references Sasha Levin
2026-10-09  9:38 ` Laurent Pinchart
2026-10-09 10:09   ` Sasha Levin
2026-10-09 10:05 ` Krzysztof Kozlowski
2026-10-09 10:24   ` Greg KH
2026-10-09 15:20   ` Jonathan Corbet
2026-10-11  1:04     ` Steven Rostedt
2026-10-12 15:42   ` Lorenzo Stoakes (ARM)
2026-10-13 11:19     ` Jori Koolstra
2026-10-13 12:38       ` Laurent Pinchart
2026-10-13 12:50         ` Jori Koolstra
2026-10-13 13:01           ` Laurent Pinchart
2026-10-13 13:24         ` Ricardo B. Marlière (SUSE)
2026-10-13 12:49       ` Lorenzo Stoakes (ARM)
2026-10-13 17:16         ` Laurent Pinchart
2026-10-13 12:31     ` Laurent Pinchart
2026-10-13 12:59       ` Lorenzo Stoakes (ARM)
2026-10-13 13:04         ` Krzysztof Kozlowski
2026-10-13 13:07           ` Lorenzo Stoakes (ARM)
2026-10-13 13:12         ` Laurent Pinchart
2026-10-13 13:29           ` Lorenzo Stoakes (ARM)
2026-10-13 13:37             ` Jori Koolstra
2026-10-13 14:52             ` Kees Cook
2026-10-13 14:54               ` Jori Koolstra
2026-10-13 14:58               ` Jori Koolstra
2026-10-13 17:12                 ` Kees Cook
2026-10-13 17:42                   ` Laurent Pinchart
2026-10-13 19:25                     ` Kees Cook
2026-10-13 20:16                       ` Laurent Pinchart
2026-10-13 20:45                         ` Linus Torvalds [this message]
2026-10-14 13:56                           ` Lorenzo Stoakes (ARM)
2026-10-16 14:19                             ` Matthew Brost
2026-10-17 17:42                               ` Troy Benjegerdes
2026-10-19 17:41                           ` lyude
2026-10-14 18:47                         ` Kees Cook
2026-10-15 23:14                           ` Jeremy Dean Clifton
2026-10-13 15:02               ` Lorenzo Stoakes (ARM)
2026-10-13 16:58                 ` Kees Cook
2026-10-13 17:32             ` Laurent Pinchart
2026-10-16 16:14               ` Lorenzo Stoakes (ARM)
2026-10-14  3:23             ` Theodore Tso
2026-10-14 11:23               ` Mark Brown
2026-10-14 13:08               ` Jori Koolstra
2026-10-14 13:42               ` Lorenzo Stoakes (ARM)
2026-10-14 14:15                 ` Mark Brown
2026-10-19 22:44                 ` Laurent Pinchart
2026-10-19 16:32               ` lyude
2026-10-19 22:24               ` Laurent Pinchart
2026-10-19 16:26             ` lyude
2026-10-16 18:26           ` Ihor Solodrai
2026-10-19 16:21           ` lyude
2026-10-13  4:24 ` Kees Cook
2026-10-13  5:56   ` Randy Dunlap
2026-10-13  6:18     ` Greg KH
2026-10-13  8:25     ` Kees Cook
2026-10-13 12:28       ` [PATCH] docs: remove Assisted-by reference Jori Koolstra
2026-10-13 12:55         ` Laurent Pinchart
2026-10-13 13:03           ` Jori Koolstra
2026-10-13 13:10             ` Laurent Pinchart
2026-10-13 13:20               ` Jori Koolstra
2026-10-13 13:55                 ` Laurent Pinchart
2026-10-19 16:50                 ` lyude
2026-10-13 15:18           ` Mark Brown
2026-10-13 17:07             ` Laurent Pinchart
2026-10-13 20:01               ` Shuah Khan
2026-10-19 16:15         ` lyude
```

* * *

```
Reply instructions:

You may reply publicly to this message via plain-text email
using any one of the following methods:

* Save the following mbox file, import it into your mail client,
  and reply-to-all from there: mbox

  Avoid top-posting and favor interleaved quoting:
  https://en.wikipedia.org/wiki/Posting_style#Interleaved_style

* Reply using the --to, --cc, and --in-reply-to
  switches of git-send-email(1):

  git send-email \
    --in-reply-to='CAHk-=whTs09_ozN+e09GU68N1wPP56MUzDjrg9rnKxQUa0A_4Q@mail.gmail.com' \
    --to=torvalds@linux-foundation.org \
    --cc=apw@canonical.com \
    --cc=axboe@kernel.dk \
    --cc=bfoster@redhat.com \
    --cc=brauner@kernel.org \
    --cc=broonie@kernel.org \
    --cc=cem@kernel.org \
    --cc=corbet@lwn.net \
    --cc=david@kernel.org \
    --cc=ddiss@suse.de \
    --cc=federico.vaga@vaga.pv.it \
    --cc=gregkh@linuxfoundation.org \
    --cc=hch@infradead.org \
    --cc=jani.nikula@intel.com \
    --cc=jkoolstra@xs4all.nl \
    --cc=joe@perches.com \
    --cc=justinstitt@google.com \
    --cc=kees@kernel.org \
    --cc=krzk@kernel.org \
    --cc=kuba@kernel.org \
    --cc=laurent.pinchart@ideasonboard.com \
    --cc=linux-doc@vger.kernel.org \
    --cc=linux-kernel@vger.kernel.org \
    --cc=ljs@kernel.org \
    --cc=rdunlap@infradead.org \
    --cc=sashal@kernel.org \
    --cc=skhan@linuxfoundation.org \
    --cc=vbabka@kernel.org \
    --cc=workflows@vger.kernel.org \
    /path/to/YOUR_REPLY

  https://kernel.org/pub/software/scm/git/docs/git-send-email.html

* If your mail client supports setting the In-Reply-To header
  via mailto: links, try the mailto: link
```

Be sure your reply has a **Subject:** header at the top and a blank line before the message body.

* * *

```
This is a public inbox, see mirroring instructions
for how to clone and mirror all data and code used for this inbox;
as well as URLs for NNTP newsgroup(s).
```
