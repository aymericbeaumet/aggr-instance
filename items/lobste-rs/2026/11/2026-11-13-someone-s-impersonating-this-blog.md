---
title: Someone's impersonating this blog
link: https://mattbee.mataroa.blog/p/someones-impersonating-this-blog/
source: lobste-rs
published: 2026-11-13T20:39:53Z
updated: 2026-11-13T20:39:53Z
first_seen: 2026-11-19T17:16:58.860737333Z
authors:
- mattbee.mataroa.blog via classichasclass
labels:
- security
summary: Comments
content: extracted
html: 2026-11-13-someone-s-impersonating-this-blog.html
remote_preview:
  url: https://mattbee.mataroa.blog/images/78d2a528.jpeg
  alt: You have been blocked from accessing abuse.cloudflare.com
---

I was googling the name of my website, as one does occasionally, and was surprised to see a link to one of my blog posts on `mataroa.site`. At first I figured it was just an alternate domain name for [mataroa.blog](https://mataroa.blog), but no, when you type `mataroa.site` into your browser, it shows you *my blog*. It's the only thing that's hosted there.

My next thought was that someone bought me a URL, but that can't be the case either: using a custom domain on mataroa requires a premium account, which I don't have. For anything but a plain redirect, I would have needed to set up the A record my end.

I then realized that the version of my blog on that site doesn't have [the latest post](https://mattbee.mataroa.blog/p/morning-shower-hack-new-website-where-ive-been). The most recent update on the homepage is [my Aug 21 entry](https://mattbee.mataroa.blog/p/well-actually). When you scroll down and start clicking some of the older links, you get 404 pages. You also get a 404 if you try to post a comment, load the RSS feed or view standalone pages like the [AI statement](https://mattbee.mataroa.blog/ai).

Someone scraped my blog and created a website to impersonate me. They're also impersonating [mataroa](https://mataroa.blog), since using the name implies some sort of official connection.

They're re-hosting the stolen images on their own server, and they all have a watermark. Bizarrely, the watermark is `mattbee.mataroa.blog`, my *real* URL. I assume this is to somehow make it seem legitimate, like it's an officially-sanctioned mirror, but I don't know who they think is going to care. It's not like they're stealing from a site with name recognition.

When you view the source of a blog post, it's identical to the rendered HTML except that there are two Cloudflare scripts at the bottom, one to obfuscate the `mailto:` link in the footer (which doesn't work[^1]) and one for analytics.

When you view the source of the index, there's a hidden link at the bottom to a Chinese `github.io` page for a website cloning tool.

I have no idea why I was targeted for impersonation. It should be obvious this isn't a high-value website.[^2] It's a personal blog where I write what's relevant to me, and that rarely overlaps with anything other people care about. If the subject matter didn't tip them off, it should be clear from the 0 comments on most posts that this is not an enterprise ripe for monetization.

If they just wanted a fake blog to scam adsense, surely a large language model would happily spew out more SEO-friendly slop than anything I have to offer. If their goal was to leech off the popularity of mataroa, why not impersonate one of the blogs with multiple front-page appearances on the orange website? Surely they have more SEO juice than me. Nothing about this makes any sense.

Anyway, the URL is registered with Namecheap, so I [sent them an abuse report](https://www.namecheap.com/support/knowledgebase/article.aspx/9196/5/how-and-where-can-i-file-abuse-complaints/). I have no idea if they'll act on it. They don't seem to have a good track record of giving a shit unless the content is actually hosted with them.

It's hosted with Cloudflare, and the abuse reporting form they insist you use [straight-up doesn't work](https://www.cloudflare.com/trust-hub/reporting-abuse/). Click "Submit an abuse report" and it'll take you to a completely blank page. Very encouraging.

I'd have to dox myself to do a DMCA takedown, and I don't have the legal resources to do anything when it gets ignored. So if neither evil corporation cares to do anything, I guess I can just get fucked. I figure the best thing I can do to protect myself is document everything here. If you see a link to `mataroa.site`, it's not affiliated with me or mataroa. It seems like it isn't set up to scam people or distribute malware at the moment, but it probably will in the future. Be careful.

## Edit

The index page also has a meta tag with a verification code for `mitgo.com`, a fake-looking website with some word salad identifying it as some kind of ad/SEO scamming platform or service. Will try to investigate this further.

## Update Nov 14, 2026: Welp

![You have been blocked from accessing abuse.cloudflare.com](https://mattbee.mataroa.blog/images/78d2a528.jpeg)

Sorry for... checking to see if your website started working? Too many times? I guess??? 🤷♀️

[^1]: It does, I had just forgotten to enable javascript.

[^2]: In a remunerative sense, I mean.
