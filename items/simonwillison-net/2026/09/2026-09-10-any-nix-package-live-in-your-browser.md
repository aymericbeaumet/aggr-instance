---
title: Any Nix package, live in your browser
link: https://simonwillison.net/2026/Sep/10/trynix/
source: simonwillison-net
published: 2026-09-10T23:44:15Z
updated: 2026-09-10T23:44:15Z
first_seen: 2026-09-11T00:29:35.512207397Z
labels:
- code-review
- github-actions
- linux
- webassembly
summary: 'Any Nix package, live in your browser Farid Zakaria calls this his "magnum opus of Nix work", and I can see why. trynix.dev provides a qemu-wasm powered x86_64 Linux virtual machine running entirely in your browser through WebAssembly. That VM can then be booted with any Nix package from the past 13 years. They are URL addressable, so you can navigate to this page: https://trynix.dev/?pkg=python3%403.6.2 Then click "Load" and get an interactive shell against a virtual machine running Python 3.6.2 from 2017. Farid is building all sorts of neat things on top of this. One recent example: Review a pull request by booting it introduces trynix-preview, described like this: GitHub action that comments a link on a pull request which lets you boot the PR’s build in the browser using https://trynix.dev. No servers, just browsers. Via Lobste.rs Tags: code-review, linux, webassembly, github-actions'
content: extracted
html: 2026-09-10-any-nix-package-live-in-your-browser.html
---

**[Any Nix package, live in your browser](https://fzakaria.com/2026/09/04/any-nix-package-live-in-your-browser)** ([via](https://lobste.rs/s/7lii0g/review_pull_request_by_booting_it "Lobste.rs")) Farid Zakaria calls this his "*magnum opus* of Nix work", and I can see why.

[trynix.dev](https://trynix.dev) provides a [qemu-wasm](https://github.com/ktock/qemu-wasm) powered x86\_64 Linux virtual machine running entirely in your browser through WebAssembly. That VM can then be booted with *any Nix package* from the past 13 years. They are URL addressable, so you can navigate to this page:

[https://trynix.dev/?pkg=python3%403.6.2](https://trynix.dev/?pkg=python3%403.6.2)

Then click "Load" and get an interactive shell against a virtual machine running Python 3.6.2 from 2017.

Farid is building all sorts of neat things on top of this. One recent example: [Review a pull request by booting it](https://fzakaria.com/2026/09/09/review-a-pull-request-by-booting-it) introduces [trynix-preview](https://github.com/marketplace/actions/trynix-preview), described like this:

> GitHub action that comments a link on a pull request which lets you boot the PR’s build in the browser using [https://trynix.dev](https://trynix.dev/). No servers, just browsers.
