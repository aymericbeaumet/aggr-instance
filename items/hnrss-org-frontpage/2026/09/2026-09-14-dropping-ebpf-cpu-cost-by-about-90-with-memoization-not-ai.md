---
title: Dropping eBPF CPU Cost by About 90% with Memoization (Not AI Gen)
link: https://nathannaveen.dev/posts/dropping-ebpf-cpu-cost-by-90/
source: hnrss-org-frontpage
published: 2026-09-14T14:29:23Z
updated: 2026-09-14T14:29:23Z
first_seen: 2026-09-15T12:38:22.798633021Z
authors:
- nathannaveen
summary: 'Article URL: https://nathannaveen.dev/posts/dropping-ebpf-cpu-cost-by-90/ Comments URL: https://news.ycombinator.com/item?id=49697477 Points: 135 # Comments: 28'
content: extracted
html: 2026-09-14-dropping-ebpf-cpu-cost-by-about-90-with-memoization-not-ai.html
preview:
  file: 2026-09-14-dropping-ebpf-cpu-cost-by-about-90-with-memoization-not-ai.preview-ce022d052cf8.webp
  width: 170
  height: 256
  alt: On a cache hit, file open builds an inode cache key and goes straight to allow or deny. On a miss, it walks parent dentries, merges the policy, stores the result, then allows or denies.
  color: '#fafbfd'
images:
- source: https://nathannaveen.dev/img/BMBlogDiagram.jpeg
  original:
    file: 2026-09-14-dropping-ebpf-cpu-cost-by-about-90-with-memoization-not-ai.image-d2caa20c7904.jpg
    width: 1000
    height: 1508
  variants:
  - file: 2026-09-14-dropping-ebpf-cpu-cost-by-about-90-with-memoization-not-ai.image-61401f6895bb.webp
    width: 320
    height: 483
  - file: 2026-09-14-dropping-ebpf-cpu-cost-by-about-90-with-memoization-not-ai.image-370d2d9116db.webp
    width: 640
    height: 965
  - file: 2026-09-14-dropping-ebpf-cpu-cost-by-about-90-with-memoization-not-ai.image-5616deef652a.webp
    width: 960
    height: 1448
  - file: 2026-09-14-dropping-ebpf-cpu-cost-by-about-90-with-memoization-not-ai.image-66852af5832a.webp
    width: 1000
    height: 1508
  color: '#fefefe'
- source: https://nathannaveen.dev/img/kernel-flamegraph-no-cache-annotated.svg
  original:
    file: 2026-09-14-dropping-ebpf-cpu-cost-by-about-90-with-memoization-not-ai.image-df4f4c888ae5.png
    width: 1200
    height: 598
  variants:
  - file: 2026-09-14-dropping-ebpf-cpu-cost-by-about-90-with-memoization-not-ai.image-771b6c7d6671.webp
    width: 320
    height: 159
  - file: 2026-09-14-dropping-ebpf-cpu-cost-by-about-90-with-memoization-not-ai.image-aa1052037f6e.webp
    width: 640
    height: 319
  - file: 2026-09-14-dropping-ebpf-cpu-cost-by-about-90-with-memoization-not-ai.image-ac673271b76f.webp
    width: 1200
    height: 598
  color: '#ecece7'
- source: https://nathannaveen.dev/img/kernel-flamegraph-cache-annotated.svg
  original:
    file: 2026-09-14-dropping-ebpf-cpu-cost-by-about-90-with-memoization-not-ai.image-3c0353f1560e.png
    width: 1200
    height: 534
  variants:
  - file: 2026-09-14-dropping-ebpf-cpu-cost-by-about-90-with-memoization-not-ai.image-e1853b5a9025.webp
    width: 320
    height: 142
  - file: 2026-09-14-dropping-ebpf-cpu-cost-by-about-90-with-memoization-not-ai.image-528d5cc380bf.webp
    width: 640
    height: 285
  - file: 2026-09-14-dropping-ebpf-cpu-cost-by-about-90-with-memoization-not-ai.image-f427fa8110e3.webp
    width: 1200
    height: 534
  color: '#edece7'
---

My brother and I spent a lot of time designing our eBPF security agent to be really fast from the ground up, but recently we discovered we could make it much faster using memoization!

A couple of weeks ago, I profiled the eBPF code and found that the most expensive part of the protection isn’t actually enforcing a policy (allow/deny), but figuring out which policy applies to a given file open.

Our policies are path based, so our eBPF leverages an LSM hook that triggers on file open. We then reconstruct the path, walk up parent dentries, and check whether the file or any ancestor directory has a matching policy. While this works, it isn’t performant, and we end up repeating much of the work for files we have already seen (for example, database accesses that repeatedly reaccess file paths).

So, we cache which policy applies for each inode. This dropped our kernel CPU cost by about 90%.

