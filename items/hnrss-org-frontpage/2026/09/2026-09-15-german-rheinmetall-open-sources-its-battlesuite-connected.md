---
title: German Rheinmetall open-sources its Battlesuite connected weapon system protcol
link: https://rheinmetall.github.io/onboardapi-documentation/9.10.0/index.html
source: hnrss-org-frontpage
published: 2026-09-15T21:07:47Z
updated: 2026-09-15T21:07:47Z
first_seen: 2026-09-16T01:36:55.400042967Z
authors:
- summarity
summary: 'Article URL: https://rheinmetall.github.io/onboardapi-documentation/9.10.0/index.html Comments URL: https://news.ycombinator.com/item?id=49718928 Points: 125 # Comments: 40'
content: extracted
html: 2026-09-15-german-rheinmetall-open-sources-its-battlesuite-connected.html
preview:
  file: 2026-09-15-german-rheinmetall-open-sources-its-battlesuite-connected.preview-8ba397325d81.webp
  width: 181
  height: 256
  alt: Logo
  color: '#003453'
images:
- source: https://rheinmetall.github.io/onboardapi-documentation/9.10.0/rhm_logo.svg
  original:
    file: 2026-09-15-german-rheinmetall-open-sources-its-battlesuite-connected.image-e5e70df21df0.png
    width: 1095
    height: 1552
  variants:
  - file: 2026-09-15-german-rheinmetall-open-sources-its-battlesuite-connected.image-093e4acf5590.webp
    width: 320
    height: 454
  - file: 2026-09-15-german-rheinmetall-open-sources-its-battlesuite-connected.image-c29a89ca17eb.webp
    width: 640
    height: 907
  - file: 2026-09-15-german-rheinmetall-open-sources-its-battlesuite-connected.image-a8066788d6fa.webp
    width: 960
    height: 1361
  - file: 2026-09-15-german-rheinmetall-open-sources-its-battlesuite-connected.image-d2f788793cfc.webp
    width: 1095
    height: 1552
  color: '#007dc0'
---

The onboardapi interface library and middleware is designed for seamless communication between sensor systems and software components. It provides a standardized data model that ensures interoperability across complex hardware and software environments.

Built on the ddkit software development kit, this library utilizes the Data Distribution Service (DDS) standard by the Object Management Group (OMG). This data-centric publish-subscribe architecture guarantees reliable, low-latency data exchange for high-demand applications.

By leveraging DDS XTypes and XCDR2 encoding, this library ensures full backward compatibility. This allows different versions of your software to coexist and communicate seamlessly, even as the data model evolves.

While the core library is provided in C++, the API supports multi-language integration via wrappers for Java, C# / .NET, and Python.

**Getting started**

- [Concepts](https://rheinmetall.github.io/onboardapi-documentation/9.10.0/concepts.html)
  - Explore the Client/Service architecture and underlying logic
- [Examples](https://rheinmetall.github.io/onboardapi-documentation/9.10.0/impl_example.html)
  - [C++](https://rheinmetall.github.io/onboardapi-documentation/9.10.0/impl_example.html)
  - [Python](https://rheinmetall.github.io/onboardapi-documentation/9.10.0/wrappers/python/index.html)
  - [C# / .NET](https://rheinmetall.github.io/onboardapi-documentation/9.10.0/wrappers/dotnet/index.html)
  - [Java](https://rheinmetall.github.io/onboardapi-documentation/9.10.0/wrappers/java/index.html)
- [Configuration](https://rheinmetall.github.io/onboardapi-documentation/9.10.0/configuration.html)
  - Learn how to configure the library
- [Interfaces](https://rheinmetall.github.io/onboardapi-documentation/9.10.0/interfaces.html)
  - Browse the Data Model and all interfaces
- [Changelog](https://rheinmetall.github.io/onboardapi-documentation/9.10.0/changenotes.html)
  - Stay up to date with the latest features

**Licenses / Disclaimer**

- Interface descriptions: [EPL v2.0](https://rheinmetall.github.io/onboardapi-documentation/9.10.0/epl.html)
- Runtime libraries: [EULA-RME-SDK-1.0](https://rheinmetall.github.io/onboardapi-documentation/9.10.0/rme_sdk.html)
