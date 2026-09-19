---
title: Zig's New CLI Progress Bar Explained
link: https://andrewkelley.me/post/zig-new-cli-progress-bar-explained.html
source: andrewkelley-me
published: 2024-05-30T03:27:38Z
updated: 2024-05-30T03:27:38Z
first_seen: 2026-09-19T21:30:23.395188495Z
summary: 'Zig''s New CLI Progress Bar Explained Sometimes, programming projects are too easy and boring. Sometimes, they''re too hard, never ending or producing subpar results. This past week I had the pleasure of completing a project that felt like maximum difficulty - only possible because I am at the top of my game, using a programming language designed for making perfect software. This problem threw everything it had at me, but I rose to the challenge and emerged victorious. What a rush. In this blog post I''ll dig into the technical implementation as well as provide the Zig Progress Protocol Specification. Demo Before we take a deep dive, let''s look at the final results by building my music player side project while recording with Asciinema: Old vs New The usage code looks basically like this: const parent_progress_node = std.Progress.start(.{}); // ... const progress_node = parent_progress_node.start("sub-task name", 10); defer progress_node.end(); for (0..10) |_| progress_node.completeOne(); To include a child process''s progress under a particular node, it''s a single assignment before calling spawn: child_process.progress_node = parent_progress_node; For me the most exciting thing about this is its ability to visualize what the Zig Build System is up to after you run zig build. Before this feature was even merged into master branch, it led to discovery, diagnosis, and resolution of a subtle bug that has hidden in Zig''s standard library child process spawning code for years. Not included in this blog post: a rant about how much I hate the fork() API. Motivation The previous implementation was more conservative. It had the design limitation that it could not assume ownership of the terminal. This meant that it had to assume another process or thread could print to stderr at any time, and it was not allowed to register a SIGWINCH signal handler to learn about when the terminal size changed. It also did not rely on knowledge of the terminal size, or spawn any threads. This new implementation represents a more modern philosophy: when a CLI application is spawned with stderr being a terminal, then that application owns the terminal and owes its users the best possible user experience, taking advantage of all the terminal features available, working around their limitations. I''m excited for projects such as Ghostty which are expanding the user interface capabilities of terminals, and lifting those limitations. With this new set of constraints in mind, it becomes possible to design a much more useful progress bar. We gain a new requirement: since only one process owns the terminal, child processes must therefore report their progress semantically so it can be aggregated and displayed by the terminal owner. Implementation The whole system is designed around the public API, which must be thread-safe, lock-free, and avoid contention as much as possible in order to prevent the progress system itself from harming performance, particularly in a multi-threaded environment. The key insight I had here is that, since the end result must be displayed on a terminal screen, there is a reasonably small upper bound on how much memory is required, beyond which point the extra memory couldn''t be utilized because it wouldn''t fit on the terminal screen anyway. By statically pre-allocating 200 nodes, the API is made infallible and non-heap-allocating. Furthermore, it makes it possible to implement a thread-safe, lock-free node allocator based on a free list. The shared data is split into several arrays: node_parents: [200]Node.Parent, node_storage: [200]Node.Storage, node_freelist: [200]Node.OptionalIndex, The freelist is used to ensure that two racing Node.start() calls obtain different indexes to operate on, as well as a mechanism for Node.end() calls to return nodes back to the system for reuse. Meanwhile, the parents array is used to indicate which nodes are actually allocated, and to which parent node they should be attached to. Finally, the other storage contains the number of completed items, estimated total items, and task name for each node. Each Node.Parent is 1 byte exactly. It has 2 special values, which can be represented with type safety in Zig: const Parent = enum(u8) { /// Unallocated storage. unused = std.math.maxInt(u8) - 1, /// Indicates root node. none = std.math.maxInt(u8), /// Index into `node_storage`. _, fn unwrap(i: @This()) ?Index { return switch (i) { .unused, .none => return null, else => @enumFromInt(@intFromEnum(i)), }; } }; The data mentioned above is mutated in the implementation of the thread-safe public API. Meanwhile, the update thread is running on a timer. After an initial delay, it wakes up at regular intervals to either draw progress to the terminal, or send progress information to another process through a pipe. In either case, when the update thread wakes up, the first thing it does is "serialize" the shared data into a separate preallocated location. After carefully copying the shared data using atomic primitives, the copied, serialized data can then be operated on in a single-threaded manner, since it is not shared with any other threads. The update thread performs this copy by iterating over the full 200 shared parents array, atomically loading each value and checking if it is not the special "unused" value (0xfe). In most programming projects, a linear scan to find allocated objects is undesirable, however in this case 200 bytes for node parent indexes is practically free to iterate over since it''s 4 cache lines total, and the only contention comes from actual updates that must be observed. This full scan in the update thread buys us a cheap, lock-free implementation for Node.start() and Node.end() via popping and pushing the freelist, respectively. Next, IPC nodes are expanded. More on this point later. Once the serialization process is complete, we are left with a subset of the shared data from earlier, this time with no gaps - only used nodes are present here: serialized_parents: [200]Node.Parent, serialized_storage: [200]Node.Storage, serialized_len: usize, At this point the behavior diverges depending on whether the current process owns the terminal, or must send progress updates via a pipe. A process that fits neither of these categories would not have spawned an update thread to begin with. Any parent process that wants to track progress from children creates a pipe with O_NONBLOCK enabled, passing it to the child as if it were a fourth I/O stream after stdin, stdout, and stderr. To indicate to the process that the file descriptor is in fact a progress pipe, it sets the ZIG_PROGRESS environment variable. For example, in the Zig standard library implementation, this ends up being ZIG_PROGRESS=3. A process that is given a progress pipe sends the serialized data over the pipe, while a process that owns the terminal draws it directly. For example, this source code: const std = @import("std"); pub fn main() !void { const root_node = std.Progress.start(.{ .root_name = "preparing assets", }); defer root_node.end(); const sub_node = root_node.start("reticulating splines", 100); defer sub_node.end(); for (0..50) |_| sub_node.completeOne(); std.time.sleep(1000 * std.time.ns_per_ms); for (0..50) |_| sub_node.completeOne(); } At the sleep() call, would display this to the terminal: preparing assets └─ [50/100] reticulating splines But if it were a child process, would send this message over the pipe instead: 0000 02 00 00 00 00 00 00 00 00 70 72 65 70 61 72 69 .........prepari 00a0 6E 67 20 61 73 73 65 74 73 00 00 00 00 00 00 00 ng assets....... 00b0 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 ................ 00c0 00 32 00 00 00 64 00 00 00 72 65 74 69 63 75 6C .2...d...reticul 00d0 61 74 69 6E 67 20 73 70 6C 69 6E 65 73 00 00 00 ating splines... 00e0 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 ................ 00f0 00 FF 00 ... Drawing to the Terminal Drawing to the terminal begins with the serialized data copy. This data contains only edges pointing to parents, and therefore cannot be used to walk the tree starting from the root. The first thing done here is compute sibling and children edges into preallocated buffers so that we can then walk the tree top-down. child: [200]Node.OptionalIndex, sibling: [200]Node.OptionalIndex, Next, the draw buffer is computed, but not written to the terminal yet. It looks like this: Start sync sequence. This makes high framerate terminals not blink rapidly. Clear previous update by moving cursor up times the number of newlines outputted last time, and then a clear to end of screen escape sequence. Recursively walk the tree of nodes, which which is now possible since we have computed children and sibling edges, outputting tree-drawing sequences, node names, and counting newlines. End sync sequence. This draw buffer is only computed - it is not sent to the write() syscall yet. At this point, we try to obtain the stderr lock. If we get it, then we write the buffer to the terminal. Otherwise, this update is dropped. If any attempt to write to the terminal fails, the update thread exits so that no further attempt is made. Inter-Process Communication Earlier, I said "IPC nodes are expanded" without further explanation. Let''s dig into that a little bit. As a final step in the serialization process, the update thread iterates the data, looking for special nodes. Special nodes store the progress pipe file descriptor of a child process rather than completed_items and estimated_total_items. The update thread reads progress data from this pipe, and then grafts the child''s sub-tree onto the parent''s tree, plugging the root node of the child into the special node of the parent. The main storage data can be directly memcpy''d, but the parents array must be relocated based on the offset within the serialized data arrays. In case of a big-endian system, the 2 integers within each node storage must be byte-swapped. Not relying on host endianness means that edge cases continue to work, such as the parent process running on an x86_64 host, with a child process running a mips program in QEMU user mode. This is a real use case when testing the Zig compiler, for example. The parent process ignores all but the last message from the pipe fd, and references a copy of the data from the last update in case there is no message in the pipe for a particular update. Performance Here are some performance data points I took while working on this. Building the Zig compiler as a sub-process. This measures the cost of the new API implementations, primarily Node.start, Node.end, and the disappearance of Node.activate. In this case, standard error is not a terminal, and thus an update thread is never spawned: Benchmark 1 (3 runs): old zig build-exe self-hosted compiler measurement mean ± σ min … max outliers delta wall_time 51.8s ± 138ms 51.6s … 51.9s 0 ( 0%) 0% peak_rss 4.57GB ± 286KB 4.57GB … 4.57GB 0 ( 0%) 0% cpu_cycles 273G ± 360M 273G … 274G 0 ( 0%) 0% instructions 487G ± 105M 487G … 487G 0 ( 0%) 0% cache_references 19.0G ± 31.8M 19.0G … 19.1G 0 ( 0%) 0% cache_misses 3.87G ± 12.0M 3.86G … 3.88G 0 ( 0%) 0% branch_misses 2.22G ± 3.44M 2.21G … 2.22G 0 ( 0%) 0% Benchmark 2 (3 runs): new zig build-exe self-hosted compiler measurement mean ± σ min … max outliers delta wall_time 51.5s ± 115ms 51.4s … 51.7s 0 ( 0%) - 0.4% ± 0.6% peak_rss 4.58GB ± 190KB 4.58GB … 4.58GB 0 ( 0%) + 0.1% ± 0.0% cpu_cycles 272G ± 494M 272G … 273G 0 ( 0%) - 0.4% ± 0.4% instructions 487G ± 63.5M 487G … 487G 0 ( 0%) - 0.1% ± 0.0% cache_references 19.1G ± 16.9M 19.1G … 19.1G 0 ( 0%) + 0.3% ± 0.3% cache_misses 3.86G ± 17.1M 3.84G … 3.88G 0 ( 0%) - 0.2% ± 0.9% branch_misses 2.23G ± 5.82M 2.22G … 2.23G 0 ( 0%) + 0.4% ± 0.5% Building the Zig compiler, with time zig build-exe -fno-emit-bin ... so that the progress is updating the terminal on a regular interval: Old: 4.115s 4.216s 4.221s 4.227s 4.234s New (1% slower): 4.231s 4.240s 4.271s 4.339s 4.340s Building my music player application with zig build, with the project-local cache cleared. This displays a lot of progress information to the terminal. This data point accounts for many sub processes sending progress information over a pipe to the parent process for aggregation: Old 65.74s 66.39s 71.09s New (1% faster) 65.51s 65.88s 66.09s Conclusion? It appears that I succeeded in making this progress-reporting system have no significant effect on the performance of the software that uses it. The Zig Progress Protocol Specification Any programming language can join the fun! Here is a specification so that any software project can participate in a standard way of sharing progress information between parent and child processes. When the ZIG_PROGRESS=X environment variable is present, where X is an unsigned decimal integer in range 0...65535, a process-wide progress reporting channel is available. The integer is a writable file descriptor opened in non-blocking mode. Subsequent messages to the stream supersede previous ones. The stream supports exactly one kind of message that looks like this: len: u8 - number of nodes, limited to 253 max, reserving 0xfe and 0xff for special meaning. 48 bytes for every len: completed: u32le - how many items already done estimated_total: u32le - guessed number of items to be completed, or 0 (unknown) name: [40]u8 - task description; remaining bytes zeroed out 1 byte for every len: parent: u8 - creates an edge to a parent node in the tree, or 0xff (none) Future versions of this protocol, if necessary, will use different environment variable names. Bonus: Using Zig Standard Library in C Code Much of the Zig standard library is available to C programs with minimal integration pain. Here''s a full, working example: example.c #include "zp.h" #include <string.h> #include <unistd.h> int main(int argc, char **argv) { zp_node root_node = zp_init(); const char *task_name = "making orange juice"; zp_node sub_node = zp_start(root_node, task_name, strlen(task_name), 5); for (int i = 0; i < 5; i += 1) { zp_complete_one(sub_node); sleep(1); } zp_end(sub_node); zp_end(root_node); } zp.h #include <stdint.h> #include <stddef.h> typedef uint8_t zp_node; zp_node zp_init(void); zp_node zp_start(zp_node parent, const char *name_ptr, size_t name_len, size_t estimated_total); zp_node zp_end(zp_node node); zp_node zp_complete_one(zp_node node); zp.zig const std = @import("std"); export fn zp_init() std.Progress.Node.OptionalIndex { return std.Progress.start(.{}).index; } export fn zp_start( parent: std.Progress.Node.OptionalIndex, name_ptr: [*]const u8, name_len: usize, estimated_total_items: usize, ) std.Progress.Node.OptionalIndex { const node: std.Progress.Node = .{ .index = parent }; return node.start(name_ptr[0..name_len], estimated_total_items).index; } export fn zp_end(node_index: std.Progress.Node.OptionalIndex) void { const node: std.Progress.Node = .{ .index = node_index }; node.end(); } export fn zp_complete_one(node_index: std.Progress.Node.OptionalIndex) void { const node: std.Progress.Node = .{ .index = node_index }; node.completeOne(); } pub const _start = void; Compile with zig cc -o example example.c zp.zig Asciinema Demo This same executable will also work correctly as a child process, reporting progress over the ZIG_PROGRESS pipe if provided! Follow-Up Work Big thanks to Ryan Liptak for helping me with the Windows console printing code, and Jacob Young for working on the Windows IPC logic, which isn''t done yet.'
content: extracted
html: 2024-05-30-zig-s-new-cli-progress-bar-explained.html
---

