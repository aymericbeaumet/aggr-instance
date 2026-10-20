---
title: patch the planet
link: https://ghuntley.com/patch/
source: ghuntley-com
published: 2026-10-20T16:41:12Z
updated: 2026-10-20T16:41:12Z
first_seen: 2026-10-20T17:55:58.869157456Z
authors:
- Geoffrey Huntley
labels:
- ai
- infosec
- nix
summary: I don't know if you've seen this, but a CA just got popped. Their keys got leaked, which means the industry as a whole now has some pretty major problems. It was revealed two hours ago. eMudhra Intermediate CA Compromise eMudhra Intermediate CA Compromise Now you&
content: extracted
html: 2026-10-20-patch-the-planet.html
remote_preview:
  url: https://storage.ghost.io/c/06/e5/06e5d12b-0328-469d-a0d0-3a0f0ba34c46/content/images/size/w1200/2026/10/Symbolic-traditional-tattoo-art-print-showing-hackers-patching-a-planet-beneath-stormy-skies--vibrant-yet-soft-pastel-colors--intricate-retro-ornamental-details--intense-dramatic-weather--high-tension-composition-balanced-by-a-gentle-soothi.jpg
---

I don't know if you've seen this, but a CA just got popped. Their keys got leaked, which means the industry as a whole now has some pretty major problems. It was revealed two hours ago.

[eMudhra Intermediate CA Compromise](https://emudhra.we-have-a-problem.com/?ref=ghuntley.com)

eMudhra Intermediate CA Compromise

![](https://static.ghost.org/v5.0.0/images/link-icon.svg)

Now you're probably wondering why I'm writing up this blog post. It's simple. This is perhaps one of the easiest security patches I've ever done, and I've discovered a few new tricks. First, understand that fixing this with ease without Nix is almost impossible. Tens of millions of dollars (if not more) in productivity are being lost worldwide right now as everyone tries to patch their transitive dependencies.

Meanwhile, because I've used Nix, I can patch the planet, since the ideas behind the blog post below extend into security as well.

[the world hasn’t figured out yet that you can literally just fix everything with a Nix overlay](https://ghuntley.com/nix/)

There are many things that are uncertain in our industry right now, but one thing I am certain about, and have been for almost 13 years, is that Nix is a terrible programming language. I remember when I first learned it, it felt like pushing shit uphill. The learning cliff

![](https://storage.ghost.io/c/06/e5/06e5d12b-0328-469d-a0d0-3a0f0ba34c46/content/images/icon/A-black-and-white--low-angle-digital-illustration-in-a-symbolic-traditional-tattoo-art-style.--A-bald--light-skinned-man-with-a-bushy-beard--prominent-eyebrows--and-a-friendly-expression-wears-denim-overalls--2--a11b26e3-d1b1-4800-9564-2a6ca417d866.jpg)Geoffrey Huntley Geoffrey Huntley

![](https://storage.ghost.io/c/06/e5/06e5d12b-0328-469d-a0d0-3a0f0ba34c46/content/images/thumbnail/Symbolic-traditional-tattoo-art-print-viewed-from-a-high--elevated-angle--portraying-a-bustling-city-transformed-by-a-Nix-overlay.-Gleaming-buildings--technological-textures--surreal-contrasts--and-limitless-possibility-evoke-hidden-power.--90d86506-96bf-47ba-8336-242084c55e95.jpg)

Because of Nix's single source of truth and the ability to patch this single source of truth, I'm able to issue a simple prompt like this:

> okay is my nixos for my fleet affected by [https://emudhra.we-have-a-problem.com/](https://emudhra.we-have-a-problem.com/?ref=ghuntley.com)?

With results in the following.

![](https://storage.ghost.io/c/06/e5/06e5d12b-0328-469d-a0d0-3a0f0ba34c46/content/images/2026/10/Screenshot-2026-10-21-at-3.30.50---AM.png)

So the problem here is the transitive dependencies. It's easy to patch one thing, but the transitive dependencies are the problem, and this is where Nix shines: one fix fixes the transitive dependencies automatically.

One patch fixes the entire fleet and its transitive dependencies. Patch, commit, and autodeploy your fleet 🍻

## the new trick

Alright, I promised a new trick. Here's the trick.

> You can prompt an agent to update the AGENTS.MD to automatically back out these patches, let's say in five days from now, just by using the AGENTS.MD as a reminder 😎

So it was one simple prompt to identify if I was affected, which turned into a plan, which then automatically fixed it, and now, in a couple of days, it will reassess the situation, give time for the ecosystem and dust to settle, and then automatically back it out once all the upstream transitive dependencies have been resolved.

Cool, huh?

```
## Temporary reminder: eMudhra root blacklist (from 2026-10-22)

On or after 2026-10-22, check whether the `nixos-26.05` channel contains nixpkgs
commit `7816d39` (the backported `security/ca.nix` distrust of the eMudhra roots), e.g.
`nix eval --raw --refresh --impure --expr '(builtins.getFlake "github:NixOS/nixpkgs/nixos-26.05").rev'`
then `git merge-base --is-ancestor`. If it does not, check again two days later.
Once it does, back out the fleet-wide workaround: update the `nixpkgs` pin to include
it, delete `nix/domains/system/public-trust.nix` (contract `fleet-distrust-emudhra-roots`)
and its imports in `nix/domains/system/catalog.nix`,
`nix/domains/artifacts/guest/configuration.nix` and the `host-kexec` modules in
`flake.nix`, and remove the eMudhra paragraph from
`marshall/priv/hldd/12-configuration.md`. Then verify that the built host and guest
bundles still lack the four roots, and delete this section. Keep Marshall's
system-store TLS defaults (`runtime-postgres-system-trust`, `mail-smtp-system-trust`)
and the `rustls-webpki` update; they are not part of the workaround.
```

I can't stress this enough. If you build something, you own it, including all the dependencies you rely on. We're entering weird times where cyber offensive models will be continuously on the attack.

The game has changed. To defend, you need more inferencing capability than the attacker, and you also need to respond rapidly.

This is by far the easiest security patch I've ever done, and the best part is I won't carry this tech debt for long because an agent will automatically back it out in a couple of days.

0:00

/0:04

patch the planet

![](https://storage.ghost.io/c/06/e5/06e5d12b-0328-469d-a0d0-3a0f0ba34c46/content/media/2026/10/hackers-patch-the-planet-dialogue--1-_thumb.jpg)

ps. socials

> 🗞️ patch the planet [https://t.co/7ZwlallBru](https://t.co/7ZwlallBru?ref=ghuntley.com) [https://t.co/aSQGD0smx2](https://t.co/aSQGD0smx2?ref=ghuntley.com) [pic.twitter.com/tDpIKpp58O](https://t.co/tDpIKpp58O?ref=ghuntley.com)
>
> — geoff (@GeoffreyHuntley) [October 20, 2026](https://x.com/GeoffreyHuntley/status/2112585326514287070?ref_src=twsrc%5Etfw&ref=ghuntley.com)
