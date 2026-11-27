---
title: 'SBD: We Have a Filesystem at Home'
link: https://fly.io/blog/sprites-block-device/
source: fly-io
published: 2026-10-22T00:00:00Z
updated: 2026-11-05T00:08:46Z
first_seen: 2026-11-27T00:01:05.630507547Z
content: extracted
html: 2026-10-22-sbd-we-have-a-filesystem-at-home.html
remote_preview:
  url: https://fly.io/blog/sprites-block-device/assets/pasta-robot.webp
  alt: A weary bird cook beside a cool pasta robot
extra:
  thumbnail: https://fly.io/blog/sprites-block-device/assets/pasta-robot.webp
---

Author

![Daniel Botha](https://fly.io/static/images/danielb.webp)

Name

Daniel Botha

![A weary bird cook beside a cool pasta robot](https://fly.io/blog/sprites-block-device/assets/pasta-robot.webp)\
 Image by [Annie Ruygt](https://annieruygtillustration.com/)

I have unkind things to say about people who make their own pasta. Here, I’ll only use them as an example of how not to think when you’re building infrastructure. It’s unfairly easy to believe that the harder you work on something, the better it gets. But infra is about making the $1 bag of spaghetti: a simple thing that works well (and doesn’t require you to spend a week scraping flour out of the corners of your kitchen).

When work on Sprites started, it was a new kind of infra for a new kind of product. So when we couldn’t find the right filesystem in the pasta section, we made our own. It was clever, and that’s exactly why we didn’t like it. This post introduces its replacement, the Sprites Block Device (SBD). It’s the bag of spaghetti you grab off the shelf, and the only difference you’ll notice is better reliability.

## It was always object storage

Our stance on one core aspect of Sprites’ storage hasn’t changed from v1: the root of a Sprite’s disk is object storage, not the NVMe in whatever server the Sprite happens to be running on. That’s what lets a Sprite sleep, wake on a different server, and still find its files where it left them. The durable state of a Sprite is, more or less, a location in a bucket. [We wrote about why](https://fly.io/blog/design-and-implementation/ ""), at length, with feeling.

The thing about object stores is they can’t change part of an object. To change one byte, you have to replace the whole thing. That’s a bad cultural fit for Linux programs which rewrite a few bytes in the middle of a file all the time. Something has to handle that translation.

At launch, that something was a heavily modded version of JuiceFS. In stock form, JuiceFS splits storage into the data (as chunks in the bucket) and the metadata (a map of which chunks make up which files) in a database. That’s not how we used it, though. Our hot rodded version ran a rewritten SQLite metadata backend, kept durable with Litestream. It stored exactly one file: a 100 GB disk image, mounted on a loop device with ext4 inside. Linux did all the ordinary ext4 things. JuiceFS just held the image.

It worked fine. Sprites shipped on it, people made a ton of them, and checkpoints were fast because checkpointing was mostly just shuffling metadata around. But as their usage grew, cracks began to emerge.

## Manifests that lie

A filesystem with a metadata database has two inseparable components: the chunks in the bucket, and the database that explains how those chunks fit together. When they agree, you have files. When they don’t, you’re screwed.

Every layer we built on top had to inherit that arrangement and add to it. A checkpoint under JuiceFS was a copy-on-write clone of the disk image, so the metadata database tracked the chunks for every checkpoint as well as the live disk. A second SQLite database, `checkpoints.db`, listed the checkpoints themselves and which image file to mount on the loop device for each one.

So a Sprite’s disk was chunks, plus a metadata database saying which chunks made up each image, plus a checkpoint database saying which image was which. Each one was made durable separately, so each one could be right while the others were wrong.

## What could go wrong?

A story which may or may not be based on lived experience: one day a Sprite stops booting. JuiceFS is asking object storage for a chunk and getting a 404. We wanted to go fast, so we ran JuiceFS in writeback mode. It records a chunk in its metadata first and uploads the bytes after. If the machine is killed at any time between those two steps, the metadata points at a chunk that doesn’t exist.

So the ext4 filesystem living on top finds a directory block that failed its checksum and goes read-only. Moving the Sprite to a new machine won’t help, nor will restoring a checkpoint, because JuiceFS checkpoints were clones. They shared the same chunks, so every checkpoint would have the same gap.

## My agent ate my dogfood

It was less the technical details of issues like this and more their timid nature that brought about the single most intense period of dogfooding in Fly.io history. And if you’re going to be dogfooding computers for agents, it better be the agents doing the eating.

So we built a harness that drives fleets of Sprites through write, checkpoint, sleep, wake and restore, over and over, verifying every byte that comes back. We pointed it at 400 JuiceFS Sprites for about 5,000 cycles. A nonzero number of them permanently lost the ability to checkpoint. Well … fuck.

For a while, we continued to interrogate our recipe. There was more reconciliation, more repair tooling, and increasingly clever ways to keep three ledgers in agreement. But spaghetti that comes with a repair kit is gonna leave a weird taste in your mouth, however you cook it.

## Just a disk

Enter: the Sprites Block Device (SBD). It takes a bucket and presents it to the Linux kernel as a block device, over NBD. The Sprite formats that device as ext4 and mounts it, the same way you’d mount a disk on any Linux box.

Duh.

Under SBD, the metadata lives in the bucket alongside the blocks it describes, so there’s no separate database to keep in sync. A daemon inside the Sprite, `sbd serve`, answers the kernel’s block reads and writes from its own local cache, uploads dirty blocks to object storage in the background, and flushes on demand when something needs a guarantee that the bytes have left the building.

Ext4 still does the legwork. We’ve just made everything underneath it much simpler: one daemon and one bucket, where there used to be a filesystem, a database and a disk image to keep in agreement.

## Dumber checkpoints are better checkpoints

Checkpoints are a huge deal for any infra that is designed to be a padded room for agents. Under JuiceFS, a checkpoint was a copy-on-write clone of the disk image, recorded in `checkpoints.db`. Under SBD, a checkpoint is a block snapshot of the whole volume, with a version number, recorded in SBD’s own checkpoint catalog. Ah…the unmitigated comfort of a single source of truth.

Restore is equally vanilla. Each checkpoint knows which version of the volume it belongs to, so a restore unmounts the filesystem, rewinds the volume in place and mounts it again. It costs about as much as a restart.

Of course things can still go wrong. The difference is that a Sprite now stops and says so instead of carrying on with a disk it can’t vouch for. SBD checks every block it reads back from object storage against what it wrote. If SBD itself dies, the whole Sprite restarts, because a clean restart is the only way back to a disk you can trust.

## Mom’s spaghetti

Continuing voracious dogfooding, a borderline abusive test harness and input from a few pioneering design partners have shown SBD to be orders of magnitude more reliable than our homemade pasta filesystem.

So SBD is going to be the Sprites filesystem from here on out. Ext4, on a block device, on a bucket. The only thing keeping track of your files is the filesystem code every Linux box already runs.

*If you have any Sprites created before 10/6, you can migrate them using [these instructions](https://docs.fly.io/sprites/sprite-maintenance#storage-upgrades "").*

Next post ↑

[Feng Shui for Robots: The Fly MCP Server](https://fly.io/blog/feng-shui-for-robots/)

Previous post ↓

[Your Agent Speaks MCP. Give It a Computer.](https://fly.io/blog/sprites-mcp/)
