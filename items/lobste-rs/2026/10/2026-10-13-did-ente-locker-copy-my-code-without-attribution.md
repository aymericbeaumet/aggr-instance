---
title: Did Ente Locker copy my code without attribution?
link: https://fairscan.org/blog/did-ente-locker-copy-my-code-without-attribution/
source: lobste-rs
published: 2026-10-13T13:44:47Z
updated: 2026-10-13T13:44:47Z
first_seen: 2026-10-14T01:50:10.887121900Z
authors:
- fairscan.org via tusharhero
labels:
- law
summary: Comments
content: extracted
html: 2026-10-13-did-ente-locker-copy-my-code-without-attribution.html
---

The last blog post I wrote explained [why I chose to make FairScan open source](https://fairscan.org/blog/open-source-and-gpl/). In particular, I said I knew there was a risk when I published FairScan's code and I was ready to accept it. I didn't expect to face that reality so soon.

## What happened to me over the past few weeks

I had never heard of Ente before September 16. On that day, I received an email from one of their engineers. The message thanked me for FairScan and announced that the Ente Locker app now used FairScan's segmentation model as part of a new document scanning feature. It referred to a blog post that credited FairScan's model and claimed that [Ente "wrote the image processing around it from scratch"](https://fairscan.org/blog/did-ente-locker-copy-my-code-without-attribution/from-scratch.webp).

I had put significant effort into FairScan's image processing so I was curious to see how Ente solved the same problems. What I found is that multiple image-processing implementations appeared to be almost direct translations of FairScan's implementations from Kotlin to Rust.

I replied by email on September 17. The engineer quickly answered that their blog post "could have been better phrased", and that it was reworded. I could see that [the new version](https://ente.com/blog/locker-document-scanner/) replaced the "from scratch" claim with something rather vague and still insisted on the segmentation model.

The same day, I sent a list of links to compare FairScan and Ente's code. I mentioned that I couldn't find any attribution to FairScan in the Ente repository.

On September 21, I received a reply from the same engineer that those were "fairly standard image-processing methods" and that the code had been subsequently fully rewritten. In their repository, I found [a pull request affecting more than 6,000 lines](https://github.com/ente/ente/pull/13053). Of course, it cannot change the code that was distributed in Locker 1.0.9.

On the same day, I sent an email to [copyright@ente.com](https://ente.com/contact/) to ask for the company's view on this. As of October 12, I still haven't received an answer.

On September 29, I published [a post](https://mastodon.social/@pynicolas/117352788210310140) on Mastodon linking to [a GitHub gist where I documented this timeline as well as code similarities](https://gist.github.com/pynicolas/e018ef84464a2cf7949fa453e41cfead).

## Standard methods or direct translation?

Feel free to have a look at the [code comparisons](https://gist.github.com/pynicolas/e018ef84464a2cf7949fa453e41cfead#code-comparison) in the gist linked above and form your own opinion, even if you're not a developer. You can use the "split view" of your browser for side-by-side comparisons. Here's an example:

[![](https://fairscan.org/blog/did-ente-locker-copy-my-code-without-attribution/comparison.webp)](https://fairscan.org/blog/did-ente-locker-copy-my-code-without-attribution/comparison.webp)

In the implementations I compared, I found the following similarities:

- The same overall structure
- The same algorithms
- The same names for functions and variables
- The same hard-coded values

To me, it looks like an almost direct translation from Kotlin to Rust. There are differences but they're not easy to spot.

Of course, I have a special position about this code. I wrote it, so I know where it comes from. I know it's the result of a lot of effort:

- I researched existing techniques like multi-scale Retinex, single view metrology, or gray world white balance.
- I assembled [an image pipeline](https://fairscan.org/blog/the-heart-of-fairscan-image-processing-pipeline-explained/) that fits the segmentation model.
- I started with some simple techniques (such as Douglas Peucker) that I later replaced.
- I developed [my own algorithm to approximate a quad based on contour orientation](https://fairscan.org/blog/where-to-cut-automatic-document-cropping/) (which is probably not optimal).
- I had to make choices about [what a scan should look like](https://fairscan.org/blog/what-s-a-good-scan/): not all apps make the same choices.
- I tested the behavior of my implementations and their parameter values [on hundreds of images](https://fairscan.org/blog/color-detection-data-driven/).
- I fixed many bugs in my implementations.
- I wrote blog posts about some of the numerous iterations I made, illustrating my trial and error process.

How can that be called "standard methods"? I would like to know where similar code can be found because it would have saved me a lot of work. Actually, if it already existed, I would probably not have started FairScan.

## Silence discourages me

**Ente has not acknowledged the code similarities as an issue**, publicly or privately. In response to my comparisons, the engineer described the code as "fairly standard image-processing methods", but this didn't address the similarities I had pointed out. If there's no problem, why did they rewrite more than 6,000 lines in 3 days after my email? Why don't they answer my questions?

Here's the key: if Ente's code was derived from FairScan in a way that makes it subject to the [GPL](https://github.com/pynicolas/FairScan/blob/main/LICENSE), its distribution raises questions about compliance with that license.

Should I take legal action? That's not what I want for FairScan. But I also don't want this issue to remain unaddressed, leaving others to form their opinion based on Ente's public statements alone.

Ente widely promoted the new feature on social media and platforms such as Reddit. [Its CEO praised Locker publicly with these words: "Best opensource document scanner I've ever used".](https://lnkd.in/p/eTpnkpNu)

On my side, I've been very careful until now. I initially communicated only privately with Ente. When I published a Mastodon post, it was very factual: I didn't make spectacular accusations and maybe because of that, there were almost no reactions.

I suspect very few people know about this situation. From the outside, Ente can continue promoting its scanning feature without having to address the concerns I've raised. The issue remains unresolved and Ente can continue ignoring me.

The fact that almost no one reacted is discouraging to me. Over the past few weeks, I've spent more time thinking about this than working on the rest of FairScan. It probably doesn't deserve it but I simply can't accept that situation. What's the point of open-source transparency and accountability?

I'd genuinely like to hear from other developers who have looked at these comparisons. Do these similarities suggest that Ente's code was derived from FairScan? **I would be grateful to anyone who can share an independent perspective on this case.**

## Open source doesn't equal respect

This episode makes me take another look at my position about open source.

FairScan is my attempt to make a respectful app: an app that is both [respectful to its users](https://fairscan.org/blog/a-respectful-app/) and respectful in the way it's made. I think it's now clearer to me that those two aspects don't necessarily go together. I still believe that open source is one of the best ways users have to assess whether an app can be trusted (caveat: it can still contain [anti-features](https://f-droid.org/docs/Anti-Features/)). However, it's perfectly possible to make an open source app in a disrespectful way, for example by copy-pasting code without acknowledging its origins.

Yes, the risk I identified before publishing my code is real. In fact, any open source code can be copied without attribution. I knew that closed-source software could incorporate open-source code without complying with its license, and that proving such a violation could be difficult. Maybe I accepted this risk because I cannot know when it actually happens. What I didn't consider was that open-source software could do the same, and that in that case, I could see it with my own eyes.

I knew that other projects could reuse FairScan's code even if they don't share my vision, but I did expect that the origins of the code would be acknowledged honestly.

What happened is something I wasn't ready for. I put a lot of myself into FairScan, perhaps too much: seeing my work dismissed as if it had no value is very hard not to take personally. Today, this makes me question my motivation to contribute to open-source software.
