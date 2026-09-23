---
title: 'WordPress: Unauthenticated path traversal leading to conditional RCE'
link: https://github.com/WordPress/wordpress-develop/security/advisories/GHSA-7hp8-65ch-5whp
source: hnrss-org-frontpage
published: 2026-09-22T16:33:45Z
updated: 2026-09-22T16:33:45Z
first_seen: 2026-09-23T00:20:14.260676283Z
authors:
- vntok
summary: 'Article URL: https://github.com/WordPress/wordpress-develop/security/advisories/GHSA-7hp8-65ch-5whp Comments URL: https://news.ycombinator.com/item?id=49803959 Points: 147 # Comments: 77'
content: extracted
html: 2026-09-22-wordpress-unauthenticated-path-traversal-leading-to.html
preview:
  file: 2026-09-22-wordpress-unauthenticated-path-traversal-leading-to.preview-3e3c8b24d9de.webp
  width: 256
  height: 128
  alt: An unauthenticated attacker can make `get_page_template()` page-template resolution include a chosen readable local `.php` file outside the active theme directories. If relevant pre-conditions for ...
  color: '#eaebeb'
images:
- source: https://opengraph.githubassets.com/0a709104f82717281f48e543b5542acf296bafdb3d15db8cefd2148be6d4d32b/WordPress/wordpress-develop/security/advisories/GHSA-7hp8-65ch-5whp
  original:
    file: 2026-09-22-wordpress-unauthenticated-path-traversal-leading-to.image-ee0977fa56cd.png
    width: 1200
    height: 600
  variants:
  - file: 2026-09-22-wordpress-unauthenticated-path-traversal-leading-to.image-179821543dfa.webp
    width: 320
    height: 160
  - file: 2026-09-22-wordpress-unauthenticated-path-traversal-leading-to.image-c82e42c8668a.webp
    width: 640
    height: 320
  - file: 2026-09-22-wordpress-unauthenticated-path-traversal-leading-to.image-c4fea8419efd.webp
    width: 1200
    height: 600
  color: '#fefefe'
---

An unauthenticated attacker can make `get_page_template()` page-template resolution include a chosen readable local `.php` file outside the active theme directories. If relevant pre-conditions for both the server environment and the active theme are met, this can lead to RCE.

The pre-conditions are:

- The active child or parent theme contains a top-level directory whose name starts with `page-` (e.g. `page-templates`). This affects the legacy Twenty Twelve and Twenty Fourteen themes, as well as some popular third party themes such as Neve, Hestia, and Sydney.
- A chosen local `.php` target file exists on the server and is readable by the web server account. The well known `pearcmd.php` PEAR→RCE transition can be used for this when `register_argc_argv` is set to `On`. The official `php` image for Docker is affected, and the default cPanel configuration is affected when PHP prior to 8.5 is in use.

WordPress 7.1.2 has been released containing a fix for the vulnerability, and as a courtesy to users on older branches the fix has been backported to all branches back to 4.7.

Discovered and responsibly disclosed by [Robert Ressl](https://ressl.ch/).
