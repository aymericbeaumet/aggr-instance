---
title: 'Redis latency spikes and the Linux kernel: a few more details'
link: http://antirez.com/news/84
source: antirez-com
published: 2014-11-03T15:58:19Z
updated: 2014-11-03T15:58:19Z
first_seen: 2026-09-19T21:30:23.395188495Z
summary: 'Today I was testing Redis latency using m3.medium EC2 instances. I was able to replicate the usual latency spikes during BGSAVE, when the process forks, and the child starts saving the dataset on disk. However something was not as expected. The spike did not happened because of disk I/O, nor during the fork() call itself. The test was performed with a 1GB of data in memory, with 150k writes per second originating from a different EC2 instance, targeting 5 million keys (evenly distributed). The pipeline was set to 4 commands. This translates to the following command line of redis-benchmark: ./redis-benchmark -P 4 -t set -r 5000000 -n 1000000000 Every time BGSAVE was triggered, I could see ~300 milliseconds latency spikes of unknown origin, since fork was taking 6 milliseconds. Fortunately Redis has a software watchdog feature, that is able to produce a stack trace of the process during a latency event. It’s quite a simple trick but works great: we setup a SIGALRM to be delivered by the kernel. Each time the serverCron() function is called, the scheduled signal is cleared, so actually Redis never receives it if the control returns fast enough to the Redis process. If instead there is a blocking condition, the signal is delivered by the kernel, and the signal handler prints the stack trace. Instead of getting stack traces with the fork call, the process was always blocked near MOV* operations happening in the context of the parent process just after the fork. I started to develop the theory that Linux was “lazy forking” in some way, and the actual heavy stuff was happening later when memory was accessed and pages had to be copy-on-write-ed. Next step was to read the fork() implementation of the Linux kernel. What the system call does is indeed to copy all the mapped regions (vm_area_struct structures). However a traditional implementation would also duplicate the PTEs at this point, and this was traditionally performed by copy_page_range(). However something changed… as an optimization years ago: now Linux does not just performs lazy page copying, as most modern kernels. The PTEs are also copied in a lazy way on faults. Here is the top comment of copy_range_range(): * Don''t copy ptes where a page fault will fill them correctly. * Fork becomes much lighter when there are big shared or private * readonly mappings. The tradeoff is that copy_page_range is more * efficient than faulting. Basically as soon as the parent process performs an access in the shared regions with the child process, during the page fault Linux does the big amount of work skipped by fork, and this is why I could see always a MOV instruction in the stack trace. While this behavior is not good for Redis, since to copy all the PTEs in a single operation is more efficient, it is much better for the traditional use case of fork() on POSIX systems, which is, fork()+exec*() in order to spawn a new process. This issue is not EC2 specific, however virtualized instances are slower at copying PTEs, so the problem is less noticeable with physical servers. However this is definitely not the full story. While I was testing this stuff in my Linux box, I remembered that using the libc malloc, instead of jemalloc, in certain conditions I was able to measure less latency spikes in the past. So I tried to check if there was some relation with that. Indeed compiling with MALLOC=libc I was not able to measure any latency in the physical server, while with jemalloc I could observe the same behavior observed with the EC2 instance. To understand better the difference I setup a test with 15 million keys and a larger pipeline in order to stress more the system and make more likely that page faults of all the mmaped regions could happen in a very small interval of time. Then I repeated the same test with jemalloc and libc malloc: bare metal, 675k/sec writes to 15 million keys, jemalloc: max spike 339 milliseconds. bare metal, 675k/sec writes to 15 million keys, malloc: max spike 21 milliseconds. I quickly tried to replicate the same result with EC2, same stuff, the spike was a fraction with malloc. The next logical thing after this findings is to inspect what is the difference in the memory layout of a Redis system running with libc malloc VS one running with jemalloc. The Linux proc filesystem is handy to investigate the process internals (in this case I used /proc//smaps file). Jemalloc memory is allocated in this region: 7f8002c00000-7f8062400000 rw-p 00000000 00:00 0 Size: 1564672 kB Rss: 1564672 kB Pss: 1564672 kB Shared_Clean: 0 kB Shared_Dirty: 0 kB Private_Clean: 0 kB Private_Dirty: 1564672 kB Referenced: 1564672 kB Anonymous: 1564672 kB AnonHugePages: 1564672 kB Swap: 0 kB KernelPageSize: 4 kB MMUPageSize: 4 kB Locked: 0 kB VmFlags: rd wr mr mw me ac sd While libc big region looks like this: 0082f000-8141c000 rw-p 00000000 00:00 0 [heap] Size: 2109364 kB Rss: 2109276 kB Pss: 2109276 kB Shared_Clean: 0 kB Shared_Dirty: 0 kB Private_Clean: 0 kB Private_Dirty: 2109276 kB Referenced: 2109276 kB Anonymous: 2109276 kB AnonHugePages: 0 kB Swap: 0 kB KernelPageSize: 4 kB MMUPageSize: 4 kB Locked: 0 kB VmFlags: rd wr mr mw me ac sd Looks like here there are a couple different things. 1) There is [heap] in the first line only for libc malloc. 2) AnonHugePages field is zero for libc malloc but is set to the size of the region in the case of jemalloc. Basically, the difference in latency appears to be due to the fact that malloc is using transparent huge pages, a kernel feature that allows to transparently glue multiple normal 4k pages into a few huge pages, which are 2048k each. This in turn means that copying the PTEs for this regions is much faster. EDIT: Unfortunately I just spotted that I''m totally wrong, the huge pages apparently are only used by jemalloc: I just mis-read the outputs since this seemed so obvious. So on the contrary, it appears that the high latency is due to the huge pages thing for some unknown reason. So actually it is malloc that, while NOT using huge pages, is going much faster. I''ve no idea about what is happening here, so please disregard the above conclusions. Meanwhile for low latency applications you may want to build Redis with “make MALLOC=libc”, however make sure to use “make distclean” before, and be aware that depending on the work load, libc malloc suffers fragmentation more than jemalloc. More news soon… EDIT2: Oh wait... since the problem is huge pages, this is MUCH better, since we can disable it. And I just verified that it works: echo never > /sys/kernel/mm/transparent_hugepage/enabled This is the new Redis mantra apparently. UPDATE: While this seemed unrealistic to me, I experimentally verified that the huge pages memory spike is due to the fact that with 50 clients writing at the same time, with N queued requests each, the Redis process can touch in the space of *a single event loop iteration* all the process pages, so its copy-on-writing the entire process address space. This means that not only huge pages are horrible for latency, but that are also horrible for memory usage. Comments'
content: extracted
html: 2014-11-03-redis-latency-spikes-and-the-linux-kernel-a-few-more-details.html
---

