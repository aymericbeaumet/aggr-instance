---
title: Stop making swap partitions—use swap files instead
link: https://gist.github.com/joshenders/c4960cec9c63a7b7d68ffa9543356c43
source: hnrss-org-frontpage
published: 2026-09-08T22:33:21Z
updated: 2026-09-08T22:33:21Z
first_seen: 2026-09-11T23:11:51.018809841Z
authors:
- jenders
summary: 'Article URL: https://gist.github.com/joshenders/c4960cec9c63a7b7d68ffa9543356c43 Comments URL: https://news.ycombinator.com/item?id=49618087 Points: 107 # Comments: 173'
content: extracted
html: 2026-09-08-stop-making-swap-partitions-use-swap-files-instead.html
preview:
  file: 2026-09-08-stop-making-swap-partitions-use-swap-files-instead.preview-385d83fea357.webp
  width: 256
  height: 128
  alt: 'Stop making swap partitions. GitHub Gist: instantly share code, notes, and snippets.'
  color: '#25282c'
images:
- source: https://github.githubassets.com/assets/gist-og-image-54fd7dc0713e.png
  original:
    file: 2026-09-08-stop-making-swap-partitions-use-swap-files-instead.image-0c7d1f3af096.png
    width: 1280
    height: 640
  variants:
  - file: 2026-09-08-stop-making-swap-partitions-use-swap-files-instead.image-5582ab775c68.webp
    width: 48
    height: 24
  color: '#1c1f23'
---

## Stop making swap partitions—use swap files instead!

[](https://gist.github.com/joshenders/c4960cec9c63a7b7d68ffa9543356c43#stop-making-swap-partitionsuse-swap-files-instead)

Swap files have had the same performance characteristics as swap partitions for more than [20 years](https://lkml.org/lkml/2005/7/7/326) and yet, linux distributions [continue](https://imgur.com/a/SgDoJqd) to encourage the use of swap partitions during install.

Swap files are easier to use/add/remove/modify/extend after installation. They're better in every way––use swap files!

1. Create a swap file.

> 💡 You can use `dd` for this too but if your filesystem supports it, `fallocate` is faster.

```
fallocate -l 4G /swapfile
```

2. Only root should be able to write to the swap file.

```
chmod 0600 /swapfile
```

3. Format the swap file.

```
mkswap /swapfile
```

4. Enable it.

```
swapon /swapfile
```

5. Make it persist across boots.

```
echo "/swapfile none swap defaults 0 0" >> /etc/fstab
```
