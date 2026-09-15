---
title: Alternatives to MinIO for single-node local S3
link: https://rmoff.net/2026/01/14/alternatives-to-minio-for-single-node-local-s3/
source: hnrss-org-frontpage
published: 2026-09-15T08:21:27Z
updated: 2026-09-15T08:21:27Z
first_seen: 2026-09-15T17:16:33.056020916Z
authors:
- rmoff
summary: 'Article URL: https://rmoff.net/2026/01/14/alternatives-to-minio-for-single-node-local-s3/ Comments URL: https://news.ycombinator.com/item?id=49709381 Points: 182 # Comments: 79'
content: extracted
html: 2026-09-15-alternatives-to-minio-for-single-node-local-s3.html
preview:
  file: 2026-09-15-alternatives-to-minio-for-single-node-local-s3.preview-f3a23add1255.webp
  width: 256
  height: 147
  color: '#4d6375'
images:
- source: https://rmoff.net/images/2026/01/h_IMG_3845.jpeg
  original:
    file: 2026-09-15-alternatives-to-minio-for-single-node-local-s3.image-92ba66343114.jpg
    width: 1800
    height: 1036
  color: '#171816'
- source: https://rmoff.net/images/2026/01/rustfs.excalidraw.png
  original:
    file: 2026-09-15-alternatives-to-minio-for-single-node-local-s3.image-fc0b6df855fb.png
    width: 682
    height: 314
  variants:
  - file: 2026-09-15-alternatives-to-minio-for-single-node-local-s3.image-123f5f9dadb9.webp
    width: 320
    height: 147
  - file: 2026-09-15-alternatives-to-minio-for-single-node-local-s3.image-f8b0acb10d37.webp
    width: 682
    height: 314
  color: '#fefefe'
- source: https://rmoff.net/images/2026/01/rustfs-gui.png
  original:
    file: 2026-09-15-alternatives-to-minio-for-single-node-local-s3.image-5c9be916063d.png
    width: 2804
    height: 1480
  color: '#fcfcfc'
- source: https://rmoff.net/images/2026/01/seaweedfs.excalidraw.png
  original:
    file: 2026-09-15-alternatives-to-minio-for-single-node-local-s3.image-a7fe16f967e8.png
    width: 709
    height: 296
  variants:
  - file: 2026-09-15-alternatives-to-minio-for-single-node-local-s3.image-cf2b451e3a0c.webp
    width: 320
    height: 134
  - file: 2026-09-15-alternatives-to-minio-for-single-node-local-s3.image-f8cd3a0767e4.webp
    width: 709
    height: 296
  color: '#fefefe'
- source: https://rmoff.net/images/2026/01/seaweedfs-ui.png
  original:
    file: 2026-09-15-alternatives-to-minio-for-single-node-local-s3.image-bf898943c973.png
    width: 2422
    height: 498
  color: '#fcfcfc'
- source: https://rmoff.net/images/2026/01/cloudserver.excalidraw.png
  original:
    file: 2026-09-15-alternatives-to-minio-for-single-node-local-s3.image-813d115c7066.png
    width: 709
    height: 296
  variants:
  - file: 2026-09-15-alternatives-to-minio-for-single-node-local-s3.image-0b447e4942a1.webp
    width: 320
    height: 134
  - file: 2026-09-15-alternatives-to-minio-for-single-node-local-s3.image-146e1676df03.webp
    width: 709
    height: 296
  color: '#fdfdfd'
- source: https://rmoff.net/images/2026/01/garage.excalidraw.png
  original:
    file: 2026-09-15-alternatives-to-minio-for-single-node-local-s3.image-c8ed15254e4f.png
    width: 842
    height: 367
  variants:
  - file: 2026-09-15-alternatives-to-minio-for-single-node-local-s3.image-82f703d8812f.webp
    width: 320
    height: 139
  - file: 2026-09-15-alternatives-to-minio-for-single-node-local-s3.image-d7138babe05c.webp
    width: 640
    height: 279
  - file: 2026-09-15-alternatives-to-minio-for-single-node-local-s3.image-a7701027f74b.webp
    width: 842
    height: 367
  color: '#fdfefd'
- source: https://rmoff.net/images/2026/01/apacheozone.excalidraw.png
  original:
    file: 2026-09-15-alternatives-to-minio-for-single-node-local-s3.image-2f364f4d2b90.png
    width: 805
    height: 296
  variants:
  - file: 2026-09-15-alternatives-to-minio-for-single-node-local-s3.image-f750bba15636.webp
    width: 320
    height: 118
  - file: 2026-09-15-alternatives-to-minio-for-single-node-local-s3.image-aeb466affc36.webp
    width: 805
    height: 296
  color: '#fdfdfd'