Sometimes, programming projects are too easy and boring. Sometimes, they're too hard, never ending or producing subpar results.

This past week I had the pleasure of completing a project that felt like maximum difficulty - only possible because I am at the top of my game, using a programming language designed for making perfect software. This problem threw everything it had at me, but I rose to the challenge and emerged victorious.

What a rush.

In this blog post I'll dig into the technical implementation as well as provide the [Zig Progress Protocol Specification](https://andrewkelley.me/post/zig-new-cli-progress-bar-explained.html#zig-progress-protocol-spec).

## Demo

Before we take a deep dive, let's look at the final results by building my [music player side project](https://codeberg.org/andrewrk/player) while recording with Asciinema:

[Old](https://asciinema.org/a/MfJdqRHlMaHeNY8KJSnHBUP2I) vs [New](https://asciinema.org/a/661404)

The usage code looks basically like this:

```
const parent_progress_node = std.Progress.start(.{});

// ...

const progress_node = parent_progress_node.start("sub-task name", 10);
defer progress_node.end();

for (0..10) |_| progress_node.completeOne();
```

To include a child process's progress under a particular node, it's a single assignment before calling `spawn`:

```
child_process.progress_node = parent_progress_node;
```

For me the most exciting thing about this is its ability to visualize what the Zig Build System is up to after you run `zig build`. Before this feature was even merged into master branch, it led to discovery, diagnosis, and [resolution](https://github.com/ziglang/zig/commit/389181f6be8810b5cd432e236a962229257a5b59) of a subtle bug that has hidden in Zig's standard library child process spawning code for years. Not included in this blog post: a rant about how much I hate the fork() API.

## Motivation

The previous implementation was more conservative. It had the design limitation that it could not assume ownership of the terminal. This meant that it had to assume another process or thread could print to stderr at any time, and it was not allowed to register a `SIGWINCH` signal handler to learn about when the terminal size changed. It also did not rely on knowledge of the terminal size, or spawn any threads.

This new implementation represents a more modern philosophy: when a CLI application is spawned with stderr being a terminal, then that application owns the terminal and owes its users the best possible user experience, taking advantage of all the terminal features available, working around their limitations. I'm excited for projects such as [Ghostty](https://mitchellh.com/ghostty) which are expanding the user interface capabilities of terminals, and lifting those limitations.

With this new set of constraints in mind, it becomes possible to design a much more useful progress bar. We gain a new requirement: since only one process owns the terminal, child processes must therefore report their progress semantically so it can be aggregated and displayed by the terminal owner.

## Implementation

The whole system is designed around the public API, which must be thread-safe, lock-free, and avoid contention as much as possible in order to prevent the progress system itself from harming performance, particularly in a multi-threaded environment.

The key insight I had here is that, since the end result must be displayed on a terminal screen, there is a reasonably small upper bound on how much memory is required, beyond which point the extra memory couldn't be utilized because it wouldn't fit on the terminal screen anyway.

By statically pre-allocating 200 nodes, the API is made infallible and non-heap-allocating. Furthermore, it makes it possible to implement a thread-safe, lock-free node allocator based on a free list.

The shared data is split into several arrays:

```
node_parents: [200]Node.Parent,
node_storage: [200]Node.Storage,
node_freelist: [200]Node.OptionalIndex,
```

The freelist is used to ensure that two racing `Node.start()` calls obtain different indexes to operate on, as well as a mechanism for `Node.end()` calls to return nodes back to the system for reuse. Meanwhile, the parents array is used to indicate which nodes are actually allocated, and to which parent node they should be attached to. Finally, the other storage contains the number of completed items, estimated total items, and task name for each node.

Each `Node.Parent` is 1 byte exactly. It has 2 special values, which can be represented with type safety in Zig:

```
const Parent = enum(u8) {
    /// Unallocated storage.
    unused = std.math.maxInt(u8) - 1,
    /// Indicates root node.
    none = std.math.maxInt(u8),
    /// Index into `node_storage`.
    _,

    fn unwrap(i: @This()) ?Index {
        return switch (i) {
            .unused, .none => return null,
            else => @enumFromInt(@intFromEnum(i)),
        };
    }
};
```

The data mentioned above is mutated in the implementation of the thread-safe public API.

Meanwhile, the update thread is running on a timer. After an initial delay, it wakes up at regular intervals to either draw progress to the terminal, or send progress information to another process through a pipe.

In either case, when the update thread wakes up, the first thing it does is "serialize" the shared data into a separate preallocated location. After carefully copying the shared data using atomic primitives, the copied, serialized data can then be operated on in a single-threaded manner, since it is not shared with any other threads.

The update thread performs this copy by iterating over the full 200 shared parents array, atomically loading each value and checking if it is not the special "unused" value (0xfe). In most programming projects, a linear scan to find allocated objects is undesirable, however in this case 200 bytes for node parent indexes is practically free to iterate over since it's 4 cache lines total, and the only contention comes from actual updates that must be observed. This full scan in the update thread buys us a cheap, lock-free implementation for `Node.start()` and `Node.end()` via popping and pushing the freelist, respectively.

Next, IPC nodes are expanded. More on this point later.

Once the serialization process is complete, we are left with a subset of the shared data from earlier, this time with no gaps - only used nodes are present here:

```
serialized_parents: [200]Node.Parent,
serialized_storage: [200]Node.Storage,
serialized_len: usize,
```

At this point the behavior diverges depending on whether the current process owns the terminal, or must send progress updates via a pipe. A process that fits neither of these categories would not have spawned an update thread to begin with. Any parent process that wants to track progress from children creates a pipe with `O_NONBLOCK` enabled, passing it to the child as if it were a fourth I/O stream after stdin, stdout, and stderr. To indicate to the process that the file descriptor is in fact a progress pipe, it sets the `ZIG_PROGRESS` environment variable. For example, in the Zig standard library implementation, this ends up being `ZIG_PROGRESS=3`.

A process that is given a progress pipe sends the serialized data over the pipe, while a process that owns the terminal draws it directly.

For example, this source code:

```
const std = @import("std");

pub fn main() !void {
    const root_node = std.Progress.start(.{
        .root_name = "preparing assets",
    });
    defer root_node.end();

    const sub_node = root_node.start("reticulating splines", 100);
    defer sub_node.end();

    for (0..50) |_| sub_node.completeOne();
    std.time.sleep(1000 * std.time.ns_per_ms);
    for (0..50) |_| sub_node.completeOne();
}
```

At the sleep() call, would display this to the terminal:

```
preparing assets
└─ [50/100] reticulating splines
```

But if it were a child process, would send this message over the pipe instead:

```
0000  02 00 00 00 00 00 00 00  00 70 72 65 70 61 72 69  .........prepari
00a0  6E 67 20 61 73 73 65 74  73 00 00 00 00 00 00 00  ng assets.......
00b0  00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  ................
00c0  00 32 00 00 00 64 00 00  00 72 65 74 69 63 75 6C  .2...d...reticul
00d0  61 74 69 6E 67 20 73 70  6C 69 6E 65 73 00 00 00  ating splines...
00e0  00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  ................
00f0  00 FF 00                                          ...
```

### Drawing to the Terminal

Drawing to the terminal begins with the serialized data copy. This data contains only edges pointing to parents, and therefore cannot be used to walk the tree starting from the root. The first thing done here is compute sibling and children edges into preallocated buffers so that we can then walk the tree top-down.

```
child: [200]Node.OptionalIndex,
sibling: [200]Node.OptionalIndex,
```

Next, the draw buffer is computed, but not written to the terminal yet. It looks like this:

1. [Start sync sequence](https://gist.github.com/christianparpart/d8a62cc1ab659194337d73e399004036). This makes high framerate terminals not blink rapidly.
2. Clear previous update by moving cursor up times the number of newlines outputted last time, and then a clear to end of screen escape sequence.
3. Recursively walk the tree of nodes, which which is now possible since we have computed children and sibling edges, outputting tree-drawing sequences, node names, and counting newlines.
4. End sync sequence.

This draw buffer is only computed - it is not sent to the write() syscall yet. At this point, we try to obtain the stderr lock. If we get it, then we write the buffer to the terminal. Otherwise, this update is dropped.

If any attempt to write to the terminal fails, the update thread exits so that no further attempt is made.

### Inter-Process Communication

Earlier, I said "IPC nodes are expanded" without further explanation. Let's dig into that a little bit.

As a final step in the serialization process, the update thread iterates the data, looking for special nodes. Special nodes store the progress pipe file descriptor of a child process rather than `completed_items` and `estimated_total_items`. The update thread reads progress data from this pipe, and then grafts the child's sub-tree onto the parent's tree, plugging the root node of the child into the special node of the parent. The main storage data can be directly memcpy'd, but the parents array must be relocated based on the offset within the serialized data arrays.

In case of a big-endian system, the 2 integers within each node storage must be byte-swapped. Not relying on host endianness means that edge cases continue to work, such as the parent process running on an x86\_64 host, with a child process running a mips program in QEMU user mode. This is a real use case when testing the Zig compiler, for example.

The parent process ignores all but the last message from the pipe fd, and references a copy of the data from the last update in case there is no message in the pipe for a particular update.

## Performance

Here are some performance data points I took while working on this.

Building the Zig compiler as a sub-process. This measures the cost of the new API implementations, primarily `Node.start`, `Node.end`, and the disappearance of `Node.activate`. In this case, standard error is not a terminal, and thus an update thread is never spawned:

```
Benchmark 1 (3 runs): old zig build-exe self-hosted compiler
  measurement          mean ± σ            min … max           outliers         delta
  wall_time          51.8s  ±  138ms    51.6s  … 51.9s           0 ( 0%)        0%
  peak_rss           4.57GB ±  286KB    4.57GB … 4.57GB          0 ( 0%)        0%
  cpu_cycles          273G  ±  360M      273G  …  274G           0 ( 0%)        0%
  instructions        487G  ±  105M      487G  …  487G           0 ( 0%)        0%
  cache_references   19.0G  ± 31.8M     19.0G  … 19.1G           0 ( 0%)        0%
  cache_misses       3.87G  ± 12.0M     3.86G  … 3.88G           0 ( 0%)        0%
  branch_misses      2.22G  ± 3.44M     2.21G  … 2.22G           0 ( 0%)        0%
Benchmark 2 (3 runs): new zig build-exe self-hosted compiler
  measurement          mean ± σ            min … max           outliers         delta
  wall_time          51.5s  ±  115ms    51.4s  … 51.7s           0 ( 0%)          -  0.4% ±  0.6%
  peak_rss           4.58GB ±  190KB    4.58GB … 4.58GB          0 ( 0%)          +  0.1% ±  0.0%
  cpu_cycles          272G  ±  494M      272G  …  273G           0 ( 0%)          -  0.4% ±  0.4%
  instructions        487G  ± 63.5M      487G  …  487G           0 ( 0%)          -  0.1% ±  0.0%
  cache_references   19.1G  ± 16.9M     19.1G  … 19.1G           0 ( 0%)          +  0.3% ±  0.3%
  cache_misses       3.86G  ± 17.1M     3.84G  … 3.88G           0 ( 0%)          -  0.2% ±  0.9%
  branch_misses      2.23G  ± 5.82M     2.22G  … 2.23G           0 ( 0%)          +  0.4% ±  0.5%
```

Building the Zig compiler, with `time zig build-exe -fno-emit-bin
...` so that the progress is updating the terminal on a regular interval:

- Old:
  - 4.115s
  - 4.216s
  - 4.221s
  - 4.227s
  - 4.234s
- New (1% slower):
  - 4.231s
  - 4.240s
  - 4.271s
  - 4.339s
  - 4.340s

Building my music player application with `zig build`, with the project-local cache cleared. This displays a lot of progress information to the terminal. This data point accounts for many sub processes sending progress information over a pipe to the parent process for aggregation:

- Old
  - 65.74s
  - 66.39s
  - 71.09s
- New (1% faster)
  - 65.51s
  - 65.88s
  - 66.09s

Conclusion? It appears that I succeeded in making this progress-reporting system have no significant effect on the performance of the software that uses it.

## The Zig Progress Protocol Specification

Any programming language can join the fun! Here is a specification so that any software project can participate in a standard way of sharing progress information between parent and child processes.

When the `ZIG_PROGRESS=X` environment variable is present, where `X` is an unsigned decimal integer in range 0...65535, a process-wide progress reporting channel is available.

The integer is a writable file descriptor opened in non-blocking mode. Subsequent messages to the stream supersede previous ones.

The stream supports exactly one kind of message that looks like this:

1. `len: u8` - number of nodes, limited to 253 max, reserving 0xfe and 0xff for special meaning.
2. 48 bytes for every `len`:
   1. `completed: u32le` - how many items already done
   2. `estimated_total: u32le` - guessed number of items to be completed, or 0 (unknown)
   3. `name: [40]u8` - task description; remaining bytes zeroed out
3. 1 byte for every `len`:
   1. `parent: u8` - creates an edge to a parent node in the tree, or 0xff (none)

Future versions of this protocol, if necessary, will use different environment variable names.

## Bonus: Using Zig Standard Library in C Code

Much of the Zig standard library is available to C programs with minimal integration pain. Here's a full, working example:

### example.c

```
#include "zp.h"
#include <string.h>
#include <unistd.h>

int main(int argc, char **argv) {
    zp_node root_node = zp_init();

    const char *task_name = "making orange juice";
    zp_node sub_node = zp_start(root_node, task_name, strlen(task_name), 5);
    for (int i = 0; i < 5; i += 1) {
        zp_complete_one(sub_node);
        sleep(1);
    }
    zp_end(sub_node);
    zp_end(root_node);
}
```

### zp.h

```
#include <stdint.h>
#include <stddef.h>

typedef uint8_t zp_node;

zp_node zp_init(void);
zp_node zp_start(zp_node parent, const char *name_ptr, size_t name_len, size_t estimated_total);
zp_node zp_end(zp_node node);
zp_node zp_complete_one(zp_node node);
```

### zp.zig

```
const std = @import("std");

export fn zp_init() std.Progress.Node.OptionalIndex {
    return std.Progress.start(.{}).index;
}

export fn zp_start(
    parent: std.Progress.Node.OptionalIndex,
    name_ptr: [*]const u8,
    name_len: usize,
    estimated_total_items: usize,
) std.Progress.Node.OptionalIndex {
    const node: std.Progress.Node = .{ .index = parent };
    return node.start(name_ptr[0..name_len], estimated_total_items).index;
}

export fn zp_end(node_index: std.Progress.Node.OptionalIndex) void {
    const node: std.Progress.Node = .{ .index = node_index };
    node.end();
}

export fn zp_complete_one(node_index: std.Progress.Node.OptionalIndex) void {
    const node: std.Progress.Node = .{ .index = node_index };
    node.completeOne();
}

pub const _start = void;
```

Compile with `zig cc -o example example.c zp.zig`

[Asciinema Demo](https://asciinema.org/a/dJ8iLKpje6fsJygM8r1qw5e6M)

This same executable will also work correctly as a child process, reporting progress over the `ZIG_PROGRESS` pipe if provided!

## Follow-Up Work

Big thanks to [Ryan Liptak](https://www.ryanliptak.com/) for helping me with the Windows console printing code, and [Jacob Young](https://github.com/jacobly0/) for [working on the Windows IPC logic](https://github.com/ziglang/zig/pull/20114), which isn't done yet.

Thanks for reading my blog post.
