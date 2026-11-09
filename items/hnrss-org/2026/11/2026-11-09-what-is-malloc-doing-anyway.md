---
title: What is malloc doing, anyway?
link: https://benmandrew.com/articles/what-is-malloc-doing-anyway
source: hnrss-org
published: 2026-11-09T11:31:05Z
updated: 2026-11-09T11:31:05Z
first_seen: 2026-11-09T21:00:57.230935818Z
authors:
- benmandrew
content: extracted
html: 2026-11-09-what-is-malloc-doing-anyway.html
remote_preview:
  url: https://benmandrew.com/assets/headers/malloc-card.png
---

`malloc` has a very nice and simple interface: you ask for a certain amount of memory, and you’re given a `void` pointer to your block of contiguous bytes that’s been magicked into existence. Once you’re done with it, you call `free` with the same pointer and the memory disappears into the ether.

But what’s actually happening here? Obviously, as we’re running as a userspace program, `malloc` must be asking the kernel for the memory through *syscalls*. However, we may be allocating many small blocks, and syscalls are expensive (575 ns for an empty one, as measured below), so some bookkeeping is done in the background. We’ll demonstrate this on GNU `libc` (`glibc`) with the program snippet below, which calls `malloc(16)` a thousand times.

```c
static void *ptrs[1000];

int main(void) {
    for (int i = 0; i < 1000; i++)
        ptrs[i] = malloc(16);
}
```

Using `strace`, we can see that the whole loop makes only two `brk` syscalls: the first asks where the *program break* is (the address where the process’s heap ends), and the second grows the heap by 132 KiB. These are both done in the first `malloc` call — the other 999 just take from that already allocated memory.

```console
$ strace -e trace=brk,mmap,munmap,write \
    ./malloc1000 > /dev/null
...
brk(NULL)              = 0x59faca231000
brk(0x59faca252000)    = 0x59faca252000
+++ exited with 0 +++
```

You may see both `brk` and `sbrk` mentioned in discussions. `brk` is a syscall that sets the program break to the address it is provided, and `sbrk` is a `libc` convenience wrapper that takes a *delta*, increasing the program break by that amount.

## The Address Space

A running process gets its own *address space*, split into a number of regions. These include `text` for executable code, `data` for initialised global variables, and `bss` for uninitialised global variables, among others. The following program prints one address from each region of the process.

```c
#define SHOW(label, p) \
    printf("%-22s %p\n", label, (void *)(p))

SHOW("main (text)", main);
SHOW("initialised (data)", &initialised);
SHOW("uninitialised (bss)", &uninitialised);
SHOW("malloc(16) (heap)", malloc(16));
SHOW("program break", sbrk(0));
SHOW("printf (libc)", printf);
SHOW("local (stack)", &local);
```

The addresses rise from the binary’s code up to its stack.

```console
$ ./layout
main (text)            0x59c0791d7149
initialised (data)     0x59c0791da020
uninitialised (bss)    0x59c0791da028
malloc(16) (heap)      0x59c0903182b0
program break          0x59c090338000
printf (libc)          0x711dcf7c15b0
local (stack)          0x7ffff86c45ec
```

Some of these regions can change as the program runs. For example, `malloc(16)` grows the heap and moves the break up. Note that in this run, the break and `printf` in `libc` are 23.4 TiB apart, about 1,530 times the machine’s 15.6 GiB of RAM. Nearly all of that range is unmapped, so it costs no physical memory.

## A Toy Allocator with `sbrk`

Because we can move the program break directly, we can write a little allocator of our own. This is called a *bump* allocator, because each call simply bumps up the amount of memory that we are allowed to use.

```c
void *my_malloc(size_t size) {
    void *p = sbrk(align_up(size));
    return p == (void *)-1 ? NULL : p;
}

void my_free(void *ptr) {
    (void)ptr;
}
```

This allocator is not very good! It moves the break on every single request, and each move is a syscall. We can time a syscall against `malloc` to see why `malloc` requests large blocks from the kernel and cuts them up itself. Each of the following three functions is timed over a million calls.

```c
static void *volatile sink;

static void call_getppid(void) {
    syscall(SYS_getppid);
}

static void call_sbrk(void) {
    sink = sbrk(16);
}

static void call_malloc(void) {
    sink = malloc(16);
}
```

