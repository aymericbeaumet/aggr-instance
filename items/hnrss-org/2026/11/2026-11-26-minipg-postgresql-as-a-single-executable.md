---
title: 'Minipg: PostgreSQL as a Single Executable'
link: https://progrium.github.io/minipg/
source: hnrss-org
published: 2026-11-26T03:00:48Z
updated: 2026-11-26T03:00:48Z
first_seen: 2026-11-26T12:13:26.788658858Z
authors:
- rguiscard
content: extracted
html: 2026-11-26-minipg-postgresql-as-a-single-executable.html
---

## About

minipg is PostgreSQL as one executable, ready to use, with no server to configure. A `.pgdb` directory is the database: its data, config, credentials, and Postgres version all live inside it, and `minipg app.pgdb` opens a psql shell on it. A server starts when a command needs one and stops on its own when it's idle.

minipg embeds Docker Hub's `postgres:18-alpine` image and runs it with [h9](https://progrium.github.io/hyper9/), used as a Zig library. h9's Linux personality runs Alpine's own Postgres binaries directly on the host's hypervisor, with h9's Plan 9 kernel answering their system calls. There's no Linux kernel to boot, no VM image, and no Docker.

## Demo

The demo downloads about 35 MB. It needs JavaScript Promise Integration, which Chrome and Edge have from version 137; other browsers are getting it. It can also be [opened on its own page](https://progrium.github.io/minipg/demo/).

The demo runs minipg's Linux build, which runs inside the Postgres image itself. It starts Postgres as ordinary processes and serves on a unix socket, since a page has no TCP. New databases come from a template cluster made at build time, because initdb takes half a minute on v86.

## Using It

```
minipg app.pgdb                     # psql shell (creates app.pgdb if missing)
minipg app.pgdb "select 1"          # one statement
minipg url app.pgdb                 # postgres://postgres:...@127.0.0.1:54321/app
minipg serve app.pgdb               # an explicit foreground server
minipg run app.pgdb -- npm start    # your app, with DATABASE_URL set
minipg dump app.pgdb > dump.sql     # and minipg restore app.pgdb dump.sql
```

A `.pgdb` holds `minipg.toml` (settings and the generated superuser password) and `pgdata/`, Postgres's data directory as plain files. A stopped one can be copied like any directory. New clusters use Postgres's builtin `C.UTF-8` locale, which sorts the same on every platform.

In CI, the Linux binary is static and needs no privileges or devices: without `/dev/kvm`, h9 runs Postgres on its native backend. A test job can be as small as:

```
minipg init test.pgdb
minipg run test.pgdb -- go test ./...
```

The [README](https://github.com/progrium/minipg/blob/main/README.md) lists every command, and the [manual](https://github.com/progrium/minipg/blob/main/MANUAL.md) is the full guide.

## How It Works

Exactly one process runs Postgres for a `.pgdb`, enforced by a lock. It holds the server's h9 machine, whose Linux root is the unpacked image and whose writable working directory is the `.pgdb`. The image's `docker-entrypoint.sh` initializes the cluster on first start. Commands never host Postgres themselves. If no server is running, they start one in the background and use it. psql, pg\_dump, and the other tools each run in their own short-lived h9 machine (about 90 ms to start) over the same root.

|                       |                                                              |
| --------------------- | ------------------------------------------------------------ |
| Linux x86\_64         | KVM, or h9's native backend without it (Linux 5.11 or later) |
| Linux arm64           | KVM                                                          |
| macOS (Apple Silicon) | Hypervisor.framework                                         |
| The browser           | h9's web host, v86, and minipg's Linux build                 |

## Source Code

The source is on [GitHub](https://github.com/progrium/minipg), under the MIT license. Building needs Zig 0.16, plus `curl`, `jq`, `tar`, and `xz` for the root:

```
git clone --recursive https://github.com/progrium/minipg
cd minipg
make build         # pulls the root for this host's arch, builds ./minipg
make root ARCH=386
make web           # this page's demo, in .zig-out/web
```

## Warning!

minipg is early, and h9's Linux personality is young. On h9's native backend, Postgres is about 2x slower than native for one client, and slower under load. Don't trust it with data you can't recreate yet.

Please report bugs and send questions through [GitHub issues](https://github.com/progrium/minipg/issues).

## Related Links

- [h9](https://progrium.github.io/hyper9/), which runs native 9front executables, and Linux ones, on host hypervisors
- [PostgreSQL](https://www.postgresql.org/) and Docker Hub's [postgres](https://hub.docker.com/_/postgres) image
- [v86](https://github.com/copy/v86), the x86 emulator the demo runs on
- [xterm.js](https://xtermjs.org/), the demo's terminal
