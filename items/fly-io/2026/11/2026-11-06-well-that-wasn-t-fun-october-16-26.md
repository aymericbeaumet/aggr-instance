---
title: 'Well, That Wasn''t Fun: October 16 ''26'
link: https://fly.io/blog/well-that-wasnt-fun/
source: fly-io
published: 2026-11-06T00:00:00Z
updated: 2026-11-10T17:55:56Z
first_seen: 2026-11-27T00:01:05.630507547Z
content: extracted
html: 2026-11-06-well-that-wasn-t-fun-october-16-26.html
remote_preview:
  url: https://fly.io/blog/well-that-wasnt-fun/assets/robot-bugs-firing-lasers-at-a-disk-eb13.webp
  alt: Three robot bugs firing pink lasers at a disk in a black void
extra:
  thumbnail: https://fly.io/blog/well-that-wasnt-fun/assets/robot-bugs-firing-lasers-at-a-disk-eb13.webp
---

Author

![Daniel Botha](https://fly.io/static/images/danielb.webp)

Name

Daniel Botha

Author

![Jae Beller](https://fly.io/static/images/jae.webp)

Name

Jae Beller

![Three robot bugs firing pink lasers at a disk in a black void](https://fly.io/blog/well-that-wasnt-fun/assets/robot-bugs-firing-lasers-at-a-disk-eb13.webp)\
 Image by [Annie Ruygt](https://annieruygtillustration.com/)

October 16, 2026 began like any other day at Fly.io (meaning, we were anticipating only a *medium* amount of chaos). By that evening, we had patched and rebooted our entire fleet in response to [a Linux kernel bug](https://www.cve.org/CVERecord/?id=CVE-2026-99999). The full rollout took 10 hours, but the emotional damage persists.

## CVE-2026-99999

The rest of this won’t make much sense without a clear read on the bug itself, so: the [Linux kernel CVE team](https://lore.kernel.org/linux-cve-announce/2026101533-CVE-2026-99999-beee@gregkh/ "") and [pwn.ai](https://pwn.ai/blog/pwnescape "") announced CVE-2026-99999 on the morning of the 15th. We heard about it that night, and started working on mitigations.

## IP sleight of hand

Protocols like TCP and UDP include [checksums](https://en.wikipedia.org/wiki/Checksum "") to detect corruption on the way across the internet. Calculating one means reading the whole message, which adds up at high speeds. Linux avoids that with [checksum offload](https://docs.kernel.org/networking/checksum-offloads.html ""): instead of calculating the checksum immediately, the kernel stores metadata saying where the checksum should read from and write to. If the checksum turns out not to be needed, the kernel never spends the CPU on it.

Not needed, for example, when sending to localhost, or when the network card can add its own checksums.

Checksum offload works the same way for virtual machines. The virtual network card accepts [checksum metadata](https://docs.oasis-open.org/virtio/virtio/v1.3/virtio-v1.3.html#:~:text=5.1.6.2%20Packet%20Transmission "") and passes it to the host. On the host side, the packet is treated like any other offloaded packet, using the metadata the guest provided.

The offload code has checks meant to reject bad metadata. For IPv4, they were wrong. To exploit that, an attacker needs to do three things: point the checksum at a different header field, pick a message that produces the checksum they want, and force the host to calculate the checksum in software. The [pwn.ai post](https://pwn.ai/blog/pwnescape "") goes into detail on pulling that off. The end result: the host receives the packet, parses it, decides where to send it, *then* applies the checksum, in the wrong place.

The attacker now controls two bytes of an IPv4 header the host has already trusted.

Based on network activity during the rollout, we believe it’s unlikely that any of our customers’ data was actually exposed. It is theoretically possible that between disclosure and our final reboot, a malicious app could have read small chunks of network traffic from other apps on the same physical host, but we have no evidence this occurred.

**If you ran on Fly.io between October 15 and 17:** the exploit is hard to detect, so out of caution, rotate any credentials you sent over HTTP or another unencrypted protocol in that window. That includes unencrypted traffic over your 6PN.

## Two bytes is plenty

Our job is to help you run your amazing things, which is why we take [strong hardware virtualization](https://docs.fly.io/reference/architecture "") very seriously. It’s the least flaky way to keep workloads apart from each other. A guest should not be able to influence the host outside of a few narrowly defined paths. A bug like this one punches holes in that. Two holes, in this case.

The first is a firewall bypass. A rewrite that targets the source or destination IP lands after the host has already applied routing and firewall rules. The upstream fix is now in place.

The second is an out-of-bounds read. A rewrite that targets the header length tricks the host into treating adjacent memory as part of the packet. Rewriting the smallest IPv4 header to the largest captures 32 bytes of memory as [options](https://en.wikipedia.org/wiki/Internet_Protocol_Options "") on the outgoing packet. That memory can contain host packets, kernel data structures, and packets belonging to the VM next door. A leak from a neighboring VM is, all by itself, a reason to drop everything.

Detecting this kind of activity is hard. Firewall-bypass packets can be perfectly well-formed. Out-of-bounds-read packets probably carry a bad header checksum once the options are included, but by the time you can see that, it’s already too late. We looked at adapting our existing [XDP](https://fly.io/blog/bpf-xdp-packet-filters-and-udp/ "") rules, but the rewrite happens after they run. Sure, we could disable checksum offload on the host, but that needs a reboot. And if we were going to reboot, we were going to reboot into a patched kernel.

## Ten hours of reboots

Each of our hosts takes around 15 to 20 minutes to reboot (server hardware is cool like that), and every Fly Machine running on it is down for that period. Our placement also scatters Machines randomly across hosts, so we couldn’t reboot more than one host at a time without taking down every Machine some unlucky subset of apps had.

Single-Machine apps were going down no matter what we did.

So, we planned around Managed Postgres. We split each region into waves so that no MPG cluster had both nodes in the same wave. Even with a limited number of MPG clusters, our largest region, `iad`, needed 17 waves.

Of course, computers behaved like computers. Most hosts came back within the estimate we’d announced, but some didn’t. A few rebooted without essential software and needed reinstalls. Others got stuck in BIOS, while a four-node chassis in `gru` lost its power supply mid-incident, with no spare on site. Machines on those hosts stayed down until we restored them on the 20th. But at the end of the day, no Fly Machines or customer data were lost.

## The communication Catch-22

At 16:22 UTC we posted an emergency maintenance notice on the status page and started telling our largest customers. The notice said “maintenance,” not security, and didn’t name the CVE. Folks were not shy about telling us this felt unacceptable, which makes perfect sense. A same-day “emergency maintenance” that reboots your whole app looks like we’ve done something stupid and are patching wildly without telling anyone why.

It didn’t feel great to us either, but telling the internet we were vulnerable (and for how long) felt like a significantly worse idea. This situation was particularly problematic because the bug was disclosed along with a detailed writeup of how to exploit it.

In the moment, we decided it was most important to avoid confirming that our hosts were vulnerable *right now*. Could we have run our communication channels better anyway? *Always.*

As part of the post-mortem, our Head of Security revamped our incident communications plan into an Incident Communications Plan. Previously, the folks in an incident channel would decide on a comms plan, since they were closest to the metal. Now, we’re taking that responsibility off their backs (since they're…actively solving an incident) and putting it on our exec team. Might as well make them useful! They’re all now armed with “Do not pass go” level instructions on exactly who says what, posts where, and when. This should make it easier to get the right level of communication out at the right time when (not if, because something is always going to go sideways) the next thing happens.

## Meanwhile…

While we dealt with…all of that, the world seemed to be catching fire around us. October was a wild ride (not sure we needed an entire Cybersecurity Awareness *Month*, but okay). We have nothing but #HugOps for our industry peers who have had a rougher time. It’s not easy out here in the security trenches right now.

But this is the world in which you (and your agent) are building software. It’s kinda gross. More bugs exist, and they’re getting found faster and exploited more easily. We’re doing what we can on our end, but we wanted to offer up a few things that really do reduce the blast zone if your project is compromised (even if they feel tedious to do in the moment).

- **Encrypt traffic between your own services, even on a private network.** The one piece of advice we had to give after CVE-2026-99999 was “rotate whatever you sent in plaintext.”
- **Give agents credentials scoped to the job, and make them expire.** An agent’s environment is the first thing a successful exploit reads, so a token that can deploy one app for one hour makes your life easier than one that’s org-wide (or, God forbid, that lives forever). If you can [keep credentials away from agents](https://fly.io/connectors/ ""), even better.
- **Assume every boundary leaks a little.** Hardware virtualization is the strongest wall we have, but for a day and a half a guest could still pull 32 bytes at a time through it. Try not to put an agent, its secrets *and* your production data in the same place on the theory that the wall will hold.
- **Treat what the agent reads as hostile.** Obviously *your* agent is an angel, but you never really know who it’s hanging out with. Be a suspicious parent, and only give it the permissions you’d hand over to the author of the worst web pages, issues or emails it might read.
- **Rehearse rotation.** “Rotate your credentials” is easy to say, but like…you have to know where they are, in five seconds. Where are they? Go look.

Nothing here makes anyone invincible, but these can at least be some hard constraints when bad things happen.

So! None of this was fun, and we’ll probably do it all again soon. See you then.

Next post ↑

[Throwing Out The Phonebook](https://fly.io/blog/throwing-out-the-phonebook/)

Previous post ↓

[Feng Shui for Robots: The Fly MCP Server](https://fly.io/blog/feng-shui-for-robots/)
