---
title: Wine ported to OpenBSD
link: https://marc.info/?l=openbsd-ports&m=179236125692175
source: hnrss-org
published: 2026-10-19T13:35:34Z
updated: 2026-10-19T13:35:34Z
first_seen: 2026-10-19T19:14:52.283302801Z
authors:
- iamnothere
content: extracted
html: 2026-10-19-wine-ported-to-openbsd.html
---

```

List:       openbsd-ports
Subject:    emulators/wine
From:       Thomas Frohwein <tfrohwein () fastmail ! com>
Date:       2026-10-18 22:08:43
Message-ID: 20261018150843.271e0c06 () babcom6 ! home ! arpa
[Download RAW message or body]


Hello,

In what might the most surprising port proposal of the year, I'm proud
to present a new port for wine. This has been a collective effort,
after some of the prodding at the %gs/gsbase question (see thread [1]).
The gates swung open with Andrii's software solution to the %gs issue -
explanation to me: "can't set gsbase, so we are emulating %gs prefix on
commands". Overall, the heavy lifting was done by Andrii and Fabien - kudos.

Turns out there is a ground-swell with interest in wine [2], different
than even 5-10 years ago because a lot of Windows software has since
moved to being 64bit.

We have since tested several dozens of 64-bit Windows software, mostly
successfully. See screenshots at [3][4][5], livestream recordings at
[6] for example. Even the Remaster of The Witcher 3 which released just
last month runs, and runs well (requires adding in vkd3d-proton; see
README; livestream at [7] which looks less fluent than it is due to
the on-system recording with ffmpeg).

Biggest limitation is that WoW64 doesn't work, so 32bit Windows
software is still a no-op. You can check .exe/.dll easily with file(1)
- if it says something like "MS-DOS executable PE for MS Windows (GUI)
Intel 80386 32-bit", then it's 32-bit.

The builtin-tests are disabled for now. Enabling them is pretty
involved as they need access to the X server and multiple tests still
hang, however that was not observed in the real life testing with a
good number of games. This is still being worked on.

For those who wish to test it, I suggest Notepad++ as a small, fairly
straightforward piece of 64-bit Windows software [8]. Note that this
is (as always) run-at-your-own-risk software (see [9]). Consider
setting up a separate, non-privileged user for this until we can
explore if some fencing in, potentially with unveil(2) can be done.

Appreciate testing of the port draft and contributions by: brynet@,
bentley@, pertho (on irc.libera.chat #openbsd-gaming), vl_ms

ok?

[1] https://marc.info/?l=openbsd-tech&m=179067466617674&w=2
[2] https://marc.info/?l=openbsd-tech&m=179070170845285&w=2
[3]
https://old.reddit.com/r/openbsd_gaming/comments/1x4e7fi/some_samples_from_wip_wine_port/
 [4]
https://old.reddit.com/r/openbsd_gaming/comments/1x5yxpu/more_wineonopenbsd_screenshots/
 [5]
https://old.reddit.com/r/openbsd_gaming/comments/1x7z2ry/even_more_wineonopenbsd_proof/
 [6] https://video.gamerstavern.online/w/8tjjFRDtGJQZwxfE4ojVoC
[7] https://video.gamerstavern.online/w/oZcTRKL5JMiU4M4FqGKcNH
[8]
https://github.com/notepad-plus-plus/notepad-plus-plus/releases/download/v8.9.8.1/npp.8.9.8.1.portable.x64.zip
 [9]
https://gitlab.winehq.org/wine/wine/-/wikis/FAQ#is-wine-malware-compatible


[Attachment #3 (application/x-compressed-tar)]
```