`sbrk(16)` costs only 35 ns more than the empty syscall, whereas `malloc(16)` runs 22.7 times faster than `sbrk(16)`.

```console
$ ./syscall-cost
syscall(SYS_getppid)      575.2 ns per call
sbrk(16)                  610.1 ns per call
malloc(16)                 26.9 ns per call
sbrk(16) / malloc(16)      22.7x
```

The bump allocator also has no way to reuse freed memory. Each request lands straight after the one before, so in the following example, `d` goes after `c` even though `b` has been freed.

```console
$ ./my-bump-1
a = my_malloc(16)   start+0
b = my_malloc(1)    start+16
c = my_malloc(100)  start+32
my_free(b)
d = my_malloc(1)    start+144
break               start+160
```

The rest of the post improves the allocator one stage at a time.

## Chunks and Headers

`malloc` actually allocates more memory than you request. This is because each allocation, or *chunk*, has a size header just before the returned pointer; this is how `free` works without passing an explicit size. Chunk sizes are always multiples of 16, so the low bits of the size are always zero, and `glibc` keeps three flags in them.

```c
size_t requests[] = {1, 24, 25, 1000};

size_t count = sizeof requests / sizeof requests[0];
for (size_t i = 0; i < count; i++) {
    char *p = malloc(requests[i]);
    size_t chunk = ((size_t *)p)[-1] & ~(size_t)7;
    printf("%8zu %8zu\n", requests[i], chunk);
}
```

Each chunk is the request plus 8 bytes for the header, rounded up to a multiple of 16, with a minimum of 32. The rounding keeps every pointer that `malloc` returns 16-byte aligned, which is the alignment x86-64 gives `max_align_t`, so the memory suits any C type. The 32-byte minimum leaves room for the list pointers that a chunk stores once it is freed.

```console
$ ./chunk-header
 request    chunk
       1       32
      24       32
      25       48
    1000     1008
```

When we free a chunk, it is put into what’s called a *free list* to allow reuse. When more data is requested, `malloc` looks through this free list and splits large chunks to fit small requests, and `free` merges free neighbours back together. Each chunk in the free list needs two pieces of information: its size (which we already have), and a pointer to the next chunk in the free list. `glibc` stores the pointer after the size, reusing the memory that the user stored their data in before it was freed!

The toy allocator stores the same two fields in each chunk.

```c
typedef struct chunk {
    size_t size;
    struct chunk *next;
} chunk_t;
```

## Splitting and Coalescing

We can improve our `my_malloc` by rounding the request plus its header up to a multiple of 16 bytes, and taking the first chunk in the free list that is big enough. To keep things simple, the toy’s header is the whole `chunk_t`, so a chunk keeps room for `next` even while it is in use. That makes the header 16 bytes where `glibc`’s is 8, and `my_malloc(100)` takes a 128-byte chunk where `malloc(100)` takes 112.

```c
void *my_malloc(size_t size) {
    size_t need = align_up(size + HEADER_SIZE, ALIGN);
    for (chunk_t **link = &free_list; *link != NULL;
         link = &(*link)->next)
        if ((*link)->size >= need)
            return take(link, need);
    if (grow_heap(need) != 0)
        return NULL;
    return my_malloc(size);
}
```

We define `take` to split that chunk, leaving whatever the request does not need in the free list.

```c
static void *take(chunk_t **link, size_t need) {
    chunk_t *c = *link;
    if (c->size > need) {
        chunk_t *rest = (chunk_t *)((char *)c + need);
        rest->size = c->size - need;
        rest->next = c->next;
        c->size = need;
        *link = rest;
    } else {
        *link = c->next;
    }
    return (char *)c + HEADER_SIZE;
}
```

When no free chunk fits, `grow_heap` moves the program break up in 64 KiB steps using `sbrk` and puts the new space on the free list as one chunk.

With headers, we can now actually implement a proper `my_free` function! It steps back from the pointer to the header and hands the chunk `c` to `insert_free`. That function walks the list to find the free chunks either side of `c` by address, `prev` and `next`, and links `c` in between them. Then, if `c` ends where `next` starts, or starts where `prev` ends, the two become one chunk.

