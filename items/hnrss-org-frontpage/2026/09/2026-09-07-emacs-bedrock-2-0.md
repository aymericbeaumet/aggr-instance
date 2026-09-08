---
title: Emacs Bedrock 2.0
link: https://lambdaland.org/posts/2026-09-06-bedrock-v2/
source: hnrss-org-frontpage
published: 2026-09-07T20:12:12Z
updated: 2026-09-07T20:12:12Z
first_seen: 2026-09-08T09:02:26.443710107Z
authors:
- ashton314
summary: 'Article URL: https://lambdaland.org/posts/2026-09-06-bedrock-v2/ Comments URL: https://news.ycombinator.com/item?id=49602490 Points: 109 # Comments: 13'
content: extracted
html: 2026-09-07-emacs-bedrock-2-0.html
---

[Bedrock](https://codeberg.org/ashton314/emacs-bedrock/) is a minimal, human-crafted starter-kit for Emacs, and today I’m pleased to announce that version 2.0 of Bedrock is available! This release requires Emacs 31, which came out in August 2026. If you are unable to upgrade Emacs that high, you should be able to remove some of the Emacs 31-specific config without too much trouble.

Bedrock is essentially a collection of *better defaults* for Emacs, and it uses *no third-party packages* in its base configuration. Part of the appeal of Emacs, of course, is the vast set of packages developed and maintained by the community, so Bedrock includes sample configurations for some of the most popular packages that I, personally, find utterly indispensable.

The idea behind Bedrock is a lot simpler than other “starter kits” or configuration frameworks: Bedrock is just an `early-init.el` and an `init.el` that you copy once into `~/.emacs.d/`, and then you modify those files directly as your needs grow and your experience with Emacs matures. The files contain enough comments to point you in the right direction when you want to dig into how something works, but not so much as to drown you in a sea of prose when you’re looking for code. The configuration for third-party packages lives in a separate `extras/` folder, and you can copy individual packages’ configuration directly into `init.el` as-needed, or import groups of packages an entire file at a time.

## Changes in Bedrock 2.0

This is a major release just because Bedrock is breaking its compatibility with Emacs 29, last released in 2024. Here is a summary of major changes in this version:

- Bedrock now uses the `lexical-binding: t` cookie in all its `.el` files.
- All packages declared with `use-package` get installed if not present on the system *by default*.
- Use `embark-auto-prefix-help-mode` instead of `which-key` if the `embark` package is loaded.
- Remove `wgrep` in favor of built-in `grep-change-to-grep-edit-mode`.
- Vastly improved `tree-sitter` configuration.
- Nicer `isearch` configuration.
- Make the built-in `*Completions*` buffer auto-update.
- Turn on `repeat-mode` by default.

Fixes in this version:

- Fixed theme colors not being respected.
- Better suppression of startup message.
- Miscellaneous little improvements.

These lists are not exhaustive (as that last bullet should make clear) but that’s the gist of it. More details are available in [the changelog](https://codeberg.org/ashton314/emacs-bedrock#headline-22).

This is very much an incremental improvement—not some total-overhaul of Bedrock. If you’re a happy user of an earlier version of Bedrock, then you might not have any reason to “upgrade”. Emacs is a stable editor (though the level of active development it enjoys is both meaningful and exciting!) and it should come as no surprise that a starter kit made for the “bleeding edge” should be, well, relatively stable itself.

## Upgrading from earlier versions of Bedrock

Bedrock does not support any sort of automatic upgrade path, as that would require the Bedrock code to live somewhere other than the `init.el` file, and the point of Bedrock is that *you* learn to own your entire config as quickly as possible. Instead, you should compare the commit you cloned/copied your version of Bedrock from with the new `main` branch and pick what you want to apply.

## Thank you

I appreciate all those who have tried out Bedrock and have sent me their feedback. I made Bedrock initially as an exercise in tweaking the builtin options to get something that felt more “usable” to me. From there it turned into a configuration I could give to my friends who expressed some curiosity with Emacs. Now it’s something that way more people than I initially imaged use as a starting point for their Emacs configurations. (I’ll never know how many because no telemetry—yay privacy!) Anyway, a *huge* thank you to those who’ve submitted bug reports or PRs, and to the wider Emacs community as well! Thanks you *your* dedication to free and open-source software, you’ve made my life better. I hope Bedrock improves your life as well.

— Ashton
