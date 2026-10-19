---
title: I've Been Deindexed by Google
link: https://kennyqin.com/deindexed-by-google/
source: lobste-rs
published: 2026-10-08T07:59:30Z
updated: 2026-10-08T07:59:30Z
first_seen: 2026-10-09T13:07:02.003473070Z
authors:
- kennyqin.com via brn
labels:
- degoogle
- meta
- practices
summary: Comments
content: extracted
html: 2026-10-08-i-ve-been-deindexed-by-google.html
---

I don't remember exactly when but, earlier in the year, I decided I didn't want this website to be indexed by search engines anymore. A lot of it has to do with AI and the relentless scraping that plagues the internet now. I know that as long as my blog is accessible by the public internet, there are no real measures I can put in to stop it but at least some of it will be respected.

I started out by setting a blanket disallow in the robots.txt.

```
User-agent: *
Disallow: /
```

When I realised that didn't do anything after a few weeks of waiting and checking, I set the `robots` meta tag.

```
<meta name="robots" content="noindex, nofollow" />
```

I thought that would do the trick. What I didn't realise is that you have to actually, ironically, **allow** the bot that will do the deindexing to visit your website so that it can see the meta tag that tells it to not index your page anymore.

```
User-agent: Googlebot
Allow: /

User-agent: *
Disallow: /
```

I also did this with several other common bots and removed them from the robots.txt whenever I found that my website had been deindexed.

It's been at least 3 months since I did my last change to have Google deindex me and, after checking today, I'm happy to announce that this website is free from Google! 🥳