```c
if (next != NULL && (char *)c + c->size == (char *)next) {
    c->size += next->size;
    c->next = next->next;
}
if (prev != NULL && (char *)prev + prev->size == (char *)c) {
    prev->size += c->size;
    prev->next = c->next;
}
```

A test program allocates `a`, `b` and `c`, frees `a` and `b`, allocates `d`, then frees the rest.

Each line shows the chunk that the call allocated, if any, and then the free list. Every chunk is an `[offset, size]` pair counted from the start of the heap, and the offset is where its header starts, 16 bytes before the pointer that `my_malloc` returns.

```console
$ ./my-freelist-2
a = my_malloc(100)   [0, 128]     free: [128, 65408]
b = my_malloc(200)   [128, 224]   free: [352, 65184]
c = my_malloc(100)   [352, 128]   free: [480, 65056]
my_free(a)                        free: [0, 128] [480, 65056]
my_free(b)                        free: [0, 352] [480, 65056]
d = my_malloc(50)    [0, 80]      free: [80, 272] [480, 65056]
my_free(c)                        free: [80, 65456]
my_free(d)                        free: [0, 65536]
```

`d` gets the chunk at offset 0, split from the 352-byte chunk that `a` and `b` merged into. The bump allocator put its fourth request after everything else. Freeing `c` merges three chunks into one, and freeing `d` leaves the single 64 KiB chunk that `grow_heap` added.

## Demand Paging

`my_free` reuses memory, but it never returns any to the kernel. Whether a free chunk still costs RAM depends on when the kernel puts physical memory behind an address. Each process sees *virtual memory*, which the kernel maps onto physical RAM in 4 KiB *pages*. That separation allows the kernel to hand out address ranges with no physical RAM behind them — a virtual page only needs to be backed by physical memory on its *first write*.

The following program moves the break up by 1 GiB, then writes one byte to every page and prints a row after each 256 MiB.

```c
char *p = sbrk(SIZE);
print_row(0);
for (size_t offset = 0; offset < SIZE; offset += PAGE_SIZE) {
    p[offset] = 1;
    if ((offset + PAGE_SIZE) % (SIZE / 4) == 0)
        print_row((long)((offset + PAGE_SIZE) >> 20));
}
```

Virtual size counts the addresses the process has mapped, and *resident set size* (RSS) counts the pages that are in RAM. Straight after `sbrk`, virtual size is already just over 1 GiB, while RSS is 1308 KiB. Each 256 MiB written then costs 65,536 page faults, one per 4 KiB page, increasing the RSS.

```console
$ ./demand-paging
   written     faults        virtual       resident
     0 MiB          3    1050916 KiB       1308 KiB
   256 MiB      65539    1050916 KiB     263536 KiB
   512 MiB     131075    1050916 KiB     525680 KiB
   768 MiB     196611    1050916 KiB     787824 KiB
  1024 MiB     262147    1050916 KiB    1049968 KiB
```

Once a chunk’s pages have been written, they stay resident after `my_free`, until the allocator gives them back to the kernel.

## Giving Memory Back

We give memory back to the kernel by *lowering* the break. We introduce a new function, `trim`, that lowers the break once the free chunk at the top of the heap reaches 128 KiB, keeping 64 KiB so that the next few requests don’t move the break straight back up.

```c
static void trim(void) {
    chunk_t *top = free_list;
    if (top == NULL)
        return;
    while (top->next != NULL)
        top = top->next;
    char *end = (char *)top + top->size;
    if (end != (char *)sbrk(0) || top->size < TRIM_THRESHOLD)
        return;
    size_t release =
        (top->size - TOP_PAD) / PAGE_SIZE * PAGE_SIZE;
    top->size -= release;
    sbrk(-(intptr_t)release);
}
```

A test program allocates 1 MiB and then a 16-byte chunk above it, then writes to every page of the 1 MiB block. (*Anonymous RSS* counts resident memory with no backing file, which keeps `libc`’s code out of the numbers.)

```console
$ ./my-trim-3
                                 break         anonymous RSS
my_malloc(1 MiB), my_malloc(16)  start+1088 KiB     1124 KiB
my_free(1 MiB)                   start+1088 KiB     1124 KiB
my_free(16)                      start+64 KiB        160 KiB
```

