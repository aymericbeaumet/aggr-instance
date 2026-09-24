---
title: Making Tailscale Faster
link: https://tailscale.com/blog/making-tailscale-faster
source: hnrss-org-frontpage
published: 2026-09-23T17:49:07Z
updated: 2026-09-23T17:49:07Z
first_seen: 2026-09-24T06:05:26.292611038Z
authors:
- yarapavan
summary: 'Article URL: https://tailscale.com/blog/making-tailscale-faster Comments URL: https://news.ycombinator.com/item?id=49819880 Points: 125 # Comments: 46'
content: extracted
html: 2026-09-23-making-tailscale-faster.html
preview:
  file: 2026-09-23-making-tailscale-faster.preview-fc4120663c11.webp
  width: 256
  height: 132
  color: '#9e4a17'
images:
- source: https://cdn.sanity.io/images/w77i7m8x/production/a61f95892ebd9647722faaff16ac3a9f6787490a-2304x1188.png
  original:
    file: 2026-09-23-making-tailscale-faster.image-a61f95892ebd.png
    width: 2304
    height: 1188
  variants:
  - file: 2026-09-23-making-tailscale-faster.image-4047e4aaf9b6.webp
    width: 320
    height: 165
  - file: 2026-09-23-making-tailscale-faster.image-81f68d02ea7e.webp
    width: 640
    height: 330
  - file: 2026-09-23-making-tailscale-faster.image-912c210aab65.webp
    width: 960
    height: 495
  - file: 2026-09-23-making-tailscale-faster.image-34a470a6d819.webp
    width: 1280
    height: 660
  - file: 2026-09-23-making-tailscale-faster.image-f11dc259cedf.webp
    width: 1600
    height: 825
  - file: 2026-09-23-making-tailscale-faster.image-1ad53a7fc58a.webp
    width: 2304
    height: 1188
  color: '#983705'
- source: https://cdn.sanity.io/images/w77i7m8x/production/424a76b03245b737cf8e2c85567ba0e1eadb1b12-1920x1080.svg?w=3840&q=75&fit=clip&auto=format
  original:
    file: 2026-09-23-making-tailscale-faster.image-f910dfdf09d2.png
    width: 1600
    height: 900
  variants:
  - file: 2026-09-23-making-tailscale-faster.image-166b8fbfff2f.webp
    width: 320
    height: 180
  - file: 2026-09-23-making-tailscale-faster.image-7405f8a65948.webp
    width: 640
    height: 360
  - file: 2026-09-23-making-tailscale-faster.image-0f2d3c8d23ae.webp
    width: 960
    height: 540
  - file: 2026-09-23-making-tailscale-faster.image-d3f252055e75.webp
    width: 1280
    height: 720
  - file: 2026-09-23-making-tailscale-faster.image-b675cbbf84b7.webp
    width: 1600
    height: 900
  color: '#5a2109'
- source: https://cdn.sanity.io/images/w77i7m8x/production/9949e454e4e84650f52821190d44aebd907262cf-1920x1080.svg?w=3840&q=75&fit=clip&auto=format
  original:
    file: 2026-09-23-making-tailscale-faster.image-f3ddd5933fd4.png
    width: 1600
    height: 900
  variants:
  - file: 2026-09-23-making-tailscale-faster.image-3c66529d81bf.webp
    width: 320
    height: 180
  - file: 2026-09-23-making-tailscale-faster.image-001a7df883af.webp
    width: 640
    height: 360
  - file: 2026-09-23-making-tailscale-faster.image-e5274d55ffa6.webp
    width: 960
    height: 540
  - file: 2026-09-23-making-tailscale-faster.image-a2b06ec3838a.webp
    width: 1280
    height: 720
  - file: 2026-09-23-making-tailscale-faster.image-9a9c6495e19f.webp
    width: 1600
    height: 900
  color: '#66260a'
---

