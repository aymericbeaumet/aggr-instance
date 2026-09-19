---
title: Redis streams as a pure data structure
link: http://antirez.com/news/128
source: antirez-com
published: 2019-03-22T15:10:15Z
updated: 2019-03-22T15:10:15Z
first_seen: 2026-09-19T21:30:23.395188495Z
summary: 'The new Redis data structure introduced in Redis 5 under the name of “Streams” generated quite some interest in the community. Soon or later I want to run a community survey, talking with users having production use cases, and blogging about it. Today I want to address another issue: I’m starting to suspect that many users are only thinking at Streams as a way to solve Kafka(TM)-alike use cases. Actually the data structure was designed to *also* work in the context of messaging with producers and consumers, but to think that Redis Streams are just good for that is incredibly reductive. Streaming is a terrific pattern and “mental model” that can be applied when designing systems with great success, but Redis Streams, like most Redis data structures, are more general, and can be used to model dozen of different unrelated problems. So in this blog post I’ll focus on Streams as a pure data structure, completely ignoring its blocking operations, consumer groups, and all the messaging parts. ## Streams are CSV files on steroids If you want to log a series of structured data items and decided that databases are overrated after all, you may say something like: let’s just open a file in append only mode, and log every row as a CSV (Comma Separated Value) item: (open data.csv in append only) time=1553096724033,cpu_temp=23.4,load=2.3 time=1553096725029,cpu_temp=23.2,load=2.1 Looks simple and people did this for ages and still do: it’s a solid pattern if you know what you are doing. But what is the in-memory equivalent of that? Memory is more powerful than an append only file and can automagically remove the limitations of a CSV file like that: 1. It’s hard (inefficient) to do range queries here. 2. There is too much redundant information: the time is almost the same in every entry and the fields are duplicated. At the same time removing it will make the format less flexible, if I want to switch to a different set of fields. 3. Item offsets are just the byte offset in the file: if we change the file structure the offset will be wrong, so there is no actual true concept of primary ID here. Entries are basically not univocally addressed in some way. 4. I can’t remove entries, but only mark them as no longer valid without the ability of garbage collecting, if not by rewriting the log. Log rewriting usually sucks for several reasons and if it can be avoided, it’s good. Still such log of CSV entries is also great in some way: there is no fixed structure and fields may change, is trivial to generate, and after all is quite compact as well. The idea with Redis Streams was to retain the good things, but go over the limitations. The result is a hybrid data structure very similar to Redis Sorted Sets: they *feel like* a fundamental data structure, but to get such an effect, internally it uses multiple representations. ## Streams 101 (you may skip that if you know already Redis Stream basics) Redis Streams are represented as delta-compressed macro nodes that are linked together by a radix tree. The effect is to be able to seek to random entries in a very fast way, to obtain ranges if needed, remove old items to create a capped stream, and so forth. Yet our interface to the programmer is very similar to a CSV file: > XADD mystream * cpu-temp 23.4 load 2.3 "1553097561402-0" > XADD mystream * cpu-temp 23.2 load 2.1 "1553097568315-0" As you can see from the example above the XADD command auto generates and returns the entry ID, which is monotonically incrementing and has two parts: -. The time is in milliseconds and the counter increases for entries generated in the same milliseconds. So the first new abstraction on top of the “append only CSV file” idea is that, since we used the asterisk as the ID argument of XADD, we get the entry ID for free from the server. Such ID is not only useful to point to a specific item inside a stream, it’s also related to the time when the entry was added to the stream. In fact with XRANGE it is possible to perform range queries or fetch single items: > XRANGE mystream 1553097561402-0 1553097561402-0 1) 1) "1553097561402-0" 2) 1) "cpu-temp" 2) "23.4" 3) "load" 4) "2.3" In this case I used the same ID as the start and the stop of the range in order to identify a single element. However I can use any range, and a COUNT argument to limit the number of results. Similarly there is no need to specify full IDs as range, I can just use the millisecond unix time part of the IDs, to get elements in a given range of time: > XRANGE mystream 1553097560000 1553097570000 1) 1) "1553097561402-0" 2) 1) "cpu-temp" 2) "23.4" 3) "load" 4) "2.3" 2) 1) "1553097568315-0" 2) 1) "cpu-temp" 2) "23.2" 3) "load" 4) "2.1" For now there is no need to show you more Streams API, there is the Redis documentation for that. For now let’s just focus on that usage pattern: XADD to add stuff, XRANGE (but also XREAD) in order to fetch back ranges (depending on what you want to do), and let’s see why I claim Streams are so powerful as a data structure. However if you want to learn more about Redis Streams and their API, make sure to visit the tutorial here: https://redis.io/topics/streams-intro ## Tennis players A few days ago I was modeling an application with a friend of mine which is learning Redis those days: an app in order to keep track of local tennis courts, local players and matches. The way you model players in Redis is quite obvious, a player is a small object, so an Hash is all you need, with key names like player:. As you model the application data further, to use Redis as its primary, you immediately realize you need a way to track the games played in a given tennis club. If player:1 and player:2 played a game, and player 1 won, we could write the following entry in a stream: > XADD club:1234.matches * player-a 1 player-b 2 winner 1 "1553254144387-0" With this simple operation we have: 1. A unique identifier of the match: the ID in the stream. 2. No need to create an object in order to identify a match. 3. Range queries for free to paginate the matches, or check the matches played in a given moment in the past. Before Streams we needed to create a sorted set scored by time: the sorted set element would be the ID of the match, living in a different key as a Hash value. This is not just more work, it’s also an incredible amount of memory wasted. More, much more you could guess (see later). For now the point to show is that Redis Streams are kinda of a Sorted Set in append only mode, keyed by time, where each element is a small Hash. And in its simplicity this is a revolution in the context of modeling for Redis. ## Memory usage The above use case is not just a matter of a more solid pattern. The memory cost of the Stream solution is so different compared to the old approach of having a Sorted Set + Hash for every object that makes certain things that were not viable, now perfectly fine. Those are the numbers for storing one million of matches in the configurations exposed previously: Sorted Set + Hash memory usage = 220 MB (242 RSS) Stream memory usage = 16.8 MB (18.11 RSS) This is more than an order of magnitude difference (13 times difference exactly), and it means that use cases that yesterday were too costly for in-memory now are perfectly viable. The magic is all in the representation of Redis Streams: the macro nodes can contain several elements that are encoded in a data structure called listpack in a very compact way. Listpacks will take care, for instance, to encode integers in binary form even if they are semantically strings. On top of that, we then apply delta compression and same-fields compression. Yet we are able to seek by ID or time because such macro nodes are linked in the radix tree, which was also designed to use little memory. All these things together account for the low memory usage, but the interesting part is that semantically the user does not see any of the implementation details making Streams efficient. Now let’s do some simple math. If I can store 1 million entries in about 18 MB of memory, I can store 10 millions in 180 MB, and 100 millions in 1.8 GB. With just 18 GB of memory I can have 1 billion items. ## Time series One important thing to note is, in my opinion, how the usage above where we used a Stream to represent a tennis match was semantically *very different* than using a Redis Stream for a time series. Yes, logically we are still logging some kind of event, but one fundamental difference is that in one case we use the logging and the creation of entries in order to render objects. While in the case of time series, we are just metering something happening externally, that does not really represent an object. You may think that this difference is trivial but it’s not. It is important for Redis users to build the idea that Redis Streams can be used in order to create small objects that have a total order, and assign IDs to such objects. However even the most basic use case of time series is, obviously, a huge one here, because before Streams Redis was a bit hopeless in regard to such use case. The memory characteristics and flexibility of streams, plus the ability to have capped streams (see the XADD options), is a very important tool in the hands of the developer. ## Conclusions Streams are flexible and have lots of use cases, however I wanted to take this blog post short to make sure that there is a clear take-home message in the above examples and analysis of the memory usage. Perhaps this was already obvious to many readers, but talking with people in the last months gave me the feeling that there was a strong association between Streams and the streaming use case, like if the data structure was only good at that. That’s not the case :-) Comments'
content: extracted
html: 2019-03-22-redis-streams-as-a-pure-data-structure.html
---