Freeing the 1 MiB block leaves the break at 1088 KiB and anonymous RSS at 1124 KiB, because the 16-byte chunk above it is still live. Once that chunk goes too, `trim` lowers the break to 64 KiB, dropping the anonymous RSS to 160 KiB.

Only the top of the heap can be given back. A live chunk holds the break above it, so every free chunk below it stays resident, as the 1 MiB block did here. Over a long run, many allocations and deallocations leave free gaps between live chunks that later requests rarely fill, which is called *fragmentation*. These gaps stay resident, so in a long-running program, fragmentation can grow RSS until a memory limit kills the process.

## Large Blocks with `mmap`

The `mmap` syscall usually maps a file into the address space. With the `MAP_ANONYMOUS` flag there is no backing file, and the kernel creates a zero-filled, page-aligned mapping anywhere in the address space. Like the heap, its resident pages count towards the anonymous RSS. The `munmap` syscall returns a block with its own mapping to the kernel as soon as it is freed, whatever lies next to it.

To integrate this into our own allocator, `my_malloc` chooses between `sbrk` and `mmap` by the size of the request. Any request of 128 KiB or more, header included, gets a mapping of its own from `map_chunk`, rounded up to whole pages; smaller requests take the existing `sbrk` free list path.

```c
static void *map_chunk(size_t need) {
    size_t len = align_up(need, PAGE_SIZE);
    chunk_t *c = mmap(NULL, len, PROT_READ | PROT_WRITE,
                      MAP_PRIVATE | MAP_ANONYMOUS, -1, 0);
    if (c == MAP_FAILED)
        return NULL;
    c->size = len | MMAPPED; // chunk is mapped
    return (char *)c + HEADER_SIZE;
}
```

For `mmap`ped chunks, we use one of the three flags in the size header (M) to record that this chunk has its own mapping. `my_free` checks this bit to know how to free it. It unmaps a mapped chunk with `munmap`, and puts any other chunk back on the free list, where `trim` can release it.

```c
void my_free(void *ptr) {
    if (ptr == NULL)
        return;
    chunk_t *c = (chunk_t *)((char *)ptr - HEADER_SIZE);
    if (c->size & MMAPPED) {
        munmap(c, c->size & ~(size_t)MMAPPED);
        return;
    }
    insert_free(c);
    trim();
}
```

Running the previous demo again, we see that freeing the 1 MiB block drops the anonymous RSS while the 16-byte chunk is still live.

```console
$ ./my-mmap-4
                                 break         anonymous RSS
my_malloc(1 MiB), my_malloc(16)  start+64 KiB       1128 KiB
my_free(1 MiB)                   start+64 KiB        100 KiB
my_free(16)                      start+64 KiB        100 KiB
```

The following diagram shows how this might look in the address space.

Why not `mmap` everything? Some allocators do! Even `glibc` creates extra heaps, called *arenas*, with `mmap`, and spreads threads across them so that they don’t all contend for the lock on the main heap. However, separate mappings have separate regions with no guarantee of contiguity, and chunks can’t be coalesced across these gaps. On the other hand, allocating with `sbrk` just bumps up the one contiguous region that you already have. A freed chunk that stays on the heap keeps its pages resident, so reusing it causes no page faults. Each new mapping starts with no physical memory behind it, and faults on every page it writes.

## Comparing Performance

We compare the performance of our allocators against `glibc`’s `malloc` with a workload that repeatedly allocates and deallocates blocks of varying sizes[^1]. To measure the speed, we find the average time of a *step*: free a random live block, allocate a replacement of random size, and write to each of its pages. At the end of the run, the live blocks hold 10.3 MiB. The last two columns give the anonymous RSS at that point, and again after every block is freed, which is the memory the allocator keeps.

| Allocator       | Average time per step (µs) | Anonymous RSS, blocks live (MiB) | Anonymous RSS, all freed (MiB) |
| --------------- | -------------------------- | -------------------------------- | ------------------------------ |
| `my-bump-1`     | 1.72                       | 1425                             | 1425                           |
| `my-freelist-2` | 5.12                       | 17.8                             | 17.8                           |
| `my-trim-3`     | 11.2                       | 14.5                             | 0.188                          |
| `my-mmap-4`     | 11.8                       | 11.2                             | 0.188                          |
| `glibc`         | 0.0789                     | 19.0                             | 19.0                           |

