---
title: FFmpeg 9.1 'Gauss'
link: https://ffmpeg.org/download.html#release_9.1
source: hnrss-org
published: 2026-11-20T04:02:39Z
updated: 2026-11-20T04:02:39Z
first_seen: 2026-11-20T16:02:15.998933044Z
authors:
- gyan
content: extracted
html: 2026-11-20-ffmpeg-9-1-gauss.html
remote_preview:
  url: https://ffmpeg.org/img/ffmpeg3d_white_20.png
  alt: FFmpeg
---

#### If you find FFmpeg useful, you are welcome to contribute by [donating](https://ffmpeg.org/donations.html). More downloading options

### Get packages & executable files

FFmpeg only provides source code. Below are some links that provide it already compiled and ready to go.

### Get the Sources

You can retrieve the source code through [Git](https://git-scm.com/) by using the command:

```
git clone https://git.ffmpeg.org/ffmpeg.git ffmpeg
```

 Cannot access Git or wish to speed up the cloning and reduce the bandwidth usage?

FFmpeg has always been a very experimental and developer-driven project. It is a key component in many multimedia projects and has new features added constantly. Development branch snapshots work really well 99% of the time so people are not afraid to use them.

#### Git Repositories

Since FFmpeg is developed with [Git](https://git-scm.com/), multiple repositories from developers and groups of developers are available.

| Clone URL                                                                                                                                                                                       | Description                                                           |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------- |
| [https://git.ffmpeg.org/ffmpeg.git](https://git.ffmpeg.org/ffmpeg.git)  [Browse](https://git.ffmpeg.org/gitweb/ffmpeg.git)                                                                    | Main FFmpeg Git repository                                            |
| [https://git.ffmpeg.org/ffmpeg-web](https://git.ffmpeg.org/ffmpeg-web)                                                                                                                         | Main ffmpeg.org website repository                                    |
| [https://git.ffmpeg.org/fateserver](https://git.ffmpeg.org/fateserver)                                                                                                                         | [fate.ffmpeg.org](http://fate.ffmpeg.org) server software repository |
| [https://github.com/FFmpeg/FFmpeg](https://github.com/FFmpeg/FFmpeg)  [Browse](https://github.com/FFmpeg/FFmpeg) [Snapshot](https://github.com/FFmpeg/FFmpeg/tarball/master)                 | Mirror of the main repository                                         |
| [https://github.com/FFmpeg/web](https://github.com/FFmpeg/web)  [Browse](https://github.com/FFmpeg/web) [Snapshot](https://github.com/FFmpeg/web/tarball/master)                             | Mirror of the website repository                                      |
| [https://github.com/FFmpeg/fateserver](https://github.com/FFmpeg/fateserver)  [Browse](https://github.com/FFmpeg/fateserver) [Snapshot](https://github.com/FFmpeg/fateserver/tarball/master) | Mirror of the FATE server repository                                  |

#### Release Verification

All FFmpeg releases are cryptographically signed with [our public PGP key](https://ffmpeg.org/ffmpeg-devel.asc) and should be verified for authenticity.

```
pub   rsa2048 2011-04-26 [SC]
    FCF986EA15E6E293A5644F10B4322F04D67658D8
uid           [  full  ] FFmpeg release signing key <ffmpeg-devel@ffmpeg.org>
sub   rsa2048 2011-04-26 [E]
```

To verify a release:

1. Import our public key into your local keyring:

   ```
   $ curl https://ffmpeg.org/ffmpeg-devel.asc | gpg --import
   ```

2. Download a release tarball and its corresponding signature.
3. Verify the signature:

   ```
   $ gpg --verify ffmpeg-4.3.2.tar.xz.asc ffmpeg-4.3.2.tar.xz
   gpg: Signature made Sun 21 Feb 2021 06:35:15 AEST
   gpg:                using RSA key FCF986EA15E6E293A5644F10B4322F04D67658D8
   gpg:                issuer "ffmpeg-devel@ffmpeg.org"
   gpg: Good signature from "FFmpeg release signing key <ffmpeg-devel@ffmpeg.org>" [full]
   ```

Optionally, you can verify that git and tarball match, the only differences should be the absence of .git\* files in the tarball and a VERSION file in the tarball containing the version. The git tags should be signed with [EDDSA key DD1EC9E8DE085C629B3E1846B18E8928B3948D64](https://ffmpeg.org/git-tag-key.asc)

A release that also has a `.pq.asc` file carries post-quantum signatures ([RFC 9980](https://www.rfc-editor.org/rfc/rfc9980)) made with both of these keys:

```
ML-DSA-87+Ed448  99669C49693683D51F174FE0D9E84915F5C2A536F5C0DA70FB8DCBD8AA8C4B3D
SLH-DSA-256s     547721479F80449E77748C0235116A1F8A73D9ECA98673B90E4E39347D229989
```

GnuPG cannot verify these yet; use [sq](https://sequoia-pgp.org/) 1.4 or later built with a [cryptographic backend](https://gitlab.com/sequoia-pgp/sequoia/-/blob/main/openpgp/README.md#crypto-backends) that supports these algorithms (OpenSSL 3.5 or RustCrypto), both of these must succeed:

```
$ sq verify --signature-file ffmpeg-9.0.2.tar.xz.pq.asc --signer-file ffmpeg-mldsa87-ed448.cert.pgp ffmpeg-9.0.2.tar.xz
$ sq verify --signature-file ffmpeg-9.0.2.tar.xz.pq.asc --signer-file ffmpeg-slhdsa256s.cert.pgp ffmpeg-9.0.2.tar.xz
```

#### Releases

Approximately every 6 months the FFmpeg project makes a new major release. Between major releases point releases will appear that add important bug fixes but no new features. Note that these releases are intended for distributors and system integrators. Users that wish to compile from source themselves are strongly encouraged to consider using the development branch (see above), this is the only version on which FFmpeg developers actively work. The release branches only cherry pick selected changes from the development branch, which therefore receives much more and much faster bug fixes such as additional features and security patches.

### FFmpeg 9.1 "Gauss"

9.1 was released on 2026-11-20. It is the latest stable FFmpeg release from the 9.1 release branch, which was cut from master on 2026-10-24.

It includes the following library versions:

```
libavutil      61. 19.100
libavcodec     63. 24.100
libavformat    63. 16.100
libavdevice    63.  5.100
libavfilter    12.  8.100
libswscale     10.  5.100
libswresample   7.  6.100
```

### FFmpeg 9.0.2 "Lei"

9.0.2 was released on 2026-09-18. It is the latest stable FFmpeg release from the 9.0 release branch, which was cut from master on 2026-06-26.

It includes the following library versions:

```
libavutil      61.  1.102
libavcodec     63.  1.102
libavformat    63.  1.102
libavdevice    63.  1.102
libavfilter    12.  1.102
libswscale     10.  1.102
libswresample   7.  1.102
```

### FFmpeg 8.1.3 "Hoare"

8.1.3 was released on 2026-09-21. It is the latest stable FFmpeg release from the 8.1 release branch, which was cut from master on 2026-03-08.

It includes the following library versions:

```
libavutil      60. 26.103
libavcodec     62. 28.103
libavformat    62. 12.103
libavdevice    62.  3.103
libavfilter    11. 14.103
libswscale      9.  5.103
libswresample   6.  3.103
```

### FFmpeg 8.0.3 "Huffman"

8.0.3 was released on 2026-06-18. It is the latest stable FFmpeg release from the 8.0 release branch, which was cut from master on 2025-08-09.

It includes the following library versions:

```
libavutil      60.  8.103
libavcodec     62. 11.103
libavformat    62.  3.103
libavdevice    62.  1.103
libavfilter    11.  4.103
libswscale      9.  1.103
libswresample   6.  1.103
```

### FFmpeg 7.1.5 "Péter"

7.1.5 was released on 2026-06-20. It is the latest stable FFmpeg release from the 7.1 release branch, which was cut from master on 2024-09-24.

It includes the following library versions:

```
libavutil      59. 39.100
libavcodec     61. 19.100
libavformat    61.  7.100
libavdevice    61.  3.100
libavfilter    10.  4.100
libswscale      8.  3.100
libswresample   5.  3.100
libpostproc    58.  3.100
```

### FFmpeg 6.1.6 "Heaviside"

6.1.6 was released on 2026-06-20. It is the latest stable FFmpeg release from the 6.1 release branch, which was cut from master on 2023-10-29.

It includes the following library versions:

```
libavutil      58. 29.100
libavcodec     60. 31.102
libavformat    60. 16.100
libavdevice    60.  3.100
libavfilter     9. 12.100
libswscale      7.  5.100
libswresample   4. 12.100
libpostproc    57.  3.100
```

### FFmpeg 5.1.10 "Riemann"

5.1.10 was released on 2026-06-21. It is the latest stable FFmpeg release from the 5.1 release branch, which was cut from master on 2022-07-13.

It includes the following library versions:

```
libavutil      57. 28.100
libavcodec     59. 37.100
libavformat    59. 27.100
libavdevice    59.  7.100
libavfilter     8. 44.100
libswscale      6.  7.100
libswresample   4.  7.100
libpostproc    56.  6.100
```

### FFmpeg 4.4.8 "Rao"

4.4.8 was released on 2026-06-21. It is the latest stable FFmpeg release from the 4.4 release branch, which was cut from master on 2021-04-08.

It includes the following library versions:

```
libavutil      56. 70.100
libavcodec     58.134.100
libavformat    58. 76.100
libavdevice    58. 13.100
libavfilter     7.110.100
libswscale      5.  9.100
libswresample   3.  9.100
libpostproc    55.  9.100
```

### Old Releases

Older versions are available at the [Old Releases](https://ffmpeg.org/olddownload.html) page. Releases are usually moved there after the last [downstream](https://trac.ffmpeg.org/wiki/Downstreams) drops support.
