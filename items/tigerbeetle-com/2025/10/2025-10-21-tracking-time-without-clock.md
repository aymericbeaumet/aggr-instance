---
title: Tracking Time Without Clock
link: https://tigerbeetle.com/blog/2025-10-21-clockless-time
source: tigerbeetle-com
published: 2025-10-21T00:00:00Z
updated: 2025-10-21T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- matklad
summary: A short note on a couple of code patterns for handling time!
content: extracted
html: 2025-10-21-tracking-time-without-clock.html
remote_preview:
  url: https://tigerbeetle.com/blog/2025-10-21-clockless-time/banner-79cd1b65.webp
---

A short note on a couple of code patterns for handling time!

The problem with time is that it is deceptively simple (just a syscall away), but is actually tricky to handle correctly in a reliable system! We had to write [a blog post](https://tigerbeetle.com/blog/2021-08-30-three-clocks-are-better-than-one/), give [a talk](https://youtu.be/7R-Iz6sJG6Q), and document [the implementation](https://github.com/tigerbeetle/tigerbeetle/blob/0.16.58/src/vsr/clock.zig#L1-L81) to explain all the details. As a result of this complexity, you generally don’t want to use the time directly as provided by your language standard library.

The brute-force pattern here is to introduce a `Clock` interface, and to require the code to pass around an *instance* of a clock:

```
pub const Clock = struct {
    context: *anyopaque,
    vtable: *const VTable,

    const VTable = struct {
        monotonic: *const fn (*anyopaque) u64,
        realtime: *const fn (*anyopaque) i64,
    };

    pub fn monotonic(self: Clock) Instant {
        return .{
            .ns = self.vtable.monotonic(self.context)
        };
    }

    pub fn realtime(self: Clock) i64 {
        return self.vtable.realtime(self.context);
    }
};
```

Full interface works, but it is somewhat complicated, as you need to arrange storage for the clock. But there are two tricks that allow bypassing the complexity for smaller components.

Often, you need only a single instant of time, the here and now. In this case, instead of parametrizing the component over `Clock`, you can pass a `now: Instant` to methods that need time ([Call Site Dependency Injection](https://matklad.github.io/2020/12/28/csdi.html)). We use this pattern for our [adaptive replication routing](https://github.com/tigerbeetle/tigerbeetle/blob/90f05d26dff66234a68f6cd5c79495c605172816/src/vsr/routing.zig#L461-L489):

```
pub fn op_prepare(
    routing: *Routing,
    now: Instant,
    op: u64,
) void {
   // ...
}

pub fn op_prepare_ok(
    routing: *Routing,
    now: Instant,
    op: u64,
    replica: u8,
) void {
    // ...
}
```

The replica passes the `now` of when an op was prepared, and when it was acknowledged by a backup:

```
self.routing.op_prepare(
    self.clock.monotonic(),
    prepare.header.op,
);

// Later

self.routing.op_prepare_ok(
    self.clock.monotonic(),
    prepare_ok.header.op,
    prepare_ok.header.replica,
);
```

The ARR component uses the two instances to learn how long it took to replicate a prepare, without needing an explicit clock. This makes testing edge-case behaviors, such as time overflows, easy as you don’t even have to come up with a fake overflowing clock, you can construct a bad `Instant` directly!

The second pattern has to do with the future. One thing you might want to do with time is to schedule some work to happen later. This sounds easy enough, but usually turns into a minor nightmare in testing, as the “future” control flow can easily outlive the system in question.

The trick here is to push time into the system at a fixed cadence. Instead of passing around a `clock` instance, define [a `tick` method](https://github.com/tigerbeetle/tigerbeetle/blob/0.16.58/src/vsr/replica.zig#L1506) on the component:

```
pub fn tick(self: *Replica) void {
    ...
}
```

The caller is required to [invoke it regularly](https://github.com/tigerbeetle/tigerbeetle/blob/0.16.58/src/tigerbeetle/main.zig#L491-L494)

```
while (true) {
    replica.tick();
    try io.run_for_ns(constants.tick_ms * std.time.ns_per_ms);
}
```

Of course, this only gives you a very coarse-grained resolution of time, but, for the use-case of scheduling the work in the future, you often don’t need precise time! And the control flow simplification here is massive!

That’s it for today! Remember, time is at the heart of essential complexity, and you want to treat it seriously! While virtualizing the entire clock is a universal approach, often times a `now` or `tick` pattern is all you need!
