---
title: 'I-have-ADHD: A skill to stop coding agents from burying the answer'
link: https://github.com/ayghri/i-have-adhd
source: hnrss-org-frontpage
published: 2026-09-08T14:13:26Z
updated: 2026-09-08T14:13:26Z
first_seen: 2026-09-08T20:11:39.387033692Z
authors:
- domhudson
summary: 'Article URL: https://github.com/ayghri/i-have-adhd Comments URL: https://news.ycombinator.com/item?id=49610631 Points: 180 # Comments: 149'
content: extracted
html: 2026-09-08-i-have-adhd-a-skill-to-stop-coding-agents-from-burying-the.html
preview:
  file: 2026-09-08-i-have-adhd-a-skill-to-stop-coding-agents-from-burying-the.preview-8d29117ecf02.webp
  width: 256
  height: 128
  alt: A skill to stop your coding agent from burying the answer. ADHD-friendly output. - ayghri/i-have-adhd
  color: '#eaedef'
images:
- source: https://opengraph.githubassets.com/1977459ee36859a275f1f9a231bdf89df8d72bebc245c6efcb30a9c5d674d3db/ayghri/i-have-adhd
  original:
    file: 2026-09-08-i-have-adhd-a-skill-to-stop-coding-agents-from-burying-the.image-0f578d8c18ec.png
    width: 1200
    height: 600
  variants:
  - file: 2026-09-08-i-have-adhd-a-skill-to-stop-coding-agents-from-burying-the.image-6f25ef10a800.webp
    width: 48
    height: 24
  - file: 2026-09-08-i-have-adhd-a-skill-to-stop-coding-agents-from-burying-the.image-3bf0ddd8ec87.webp
    width: 320
    height: 160
  - file: 2026-09-08-i-have-adhd-a-skill-to-stop-coding-agents-from-burying-the.image-7b6b694b5ff1.webp
    width: 640
    height: 320
  - file: 2026-09-08-i-have-adhd-a-skill-to-stop-coding-agents-from-burying-the.image-0c42b2080ec4.webp
    width: 960
    height: 480
  - file: 2026-09-08-i-have-adhd-a-skill-to-stop-coding-agents-from-burying-the.image-c4bd02739a71.webp
    width: 1200
    height: 600
  color: '#fefefe'
- source: https://github.com/ayghri/i-have-adhd/raw/main/logo.png
  original:
    file: 2026-09-08-i-have-adhd-a-skill-to-stop-coding-agents-from-burying-the.image-4ad687bc5cd7.png
    width: 256
    height: 256
  variants:
  - file: 2026-09-08-i-have-adhd-a-skill-to-stop-coding-agents-from-burying-the.image-e1242a39f5ae.webp
    width: 48
    height: 48
  - file: 2026-09-08-i-have-adhd-a-skill-to-stop-coding-agents-from-burying-the.image-92bb06d66493.webp
    width: 256
    height: 256
  color: '#fba7c7'
---

[![i-have-adhd](https://github.com/ayghri/i-have-adhd/raw/main/logo.png)](https://github.com/ayghri/i-have-adhd/blob/main/logo.png)

**ADHD-friendly outputs. No ADHD diagnosis needed!**

[![License](https://camo.githubusercontent.com/c52dae836dd56b750f0287b501f758eebf035a01fe7f7365260cafdf5bbcc66f/68747470733a2f2f696d672e736869656c64732e696f2f6769746875622f6c6963656e73652f6179676872692f692d686176652d616468643f7374796c653d666c6174)](https://github.com/ayghri/i-have-adhd/blob/main/LICENSE)

**🇬🇧** · [🇨🇳](https://github.com/ayghri/i-have-adhd/blob/main/.github/readme/README.zh-CN.md "简体中文") · [🇧🇷](https://github.com/ayghri/i-have-adhd/blob/main/.github/readme/README.pt-BR.md "Português (Brasil)") · [🇯🇵](https://github.com/ayghri/i-have-adhd/blob/main/.github/readme/README.ja.md "日本語") · [🇻🇳](https://github.com/ayghri/i-have-adhd/blob/main/.github/readme/README.vi.md "Tiếng Việt") · [🇰🇷](https://github.com/ayghri/i-have-adhd/blob/main/.github/readme/README.ko.md "한국어") · [🇹🇭](https://github.com/ayghri/i-have-adhd/blob/main/.github/readme/README.th.md "ภาษาไทย")

## Install

[](https://github.com/ayghri/i-have-adhd#install)

Copy/paste into your CLI prompt:

```
Install the i-have-adhd skill/plugin from https://github.com/ayghri/i-have-adhd, refer to the repo's AGENTS.md for instructions.
```

Or 🔗 [check the installation instructions](https://github.com/ayghri/i-have-adhd/blob/main/INSTALL.md).

## What it does

[](https://github.com/ayghri/i-have-adhd#what-it-does)

A skill for your coding assistant that stops it from burying the answer. Action first. Steps numbered. No "Hope this helps!"

## What changes

[](https://github.com/ayghri/i-have-adhd#what-changes)

## Before

[](https://github.com/ayghri/i-have-adhd#before)

> Great question! Let me think about this. Your auth flow has a few moving pieces: the middleware, the token verification, and the cookie handling. Looking at `src/auth.ts`, the `verifyToken` function (around lines 42-58) seems to be using an older `jsonwebtoken` API. One approach would be to update the package and rewrite that function. After making the change, you'd want to run the auth tests to confirm nothing breaks. By the way, you might also want to look at your dependency versions overall. Hope this helps! Let me know if you want to dig deeper.

## After

[](https://github.com/ayghri/i-have-adhd#after)

> Run `npm install jsonwebtoken@latest`, then edit `src/auth.ts:42`.
>
> 1. Open `src/auth.ts`
> 2. Replace `verifyToken` (lines 42–58) with the snippet below
> 3. Run `npm test -- auth.spec.ts`
>
> Next: paste the first failing line if any test fails.

## The rules

[](https://github.com/ayghri/i-have-adhd#the-rules)

10 rules. Full text in [SKILL.md](https://github.com/ayghri/i-have-adhd/blob/main/skills/i-have-adhd/SKILL.md).

1.  Lead with the next action.
2.  Number multi-step tasks.
3.  End with one concrete next step.
4.  Suppress tangents.
5.  Restate state every turn.
6.  Specific time estimates (minutes, not "a bit").
7.  Make wins visible.
8.  Matter-of-fact errors.
9.  Cap lists at 5 items.
10. No preamble. No recap. No closers.

## Tune it

[](https://github.com/ayghri/i-have-adhd#tune-it)

Fork, edit `skills/i-have-adhd/SKILL.md`, then swap your copy in:

```
claude plugin uninstall i-have-adhd            # drop the upstream copy first:
claude plugin marketplace remove i-have-adhd   # fork and upstream share both names
claude plugin marketplace add <your-username>/i-have-adhd
claude plugin install i-have-adhd@i-have-adhd
```

Restart Claude Code, then re-invoke `/i-have-adhd`.

## Credits

[](https://github.com/ayghri/i-have-adhd#credits)

Loosely based on *The Adult ADHD Tool Kit* by J. Russell Ramsay and Anthony L. Rostain. Adapted for how an LLM should respond, not how a human should organize their day.

## License

[](https://github.com/ayghri/i-have-adhd#license)

MIT.

Star ⭐ if it saved you one scroll past one "Great question!"
