---
title: If you love a website, back it up
link: http://blog.mistys-internet.website/post/2026/10/29/if-you-love-a-website-back-it-up/
source: lobste-rs
published: 2026-10-30T01:29:00Z
updated: 2026-10-30T01:29:00Z
first_seen: 2026-10-30T22:55:36.634700404Z
authors:
- blog.mistys-internet.website via calvin
labels:
- web
summary: Comments
content: extracted
html: 2026-10-30-if-you-love-a-website-back-it-up.html
remote_preview:
  url: https://blog.mistys-internet.website/images/webarchiving/domaincrawl.png
  alt: Screenshot of the Browsertrix interface, depicting a crawl scope of "Pages on the same domain"
---

This story starts with a little tragedy. See, I’ve got these old webpages I love to go back and look on every now and then. There’s websites that have been online unchanged for years and years, or decades, and there’s something a little cozy about checking in on them every now and then to see an old friend is still doing okay. Sometimes, of course, tragedy strikes.

There was this fansite called teapuppies.com for an old anime series and and toy line called *Ochaken*. It’s about these cute cartoon dog characters who have tea leaves for ears, each of them named after a different kind of tea.[^1] The site had been online since 2004 and had basically not changed since 2005, but it was nice just having it around. Except when I went to visit it a few weeks ago, and discovered that not only was the site gone but the domain is now being used to serve online gambling ads.

I work in web archiving, by the way. Which is to say that I should have known the site would go offline someday, *and* that I have the tools to have done something about it. I could have archived that site while it was still around, but I didn’t. This post is here to beg you to do what I didn’t think to, and to teach you how.

You see, when I talk to a lot of people about web archiving, they always seem to believe it’s something *they* couldn’t do. It’s something a big organization like the Internet Archive does, and if you want a webpage to be saved you just have to wait and hope it gets done. But that’s not true! What I really want everyone to know is that *you* have control over the world around you, *you* can take action to preserve things and make things better. And not just *can*, but *should*! Don’t just wait for someone else to act - get out there and and make it happen! Got a favourite site? Back it up! Got some information you don’t want to get lost? Back it up! Favourite post somewhere? All together now. Back! It! Up!

