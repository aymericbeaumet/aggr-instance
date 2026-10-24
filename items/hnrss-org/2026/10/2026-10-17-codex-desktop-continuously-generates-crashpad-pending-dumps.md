---
title: Codex Desktop continuously generates Crashpad pending dumps
link: https://github.com/openai/codex/issues/25921
source: hnrss-org
published: 2026-10-17T17:45:08Z
updated: 2026-10-17T17:45:08Z
first_seen: 2026-10-18T06:32:49.699637230Z
authors:
- AIorNot
content: extracted
html: 2026-10-17-codex-desktop-continuously-generates-crashpad-pending-dumps.html
extra:
  comments_url: https://news.ycombinator.com/item?id=50133231
  num_comments: 42
  points: 117
---

## Activity

1.
2.
3.
4.
5.
6.  changed the title \[-\]Codex Desktop continuously generates Crashpad pending dumps, growing to ~5GB\[/-\] \[+\]Codex Desktop continuously generates Crashpad pending dumps, growing without any limit: +5GB per day at least\[/+\]

    [on Jun 9, 2026](https://github.com/openai/codex/issues/25921#event-26526800169)

7.
8.
9.
10.
11.
12.
13.
14.
15.
16.
17.
18.
19.
20.
21.
22.
23.
24.
25.
26. Still happening on the current build, and at a much larger scale. Measured 2026-10-15 to 2026-10-17:

    **Environment**

    - ChatGPT.app `26.1007.71326` (build `20398`), Codex Framework (Chromium) `155.0.8059.40`
    - macOS `15.7.4`, Apple Silicon (arm64)

    **What I found**

    | Folder                                                                          | Size         | Files              |
    | ------------------------------------------------------------------------------- | ------------ | ------------------ |
    | `~/Library/Application Support/com.openai.codex/web/Crashpad`                   | 180 GB       | 2,251,765          |
    | `~/Library/Application Support/Codex/Crashpad` (almost all of it in `pending/`) | 143 GB       | 1,785,095          |
    | **Total**                                                                       | **\~323 GB** | **\~4.04 million** |

    That was about 35% of a 926 GB disk. Free space fell to 1.4 GB before I found it. Nothing in the app warned me. I only found it by running `du` over `~/Library`.

    The app kept working the whole time, as others have reported here. So the dumps build up silently in the background with no limit.

    **Cleanup**

    With the app still running, I renamed both `Crashpad` folders and deleted the renamed copies. Free space went from 1.4 GB to 50+ GB within minutes, and the delete is still running (millions of small files).

    **Asks**

    1. Put a size or count cap on `Crashpad/pending`, with automatic pruning (see also [Add storage limits and automatic cleanup for Codex-generated data #51361](https://github.com/openai/codex/issues/51361)).
    2. Stop the handler crash loop that produces the dumps ([\[app\]\[macOS\]\[26.727.51351\] browser\_crashpad\_handler SIGTRAPs 2ms after launch, launchd respawns it, Crashpad/pending fills disk at ~17GB/day #36982](https://github.com/openai/codex/issues/36982)).
    3. Show a warning in the app when its own data goes over a few GB.

    Related: [#36982](https://github.com/openai/codex/issues/36982), [#39875](https://github.com/openai/codex/issues/39875), [#51739](https://github.com/openai/codex/issues/51739), [#52194](https://github.com/openai/codex/issues/52194), [#51361](https://github.com/openai/codex/issues/51361).

27.
28.
