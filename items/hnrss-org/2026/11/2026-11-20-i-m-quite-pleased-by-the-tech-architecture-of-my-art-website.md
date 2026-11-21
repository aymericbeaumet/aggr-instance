---
title: I'm quite pleased by the tech architecture of my art website
link: https://klauria.tech/klauria-art/
source: hnrss-org
published: 2026-11-20T17:03:39Z
updated: 2026-11-20T17:03:39Z
first_seen: 2026-11-21T23:45:44.123432357Z
authors:
- evakhoury
content: extracted
html: 2026-11-20-i-m-quite-pleased-by-the-tech-architecture-of-my-art-website.html
remote_preview:
  url: https://klauria.tech/klauria-art.jpg
---

[This piece has embedded interactive widgets from my art website!](https://klauria.tech/klauria-art/#first-demo)

![Mono Lake Tufa at Sunset](https://klauria.tech/_astro/2026-07-10-mono-lake-tufa-at-sunset.Cg94MFl2_Z1RD1X4.webp)

It is a truth universally acknowledged, that every developer blog begins by explaining the new static site generator they wrote to make it. This post isn’t *exactly* that, but maybe only on a technicality, in that it’s not about *this* website…

## The problem

Earlier this year I decided to put a some of my paintings and sketchbook pages online. I don’t much use social media besides Hacker News, but I wanted to share my work with friends and family. So I’d been sending scans of my work in group chats. That was fine for a bit but so messy and disorganized. Sometimes someone asked, “Where did you paint this?” Sometimes a friend wanted to see what it looked like in real life. My little brother suggested a map with a marker for each piece. That was a good idea! For that I’d need a website and some code. Here, take a look at the map, its done now! You can zoom and click on markers to see the painting!

When it comes to tech side projects there are two wolves inside me. One is the Wolf of Pragmatism, which wants to smash every problem with whatever hammer is nearest at hand: “Isn’t this just some trivial React? Dump the images in S3 and call it a day. This website is for friends and family; you have no traffic to speak of. Put a CDN in front so your parents in India get reasonable performance.”

The other is the Aesthetic Wolf: “This should be a static website. The content almost never changes.”

Yes. This is a static website. I do at most four, and on average less than 0.3, paintings a day. The content almost never changes. Why should javascript render the page on the user’s browser? Why should any code even run on the server? I should have the bytes for every page just sitting there, ready to be shipped. The correct server implementation for this problem is [open](https://man7.org/linux/man-pages/man2/open.2.html) followed by [sendfile](https://man7.org/linux/man-pages/man2/sendfile.2.html) inside the event loop (I’m joking but only kind of?). The Aesthetic Wolf won.

But the problem with a static website is that for every new post, I need to add a file, and run a build, and deploy it to production. I made a hundred paintings this year! Even if I automate a lot of it, I wanted a better UI for stuff like adding tags and setting location. And the best UI for what’s essentially a CRUD app is … probably a web UI. But if I have a web UI that changes the content of the website, that’s not a static website any more.

Now look at this colorful house!

![House By Red Barn](https://klauria.tech/_astro/2026-08-30-house-by-red-barn.CilC71cF_1hLcV9.webp)

So what I really want is a website that’s static for everyone else, but dynamic for me. I want a build system but I want it triggered by actions in a nice CRUD UI that I make, not GitHub actions or a terminal.

Here’s my silly little solution to this silly conundrum. I’m sure the design I’m about to describe isn’t novel, but I did come up with it. I’m just pleased by it because it’s an efficient solution to the exact problem I happened to have. It’s dollar-efficient for me, i.e., it’s free\*. It’s resource-efficient for Cloudflare, because the static assets are rarely rebuilt and mostly sit there cached. And it’s fast, because the bytes are mostly sitting there in a CDN waiting to be sent.

(Asterisk: the compute and bandwidth are free. The storage is currently free, but if I paint enough then I’ll go past the 10 GB free tier and pay a couple cents per month.)

## The solution

The main idea is that every html page *is* rendered on the server, but with a cache header telling Cloudflare’s infrastructure to cache the response forever (well, for 30 days). Then, whenever I upload a new art piece or change the set of tags or anything like that, the request handler for this write operation invalidates all cached pages. That’s it.

There’s one detail that makes this work well for an image-heavy website: we generate several smaller sizes of each image *at upload time*, and I actually do it on the client’s browser. Since I’m the only uploader, that would be my browser. We need the smaller sizes for thumbnails and cards, where sending the full archival images would be way too slow. Those downsampled images are uploaded along with originals to Cloudflare’s R2, their S3-equivalent.

Why downsample at upload time? Cloudflare does offer an image service that’ll do the resizing the first time you make a request for that image at that size, and it’ll cache that resized copy. But in my case I already know the three sizes I want, so it’s faster to just precompute and store them. The additional storage doesn’t add up to much, because the archival images are enormous compared to downsampled copies.

All the metadata for all pieces goes in a single json file that also lives in R2. When someone requests a dynamic html page, like the one for [pieces tagged #summer](https://klauria.art/tag/summer), a piece of code runs in a Cloudflare worker. It reads the json and generates the html, which is then cached until the next invalidation. Cloudflare’s CDN handles the rest: if someone in India later requests the same page, the cached artifact is sent to an edge machine in India and cached there for a while. I suppose Cloudflare can choose to rerun the worker instead of copying the artifact, which is fine by me. Based on my testing that’s not what happens: the generator writes a timestamp on each page so I can see when it was generated.

Now look at these cool rocks!

![Kodachrome Entrada](https://klauria.tech/_astro/2026-08-16-kodachrome-entrada.CKwDdWO1_jjGmM.webp)

## Interactive elements

The website has some interactive stuff like maps and timelines. Those are just embedded javascript that run on the client’s browser and read that single metadata json file to initialize. They don’t write anything back to the server, so there’s no database or anything. They’re just displaying the information in different ways. For example, here’s an interactive timeline of my pieces over the years. I haven’t uploaded everything I’ve ever made, so this is biased toward recent stuff.

(To be very technically correct, this timeline component doesn’t read the json directly. Its bars are drawn on the server, and it gets a per-month list derived from the metadata to handle clicking and paging.)

Here’s what the page for a piece looks like. The metadata json has the information to figure out what pieces were done nearby and around the same time, so I show all that. This is an embedded iframe so you can scroll and click:

## Why invalidate everything?

This is where Pragmatic Wolf won. Here’s the possible optimization: whenever I make an edit, we should invalidate only those pages that are potentially affected by that edit. A good build system certainly works this way: when you change one file, it recompiles only those artifacts that (transitively) depend on it.

In my case, however, dependencies aren’t explicitly written down anywhere. The code to generate a page just runs, and potentially any change in the metadata json could cause its output to be different. I could try to encode the dependencies for each page and component, but it’s so easy to get this wrong and introduce bugs in the future. The result is a half-updated website, i.e., a correctness bug.

So while it’s true that adding a new piece only changes the [#evening](https://klauria.art/tag/evening) page if the new piece actually has that tag, I considered it too tricky to get this cache invalidation right, and decided to just invalidate everything. If I were using a normal static site generator then the CDN could potentially do this optimization by noticing that the hash of an artifact hasn’t changed, so it doesn’t need to be updated on every edge machine.

Anyway here is a waterfall in Washington, the one in Twin Peaks apparently:

![Twin Peaks](https://klauria.tech/_astro/2026-08-29-twin-peaks.CHf2WPtS_2ms8wj.webp)

And here’s the view from right outside my house in Brooklyn:

![Hot Morning Outside My House](https://klauria.tech/_astro/2026-07-04-hot-morning-outside-my-house.DMUyTAkt_1xztog.webp)

By the way, the specific javascript framework I ended up using is called Astro. Cloudflare bought them so they’re pretty nicely integrated. But you don’t really need it for this, you could write your own code to run in the Worker to generate the page. The whole trick is in some careful management of cache.
