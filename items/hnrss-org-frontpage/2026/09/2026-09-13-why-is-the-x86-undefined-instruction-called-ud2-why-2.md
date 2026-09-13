---
title: Why is the x86 undefined instruction called ud2? Why 2?
link: https://devblogs.microsoft.com/oldnewthing/20260910-00/?p=112689
source: hnrss-org-frontpage
published: 2026-09-13T12:30:15Z
updated: 2026-09-13T12:30:15Z
first_seen: 2026-09-13T19:20:48.145884900Z
authors:
- ibobev
summary: 'Article URL: https://devblogs.microsoft.com/oldnewthing/20260910-00/?p=112689 Comments URL: https://news.ycombinator.com/item?id=49683262 Points: 122 # Comments: 32'
content: extracted
html: 2026-09-13-why-is-the-x86-undefined-instruction-called-ud2-why-2.html
preview:
  file: 2026-09-13-why-is-the-x86-undefined-instruction-called-ud2-why-2.preview-f2ac05282b2e.webp
  width: 110
  height: 145
  color: '#5e5755'
images:
- source: https://devblogs.microsoft.com/oldnewthing/wp-content/uploads/sites/38/2019/02/ShowCover.jpg
  original:
    file: 2026-09-13-why-is-the-x86-undefined-instruction-called-ud2-why-2.image-566f29379af9.jpg
    width: 110
    height: 145
  variants:
  - file: 2026-09-13-why-is-the-x86-undefined-instruction-called-ud2-why-2.image-b175d96d6271.webp
    width: 48
    height: 63
  color: '#030202'
- source: https://devblogs.microsoft.com/oldnewthing/wp-content/uploads/sites/38/2019/02/RaymondChen_5in-150x150.jpg
  original:
    file: 2026-09-13-why-is-the-x86-undefined-instruction-called-ud2-why-2.image-19dbb31aeed3.jpg
    width: 150
    height: 150
  variants:
  - file: 2026-09-13-why-is-the-x86-undefined-instruction-called-ud2-why-2.image-c4c4aff94af1.webp
    width: 48
    height: 48
  color: '#66798b'
---

If you look at x86 compiler output (or if, like me, you’re looking at a crash caused by some software that tried to detour an API), you may see an instruction `ud2`. What’s up with that?

The `ud2` instruction is an architecturally undefined instruction, guaranteed to raise an “invalid opcode” exception. Some compilers generate it to mark “unreachable” code, so that if execution somehow manages to reach it, you get a crash rather than executing random instructions. For example, if a function marked `[[noreturn]]` somehow returns, the compiler will put a `ud2` after the call so that the program crashes instead of falling through to the next function.

Anyway, why is this instruction called `ud2` instead of just `ud`? Was there a `ud1`? What was so wrong about `ud1` that we had to make a `ud2`?

I think I can reconstruct what happened.

Originally, there was no architecturally undefined instruction on x86. So people who wanted to force an invalid opcode exception went looking for some byte sequence that reliably raised the invalid opcode exception when executed.

Somebody found that the `0F FF` sequence led to an invalid opcode exception. Though, for whatever reason, the instruction internally decoded as if it took two parameters, a register destination and a register-or-memory source. The parameters aren’t actually used because the invalid opcode exception gets raised before anything else can happen.

Meanwhile, somebody else found that the `0F B9` sequence also had the same properties. So you now had two factions, the `0F FF` believers and the `0F B9` adherents. There really wasn’t much of a battle between them, because both techniques seemed to work, and it’s not like one was coming at the detriment of the other.

Intel then worked on their next processor, and maybe they made some changes that resulted in `0F FF` no longer raising an invalid opcode exception. Maybe they tried introducing a new instruction that uses `0F FF`. Or maybe it was still undefined but just performed some random operation instead of raising the invalid opcode instruction. And when they started running software on their new processor, they found that some programs stopped working, and after laborious investigation, they discovered that the programs were relying on `0F FF` being an invalid opcode.

In other words, they ran into [Hyrum’s Law](https://www.hyrumslaw.com/): With a sufficient number of users, all observable behaviors will be depended upon by somebody. [Obligatory XKCD](https://xkcd.com/1172/).

A similar discovery was made with `0F B9`.

Now that they realized that people wanted a reliable way to trigger an invalid opcode exception, the folks at Intel decided to make it official, and they created an actual supported permanently-invalid instruction and called it `ud2`.

It’s called `ud2` because the `0F FF` variant was retroactively named `ud0`, and the `0F B9` variant was retroactively named `ud1`, leaving `ud2` as the recommended undefined opcode.

One advantage of `ud2` is that it is a two-byte instruction with no parameters, so you don’t have to deal with the random decoded-but-unused source and destinations.

**Bonus chatter**: But why do we care about the unused parameters to `ud0` and `ud1`? Can’t we just say that `ud0` and `ud1` are also two-byte invalid opcodes? I mean, sure, there’s a third byte, or possibly more if the memory operand has an offset or a scaled index, but the processor doesn’t use it.

It matters, because even though the processor doesn’t use it, it still *decodes* it. And if the decoding of the instruction crosses into a not-present page, you don’t get an invalid opcode exception at all. You get an access violation.

**Bonus bonus chatter**: Except that some older processors raised the invalid opcode instruction as soon as they decoded the `0F FF` without checking whether the rest of the instruction decoded properly. So if your `0F FF` is at the end of a page, and the next page is not present, you sometimes got an invalid opcode exception and you sometimes got an access violation.

Better to stick with `ud2`. Its behavior is consistent and architecturally guaranteed.

## Author

![Raymond Chen](https://devblogs.microsoft.com/oldnewthing/wp-content/uploads/sites/38/2019/02/RaymondChen_5in-150x150.jpg)

Raymond has been involved in the evolution of Windows for more than 30 years. In 2003, he began a Web site known as The Old New Thing which has grown in popularity far beyond his wildest imagination, a development which still gives him the heebie-jeebies. The Web site spawned a book, coincidentally also titled The Old New Thing (Addison Wesley 2007). He occasionally appears on the Windows Dev Docs Twitter account to tell stories which convey no useful information.
