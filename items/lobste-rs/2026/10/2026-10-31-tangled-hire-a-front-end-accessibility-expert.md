---
title: Tangled, hire a front-end accessibility expert
link: https://dbushell.com/2026/11/01/tangled-accessibility/
source: lobste-rs
published: 2026-10-31T16:29:45Z
updated: 2026-10-31T16:29:45Z
first_seen: 2026-11-06T11:27:16.888185679Z
authors:
- dbushell.com via diktomat
labels:
- a11y
- rant
- vcs
summary: A response to https://lobste.rs/s/ukj8mm/federated_code_forge Comments
content: extracted
html: 2026-10-31-tangled-hire-a-front-end-accessibility-expert.html
remote_preview:
  url: https://dbushell.com/images/articles/2026-11-01-tangled-accessibility.png
---

Not me, this is just friendly advice from a struggling user.

Alongside [Codeberg](https://codeberg.org/) I’d consider [Tangled](https://tangled.org/) to be a real GitHub replacement. [We desperately need to leave GitHub behind.](https://dbushell.com/2026/04/29/github-is-sinking/) It’s difficult to recommend any tech these days. I used to spread the gospel of Proton and [that backfired](https://dbushell.com/notes/2026-10-30T07:29Z/). From what I’ve seen, Tangled seem to be saying and doing the right things so far. I have trepidations about their [venture capital funding](https://tangled.org/blog/seed/) (see [Deno’s demise](https://dbushell.com/notes/2026-10-10T10:30Z/)) but pessimism is no fun.

Tangled’s recent blog post on [federated code forges](https://tangled.org/blog/forge/) piqued my interest. [I self-host Forgejo](https://dbushell.com/2025/08/15/self-hosted-forgejo-actions-runner/) and a big downside is that no one else can participate easily. [Forgejo federation](https://forgejo.codeberg.page/faq/#what-about-forge-federation) is under development. Maybe the two can co-exist? But that’s not what I’m here to discuss.

## Tangled’s new look

In the same post Tangled revealed their new UI design.

> We’ve redone the UI on top of our new architecture (described further below), in Svelte. Things should now be quicker, visually consistent and prettier! The keen eyed of you might’ve also spotted that we’ve swapped out Inter for Timeless Sans.
>
> [The federated code forge](https://tangled.org/blog/forge/#a-fresh-look) - Anirudh (Tangled CEO)

I quiped on Bluesky: [“keen eyed? eagle-eyed maybe!”](https://bsky.app/profile/did:plc:rgrzo7xept2yb2auooex4rlg/post/3mz5zpy3yus2j) — the new font size is exceedingly tiny. It’s coded as `12px` with `16px` line height. Tangled is practically unusable for me at this size. It was small before but I could zoom in. The way the new front-end is designed and built doesn’t leave much room for personal remedy. This is not just a subjective opinion there are real accessibility failures here.

*(Edit: to be clear, below is one example of many issues.)*

## Font failure

Below is a screenshot from Tangled’s new user interface design.

![Tangled news feed panel titled 'Announcements'. It has three items that each include a date, headline, and short description.](https://dbushell.com/images/blog/2026/tangled-12.png)

Tangled news feed panel titled 'Announcements'. It has three items that each include a date, headline, and short description.

Visit [`tangled.org`](https://tangled.org/timeline) and experience the new UI yourself.

12 pixels is too small for any meaningful text (which is typically all text). Maybe you feel the size is totally fine? That’s fine for you. But it’s not fine for a *significant* number of people. You won’t find any accessibility expert happy with that size. (Low-DPI displays may even struggle to render the font legibly.)

[WCAG 2.2 accessibility guidelines](https://www.w3.org/WAI/WCAG22/Understanding/) don’t recommend a minimum font size. [1.4.4 Resize Text](https://www.w3.org/TR/WCAG22/#resize-text) does require that text can be resized up to 200% “without loss of content or functionality”.

Tangled uses pixel units in CSS for font size†. That means the UI won’t adapt to the browser default setting. [I can *force* a minimum](https://support.mozilla.org/en-US/kb/font-size-and-zoom-increase-size-of-web-pages#w_set-a-minimum-text-size) `16px` font size and look what happens.

**† Correction:** I was wrong about pixel units everywhere (I misread the computed values), it’s `font-size: 16px` on the `<html>` element that blocks the relative units from scaling properly with browser settings [(an easier fix)](https://adrianroselli.com/2024/03/the-ultimate-ideal-bestest-base-font-size-that-everyone-is-keeping-a-secret-especially-chet.html).

![Tangled's news panel with 16px font size, described below.](https://dbushell.com/images/blog/2026/tangled-16.png)

Tangled's news panel with 16px font size, described below.

Text lines get too close for comfort. Characters are starting to get clipped. See the “g” descender in “Tangled” for example.

And if I force a 200% resize at `24px` this happens.

![Tangled's news panel with 24px font size. Text is truncated and characters are heavily clipped.](https://dbushell.com/images/blog/2026/tangled-24.png)

Tangled's news panel with 24px font size. Text is truncated and characters are heavily clipped.

Oh dear! Text that is [“clipped, truncated or obscured”](https://www.w3.org/WAI/WCAG22/Techniques/failures/F69) is a failure.

Remember that the WCAG guidelines are a floor. Nothing can be perfectly accessible, but a `12px` font size and basic front-end failures are a concern. A proper audit would reveal other issues (e.g. with how media/container queries adapt to font size).

## Expect better

I’m not posting this to seek employment myself or to provide answers. I want to remind readers that accessibility concerns affect everyone. Serious projects should seek professional expertise. It’s disappointing to see yet another tech start-up fail the basics.

I’ll provide two suggestions for free though:

1. Use relative units for font sizes
2. Use unit-less numbers for line height

*(Edit: see correction above, the fix is even easier.)*

Frankly, what Tangled’s redesign suggests to me is that nobody involved with the process gave accessibility enough thought (if any) *(edit: that part was unfair, there was some effort but far from adequate testing)*. That may sound harsh but it’s usually the reality from my experience. Whatever the case, Tangled must do better in this area.

If I may answer a few predictable but hypothetical comments.

> This is just v1. Things will improve.

I’m sure they will and maybe even pushed by my criticism. The struggle is that accessibility concerns must be integral to design from day one, not an afterthought to patch.

> It’s open source, where’s your merge request?

Sorry, once a project crosses the line into venture capital “open source” takes on a different meaning. If people want to contribute that’s their prerogative. Personally, I’m not in the habit of giving away professional expertise for free to a for-profit business. Such expectations are especially a problem for accessibility specialists (of which I am not).

> Aha! What about this accessibility failure on your website?!

That’s a bit embarrassing, but thanks for the heads-up!

It’s far easier to hire an accessibility expert before you ship but it’s never too late to retroactively make improvements. I’ve been making accessibility improvements on my own website for decades.

I like Tangled and I want to see it do well. Maybe someone will share a [milkshake duck](https://en.wikipedia.org/wiki/Milkshake_Duck) or they donate to [fascist Linux](https://brennan.day/normalized-fascism-in-open-source-12-million-given-to-dhh/) and I’ll eat my words. I really hope not! I hope Tangled improves so that those lingering in GitHub’s slop graveyard have fewer excuses to hang around.
