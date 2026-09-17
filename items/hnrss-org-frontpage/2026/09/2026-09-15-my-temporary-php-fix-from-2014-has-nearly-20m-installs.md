---
title: My temporary PHP fix from 2014 has nearly 20M installs. Today I'm deprecating it
link: https://jakeasmith.com/blog/http-build-url/
source: hnrss-org-frontpage
published: 2026-09-15T20:53:36Z
updated: 2026-09-15T20:53:36Z
first_seen: 2026-09-17T12:46:46.439381799Z
authors:
- jakeasmith
summary: 'Article URL: https://jakeasmith.com/blog/http-build-url/ Comments URL: https://news.ycombinator.com/item?id=49718773 Points: 170 # Comments: 42'
content: extracted
html: 2026-09-15-my-temporary-php-fix-from-2014-has-nearly-20m-installs.html
preview:
  file: 2026-09-15-my-temporary-php-fix-from-2014-has-nearly-20m-installs.preview-9abfd89eb259.webp
  width: 256
  height: 134
  alt: 'Packagist install statistics for jakeasmith/http_build_url: 19,864,271 total installs, 401,308 in the last 30 days, and daily installs climbing from 2014 to 2026.'
  color: '#f7f7f9'
images:
- source: https://jakeasmith.com/blog/http-build-url/og.png
  original:
    file: 2026-09-15-my-temporary-php-fix-from-2014-has-nearly-20m-installs.image-c8e43a042f41.png
    width: 1200
    height: 630
  variants:
  - file: 2026-09-15-my-temporary-php-fix-from-2014-has-nearly-20m-installs.image-06eb754fc9d8.webp
    width: 320
    height: 168
  - file: 2026-09-15-my-temporary-php-fix-from-2014-has-nearly-20m-installs.image-205bb87b0406.webp
    width: 640
    height: 336
  - file: 2026-09-15-my-temporary-php-fix-from-2014-has-nearly-20m-installs.image-4742b847478d.webp
    width: 960
    height: 504
  - file: 2026-09-15-my-temporary-php-fix-from-2014-has-nearly-20m-installs.image-480ce89497e6.webp
    width: 1200
    height: 630
  color: '#fdfdfe'
- source: https://jakeasmith.com/blog/http-build-url/1.png
  original:
    file: 2026-09-15-my-temporary-php-fix-from-2014-has-nearly-20m-installs.image-281fd9df0ae8.png
    width: 1440
    height: 900
  variants:
  - file: 2026-09-15-my-temporary-php-fix-from-2014-has-nearly-20m-installs.image-979aa5aeda93.webp
    width: 320
    height: 200
  - file: 2026-09-15-my-temporary-php-fix-from-2014-has-nearly-20m-installs.image-dcbd3e33a47e.webp
    width: 640
    height: 400
  - file: 2026-09-15-my-temporary-php-fix-from-2014-has-nearly-20m-installs.image-829a737bb66a.webp
    width: 960
    height: 600
  - file: 2026-09-15-my-temporary-php-fix-from-2014-has-nearly-20m-installs.image-b801986c95df.webp
    width: 1280
    height: 800
  - file: 2026-09-15-my-temporary-php-fix-from-2014-has-nearly-20m-installs.image-4ae1bd7d30ae.webp
    width: 1440
    height: 900
  color: '#fdfdfd'
---

