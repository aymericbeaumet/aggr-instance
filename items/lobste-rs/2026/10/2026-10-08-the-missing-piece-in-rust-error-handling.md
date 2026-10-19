---
title: The Missing Piece in Rust Error Handling
link: https://mcmah309.github.io/posts/the-missing-piece-in-rust-error-handling/
source: lobste-rs
published: 2026-10-08T14:43:19Z
updated: 2026-10-08T14:43:19Z
first_seen: 2026-10-09T18:47:50.488026480Z
authors:
- mcmah309.github.io via snej
labels:
- rust
summary: Comments
content: extracted
html: 2026-10-08-the-missing-piece-in-rust-error-handling.html
---

Rust already has most of what I want from error handling: explicit control flow, errors as values, and concise propagation with `?`. The friction comes when deciding what to put in the error half of `Result`. We often end up choosing between precise types that require boilerplate and convenient types that hide which errors can occur. But precision and convenience do not have to be competing goals. Error types should compose as easily as the functions that return them.

## The Problem With Rust Error Handling

Consider reading a server port from a file. Reading can fail with an `io::Error`, and parsing can fail with a `ParseIntError`. A conventional implementation might look like this:

```rust
use std::{io, num::ParseIntError};

#[derive(Debug, thiserror::Error)]
pub enum PortError {
    #[error(transparent)]
    Io(#[from] io::Error),
    #[error(transparent)]
    Parse(#[from] ParseIntError),
}

fn load_port(path: &str) -> Result<u16, PortError> {
    let contents = std::fs::read_to_string(path)?;
    Ok(contents.trim().parse()?)
}
```

