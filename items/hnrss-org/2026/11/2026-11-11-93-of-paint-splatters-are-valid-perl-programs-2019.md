---
title: 93% of paint splatters are valid Perl programs (2019)
link: https://www.mcmillen.dev/sigbovik/
source: hnrss-org
published: 2026-11-11T22:34:23Z
updated: 2026-11-11T22:34:23Z
first_seen: 2026-11-12T02:51:23.435329786Z
authors:
- zenincognito
content: extracted
html: 2026-11-11-93-of-paint-splatters-are-valid-perl-programs-2019.html
remote_preview:
  url: https://www.mcmillen.dev/img/rss.svg
  alt: Atom feed
---

TLDR: [read the paper](https://www.mcmillen.dev/sigbovik/2019.pdf) and [view the gallery of pretty Perl programs](https://www.mcmillen.dev/sigbovik/splatters.html).

In this paper, we aim to answer a long-standing open problem in the programming languages community: *is it possible to smear paint on the wall without creating valid Perl?*

We answer this question in the affirmative: it **is possible** to smear paint on the wall without creating a valid Perl program. We employ an empirical approach, using optical character recognition (OCR) software, which finds that merely 93% of paint splatters parse as valid Perl. We analyze the properties of paint-splatter Perl programs, and present seven examples of paint splatters which are not valid Perl programs.

> ![Screenshot of a Twitter conversation. Adrienne Porter Felt says: \"I don't want to teach my kid to code. I want him to splash in muddy puddles and smear paint on the walls and read novels under the covers way too late at night. I grew up too soon and wish I'd had more time to be a kid. Why do schools teach vocational skills so young these days?\" Jake Archibald replies: \"but is it possible to smear paint on the wall without creating valid Perl?\"](https://www.mcmillen.dev/media/20190401-sigbovik-tweet.png)
>
> [@jaffathecake on X](https://x.com/jaffathecake/status/1095706032448393217)

Accepted for publication at SIGBOVIK 2019, held April 1st 2019 in Pittsburgh. Winner of a Unwitting Participation Ribbon, “an unwelcome brand we’ve affixed to each paper determined after careful scrutiny to have included a genuine artifact, thereby furthering the admirable causes of open science and fruitful procrastination.”

Read it on [Google Docs](https://docs.google.com/document/d/1ZGGNMfmfpWB-DzWS3Jr-YLcRNRjhp3FKS6v0KELxXK8/preview) or download a [PDF](https://www.mcmillen.dev/sigbovik/2019.pdf). Or grab the [entire SIGBOVIK 2019 proceedings](http://sigbovik.org/2019/proceedings.pdf); I’m on page 174.

## Supplementary Materials

Here’s [all the paint splatters](https://www.mcmillen.dev/sigbovik/splatters.html) on a single page, along with the valid Perl source code corresponding to each. “Not valid” is written in red for those images which did not parse as valid Perl programs. If different OCR settings recognized multiple valid Perl programs, I chose the one that seemed the most “interesting”, according to my own aesthetic sense.

Here’s a [tarball of 100 paint-splatter images](https://www.mcmillen.dev/sigbovik/splatters.tar.gz) that were used as the main dataset for this paper.

**(source code not available yet because i am bad at GitHub)**

## Errata

There are a few paint splatter Perl programs that I didn’t recognize as “interesting” until after the SIGBOVIK submission deadline. For example, this splatter is recognized by OCR as the string `lerzfijglpFiji-j`, which evaluates to the number `0` in Perl:

![paint splatter](https://www.mcmillen.dev/sigbovik/splatters/6b78f8696b05f9322b2dda21b6932776.jpg)

The image below is recognized as the string `-*?`, which also evaluates to the number `0` in Perl:

![paint splatter](https://www.mcmillen.dev/sigbovik/splatters/e47b8463b359906947c66ec4c852a2a3.jpg)

Another surprising program is shown below; OCR recognizes this image as the string `;i;c;;#\\?z{;?;;fn':.;`, which evaluates to the string `c` in Perl:

![paint splatter](https://www.mcmillen.dev/sigbovik/splatters/803dd5a54c42ed93462c78ad7da357b0.jpg)

Finally, this image is recognized as the string `;E,'__'`, which evaluates to the string `E__` in Perl:

![paint splatter](https://www.mcmillen.dev/sigbovik/splatters/dc86c1c3553705b7b2f973d5be9e0389.jpg)
