---
title: What Zig felt like, coming from Rust
link: https://besok.github.io/posts/what-zig-felt-like-coming-from-rust/
source: hnrss-org-frontpage
published: 2026-09-19T13:55:18Z
updated: 2026-09-19T13:55:18Z
first_seen: 2026-09-19T19:18:40.363745098Z
authors:
- ksec
summary: 'Article URL: https://besok.github.io/posts/what-zig-felt-like-coming-from-rust/ Comments URL: https://news.ycombinator.com/item?id=49766637 Points: 135 # Comments: 152'
content: extracted
html: 2026-09-19-what-zig-felt-like-coming-from-rust.html
---

## Intro

I’ve spent the last 7 years as a Rust developer, working mostly on open source projects, and I’d like to think I’ve built a solid feel for the language and its ecosystem along the way. I gravitate toward the functional side of Rust like clean functions, expressive types, that sort of thing. But I’m always curious about other languages, and Zig has been on my radar for a while as a candidate C successor: lower-level, lighter-weight, and steadily earning its place among the languages people take seriously. I spent time with C earlier in my career, so the comparison always felt like it would be interesting to make.

One caveat worth stating up front: my experience with Zig begins with this project. Some of the observations will look naive and obvious for the people who work with Zig on daily basis and some of the decisions I made along the way were almost certainly not the optimal ones, they were shaped more by habits carried over from Rust than by deep Zig idiom. That’s fine, everyone has to start somewhere, and in the meantime I’m leaning on whatever cross-language intuition I’ve built up over the years, for better or worse.

