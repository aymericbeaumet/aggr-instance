---
title: It's the memory latency, stupid
link: https://pivotlake.io/blog/its-the-memory-latency-stupid/
source: hnrss-org
published: 2026-10-31T18:38:16Z
updated: 2026-10-31T18:38:16Z
first_seen: 2026-11-01T02:06:10.004030692Z
authors:
- ntur1337
content: extracted
html: 2026-10-31-it-s-the-memory-latency-stupid.html
remote_preview:
  url: https://pivotlake.io/assets/pivot-logo.png
  alt: pivot
---

Making GROUP BY faster by doing more (CPU!) work

When you see your databases CPU at 100%, what is it *actually* doing? We like to act as though CPUs run code linearly for simplicity's sake, but in truth CPUs are incredibly complex beasts; they can potentially do many instructions at once, or sometimes none at all (despite our console showing 100%). Because CPUs can do so many things, one CPU at 100% may be doing much less than another at 100%. A CPU is considered "idle" only if no task (Linux task) needs CPU time; this does not mean the CPU itself is being fully saturated. So why would a CPU not be fully utilized, even if it appears to be at 100%?

The answer is usually **memory**.

Memory accesses are orders of magnitude slower than internal CPU operations. To give a good picture of how much so, a modern CPU might be able to process somewhere in the realm of 4-8 instructions per nanosecond. A memory access to RAM can take somewhere in the realm of 50-100ns. To ease your mathing, that means a modern CPU can do around 400-800 instructions for every *one* memory access from RAM.

CPU

CPU core 4–8 instr / ns

L1 cache \~1 ns

\~64 KB · ~4–8 instructions per access

L2 cache \~3–4 ns

\~1 MB · ~15–30 instructions per access

L3 cache \~10–15 ns

up to a few hundred MB · ~50–100 instructions per access

DRAM \~50–100 ns

GBs · ~400–800 instructions per access

Given this, CPUs are built from the ground up to work their way around this. The most obvious way this is dealt with is CPU caches. CPU caches keep memory closer to the CPU than RAM, literally baked into the CPU chip. The closest cache (the L1 cache) takes around 1ns per memory operation- that's 50-100x faster than RAM! The "problem" here of course is that the CPU caches are fairly small; the L1 cache for a modern CPU is probably around ~64KB, while the farther caches max out at a few hundred MB for top end processors. A good database tries manipulating data in small "chunks" to take full advantage of caches, and you'll often hear cache utilization as being one of the top concerns of performance engineering.

Let's look at GROUP BY aggregations to get a better grip on cache utilization. In GROUP BY, we're looking to take a dataset, group it by a certain column (or more), and perform certain aggregations (for example, count, or sum on another column). The "naive" method might be to open a HashTable keyed by the group key that grows ever larger. For every new row, you would check if that key exists- if it does, go to it's corresponding aggregation and update the result. Simple, right? The problem is that if you have millions of unique groups, you're not utilizing your cache at all! Nearly every HashTable access will be a "cold" hit that goes to RAM. This means that although your CPU may /look/ like it's 100% utilized, in truth it'll constantly be waiting for cold memory accesses.

To demonstrate this, we'll take our state-of-the-art Pivot engine and ravage it! We'll remove all our complex logic and replace it with simplicity:

```rust
fn consume(
    &mut self,
    batch: RecordBatch,
    _sender: &mut dyn Sender<RecordBatch>,
) -> unary::Result<()> {
    let group_keys = batch.column(self.key_column).as_primitive::<Int64Type>();
    for &key in group_keys.values() {
        // If the Group Key doesn't exist- set it to 0
        // Afterwards, add 1 to the group- this will give us a count(*) GROUP BY key_column
        *self.counts.entry(key).or_insert(0) += 1;
    }
}
```

We'll then run a simple query `SELECT count(*), UserID FROM hits GROUP BY UserID` on the "hits" dataset ClickBench provides (99m rows). This query will give us a count per `UserID`, meaning we'll have an entry in our HashMap per `UserID`\- I'll give a spoiler here that there are ~17m unique `UserID`.\
 Our performance checks will be on a bare metal server from our good friends over at CherryServers (AMD EPYC 9124 Genoa, 16 cores, 128 GB DDR5). We unfortunately can't use a common AWS/GCP machine because we want access to counters that aren't exposed there for security reasons (for example, we want to see how much memory is passing from DRAM controller to us). We'll confine ourselves to a single core, since this naive group by can't run on many cores.

To run it:

```shell
cat > config.yaml <<'EOF'
server:
  bind: 127.0.0.1:5440
workers: 1
datastores:
  default:
    kind: pivotlake
    location: ./data
    default: true
users:
  pivot:
    auth:
      method: trust
EOF
taskset -c 3 pivot server --config config.yaml &
taskset -c 7 psql -h 127.0.0.1 -p 5440 -U pivot -c '\timing on' -c "SELECT count(*) FROM (SELECT COUNT(*), UserID FROM hits GROUP BY UserID)"
```