---

Let’s now explore the different alternatives to MinIO, and how easy they are to switch MinIO out for.

I’ve taken the above project and tried to implement it with as few changes to use the replacement for MinIO. I’ve left the MinIO S3 client, `mc` in place since that’s no big deal to replace if you want to rip out MinIO completely (s3cmd, `aws` CLI, etc etc).

### [S3Proxy](https://github.com/gaul/s3proxy) [ 🔗](https://rmoff.net/2026/01/14/alternatives-to-minio-for-single-node-local-s3/#_s3proxy)

Version tested: `3.0.0`

- ✅ [Docker image](https://hub.docker.com/r/andrewgaul/s3proxy/) (5M+ pulls)

- ✅ Licence: [Apache 2.0](https://github.com/gaul/s3proxy/blob/master/LICENSE)

- ✅ [S3 compatibility](https://github.com/gaul/s3proxy?tab=readme-ov-file#limitations)

Ease of config: 👍👍

Very easy to implement, and seems like a nice lightweight option.

One thing I did notice was that one of the projects that S3Proxy uses, [jclouds](https://jclouds.apache.org/), was moved to the Apache Attic (i.e. retired) in mid-2025—although probably nbd if you’re only using local storage anyway?

### [RustFS](https://github.com/rustfs/rustfs) [ 🔗](https://rmoff.net/2026/01/14/alternatives-to-minio-for-single-node-local-s3/#_rustfs)

Version tested: `1.0.0-alpha.79`

Ease of config: ✅✅

- ✅ [Docker image](https://hub.docker.com/r/rustfs/rustfs) (100k+ pulls)

- ✅ Licence: [Apache 2.0](https://docs.rustfs.com/developer/license.html)

- ✅ [S3 compatibility](https://docs.rustfs.com/features/s3-compatibility/)

Be aware that there was recently a [pretty bad security vuln](https://github.com/rustfs/rustfs/security/advisories/GHSA-h956-rh7x-ppgj) found in RustFS, which has put some people off from using it. The website looks pretty smart but several links resolve to the same page, giving it that "fresh paint" smell of a new project :) This might matter less for demos, if it’s easy to switch out. You’ll also note that the project is currently only 'alpha' release.

![rustfs.excalidraw](https://rmoff.net/images/2026/01/rustfs.excalidraw.png)

RustFS also includes a GUI:

![rustfs gui](https://rmoff.net/images/2026/01/rustfs-gui.png)

### [SeaweedFS](https://github.com/seaweedfs/seaweedfs) [ 🔗](https://rmoff.net/2026/01/14/alternatives-to-minio-for-single-node-local-s3/#_seaweedfs)

Version tested: `4.06`

- ✅ [Docker image](https://hub.docker.com/r/chrislusf/seaweedfs) (5M+ pulls)

- ✅ Licence: [Apache 2.0](https://github.com/seaweedfs/seaweedfs/blob/master/LICENSE)

- ✅ [S3 compatibility](https://github.com/seaweedfs/seaweedfs/wiki/Amazon-S3-API)

Ease of config: 👍

![seaweedfs.excalidraw](https://rmoff.net/images/2026/01/seaweedfs.excalidraw.png)

[This quickstart](https://github.com/seaweedfs/seaweedfs/wiki/Quick-Start-with-weed-mini) is useful for getting bare-minimum S3 functionality working. (That said, I still just got Claude to do the implementation…). Overall there’s not too much to change here; a fairly straightforward switchout of Docker images, but the auth does need its own config file (which as with Garage, I inlined in the Docker Compose).

*Edit: Straight after posting this blog, the project replied to say they’ll be removing this extra requirement, making it even easier to use! How cool is that :)*

SeaweedFS comes with its own basic UI which is handy:

![seaweedfs ui](https://rmoff.net/images/2026/01/seaweedfs-ui.png)

The [SeaweedFS website](https://seaweedfs.com/) is surprisingly sparse and at a glance you’d be forgiven for missing that it’s an OSS project, since there’s a "pricing" option and the title of the front page is "SeaweedFS Enterprise" (and no GitHub link that I could find!). But an OSS project it is, and a long-established one: SeaweedFS has been around with S3 support since its [0.91 release in 2018](https://github.com/seaweedfs/seaweedfs/releases/tag/0.91). You can also learn more about SeaweedFS from these [slides](https://docs.google.com/presentation/d/1tdkp45J01oRV68dIm4yoTXKJDof-EhainlA0LMXexQE/edit?slide=id.p#slide=id.p), including a [comparison chart with MinIO](https://docs.google.com/presentation/d/1tdkp45J01oRV68dIm4yoTXKJDof-EhainlA0LMXexQE/edit?slide=id.g3593a652b55_0_473#slide=id.g3593a652b55_0_473).

### [Zenko CloudServer](https://github.com/scality/cloudserver) [ 🔗](https://rmoff.net/2026/01/14/alternatives-to-minio-for-single-node-local-s3/#_zenko_cloudserver)

Version tested: `9.2.8`

- ✅ [Docker image](https://github.com/scality/cloudserver/pkgs/container/cloudserver/606409638?tag=9.2.8) (also outdated ones on [Docker Hub](https://hub.docker.com/r/zenko/cloudserver) with 5M+ pulls)

- ✅ Licence: [Apache 2.0](https://github.com/scality/cloudserver/blob/development/9.2/LICENSE)

- ✅ [S3 compatibility](https://github.com/scality/cloudserver?tab=readme-ov-file#overview)

Ease of config: 👍

![cloudserver.excalidraw](https://rmoff.net/images/2026/01/cloudserver.excalidraw.png)

Formerly known as S3 Server, CloudServer is part of a toolset called Zenko, published by Scality. It drops in to replace MinIO pretty easily, but I did find it slightly tricky at first to disentangle the set of names (cloudserver/zenko/scality) and what the actual software I needed to run was. There’s also a slightly odd feel that the docs link to an outdated Docker image.

### [Garage](https://git.deuxfleurs.fr/Deuxfleurs/garage) [ 🔗](https://rmoff.net/2026/01/14/alternatives-to-minio-for-single-node-local-s3/#_garage)

Ease of config: 😵

Version tested: `1.0.0`

- ✅ [Docker image](https://hub.docker.com/r/dxflrs/garage) (1M+ pulls)

- ✅ Licence: [AGPL](https://git.deuxfleurs.fr/Deuxfleurs/garage/src/branch/main-v2/LICENSE)

- ✅ [S3 compatibility](https://garagehq.deuxfleurs.fr/documentation/reference-manual/features/)

I had to get [a friend](https://code.claude.com/docs/en/overview) to help me with this one. As well as the `garage` container, I needed another to do the initial configuration, as well as a TOML config file which I’ve inlined in the Docker Compose to keep things concise.

![garage.excalidraw](https://rmoff.net/images/2026/01/garage.excalidraw.png)

Could I have sat down and RTFM’d to figure it out myself? Yes. Do I have better things to do with my time? Also, yes.

So, Garage *does* work, but gosh…it is *not* just a drop-in replacement in terms of code changes. It requires different plumbing for initialisation, and it’s not simple at that either. A simple example: `The specified key ID is not a valid Garage key ID (starts with GK, followed by 12 hex-encoded bytes)`. Excellent for production hygiene…overkill for local demos, and in fact somewhat of a hindrance TBH.

Is this an entirely fair assessment? If I were looking at it as a new piece of technology in its own right, completely not! Many pieces of excellent technology, particularly those that can support running distributed, will have a steep learning curve for configuration. However, my requirement here is a *simple* drop-in replacement for MinIO—which Garage is not.

### [Apache Ozone](https://github.com/apache/ozone) [ 🔗](https://rmoff.net/2026/01/14/alternatives-to-minio-for-single-node-local-s3/#_apache_ozone)

Version tested: `2.1.0`

- ✅ [Docker images](https://hub.docker.com/r/apache/ozone) (1M+ pulls)

- ✅ Licence: [Apache 2.0](https://github.com/apache/ozone/blob/master/LICENSE.txt)

- ✅ [S3 compatibility](https://ozone.apache.org/docs/2.1.0/interface/s3.html)

Ozone was spun out of Apache Hadoop (remember that?) [in 2020](https://apache.org/foundation/records/minutes/2020/board_minutes_2020_10_21.txt), having been initially created as part of the HDFS project back in 2015.

Ease of config: 😵

![apacheozone.excalidraw](https://rmoff.net/images/2026/01/apacheozone.excalidraw.png)

It does work as a replacement for MinIO, but it is not a lightweight alternative; neither I nor Claude could figure out how to deploy it with any fewer than **four** nodes. It gives heavy Hadoop vibes, and I wouldn’t be rushing to adopt it for my use case here.

### [Ceph Object Gateway](https://docs.ceph.com/en/reef/radosgw/#object-gateway) [ 🔗](https://rmoff.net/2026/01/14/alternatives-to-minio-for-single-node-local-s3/#_ceph_object_gateway)

Ozone (above) is heavyweight enough; I’m sure both are great at what they do, but they are not a lightweight container to slot into my Docker Compose stack for local demos.
