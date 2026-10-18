---
title: Iroh global content discovery
link: https://www.iroh.computer/blog/iroh-global-content-discovery
source: lobste-rs
published: 2026-10-04T19:19:42Z
updated: 2026-10-04T19:19:42Z
first_seen: 2026-10-05T18:49:38.624325496Z
authors:
- iroh.computer via sanqui
labels:
- distributed
summary: Comments
content: extracted
html: 2026-10-04-iroh-global-content-discovery.html
---

What got me excited about [IPFS](https://ipfs.tech) many years ago, briefly after it was announced, was being able to publish a personal website, blog post, or political pamphlet and have it remain available globally as long as enough people are interested in the content. As governments have been more sophisticated in their firewalling methods, this use case for circumvention tools and permissionless global content discovery is still incredibly relevant.

Recent events have added some urgency to this. IPFS [shipyard is shutting down](https://ipshipyard.com/blog/2026-the-end-of-ipfs-at-shipyard/). This does not mean that IPFS will stop working, but it does not bode well for the future of the project.

When we had to solve hole punching, we started looking at existing systems and chose the best open source system as an initial starting point for our own implementation. So let's do the same for global content discovery.

There are a number of projects trying to solve this problem. But one project stands above all others: [BitTorrent](https://www.bittorrent.org/). It *just works* and has done so for over two decades.

So let's take a look at what makes BitTorrent the current leader in permissionless global content discovery. BitTorrent has a relatively simple protocol for blob transfer and a [DHT](https://en.wikipedia.org/wiki/Distributed_hash_table) called [*Mainline*](https://www.bittorrent.org/beps/bep_0005.html) for global content discovery.

The transfer protocol and content discovery are *separate systems*. In fact, the DHT was developed later than the transfer protocol. BitTorrent was released in 2001 using centralized trackers for content discovery; the Mainline DHT was added in 2005.

BitTorrent works by creating a `.torrent` file that contains information about the data to be downloaded. The file is encoded using [bencode](https://www.bittorrent.org/beps/bep_0003.html#bencoding), which is conceptually similar to JSON.

`pieces` contains the concatenated SHA-1 hashes of all `piece length`-sized pieces. The transfer protocol downloads blocks of these pieces from different peers.[^1]

The transfer protocol allows requests for ranges within pieces, but you can only validate a piece **after** you have downloaded it completely and computed its SHA-1 hash.

I don't want to dwell on this too long, but I think that [BLAKE3 verified streaming](https://github.com/oconnor663/bao#readme) is a superior replacement for the transfer protocol. We implemented a protocol called [iroh-blobs](https://docs.iroh.computer/protocols/blobs) that uses BLAKE3 verified streaming for sharing blobs of data over iroh connections. With BLAKE3, we only need a single root hash, and neither a `piece length` parameter nor hashes of the individual pieces. This is what allows iroh-blobs to validate any piece of a large blob without needing intermediate hashes.

For a visual explanation of how it works, see my [BLAKE3 and Bao deep dive](https://www.youtube.com/watch?v=nk4nefmguZk), which covers verified streaming, outboard encoding, and range requests.

We still have some work to do to make multiprovider downloads of single blobs efficient, but the streaming protocol itself with its fine grained validation is superior to the BitTorrent transfer protocol.

Initially BitTorrent used trackers to get providers for a torrent. What the DHT adds is a way to get providers *without* having to rely on trackers.

This works by computing the SHA-1 hash of the `info` section, which includes the hashes of all pieces. Then you announce on the DHT that you have content for this hash. The mainline functions for this are [`announce_peer`](https://www.bittorrent.org/beps/bep_0005.html#announce-peer) and [`get_peers`](https://www.bittorrent.org/beps/bep_0005.html#get-peers). Each DHT node will store a large set of providers.

![Bob and Carol announce the same infohash to DHT nodes. Each node keeps both provider addresses in a list. Alice uses get_peers to retrieve the providers.](https://www.iroh.computer/animations/announce-peer-dht-dark.svg?v=12)![Bob and Carol announce the same infohash to DHT nodes. Each node keeps both provider addresses in a list. Alice uses get_peers to retrieve the providers.](https://www.iroh.computer/animations/announce-peer-dht.svg?v=12)

Many more modern protocols also use DHTs for content discovery. But Mainline works extremely well compared to many modern alternatives. It is also an extremely large and stable public DHT deployment, so it is unlikely to go away any time soon. You can look at current [mainline statistics](https://ipinfo.io/tags/bittorrent).

![Mainline DHT statistics from IPinfo.](https://www.iroh.computer/blog/iroh-global-content-discovery/mainline-statistics.png)

So let's take a look at why.

The mainline protocol takes into account that a DHT operates across an *extremely large* number of nodes. You **can't** afford large per-node connection state. Therefore both queries and responses are constrained to fit into single non-fragmented UDP packets.

There are a number of other limitations compared to modern DHT implementations that all serve an important purpose:

- the data stored for a provider is just the public `host:port` pair of the provider *as seen from the DHT node*. There is no user defined information in a provider record.[^2]

- For newer extensions like [BEP 44](https://www.bittorrent.org/beps/bep_0044.html) the data is fully self-contained and verifiable, either a tiny piece of data or a signed record, both limited to fit into a typical MTU.

- you can only store data after first querying the DHT node and returning the short-lived token from its response, *proving* that *at publishing time* you can receive packets at your claimed IP address.[^3]

The result of this minimalism is that mainline lookups typically complete in less than a second.

Here is a real BEP 44 lookup of a pkarr record using the `get_mutable` example from [n0-mainline](https://github.com/n0-computer/n0-mainline).

If you are traumatized by DHT lookups taking forever or timing out: **it doesn't have to be this way**. The mainline DHT shows that millisecond lookups are possible at a global scale.

## [Mainline for finding blobs providers](https://www.iroh.computer/blog/iroh-global-content-discovery#mainline-for-finding-blobs-providers)

Now that we have established why mainline works so well, let's see if there is a way to use it for iroh blobs content discovery.

Mainline provider records are just an IPv4 `host:port` pair. But iroh connections are dialed by cryptographic identity, currently the Ed25519 public key aka `EndpointId`. So to use mainline provider records for iroh blobs endpoint discovery, we would need a way to know which `EndpointId` is currently listening on this `host:port`.

In most cases this `host:port` will be behind a NAT, so it is *not reachable*.

I tried a number of ways to use current mainline mechanisms such as BEP 44 to store this mapping, but currently this is not possible. So we need a tiny extra UDP address index service ([`udp-addr-index`](https://github.com/n0-computer/iroh-content-discovery/tree/main/udp-addr-index)) to provide this mapping.

This can be an incredibly simple service. It just stores some tiny arbitrary metadata for each verified UDP `host:port` pair and is reachable exclusively via a simple single-packet UDP protocol. Since mainline requires UDP, and this is an extension for mainline, we don't need to handle the case where sending UDP packets is not possible.

Writes need a mechanism similar to the mainline or QUIC token mechanism to verify that the remote is actually reachable on the `host:port` pair. Reads don't need this, we just require a padded query packet to prevent amplification.

The service stores up to 1 KiB of arbitrary metadata. It's a *last writer wins* map from a live UDP `host:port` pair to a tiny blob. Mappings expire after some time, so a content provider has to update the mapping at regular intervals as well as when its public UDP `host:port` pair changes.

The current implementation keeps the map purely in memory. Records have to be updated at regular intervals anyway, and not requiring persistence makes the service much simpler and cheaper to operate. You can run an address index service for millions of iroh endpoints on a single small box with a publicly reachable UDP socket.

The address index service does not depend in any way on iroh. It is infrastructure that could also be useful for non iroh applications using mainline.

In the long term I would love the address index service function to be handled by a BitTorrent Mainline extension. It is simple, generically useful, and not specific to iroh.

For our endpoint discovery purposes we store a record containing the endpoint id, current UDP socket addr, and a timestamp, signed with the endpoint private key. So *only the owner of the endpoint key* can write a valid signed record, so you can't impersonate other endpoints.

The address index service however does **not** check anything about the endpoint. What `get_peers` in conjunction with this service gives us is just a list of *candidate* iroh endpoints which *might* serve the content.

Endpoint discovery doesn't have to be for content discovery. We also have an example that shows how to [discover peers for a gossip topic](https://github.com/n0-computer/iroh-content-discovery/blob/main/iroh-mainline-endpoint-discovery/examples/gossip.rs).

So now let's take a look at the overall workflow when announcing and discovering content. For both announce and discovery we will need a mainline DHT node. We use the [`n0_mainline`](https://docs.rs/n0-mainline) crate just like in the existing [`iroh-mainline-address-lookup`](https://docs.rs/iroh-mainline-address-lookup) crate.

### Announce

We want to associate the UDP socket of `n0_mainline` with our `EndpointId`. As outlined above, we use an UDP addr index server for this: we publish a signed record containing our `EndpointId` via the same UDP socket to the addr index server. `n0_mainline` has a mechanism to publish arbitrary UDP packets on its socket and to intercept incoming UDP packets. This needs to happen at regular intervals as well as immediately when the public `host:port` pair changes.

For the announce itself: the mainline keyspace is 20 bytes, usually used to announce SHA-1 hashes of the info section of a .torrent file. We want to announce BLAKE3 hashes, so we first compute the SHA-1 hash of the BLAKE3 hash. Then we use `announce_peer` to announce that we are providing data for that hash. These announces also need to happen at regular intervals.

You might wonder if SHA-1 is still safe for this purpose. Its collision resistance is broken, but there is no known practical preimage attack. More importantly, the DHT is just a best-effort mechanism for finding candidate providers. We verify downloaded data against the original BLAKE3 hash, so a misleading DHT result can waste time, but cannot make us accept the wrong content.

### Discovery

For discovery we first need to find at least one working service that can translate `host:port` pairs into `EndpointId`s. We have two mechanisms built in for this: a rendezvous hash that allows address index services to announce themselves, and a BEP 44 record that is a *curated list* of good address index services. Announce and discovery need to agree on the rendezvous hash or BEP 44 record name to find address index services.

Once we have at least one working address index service, we compute the SHA-1 hash of the BLAKE3 hash we are looking for and call `get_peers`. The output of `get_peers` is a list of `host:port` pairs which we translate to `EndpointId`s using the address index service.

At this point we get a stream of *unverified* `EndpointId`s. We currently do a quick BLAKE3 size query for the content we are looking for to make sure they are live and serve the right content, then hand them over to the iroh-blobs downloader to download the actual content.

Note that the actual connection now uses the `EndpointId` and iroh's built in address lookup and hole punching to establish a connection. The QUIC connection does *not* work via the announced UDP `host:port` pair. That is just a key for the address index service.

![Bob and Carol publish address-to-endpoint mappings to the address index service and announce content to Mainline. Alice gets provider addresses from Mainline, then resolves them to candidate endpoint IDs through the address index service.](https://www.iroh.computer/animations/content-discovery-workflow-dark.svg?v=9)![Bob and Carol publish address-to-endpoint mappings to the address index service and announce content to Mainline. Alice gets provider addresses from Mainline, then resolves them to candidate endpoint IDs through the address index service.](https://www.iroh.computer/animations/content-discovery-workflow.svg?v=9)

All of the above is implemented in the experimental [iroh-content-discovery](https://github.com/n0-computer/iroh-content-discovery) repository. Its workspace contains the address index service, its protocol and client as well as a few extra crates that make use of it.

To see the entire workflow in action, we can run the [`blobs` example](https://github.com/n0-computer/iroh-content-discovery/blob/main/iroh-mainline-endpoint-discovery/examples/blobs/main.rs). It runs both publish and resolve in one process, but discovery nevertheless goes via mainline and the address index service.

So now we have a mechanism to do content discovery for blobs. This will be useful for tools like [sendme](https://www.iroh.computer/sendme) and in general for sending around large amounts of data.

But what about permissionless publishing of websites such as a blog? To make this work we need some extra components.

We need a syntax for content-addressed links. We don't go into a giant rabbit hole about how to encode these links. Our content addressed links are always BLAKE3. We need to reserve some global namespace for them, so we reserved a domain `blake3.net`. A content-addressed link is just `https://<hash>.blake3.net`, with the hash encoded using [zbase32](https://philzimmermann.com/docs/human-oriented-base-32-encoding.txt).

![A content-addressed URL with its 52-character z-base-32 BLAKE3 hash and blake3.net namespace labeled.](https://www.iroh.computer/blog/iroh-global-content-discovery/link-syntax-dark.svg?v=2)![A content-addressed URL with its 52-character z-base-32 BLAKE3 hash and blake3.net namespace labeled.](https://www.iroh.computer/blog/iroh-global-content-discovery/link-syntax.svg?v=2)

We don't want to run an actual gateway at `blake3.net`. That would be a *very bad idea* for various reasons.

Instead we want the browser to interpret these links as something that can be resolved in a different way. So we wrote a simple browser plugin that just rewrites these links to `http://<hash>.blake3.localhost:<port>` where the port is configurable in the plugin.

That's all the plugin does.

It currently works for Brave, Chrome, and Firefox.

For Chrome and Brave, [install iroh link from the Chrome Web Store](https://chromewebstore.google.com/detail/iroh-link/aajlbmaphckgbinhnifpiggcmdfnofcd).

The Firefox extension is awaiting review. For now, save the [unsigned Firefox XPI](https://github.com/n0-computer/iroh-content-discovery/releases/download/extension-v0.2.1/iroh-link-0.2.1-firefox-unsigned.xpi) to your computer using **Save Link As**. In Firefox 140 or later, open `about:debugging#/runtime/this-firefox`, click **Load Temporary Add-on**, and select the downloaded file. Open the extension popup, click **Save**, and allow access to the requested sites. You must load it again after restarting Firefox; the unsigned package cannot be installed through **Install Add-on From File** in regular Firefox.

The last component is a local gateway that serves content-addressed data on localhost. It interacts with mainline to find content and orchestrates the actual blobs downloads. It supports [iroh-blobs collections](https://docs.iroh.computer/protocols/blobs#collections) and shows a directory index for them. It also does file type detection. It is derived from iroh blobs gateway in [iroh-examples](https://github.com/n0-computer/iroh-examples/tree/main/iroh-gateway).

You can [compile the gateway from source](https://github.com/n0-computer/iroh-content-discovery/tree/main/iroh-link-gateway), or [download the latest release](https://github.com/n0-computer/iroh-content-discovery/releases/latest).

We could build a service worker to verify the data inside the browser process.

Instead we verify the data inside the gateway.

It is software that you have to trust, whether it lives in the browser process or elsewhere doesn't matter much.

With these two components in place we can browse content-addressed data.

![The local iroh gateway displaying the Open Content Library directory with Books, Essays, Films, LLM, and Website folders.](https://www.iroh.computer/blog/iroh-global-content-discovery/content-addressed-browsing.png)

Now we have a mechanism to publish and consume content-addressed data. But we don't have a way to refer to *mutable* data. You could use `blake3.net` links from an existing website, but for that you need a registrar and a hoster, so it is not the permissionless publishing we are after.

Fortunately a permissionless DNS system already exists, [pkarr](https://pkarr.org). We [have been using it](https://www.iroh.computer/blog/iroh-global-node-discovery) for years for endpoint address lookup. Pkarr is a standard to publish a DNS record for a keypair, so only the owner of the secret key can publish new versions. The records are published on mainline DHT, which fits our setup neatly.

To make this compatible with our browser plus local gateway usage scenario, we reserved another domain `pkarr.net` and added a rewrite rule to the plugin and pkarr support to the gateway.

A pkarr link has the format `<name>.pkarr.net`, where `name` is the [zbase32](https://philzimmermann.com/docs/human-oriented-base-32-encoding.txt) encoding of an Ed25519 public key. The plugin rewrites it to `<name>.pkarr.localhost:<port>`. The gateway will then resolve the pkarr record and either perform a redirect or directly serve content-addressed data if the pkarr record links to `hash.blake3.net`.

![The browser plugin keeps the public key unchanged while rewriting https://uinsazmmp47ejo8gs5dbc6rfxya14cgqhxmdqin8ae55w5aqnsio.pkarr.net to http://uinsazmmp47ejo8gs5dbc6rfxya14cgqhxmdqin8ae55w5aqnsio.pkarr.localhost:1234.](https://www.iroh.computer/animations/pkarr-link-rewrite-dark.svg?v=2)![The browser plugin keeps the public key unchanged while rewriting https://uinsazmmp47ejo8gs5dbc6rfxya14cgqhxmdqin8ae55w5aqnsio.pkarr.net to http://uinsazmmp47ejo8gs5dbc6rfxya14cgqhxmdqin8ae55w5aqnsio.pkarr.localhost:1234.](https://www.iroh.computer/animations/pkarr-link-rewrite.svg?v=2)

If you are familiar with IPFS, this is very similar to [IPNS](https://docs.ipfs.tech/concepts/ipns/). I even did a little toy experiment called [Iroh Pkarr Naming System](https://github.com/n0-computer/iroh-experiments/tree/main/iroh-pkarr-naming-system)

The [`pkarr-publish-resolve` example](https://github.com/n0-computer/iroh-content-discovery/blob/main/iroh-mainline-endpoint-discovery/examples/pkarr-publish-resolve.rs) in [iroh-content-discovery](https://github.com/n0-computer/iroh-content-discovery) demonstrates the whole workflow: generate a keypair, publish a blob and its name, then resolve the name, discover a provider, and download the verified content from a separate client.

Pkarr gives us permissionless names, but not human-readable ones: anyone can generate a keypair and publish under its public key, without asking a naming authority. This is the tradeoff illustrated by [Zooko’s triangle](https://en.wikipedia.org/wiki/Zooko%27s_triangle): pkarr names are decentralized and cryptographically verifiable, but a 52-character encoded public key is not a memorable name.

![Zooko’s triangle: pkarr sits between decentralized and secure; DNS with DNSSEC sits between human-readable and secure.](https://www.iroh.computer/blog/iroh-global-content-discovery/zookos-triangle-dark.svg?v=3)![Zooko’s triangle: pkarr sits between decentralized and secure; DNS with DNSSEC sits between human-readable and secure.](https://www.iroh.computer/blog/iroh-global-content-discovery/zookos-triangle.svg?v=3)

You can combine human readable naming systems with pkarr to get a memorable name that you can retarget in a permissionless way. We have some ideas about how to do this, stay tuned.

The first step is to run the local gateway. Most readers of this blog will probably just compile it from source, but there are also installers for windows and MacOS on github.

The next step is to install the browser plugin. For Chrome and Brave, [install iroh link from the Chrome Web Store](https://chromewebstore.google.com/detail/iroh-link/aajlbmaphckgbinhnifpiggcmdfnofcd).

Make sure the port configured on the gateway and the browser plugin match. The default is `45475` or `B1A3`.

And then you are ready to browse the content-addressed web.

Try [https://y7rmokt6h5mryuauw83em4u1br6tqrukaw3ngtde7zp8p3bg6hto.blake3.net/](https://y7rmokt6h5mryuauw83em4u1br6tqrukaw3ngtde7zp8p3bg6hto.blake3.net/) for some static content, or [https://5ti57aszf7kaicsncb4wgigkf9bju39kofiz8dthwdujkmz85u8y.pkarr.net/](https://5ti57aszf7kaicsncb4wgigkf9bju39kofiz8dthwdujkmz85u8y.pkarr.net/) for a pkarr record *currently* pointing to the above.

Traditional website publishing just means copying your files to a directory on a server. But for content-addressed data and permissionless pkarr DNS records, *you* are the publisher.

So we wrote a tool iroh-share that simplifies publishing. You can think of it as [sendme](https://www.iroh.computer/sendme), but running as a daemon with a separate user interface.

Both content-addressed data and pkarr records need continuous announcements, so you might want to run this daemon on a small box in your attic that is on 24/7, or a vm in the cloud. I run it on an old Synology NAS in my attic. It's behind a NAT of course, but you do get direct connections anyway.

You can find releases at [https://github.com/n0-computer/iroh-share/releases](https://github.com/n0-computer/iroh-share/releases).

We now have a system for global content discovery of BLAKE3 hashed content addressed data. It is *far from perfect*, but *it's a start*.

The *user interface* of sharing content addressed data is extremely simple. You just state what content you want, and the gateway gets it for you.

The *system behind it* has a lot of room for improvement.

- Mainline is quite scalable, but it probably won't scale for the vision of making all content on the internet content-addressable.

- Mainline traffic is also unencrypted and therefore easily blocked by middleboxes.

- The existing pkarr naming mechanism is using Ed25519 and therefore is not post quantum secure.

- And perhaps most importantly, mainline *does not provide any privacy*. If you share content, *anybody can look up your ip address*.

But we are currently in the *food and shelter* phase of global content discovery. We will continue to improve the underlying content discovery system, possibly by extending mainline, possibly by [writing our own DHT](https://www.iroh.computer/blog/lets-write-a-dht-1) using iroh connections.

But the current system is certainly *better than nothing*, and we can do all these improvements while keeping the user interface stable.

We are currently working on getting our most frequently used iroh protocols `irpc`, `iroh-gossip` and `iroh-blobs` to 1.0. The endpoint discovery mechanism is experimental, but all relevant crates are published on crates.io for you to play with.

* * *

Iroh is a dial-any-device networking library that just works. Compose from an ecosystem of ready-made protocols to get the features you need, or go fully custom on a clean abstraction over dumb pipes. Iroh is open source, and already running in production on hundreds of thousands of devices.\
To get started, take a look at our [docs](https://iroh.computer/docs), dive directly into [the code](https://github.com/n0-computer/iroh), or chat with us in our [discord channel](https://iroh.computer/discord).

[^1]: In *endgame mode*, it may request the same remaining blocks from multiple peers and use whichever responses arrive first.

[^2]: Technically, you can choose the port in an `announce_peer` request. But that gives you only 16 bits, which is not enough to store, for example, a 32-byte iroh `EndpointId`.

[^3]: This mechanism is similar to the [address validation token](https://www.iroh.computer/blog/address-validation-tokens) in QUIC.
