---
title: Your executable is a SQLite database
link: https://simonwillison.net/2026/Aug/24/your-executable-is-a-sqlite-database/
source: simon-willison
published: 2026-08-24T11:38:15Z
updated: 2026-08-24T11:38:15Z
first_seen: 2026-09-02T14:07:29.769486161Z
tags:
- c
- linux
- sqlite
summary: 'Your executable is a SQLite database Farid Zakaria describes a neat Linux pattern for creating a SQLite database file that can be directly used as an executable binary. The trick sets the SQLite file format''s 4-byte application ID (68 bytes into the file) to SELF, standing for Structured Executable & Linkable Format. The various components of the ELF executable format are then arranged into a number of different SQLite tables, using this schema. Their self-exec interpreter (C code here) can then extract and execute the necessary pieces. You can additionally use a Linux mechanism called binfmt_misc to teach the kernel to execute that any time it encounters an executable matching that binary pattern. Farid uses NixOS here, but without NixOS I think registration looks something like this: printf ''%s\n'' '':self:M:68:SELF::/usr/local/bin/self-exec:'' \ > /proc/sys/fs/binfmt_misc/register Via Hacker News Tags: c, linux, sqlite'
content: feed
html: 2026-08-24-your-executable-is-a-sqlite-database.html
---

**[Your executable is a SQLite database](https://fzakaria.com/2026/08/23/your-executable-is-a-sqlite-database)**

Farid Zakaria describes a neat Linux pattern for creating a SQLite database file that can be directly used as an executable binary.

The trick sets the SQLite file format's 4-byte application ID (68 bytes into the file) to SELF, standing for Structured Executable & Linkable Format. The various components of the ELF executable format are then arranged into a number of different SQLite tables, using [this schema](https://github.com/fzakaria/selfdb/blob/main/schema/self.sql).

Their `self-exec` interpreter ([C code here](https://github.com/fzakaria/selfdb/blob/main/loader/self-exec.c)) can then extract and execute the necessary pieces.

You can additionally use a Linux mechanism called [binfmt\_misc](https://docs.kernel.org/admin-guide/binfmt-misc.html) to teach the kernel to execute that any time it encounters an executable matching that binary pattern. Farid uses NixOS here, but without NixOS I think registration looks something like this:

```
printf '%s\n' ':self:M:68:SELF::/usr/local/bin/self-exec:' \
  > /proc/sys/fs/binfmt_misc/register
```

Via [Hacker News](https://news.ycombinator.com/item?id=49415271)

Tags: [c](https://simonwillison.net/tags/c), [linux](https://simonwillison.net/tags/linux), [sqlite](https://simonwillison.net/tags/sqlite)