This takes... a whopping 3.9 seconds. Is that good? Is that bad? Everything in this world is relative, or so they say. If we run the query again (without restarting the server), it takes 3.6s. The second run is the one we're more interested in- there's no disk involved as everything is already sitting in our page-cache.

You'll notice we did `taskset -c`. This pins the process to a given CPU; this will allow us to run `perf stat` on that CPU and see what's going on internally. We'll measure two metric groups- percentage of CPU slots stalling because of memory (and not doing anything in the meanwhile), and percentage of CPU slots actually retiring a micro-op:

```shell
sudo perf stat -C 3 -M backend_bound_memory,retiring -- taskset -c 7 psql -h 127.0.0.1 -p 5440 -U pivot -c "SELECT count(*) FROM (SELECT COUNT(*), UserID FROM hits GROUP BY UserID)"
```

and we get back this

```
Performance counter stats for 'CPU(s) 3':

        8364109137      ex_no_retire.load_not_complete   #     50.1 %  backend_bound_memory
       38233214040      de_no_dispatch_per_slot.backend_stalls
        8977647849      ex_no_retire.not_complete
       11861387798      ls_not_halted_cyc
       11431618219      ex_ret_ops                       #     16.1 %  retiring
       11858248045      ls_not_halted_cyc

       4.030148989 seconds time elapsed
```

This tells us that 50.1% of the time, the CPU couldn't do anything because it was stalling on memory! And only 16.1% of the time the CPU was actually retiring (aka doing) operations.

Another interesting stat to get is how many of our CPU memory loads are coming from DRAM (and not from caches):

```shell
sudo perf stat -C 3   -e ls_dmnd_fills_from_sys.dram_io_near   -- taskset -c 7 psql -h 127.0.0.1 -p 5440 -U pivot -c "SELECT count(*) FROM (SELECT COUNT(*), UserID FROM hits GROUP BY UserID)"
```

And the result:

```
 Performance counter stats for 'CPU(s) 3':

          76190837      ls_dmnd_fills_from_sys.dram_io_near

       3.996301634 seconds time elapsed
```

Every load is 64 bytes (the minimum amount you can bring, a cache line), so we can multiply by 64 to see we pulled 4.5GB during this query from DRAM on CPU loads (this is without any prefetching).

Let's now try to improve our algorithm to take advantage of the CPU caches. To do this, we'll use a method called radix partitioning. The idea is to split the merge into two distinct phases:

**Phase 1:** In this phase, we're simply going to separate the keys into 4096 different partitions (simply a Vec of SlabVecs- a SlabVec is a data structure similar to a Vec that is above memory that is already paged in. We don't use Vecs for raw memory in Pivot because it creates a lot of variance; not something for this blog post!). The partition will be decided by hashing the key and taking it's top 12 bits (2\*\*12=4096).

```rust
fn consume(
    &mut self,
    batch: RecordBatch,
    _sender: &mut dyn Sender<RecordBatch>,
) -> unary::Result<()> {
    let keys = batch.column(self.key_column).as_primitive::<Int64Type>();
    for &key in keys.values() {
        let hash = self.hasher.hash_one(key);
        // Since we want top 12 bits, we shift the hash (a u64) 52 bits to the right
        let partition = (hash >> 52) as usize;
        self.partitions[partition].push(&mut self.allocator, (hash, key));
    }
    Ok(())
}
```

| UserID | hash(UserID)    |
| ------ | --------------- |
| 7      | 0x**002**A91F3… |
| 42     | 0x**000**7C4E2… |
| 13     | 0x**FFF**3B05D… |
| 7      | 0x**002**A91F3… |
| 5      | 0x**001**D6A88… |
| 42     | 0x**000**7C4E2… |
| 99     | 0x**002**E17B4… |
| 7      | 0x**002**A91F3… |

→

partition = `hash >> 52`, the first 3 hex digits

**Partition 0** 0x000000… –\
0x000FFF…

42 42

**Partition 1** 0x001000… –\
0x001FFF…

5

**Partition 2** 0x002000… –\
0x002FFF…

7 7 99 7

…

**Partition 4095** 0xFFF000… –\
0xFFFFFF…

13

**Phase 2:** When all data has come through, go partition by partition, each time creating a HashMap, aggregating the partition into it, and then outputting it. This phase is very similar to our original algorithm, but we're going to use the hash from phase 1.

