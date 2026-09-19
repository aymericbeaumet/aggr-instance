---
title: My adventures hunting down a Ruby memory leak 🎢
link: https://dev.37signals.com/adventures-hunting-down-ruby-memory-leak/
source: dev-37signals-com
published: 2024-03-07T18:00:00Z
updated: 2024-03-07T18:00:00Z
first_seen: 2026-09-19T21:30:23.395188495Z
authors:
- Jacopo Beschi
summary: What I learned on the journey chasing a tricky memory leak in HEY.
content: extracted
html: 2024-03-07-my-adventures-hunting-down-a-ruby-memory-leak-roller-coaster.html
preview:
  file: 2024-03-07-my-adventures-hunting-down-a-ruby-memory-leak-roller-coaster.preview-e1537f18b4de.webp
  width: 256
  height: 134
  alt: 37signals Dev
  color: '#101010'
images:
- source: https://dev.37signals.com/assets/images/opengraph/adventures-hunting-down-ruby-memory-leak.png
  original:
    file: 2024-03-07-my-adventures-hunting-down-a-ruby-memory-leak-roller-coaster.image-7b940ec546fc.png
    width: 2400
    height: 1260
  color: '#000000'
extra:
  thumbnail: https://dev.37signals.com/assets/images/opengraph/adventures-hunting-down-ruby-memory-leak.png
---

