---
title: The Hardest Tarball?
link: https://stoppels.ch/2026/11/07/the-hardest-tarball.html
source: hnrss-org
published: 2026-11-10T15:00:39Z
updated: 2026-11-10T15:00:39Z
first_seen: 2026-11-10T19:38:28.320395710Z
authors:
- thatslast
content: extracted
html: 2026-11-10-the-hardest-tarball.html
remote_preview:
  url: https://stoppels.ch/2026/11/07/fig-hardest.svg
  alt: The structure of hardest.tar, with two basic interpretations based on whether a hard link can have a size or not.
---

*Nov 7th, 2026*

Here is a curiosity: an archive that can be constructed using a single GNU tar invocation, that is extracted in 8 distinct ways depending on what tool you use.

```
$ printf hello > file
$ touch empty
$ tar --format=pax --pax-option='delete=?time,size:=512' \
      --transform='s,^\./file$,café,' \
      -cf hardest.tar file ./file empty
```

It creates an archive `hardest.tar` from three files `file ./file empty` in sequence, where `file` is repeated so that GNU tar stores the second one as a hard link, and `empty` is empty. Exploiting the unique path of the second entry, the hard link is renamed to `café`. This triggers tar to create a pax header to store the file name as UTF-8. We add a `size=512` record to the pax header, which overrides the default `size=0` from the hard link. The value 512 is not a coincidence: it is exactly the size of a tar header.

Visually, this archive has the following structure, with two basic interpretations based on whether a hard link can have a size or not.

![The structure of hardest.tar, with two basic interpretations based on whether a hard link can have a size or not.](https://stoppels.ch/2026/11/07/fig-hardest.svg)

Now, POSIX says that data blocks for a hard link *“may be included”* in pax archives, but the implications of this are not entirely clear. So the 512 bytes after the hard link are its data, and not the header of `empty` as we archived it? How can a hard link have different contents from its previous counterpart `file`?

## Extracting it

This archive gives by far the richest and most surprising results I've encountered so far when extracting it with well-known tar utilities and libraries. The results below were produced using the latest releases of all packages at the time of writing.

### GNU tar, BusyBox, Python, Go and Perl

This group is reasonable in principle: they ignore the size on the hard link, and extract `file`, its hard link `café`, and `empty`.

### bsdtar

This is the system `tar` utility of macOS, which is closer to POSIX in spirit regarding the concept of size on hard links, and writes the 512 bytes into the hard link. The end result is that `file` now contains the tar header of `empty`.

### 7-Zip, node-tar, tar-fs, and Rust's tar & astral-tokio-tar

This group agrees with bsdtar that the 512 bytes are data of the hard link, but doesn't write them into the hard link. Instead, they skip forward over the data, and you are left with just `file` and `café`, without `empty`.

### Java's Commons Compress

This is where you begin to need more imagination. Because `café` contains data, a *separate* file is created that contains the header of `empty` as data, while `file` keeps its original contents.

### PHP's PharData

Makes `café` a separate, empty file, and also extracts `empty`. Why not?

### RubyGems

Does not extract `café` at all, but to make up for it, it creates an empty directory `PaxHeaders` next to `file` and `empty`.

### R

R has an entirely different approach: it extracts `empty` with 512 null bytes. It mistakenly applies the size to `empty` instead of the hard link, and those 512 bytes are from an end-of-archive marker.

### Bonus: Haskell

Haskell's tar would be exactly like GNU tar, if only it did not have string decoding issues, which turn the file `café` into `cafÃ©` on disk (in the year 2026, this is not a rendering quirk of your browser).

### .NET, Julia and uv's tar-codec

These implementations reject the tarball. I believe this is the best thing you can do, even if it's technically not POSIX-compliant.

## More of this

This tarball is one of 134 tricky tarballs that I'm collecting and categorizing in my test suite [haampie/tar-minefield](https://github.com/haampie/tar-minefield), developed since the last post. Its website [https://stoppels.ch/tar](https://stoppels.ch/tar/) features a big table that shows how implementations deal with all these cases, as well as detailed info per test case. A variant from the tarball of this post [is included as a test case](https://stoppels.ch/tar/F35.html).

If you'd like to discuss this, send me an email at *firstname\[at\]lastname.ch*.
