---
title: A Modern Web App in C
link: https://prophitt.me/writing/a-modern-web-app-in-c
source: hnrss-org
published: 2026-10-12T16:13:10Z
updated: 2026-10-12T16:13:10Z
first_seen: 2026-10-12T22:35:36.108955126Z
authors:
- prophittcorey
content: extracted
html: 2026-10-12-a-modern-web-app-in-c.html
extra:
  comments_url: https://news.ycombinator.com/item?id=50058528
  num_comments: 136
  points: 326
---

If you wanted a dynamic website in 1994, you wrote a C program, dropped it in a cgi-bin directory and let Apache fork it on every request. Thirty-two years later, I shipped a commercial web product using the same technology.

From the outside, [marketyour.games](https://marketyour.games) is modern. It's a progressively enhanced web product with user accounts, a search dashboard for content creators, folders for organizing them, payments using Stripe and transactional emails via Postmark.

However, on the inside it is anything but modern. Below are my thoughts on shipping a real product written entirely in C and served using CGI in 2026.

## The Foundation

CGI is an interface between a web server and your application. There's no framework or library to depend on. However, you do need a strong foundation to build on top of.

The foundation I like is called the [BCHS](https://learnbchs.org/) stack. That's OpenBSD, C, httpd and SQLite.

There's no framework, templating language, database server or containters. The entire workflow is straight forward:

- You write a C program that reads CGI environment variables.
- You write responses directly to stdout and you write log messages to stderr.
- If you need a database connection you open the database file directly using SQLite's C API.
- You build a statically linked binary and drop it in the cgi-bin directory.

Since every request spawns a new process, deploying a new version is as simple as replacing the old one. There's no server restart or downtime. New requests will simply pick up the new binary.

OpenBSD ships with sane defaults and gives you the tools to actually secure a C program like pledge and unveil. Httpd serves from a chroot by default, so your CGI binary runs sandboxed to its own corner of the filesystem.

I lean on three additional libraries:

- [libcurl](https://curl.se/libcurl/) for outbound web requests to Stripe and Postmark.
- [libsodium](https://libsodium.gitbook.io/doc) for password hashing, cookie encryption and HMAC verification.
- [libcjson](https://github.com/DaveGamble/cJSON) for parsing JSON responses from Stripe and Postmark.

Only libcurl and libsodium are actual build dependencies.

Both libcjson and SQLite are vendored and built as part of the project. That's possible because libcjson is a single C file and has no external dependencies. SQLite ships an official amalgamation (single source) file for this exact purpose.

To simplify deployment, all assets are baked into the binary at build time. The result is a build with almost no moving parts. A clean build takes a few seconds and rebuilds complete in less than half a second.

## The Good

### Fundamentals over frameworks.

CGI isn't another framework to learn. The 1.1 specification was published in 2004 and hasn't changed since. There's no API to internalize. The entire interface is a handful of fundamental HTTP concepts:

- Request headers.
- Request methods.
- Query strings.
- Body data.

If you understand HTTP, you already understand CGI. It's language and platform agnostic. You learn it once and the knowledge is good forever.

### Clear separation of concerns.

A modern web application typically puts a web server like nginx in front and reverse-proxies to an application server written in another language like Ruby, Python, Go or Rust. That always felt a bit strange to me; you essentially have two web servers and one of them is also responsible for your business logic.

CGI cuts the second server out of the picture. You have a web server like nginx (or httpd) that calls your business logic directly. It's clear who does what.

It makes debugging simpler, too. Your application is an ordinary program you can run from a shell: pipe it a request, watch stderr, step through it with gdb, etc.

CGI resembles serverless architecture: a supervisor receives a request, invokes your code, hands it a bag of environment variables and then it collects the result. It's like a modern serverless application, minus the vendor (and surprise bills).

### Minimal supply chain risk.

With the rise of autonomous agents and weekly attacks on package repositories, it feels reassuring to have very few dependencies.

Most modern tech stacks come with a package manager and an ecosystem to go along with it. It's convenient, but the dependency graph balloons. One or two direct dependencies can pull in hundreds of indirect ones, each maintained by somebody you've never heard of, with publish credentials protected by a password reused since 2015.

My supply chain is a C compiler and three libraries. Compromising it would require compromising libcurl... a project the entire internet watches, not a package with eleven weekly downloads and an owner who left in 2019.

### Simple tooling.

I'm a minimalist at heart. I use classic Vim with zero plugins. That means no LSP, no autocomplete, no code formatters and no AI.

Complex tech stacks demand complex tooling: they demand a language server to wade through the layers of abstraction. But this stack is simple enough a plain editor is all you need. The entire application is a directory of C files and a Makefile.

Open a file, read it, and you know what it does.

### Great tooling is within reach.

When you do want tooling, it's not far. Sanitizers, valgrind, gdb, ctags, cscope... these are mature (well, geriatric) tools that have been refined for decades and they're a package install away on any system that can compile C.

The maturity goes further than the tools themselves. Integration is built natively into many editors. For example, vim ships with built-in support for ctags, cscope and has a graphical gdb frontend (see :h Termdebug).

### Writing HTML in C isn't too bad.

I was hesitant to write the entire frontend in C, but it wasn't nearly as painful as I imagined.

All too many times have I:

- Fought vim to highlight Go's html/template syntax.
- Fought helix to detect Go templates without breaking html detection.
- Fought rustrover to recognize jinja templates for a project using askama.

Using C means there's no second language for your editor to half-detect, no mixed-syntax where highlighting falls apart, no plugin dance to get the two languages to cooperate. It isn't pretty, but it gets the job done.

### SQLite is a breath of fresh air.

I've always defaulted to Postgres for web applications. It's a great database, but it comes with some operational burden. There's a server process to babysit and frequent releases to upgrade to.

SQLite has none of that. There's no process, no configuration, no dependencies. Your application opens the database file directly. The operational burden is minimal.

## The Bad

Since CGI is only an interface, you get very little out of the box. As a result you need to write your own utilities to handle things like:

- Cookie parsing and creation.
- Query parameter parsing and validation.
- Form data parsing and validation.
- Escaping input for display.

I rolled my own in a single header file (cgi.h), but I'm not convinced that was the right solution. Libraries like [kcgi](https://kristaps.bsd.lv/kcgi/) exist, but adopting one means buying into its API and security model.

I'd like to see a set of shared utilities that developers can drop in (like the stb libraries).

## Final Thoughts

CGI is old, but it's not forgotten.

Most of the reasons we abandoned it in the early 2000s don't hold up today. Process forking isn't nearly as expensive as it was; modern kernels optimize it heavily.

And if the cost of forking ever became an issue, FastCGI is a simple enough upgrade.

Writing a modern web application in C is more than feasible— [marketyour.games](https://marketyour.games) is an example of such an application. The overall experience is quite enjoyable in a way modern tech stacks don't quite match.

I'll be reaching for this stack again.