In this article, I tell the story of a memory leak we had in our [HEY app](https://www.hey.com/), the cool tools I could use to investigate and how I finally figured out the root cause. Memory leaks can be tricky to diagnose and having the right set of tools makes a huge difference. I hope my adventures help you the next time you’re in a similar situation.

* * *

## The beginning

Everything started with a report from our Operations team mentioning that HEY memory usage was often getting close to boiling over. We rarely deploy HEY during the weekends, and that’s when the issue manifested itself the most:

After a few days without deploying the app, the memory usage almost reached 100%, and then a deployment would reset it back to normal. This signals a slow and steady memory leak.

* * *

## Checking Ruby heap allocations

The first approach I tried was logging and analyzing the Ruby Heap allocation stats. In particular, I looked for endpoints where:

1. A minor (or major) GC run happened.
2. `heap_available_slots` increased greatly.

The theory is that when GC runs, it should clear most of the newly allocated objects, hence an increase of `heap_available_slots` paired with a GC run shouldn’t happen unless there is a leak.

The analysis exposed 2 endpoints:

- `MessagesController#update`
- `TopicsController#show`

These are the most heavily used endpoints of the app, so the result wasn’t conclusive at all.

* * *

## Heap dump

I decided to proceed by extracting a Ruby heap dump. Luckily this can be done by running [rbtrace](https://github.com/tmm1/rbtrace) in the app servers:

```
bundle exec rbtrace -p $worker_pid -e 'Thread.new{GC.start; require "objspace"; File.open("/tmp/0.json","w"){|f| ObjectSpace.dump_all(output: f) }}'
```

This command writes the `$worker_pid` heap dump to `/tmp/0.json`.

Unfortunately, a heap dump extracted without enabling object trace allocation doesn’t contain key information such as:

- The GC generation it was allocated in
- The filename and line number it was allocated in
- A truncated value
- Object bytesize

So even the basic heap dump wasn’t enough to find the culprit.

### ObjectSpace.trace\_object\_allocations\_start

Enabling object trace allocation [greatly slows down the response times and increases memory usage](https://ruby-doc.org/stdlib-trunk/libdoc/objspace/rdoc/ObjectSpace.html#method-c-trace_object_allocations_start) but I had no more options left, so I still decided to proceed. To mitigate this performance degradation, I enabled object trace allocation **only on one app host** and only for the time needed to extract the heap dumps I needed. I also monitored the host during the process: If something went wrong, I could’ve immediately stopped the host traffic from the load balancer to mitigate it.

I managed to extract 3 heap dumps without any issues:

1. 0.json: ~15min after deploy
2. 1.json: ~40min after deploy
3. 2.json: ~1h after the deploy

It was time to analyze them!

### Analysis

The first analysis was via [heapy diff](https://github.com/zombocom/heapy?tab=readme-ov-file#diff-2-heap-dumps):

```
jacopo-37s-mb 3.3.0-preview2 ~/Desktop/Hey heap dumps/1-tracing heapy diff 0.json 1.json 2.json |head -n 10
Retained STRING 18947 objects of size 1290614/16768824 (in bytes) at: /usr/local/bundle/ruby/3.3.0/bundler/gems/okra-d3937f2a023c/lib/okra/html.rb:19
Retained OBJECT 14380 objects of size 1150400/16768824 (in bytes) at: /usr/local/bundle/ruby/3.3.0/bundler/gems/okra-d3937f2a023c/lib/okra/html.rb:19
Retained ARRAY 13588 objects of size 608192/16768824 (in bytes) at: /usr/local/bundle/ruby/3.3.0/bundler/gems/okra-d3937f2a023c/lib/okra/html.rb:19
Retained STRING 3634 objects of size 247720/16768824 (in bytes) at: /usr/local/bundle/ruby/3.3.0/gems/prometheus-client-mmap-1.0.0/lib/prometheus/client/histogram.rb:21
Retained STRING 2895 objects of size 939094/16768824 (in bytes) at: /usr/local/bundle/ruby/3.3.0/gems/json-2.6.3/lib/json/common.rb:312
```

Which outlined `lib/okra/html.rb:19`, and I thought this was the root cause; but after digging further, I wasn’t able to find any leaking code related. Luckily, the heap dumps extracted with object trace allocation enabled contained the object dependency tree, so I decided to take a look at it and figure out what was retaining `lib/okra/html.rb:19`; and for that, I used [sheap](https://github.com/jhawthorn/sheap).

Initially, I extracted from the heap dump a few addresses associated with `lib/okra/html.rb:19`:

```
jacopo-37s-mb 3.3.0 ~/Desktop/Hey heap dumps/1-tracing grep "file.*lib/okra/html.rb.*line.*19" 2.json | head |  grep -o "address\":\"[^\"]\+\""
address":"0x7fbfec2c0098"
address":"0x7fbfec2c0110"
address":"0x7fbfec2c0138"
address":"0x7fbfec2c0408"
address":"0x7fbfec2c0430"
address":"0x7fbfec2c0480"
address":"0x7fbfec2c04d0"
address":"0x7fbfec2c04f8"
address":"0x7fbfec2c0908"
address":"0x7fbfec2c0980"
```

Then, I checked their dependency tree with sheap, and found a common pattern:

```
jacopo-37s-mb 3.3.0 ~/Desktop/Hey heap dumps/1-tracing sheap 0.json 2.json
irb#1():004> $diff.after.find_path($diff.after.at("0x7fbfec2c0098"))
=>
[#<ROOT vm  (3194 refs)>,
 #<DATA 0x7fc02024b070 yjit_root (11268 refs)>,
 #<IMEMO 0x7fbfecc9b4a0 ment (4 refs)>,
 #<IMEMO 0x7fbffd0b2948 iseq (111 refs)>,
 #<OBJECT 0x7fbfbb8b53d0 (0x7fbff0a537c0) (8 refs)>,
 #<OBJECT 0x7fbfecd3e150 ActiveModel::AttributeMutationTracker (2 refs)>,
 #<OBJECT 0x7fc01ab32900 ActiveModel::LazyAttributeSet (6 refs)>,
 #<HASH 0x7fbff0f58b80  (4 refs)>,
 #<OBJECT 0x7fbff0f56ec0 ActionText::Content (1 refs)>,
 #<OBJECT 0x7fbfec64da08 ActionText::Fragment (1 refs)>,
 #<OBJECT 0x7fc007f95140 Okra::HTML::Node (1 refs)>,
 #<ARRAY 0x7fc01ab32040  (7 refs)>,
 #<OBJECT 0x7fc007f95190 Okra::HTML::Node (3 refs)>,
 #<ARRAY 0x7fbfc485d960  (2 refs)>,
 #<OBJECT 0x7fc007f95230 Okra::HTML::Node (3 refs)>,
 #<ARRAY 0x7fbfc485d870  (3 refs)>,
 #<OBJECT 0x7fc007f95410 Okra::HTML::Node (3 refs)>,
 #<ARRAY 0x7fbfbb8b5150  (21 refs)>,
 #<OBJECT 0x7fc007f95690 Okra::HTML::Node (3 refs)>,
 #<ARRAY 0x7fbfec37e4f8  (3 refs)>,
 #<OBJECT 0x7fc007f95780 Okra::HTML::Node (3 refs)>,
 #<ARRAY 0x7fbff09af5a8  (11 refs)>,
 #<OBJECT 0x7fc007f959b0 Okra::HTML::Node (3 refs)>,
 #<ARRAY 0x7fbff09ae9c8  (11 refs)>,
 #<OBJECT 0x7fc007f95e10 Okra::HTML::Node (3 refs)>,
 #<ARRAY 0x7fbfec378530  (3 refs)>,
 #<OBJECT 0x7fc007f96040 Okra::HTML::Node (3 refs)>,
 #<ARRAY 0x7fbfec378490  (3 refs)>,
 #<OBJECT 0x7fc007f96180 Okra::HTML::Node (3 refs)>,
 #<ARRAY 0x7fc0202ff520  (39 refs)>,
 #<OBJECT 0x7fc007f96310 Okra::HTML::Node (3 refs)>,
 #<ARRAY 0x7fbfec36acf0  (3 refs)>,
 #<OBJECT 0x7fc007f96400 Okra::HTML::Node (3 refs)>,
 #<ARRAY 0x7fbff09a4ba8  (11 refs)>,
 #<OBJECT 0x7fc007f96540 Okra::HTML::Node (3 refs)>,
 #<ARRAY 0x7fbfec331e50  (3 refs)>,
 #<OBJECT 0x7fc007f966d0 Okra::HTML::Node (3 refs)>,
 #<ARRAY 0x7fbfec331dd8  (3 refs)>,
 #<OBJECT 0x7fc007f96810 Okra::HTML::Node (3 refs)>,
 #<ARRAY 0x7fc0202fe120  (39 refs)>,
 #<OBJECT 0x7fc007f96a40 Okra::HTML::Node (3 refs)>,
 #<ARRAY 0x7fbfec311ad8  (3 refs)>,
 #<OBJECT 0x7fc007f96b30 Okra::HTML::Node (3 refs)>,
 #<ARRAY 0x7fbff0999118  (11 refs)>,
 #<OBJECT 0x7fc007f96db0 Okra::HTML::Node (3 refs)>,
 #<ARRAY 0x7fbfec300800  (3 refs)>,
 #<OBJECT 0x7fc007f96f40 Okra::HTML::Node (3 refs)>,
 #<ARRAY 0x7fbfec3007b0  (3 refs)>,
 #<OBJECT 0x7fc007f97080 Okra::HTML::Node (3 refs)>,
 #<ARRAY 0x7fbfbe42eed0  (21 refs)>,
 #<OBJECT 0x7fc007f971c0 Okra::HTML::Node (3 refs)>,
 #<ARRAY 0x7fbfec2ca138  (3 refs)>,
 #<OBJECT 0x7fc007f972b0 Okra::HTML::Node (3 refs)>,
 #<ARRAY 0x7fbff0993538  (11 refs)>,
 #<OBJECT 0x7fc01b8fdb20 Okra::HTML::Node (3 refs)>,
 #<ARRAY 0x7fbfec2c2b90  (3 refs)>,
 #<OBJECT 0x7fc01b8fdcb0 Okra::HTML::Node (3 refs)>,
 #<ARRAY 0x7fbfec2c0138  (2 refs)>,
 #<ARRAY 0x7fbfec49ff30  (2 refs)>,
 #<STRING 0x7fbfec2c0098 "MsoNormal">]
```

All these objects were retained by YJIT, this is clearly shown by the `yjit_root` node in the top of the dependency tree! After finding the root cause I opened an [issue](https://github.com/Shopify/ruby/issues/552) for the YJIT team, which [promptly fixed it](https://github.com/ruby/ruby/pull/9693).

* * *

## Conclusion

The current tooling in Ruby to troubleshoot memory leaks is pretty advanced. Generally, an effective approach is to use `rbtrace`, to extract a heap dump (with object trace allocation enabled); and then analyze it via [heapy](https://github.com/zombocom/heapy), [sheap](https://github.com/jhawthorn/sheap), or any other similar tool.

Happy hunting!