If you’ve been following Tailscale at all, you know we’re really just a bunch of geeks who care a lot about internet connectivity. One thing we love to talk about is NAT Traversal. That’s one of the core value-adds with Tailscale: [we tamed NAT](https://tailscale.com/blog/nat-traversal-improvements-pt3-looking-ahead). Not every network is friendly, but Tailscale can still find a path in a wide range of conditions. That’s not the only important thing for an internet protocol: the data plane also has to be performant.

Over the years we’ve been investing in making Tailscale fast. We started by [increasing TCP throughput on Linux devices](https://tailscale.com/blog/throughput-improvements). Then we made significant breakthroughs in wireguard-go to [surpass 10Gb/s](https://tailscale.com/blog/more-throughput) on bare metal. We later leveraged segmentation offloads to [increase throughput over 4x](https://tailscale.com/blog/quic-udp-throughput) for UDP-based applications. Alongside these improvements to our data plane, we built primitives like [Tailscale Peer Relays](https://tailscale.com/blog/peer-relays-beta), which can [improve network performance in tricky conditions](https://tailscale.com/blog/peer-relays-international-networks).

All of this has made Tailscale practical for more performance-sensitive workloads. It means you can use Tailscale for continuous integration, agentic workflows, remote development environments, robotic edge devices, heavy data and telemetry workloads, and more. Tailscale helps those devices connect across a wide range of network conditions.

So yeah, we think Tailscale is fast. But we also think we can make it faster.

Today we’ll detail how we’re boosting throughput for app connectors, subnet routers, and exit nodes, with some multi-queue technology (landing in the second half of 2026). We’ll also preview some throughput and memory overhead improvements we’re deploying in upcoming stable client releases. And we’ll look at some performance tooling issues we want to solve for our customers.

## [Less memory overhead for small packets](https://tailscale.com/blog/making-tailscale-faster#less-memory-overhead-for-small-packets)

Most network packets are tiny, like 1 KiB. But to use Linux’s most efficient throughput tools, like Generic Receive Offload (GRO), Tailscale has to be ready to accept 64 KiB of traffic at once. It’s a bit like container shipping: the ports, ships, and trucks are built for one container shape, however full it happens to be.

Tailscale has to unpack those containers—every packet gets decrypted and delivered on its own. The wireguard-go implementation that informs Tailscale’s cryptography and networking essentials, only offers one 64 KiB buffer size to unpack into. So a 1 KiB packet is copied into its own 64 KiB buffer, every time. That’s a rich optimization target.

On Linux and Android, Tailscale now leaves those packets where they landed. It identifies where each one starts and ends inside the single large read instead of copying it somewhere new. Small packets stay small in memory, many share one allocation, and they spend less time being copied. In itself, this led to a roughly 5% speed-up in many network configurations.

![Diagram comparing packet buffer allocation before and after optimization. Before: three packets each copied into separate 64 KiB buffers with unused space. After: three packets sized and tagged for destination, consolidated into a single buffer with dividers.](https://cdn.sanity.io/images/w77i7m8x/production/424a76b03245b737cf8e2c85567ba0e1eadb1b12-1920x1080.svg?w=3840&q=75&fit=clip&auto=format)

Separately, we shortened packet queues—the lines packets wait in between stages of the pipeline. The queues are there to absorb bursts of traffic. Testing showed that most of that depth went unused, while shorter queues meant less waiting time and less memory overhead.

What do we do with all that freed-up memory space? We passed the savings on to some of the hardest-working nodes: subnet routers and app connectors.

## [Multi-queue for subnet routers, app connectors, and exit nodes](https://tailscale.com/blog/making-tailscale-faster#multi-queue-for-subnet-routers-app-connectors-and-exit-nodes)

[Subnet routers](https://tailscale.com/docs/features/subnet-routers) can look completely different across different tailnets. For someone running a small homelab network, a subnet router can easily handle a small set of `192.168.x.y` non-Tailscale devices. A subnet router that fronts a cloud deployment, one with hundreds of peers, will carry substantially more traffic.

Until recently, subnet routers, app connectors, and exit nodes processed packets for multiple independent streams in one ordered, single-thread pipeline. That meant a single lane was shared across many connections, because a receiving application must never see its own packets arrive out of order.

Having reduced our memory footprint, we had capacity to implement a multi-queue system: several lanes instead of one, scaled to the machine’s resources rather than the number of peers. Each stream of packets gets a lane and stays there, while the lanes run in parallel, allowing work to spread across CPU cores.

![Diagram comparing single pipeline vs multi-queue processing architecture. Before: packets flow through one Reader to four Crypto stages then Writer to Devices. After: packets distributed across four parallel Reader-Crypto-Writer pipelines to Devices.](https://cdn.sanity.io/images/w77i7m8x/production/9949e454e4e84650f52821190d44aebd907262cf-1920x1080.svg?w=3840&q=75&fit=clip&auto=format)

It results in higher aggregate capacity and lower delay between receiving and forwarding packets for subnet routers and app connectors. Hardware you already have gets used more efficiently. App connectors and exit nodes, typically serving many users with short-lived connections, get a particularly noticeable boost.

“This translates into lower latency, essentially faster processing of data from the moment we read it off the wire to the moment we send it to the OS,” said Alex Valiushko, member of technical staff at Tailscale.

## [Throughput gains with `writev`](https://tailscale.com/blog/making-tailscale-faster#throughput-gains-with-writev)

Taking advantage of [Linux’s `writev` capabilities](https://linux.die.net/man/2/writev) in the Tailscale client, Tailscale can pass multiple pieces of packet data to the Linux kernel in one operation, rather than having to copy and combine those pieces before passing them to the kernel. The v in `writev` stands for “vector”: Tailscale can describe separate pieces of data that need to be moved, without moving them. It means fewer copies of packet data in memory, fewer write operations, and higher throughput.

## [Faster startup with netmap caching](https://tailscale.com/blog/making-tailscale-faster#faster-startup-with-netmap-caching)

For now, these speed-ups are available only on Linux and, where applicable, Android systems. But we’ve also been working on features that apply to other systems. Tailscale clients will soon be able to use **netmap caching** to start more quickly in many conditions.

A machine connecting to Tailscale usually starts by connecting to Tailscale’s control plane, in something like 100 milliseconds on a typical network. The machine authenticates and gets a "network map" (netmap) describing the devices it can reach and how to reach them. This startup process should feel fast, maybe instantaneous, and with a good network connection, it typically does.

But when you’re on bad airplane Wi-Fi, or inside a hotel with aggressive filtering, or other not-great connectivity setups, it can take a while for the machine to reach the control plane—and sometimes you may not be able to reach it at all. It’s often not obvious where the problem is, but the effect is that you can’t reach other devices.

Even under ideal network conditions, 100 milliseconds of startup latency may be too much for some latency-sensitive workloads.

Netmap caching helps machines get connected when the control plane is not quickly reachable. When it’s enabled, each device on your tailnet stores a copy of the netmap on disk. When a device starts up, it can use that cached copy to establish connections with other devices on the tailnet, until it’s able to contact the control plane to get the latest info. (These connections are negotiated between the devices directly, and Tailscale does not see any of the traffic, as usual).

“Bad network conditions—that’s really the space where people can get a lot of utility out of netmap caching,” said Claus Lensbøl, member of technical staff. “\[A device client says\], ‘You know what? We haven’t talked to control yet. We’ll probably get there soon. In the meantime, you can still start doing something.’”

There are a few limitations. Caching can only work if the device has previously connected to the tailnet at least once, to fetch a network map from the control plane. In addition, netmap caching requires the device to have persistent disk space to store the cache. We’ve taken care to minimize unnecessary disk writes, but in some cases you may not want to enable it. For example, on exceptionally large tailnets, updating a cache may require a lot of disk traffic. Likewise, devices that use slow or wear-sensitive storage like SD cards may prefer not to enable netmap caching.

For most devices on most tailnets, though, this feature can notably speed up how quickly devices can establish contact with each other at startup. We’ve seen tailnets with poor control plane reachability start sending through the data plane, on a “warm” cache start, one to two orders of magnitude faster than from a “cold” start. For devices facing variable startup latency, or far away from a DERP server or the control plane, the benefits are particularly tangible.

## [When you can see all these speed-ups](https://tailscale.com/blog/making-tailscale-faster#when-you-can-see-all-these-speed-ups)

- **Memory reduction** via buffer changes (Linux/Android) is expected in the v1.104 client.
- **Multi-queue** to benefit subnet routers and app connectors is planned for a release after v1.104.
- **Throughput gains** (Linux/Android) were partially implemented in spring 2026; leveraging the additional gains in memory and throughput is planned for a release after v1.104.
- **Netmap caching** is available as a feature flag in the current Tailscale client; it is expected to arrive by default in v1.104, following further testing. Mobile clients are expected to have the feature in a release after v1.104.

## [Performance is still difficult to diagnose and test](https://tailscale.com/blog/making-tailscale-faster#performance-is-still-difficult-to-diagnose-and-test)

Sure, we think Tailscale is fast. But you shouldn’t have to trust us on that. That’s why we’re exploring a Tailscale-aware monitoring and testing toolkit. We want to give our customers the tooling they need to test, diagnose, and understand their network configuration, in a way that’s Tailscale-native.

Here are the gaps we see in modern performance testing:

- **Distribution tax:** Most performance tooling is point-to-point, and requires you to install something on every endpoint.
- **Workflows are rigid:** It’s pretty easy to run the wrong test, get the wrong output, and chase a problem that’s not there.
- **Protocol support:** Many tools don’t support newer protocols, such as QUIC and HTTP/3.
- **Tailscale-awareness:** General-purpose tooling is not Tailscale-native. It can’t tell you if a connection is using DERP or is direct, whether a peer relay might help, or how the connection path changes over time.

Existing tooling doesn’t understand Tailscale-native paths and states. So we’re exploring tooling that does. [Help us shape the future](https://tailscale.typeform.com/performance) of performance testing at Tailscale.
