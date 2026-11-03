---
title: Mac at Any Scale
link: https://namespace.so/blog/mac-at-any-scale
source: hnrss-org
published: 2026-11-03T14:53:42Z
updated: 2026-11-03T14:53:42Z
first_seen: 2026-11-03T20:48:58.593592327Z
authors:
- anner_
content: extracted
html: 2026-11-03-mac-at-any-scale.html
remote_preview:
  url: https://namespace.so/blog-images/mac-at-any-scale/banner.png
---

Chances are [you’ve seen me on X](https://x.com/namespacelabs/status/2091895197151952950): unpacking hundreds of MacBook Pros to deploy in a datacenter. The video hit a nerve: insane enough at the first sight, yet kind of rational if you know what we know now.

> Namespace
>
> @namespacelabs
>
> Unboxing Macs at scale for our new server racks.
>
> [@namespacelabs on X](https://x.com/namespacelabs/status/2091895197151952950)

1K 964 5K 1.8K

So today I want to give some background into what it takes to build custom Mac racks, how we can start macOS instances from scratch in seconds and deliver new Xcode versions [mere hours after Apple releases them](https://x.com/nichtverstehen/status/1932224710928216116).

## So why MacBooks?

Because our customers need Macs. And because Apple didn’t give us another option.

Many of our customers build and test iOS apps. You can't build iOS apps without a Mac. The iOS Simulator only runs on a Mac. Moreover, in order to release an app to the App Store, Apple requires using a recent Xcode version.xtool [xtool](https://github.com/xtool-org/xtool) is cool, but it is not (yet) a serious replacement. So your coding agent needs a Mac.

But still, why laptops? Isn’t it a major waste of screen pixels and keyboard buttons and battery ions? Well yes, Mac Mini would be a better choice for a datacenter. Sure, it has very rudimentary remote management, no redundant power, and a beautiful case that ensures that cooling will be difficult regardless of which side you lay it on. But these are all known challenges that we have learned to manage.

But have you actually tried to order a couple hundred Minis? We have. Ordered in January, delivered in July. Welcome to the world of 2026 when the supply chains are in disarray. Never mind that Apple doesn’t release updated CPU models to all devices at once. So Mac Mini didn’t get M5 cores for 10 months (and then suddenly got M6 before any other device model!). We promise the best and the newest to our customers so we had to be inventive. That’s how the MacBook-in-Datacenter project was born.

## Our beautiful Mac racks

Good design takes iterations; and we surely didn’t get everything right from the first try. But I think we have the system nailed down.

![MacBooks in a Namespace rack](https://namespace.so/_next/image?url=%2F_next%2Fstatic%2Fmedia%2Fmac-rack.657593ec.jpg&w=1080&q=75)

Every MacBook is connected to an IP KVM for remote management. Indispensable when dealing with laptops. Unlike Mac Mini, laptop firmware is designed with the screen in mind: some maintenance actions can only be done by clicking buttons in the beautiful GUI.

Of course, every MacBook gets an external 10G Ethernet NIC. Many folks at Namespace are networking geeks and we are proud of the connection speed and reliability that we provide to customers.

And then there is power. So in total, four cables per laptop, forty-eight per shelf, plus twelve for Ethernet, plus KVM wiring. Let’s just say that careful cable management is essential.

Heat management too. Currently we are deploying custom-made holders, designed to wick the heat from the hotspots and guide hot air away instead of blowing it at the nearby machines.

### Not the problems we expected: HDMI, FileVault, and USB NICs

Naturally, we expected problems and challenges. Just not where they appeared.

I bet some of you looked at the pic above, saw MagSafe cables plugged in and already started writing a snarky tweet that MagSafe is not safe and may get accidentally disconnected.

And yes, I was worried about that too. But the reality is that we have seen very few cases of MagSafe disconnections.

The most unreliable cable? HDMI! Often thick and inflexible. Tends to fall out every time you touch the shelf.

Luckily by now we found almost perfect cables that bend exactly as we want them to bend. And we bought the entire supply of them. There are zero more of them available to order on Amazon.

Another source of pain: FileVault disk encryption. Or rather decryption. When a MacBook reboots (in an unplanned fashion) it prompts for a password to unlock the local disk. The prompt is shown on the screen (of a closed laptop somewhere in the rack, doh). No problem, we thought: we have the KVM connected just for this case!

Nope, not so easy. The password prompt disappears from the external screen after 5 minutes of inactivity. Then we remembered that macOS Tahoe introduced a useful feature: we could [unlock FileVault remotely](https://keith.github.io/xcode-man-pages/apple_ssh_and_filevault.7.html) using SSH protocol. Still no luck. On laptops it expires after 5 minutes too. Looks like the only way out of this state is to reboot by physically pressing the button and racing to unlock in time.Mac reboot While on this topic, a small tip: you can reboot a Mac in a way that will keep the disk unlocked when you need it: `fdesetup authrestart`.

And then there are simply bugs. Under certain conditions, an external USB NIC stops working and acts as if it is not powered and shows no link. Trying another cable, another NIC, another switch port, no luck. But a quick visit to Network Preferences, deleting and recreating the “network service” in MacOS Settings brings it back to life.

### What about Mac Minis and Studios?

At the moment we actually have it easy: M5 and M6 Mac Minis and Studios are coming into our fleet soon. They are way simpler to manage in the DC: they have a built-in 10 Gb network card, they boot up automatically after a power cycle and the OS can be configured entirely automatically via MDM.

But Namespace has some special sauce for the Minis too. We design custom-made shelves that hold the machines with focus on efficient cooling. We have tested a few designs using airflow simulation to arrive at what we are deploying now.

In the end installing Mac Minis is a breeze: unpack them, throw 10 pieces into the holder, connect power and network and press the power button once (famously inconveniently located). From there on automation takes care of them.

## Virtualizing Macs

Under the hood a Namespace instance is backed by a virtual machine. We have multiple virtual machine engines integrated into a unified platform to run Linux, Windows, and Mac instances in the most efficient way. This way we can provide the same features that make Namespace special, like [cache volumes](https://namespace.so/docs/architecture/storage/cache-volumes), telemetry, or VPC connections.

Unsurprisingly, our Mac solution is based on Apple’s excellent [Virtualization framework](https://developer.apple.com/documentation/virtualization). It is an embeddable and flexible virtual machine engine. It’s easiest to compare it to Firecracker in how it fits the system architecture: it implements generic VM operations end-to-end as well as typical peripherals.

For completeness, Apple also provides a lower-level [Hypervisor framework](https://developer.apple.com/documentation/hypervisor), which is comparable to the KVM API of Linux. For instance QEMU uses it on macOS under the hood. Hypervisor provides a way to execute machine code in isolation, but requires the application developer to implement the bulk of machine architecture and peripherals. It could give us a lot of control, but unfortunately it cannot run macOS (it requires an Apple proprietary environment that third parties cannot, or shouldn’t, try to recreate).

In fact, most competing platforms are built on the same foundation. But the details matter: Apple gives us a few primitives that need to be arranged in just the right way to work fast and be secure. The more interesting thing is to watch how different people solve similar problems with different constraints.

### Limitations of Apple Virtualization

A well-known limitation is that Apple only allows two macOS VMs per physical Mac. It can be a Mac Pro with 24 cores and 192 GB memory, yet it is limited to running only two guest VMs. This limitation leaks into our product offering: we can only slice Mac resources in two halves. So we run instances with 6 or 12 cores on 14-core Mac Minis and 8/16-core instances on 18-core MacBooks (we reserve a couple cores for management overhead).

An important consideration of Virtualization VMs is that the virtual CPU is executed by a separate OS process (VMM process in the diagram) from the software integrating Virtualization (here Namespace VM host). If we want to hook some VM behavior or implement custom virtual devices, every interaction with them requires an IPC roundtrip between the VM process and the “host” process. Such context switches are unavoidable and need to be taken into account when designing the system.

### Virtual networking

Virtualization framework offers a few ways to connect a virtual machine to the network. For instance, bridged and NAT networking modes route traffic directly from the VMM process to the host network interfaces. These are suitable for VMs running on a local computer where the workload is trusted.

Namespace instances run untrusted code and we need to enforce policies on what a VM can or cannot access (these policies [can also be customer-defined!](https://namespace.so/docs/security/egress-policy)). Also we provide a few additional services to the instances that should appear “as if” they were on the local network for the guest system.

This requires us to take full control of guest networking in the mode where we process every packet that comes out and into the VM in software. We based our guest networking on a userspace TCP/IP stack from Google gVisor project and use it to implement an IP gateway/firewall for the instances. This way we minimize exposure of the host OS to unprocessed packets controlled by the guest.

Network packet handling needs to be very efficient, but as outlined above the path involves IPC. The VMM process needs first to pass the packets to the “VM host” process, which then processes and passes them to the network interface. It takes careful implementation and batch processing wherever possible to minimize the number of IPC roundtrips.

The above approach allows Namespace MacOS instances to reach 4 Gbps upstream bandwidth (per [Fast.com](https://fast.com)). This is unfortunately still lower than the line rate. We have been exploring alternative approaches, including ideas on how to avoid IPC for network processing entirely. But productionizing the new setup is still left for the future. After all, 4 Gbps is still quite respectable.

## VM images

Creating your first VM using Apple Virtualization is very easy: download a macOS installer XIP, use UTM/Tart/any of the dozens of open-source Virtualization wrappers to start a machine, click through Setup Assistant, install Xcode and you are good to go. Now you have a VM image that you can fork and run any workload locally.

But you need to test on multiple versions of iOS, so you install a few iOS runtimes. And then you install the upcoming Xcode beta version (and new runtimes too). And before you know it your VM image has grown to 500 GB (iOS runtime packages are huge).

You also want to have the previous version of macOS for testing that one little thing. Another 500 GB right here. Apple releases the new macOS developer preview, one more image.

And then you realize that the Xcode beta version subtly breaks the non-beta Xcodes on the same system (true story!). After all, Xcode installation bumps the “global” versions of CoreDevice and CoreSimulator. Hence any bugs in the beta Xcode simulator are visible when you’re using ostensibly stable Xcode versions. So now you are maintaining image versions with and without the pre-release Xcodes.

And now you have 2 TB of VM images. On what? A 2 TB MacBook hard drive?

That’s the challenge we faced: we need to build, distribute, and store huge images across the fleet of hundreds of machines.

### Build process

Building images is a repeatable process so that we can reliably produce new images when Apple releases new software versions. Building is driven by a script that replicates installation and configuration as a human user would do it (e.g. the build script can run shell commands, click UI buttons or reboot the VM). Wherever possible we stick to using controls designed by Apple to configure the VMs in order to avoid surprising behaviours inconsistent with what a typical local user would be seeing. Naturally, building images is an automated and repeatable process. It is driven by a workflow expressed as a sequence of operations in a YAML file. Operations can be VM control (e.g. reboot), running a shell script or programmatic interaction with the system or setup UI over VNC.

Resulting VM images are handed over to [Namespace internal Image Registry](https://namespace.so/docs/architecture/storage/container-registry), in fact the same one that handles Linux and Windows image storage, replication and distribution (covered in [the excellent article by Matteo](https://namespace.so/blog/container-image-loading)). Disk images are of course quite different from normal container images: OCI layers are simple tarballs of files, while disks are generally flat arrays of bytes. Very large arrays in fact. And to a large extent they contain empty space: that’s the disk space that would be available to users as free space.

Naturally, it would be very wasteful to transfer and store the “empty” data in the registry. So we developed a simple format that allows us to preserve the “sparse-ness” of the images when stored in the registry. Unallocated spans of the image take just a few bytes.

And there is another important optimization. Remember the example scenario mentioned in the VM Images section: we have an image of the same system that differs only in addition of an extra Xcode installation. Obviously the latter image is built using the former as a base and most of the bytes are unchanged.

Our storage format preserves the “shallow copy” relationship and stores such images as multiple layers: the base layer is shared between them, while subsequent layers only contain the modified blocks.

### Distribution

At this point we have the images in an OCI registry now. But of course Virtualization framework cannot start a VM directly using a disk residing in the registry. It needs to have something on the node itself: a VM disk can be backed by either an image file on the local host SSD, or it can route disk I/O requests to a server using Network Block Device (NBD) protocol. The latter would allow us to intelligently serve highly demanded data from a local cache, while conserving on-device space by not storing unneeded blocks.

But NBD has the same drawback as userspace networking from above: every I/O operation would incur an IPC roundtrip at minimum, including buffer copying and context switches. You see the same pattern as with networking: a very direct hardwired and fast option and an indirect “software-defined” but disappointingly slow option.

At Namespace we value performance a lot, so the choice is clear: we’ll serve the images from the local disk. We need to materialize them from OCI images in the registry on the SSD of every node in the fleet. 500 gig images to hundreds and thousands of nodes. And we need to do it fast.

Obviously, every node naively downloading from the central registry was not going to work. With the example numbers we are talking about half a petabyte of traffic transferred within, conservatively, tens of minutes. The registry (and the network fabric) would need to serve terabits per second!

The bytes that we are distributing are the same going to every node so the obvious solution is to organize a peer-to-peer delivery of these blobs. Each block only needs to be downloaded from the registry once by some node. Then another node that is close by (by certain metric) can download the block directly from the peer and we have two copies. So two more nodes can pull these bytes and so on. The fleet is exponentially flooded.BitTorrent Obviously, BitTorrent was a heavy inspiration for this design. But unlike BitTorrent, which works in a public network of untrusted peers, we could make different tradeoffs leading to better efficiency and predictability. But that’s a topic for another post.

### Storage

Now the bytes are on the node disk. But remember the storage optimizations we did in the Build Process? With the naive approach of storing the images on disk there are a lot of bytes that are useless: they are either holes or could be shared with another image.

Some of you probably know that these are exactly the types of storage optimization techniques that are possible with modern file systems, namely APFS.ZFS Linux users have seen the same features in ZFS.

Using these cool features is also pretty easy: open a new file for writing, seek to offset 100G, write a block. And there you just made a file with a large hole in it. Only the block you wrote is allocated on disk, the rest is 100 GB of imaginary zeroes. When reading the file you won’t even notice the holes. The OS will return actual zeros.

Sharing the blocks is similarly transparent: just use the `cp -c` mode or `clonefile` syscall. The new file will share the same disk blocks and essentially not take any space.

Writing to the cloned file is also transparent: as soon as you try to overwrite some blocks, these particular blocks will be allocated on the disk, while the shared portion will remain shared.

## What’s next?

The pace at which we are expanding our fleet is increasing and the experience from all the above has transformed into a repeatable process. We know exactly how long it takes to bring a MacBook rack into prod. The rack assembly line is dialed in. DCOps people are trained. Disk images are built and distributed. We can bring whatever number of machines our customers need into the fleet when we need to (well, as long as we can purchase them at all).

We can still do better! What if there was a “holy-grail” device to host a shelf of MacBooks, providing power, ethernet and KVM connections over a single USB-C cable? It should be possible, yet there is currently no hardware on the open market that satisfies our requirements. So we are working on something very custom.

Or maybe we’ll do another crazy thing? What if we deploy phones to the datacenter? See what the future brings on our X soon!
