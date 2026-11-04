---
title: So my ISP was hijacking my DNS
link: https://karthihegde.dev/posts/dns_tampering
source: lobste-rs
published: 2026-11-04T12:06:23Z
updated: 2026-11-04T12:06:23Z
first_seen: 2026-11-04T21:49:17.908350494Z
authors:
- karthihegde.dev by dracarys_18
labels:
- networking
summary: Comments
content: extracted
html: 2026-11-04-so-my-isp-was-hijacking-my-dns.html
remote_preview:
  url: https://www.karthihegde.dev/posts/assets/dns/plaintext_dns.png
  alt: A plaintext DNS query returning my ISP's IP address
---

It was just a boring Saturday afternoon, and I thought I'd fix some of the problems with my homelab. One of those problems was that an external site my Podman container was trying to talk to just wasn't working.

I kept getting "Site cannot be reached" almost immediately. This made me suspect something was wrong with my DNS setup, so I started doing a `dig` on the domain. I tried different providers, Google DNS, Cloudflare, pretty much everyone. Almost all of them returned the same IP address. But as soon as I tried Cloudflare's DoH endpoint, I got a completely different IP address.

"Huh?" was my exact reaction.

I took the IP I got from the first DNS resolution and tried to look up who owned it. Lo and behold, it was my goddamn ISP. That's when I realised my ISP was hijacking my DNS queries and returning a tampered IP address.

![A plaintext DNS query returning my ISP's IP address](https://www.karthihegde.dev/posts/assets/dns/plaintext_dns.png) ![The same query over Cloudflare's DoH endpoint returning the real IP address](https://www.karthihegde.dev/posts/assets/dns/encrypted_dns.png) ![IP ownership lookup showing the tampered address belongs to my ISP](https://www.karthihegde.dev/posts/assets/dns/ip_info.png)

Well, the obvious answer was DoT (DNS over TLS) or DoH (DNS over HTTPS). [I made a tweet about this, and it somehow blew up](https://x.com/karthihegde/status/2116533888025874611).

But going through the replies, I noticed quite a few misconceptions and questions that I couldn't answer one by one.

So I figured I'd use this article to address some of those questions and clear up a few misconceptions along the way.

## I self host Unbound I am safe

For those of you who don't know, Unbound is basically a DNS resolver, kind of like what Cloudflare runs at `1.1.1.1`. The difference is, you can run it locally. It works by querying DNS servers to figure out the IP address for a domain, starting at the root nameservers and eventually making its way to the domain's authoritative nameserver.

Now here's the catch. DNS resolution doesn't happen over a single connection. Your resolver might have to talk to multiple nameservers to figure out an IP address, and these queries are traditionally sent over plaintext DNS, usually UDP. Which means, you guessed it, your ISP can snoop on or even tamper with your DNS traffic.

But you can also configure a [forward-zone](https://unbound.docs.nlnetlabs.nl/en/latest/manpages/unbound.conf.html#forward-zone-options) in Unbound. Instead of resolving DNS queries itself, Unbound can forward them to another DNS resolver. And if that resolver supports DoT, the connection to it is encrypted, which means your ISP can't casually snoop on or tamper with your DNS queries in transit.

## Just use DNSSEC

It's understandable why someone would assume DNSSEC is what protects them here. I mean, it has DNS and Security right in the name. And in a way, it does, but it works through verification, not encryption.

You see, DNSSEC doesn't encrypt your DNS traffic before sending it to a resolver. It uses cryptographic signatures to verify that the DNS data is authentic and hasn't been tampered with. But the problem here remains the same: your DNS traffic is still unencrypted in transit. Which means, your ISP can still snoop on it or mess with the responses.

And for DNSSEC validation to work, the domain you're querying needs to have DNSSEC properly configured, with valid signatures and a chain of trust. And here's another problem: [according to the October 2026 Defaults.Exposed census](https://defaults.exposed/methodology/dnssec), only 7.03% of graded domains have a valid, fully validating DNSSEC chain.

If a tampered response fails DNSSEC validation, your resolver will return SERVFAIL instead of handing you that bogus IP address.

## DoH and DoT are not Tamper free

And yes, this is true. DoT and DoH rely on you trusting the DNS resolver you're sending your queries to. They encrypt the connection, so your ISP can't casually snoop on or tamper with your DNS traffic in transit. But that doesn't mean the resolver itself can't mess with your responses. You're still putting your trust in whoever runs that DNS server.

## Your ISP can snoop on the sites regardless

Indeed. Although your regular internet traffic is encrypted with TLS, your ISP can see the hostname during the TLS handshake (SNI). They can also figure out which site you're visiting just from the destination IP sometimes.

Unless, of course, you tunnel your traffic through a VPN. I use Mullvad through Tailscale's exit node feature.

Of course, this just shifts some of the trust to your VPN provider. But that's a story for another day.

## So what does my setup look like?

I've been using Pi-hole/AdGuard for over 3 years now. That's actually what got me into self-hosting in the first place.

But there's another reason I don't route every single container through my local DNS server. When I'm updating containers, if something goes wrong and my DNS server goes down, I don't want that to take down DNS resolution for other containers that depend on it. So for some containers, I wanted an external DNS resolver as a fallback.

Right now, I use AdGuard for my DNS, with `dnscrypt-proxy` handling the upstream DNS queries over DNSCrypt, so my DNS traffic is encrypted in transit. For containers where I really can't afford DNS failures, I also have Cloudflare configured as a fallback. My local AdGuard is still the primary DNS for them, though.

I also run NixOS on my servers, and my [dotfiles](https://github.com/dracarys18/dotfiles) are open source if you're interested