Additionally, we recently open sourced our repo, so everything in this blog post can be found at [https://github.com/bomfather/agent](https://github.com/bomfather/agent).

## The Problem

Before the cache, every file open would walk through the entire path. So the flow would look like this:

1. Get the file path.
2. Walk up the file path with dentries.
3. At each level, check whether a policy exists for the path.
4. Then combine the results to get a final policy, which we can use to decide whether to allow or deny.

This works, but if the same file is opened multiple times or multiple files in the same subtree are opened, we have to repeat these steps for each file.

For example: In Postgres, if we only want Postgres to be able to touch `/var/lib/postgres`, we can have this example policy:

```
policies:
  - executable: "filepath = /usr/lib/postgresql/16/bin/postgres"
    can_access_dirs:
      - "/var/lib/postgres:read"
```

Then Postgres retrieves files from `var/lib/postgres/data/base/123`, `var/lib/postgres/data/base/234`, and `var/lib/postgres/data/base/345`. We would have to walk the entire path of dentries for each of these file accesses, which is really inefficient.

For the rest of this blog post, I’ll call this inefficient path walk “the slow path.”

## What’s in the Cache?

Our solution is to use a cache. But we need to make sure the cache isn’t heavy and that it’s safe to reuse cached items.

We were thinking of using dentries, but dentries are pointers, and pointers can’t be stored inside eBPF maps. If we wanted to use dentries, we could store the dentries’ contents in a struct and use that struct as the map key, but it would be a pretty heavy struct.

So instead, we decided to use an `inode` based cache. Our cache key has three fields: the mount namespace ID, the mount ID, and the inode number.

We can’t cache the `inode` by itself because inode numbers are unique to a specific mount tree (so if a policy covers multiple mount trees, inodes could overlap). The mount ID helps us identify which mounted tree we observed the file through. The mount namespace ID also prevents us from using cached entries in a different namespace.

The cache value has two parts: an `access_index` and a cache state. We store our policies as bitmasks for space efficiency, and the `access_index` is the bit position for the path policy ([https://nathannaveen.dev/posts/optimizing-ebpf-policies-for-speed-and-space/)](https://nathannaveen.dev/posts/optimizing-ebpf-policies-for-speed-and-space/%29).

So, our cache, along with the keys and values, looks something like this:

```
#define INODE_POLICY_CACHE_NO_POLICY 0
#define INODE_POLICY_CACHE_ACCESS_INDEX 1
#define INODE_POLICY_CACHE_GLOBAL_READ_ONLY 2
#define INODE_POLICY_CACHE_ACCESS_INDEX_AND_GLOBAL_RO 3 

struct inode_cache_key {
    u64 mntns_id;
    u64 mount_id;
    u64 inode;
};

struct inode_policy_cache_value {
    u32 access_index;
    u8 state;
};

struct {
    __uint(type, BPF_MAP_TYPE_LRU_HASH);
    __uint(max_entries, 10000);
    __type(key, struct inode_cache_key);
    __type(value, struct inode_policy_cache_value);
} bomfather_inode_policy_cache SEC(".maps");
```

Now with the cache, our flow looks something like this:

1. We need to build the cache key.
2. We can look up the key in the LRU hash map.
3. If there is a hit, we can enforce the file open based on the cached result.
4. If there is a miss, we can do the slow path and store the result in the cache.

![On a cache hit, file open builds an inode cache key and goes straight to allow or deny. On a miss, it walks parent dentries, merges the policy, stores the result, then allows or denies.](https://nathannaveen.dev/img/BMBlogDiagram.jpeg)

## Performance Changes

In our benchmark tests, we opened the same file 200,000 times to analyze performance; the cache reduced kernel cycles from 28 billion to 3.03 billion. Without the cache, our `tail_call_security_check` appeared on the stack 89.2%, `is_restricted_filepath` 81.9%, and `path_check_callback` 63.7% of the time.

In the flamegraphs below, we can see that with the cache, the expense from path traversal pretty much disappears after the first lookup. For example, `is_restricted_filepath` and `path_check_callback` each shrink to roughly 0.02%, which is small enough to effectively disappear from the graph.

**Before (without cache):**

![Kernel flamegraph without the inode cache, with tail_call_security_check, is_restricted_filepath, and path_check_callback dominating the stack.](https://nathannaveen.dev/img/kernel-flamegraph-no-cache-annotated.svg)

**After (with cache):**

![Kernel flamegraph with the inode cache, where path traversal cost has mostly disappeared after the first lookup.](https://nathannaveen.dev/img/kernel-flamegraph-cache-annotated.svg)

We profiled the kernel CPU with `perf` using the `cycles:k` event. This measures kernel side CPU cost during file opens.

## Edge Cases

One thing we had to account for with this cache is that multiple paths can share a single inode. Hardlinks are the easiest example; with a hardlink, two different paths can share the same inode. This is a big problem because accurate results matter more than cache performance.

Our solution is more of a workaround than a real solution. Inodes have a link count (`i_nlink`) that tells us how many paths point to the inode; we can read it, and if it is greater than 1, we don’t use that cache entry and fall back to the slow path.

```
if (BPF_CORE_READ_INTO(&nlink, inode, i_nlink)) {
    return false;
}

if (nlink != 1) {
    inode_cache_stats_inc(INODE_CACHE_STATS_SKIPS_NLINK);
    return false;
}
```

This is a trade off since we are giving up some cache coverage, but I don’t think it is too big a deal because having an accurate cache is most important.

## Final Thoughts

In the end, this was a really fun thing to work on since I had to work through multiple different ideas for the cache until I landed on this.

I am also pretty happy the cache is entirely internal, so a user’s policy doesn’t need to change for the agent to speed up!
