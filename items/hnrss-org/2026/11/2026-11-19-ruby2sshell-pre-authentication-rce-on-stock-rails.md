---
title: 'Ruby2ßhell: Pre-authentication RCE on stock Rails'
link: https://verialabs.com/ruby2shell/
source: hnrss-org
published: 2026-11-19T05:09:11Z
updated: 2026-11-19T05:09:11Z
first_seen: 2026-11-19T17:16:58.860737333Z
authors:
- jdns
content: extracted
html: 2026-11-19-ruby2sshell-pre-authentication-rce-on-stock-rails.html
remote_preview:
  url: https://verialabs.com/og/ruby2shell.png
  alt: Ruby2Shell (GHSA-34m4-m9fh-gqp5)
---

A one-byte heap overflow to remote code execution in Ruby's string case conversion. Reachable without authentication in the default configuration of common Ruby web frameworks and self-hosted software.

## Affected software

### Ruby versions

| Ruby series   | Affected                       | Fixed in   |
| ------------- | ------------------------------ | ---------- |
| 4.0           | **4.0.0** to **4.0.9**         | **4.0.10** |
| 3.4           | **3.4.0** to **3.4.12**        | **3.4.13** |
| 3.3           | **3.3.0** to **3.3.13**        | **3.3.14** |
| 3.2 and older | All releases back to **2.4.0** | EOL        |

### Your deployment

The bug is reachable from the following frameworks in their default configuration. Other frameworks are not exposed by default, but **application code that upcases strings received in a legacy encoding may also be vulnerable.**

| Framework                       | Status on an affected Ruby |
| ------------------------------- | -------------------------- |
| Rails                           | Affected                   |
| Sinatra (classic style)         | Affected                   |
| Other frameworks and plain Rack | Not exposed by default     |

### Self-hosted software

The latest version of the following popular software bundles an affected Ruby.

| Software                                    | Bundled Ruby | Status   |
| ------------------------------------------- | ------------ | -------- |
| GitLab **19.6.0**                           | **3.3.13**   | Affected |
| GitHub Enterprise Server **3.22.5**         | **4.0.9**    | Affected |
| Mastodon **4.7.6**                          | **4.0.10**   | Fixed    |
| OpenProject **17.9.8**                      | **4.0.10**   | Fixed    |
| Discourse, base image **2.0.20261119-0203** | **4.0.10**   | Fixed    |
| Chatwoot **4.20.0**                         | **3.4.4**    | Affected |

Updated 19 November 2026 at 16:59 UTC.

### Your deployment

Upgrade Ruby to a fixed release: **3.3.14**, **3.4.13** or **4.0.10**.

### Self-hosted software

Install the vendor's fixed release. See [Self-hosted software](https://verialabs.com/ruby2shell/#self-hosted) for fixed versions of popular products.

## Technical details

### Root cause

Most of Ruby's single-byte encodings have no capital `ß`. Upcasing it writes two bytes, `SS`, but the loop only bounds-checks one (`to < to_end`).

```c
1while (*pp < end && to < to_end) {    /* room for one byte */2    code = *(*pp)++;3    if (code == SHARP_s) {4        if (flags & ONIGENC_CASE_UPCASE) {5            *to++ = 'S';              /* first byte */6            code = 'S';7        }8        /* ... */9    }10    /* ... */11    *to++ = code;                     /* second byte, unchecked */12}
```

The output buffer is allocated as the input length plus 20 bytes. Each `ß` adds one byte to the output, so an input with 21 of them overflows the buffer by one byte.

The same loop is in several ISO-8859 and Windows-125x encodings and has been there since Ruby 2.4.0. It is reached by `upcase`, `swapcase`, `downcase(:fold)` and `casecmp?`.

UTF-8 strings take a different code path and are not affected.

Fixed upstream in [`ruby/ruby@d582f8f`](https://github.com/ruby/ruby/commit/d582f8f13d700d8e39714d3a8f11eb4bd2bc69f0).

### Primitive

A one-byte heap overflow. The byte written is `S` (`0x53`) when upcasing or `s` (`0x73`) when case-folding, and it lands directly after the output buffer. The attacker controls the size of the buffer through the input length.

Copying the result out of the buffer then reads one byte out of bounds.

## Proof of concept

We'll be publishing a proof of concept in a week, once people have had time to upgrade.