[thiserror](https://github.com/dtolnay/thiserror) removes the manual `Display`, `Error`, and `From` implementations. But we still have to decide how this enum relates to every other error enum in our program.

Now load a host address, bind a socket, and initialize a database. Each operation has its own errors. We can wrap those enums in another enum, flatten their variants into a new enum, or give everything one large crate-wide error type. The first approach creates nesting, the second creates conversions, and the third means functions advertise errors they cannot actually return. An I/O error may also end up in several different nested variants, making handling it at a higher level unnecessarily awkward.

Alternatively, an [anyhow](https://github.com/dtolnay/anyhow) like approach makes propagation and attaching context straightforward. We can downcast when we need to inspect a concrete error. However, the function signature no longer tells us which error types are possible, and the compiler cannot track whether we have handled all of them.

The usual advice is to use typed errors in libraries and opaque errors in applications. But applications need typed recovery too, and libraries often contain internal operations whose callers only need to propagate a failure. The useful distinction is whether a caller needs to **do something different based on the error type**.

## Error Types Should Compose

What we actually want to say is simple: this function can fail with an `io::Error` or a `ParseIntError`. Declaring an enum is one way to express that, but the combination itself should not need a new type declaration.

I use [eros](https://github.com/mcmah309/eros) to express this as an error set. The port example becomes:

```rust
use eros::IntoUnion;
use std::{io, num::ParseIntError};

fn load_port(path: &str) -> eros::Result<u16, (io::Error, ParseIntError)> {
    let contents = std::fs::read_to_string(path).union()?;
    contents.trim().parse().union()
}
```

No new enum or conversions need to be declared. For reuse, the set can be named with a normal type alias:

```rust
type PortErrors = (std::io::Error, std::num::ParseIntError);
```

`eros::Result<T, E>` is an alias for the ordinary `Result<T, ErrorUnion<E>>`. Here, `ErrorUnion<(io::Error, ParseIntError)>` holds **one** of the listed errors. The tuple describes the possible types; it does not store both errors. This is an *open sum type*: we describe the combination we need without declaring a new named enum for that combination.

`.union()` wraps an ordinary result’s error in an `ErrorUnion`, inferring the destination set from the surrounding code. If we remove `io::Error` from this signature, the file read no longer compiles. We cannot accidentally propagate an error that the signature does not include.

This becomes more useful when functions are combined. Suppose we also load the server’s host address. Building on `load_port`:

```rust
use eros::ReshapeUnion;
use std::net::{AddrParseError, IpAddr, TcpListener};

fn load_host(path: &str) -> eros::Result<IpAddr, (io::Error, AddrParseError)> {
    let contents = std::fs::read_to_string(path).union()?;
    contents.trim().parse().union()
}

fn bind_server() -> eros::Result<TcpListener, (io::Error, AddrParseError, ParseIntError)> {
    let host = load_host("config/host.txt").widen()?;
    let port = load_port("config/port.txt").widen()?;
    TcpListener::bind((host, port)).union()
}
```

`.widen()` converts an existing union into a union whose set contains all its possible errors. Both configuration operations can return an `io::Error`, so we list it once. Context can describe which operation failed.

Widening into a set that omits a possible error is rejected at compile time. The caller describes the combined possibilities without wrapping each function’s errors in another layer of enums. Adding another operation means adding its possible errors to the set, and the compiler checks that we have accounted for them.

## Handling Errors Changes The Type

Declaring precise errors becomes much more useful when handling an error removes it from the set.

For example, suppose our policy is to use port 8080 whenever reading the port file fails, while still rejecting malformed contents:

```rust
fn port_or_default(path: &str) -> eros::Result<u16, (ParseIntError,)> {
    load_port(path).recover::<io::Error, _>(|error| {
        eprintln!("{error:?}; using port 8080");
        8080
    })
}
```

The return type now contains only `ParseIntError`. `recover` handles the selected error type and turns the handler’s value into a success. Other errors pass through unchanged.

This is the part I find most useful. The signature describes what can still go wrong after our recovery policy has run. A caller does not need to know that an I/O error was possible somewhere below it, because that error has already been handled.

We can also recover a group of error types. If both unreadable files and invalid numbers should use a default, all possible errors can be removed:

```rust
fn forgiving_port(path: &str) -> u16 {
    load_port(path)
        .recover::<(io::Error, ParseIntError), _>(|_| 8080)
        .into_value()
}
```

After recovery, the result has the empty error set `()`. `.into_value()` extracts the value, and only compiles when no possible errors remain.

## Types Only Where They Matter

Sometimes the caller has no useful recovery policy. It only needs to propagate an error or report it at the top of the program. Carrying every possible error type through that signature may just be noise:

```rust
fn load_port_untyped(path: &str) -> eros::Result<u16> {
    let contents = std::fs::read_to_string(path)?;
    Ok(contents.trim().parse()?)
}
```

Without a tuple, the error set defaults to `AnyError`. Typed results can flow into this catch-all form with `?` as well:

```rust
fn start_server() -> eros::Result<TcpListener> {
    Ok(bind_server()?)
}
```

We can keep lower-level functions precise for callers that need recovery, while allowing other callers to propagate the same errors through a simpler signature. Context and backtraces survive this conversion.

This choice can be made at each boundary. We do not need to commit an entire library or application to one approach. Keep the types where callers make decisions based on them, and erase them where callers only need to pass the failure along.

## Errors Need Operational Context

A precise error type does not tell us which file was being read or why. `PermissionDenied` is useful for making a decision, but we still need the path and operation to understand the failure.

That information belongs with the error as it moves through the program. For example:

```rust
use eros::{Context, context};

#[context("Load server port from {}", path)]
fn load_port_with_context(path: &str) -> eros::Result<u16, (io::Error, ParseIntError)> {
    let contents = std::fs::read_to_string(path).union()?;
    contents.trim().parse().union()
}

fn configure_server(path: &str) -> eros::Result<u16> {
    Ok(load_port_with_context(path).context("Configure server")?)
}

fn main() {
    if let Err(error) = configure_server("config/port.txt").context("Start application") {
        eprintln!("{error:#?}");
    }
}
```

If the file contains an invalid number, the report is:

```plain
invalid digit found in string

  Context (innermost first):
    1. Load server port from config/port.txt
    2. Configure server
    3. Start application
```

The original error stays the main message, with the operations listed in the order they were added.

I generally prefer a function to describe its own operation and relevant inputs. Every caller then gets that context. Call sites can add context too, when they know something the callee does not. We can report the failure once with the operations that led to it.

The type tells us which recovery policy to apply. The context tells us what happened if recovery is not possible. We should be able to keep both without building a new error enum every time an error passes through another function.

## Conclusion

I previously explored precise error sets with [error\_set](https://mcmah309.github.io/posts/introducing-error-set/). What still interests me is how little needs to change about Rust’s existing error handling to make this work. We still return `Result`, propagate with `?`, and handle errors as values. The missing piece is making the possible errors easy to compose and reduce as they move through the program.

This is why I think Rust’s error handling is near perfect with the right constructs. Each function can describe the errors its callers need to reason about. We can handle those errors where there is a useful policy, simplify the signature where there is not, and keep the operational context needed to understand a failure. Precise error handling becomes much easier to use when it follows the way we already compose functions. That is why eros lets me love error handling—pun intended.

*The eros source and README are available on [GitHub](https://github.com/mcmah309/eros).*

## Bonus: The Connection To Zig

Zig’s native [error sets](https://ziglang.org/documentation/0.17.0/#Error-Set-Type) follow the same idea:

```zig
const std = @import("std");

const PortErrors = std.fmt.ParseIntError || error{ ZeroPort };

fn parsePort(input: []const u8) PortErrors!u16 {
    const port = try std.fmt.parseInt(u16, input, 10);
    if (port == 0) return error.ZeroPort;
    return port;
}
```

`||` combines the sets, and `try` propagates errors like `?`. Zig can also infer the set when the return type is written as `!u16`.

Zig’s error codes have no attached payloads. In Rust, we can keep the same composability and carry actual data:

```rust
use eros::IntoUnion;
use std::num::ParseIntError;

#[derive(Debug, thiserror::Error)]
#[error("Port must be nonzero, got {input:?}")]
struct ZeroPort {
    input: String,
}

type PortErrors = (ParseIntError, ZeroPort);

fn parse_port(input: &str) -> eros::Result<u16, PortErrors> {
    let port = input.parse::<u16>().union()?;
    if port == 0 {
        return Err(ZeroPort { input: input.into() }).union();
    }
    Ok(port)
}
```
