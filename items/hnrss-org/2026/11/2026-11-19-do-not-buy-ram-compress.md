---
title: Do not buy RAM, compress
link: https://curiousbits.nl/blog/do-not-buy-ram-compress/
source: hnrss-org
published: 2026-11-19T08:56:47Z
updated: 2026-11-19T08:56:47Z
first_seen: 2026-11-19T17:16:58.860737333Z
authors:
- Ecno92
content: extracted
html: 2026-11-19-do-not-buy-ram-compress.html
remote_preview:
  url: https://curiousbits.nl/static/blog/do-not-buy-ram-compress/apps.png
  alt: List of apps with more memory demand than available memory
---

AI has changed a lot. We do more work in parallel. This increases the memory demand on our workstations. Also, memory is expensive and often not extendable. So buying more RAM is not an easy option. Recently I was running out of memory, I found my solution in `zram`, a solution to compress memory.

Memory compression is not new. If you have a Windows or Mac device you are already using it. Also, many Android smartphones utilize the technique of using compression to put more data into the RAM. Unfortunately not all Linux environments have RAM compression enabled by default. Fedora, Pop!\_OS do, but Ubuntu doesn’t. Amazon Linux only enables zram on smaller virtual machines. Most other distributions do not have it enabled.

Compressing memory does come with small, but not really noticeable, downsides. It takes a little bit more from your CPU and adds latency to some processes. What you gain is gigantic amounts of extra capacity. It is not uncommon to see compression ratios up to 1:4.

I remember that in the early days of writing SaaS software we would often need double the size of the machines when we added more features to our monolithic app. Often the CPUs were under little stress, so we could have avoided upgrading if we had enabled RAM compression. This could have cut our Cloud spend in half!

## How it looks in practice

I opened a lot of websites in Firefox. So many, it would almost fully take all system memory. (I have 32GB with some reserved for the graphics)

```
for u in nos.nl nu.nl telegraaf.nl ad.nl volkskrant.nl nrc.nl rtl.nl tweakers.net buienradar.nl ns.nl npo.nl \
  bbc.com/news edition.cnn.com theguardian.com nytimes.com reuters.com apnews.com aljazeera.com dw.com spiegel.de lemonde.fr washingtonpost.com bloomberg.com \
  theverge.com arstechnica.com techcrunch.com wired.com engadget.com phoronix.com lwn.net news.ycombinator.com tomshardware.com \
  github.com/torvalds/linux github.com/microsoft/vscode github.com/rust-lang/rust stackoverflow.com/questions gitlab.com developer.mozilla.org docs.python.org kernel.org ubuntu.com huggingface.co \
  youtube.com "youtube.com/results?search_query=linux" "youtube.com/results?search_query=music" "youtube.com/results?search_query=documentary" twitch.tv vimeo.com dailymotion.com soundcloud.com open.spotify.com \
  reddit.com reddit.com/r/linux reddit.com/r/worldnews reddit.com/r/pics reddit.com/r/technology x.com instagram.com linkedin.com pinterest.com tumblr.com imgur.com 9gag.com medium.com quora.com \
  bol.com coolblue.nl marktplaats.nl amazon.nl amazon.com ebay.com aliexpress.com ikea.com/nl/nl zalando.nl etsy.com mediamarkt.nl ah.nl \
  google.com/maps openstreetmap.org booking.com airbnb.com tripadvisor.com klm.nl schiphol.nl \
  en.wikipedia.org/wiki/Linux en.wikipedia.org/wiki/Netherlands en.wikipedia.org/wiki/Amsterdam en.wikipedia.org/wiki/World_War_II nl.wikipedia.org/wiki/Nederland \
  imdb.com rottentomatoes.com archive.org nationalgeographic.com nasa.gov flickr.com/explore espn.com weather.com \
  canva.com figma.com notion.so trello.com office.com dropbox.com wikihow.com; do
  firefox --new-tab "https://$u" >/dev/null 2>&1; sleep 1
done
```

If we look at the running apps, then we see it should not fit.

![List of apps with more memory demand than available memory](https://curiousbits.nl/static/blog/do-not-buy-ram-compress/apps.png)

In the memory usage we see that some demand was placed into the zram swap space.

![Overview of memory usage. Some applications are in zram swap space](https://curiousbits.nl/static/blog/do-not-buy-ram-compress/memory.png)

In the terminal we can check how much memory is used, and we can calculate the compression ratio.

![zramctl overview](https://curiousbits.nl/static/blog/do-not-buy-ram-compress/zramctl.png)

5.7G of data in 1.7G of RAM. That’s a compression ratio of 1:3.35.

## Getting started

Compressing RAM is something which helps to get you more out of your own hardware, but also out of your cloud environment. Do more with less.

[The Arch Linux wiki has a good article on how to get started](https://wiki.archlinux.org/title/Zram).

Looking forward to hear about your experiences.

## Get in touch

Looking to hire a software engineer or technical leader for your project?

[More contact options →](https://curiousbits.nl/contact/)