## [Redis streams as a pure data structure](https://antirez.com/news/128)

[antirez](https://antirez.com/user/antirez) 2738 days ago. 201520 views.

```
The new Redis data structure introduced in Redis 5 under the name of “Streams” generated quite some interest in the community. Soon or later I want to run a community survey, talking with users having production use cases, and blogging about it. Today I want to address another issue: I’m starting to suspect that many users are only thinking at Streams as a way to solve Kafka(TM)-alike use cases. Actually the data structure was designed to *also* work in the context of messaging with producers and consumers, but to think that Redis Streams are just good for that is incredibly reductive. Streaming is a terrific pattern and “mental model” that can be applied when designing systems with great success, but Redis Streams, like most Redis data structures, are more general, and can be used to model dozen of different unrelated problems. So in this blog post I’ll focus on Streams as a pure data structure, completely ignoring its blocking operations, consumer groups, and all the messaging parts.

## Streams are CSV files on steroids

If you want to log a series of structured data items and decided that databases are overrated after all, you may say something like: let’s just open a file in append only mode, and log every row as a CSV (Comma Separated Value) item:

(open data.csv in append only)
time=1553096724033,cpu_temp=23.4,load=2.3
time=1553096725029,cpu_temp=23.2,load=2.1

Looks simple and people did this for ages and still do: it’s a solid pattern if you know what you are doing. But what is the in-memory equivalent of that? Memory is more powerful than an append only file and can automagically remove the limitations of  a CSV file like that:

1. It’s hard (inefficient) to do range queries here.

2. There is too much redundant information: the time is almost the same in every entry and the fields are duplicated. At the same time removing it will make the format less flexible, if I want to switch to a different set of fields.

3. Item offsets are just the byte offset in the file: if we change the file structure the offset will be wrong, so there is no actual true concept of primary ID here. Entries are basically not univocally addressed in some way.

4. I can’t remove entries, but only mark them as no longer valid without the ability of garbage collecting, if not by rewriting the log. Log rewriting usually sucks for several reasons and if it can be avoided, it’s good.

Still such log of CSV entries is also great in some way: there is no fixed structure and fields may change, is trivial to generate, and after all is quite compact as well. The idea with Redis Streams was to retain the good things, but go over the limitations. The result is a hybrid data structure very similar to Redis Sorted Sets: they *feel like* a fundamental data structure, but to get such an effect, internally it uses multiple representations.

## Streams 101 (you may skip that if you know already Redis Stream basics)

Redis Streams are represented as delta-compressed macro nodes that are linked together by a radix tree. The effect is to be able to seek to random entries in a very fast way, to obtain ranges if needed, remove old items to create a capped stream, and so forth. Yet our interface to the programmer is very similar to a CSV file:

> XADD mystream * cpu-temp 23.4 load 2.3
"1553097561402-0"
> XADD mystream * cpu-temp 23.2 load 2.1
"1553097568315-0"

As you can see from the example above the XADD command auto generates and returns the entry ID, which is monotonically incrementing and has two parts: <time>-<counter>. The time is in milliseconds and the counter increases for entries generated in the same milliseconds. 

So the first new abstraction on top of the “append only CSV file” idea is that, since we used the asterisk as the ID argument of XADD, we get the entry ID for free from the server. Such ID is not only useful to point to a specific item inside a stream, it’s also related to the time when the entry was added to the stream. In fact with XRANGE it is possible to perform range queries or fetch single items:

> XRANGE mystream 1553097561402-0 1553097561402-0
1) 1) "1553097561402-0"
   2) 1) "cpu-temp"
      2) "23.4"
      3) "load"
      4) "2.3"

In this case I used the same ID as the start and the stop of the range in order to identify a single element. However I can use any range, and a COUNT argument to limit the number of results. Similarly there is no need to specify full IDs as range, I can just use the millisecond unix time part of the IDs, to get elements in a given range of time:

> XRANGE mystream 1553097560000 1553097570000
1) 1) "1553097561402-0"
   2) 1) "cpu-temp"
      2) "23.4"
      3) "load"
      4) "2.3"
2) 1) "1553097568315-0"
   2) 1) "cpu-temp"
      2) "23.2"
      3) "load"
      4) "2.1"

For now there is no need to show you more Streams API, there is the Redis documentation for that. For now let’s just focus on that usage pattern: XADD to add stuff, XRANGE (but also XREAD) in order to fetch back ranges (depending on what you want to do), and let’s see why I claim Streams are so powerful as a data structure.

However if you want to learn more about Redis Streams and their API, make sure to visit the tutorial here: https://redis.io/topics/streams-intro

## Tennis players

A few days ago I was modeling an application with a friend of mine which is learning Redis those days: an app in order to keep track of local tennis courts, local players and matches. The way you model players in Redis is quite obvious, a player is a small object, so an Hash is all you need, with key names like player:<id>. As you model the application data further, to use Redis as its primary, you immediately realize you need a way to track the games played in a given tennis club. If player:1 and player:2 played a game, and player 1 won, we could write the following entry in a stream:

> XADD club:1234.matches * player-a 1 player-b 2 winner 1
"1553254144387-0"

With this simple operation we have:

1. A unique identifier of the match: the ID in the stream.
2. No need to create an object in order to identify a match.
3. Range queries for free to paginate the matches, or check the matches played in a given moment in the past.

Before Streams we needed to create a sorted set scored by time: the sorted set element would be the ID of the match, living in a different key as a Hash value. This is not just more work, it’s also an incredible amount of memory wasted. More, much more you could guess (see later).

For now the point to show is that Redis Streams are kinda of a Sorted Set
in append only mode, keyed by time, where each element is a small Hash. And in its simplicity this is a revolution in the context of modeling for Redis.

## Memory usage

The above use case is not just a matter of a more solid pattern. The memory cost of the Stream solution is so different compared to the old approach of having a Sorted Set + Hash for every object that makes certain things that were not viable, now perfectly fine.

Those are the numbers for storing one million of matches in the configurations exposed previously:

Sorted Set + Hash memory usage = 220 MB (242 RSS)
Stream memory usage                  = 16.8 MB (18.11 RSS)

This is more than an order of magnitude difference (13 times difference exactly), and it means that use cases that yesterday were too costly for in-memory now are perfectly viable. The magic is all in the representation of Redis Streams: the macro nodes can contain several elements that are encoded in a data structure called listpack in a very compact way. Listpacks will take care, for instance, to encode integers in binary form even if they are semantically strings. On top of that, we then apply delta compression and same-fields compression. Yet we are able to seek by ID or time because such macro nodes are linked in the radix tree, which was also designed to use little memory. All these things together account for the low memory usage, but the interesting part is that semantically the user does not see any of the implementation details making Streams efficient.

Now let’s do some simple math. If I can store 1 million entries in about 18 MB of memory, I can store 10 millions in 180 MB, and 100 millions in 1.8 GB. With just 18 GB of memory I can have 1 billion items.

## Time series

One important thing to note is, in my opinion, how the usage above where we used a Stream to represent a tennis match was semantically *very different* than using a Redis Stream for a time series. Yes, logically we are still logging some kind of event, but one fundamental difference is that in one case we use the logging and the creation of entries in order to render objects. While in the case of time series, we are just metering something happening externally, that does not really represent an object. You may think that this difference is trivial but it’s not. It is important for Redis users to build the idea that Redis Streams can be used in order to create small objects that have a total order, and assign IDs to such objects.

However even the most basic use case of time series is, obviously, a huge one here, because before Streams Redis was a bit hopeless in regard to such use case. The memory characteristics and flexibility of streams, plus the ability to have capped streams (see the XADD options), is a very important tool in the hands of the developer.

## Conclusions

Streams are flexible and have lots of use cases, however I wanted to take this blog post short to make sure that there is a clear take-home message in the above examples and analysis of the memory usage. Perhaps this was already obvious to many readers, but talking with people in the last months gave me the feeling that there was a strong association between Streams and the streaming use case, like if the data structure was only good at that. That’s not the case :-)
```

[blog comments powered by Disqus](https://disqus.com)