`my-bump-1` never frees or reuses memory, so its RSS is the 1425 MiB that the run allocates in total. Its 1.72 µs per step comes from an `sbrk` call for every request and page faults on new pages. `my-freelist-2` reuses memory, so its RSS of 17.8 MiB is much closer to the 10.3 MiB of live data. However, each step walks the list twice: `my_malloc` walks it to find a fit, and `insert_free` walks it to find the chunk’s neighbours. `my-trim-3` adds a third walk, because `trim` looks for the last free chunk on every `my_free`. `my-freelist-2` and `my-trim-3` run 65 and 142 times slower than `glibc` respectively. In return, `my-trim-3` keeps only 0.188 MiB once every block is freed. `my-mmap-4` calls `mmap` for large requests, which raises its time per step to 11.8 µs but gives it the lowest RSS with blocks live, at 11.2 MiB.

Why is `glibc` so fast? The slowdowns in our allocators are due to having to walk a long free list of chunks multiple times. `glibc` instead keeps free chunks in *bins*, separate lists by size, so a request can walk a list of appropriately sized chunks directly. It also keeps up to seven freed chunks of each small size in a thread-local cache (tcache) and hands them straight back to the next request of that size.

The tcache also explains the last column, where `glibc` keeps 19.0 MiB after every block is freed. The heap counts a cached chunk as in use, so one near the top stops `glibc` from lowering the break. The `GLIBC_TUNABLES` environment variable can turn the tcache off with `GLIBC_TUNABLES=glibc.malloc.tcache_count=0`, or fix the `mmap` threshold at 128 KiB with `GLIBC_TUNABLES=glibc.malloc.mmap_threshold=131072`.

| Allocator                        | Average time per step (µs) | Anonymous RSS, blocks live (MiB) | Anonymous RSS, all freed (MiB) |
| -------------------------------- | -------------------------- | -------------------------------- | ------------------------------ |
| `glibc`                          | 0.0789                     | 19.0                             | 19.0                           |
| `glibc`, `tcache_count=0`        | 0.121                      | 17.4                             | 0.250                          |
| `glibc`, `mmap_threshold=131072` | 0.843                      | 12.1                             | 5.85                           |

With the tcache off, freeing everything leaves 0.250 MiB, and each step takes 1.5 times as long as before.

`glibc` makes 51 `mmap` calls in the whole run, against the 5040 that `my-mmap-4` makes. Freeing a mapped chunk raises `glibc`’s threshold to that chunk’s size, so later large requests come from the heap. With the threshold fixed at 128 KiB, every large request pays for its syscalls and page faults again, and `glibc` runs 10.7 times slower.

`malloc` spends memory to stay away from the kernel, parcelling out large blocks in userspace so that most calls never make a syscall.

## Links

- Reproducible snippets for each program ([link](https://github.com/benmandrew/malloc-snippets/tree/v1)).
- `glibc`’s own description of its allocator’s internals, covering chunks, bins, arenas and the tcache ([link](https://sourceware.org/glibc/wiki/MallocInternals)).
- The man pages for `brk(2)` and `sbrk(2)` ([link](https://man7.org/linux/man-pages/man2/brk.2.html)), `mmap(2)` ([link](https://man7.org/linux/man-pages/man2/mmap.2.html)), `mallopt(3)` ([link](https://man7.org/linux/man-pages/man3/mallopt.3.html)) and `malloc_trim(3)` ([link](https://man7.org/linux/man-pages/man3/malloc_trim.3.html)).

[^1]: The benchmark keeps 4000 blocks live, and each step frees a random block and allocates a replacement of a random size. Of the requests, 90% are 16 to 512 bytes, 9% are up to 16 KiB, and 1% are 128 KiB to 256 KiB. Each new block gets one write per page. An allocator runs 500,000 steps in a fresh process, built at `-O2`. Run on an Intel Xeon E3-1230 v3.