All of the tools I’ll be showing off in this post use standard web archiving formats like [WARC](https://en.wikipedia.org/wiki/WARC_\(file_format\))[^2]. You don’t have to worry about any of the technical details[^3], but I’m telling you that because it means there are a lot of really great tools to do whatever you want with them. Here, just as an example, is an entire archived website embedded right in this blog post:

You can even click through all the links, look at all of the images, the works. It’s an entire website right in a post! (And if you don’t trust me to keep this up, and you want the web archive to host somewhere else? You can download it for yourself right from there. Just click the kebab menu and choose “download archive”, and you can do whatever you like with it. In fact, please do!)

By the way, for disclosure: the tools I’m going to mention in this blog post are made by the company I work for, but that’s not because they’ll pay my salary (most of them are free!). I just happen to think these are the best tools there are for personal web archiving.

### Option 1 - ArchiveWeb.page browser extension

By far the easiest personal web archiving tool is the [ArchiveWeb.page](https://archiveweb.page/guide) browser extension for Chrome. This doesn’t take much in the way of setup or technical skills, and it works for anything: a single page, an entire website, a social media post or profile. So long as you can see it in a web browser, you can save it. All you really need to do is:

1. [Install the browser extension](https://chromewebstore.google.com/detail/webrecorder-archivewebpag/fpeoodllldobpkbkabpblcfaogecpndd).
2. Visit the first page you want to archive.
3. Click the ArchiveWeb.page icon in the toolbar, then click “Start Archiving”.
4. If your page has any interactive elements, click on those to make sure all their data loads.
5. Visit any other pages you want captured in the same collection and let the extension grab them.
6. Click the ArchiveWeb.page icon in the toolbar, then click “Stop Archiving”.

You’re done! *You just archived a web page!* Now that it’s all saved, you can preview it and see how it went by clicking the extension’s icon and then the home icon to open the extension dashboard. From there, you can click on your archiving session to preview the pages you archived, load them in browser, and see if they work. (I’ll go into more details on how you can look at web archives and share them with others later in this post.)

### Option 2 - archiveweb.page app

If you don’t want to install a browser extension, or you don’t have Chrome installed, you can also grab the [standalone archiveweb.page app](https://archiveweb.page/). This is basically the same thing as the browser extension above, it just has its own web browser built in. It also opens straight to the menu that lets you browse your web archives, so it’s a handy way to look at your web archives locally. Pretty much everything else I described above applies.

### Option 3 - Browsertrix

The above options are great for single pages or sites that are small enough for you to browse manually. But if you’ve got a webpage that’s big enough that it’s going to be annoying visiting all of the pages by hand, you probably want to look at a *web crawler* tool for web archiving. There are a bunch of options out there, but the easiest to use as an individual is probably [Browsertrix](https://webrecorder.net/browsertrix/). A web crawler is basically an automation tool built around a web browser; you provide instructions on what page to start from and what pages it should visit, and it will go ahead and click links to automatically find pages so that it can archive the page as completely as possible. The web archive I embedded above was done using Browsertrix on pretty close to its default settings: I just told it I wanted to do a site crawl for the entire domain `https://teapuppies.mistys-internet.website`, and it worked it out from there:

![Screenshot of the Browsertrix interface, depicting a crawl scope of \"Pages on the same domain\"](https://blog.mistys-internet.website/images/webarchiving/domaincrawl.png)

But, depending, on what you’re looking for, it has options which can be customized to narrow down what content it archives. For example, if you decided you only want pages for the episodes, you could tell it to only capture anything at the `/episodes/` section of the website:

![Screenshot of the Browsertrix interface, depicting a crawl scope of \"Pages in the same directory\" with a seed of https://teapuppies.mistys-internet.website/episodes/](https://blog.mistys-internet.website/images/webarchiving/directory.png)

Or even just a single page and anything that page links to:

![Screenshot of the Browsertrix interface, depicting a crawl scope of \"Single page\" and the checkbox \"Include directly linked pages\" selected](https://blog.mistys-internet.website/images/webarchiving/singlepagewithlinks.png)

There are a *lot* more options than I can go into in a quick overview - it’s a comprehensive tool with a lot of options. But if you’re looking to archive a whole website, or a complete social media profile, I’d recommend giving it a try.

Unlike the other tools, which you mainly run on your own computer, Browsertrix is primarily a web app. It’s also the only paid tool in this post - there’s a seven day trial, but after that you do have to pay per use. Since it’s open source, though, technically inclined users can run their own copies on their computer or deploy a copy somewhere if they like.

## Viewing your archived webpage

Once you’ve actually backed the site up, you can *replay* it to use it in your browser just like a live webpage. It’s kind of like having your own private version of the wayback machine. You can test it out yourself for your own private use using the interface built into the version of archiveweb.page you used, but if you want to share it with someone else you have a few options. If you want to share it with someone privately, you can send them the WARC/WACZ file and let them load it into the ArchiveWeb.page app or the [ReplayWeb.page](https://replayweb.page) website. If you want to share it with the world, though, you can set up a website with your web archive on it.

This post is using the [ReplayWeb.page](https://replayweb.page/docs/embedding/) embedded viewer, which is open source and can be embedded inside any other web page you want. You don’t have run any kind of app on your server to do this; as long as you have the ability to upload the archive and a little bit of JavaScript, you can easily add a copy of the replay interface into any web page or blog post.

## Preserving the data once you have it

Since everything you backed up is bundled inside a single WACZ file, it’s really easy to keep safe - or share with someone else. Lots of copies keeps stuff safe, after all; if you think you have friends who’d like to hold on to a copy of whatever site you’ve backed it up, pass it along and share the duty of preserving it!

* * *

If any of this sounds interesting to you, I’d encourage you to at least give it a try. I’d *love* for the world to be filled with thousands upon thousands of little web archives, with people and community groups archiving and sharing the things that matter to them for themselves. If you’re finding any of this overwhelming, and you’re not sure where to get started, *please reach out to me* - I’m very happy to give you a hand.

[^1]: The appeal to me is, I hope, obvious.

[^2]: Most of the tools I’m mentioning also use the [WACZ](https://specs.webrecorder.net/wacz/1.1.1/) format, which packages up WARC files along with extra metadata.

[^3]: For those who want a *little* extra technical detail, though: one of the things that makes WARC special is that it records complete information about requests and responses, including all headers. This gives WARC a much more complete view of the whole process of visiting a website, and makes it possible to archive even complicated sites with client-side JavaScript that make a ton of requests.
