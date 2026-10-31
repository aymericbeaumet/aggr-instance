---
title: 'Sanstun: A sans-IO, parallel-friendly WireGuard implementation in Rust'
link: https://www.firezone.dev/blog/introducing-sanstun
source: hnrss-org
published: 2026-10-30T21:30:00Z
updated: 2026-10-30T21:30:00Z
first_seen: 2026-10-31T08:47:39.594630982Z
authors:
- wh33zle
content: extracted
html: 2026-10-30-sanstun-a-sans-io-parallel-friendly-wireguard.html
remote_preview:
  url: https://www.firezone.dev/blog/opengraph-image?dc0a4d310bb2b9d5
  alt: Firezone Blog
---

Today, we are announcing [`sanstun`](https://github.com/firezone/sanstun): a sans-IO Rust implementation of the WireGuard® state machine, optimized for ease of use, security and performance through a parallel-friendly and idiomatic API.

## [What is sans-IO?](https://www.firezone.dev/blog/introducing-sanstun#what-is-sans-io)

sans-IO is the idea that you design your system such that IO and side-effects are cleanly separated from the state and "business logic". The IO in sans-IO doesn't just mean file or network IO. It also includes things like time and randomness!

Due to their lack of side-effects, sans-IO components compose very well and are relatively easy to test. The design also avoids problems like function coloring and plays well with Rust's ownership rules.

The code examples and references in this post are in Rust but sans-IO isn't Rust-specific at all. Its patterns and ideas can be applied in any language. For a more in-depth explanation, our [sans-IO post](https://www.firezone.dev/blog/sans-io) is a good place to start.

## [History](https://www.firezone.dev/blog/introducing-sanstun#history)

When we first built Firezone, we reached for [`boringtun`](https://github.com/cloudflare/boringtun) as our WireGuard implementation. `boringtun` more or less did what we wanted, but over time, we found ourselves patching our fork over and over with bug fixes as well as API additions. The goal was to allow these patches to still be upstreamed, so we always kept the existing API working the way it was.

What we found was that retaining 100% API compatibility with upstream was increasingly difficult as our product requirements evolved. One of the first big changes we made to `boringtun` was to make it fully sans-IO with regard to time handling ([firezone/boringtun#5](https://github.com/firezone/boringtun/pull/5)). This meant adding new APIs for everything with a `_at` suffix that would take an `Instant`. sans-IO time handling is very important if you want to accurately simulate an entire system. See [this blog post](https://www.firezone.dev/blog/finding-bugs-you-didnt-think-to-test-for) to learn more about how we do that.

Another thing we feature-flagged was internal buffering. At first, it seems convenient that `boringtun` establishes a session automatically if you don't have one yet (or anymore) and you are trying to send a packet. Internal buffering, however, comes with several problems:

1. At some point, the buffer needs to be drained. `boringtun` solves this by requiring you to call `decapsulate` with a zero-length datagram. Rather unintuitive.
2. It incurs additional allocations (at least in the way it was implemented in `boringtun`).
3. Buffer sizes should be configurable / tunable.

All of these problems point at the same solution: Just don't buffer in the library. It simplifies the interface and the implementation on both sides.

The [full list](https://github.com/cloudflare/boringtun/compare/master...firezone:boringtun:master) of patches and fixes is pretty long. Even excluding dependency bumps, that is more than 70 PRs. Several of them are fixes for edge-cases we discovered. Others add new APIs or make certain aspects configurable.

After more than 2 years of using and patching `boringtun`, we had a pretty good idea of what the overall API of a userspace WireGuard implementation should be. Now it exists as `sanstun`.

## [A better sans-IO API](https://www.firezone.dev/blog/introducing-sanstun#a-better-sans-io-api)

The core building blocks of the sans-IO API are now:

- Handling inbound packets:
  - `Tunnel::handle_handshake_initiation`
  - `Tunnel::handle_handshake_response`
  - `Tunnel::handle_cookie_reply`
  - `Tunnel::begin_decapsulate` & `Tunnel::commit_decapsulate`
- Handling outbound packets: `Tunnel::begin_encapsulate`
- Handling timer updates: `Tunnel::handle_timeout` & `Tunnel::poll_timeout`
- Handling events: `Tunnel::poll_event`

Without internal buffering, there is no API contract that requires you to call certain APIs in a loop, which fits much better into the overall sans-IO model. To send a packet, you call `Tunnel::begin_encapsulate`. It is up to you to buffer the packet if you don't have a session yet. Creating a new session means sending a handshake. You can initiate a new handshake at any point with `Tunnel::initiate_handshake`.

The design of these APIs is very deliberate and will make more sense if we quickly recap the WireGuard protocol.

## [WireGuard protocol](https://www.firezone.dev/blog/introducing-sanstun#wireguard-protocol)

WireGuard's message format supports 4 types of messages: handshake init, handshake response, cookie reply and encrypted data.

Every WireGuard session starts with a Diffie-Hellman key exchange which is a moderately expensive operation. WireGuard's design ensures that exposing an endpoint doesn't open up a DoS opportunity.

Anyone who knows the endpoint's public key can easily create bogus handshake initiations and forge the source IP of the incoming UDP packet. The receiving WireGuard implementation has to run its half of the DH operations before it can tell that the supplied handshake data is bogus. But at that point, the CPU resources have already been spent. Send enough handshake initiations and the receiving end will spend all its CPU cycles on handling irrelevant handshakes.

To prevent this, WireGuard specifies a cookie mechanism that forces the remote to prove ownership of its claimed source IP, which in turn allows rate-limiting handshakes per source IP.

These rate limits apply to an entire WireGuard interface and not just to a single tunnel.

Once a handshake has been completed, both peers can exchange encrypted data packets with a symmetric cipher. As part of the handshake, the peers also exchange their locally-chosen session indices. These indices enable de-multiplexing of tunnels as well as multiple sessions within a tunnel.

## [Mapping protocol invariants to API design](https://www.firezone.dev/blog/introducing-sanstun#mapping-protocol-invariants-to-api-design)

If you are working in a language like Rust, chances are you enjoy using types. And using types well means enforcing invariants at compile-time such that you can't even ship a certain bug in the first place. For example, forgetting to apply the rate limiter to a message would be pretty bad because it doesn't show up under normal operation. Similarly, routing an incoming packet to the wrong tunnel is something that will only show up in production.

A good API design makes it impossible to build things that are broken and also guides you along in understanding how the protocol works.

## [One interface, many tunnels](https://www.firezone.dev/blog/introducing-sanstun#one-interface-many-tunnels)

A WireGuard interface is identified by a single key and a tunnel is a connection between two interfaces. Any time we want to send a packet, we therefore need to specify which tunnel it should be sent on and when we receive a packet, we need to figure out which tunnel it belongs to.

In `sanstun`, these concepts directly map to the `Interface` and `Tunnel` types. All inbound packets go through `Interface::receive`. That is the only public API that parses a raw datagram. It parses the packet, applies rate limits and hands you back a `ReceivedMessage` with one enum variant per WireGuard message type, or a cookie reply to send back if the interface is under load. These then map directly to the corresponding functions on a `Tunnel`: `Tunnel::handle_handshake_initiation`, etc. Handshake initiations are routed by the initiator's static public key. All other messages expose *which* `Tunnel` they should be dispatched to, identified by a `TunnelId`.

Every inbound datagram has to go through `Interface::receive`.

Under the hood, `sanstun` does a lot of things for you here. It parses the tunnel index and extracts the upper 24 bits, it applies rate limits, generates cookie replies and validates handshake messages.

Crucially, the constructors for types like `HandshakeInitiation` are all crate-private. Even though `Tunnel` exposes a public `handle_handshake_initiation` function, the only way to call that is by going through `Interface::receive` first to obtain such a type. And `Interface::receive` protects your application by applying the rate limiter, all without you having to think about it.

## [Parallel crypto](https://www.firezone.dev/blog/introducing-sanstun#parallel-crypto)

One of the best features of `sanstun` is its parallel-friendly API for encrypting and decrypting packets.

On a protocol level, WireGuard doesn't share any state between tunnels. Therefore, parallelizing a WireGuard implementation across peers is very easy.

But what about parallelizing work within a single tunnel? WireGuard operates on layer 3, and thus a single session with another peer carries the traffic of many individual network flows. Even if we handle tunnels to different peers on different threads, all network flows (i.e., unique 5-tuples) toward the same host or subnet will be processed sequentially, effectively bounded by how fast the CPU is at ChaCha20-Poly1305, WireGuard's cipher for data packets. To protect against replay by malicious on-path observers, WireGuard data packets have a monotonically increasing packet counter. This counter is scoped to a single session and therefore would require synchronization if we were to access this state from multiple threads simultaneously.

To avoid this, `sanstun` separates the bookkeeping from the actual encryption and decryption. Calling `Tunnel::begin_encapsulate` doesn't in fact encrypt anything. It checks for the current session, reserves a packet counter, advances timer state and then hands you a `PendingEncapsulate`:

```rust
impl PendingEncapsulate {
    pub fn encrypt<'a>(self, plaintext: &[u8], dst: &'a mut [u8]) -> Result<DataMessage<'a>, Error> {
        // ...
    }
}
```

`PendingEncapsulate` implements `Send` and can thus be transferred to another thread to perform the bulk of the work. Running `cargo bench data_path` in the `sanstun` repository showcases the effect of this split:

```
data_path/64B/begin_encapsulate
                        time:   [20.785 ns 20.828 ns 20.869 ns]
data_path/64B/encrypt   time:   [226.15 ns 226.22 ns 226.29 ns]
data_path/64B/begin_decapsulate
                        time:   [37.600 ns 37.675 ns 37.744 ns]
data_path/64B/decrypt   time:   [205.78 ns 205.83 ns 205.87 ns]
data_path/64B/commit_decapsulate
                        time:   [17.486 ns 17.529 ns 17.575 ns]

data_path/1280B/begin_encapsulate
                        time:   [20.785 ns 20.828 ns 20.869 ns]
data_path/1280B/encrypt time:   [933.49 ns 933.54 ns 933.59 ns]
data_path/1280B/begin_decapsulate
                        time:   [37.606 ns 37.681 ns 37.751 ns]
data_path/1280B/decrypt time:   [914.81 ns 914.89 ns 914.97 ns]
data_path/1280B/commit_decapsulate
                        time:   [17.657 ns 17.707 ns 17.764 ns]
```

As expected, the bookkeeping parts (`begin_encapsulate`, `begin_decapsulate` and `commit_decapsulate`) do not scale with packet size. What does scale with packet size are the `encrypt` and `decrypt` functions. At a packet size of 1280 bytes, the bookkeeping part of encrypting a new packet is only 2%. If we move the actual encryption to another thread, this hints at a potential speedup of more than 45x!

An effective speedup of 45x is hard to achieve in practice because it ignores the cost of passing `PendingEncapsulate` to another thread and assumes no other overhead is introduced. Nevertheless, being able to offload the crypto to worker threads allows for some great throughput improvements.

The owner thread only does the bookkeeping, while encryption and decryption run on worker threads.

Being a sans-IO library, `sanstun` doesn't concern itself with how exactly you want to parallelize your crypto. If you want to keep it simple, you can also just call `encrypt` and `decrypt` inline and not bother with parallelization at all.

The key thing we wanted to achieve with `sanstun`'s API design was to enable parallel crypto while keeping everything around `Interface` single-threaded. This lets the internal rate limiter work without locks or atomic counters and take advantage of Rust's borrow checker, all while combining state from multiple tunnels.

## [Closing](https://www.firezone.dev/blog/introducing-sanstun#closing)

With `sanstun`, our fork at [`firezone/boringtun`](https://github.com/firezone/boringtun) is now deprecated. If you are building on top of WireGuard, whether that's a VPN, a mesh network or something else entirely, we hope `sanstun` makes your life a little easier. It is available on [crates.io](https://crates.io/crates/sanstun).

`sanstun` is open source under the Apache 2.0 license. Check it out on [GitHub](https://github.com/firezone/sanstun); issues and pull requests are welcome!