To make the comparison fair, I decided to reimplement something I’d already built in Rust, not a toy, but not a sprawling project either, and ideally something the community could actually use. I settled on JSONPath: a query language for JSON, specified in RFC 9535. The Rust version already existed ([jsonpath-rust](https://github.com/besok/jsonpath-rust)), and the goal was to bring the same thing to Zig: [zig-jsonpath](https://github.com/besok/zig-jsonpath).

## IDE support

The first thing that caught me off guard — and honestly, who would’ve expected this to be the memorable part — was IDE support, or the near-total lack of it. I’d been using RustRover for Rust and various JetBrains flavors for other languages, and Zig, by comparison, offered little beyond syntax highlighting and basic autocompletion. It wasn’t exactly surprising, but it did force me back to basics: learning to work with the language largely from the command line. What started as a drawback turned into one of the more interesting parts of the experience. It turns out I’d simply forgotten how straightforward it can be to rely on bare CLI tooling.

The first real lesson here was `build.zig`, which handles this with surprising ease. I eventually settled on this setup:

```shell
zig build test                                              # run all tests
zig build test -Dfilter="filter match function basic"       # run one test
zig build test -Ddebug-query=true                           # all tests with debug
zig build compliance                                        # compliance suite
zig build check                                             # unit tests + compliance
```

Once you accept the terms, it’s genuinely refreshing to work with.

I have Zig to thank, in a roundabout way, for kicking off a bigger chain reaction, namely my move away from a full IDE toward a [helix + alacritty + zellij](https://github.com/besok/dotfiles) setup.

## Flat structure

With Rust, and most other languages, I’ve always spent a fair amount of time (going back and forth) trying to find the right balance between file size and folder depth. You’re free to fragment files and grow the folder hierarchy as deep as you like. Zig, it turned out, is fine with this too, but somehow doesn’t really encourage it (like C, which is no surprise for a low-level systems language). You can nest files and folders if you want, but doing so brings a bit of import friction, and the real question becomes: why bother? What do you actually gain in readability by splitting everything across more files and folders? In theory, better readability. In practice, when you collapse related things into one larger file, you can just slice it and navigate section by section instead and there’s a real benefit to having everything in one place. Mostly, Zig nudges you toward flat. If something needs a companion for a model, I just create a `model_<companion>` file next to it and move on.

I don’t think this scales to large projects, meaning at some point you need a real hierarchy but the threshold for needing one turned out to be much higher in Zig than I expected. In Rust, I tend to reach for folder structure early, almost by default. In Zig, I kept deferring it, and by the end of this project, I never needed it at all.

That contrast was useful beyond just Zig, because it made me reconsider, even in other languages, whether I’m organizing files because the project genuinely needs it, or out of habit. It’s also a pretty honest way to gauge how big a project actually is: if you can’t resist reaching for folders on day one,maybe it’s smaller than it feels.

Here’s the actual difference, side by side:

Rust (`src/`):

```shell
src/
├── lib.rs
├── parser.rs
├── parser/
│   ├── errors.rs
│   ├── macros.rs
│   ├── model.rs
│   ├── tests.rs
│   └── grammar/
│       └── json_path_9535.pest
├── query.rs
└── query/
    ├── atom.rs
    ├── comparable.rs
    ├── comparison.rs
    ├── filter.rs
    ├── jp_query.rs
    ├── queryable.rs
    ├── segment.rs
    ├── selector.rs
    ├── state.rs
    ├── test.rs
    └── test_function.rs
```

Zig (`src/`):

```shell
src/
├── root.zig
├── parser.zig
├── model.zig
├── model_query.zig
└── query.zig
```

## Tests

Setting the rfc9535 compliance suite aside for now and focusing purely on the language itself:

In Rust, I tend to stick with two approaches to testing:

- Inline unit tests, living in the same file or same folder as the code they cover. This is the convenient default always there, no extra setup.
- Integration tests, in an independent folder (like `tests`) outside the main source tree. This is the exception not the default, and sometimes absent altogether.

I expected roughly the same split from Zig. On paper, it looks similar: you can write tests directly inside the same file. The problem, at least for me, was verbosity. Given the flat structure I’d already settled into, I was left with two options, either a separate `model_test` file per model, or tests inlined directly into the model file itself. Both approaches ended up cluttering things: either the individual files or the main folder as a whole.

I went with the second option, which meant configuring it explicitly in `build.zig`. Once that was wired up, though, it worked well and stayed clean.

So overall: writing and managing tests feels easier to me in Rust. But in Zig’s case, much of that extra friction is language-specific, it comes down to Zig’s manual memory management rather than testing infrastructure itself.

## No functional paradigm

Rust is technically an imperative language, but it draws heavily on functional concepts: zero-cost iterators, lazy evaluation, ADTs, pattern matching, monadic types, traits, closures, and so on. Having also spent time with Haskell and Erlang, I’ve become fairly inclined toward the functional style, and it shows in this library. It leans heavily on FP idioms:

- Monadic error control via combinators like `Queryable` and related types
- Monadic-style data types like `Data<T>` with `map`, `flat_map`, `reduce`, and friends
- Pure, immutable transformations
- Combinators over iterators instead of loops
- Closures for local abstraction
- Declarative macros as a small embedded DSL
- Sum types and product types

I knew going in that I wouldn’t be able to bring all of this to Zig, but I hoped I could at least preserve the core concepts. In practice, where Rust leans on immutability and combinators, Zig pushed me toward in-place mutation and the pattern most native to the imperative world.

Where the two stay close: **sum types**.

Pure and direct in Rust:

```rust
pub trait Query {
    fn process<'a, T: Queryable>(&self, state: State<'a, T>) -> State<'a, T>;
}

impl Query for Segment {
    fn process<'a, T: Queryable>(&self, step: State<'a, T>) -> State<'a, T> {
        match self {
            Segment::Descendant(segment) => segment.process(step.flat_map(process_descendant)),
            Segment::Selector(selector) => selector.process(step),
            Segment::Selectors(selectors) => process_selectors(step, selectors),
        }
    }
}
```

Duck-typed in Zig:

```zig
pub fn query(node: anytype, iteration: *JsonPathIter) !void {
    const T = switch (@typeInfo(@TypeOf(node))) {
        .pointer => |p| p.child,
        else => @TypeOf(node),
    };
    if (!@hasDecl(T, "query")) {
        return; // no compile-time trait; just checks the method exists
    }
    try node.query(iteration);
}
```

**Recursion holds up on both sides too**.

Rust:

```rust
fn process_descendant<T: Queryable>(data: Pointer<T>) -> Data<T> {
    if let Some(array) = data.inner.as_array() {
        Data::Ref(data.clone()).reduce(
            Data::new_refs(/* children */).flat_map(process_descendant)
        )
    } else { Data::Nothing }
}
```

Zig:

```zig
fn collectDescendants(allocator, value: *std.json.Value, path, out) !void {
    try out.append(allocator, .{ .json = value, .path = try allocator.dupe(u8, path) });
    switch (value.*) {
        .array => |arr| for (arr.items) |*elem| try collectDescendants(allocator, elem, child_path, out),
        else => {},
    }
}
```

But the language quickly forces you to diverge from the functional style, mostly because you’re now dealing with allocators directly, and a genuinely pure functional approach means constantly constructing new structures. That’s either expensive in memory or expensive in the manual bookkeeping needed to avoid it.

**Mutation vs. immutable monad is the core difference**.

Rust does a straightforward monadic transformation:

```rust
pub fn flat_map<F>(self, f: F) -> Data<'a, T> {
    match self {
        Data::Ref(data) => f(data),      // returns a *new* Data
        Data::Refs(v) => Data::Refs(v.into_iter().flat_map(...).collect()),
        _ => Data::Nothing,
    }
}
```

Zig switches to mutation:

```zig
pub fn queryName(name: []const u8, iteration: *q.JsonPathIter) !void {
    while (i < iteration.cursors.items.len) {
        if (obj.getPtr(name)) |val| {
            iteration.cursors.items[i] = .{ .json = val, .path = new_path }; // in-place overwrite
        } else iteration.remove(i);                                          // mutate list directly
    }
}
```

**Reduce vs Fork**.

Rust:

```rust
selectors.iter().map(|s| s.process(step.clone())).reduce(State::reduce)
```

Zig:

```zig
var lhs_branch = try iter.fork();   // deep copy of cursor state
defer lhs_branch.deinit();          // then discarded
```

**Combinators vs. loops**.

Rust:

```rust
items.iter().enumerate().filter(|(_, i)| cond(i)).map(|(idx, i)| Pointer::idx(i, path, idx)).collect()
```

Zig:

```zig
while (i < cursors.len) {
    if (actual_index < arr.items.len) { cursors[i] = .{...}; i += 1; }
    else iteration.remove(i);
}
```

All told, this reflects each language’s design goals and target domain, and it’s a reasonable trade-off but subjectively, I found the resulting Zig code less readable than its Rust counterpart.

## Allocators

Allocators are everywhere. Almost every function accepts one; every structure holds one. It’s explicit, and once you accept that as the cost of entry, it’s relatively straightforward to follow. This is more or less the language’s defining feature, so I can’t say I wasn’t warned.

In practice, though, the process is tedious. You have to meticulously follow the init/deinit convention, and that discipline gets shaky the moment your call stack grows long. It’s a clear improvement over a silent segfault or corrupted memory in C, but coming from Rust, you’re still the one enforcing the rule by hand: allocate something, handle the failure path, decide who’s responsible for deinit, every single time.

Fortunately, Zig’s `TestAllocator` comes to the rescue here. It won’t catch everything automatically, you still need to write the test cases that exercise the failure paths — but once you do, it’s fairly reliable. And that’s the trap: this all looks obvious on paper, right up until the code gets more complex, at which point these bugs tangle themselves up and hide.

Here are the cases that hit hardest, each compared against how Rust handles the same shape:

### Memory leak: forgotten deinit

```zig
var iter = q.JsonPathIter.init(&root, std.testing.allocator);
try iter.append(&root, "$['a']");
// BUG: no iter.deinit()
```

**Caught by**: `MemoryLeakDetected`, pointing at the `dupe` call inside `append`.

**Fix**: `defer iter.deinit();` right after init.

**Rust**: `Drop` runs automatically at scope end, so this specific bug simply doesn’t exist. Though technically, leaks are still possible in Rust like `Rc` reference cycles, or an explicit `Box::leak` so “never leaks” isn’t a hard guarantee, just something you’d have to go out of your way to trigger.

### Memory leak: deinit skipped on error path

```zig
fn build(json: *Value, a: Allocator) !q.JsonPathIter {
    var iter = q.JsonPathIter.init(json, a);
    try iter.append(json, "$['a']"); // ok
    try iter.append(json, "$['b']"); // fails -> iter leaked
    return iter;
}
```

**Caught by**: `FailingAllocator{ .fail_index = 1 }`, which forces the second append into `MemoryLeakDetected`.

**Fix**: `errdefer iter.deinit();` right after init.

**Rust**: truly eliminated. `Drop::drop` fires unconditionally on any scope exit, including early returns from `?`.

### Memory corruption: deinit called twice

```zig
fn runQuery(json: *Value, qstr: []const u8, a: Allocator) !q.JsonPathResult {
    var iter = q.JsonPathIter.init(json, a);
    errdefer iter.deinit();
    try q.query(qstr, &iter);
    return iter.toResult(parsed); // ownership moves to caller
}

fn cacheAndLog(json: *Value, qstr: []const u8, a: Allocator, cache: *std.ArrayList(q.JsonPathResult)) !void {
    var result = try runQuery(json, qstr, a);
    try cache.append(result);   // cache now holds a (shallow) copy of result's pointers
    defer result.deinit();      // BUG: frees the same heap data cache.items still points to
    printResults(&result);
}

fn processAll(json: *Value, queries: [][]const u8, a: Allocator) !void {
    var cache = std.ArrayList(q.JsonPathResult).init(a);
    defer {
        for (cache.items) |*r| r.deinit();  // frees the SAME memory Layer 2 already freed
        cache.deinit();
    }
    for (queries) |qs| try cacheAndLog(json, qs, a, &cache);
}
```

**Caught by**: running under `std.testing.allocator`, which fails on the second query’s `cache.items[0].deinit()` during `processAll`’s cleanup, `DoubleFree` pointing at both free sites, confirming this is a cross-function ownership bug, not a single-line typo.

**Fix**: only one layer may own the value. Since `cache` outlives `cacheAndLog`, ownership belongs to layer three; layer two must not `defer deinit` after handing it off:

```zig
fn cacheAndLog(json: *Value, qstr: []const u8, a: Allocator,
                cache: *std.ArrayList(q.JsonPathResult)) !void {
    var result = try runQuery(json, qstr, a);
    printResults(&result);      // use it first
    try cache.append(result);   // then hand off ownership — no defer after this
}
```

**Rust**: this exact shape can’t compile. `cache.push(result)` moves `result` — after that line, `result` no longer exists as a usable binding, so there’s no way to later call `drop(result)` by accident.

### Memory corruption: orphaned allocation when moving into a struct fails

```zig
pub fn appendBuggy(self: *Iter, v: *Value, path: []const u8) !void {
    const duped = try self.allocator.dupe(u8, path);
    // BUG: no errdefer
    try self.cursors.append(self.allocator, .{ .json = v, .path = duped });
}
```

**Caught by**: `FailingAllocator{ .fail_index = 1 }` failing the array’s growth (the second allocation), orphaning `duped` (the first allocation).

This leaks in a way distinct from case one: `iter.deinit()` runs fine, it just never sees this particular string.

**Fix**:

```zig
const duped = try self.allocator.dupe(u8, path);
errdefer self.allocator.free(duped);   // only fires if append below fails
try self.cursors.append(self.allocator, .{ .json = v, .path = duped });
```

**Rust**: true by construction. `Vec::push(item)` moves `item` in and either succeeds or aborts on OOM and there’s no fallible push in the standard API that hands you back an “allocated but unlinked” value to accidentally lose. The gap that `errdefer` fills here simply doesn’t exist to begin with.

## Libraries and the core API

The ecosystem is still very young. There’s a real scarcity of libraries, and even something as basic as regex isn’t fully mature, for instance `mvzr`, the regex engine available in Zig, doesn’t support Unicode property escapes (`\p{...}`), which surfaced directly as a gap while implementing RFC 9535’s filter functions. On top of that, the language’s own standard library changes its API from version to version. None of this was surprising going in, but it’s worth noting for the record.

## Overall impression

The language is different from Rust (who could’ve thought that, yeah), but it left a genuinely good impression. It’s straightforward, modern, and blazingly fast. I believe it has real potential to become the true successor to C. On the other hand, it’s still young, and it shows: the shape of the language itself feels unfinished in places, and I suspect it’ll pick up more of the cooler quality-of-life features and syntax sugar as it matures.

As for me, I’d like to keep contributing to the ecosystem, and I will, whenever I come across a project worth building.

## Links

- [jsonpath-rust](https://github.com/besok/jsonpath-rust)
- [zig-jsonpath](https://github.com/besok/zig-jsonpath)
- [Zig](https://ziglang.org)
- [Rust](https://www.rust-lang.org)

*Disclaimer: styling and error handling throughout this article were cleaned up with the help of AI.*
