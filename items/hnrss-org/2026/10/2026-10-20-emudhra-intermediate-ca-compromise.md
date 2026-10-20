---
title: EMudhra Intermediate CA Compromise
link: https://emudhra.we-have-a-problem.com/
source: hnrss-org
published: 2026-10-20T13:58:33Z
updated: 2026-10-20T13:58:33Z
first_seen: 2026-10-20T17:55:58.869157456Z
authors:
- galadran
content: extracted
html: 2026-10-20-emudhra-intermediate-ca-compromise.html
---

The private keys of eight eMudhra intermediate CA certificates have been compromised. They chain to roots trusted by the Mozilla, Microsoft, Apple and Chrome root programs and by many downstream trust stores.

## How do we know?

To demonstrate that the keys are compromised, the original reporter Wayne signed the following message with the private key of each of the eight intermediate CAs and reported it to the Root Program Teams on the 17th of October:

> We have a problem here - Wayne

[proof.zip](https://emudhra.we-have-a-problem.com/proof.zip) contains the message, and for each intermediate the certificate, its public key and a signature on the message. To verify with OpenSSL:

```
openssl x509 -in EM-DV-TLS-CA-G3B.crt.pem -noout -pubkey | diff - EM-DV-TLS-CA-G3B.pub.pem
openssl dgst -sha384 -verify EM-DV-TLS-CA-G3B.pub.pem -signature EM-DV-TLS-CA-G3B.sig message.txt
```

## What actions have been taken?

The root programs and many downstream trust stores have taken action to protect their users. The certificates have been blocked in Firefox and Chrome since the 17th of October, in Apple devices since the 19th and in Microsoft since the 20th.

The issue was responsibly disclosed to downstream trust stores on the 19th of October. Many have already released updates or are in the process of releasing updates.

## What are the consequences?

An attacker with the private key of any of the compromised intermediate CAs could impersonate any website or service.

This has already been remediated by Mozilla, Microsoft, Apple, and Chrome for their users through their tools for distributing revocation information such as [OneCRL](https://blog.mozilla.org/security/2015/03/03/revoking-intermediate-certificates-introducing-onecrl/), [CRLite](https://hacks.mozilla.org/2025/08/crlite-fast-private-and-comprehensive-certificate-revocation-checking-in-firefox/) and [CRLSets](https://www.chromium.org/Home/chromium-security/crlsets/).

The CA has also revoked the compromised intermediate certificates. However, many other clients do not consume revocation information and require software updates to ensure they no longer trust the compromised intermediates.

## Does your client still trust the compromised intermediates?

| Intermediate CA | Chains to | Result |
| --------------- | --------- | ------ |

To check curl or other command-line clients, run:

```
curl https://dv-g3b-emudhra.we-have-a-problem.com/
curl https://dv-g3b-aaa-emudhra.we-have-a-problem.com/
```

A certificate error means the client does not trust the compromised intermediates.

To check a trust store offline, [chains.zip](https://emudhra.we-have-a-problem.com/chains.zip) contains the certificate chain served by each of the twelve hosts above in PEM format, with instructions for verifying them with OpenSSL.

## What actions can users take?

Users should update their browsers and operating systems to the latest versions as soon as possible.

### What about developer software and packages?

| Software                              | Patched version                                                                                                                                                               |
| ------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Ubuntu ca-certificates                | [20260816ubuntu0.22.04.1, 20260816ubuntu0.24.04.1, 20260816ubuntu0.26.04.1, 20260816ubuntu0.26.10.1](https://bugs.launchpad.net/ubuntu/+source/ca-certificates/+bug/2171507) |
| Go                                    | [1.27.3, 1.26.10](https://groups.google.com/g/golang-announce/c/66pojueaQek)                                                                                                 |
| Electron                              | [44.9.1, 43.8.1, 42.11.16](https://github.com/electron/electron/releases/tag/v44.9.1)                                                                                        |
| Python certifi                        | [2026.10.19](https://github.com/certifi/python-certifi/releases/tag/2026.10.19)                                                                                              |
| Rust rustls-webpki                    | [0.103.16](https://github.com/rustls/webpki/releases/tag/v%2F0.103.16)                                                                                                       |
| Rust webpki-roots / webpki-root-certs | [1.0.10](https://github.com/rustls/webpki-roots/releases/tag/v%2F1.0.10)                                                                                                     |
| Homebrew ca-certificates              | [2026-09-25\_1](https://github.com/Homebrew/homebrew-core/blob/d02b8762f4a29859c47a635a0d2683223fea9812/Formula/c/ca-certificates.rb)                                        |
| Node.js                               | [pending](https://github.com/nodejs/node/pull/66871)                                                                                                                         |
| Composer ca-bundle                    | [pending](https://github.com/composer/ca-bundle/pull/154)                                                                                                                    |
| NSS                                   | [3.131.1](https://groups.google.com/a/mozilla.org/g/dev-tech-crypto/c/vSxZxnOMMvs)                                                                                           |
| uv                                    | [0.13.4](https://github.com/astral-sh/uv/releases/tag/0.13.4)                                                                                                                |

Add more at [github.com/galadran/we-have-a-problem.com](https://github.com/galadran/we-have-a-problem.com).

### Are there workarounds for macOS curl?

macOS blocks the compromised intermediates, but the built-in `/usr/bin/curl` verifies certificates against `/etc/ssl/cert.pem` without checking revocation, and that file has not been updated. To remove the affected roots from it, run:

```
sudo cp /etc/ssl/cert.pem /etc/ssl/cert.pem.bak &&
awk '/^(###|===) /{skip=0}
  /^=== .*\/CN=(AAA Certificate Services|emSign ECC Root CA - G3)$/{skip=1}
  !skip' /etc/ssl/cert.pem.bak | sudo tee /etc/ssl/cert.pem > /dev/null
```

### Are there workarounds for Linux?

Until a distribution ships an updated `ca-certificates` package, expert users can distrust the affected roots in the system trust store. This is NOT recommended for ordinary users.

On Debian and Ubuntu, run as root:

```
sed -i \
  -e 's|^mozilla/emSign_ECC_Root_CA_-_G3.crt|!&|' \
  -e 's|^mozilla/Comodo_AAA_Services_root.crt|!&|' \
  /etc/ca-certificates.conf && update-ca-certificates
```

On Alpine, run as root:

```
apk add ca-certificates &&
sed -i -e 's|^mozilla/emSign_ECC_Root_CA_-_G3.crt|!&|' \
  /etc/ca-certificates.conf && update-ca-certificates
```

On Fedora, RHEL, CentOS Stream, Rocky Linux, AlmaLinux and Amazon Linux 2023, run as root:

```
d=/etc/pki/ca-trust/source/blocklist; [ -d $d ] || d=/etc/pki/ca-trust/source/blacklist
trust extract --overwrite --format=pem-bundle \
  --filter='pkcs11:object=Comodo%20AAA%20Services%20root;type=cert' \
  $d/Comodo_AAA_Services_root.pem &&
trust extract --overwrite --format=pem-bundle \
  --filter='pkcs11:object=emSign%20ECC%20Root%20CA%20-%20G3;type=cert' \
  $d/emSign_ECC_Root_CA_-_G3.pem &&
update-ca-trust
```

On openSUSE and SLES, run as root:

```
d=/etc/pki/trust/blocklist; [ -d $d ] || d=/etc/pki/trust/blacklist
trust extract --format=pem-bundle \
  --filter='pkcs11:object=Comodo%20AAA%20Services%20root;type=cert' \
  $d/Comodo_AAA_Services_root.pem &&
trust extract --format=pem-bundle \
  --filter='pkcs11:object=emSign%20ECC%20Root%20CA%20-%20G3;type=cert' \
  $d/emSign_ECC_Root_CA_-_G3.pem &&
update-ca-certificates
```

On Arch Linux, run as root:

```
trust extract --format=pem-bundle \
  --filter='pkcs11:object=emSign%20ECC%20Root%20CA%20-%20G3;type=cert' \
  /etc/ca-certificates/trust-source/blocklist/emSign_ECC_Root_CA_-_G3.pem &&
update-ca-trust
```

On NixOS, add to your configuration:

```
security.pki.caCertificateBlacklist = [
  "Comodo AAA Services root"
  "emSign ECC Root CA - G3"
];
```

### Are there workarounds for NetBSD?

On NetBSD 10 and later, run as root:

```
certctl untrust /usr/share/certs/mozilla/all/emSign_ECC_Root_CA_-_G3.pem &&
certctl untrust /usr/share/certs/mozilla/all/Comodo_AAA_Services_root.pem
```

## What actions can trust store operators take?

Please see [this thread](https://groups.google.com/a/mozilla.org/g/dev-security-policy/c/0R3XLJL6FuE) in Mozilla's dev-security-policy forum.

## Which intermediates were compromised?

[impacted-intermediates.json](https://emudhra.we-have-a-problem.com/impacted-intermediates.json) lists the eight intermediates and their Sectigo cross-signs. [roots.json](https://emudhra.we-have-a-problem.com/roots.json) lists the roots they chain to.

* * *

Site published by [Dennis Jackson](https://github.com/dennisjackson). The source is on [GitHub](https://github.com/galadran/we-have-a-problem.com) and PRs are welcome.
