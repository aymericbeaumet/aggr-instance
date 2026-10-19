---
title: Fancy Curl Sites
link: https://nytpu.com/gemlog/2026-03-18
source: lobste-rs
published: 2026-10-14T06:24:41Z
updated: 2026-10-14T06:24:41Z
first_seen: 2026-10-15T21:44:29.602546406Z
authors:
- nytpu.com via seb
labels:
- web
summary: Comments
content: extracted
html: 2026-10-14-fancy-curl-sites.html
---

### March 18, 2026 (Updated: October 14, 2026)

So a good while ago I added a fun curl version of my site, meant for viewing in the terminal. Try running these commands!

```
curl nytpu.com/gemlog/2026-03-18
# or
curl nytpu.com
```

(sorry Windows users, not my fault that ANSI escape codes are disabled-by-default in `cmd.exe`. And not my fault that in Powershell you have to use `curl.exe` instead of `curl` because Powershell inexplicably aliases that to `Invoke-WebRequest`.)

I posted about it a bit on the Fediverse, but otherwise didn't elaborate much on it becuase I figured it was fairly self-explanatory. But I've had a handful of people ask me how I did it, so I decided it might actually be worth it to write a post about how I did it (…like seven months late).

I should note this idea came from trinkey's (`curl trinkey.com`) and strongsand's (`curl proot.party`) curl sites, I didn't come up with it myself. Even though I did go about it quite a bit differently from them, all credit for the very cool idea goes to them. (UPDATE: unfortunately neither of their curl versions are functional anymore as of 2026-10-14.)

First, since it's the more interesting part, actually generating the ANSI-marked-up files to send to the clients. I entirely hand-wrote the [index page](https://git.sr.ht/~nytpu/nytpu.com/tree/master/item/static/curl/index.ansi) and [about page](https://git.sr.ht/~nytpu/nytpu.com/tree/master/item/static/curl/about.ansi).

Note that I believe trinkey and strongsand generate theirs on-the-fly with the [ratatui](https://github.com/ratatui/ratatui) library (or similar), which is probably more convenient; hand-writing it seemed more expedient to me though.

I had the main escape code—`ESC[`, i.e. `\x1b[`—[^1] in a Vim register to easily paste when needed, and then the [Wikipedia page for the main graphics codes](https://en.wikipedia.org/wiki/ANSI_escape_code#Select_Graphic_Rendition_parameters) open to reference the specific escape codes. And, yeah, that's about it. I experimented to decide on consistent styling I liked, and just referenced the Gopher version of my pages when crafting the marked-up curl pages.

Now, the notable thing about my version is that I have curl versions of the entirety of my blog, rather than just one or two pages being available. This is entirely due to a convenient coincidence: over four years ago I implemented outputting ANSI-marked-up plaintext in my custom markup language I use for my blog, [Gemroff](https://git.sr.ht/~nytpu/gemroff). Originally it was just to allow convenient previewing of drafts in the terminal while writing; but it was trivial to plug into my static site generator to immediately get Curl versions generated.

I did manually write a [wrapper template for blog posts](https://git.sr.ht/~nytpu/nytpu.com/tree/master/item/templates/gemlog-post.ansi), as well as a [template for the index page listing posts](https://git.sr.ht/~nytpu/nytpu.com/tree/master/item/templates/gemlog-index.ansi), but that plus a few extra lines in my static site generator was all that was needed to wire it up.

This is the main thing that makes it hard to say it's easy! since it was only easy for me because I use an entirely custom markup language and static site generator and I happened to make the right design choices years ago—e.g. my SSG also happened to already have support for multiple, modular output formats instead of solely HTML like other SSGs, since I tri-host my site on HTTP, Gemini, and Gopher.

However, offering nothing but a few hand-written pages for fun is a completely acceptable way to set up a site like this anyways, since it's just a neat gimmick and no one's likely to actually use it more than a few minutes of browsing.

Finally, the last step is actually presenting the Curl version of your site to any user agent containing `curl`, which for me was just a [few changes to my Nginx config](https://nytpu.com/gemlog/files/nytpu.com.conf), with other servers it'd presumably be equivalently simple. The main feature I wanted was an override header (`X-Original: true`) so you could still get an unmodified HTTP response like someone casually running Curl likely wanted, and I wanted everything to be served statically still.

And… that's really it! As I said, when I first set it up it seemed fairly self-explanatory to me, but it is very non-orthogonal to what people normally do with either web servers or ANSI escape codes so I can see where people's curiosity/confusion came from. It would be neat to see more sites doing this trick around!

[^1]: EMDASH! AI-GENERATED! acksually I'll have you know that my markup language has had [an explicit markup element for emdashes since November 2022](https://git.sr.ht/~nytpu/gemroff/tree/999d2f7da1757d756b9780c500f233d0b98e7628/item/doc/elements.md?view-source#L49). And believe it nor not: LLMs are trained on shit copied from actual authors, so LLMs wouldn't be using emdashes if they weren't already used in human-written works.

    And what, am I supposed to avoid having twenty parentheticals a paragraph, to where I feel the need to use emdashes to vary it up? Unthinkable!
