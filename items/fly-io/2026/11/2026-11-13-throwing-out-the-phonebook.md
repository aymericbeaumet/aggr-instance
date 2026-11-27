---
title: Throwing Out The Phonebook
link: https://fly.io/blog/throwing-out-the-phonebook/
source: fly-io
published: 2026-11-13T00:00:00Z
updated: 2026-11-16T04:21:03Z
first_seen: 2026-11-27T00:01:05.630507547Z
content: extracted
html: 2026-11-13-throwing-out-the-phonebook.html
remote_preview:
  url: https://fly.io/blog/throwing-out-the-phonebook/assets/frankie-on-the-phone-with-servers-c20b.webp
  alt: Frankie the hot air balloon on a corded phone above an open phonebook, with servers linked across the clouds
extra:
  thumbnail: https://fly.io/blog/throwing-out-the-phonebook/assets/frankie-on-the-phone-with-servers-c20b.webp
---

Author

![Daniel Botha](https://fly.io/static/images/danielb.webp)

Name

Daniel Botha

Author

![Peter Cai](https://fly.io/static/images/peter.webp)

Name

Peter Cai

@PeterCxy

[@PeterCxy](https://comfy.social/@PeterCxy)

![Frankie the hot air balloon on a corded phone above an open phonebook, with servers linked across the clouds](https://fly.io/blog/throwing-out-the-phonebook/assets/frankie-on-the-phone-with-servers-c20b.webp)\
 Image by [Annie Ruygt](https://annieruygtillustration.com/)

There’s really only one hard problem in software engineering: synchronization. OK, maybe two: cache invalidation and synchronization. Synchronization is the harder one though, especially when the thing on the other side is eventually consistent. This year we rebuilt 6PN, the private network between your Fly Machines, and we did it by avoiding the hard thing.

To start, here’s the status quo: every Fly Machine gets a private IPv6 address, and every other Machine in your organization can reach it. We call that network 6PN. It’s what `my-db.internal` points at, and it’s how your app server talks to your database without either of them touching the public internet.

Until this year, every host in our fleet carried a phonebook: a map of every Fly Machine that had ever moved, and where it lives now. Hundreds of thousands of entries, on every host, kept up to date in real time. Phonebooks are supposed to fit inside your house, and this was getting, uh…unwieldy. So we changed it! The new design keeps, in total, somewhere between 100 and 200 records. There’s no clever memorization mechanics and when a Machine calls a number it doesn’t know, the host looks it up, writes it down, and moves on.

Here’s the story of how we got into the phonebook business, and how we got out by the skin of our teeth. (It wasn’t that dramatic, but this design does work better.)

## The address is the route

A 6PN address isn’t random. It carries its own routing information:

```wrapper
fdaa:netid:netid:hostid:hostid:mach:mach:2
```

`netid` is your organization’s private network. `hostid` is the physical server the Machine was born on. `mach` is the Machine. Our servers talk to each other over a [fully connected WireGuard mesh](https://fly.io/blog/corrosion/ ""), and each host owns a `/48` on it: `fdaa:hostid:hostid::/48`.

You’ll notice the host ID sits in the wrong place for that. So `udp-xdp`, an eBPF program hooked onto every Machine’s network interface, shuffled the bytes on the way out:

```wrapper
fdaa:netid:netid:hostid:hostid:mach:mach:2   # what the Machine sees
fdaa:hostid:hostid:netid:netid:mach:mach:2   # what goes over the wire
```

Now the packet matches a kernel route for the destination host, WireGuard carries it there, and the far side swaps it back. There aren’t any lookups (or state, or databases). The address is the route.

A truly elegant design! Until a Machine moves.

## Machines learn to move

For a long list of reasons, sometimes we need to migrate Machines between hosts. A Machine that moves to a new host still has the old host’s ID baked into its address, which meant that it was pointing in the wrong direction.

Our first fix was `dnat6pn`: remember every address a Machine has ever had, and rewrite traffic aimed at an old one to the new one. That table grew faster than a 12th century rat population, and it was equally tricky to manage. The address a Machine saw on its own interface wasn’t the address other Machines used to reach it. Move an app to a different organization and it fell apart entirely. It even ended up breaking our own apps. It was strapped on top of the swapping design rather than built into it. Mistakes had, indeed, been made.

So we flipped it around. A Machine’s canonical address became its *original* address, forever, no matter where it lives. `udp-xdp` took on translating every 6PN address into a “routable” one, with the host ID replaced by whichever host the Machine is on *right now*.

Which means `udp-xdp`, on every host, needed to know where every migrated Machine in the fleet currently lives.

That’s the phonebook. A BPF map on every host, fed by [Corrosion](https://fly.io/blog/corrosion/ ""), our gossip-replicated SQLite, through a subscription to every Machine update across the whole fleet.

## A phonebook in Elvish

All this worked…mostly.

**Life in a perpetual growth spurt.** eBPF can’t call out to userspace to fetch an entry when it needs one, so every entry has to be loaded ahead of time. The map grew steadily with no sign of slowing down. When we last looked, we were within the same order of magnitude as the cap we’d set on it.

**Every host had a different phonebook.** Every host was supposed to hold the same map. They held vastly different numbers of entries. There’s only one way to read that: synchronization wasn’t working, and we couldn’t tell you where.

**We couldn’t see it.** We had incidents where syncing Corrosion into the map broke. But we had no good way to monitor the map itself, so if it caused problems we didn’t hear about directly, we wouldn’t have known. And when we did go looking, the rewriting made debugging miserable: the address on the sending end and the address on the receiving end were different, so nothing lined up.

**It couldn’t route around anything.** Traffic over the WireGuard mesh went to the host in the address, full stop. Teaching it to take a detour around a bad link meant more logic in `udp-xdp`, the component we already couldn’t debug.

Sadly, you can’t fix a fundamental design flaw with a patch. Holding a real-time copy of an eventually consistent database, on every host in the fleet, inside the kernel; that’s the hardest kind of problem, and we were kind of sick of holding it.

## The network already asks

For unrelated reasons (Egress IPs), we’d ended up running a Layer 2 network, VXLAN, on top of the WireGuard mesh.

VXLAN gives you an Ethernet segment over any IP network; all it needs is a UDP port. On a real Ethernet segment, a machine that doesn’t know where an address lives shouts for it with ARP or Neighbor Discovery, and someone answers. VXLAN can’t shout across an IP network. You either tell it about every peer in advance, or you fake the answer.

If we have to tell it about every peer in advance, we’re back in the phonebook industry. So we fake it. When VXLAN sees an address it can’t map to a peer, it emits a netlink event, and `drift-agent` listens for those and configures the peer on the spot.

Drift is another clever internal tool. Look out for an upcoming post that will tell you all about it.

Here’s the part that matters. That “I don’t know this address” event fires for *any* unknown address VXLAN sees. Not just VXLAN peers. Anything. We’d built a mechanism that tells us, lazily and precisely, the moment a host needs to know something it doesn’t.

That hefty phonebook is now smart directory assistance.

## Ask, write it down, move on

`s6pnforwarderd` is the daemon that used to load every migrated Machine into `udp-xdp`‘s map. Now it does almost nothing until someone asks.

All machine 6PN traffic goes to its own routing table, `0xfdaa`, which starts out with one rule:

```wrapper
fdaa::/16 dev vxlan0
```

When a Machine sends a packet to another Machine’s 6PN address, `vxlan0` has no idea where that is. It only knows about hosts. So it raises the “who is this?” event. `drift-agent` handles host peers itself and passes everything else on over an HTTP SSE stream. `s6pnforwarderd` is subscribed to that stream. It asks Corrosion’s local SQLite two questions: does this 6PN address exist, and which host is that Machine on right now? Then it writes one route:

```wrapper
fdaa:netid:netid:hostid:hostid:mach:mach:2 via fdaf:newhostid:newhostid:: dev vxlan0
```

Every packet after that goes straight through. Every so often, `s6pnforwarderd` re-checks the routes it has loaded in case a Machine has moved, and it uses Corrosion’s lightweight per-row update notifications to notice sooner. That part’s optional; the periodic check covers it.

No translation needs to happen either. The address the Machine sees on its interface is the address on the wire is the address at the other end. When something goes wrong, both sides finally agree on what they’re looking at.

## What changed

- **The table is tiny.** On most hosts, `0xfdaa` holds somewhere between 100 and 200 routes. The old map held hundreds of thousands.
- **There’s no fleet-wide subscription.** A host learns about the Machines its Machines actually talk to, and nothing else.
- **Nothing is permanently fatal.** Corrosion propagation is slow? A SQL query fails? The route gets re-checked on the next loop. And if a lookup fails outright, the sending Machine will try again anyway (TCP retransmits, that’s its whole personality), which fires another event, which triggers another lookup.
- **We can finally see it.** We’ve put real work into monitoring for when this goes wrong, because we’ve essentially scoped down what each host cares about. Much more of this logic now uses standard kernel components and an observable userspace control plane. We had nothing equivalent for the old map.

The `via` host doesn’t have to be the host running the Machine. With forwarding on, a host that gets traffic for a Machine it no longer has can pass it to the host that does. Instead of breaking the connection, a stale route just needs an extra hop and voila. It also means we can send traffic around a bad link on purpose.

## Safer too

We didn’t delete `udp-xdp`. We let it keep doing its one job, since it’s fairly good at it: checking that the source and destination 6PN addresses belong to the same network. Your Machines can reach your Machines and nobody else’s.

When `udp-xdp` validates a packet, it stamps it with a `0xfdaa` fwmark, and only stamped packets get routed through the `0xfdaa` table to `vxlan0`. That order is the important bit. If `udp-xdp` fails to load, nothing gets stamped, and nothing crosses. The network fails closed. If you do it the other way around (route everything, then have a filter drop the bad stuff), then a filter that fails to load means that everything crosses.

This check also doesn’t fit well in `nftables`, because it compares fields from the packet against each other. In `udp-xdp` it’s a few lines of code that depend on no external data at all. That’s the kind of thing that belongs in eBPF. Complex logic and a synchronized copy of the fleet don’t, and longer term we’d like to move UDP encapsulation out of there too.

## What it costs

Lazy lookups have a price. In this case, it’s the first packet that’s forking over the toll.

The lookup itself is fast, usually under 100ms on our side, mostly database I/O. But the first packet to a Machine that your Machine has never talked to arrives at a host that doesn’t have a route yet. With TCP, the retry is up to your Machine’s kernel, and Linux’s default retransmission timeout is one second. So the first connection between two Machines that have never spoken can take about a second. Every connection after that goes straight through.

We’re going to fix that, most likely by holding the first few packets (up to a cap) while the lookup runs, instead of letting them fall on the floor.

The other cost: if `drift-agent` or `s6pnforwarderd` crashes, routes that already exist keep working. What breaks is new conversations: Machines that have never talked to each other, and Machines that just migrated. The second one isn’t new; a freshly moved Machine was a failure mode under the old design too. The first one is new, but compared to loading every route in the fleet onto every host, all the time, well…this is the lesser evil we’ve chosen.

## Changing the engine at cruising altitude

You don’t get to schedule a maintenance window for every private network on the platform. So we didn’t swap one system for the other. We ran both, and let each host opt in.

The new VXLAN path only touches packets carrying the `0xfdaa` fwmark, and `udp-xdp` only sets that mark when all of these are true:

1. The new `s6pnforwarderd` and `drift-agent` are running on the host.
2. `s6pnforwarderd` has flipped a feature flag in `udp-xdp`.
3. The packet is outgoing 6PN traffic headed for another host, or it’s incoming traffic that arrived on `vxlan0`, and specifically *not* on `wg0`.

That last condition is what made it safe. A host on the new system sends over VXLAN. If the destination host hasn’t been upgraded yet, it sends its replies the old way over `wg0`, where the swapping code still applies and nothing gives the packet the new mark. We kept the old map up to date the whole time, so old-style hosts kept working.

The route is asymmetric for a while: out over VXLAN, back over WireGuard. That’s fine. We told the kernel to accept it.

Then we turned it on, host by host, across the fleet. The rollout wasn’t noticeable in most regions; during the trial we had some temporary Flycast trouble in `yyz`. Once every host was on the new path, we deleted the old one. As of last month, the swapping-based 6PN code is gone.

## Rerouting, on purpose

Because a route is just “send this to that host over VXLAN”, the next host doesn’t have to be the final one. That means we can send traffic around a degraded link, and build highly available gateways that don’t each need a full copy of the fleet.

If you’ve been on Fly.io a while, rerouting might sound familiar. `fly-proxy`, our load balancer, has done it for years. But it only ever covered traffic coming in through Anycast and Flycast, and it took what felt like a thousand state machines in the load balancer to pull off.

Rerouting in the network itself works for basically everything, and it’s a lot simpler. So the `fly-proxy` version is going to be put out to pasture soon. It was rerouting at the wrong layer.

The old design knew where everything was, all the time, but it still couldn’t tell us when it was wrong. The new one knows almost nothing, and it’s the first version of 6PN we can trust to tell us when it breaks.

Good riddance to the phonebook! Turns out you can just call the operator and ask.

Next post ↑

[Petsem: Sometimes Dead Is Better](https://fly.io/blog/petsem/)

Previous post ↓

[Well, That Wasn't Fun: October 16 '26](https://fly.io/blog/well-that-wasnt-fun/)