## [Redis latency spikes and the Linux kernel: a few more details](https://antirez.com/news/84)

[antirez](https://antirez.com/user/antirez) 4338 days ago. 326762 views.

```
Today I was testing Redis latency using m3.medium EC2 instances. I was able to replicate the usual latency spikes during BGSAVE, when the process forks, and the child starts saving the dataset on disk. However something was not as expected. The spike did not happened because of disk I/O, nor during the fork() call itself.

The test was performed with a 1GB of data in memory, with 150k writes per second originating from a different EC2 instance, targeting 5 million keys (evenly distributed). The pipeline was set to 4 commands. This translates to the following command line of redis-benchmark:

    ./redis-benchmark -P 4 -t set -r 5000000 -n 1000000000

Every time BGSAVE was triggered, I could see ~300 milliseconds latency spikes of unknown origin, since fork was taking 6 milliseconds. Fortunately Redis has a software watchdog feature, that is able to produce a stack trace of the process during a latency event. It’s quite a simple trick but works great: we setup a SIGALRM to be delivered by the kernel. Each time the serverCron() function is called, the scheduled signal is cleared, so actually Redis never receives it if the control returns fast enough to the Redis process. If instead there is a blocking condition, the signal is delivered by the kernel, and the signal handler prints the stack trace.

Instead of getting stack traces with the fork call, the process was always blocked near MOV* operations happening in the context of the parent process just after the fork. I started to develop the theory that Linux was “lazy forking” in some way, and the actual heavy stuff was happening later when memory was accessed and pages had to be copy-on-write-ed.

Next step was to read the fork() implementation of the Linux kernel. What the system call does is indeed to copy all the mapped regions (vm_area_struct structures). However a traditional implementation would also duplicate the PTEs at this point, and this was traditionally performed by copy_page_range(). However something changed… as an optimization years ago: now Linux does not just performs lazy page copying, as most modern kernels. The PTEs are also copied in a lazy way on faults. Here is the top comment of copy_range_range():

         * Don't copy ptes where a page fault will fill them correctly.
         * Fork becomes much lighter when there are big shared or private
         * readonly mappings. The tradeoff is that copy_page_range is more
         * efficient than faulting.

Basically as soon as the parent process performs an access in the shared regions with the child process, during the page fault Linux does the big amount of work skipped by fork, and this is why I could see always a MOV instruction in the stack trace.

While this behavior is not good for Redis, since to copy all the PTEs in a single operation is more efficient, it is much better for the traditional use case of fork() on POSIX systems, which is, fork()+exec*() in order to spawn a new process.

This issue is not EC2 specific, however virtualized instances are slower at copying PTEs, so the problem is less noticeable with physical servers.

However this is definitely not the full story. While I was testing this stuff in my Linux box, I remembered that using the libc malloc, instead of jemalloc, in  certain conditions I was able to measure less latency spikes in the past. So I tried to check if there was some relation with that.

Indeed compiling with MALLOC=libc I was not able to measure any latency in the physical server, while with jemalloc I could observe the same behavior observed with the EC2 instance. To understand better the difference I setup a test with 15 million keys and a larger pipeline in order to stress more the system and make more likely that page faults of all the mmaped regions could happen in a very small interval of time. Then I repeated the same test with jemalloc and libc malloc:

bare metal, 675k/sec writes to 15 million keys, jemalloc: max spike 339 milliseconds.
bare metal, 675k/sec writes to 15 million keys, malloc: max spike 21 milliseconds.

I quickly tried to replicate the same result with EC2, same stuff, the spike was a fraction with malloc.

The next logical thing after this findings is to inspect what is the difference in the memory layout of a Redis system running with libc malloc VS one running with jemalloc. The Linux proc filesystem is handy to investigate the process internals (in this case I used /proc/<pid>/smaps file).

Jemalloc memory is allocated in this region:

7f8002c00000-7f8062400000 rw-p 00000000 00:00 0
Size:            1564672 kB
Rss:             1564672 kB
Pss:             1564672 kB
Shared_Clean:          0 kB
Shared_Dirty:          0 kB
Private_Clean:         0 kB
Private_Dirty:   1564672 kB
Referenced:      1564672 kB
Anonymous:       1564672 kB
AnonHugePages:   1564672 kB
Swap:                  0 kB
KernelPageSize:        4 kB
MMUPageSize:           4 kB
Locked:                0 kB
VmFlags: rd wr mr mw me ac sd

While libc big region looks like this:

0082f000-8141c000 rw-p 00000000 00:00 0                                  [heap]
Size:            2109364 kB
Rss:             2109276 kB
Pss:             2109276 kB
Shared_Clean:          0 kB
Shared_Dirty:          0 kB
Private_Clean:         0 kB
Private_Dirty:   2109276 kB
Referenced:      2109276 kB
Anonymous:       2109276 kB
AnonHugePages:         0 kB
Swap:                  0 kB
KernelPageSize:        4 kB
MMUPageSize:           4 kB
Locked:                0 kB
VmFlags: rd wr mr mw me ac sd

Looks like here there are a couple different things.

1) There is [heap] in the first line only for libc malloc.
2) AnonHugePages field is zero for libc malloc but is set to the size of the region in the case of jemalloc.

<this is wrong>
Basically, the difference in latency appears to be due to the fact that malloc is using transparent huge pages, a kernel feature that allows to transparently glue multiple normal 4k pages into a few huge pages, which are 2048k each. This in turn means that copying the PTEs for this regions is much faster.
</this is wrong>

EDIT: Unfortunately I just spotted that I'm totally wrong, the huge pages apparently are only used by jemalloc: I just mis-read the outputs since this seemed so obvious. So on the contrary, it appears that the high latency is due to the huge pages thing for some unknown reason. So actually it is malloc that, while NOT using huge pages, is going much faster. I've no idea about what is happening here, so please disregard the above conclusions.

<wrong advice, read later EDIT2>
Meanwhile for low latency applications you may want to build Redis with “make MALLOC=libc”, however make sure to use “make distclean” before, and be aware that depending on the work load, libc malloc suffers fragmentation more than jemalloc.
</wrong>

More news soon…

EDIT2: Oh wait... since the problem is huge pages, this is MUCH better, since we can disable it. And I just verified that it works:

echo never > /sys/kernel/mm/transparent_hugepage/enabled

This is the new Redis mantra apparently.

UPDATE: While this seemed unrealistic to me, I experimentally verified that the huge pages memory spike is due to the fact that with 50 clients writing at the same time, with N queued requests each, the Redis process can touch in the space of *a single event loop iteration* all the process pages, so its copy-on-writing the entire process address space. This means that not only huge pages are horrible for latency, but that are also horrible for memory usage.
```

[blog comments powered by Disqus](https://disqus.com)
