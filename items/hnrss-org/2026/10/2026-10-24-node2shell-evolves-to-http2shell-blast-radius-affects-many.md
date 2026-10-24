---
title: Node2Shell evolves to HTTP2Shell, blast radius affects many HTTP libs and apps
link: https://www.aikido.dev/blog/python-packages-vulnerable-node2shell-brotli-cve-2026-111497
source: hnrss-org
published: 2026-10-24T08:06:39Z
updated: 2026-10-24T08:06:39Z
first_seen: 2026-10-24T19:24:41.714243814Z
authors:
- theanonymousone
content: extracted
html: 2026-10-24-node2shell-evolves-to-http2shell-blast-radius-affects-many.html
remote_preview:
  url: https://cdn.prod.website-files.com/642adcaf364024654c71df23/6adbe4dc7746ea7df4365811_Node-JS_OG.jpg
---

## **TL;DR**

#### ⚠️ Brotli is a widely used compression library from Google. On Wednesday, October 21st NodeJS disclosed CVE-2026-111497, dubbed “node2shell,” wherein a Node.js process that decompresses attacker-controlled Brotli data can be made to leak memory or run arbitrary code. On Thursday, Python disclosed that Python packages were vulnerable to CVE-2026-111497 as well.

On Friday, October 23rd, it has become known that this CVE affects other major languages and service applications using HTTP, where patches are not yet fully available.

Check if you are decoding Brotli as a dependency within your libraries. Stay on guard for wider announcements and be ready to upgrade as soon as possible. If there is no patch available for a library or application that may be affected, we recommend disabling Brotli decoding until patches become available.

