---
title: What I Self-Host
link: https://viceconsole.fyi/blog/2026/10/10/what-i-self-host/
source: lobste-rs
published: 2026-10-14T15:32:48Z
updated: 2026-10-14T15:32:48Z
first_seen: 2026-10-15T14:23:03.950687001Z
authors:
- viceconsole.fyi via tonymet
labels:
- devops
summary: Comments
content: extracted
html: 2026-10-14-what-i-self-host.html
---

I’ve always had a foot in the self-hosting world, long before it was called that. My freshman year of university, I ran a website from a Linux box underneath my bed. There was some downtime when my roommate got tired of the fan noise and turned it off. For several years I ran [MythTV](https://www.mythtv.org/), then [XBMC](https://github.com/xbmc/xbmc) (later known as Kodi).

Recently, I lived for a few years in India, where streaming services are extremely inexpensive by western standards. We had Netflix, Disney Plus (Hotstar in India), and Spotify. After leaving India, I was unwilling to pay western prices for these services. But I had also become fed up with the engagement treadmill, particularly for my son. We wanted to limit his viewing choices to certain programming, but the constant “recommended for you” and “up next” caused conflict. While there are ways to limit content to specific titles, or disable auto-play, doing this for each service and keeping track of when client applications change and need to be reconfigured is too much work.

So, I “cut the cord” in a different sense, ditching streaming services for self-hosted [Jellyfin](https://jellyfin.org/). Over the course of the next year and a half, Jellyfin opened the door to self-hosting many other services, reducing my dependence on cloud service providers, and providing a welcome stability. No longer am I constantly confronted with unnecessary site redesigns, new features of questionable value, and a barrage of e-mails about terms-of-service updates, binding arbitration, data breaches, etc. I read the release notes of new versions of the software I host and decide if and when I want to upgrade.

## Rules

I have a few ground rules.

With the exception of VPN servers that run on my router, I do not expose any services directly to the public internet. I don’t care how convenient it would be, or how secure an application is. Especially with AI-assisted hacking, I simply don’t think it’s wise. My ISP also uses [CGNAT](https://en.wikipedia.org/wiki/Carrier-grade_NAT) for its IPv4 addresses, so my home network is not directly reachable from the public internet via IPv4. My VPN servers are only publicly routable on IPv6, a limitation that has some downsides but is workable for my use cases, and likely helps limit unfriendly attempted connections. The biggest downside is that my VPN will almost never function on a public Wifi network, since these rarely offer IPv6. When using my phone, I can disable Wi-Fi and connect over mobile data… if there is service.

I prefer running services natively when possible, but will containerize complex services or those with many dependencies. Love: a service that’s a single Go binary. A service that only comes as a Docker compose file and wants to run MySQL, Redis, and three other containers? I’m already looking for an alternative. If the dependencies aren’t overwhelming, native is better, faster, and more efficient. I can use the same management tools and commands across multiple services. There’s no mental overhead of having to get into this container to run some command, finding out this container is missing curl, that one runs busybox so all the arguments are different, or dealing with a virtual network and host port mapping.

Related, I don’t do Docker anymore. It’s not really a principled position, but I grew tired of having to manage a completely separate service control system on my server. I’m already using systemd, and at this point [Podman’s integration with systemd](https://docs.podman.io/en/v5.0.1/markdown/podman-systemd.unit.5.html) is excellent.

## Hardware

Last year I bought a Beelink Mini PC, specifically an S12 Pro with an Intel N100 processor, 16 GB RAM, and a 512 GB internal SSD. It was before the rampocalypse, so I paid EUR 180. The same listing on Amazon is currently EUR 359. Ouch! Attached to the server is a 6 TB hard drive for media storage.

My [comment on Hacker News](https://news.ycombinator.com/item?id=49985548#50014938) about what I self-host prompted some questions about how all of this could run on 16 GB RAM. As I describe below, there are some choices I’ve made that reduce resource demand on my server, but ultimately it turns out you don’t need much hardware to run a lot of stuff. Obviously I’m not running any local inference on this hardware, but for everything non-AI, it has just worked out of the box, or with some simple configuration tweaks.

## Services

In alphabetical order.

### Actual Budget

Budgeting software with desktop clients and a web application, custom reports, and rules-based categorization. I only use this for budgeting and checking/savings/credit accounts. For tracking investment accounts, I use a cloud service. Some day I’ll try to move away from the latter…

While [Actual Budget](https://actualbudget.org/docs/releases/) supports automatic transaction import from various banks, I don’t use this feature. Instead, after each month end, I download the prior month’s transactions (OFX format preferred, CSV if no other option) and import them manually. This works for me because I primarily use Actual Budget for expense tracking, not… actual… budgeting.

Podman.

### Audiobookshelf

Add, download and store audiobooks and podcasts on your server, listen to them on your phone. I haven’t used the audiobook feature much, I set it up more for my kids, but it has very nice chapter handling. In the past for podcasts I used PlayerFM, then Pocket Casts, but now unless I’m out of the house without VPN connectivity, I use [Audiobookshelf](https://audiobookshelf.org/).

Native.

### Automatic Ripping Machine (ARM)

[ARM](https://github.com/automatic-ripping-machine/automatic-ripping-machine) automates ripping and transcoding DVDs (and Blu-rays, but I don’t do those). Yes, it’s 2026 and I still rip DVDs. Remember, we cut the cord, so not only is this a conduit for new-to-us content, but it’s also a way for us to get bilingual content (English and French).

Podman.

### Backrest

Web UI for [Restic](https://restic.net/) backup software. Backups are important! Especially for Immich (photos). Restic does a good job deduplicating and compressing backed up data, and [Backrest](https://github.com/garethgeorge/backrest) makes it easy to manage the storage retention policies. Notifications in case of failure go to Gotify.

Native.

### Beszel

[Beszel](https://beszel.dev/) is a simple monitoring dashboard that collects basic stats from agents (in the old-fashioned sense of a small background service) to display CPU load, memory and disk usage, temperature, battery, etc. Helps me keep an eye on various other hosts I have around the house (several Rapsberry Pis and a Proxmox cluster that may be the subject of a future post).

Native.

### Caddy

[Caddy](https://caddyserver.com/) is a web server and reverse proxy. Early on I didn’t understand the value of a reverse proxy, and I thought I would be fine accessing my services with a local DNS name, or even IP address, and port number. But then I started adding more and more services, and some required HTTPS. So I got a domain name (not this one) and set up Caddy to automatically obtain LetsEncrypt SSL certificates, and now nearly all of my services are accessible on my LAN (or over VPN) with HTTPS and friendly subdomain names. Adding a new service is a quick edit to the Caddyfile.

Native.

### Cockpit

[Cockpit](https://cockpit-project.org/) is a web-based admin console for the server. It uses normal system authentication methods. For me, it’s mainly useful for spotting failed services when no notifications are set up for them.

Native.

### Forgejo

[Forgejo](https://forgejo.org/) is a fork of Gitea, and deploys as a single binary. Github’s uptime is the butt of a lot of jokes these days, but I also appreciate the speed of having my own git remote. Eventually I’ll set up CI/CD and runners, if only to deploy this blog.

Native.

[Web-based RSS reader](https://www.freshrss.org/). These days I basically visit two websites, though, so I’ve not been using it much. I do subscribe to the Github releases of some of the services I self-host so I can check out the features and bugfixes in new versions.

Native.

### Frigate

Network video recorder (NVR). I first started with [Zoneminder](https://zoneminder.com/) because I’m of a certain age. But it became clear that Zoneminder is past its prime. [Frigate](https://github.com/blakeblackshear/frigate) has been fantastic. I run it with 8 IP cameras. It provides alerts for people and cars entering the driveway, integrates with Home Assistant to power various automations (e.g., when a person is detected in the backyard, turn on the camera floodlight). I may write a future post with some details about my IP camera adventures.

I run all of my cameras at lower resolutions than they support, primarily to avoid Wi-Fi network congestion. Most cameras also support an even lower-resolution RTSP substream, which is what Frigate uses for motion and object detection. Keeping the camera resolutions low avoids excessive CPU and memory from video processing, and excessive disk usage from recordings. They are good enough to identify faces, and privacy laws in my country prohibit filming public rights of way, so my cameras are not aimed at distant points.

Podman.

### Gonic

Subsonic-compatible server written in Go. I purchased a perpetual Subsonic license a long time ago, but the original project is ancient and the modern alternatives are better. I run two separate instances of [Gonic](https://github.com/sentriz/gonic), one serves kid music, the other serves adult music.

Native.

### Gotify

Simple notification service written in Go. Many self-hosted services directly support [Gotify](https://gotify.net/) notifications, or can send them via a custom HTTP POST. In conjunction with Uptime Kuma, I use Gotify to get notifications about any services or devices that are down, what my kids are watching on Jellyfin, when a new podcast episode is available in Audiobookshelf, etc.

Native.

### Home Assistant

[Home Assistant](https://www.home-assistant.io/) probably deserves its own post. It’s fantastic, and I cannot imagine trying to accomplish the same things within a proprietary ecosystem like Google Home, Amazon Alexa, or Apple Home. Things are better now that device manufacturers are standardizing on protocols like Matter, but the control and flexibility you get with Home Assistant is hard to beat. It has a steep learning curve, though, and I need to think about realistic ways to handle potential server failure without compromising the functionality of my home.

Podman.

### Homepage

At some point I realized I was running so many services, I needed another service just to keep track of them all. [Homepage](https://gethomepage.dev/) was a good solution. It has widgets for almost all the services I run, so I can get a quick overview of what’s going on, or just a quick link to the subdomain of a service.

Podman.

### Immich

Google Photos alternative. I was tired of Google constantly promoting paid storage plans in their apps. I was able to manage my storage space, but my wife was always about to run out of room. We moved to [Immich](https://immich.app/) last year, just after version 2 was released. It’s much faster to load pages full of photos when all the network activity is within your house. I’ve come up with a solution for sharing albums with family, as well. Still a lot of work to do with Immich - I have many duplicates, and now that storage isn’t a concern, I’ve been less disciplined about culling photos and videos.

Podman.

### Jellyfin

As I mentioned in the introduction, I’ve self-hosted media for a long time, going all the way back to MythTV twenty-some years ago. [Jellyfin](https://jellyfin.org/) is opinionated about its media organization, but it generally works well. I only use it for movies, TV, and home videos, so I can’t speak to its other features (e-books, music, photos, etc.). While my son still remembers Netflix, he is satisfied with Jellyfin. I appreciate the lack of auto-play, never seeing recommendations, and not having to scroll past a bunch of garbage to find what I want to watch.

One of my ways of managing resource demand is by generally preferring 720p media over 1080p or higher. And obviously when ripping DVDs, it’s either 576p or 480p. I do have a 4k TV, but it’s not extremely large, and while I can tell the difference in resolution between SD and HD, for almost everything, 720p seems to be good enough for me.

Podman.

### Music Player Daemon (MPD)

Sometimes there are solutions that just work, and don’t really need to change. I think [MPD](https://www.musicpd.org/) is one of these. It’s been around for 23 years. While Gonic is great for listening to music on my phone, even streaming music from home to my phone while I’m away via VPN, MPD is how I play music through Snapcast to the whole house audio system. I run two instances of MPD, one for music, and one for podcasts.

Native.

### PocketBase

I used to keep track of transactions and receipts for special accounts (HSA, 529, etc.) in a SQLite file I accessed directly with SQL Workbench/J. But things became more complicated as I moved between countries, currencies, and different insurance systems, and also wanted to offer my wife the ability to enter transactions. I migrated to [PocketBase](https://pocketbase.io/), which is ultimately still just a SQLite file, but with a nice web-based management frontend that I abuse as a generic CRUD frontend. It’s simple enough for my wife to use as well, and I can still run arbitrary queries against the underlying database file.

Native.

### Readeck

[Readeck](https://readeck.org/en/) lets me tag and archive copies of stuff I read. I was a heavy [Zotero](https://www.zotero.org/) user in graduate school, but I’ve not needed to write a bibliography in years. However, I still read things that I want to ensure I can access later. I tried [Wallabag](https://wallabag.org/) first, but it had trouble archiving things that were paywalled, even when I was logged in on my phone. In contrast, Readeck will archive what my phone is seeing and store it on my server.

Native.

### Samba

[Samba](https://www.samba.org/) provides shared network storage, making it easy to copy files to and from the server, or share important documents with my wife. It’s funny how a file sharing protocol first developed in the 1980s and most associated with Windows remains the best option for cross-platform file sharing on a LAN.

Native.

### slskd

[IYKYK](https://github.com/slskd/slskd).

Native.

### Snapcast

[Snapcast](https://github.com/snapcast/snapcast) provides whole-house synchronized audio. Think Sonos, but without the expensive vendor lock-in and planned obsolescence. This also deserves its own post. It’s complicated to set up, but it works really well. I eventually plan to purchase some additional devices primarily for use as Snapcast receivers.

Native.

### Syncthing

[Syncthing](https://syncthing.net/) is part of a backup strategy for my photos in Immich. I’ve set up a one-way sync (receive only) on my Macbook, so that a copy of all the photos in Immich eventually make their way to my Macbook’s external drive.

Native.

### Transmission Daemon

This one is also [self-explanatory](https://github.com/transmission/transmission/).

Native.

### Uptime Kuma

After a few instances of my wife saying “X isn’t working” and needing to restart a service manually, I realized I needed a way to keep track of what services were running, or not. [Uptime Kuma](https://github.com/louislam/uptime-kuma) is simple but has a useful set of features. I can ping devices to ensure they’re online, check the status of Podman containers, look for HTTP 2XX response codes from various services, or even parse a JSON payload and extract some details for the health check. If something goes down, I can send a Gotify notification (after a few retries, of course).

Podman.

### Vaultwarden

Self-hosted version of Bitwarden. For years, I used [KeePass](https://keepass.info/), but never had a good syncing solution, so just avoided logging into things on my phone. Meanwhile, my wife used LastPass, which had various breaches and incidents, besides autofill just not working very well. Migrating her to KeePass was not practical, and I settled on [VaultWarden](https://github.com/dani-garcia/vaultwarden), which is an open-source API-compatible reimplementation of BitWarden that works with the official BitWarden clients. I’m not a huge fan of the BitWarden browser extension, but it’s mostly fine. The Android application is good enough, and syncs seamlessly and automatically when I’m connected to my LAN; otherwise, I can connect via VPN and manually trigger a sync if needed.

I am a little concerned that Bitwarden may eventually become hostile to VaultWarden and break the client compatibility. At this point, the only other major Bitwarden compatible client I’m aware of is [Keyguard](https://github.com/AChep/keyguard-app), which does not have an open source license and uses a freemium model.

Podman.

### Vikunja

I’ve been a Trello user for about a decade. But after Trello’s [disastrous multi-day outage](https://trello.status.atlassian.com/incidents/h11gfkgvjhhk) earlier this year, I decided to find a self-hosted alternative. I ruled out anything that involved spinning up a PostgreSQL or MySQL database for a service that will serve two users. [Vikunja](https://vikunja.io/) uses SQLite by default, though it can be configured to use other database backends. It’s not perfect, and the Android application has some missing features, but it’s good enough for me. Single binary deployment? Yes, please.

Podman.

### Yubal

Simple web application to download YouTube Music playlists. I normally use [yt-dlp](https://github.com/yt-dlp/yt-dlp) for downloading YouTube videos, or [NewPipe](https://newpipe.net/) for watching on my phone, but [Yubal](https://github.com/guillevc/yubal) will properly name, organize, and tag music downloaded from YouTube Music.

Podman.

## Non-Services

Here are a few things I don’t self-host, tried but decided against, or am unlikely to try.

### The \*arr Stack

[Sonarr, Lidarr, Radarr](https://wiki.servarr.com/), etc. Every time I read about setting it up, it just sounds like a resource-intensive mess that’s fundamentally prone to frequent breakage. That’s the opposite of what I’m looking for when choosing self-hosted services.

### Collabora Online

While it would be nice to completely part ways with Google, it’s hard to beat Google Docs as a highly convenient and easily accessible way to store and share information. Still, I choose LibreOffice Writer and Calc unless I have a specific reason why the document needs to be in the cloud. Since I already have a shared network drive, it seems pointless to run a service like [Collabora Online](https://www.collaboraonline.com/) (a web-based office suite) instead of just using LibreOffice locally.

### Firefly III

I tried [Firefly III](https://www.firefly-iii.org/) before Actual Budget, thinking I needed built-in support for multiple currencies. It was a complex setup, involving multiple containers, including a container just for importing data. Actual Budget was much simpler to set up and use. For my foreign currency accounts, I have my own simple script to handle exchange rate conversion, so everything in Actual Budget stays in USD.

### Owntracks

I like Google Maps Timeline, but after Google limited the location data to the individual device, I hoped to find a self-hosted version. But I could never get the [OwnTracks](https://owntracks.org/) Android application to consistently report my position, no matter how many battery or location settings I tried. [Dawarich](https://dawarich.app/docs/intro/) is another option in this space, but it wants to run “at least” four containers (PostgreSQL, Redis, Sidekiq, web application). For me, that’s not a compelling trade of resources required versus value I’m likely to obtain.

### Music Assistant

[Music Assistant](https://www.music-assistant.io/) might work better for you if you don’t already have a music setup you like. In my case, I already had Gonic, Snapcast, and MPD working well. Music Assistant felt like it wanted to “own” everything; for example, it seemed to continually rearrange my Snapcast device groups.

### E-mail

Self-hosting e-mail seems like a lot of work for very little benefit. I cannot risk not being able to send or receive e-mails. Eventually I would like to transition away from Gmail to a paid provider.

## Conclusion

That’s 17 native services and 11 Podman services, all managed by systemd. `htop` currently shows about 7.5 GB of memory in use, and CPUs fluctuating between 30% and 40%. It turns out this Mini PC can run a lot! Given how expensive computer hardware has become due to the AI crunch, I hope we’ll continue to see self-hostable projects that prioritize simplicity and efficiency.