Twelve years ago, I wrote [174 lines of PHP](https://github.com/jakeasmith/http_build_url) as a stopgap for AOL’s content management system. I put it on Packagist in case anyone else needed the same patch, and somehow it’s been installed nearly 20 million times since. Today I marked it deprecated.

## A temporary shim

In 2014, we were in the middle of upgrading AOL’s CMS from PHP 5.2 to 5.3. Part of that upgrade was dropping version 1 of the `pecl_http` extension, which gave us a function called `http_build_url()`. A CMS deals with a lot of URLs, and ours called that function in dozens of places. I wasn’t touching those. The function seemed straightforward enough to reproduce, so I wrote my own `http_build_url()`, defined only if the real one didn’t already exist. The old code never knew anything had changed.

Composer was just taking off at the time, which made sharing it easy. I figured it would earn its keep for a year or two, until the PHP community moved on to something better.

## That’s a lot of installs

Well, it wasn’t temporary. It’s been installed from Packagist nearly 20 million times, and it still picks up over 400,000 installs a month.

![Packagist install statistics for jakeasmith/http_build_url as of September 15, 2026: 19,864,271 total installs and 401,308 in the last 30 days. Daily installs climb steadily from near zero in 2014 to about 13,000 a day in 2026.](https://jakeasmith.com/blog/http-build-url/1.png)

And it turns out Composer is only part of the picture. [WPML](https://wpml.org/home/about-us/), the market-leading multilingual plugin for WordPress, bundles the polyfill directly in its codebase, and WPML says it’s installed on over 1.5 million sites. The domain-name library [idna-convert](https://github.com/algo26-matthias/idna-convert) depends on it too, which is how it ships inside [the source of SPIP](https://sources.debian.org/src/spip/unstable/vendor/jakeasmith/http_build_url/), a French content management system, and how it ended up [packaged in Debian](https://packages.debian.org/sid/php-jakeasmith-http-build-url) and Ubuntu. Between all of them, there’s a pretty good chance you’ve visited a website that is still running my code.

I never imagined it would go this far.

## Coming back to it

I didn’t grasp how far it had spread until a few months ago, when I looked at the package for the first time in years. I knew it had users. By 2021 I’d been out of PHP for a while, and the downloads were surprising enough that I [asked for a new maintainer](https://github.com/jakeasmith/http_build_url/issues/27). Three people offered. Shortly after I asked, we lost a family member unexpectedly, and it turned our world upside down for a while. I never followed up, and that’s on me. By the time things settled, other goals had taken over, and I forgot about the package for years.

Along with the numbers, there were a handful of GitHub issues, including one where joining a path onto a URL with a trailing slash strips every letter “a” out of the path. So much for straightforward. Under a comment that reads `// Workaround for trailing slashes`, my code tacks an “a” onto the path so there’s always a last segment to cut off, then cuts it off with a find-and-replace. When the path ends in a slash, that last segment is just the “a”, and the find-and-replace takes every other “a” in the path with it. I can’t believe the bug went unnoticed for as long as it did.

So I had a decision to make. I could dive back into PHP after almost a decade away, hand the package to one of the people who’d offered, or let it keep sitting there.

## None of the above

It was always meant to be temporary, so I’m retiring it. [The PHP League’s URI library](https://uri.thephpleague.com/) has been the community’s answer for years, and [PHP 8.5](https://www.php.net/releases/8.5/en.php) now ships [a standards-compliant URI API in the language itself](https://sensiolabs.com/blog/2025/php-85-new-uri-extension) (thanks to [jawira](https://github.com/jawira) for pointing me at it). Both are better than a 174-line shim from 2014. Maintaining the package would only delay the move everyone should be making, and handing it over would add a risk on top of that. I don’t doubt anyone who offered, and [ozh](https://github.com/ozh) has kept [a fork](https://github.com/ozh/http_build_url) going for YOURLS. But a widely installed package with a new maintainer nobody downstream has vetted is exactly what attackers look for. [Veritasium’s video on the xz Utils backdoor](https://www.youtube.com/watch?v=aoag03mSuXQ) is the best telling I’ve seen of how that plays out.

The package will keep installing, but it won’t get new fixes, including for the missing-”a” bug. After this long without a change, even a one-line fix could have unintended consequences for someone, with no one around to support it. [The README](https://github.com/jakeasmith/http_build_url) shows how to switch.

I wrote this code to ease a painful migration, for myself and anyone else going through the same one. Thank you to everyone who sent a pull request or offered to take it over, and to the people who kept filing issues long after I’d stopped reading them. It was a good run for a temporary fix.

*P.S. We never migrated AOL’s CMS off the “temporary” polyfill. It ran there until the whole platform was shut down around 2020.*
