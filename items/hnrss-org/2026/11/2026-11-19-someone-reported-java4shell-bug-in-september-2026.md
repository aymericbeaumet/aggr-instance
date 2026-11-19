---
title: Someone reported java4shell bug in September 2026
link: https://github.com/openjdk/jdk/pull/32781
source: hnrss-org
published: 2026-11-19T11:47:03Z
updated: 2026-11-19T11:47:03Z
first_seen: 2026-11-19T17:16:58.860737333Z
authors:
- notmine1337
content: extracted
html: 2026-11-19-someone-reported-java4shell-bug-in-september-2026.html
remote_preview:
  url: https://opengraph.githubassets.com/135519046b46ca3a3a2d090f275de4de1d05df4e79fb1c077da71ec4f7f24286/openjdk/jdk/pull/32781
  alt: In StubGenerator::generate_base64_decodeBlock() (x86_64 AVX-512 VBMI path), L_finalBit handles 1-63 leftover bytes with a masked vector sequence that computes output_size = (len/4)*3. Valid inputs ...
---

In `StubGenerator::generate_base64_decodeBlock()` (x86\_64 AVX-512 VBMI path), `L_finalBit` handles 1-63 leftover bytes with a masked vector sequence that computes `output_size = (len/4)*3`. Valid inputs always have a length multiple of 4, so this bug triggers only on invalid inputs: with `length < 4` (e.g. `"A".repeat(64)+"="` leaves 1 byte after `L_process64Loop`), `output_size` starts at 0 and the padding path decrements it below zero, producing a wrong `output_mask`. The stub stores past the logical output length before returning to Java, corrupting the heap.

The suggested fix is to early-return to Java if `length < 4`.

* * *

- I confirm that I make this contribution in accordance with the [OpenJDK Interim AI Policy](https://openjdk.org/legal/ai).

* * *

### Progress

- Change must not contain extraneous whitespace
- Commit message must refer to an issue
- Change must be properly reviewed (2 reviews required, with at least 1 [Reviewer](https://openjdk.org/bylaws#reviewer), 1 [Author](https://openjdk.org/bylaws#author))

### Integration blocker

 ⚠️ Failed to retrieve information on issue `8392044`. Please make sure it exists and is accessible.

### Issue

- ⚠️ Failed to retrieve information on issue `8392044`.

### Reviewing

Using `git`

Checkout this PR locally: \
 `$ git fetch https://git.openjdk.org/jdk.git pull/32781/head:pull/32781` \
 `$ git checkout pull/32781`

Update a local copy of the PR: \
 `$ git checkout pull/32781` \
 `$ git pull https://git.openjdk.org/jdk.git pull/32781/head`

Using Skara CLI tools

Checkout this PR locally: \
 `$ git pr checkout 32781`

View PR using the GUI difftool: \
 `$ git pr show -t 32781`

Using diff file

Download this PR as a diff file: \
 [https://git.openjdk.org/jdk/pull/32781.diff](https://git.openjdk.org/jdk/pull/32781.diff)
