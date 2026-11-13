---
title: Lonewolf 0.3 – A standalone XMPP server written in Rust
link: https://github.com/lonewolf-im/lonewolf/releases/tag/v0.3.0
source: hnrss-org
published: 2026-11-13T14:28:40Z
updated: 2026-11-13T14:28:40Z
first_seen: 2026-11-13T21:55:15.639657374Z
authors:
- ortuman
content: extracted
html: 2026-11-13-lonewolf-0-3-a-standalone-xmpp-server-written-in-rust.html
remote_preview:
  url: https://opengraph.githubassets.com/bd4fda2fe540b4efaca7d0a05a6a6170e88682c39797d2e786035038ce16b587/lonewolf-im/lonewolf/releases/tag/v0.3.0
  alt: 'The big one in this release is federation: Lonewolf can now talk to other XMPP servers. We also added direct TLS and a handful of extensions that desktop clients rely on. Lonewolf is still pre-1.0 ...'
---

The big one in this release is federation: Lonewolf can now talk to other XMPP servers. We also added direct TLS and a handful of extensions that desktop clients rely on.

Lonewolf is still pre-1.0 and runs as a single node. Full XMPP compliance and clustering are on the [roadmap](https://github.com/lonewolf-im/lonewolf/blob/v0.3.0/docs/ROADMAP.md).

## What's new

### Federation

Your users can now exchange messages, presence, IQs, and roster subscriptions with people on other XMPP servers. If a host has a certificate and a public DNS name, federation is on by default, with nothing extra to configure.

Lonewolf authenticates peers by certificate only. The other server needs a trusted certificate and SASL EXTERNAL support. We don't support Server Dialback.

When the other server supports it, both directions share a single connection ([XEP-0288](https://xmpp.org/extensions/xep-0288.html)). Servers also tell each other their stanza size and idle limits up front ([XEP-0478](https://xmpp.org/extensions/xep-0478.html)).

Federation checks certificate revocation lists published by the CA, fetched over plain HTTP. If a CRL can't be downloaded, the connection goes ahead anyway. You can turn downloads off with `s2s.tls.fetch_crls = false`, or point `s2s.tls.crls_path` at a local file to use only that.

### Direct TLS

Clients and servers can now connect with TLS from the first byte ([XEP-0368](https://xmpp.org/extensions/xep-0368.html)), using new dedicated listeners. Outbound federation discovers both STARTTLS and direct TLS endpoints.

### More for desktop clients

- **Private XML Storage** ([XEP-0049](https://xmpp.org/extensions/xep-0049.html)) gives clients a place to keep private settings and legacy bookmarks.
- **Software Version** ([XEP-0092](https://xmpp.org/extensions/xep-0092.html)) answers "what server is this?" queries. It doesn't reveal the operating system unless you turn that on.

Both are enabled by default.

### Short-lived certificates

Use `letsencrypt.certificate_profile` to pick one of the profiles your CA advertises, including short-lived certificates. If you change the profile, Lonewolf renews early so the new one takes effect right away.

### SCRAM-SHA-512 (opt-in)

You can now enable SCRAM-SHA-512 and SCRAM-SHA-512-PLUS. Be aware that they come from an [expired Internet-Draft](https://datatracker.ietf.org/doc/html/draft-melnikov-scram-sha-512-05), not an RFC, and aren't registered with IANA.

Even if you leave SHA-512 login disabled, new accounts and password changes now store credentials that v0.2.0 can't read, so rolling back isn't straightforward.

See the [standards reference](https://github.com/lonewolf-im/lonewolf/blob/v0.3.0/docs/STANDARDS.md) for exactly what's supported and the [configuration reference](https://github.com/lonewolf-im/lonewolf/blob/v0.3.0/examples/lonewolf.toml) for every setting and its default.

## Build from source

Grab **Source code (tar.gz)** or **Source code (zip)** below, extract it, and run:

```shell
cargo build --release --locked -p lonewolf -p lonewolfctl -p lonewolfbench
```

The benchmark tool is now called `lonewolfbench` instead of `lonewolf-bench`, so update any scripts that call it. All you need is a stable Rust toolchain and a system linker on Linux or macOS. The [quick start](https://github.com/lonewolf-im/lonewolf/blob/v0.3.0/README.md#quick-start) walks you through setup.

**Full changelog:** [v0.2.0…v0.3.0](https://github.com/lonewolf-im/lonewolf/compare/v0.2.0...v0.3.0)

Licensed under [Apache-2.0](https://github.com/lonewolf-im/lonewolf/blob/v0.3.0/LICENSE).