Update 4:12PM PT:

 Patches are available for [libbrotli](https://intel.aikido.dev/cve/AIKIDO-2026-100918), [Python](https://intel.aikido.dev/cve/AIKIDO-2026-166481), [C++](https://intel.aikido.dev/cve/AIKIDO-2026-425328), [NodeJS](https://intel.aikido.dev/cve/AIKIDO-2026-303617), [bun](https://github.com/oven-sh/bun/security/advisories), [Envoy](https://github.com/envoyproxy/envoy/security/advisories/GHSA-3wvj-j6qm-62mr) and more coming. Apache and Nginx do not decode brotli by default.

#### **Update 4:15 PM PT:** ;OpenAI shares that [they discovered this vulnerability](https://x.com/OpenAI/status/2113766354386084338?s=20) in February. While the root cause issue was fixed in the Brotli source code at the time, it wasn’t known that it gave rise to a security vulnerability. OpenAI's advisories [here](https://patchtheplanet.org/brotli).

![](https://cdn.prod.website-files.com/642adcaf364024654c71df23/6adbe4dc7746ea7df4365811_Node-JS_OG.jpg)

## What is Brotli?

Brotli is a lossless compression algorithm and data format developed by Google. It’s a way to make data smaller for storage or sending over the internet. Software then unpacks it to recover the original data. It’s widely used for HTTP compression to compress web content such as HTML, CSS, and JavaScript.

## What is CVE-2026-111497?

[CVE-2026-111497](https://app.opencve.io/cve/CVE-2026-111497) is an out-of-bounds write in the Google Brotli decoder affecting versions 1.0.3 through 1.2.0 on all platforms. This is the same Brotli decoder flaw behind Node2Shell announced a couple days ago. A remote, unauthenticated attacker can send a crafted Brotli-compressed stream to corrupt memory, leak sensitive data, crash the process, or potentially execute arbitrary code. It carries a CVSS v4.0 score of 9.2 (Critical), and Node.js is affected through its bundled Brotli library.

A bug makes the Brotli decoder keep accepting more bytes when it should reject the data. Those bytes can overflow a small working buffer and damage the program’s memory. An attacker can use that damage to control what runs, such as their own code.

[OpenAI found this in February](https://patchtheplanet.org/brotli) reported it as unauthenticated remote code execution that works in both directions, so a malicious client can take over an unpatched server and a malicious server can take over an unpatched client that accepts compressed responses.

The root cause was fixed in the Brotli source code in February, but it wasn't recognized as a security issue at the time and no release shipped with the fix until now. That means every project that bundles Brotli has to ship its own update.

You’re exposed if any environment has Brotli 1.2.0 or earlier or brotlicffi 1.2.0.x installed, since HTTP clients such as requests, httpx, and aiohttp load these packages automatically when they’re present. Upgrade to Brotli 1.2.1 or brotlicffi 1.2.1.0 and rebuild any container images that bundle them. As of October 23, advisory databases don’t map the CVE to either PyPI package, so pip-audit, Dependabot, and Safety won’t flag vulnerable versions yet.

You can see what we’ve tracked here in Aikido Intel.

- [Python](https://intel.aikido.dev/cve/AIKIDO-2026-166481)
- [C++](https://intel.aikido.dev/cve/AIKIDO-2026-425328)
- [Libbrotil](https://intel.aikido.dev/cve/AIKIDO-2026-100918)
- [NodeJS](https://intel.aikido.dev/cve/AIKIDO-2026-303617)

As for unaffected software, Apache’s standard `mod_brotli` does not decode Brotli

For CVE-2026-111497, update the affected runtime or Brotli library:

- **Node.js:** upgrade to **22.23.4, 24.22.1, or 26.12.1**, or a newer supported release in the corresponding release line.
- **Native Brotli / libbrotli:** upgrade to **1.2.1 or later**. For older release lines, use patched **1.0.10** or **1.1.1**.
- **Python packages:** upgrade Brotli to **1.2.1 or later** and brotlicffi to **1.2.1.0 or later**, wherever installed.
- **curl / libcurl:** update its Brotli dependency to **1.2.1**, a patched older release, or the vendor’s security-fixed package. Bundled or statically linked copies require an updated application build.
- **Bun:** upgrade to **1.4.4 or later**. Rebuild compiled executables that bundle Bun.
- **Envoy:** upgrade to **1.40.0, 1.39.3, 1.38.6, or 1.37.8**, or a newer release in a supported branch. **1.36.12** also includes the fix, but the 1.36 branch is unsupported.
- **Istio:** the fix is included in **1.29.9, 1.30.6, and 1.31.2**. Use a newer supported release in the corresponding line; the 1.29 branch is end of life.
- **Operating-system packages:** install the vendor’s security update. Vendors may backport the fix without changing the upstream version number.
- **Other HTTP servers, reverse proxies, API gateways, and HTTP clients:** update the affected runtime or Brotli library they use. Where Brotli is bundled into the product, install the product vendor’s release that explicitly addresses this CVE. For managed services, confirm remediation with the provider.

Update dependency locks, rebuild affected applications and container images, redeploy, and restart services. Verify that running workloads use the patched components.

Fixed releases, reviewed October 23, 2026
| Project                  | Patch status    | Fixed versions                    | Notes                                                                           |
| ------------------------ | --------------- | --------------------------------- | ------------------------------------------------------------------------------- |
| Node.js                  | Patch available | 22.23.4 · 24.22.1 · 26.12.1       | Use a supported release line.                                                   |
| Bun                      | Patch available | 1.4.4                             | Includes Brotli 1.2.1.                                                          |
| Envoy                    | Patch available | 1.40.0 · 1.39.3 · 1.38.6 · 1.37.8 | 1.36.12 also includes the fix, but 1.36 is unsupported. Use a supported branch. |
| Istio                    | Patch available | 1.29.9 · 1.30.6 · 1.31.2          | 1.29 is end of life, so move to a supported line.                               |
| Google Brotli (upstream) | Patch available | 1.0.10 · 1.1.1 · 1.2.1            | Choose the fixed release for your library branch.                               |

## What to do if you can't upgrade yet

These workarounds reduce exposure while you prepare the upgrade:

- **Python applications:** where Brotli support is optional, uninstall both Brotli and brotlicffi from affected environments. Remove dependency extras such as httpx\[brotli\] and any other requirements that would reinstall them, then regenerate dependency locks and rebuild/restart workloads. HTTPX, Requests, and urllib3 retain gzip and deflate support without these optional packages. However, a server that sends Brotli anyway may produce an unreadable response or an application error; automatic fallback is not guaranteed.

  - Uninstalling both Python packages and restarting workloads removes the decoding paths provided by those packages for incoming and outgoing data. It does not remove separate Brotli decoders bundled into other runtimes, libraries, or products.
- **Incoming HTTP requests:** reject requests whose Content-Encoding includes br at the reverse proxy, load balancer, or application middleware, before any vulnerable decoder processes the body. Match br case-insensitively, including within lists of encodings.
- **Outbound HTTP requests:** remove br from Accept-Encoding. Also disable Brotli response decoding or reject responses whose Content-Encoding includes br before decoding begins. Ensure this check occurs before automatic body processing; checking after the client has downloaded and decoded the response is too late.

Changing Accept-Encoding alone is insufficient: a malicious server can send a Brotli response regardless of what the client advertises, and urllib3 selects its decoder from the response’s Content-Encoding header.

Keep these controls in place until affected components are patched and verified. Review coverage as vendors confirm additional affected components or attack paths.

## How Aikido helps with CVE-2026-111497

Aikido flags vulnerable versions of Brotli and brotlicffi in your repositories, [container images](https://www.aikido.dev/code/container-image-scanning), and dependencies, including when they arrive as transitive dependencies. Your dashboard shows which services still run a vulnerable version, so you can upgrade the ones that call outside servers or accept compressed request bodies first.

Once you've upgraded, Aikido checks if the finding is gone.

## The timeline

Brotli is a widely used compression library from Google. On Wednesday, October 22nd NodeJS disclosed CVE-2026-111497, dubbed “node2shell,” wherein a Node.js process that decompresses attacker-controlled Brotli data can be made to leak memory or run arbitrary code. Users were exposed if their app or a library decompresses bad Brotli data (e.g. the built-in fetch(), body-parser).

The next day, Python released a security update that patches the same flaw; Python packages were also vulnerable to CVE-2026-111497. Brotli flaw may affect even more languages and service applications, given how widely integrated the library is downstream. [It turns out OpenAI had found this back in February.](https://x.com/gdb/status/2113773790207283353?s=20)

## Techincal details of CVE-2026-111497

The flaw sits in`DecodeBlockTypeAndLength()`, which handles block switches, the points where a Brotli stream changes which Huffman tree it uses. In vulnerable versions the function returns a plain boolean, and BROTLI\_FALSE can mean either "this stream is malformed" or "I need more input." Callers can't tell the two apart, and on the fast path the decoder discards the return value entirely.

A crafted stream can declare a single block type and then exhaust that block's length, which forces a switch that can't succeed. The decoder then either reports that it needs more input for a stream that's complete, or keeps decoding with the block state it already had. That second path leads to the out-of-bounds write.

The upstream fix,[google/brotli commit 0ed4268](https://github.com/google/brotli/commit/0ed4268a4401abcb44720963), makes the function return an error code and adds a new error, BROTLI\_DECODER\_ERROR\_FORMAT\_BLOCK\_SWITCH (-17). Decoding now stops on a format error on both the safe and fast paths. OSM diffed the Brotli 1.2.0 and 1.2.1 source distributions and found that 1.2.1 differs only in the version bump and the decoder changes from that commit.

## Brotli vulnerabilities in Python libraries (CVE-2026-111497)

The Brotli flaw behind [Node2Shell](https://www.aikido.dev/blog/node2shell-nodejs-brotli-rce-cve-2026-111497) (CVE-2026-111497)  affects Python. Two PyPI packages, Brotli and brotlicffi, compile the same vulnerable Brotli decoder that Node.js bundles. [An official advisory was posted on GitHub](https://github.com/advisories/GHSA-9v8h-qqgm-4m35). Brotli has **82.8 million downloads** per month.

You're affected if any Python environment you run has Brotli 1.2.0 or earlier, or brotlicffi 1.2.0.x, installed. Popular HTTP clients such as requests and httpx load these packages automatically **when they're present**. Some packages automatically install Brotli as a dependency such as Scrapy, while others require explicit mention of it. An attacker can reach the vulnerable decoder by sending a malicious Brotli response to a request your app makes, or by sending your app a request body compressed with Brotli. That means an app can be exposed without importing Brotli anywhere in its own code. **It’s a popular transitive dependency, so check your systems.**

To fix it, upgrade to the patched releases:

- Brotli -> 1.2.1
- brotlicffi -> 1.2.1.0

The release notes for both fixed versions don't mention the CVE. As of October 23, advisory databases don't map it to either PyPI package yet, so pip-audit, Dependabot, and Safety won't flag these versions.

### **Who is affected by CVE-2026-111497 in Python?**

You’re affected if any of your Python environments have one of these packages installed:

| Package           | Affected versions | Fixed version |
| ----------------- | ----------------- | ------------- |
| Brotli (PyPI)     | 1.2.0 and earlier | 1.2.1         |
| brotlicffi (PyPI) | 1.2.0.x           | 1.2.1.0       |

Your code doesn’t have to import either package. These HTTP clients advertise Accept-Encoding: br and decode Brotli responses automatically:

- urllib3 (and therefore requests) when Brotli or brotlicffi is installed
- httpx with the brotli extra
- aiohttp when Brotli is installed
- Scrapy, and anything built on the clients above

Attacker data can reach the decoder through two routes:

- Outbound requests. Your app fetches a Brotli response from a server an attacker controls, such as through a webhook delivery, URL preview, crawler or “fetch this URL” feature.
- Inbound requests. Your framework or middleware decodes a request body sent with Content-Encoding: br, which is the Python equivalent of the Express 5 case in Node.js.

### **How to fix CVE-2026-111497 in Python**

The fix is a package upgrade, and it takes four steps:

1. Upgrade whichever package you have installed with pip install -U "Brotli>=1.2.1" or pip install -U "brotlicffi>=1.2.1.0". If you use a lockfile with Poetry, uv, or pip-tools, update the constraint and regenerate the lockfile.
2. Rebuild your container images and any wheels that bundle either package. An image built on a vulnerable version stays vulnerable until you rebuild it.
3. Check every environment you run, including laptops, CI runners, servers and containers. Many projects pull in Brotli through an HTTP client extra, so look at your full resolved dependency tree.
4. Rely on a manual check for now. pip-audit, Dependabot and Safety won't report this until the advisory databases map the CVE to the PyPI packages.

If your Python Brotli bindings came from a Linux distribution package instead of PyPI, the fix comes through your distribution, so check its security advisory as well.

#### **What is the Python Brotli vulnerability (CVE-2026-111497)?**

**Python Brotli vulnerability** refers to CVE-2026-111497, the Brotli decoder flaw known as [Node2Shell](https://www.aikido.dev/blog/node2shell-nodejs-brotli-rce-cve-2026-111497), as it affects Python packages on PyPI. The CVE record pairs CWE-252 (unchecked return value) with CWE-787 (out-of-bounds write).

#### **How serious is CVE-2026-111497 for Python, and is it being exploited?**

The CVE record gives the flaw a CVSS 4.0 score of 9.2, and Node.js rates it high severity. OSM built Brotli 1.2.0 and 1.2.1 with AddressSanitizer and ran a crafted stream through both. Version 1.2.0 consumed all of the roughly 23 MB stream and kept reporting that it needed more input, whether the stream arrived in one piece or in chunks as small as one byte. Version 1.2.1 rejected the stream with error -17 every time.

OSM didn't pursue memory corruption or code execution in the Python extension. The decoder code is the same code that researchers exploited against an unmodified Node.js 24.22.0 build, as [Imperva](https://www.imperva.com/blog/node2shell-from-brotli-data-to-remote-code-execution/) and others reported. For Node.js, The Hacker News reported no exploitation in the wild as of October 22.

Treat it as urgent anyway. The bug is a memory-safety flaw in a component that handles untrusted network input, and the fix is a routine version bump. Services that call outside servers or accept compressed request bodies should go first.

### **How to check if your Python app is exposed**

1. Check installed versions with pip show Brotli brotlicffi in each environment, and with docker run --rm \<image> pip show Brotli brotlicffi for each container image.
2. Find what pulls the packages in. Run pipdeptree -r -p Brotli to see which dependencies require it, and search your lockfiles with grep -i brotli poetry.lock uv.lock requirements\*.txt.
3. List the outbound HTTP calls that go to servers you don't control, and any endpoints that decode Content-Encoding: br request bodies.
4. Upgrade the services that match step 3 first, then the rest.

If you're reproducing the bug from Python, brotli.decompress() raises a generic brotli.error: decoder failed for any error, so you can't see which error code fired. A Decompressor() fed the crafted stream incrementally will never finish on 1.2.0.

## **FAQ**

**Which versions fix CVE-2026-111497 in Python?**

Brotli 1.2.1 and brotlicffi 1.2.1.0, along with any newer releases.

### **Do I need to import Brotli in my code to be affected?**

An installed package is enough. urllib3, requests, httpx and aiohttp use Brotli or brotlicffi automatically when either is present and decode Brotli responses from any server your app calls.

### **Will pip-audit or Dependabot flag this?**

Not yet. As of October 23, the CVE record, OSV and the GitHub Advisory Database don't list either PyPI package. OSM is submitting PYSEC entries and a PyPI package mapping for the GitHub advisory so that tooling picks this up.

### **Is there a workaround?**

Uninstalling Brotli and brotlicffi removes the vulnerable decoder, and HTTP clients fall back to gzip and deflate. Rejecting Brotli-encoded requests at the edge reduces inbound exposure, but it doesn't protect against Brotli responses your app fetches.