```rust
fn into_outputter(self) -> unary::Result<Option<Self::Outputter>> {
    let mut groups = Vec::new();
    for rows in &self.partitions {
        let mut table: HashTable<(u64, i64, u64)> = HashTable::new();   // (hash, key, count)
        rows.for_each(|(hash, key)| {
            match table.entry(hash, |group| group.1 == key, |group| group.0) {
                Entry::Occupied(mut group) => group.get_mut().2 += 1,
                Entry::Vacant(slot) => { slot.insert((hash, key, 1)); }
            }
        });
        groups.extend(table.into_iter().map(|(_, key, count)| (key, count)));
    }
    Ok(Some(Outputter::new(groups)))
}
```

Partitions, one at a time

0–1 done 2 in progress 3–4095 waiting

Partition 2

HashTable

Output

7 7 99 7

→

UserID count

7 3

99 1

→

(UserID, count)

(7, 3)

(99, 1)

At first glance, this solution may seem rather odd. We're doing **more** operations here, not less! Every row, in both solutions, is hashed and sent into a HashMap. But in this solution, we're first pushing rows into seemingly meaningless vecs.

The key here (no pun intended) is to understand why we've done this; it's to make the aggregating HashMaps **smaller**. By dividing our group by into 4096 pieces, we've made our working HashMaps much likelier to fit into our CPU caches. Every time we do a lookup on our HashMap, our CPU is more likely to find the relevant memory sitting in it's caches, which saves a big chunk of time. But why trust me? Let's run it!

```shell
taskset -c 7 psql -h 127.0.0.1 -p 5440 -U pivot -c '\timing on' -c "SELECT count(*) FROM (SELECT COUNT(*), UserID FROM hits GROUP BY UserID)"
```

And... Boom! 3.3 seconds, and then on second run 2.7 seconds! We've definitely made an improvement. But to really drive this point home, let's run perf again:

```shell
sudo perf stat -C 3 -M backend_bound_memory,retiring -- taskset -c 7 psql -h 127.0.0.1 -p 5440 -U pivot -c "SELECT count(*) FROM (SELECT COUNT(*), UserID FROM hits GROUP BY UserID)"
```

And our results:

```
 Performance counter stats for 'CPU(s) 3':

        3790955223      ex_no_retire.load_not_complete   #     23.5 %  backend_bound_memory
       20376118172      de_no_dispatch_per_slot.backend_stalls
        5442424751      ex_no_retire.not_complete
       10083617764      ls_not_halted_cyc
       19350459182      ex_ret_ops                       #     32.2 %  retiring
       10023435553      ls_not_halted_cyc

       3.575992317 seconds time elapsed
```

Now we can see the picture has completely changed- 23.5% of the time is spent on waiting for memory (as opposed to 50% before), 32% for actually retiring operations. You may notice we're actually doing **more** work, not less- in this run, we did 19350459182 operations (the `ex_ret_ops` counter), whereas before the database did 11431618219 operations. This is worth it though, because as we said before- if we can translate cold DRAM accesses into CPU instructions, it's nearly always a good trade to make. In this case, we went down from 8364109137 CPU slots stalled because of memory to 3790955223.

To complete the picture and really see we're pulling less memory from the DRAM, let's again check how many our memory loads come from DRAM:

```shell
sudo perf stat -C 3   -e ls_dmnd_fills_from_sys.dram_io_near   -- taskset -c 7 psql -h 127.0.0.1 -p 5440 -U pivot -c "SELECT count(*) FROM (SELECT COUNT(*), UserID FROM hits GROUP BY UserID)"
```

```
 Performance counter stats for 'CPU(s) 3':

          25688280      ls_dmnd_fills_from_sys.dram_io_near

       3.009547012 seconds time elapsed
```

Whoo! That means that on demand we pulled 1.5GB from DRAM, instead of 4.5GB in the naive version.

That, of course, isn't the limit of how much we can utilize our CPU cache for GROUP BYs. And utilizing our CPU cache isn't the only way we can "hide" DRAM latency; another trick is prefetching memory to our cache before we need it, and yet another trick is creating algorithms that allow maximization of memory throughput- but that's not for this blog! We'll go into that in our next blog where we look at our /seemingly very weird/ implementation of JOIN probing.

For now, without explaining too much, I'll leave you with Pivot's speed for this query on a single core:

```
Time: 1848.353 ms (00:01.848)
```

and the amount of RAM we bring:

```
 Performance counter stats for 'CPU(s) 3':

          14917958      ls_dmnd_fills_from_sys.dram_io_near

       2.182674864 seconds time elapsed
```

Which comes out to 954 MB.

Of course, Pivot knows to run in parallel on our 16 cores (where there are many other interesting cases), in which case it comes out to:

```
Time: 171.773 ms
```

nice :)

Try out Pivot if you want to have super speeds above parquets (OSS fun!), see you next time.
