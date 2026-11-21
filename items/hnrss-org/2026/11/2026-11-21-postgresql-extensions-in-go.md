---
title: PostgreSQL Extensions in Go
link: https://jespino.github.io/gopgext/
source: hnrss-org
published: 2026-11-21T17:08:04Z
updated: 2026-11-21T17:08:04Z
first_seen: 2026-11-21T23:45:44.123432357Z
authors:
- valyala
content: extracted
html: 2026-11-21-postgresql-extensions-in-go.html
remote_preview:
  url: https://jespino.github.io/gopgext/img/social.png
  alt: 'gopgext: PostgreSQL extensions in Go'
---

gopgext builds PostgreSQL extensions from plain Go code, using TinyGo. Mark a function with `//gopgext:function` and it becomes a SQL function: gopgext writes the C glue, the install script and the control file.

![The Go gopher in a PostgreSQL elephant costume](https://jespino.github.io/gopgext/img/gopher-elephant.png)

Experimentalgopgext is experimental. Don’t use it in production: a bug in the generated code or the Go runtime runs inside the server, where it can crash it or corrupt data, and the API and generated SQL may change without notice.

### Just Go

A project is plain Go files. The standard library works: `fmt`, `regexp`, `encoding/json`, `crypto/*`, maps, generics, iterators, `defer`/`recover`.

### Types map to SQL

Strings, numbers, arrays, `NULL`s, sets, rows, timestamps, UUIDs and JSON map to SQL types. Define your own data types and composite types in Go.

### Queries and errors

Run SQL through SPI, return errors with SQLSTATE codes, log, read settings and stay responsive to query cancellation.

### Beyond functions

Write triggers, aggregates, GiST, GIN, SP-GiST and BRIN operator classes, index access methods, foreign data wrappers and query hooks.

### Proven on contrib

Ports of PostgreSQL’s contrib modules (`pgcrypto`, `hstore`, `pg_stat_statements`, `cube`, …) pass upstream’s regression tests.

### Safe coexistence

Several Go extensions load in one backend. PostgreSQL errors never unwind through Go frames, and results are copied into PostgreSQL memory.

## Hello, world

```go
package main

import (
	"strings"

	"gopgext/pg"
)

// Hello returns a greeting for name.
//
//gopgext:function immutable parallel=safe
func Hello(name string) string {
	return "Hello, " + name + "!"
}

// Shout upper-cases s. An empty string raises an error.
//
//gopgext:function immutable parallel=safe
func Shout(s string) (string, error) {
	if s == "" {
		return "", pg.Errorf(pg.CodeInvalidParameterValue, "nothing to shout")
	}
	return strings.ToUpper(s) + "!", nil
}

func main() {} // runs once per backend, when PostgreSQL loads the library
```

```sh
gopgext init hello       # creates hello/go.mod and hello/main.go
gopgext install hello    # builds hello.so, hello.control, hello--1.0.sql and installs them
psql -c 'CREATE EXTENSION hello' -c "SELECT hello('world')"
#     hello
# ---------------
#  Hello, world!
```

The [tutorial](https://jespino.github.io/gopgext/docs/tutorial/) walks through it step by step.

## A sibling project

gopgext uses the same approach as [gokm](https://jespino.github.io/gokm/), which builds Linux kernel modules from Go: a custom TinyGo target and runtime layer for the host, a C runtime compiled by the host’s own build system (PGXS here, kbuild there), and generated glue.
