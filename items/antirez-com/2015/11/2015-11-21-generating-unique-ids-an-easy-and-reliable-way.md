---
title: 'Generating unique IDs: an easy and reliable way'
link: http://antirez.com/news/99
source: antirez-com
published: 2015-11-21T14:47:01Z
updated: 2015-11-21T14:47:01Z
first_seen: 2026-09-19T21:30:23.395188495Z
summary: 'Two days ago Mike Malone published an interesting post on Medium about the V8 implementation of Math.random(), and how weak is the quality of the PRNG used: http://bit.ly/1SPDraN. The post was one of the top news on Hacker News today. It’s pretty clear and informative from the point of view of how Math.random() is broken and how should be fixed, so I’ve nothing to add to the matter itself. But since the author discovered the weakness of the PRNG in the context of generating large probably-non-colliding IDs, I want to share with you an alternative that I used multiple times in the past, which is fast and extremely reliable. The problem of unique IDs - - - So, in theory, if you want to generate unique IDs you need to store some state that makes sure an ID is never repeated. In the trivial case you may use just a simple counter. However the previous ID generated must be stored in a consistent way. In case of restart of the system, it should never happen that the same ID is generated again because our stored counter was not correctly persisted on disk. If we want to generate unique IDs using multiple processes, each process needs to make sure to prepend its IDs with some process-specific prefix that will never collide with another process prefix. This can be complex to manage as well. The simple fact of having to store in a reliable way the old ID is very time consuming when we want to generate an high number of IDs per second. Fortunately there is a simple solution. Generate a random number in a range between 0 and N, with N so big that the probability of collisions is so small to be, for every practical application, irrelevant. This works if the number we generate is uniformly distributed between 0 and N. If this prerequisite is true we can use the birthday paradox in order to calculate the probability of collisions. By using enough bits it’s trivial to make the probability of a collision billions of times less likely than an asteroid centering the Earth, even if we generate millions of IDs per second for hundreds of years. If this is not enough margin for you, just add more bits, you can easily reach an ID space larger than the number of atoms in the universe. This generation method has a great advantage: it is completely stateless. Multiple nodes can generate IDs at the same time without exchanging messages. Moreover there is nothing to store on disk so we can go as fast as our CPU can go. The computation will easily fit the CPU cache. So it’s terribly fast and convenient. Mike Malone was using this idea, by using the PRNG in order to create an ID composed of a set of characters, where each character was one of 64 possible characters. In order to create each character the weak V8 PRNG was used, resulting into collisions. Remember that our initial assumption is that each new ID must be selected uniformly in the space between 0 and N. You can fix this problem by using a stronger PRNG, but this requires an analysis of the PRNG. Another problem is seeding, how do you start the process again after a restart in order to make sure you don’t pick the initial state of the PRNG again? Otherwise your real ID space is limited by the seeding of the PRNG, not the output space itself. For all the above reasons I want to show you a trivial technique that avoids most of these problems. Using a crypto hash function to generate unique IDs - - - Cryptographic hash functions are non invertible functions that transform a sequence of bits into a fixed sequence of bits. They are designed in order to resist a variety of attacks, however in this application we only rely on a given characteristic they have: uniformity of output. Changing a bit in the input of the hash function will result in each bit of the output to change with a 50% probability. In order to have a reliable seed, we use some help from the operating system, by querying /dev/urandom. Seeding the generator is a moment where we really want some external entropy, otherwise we really risk of doing some huge mistake and generating the same sequence again. As an example of crypto hash function we''ll use the well known SHA1, that has an output of 160 bits. Note that you could use even MD5 sum for this application: the vulnerabilities it has have no impact in our usage here. We start creating a seed, by reading 160 bits from /dev/urandom. In pseudocode it will be something like: seed = devurandom.read(160/8) We also initialize a counter: counter = 0 Now this is the function that will generate every new ID: function get_new_id() myid = SHA1(string(counter) + seed) counter = counter + 1 return myid end Basically we have a fixed string, which is our seed, and we hash it with a progressive counter, so if our seed is “foo”, we output new IDs as: SHA1(“0foo”) SHA1(“1foo”) SHA1(“2foo”) This is already good for our use case. However we may also need that our IDs are not easy to predict. In order to make the IDs very hard to predict instead of using SHA1 in the get_new_id() function, just use SHA1_HMAC() instead, where the seed is the secret, and the counter the message of the HMAC. This method is fast, has guaranteed good distribution, so collisions will be as hard as predicted by the birthday paradox, there is no analysis needed on the PRNG, and is completely stateless. I use it on my Disque project in order to generate message IDs among multiple nodes in a distributed system. Hacker News thread here: https://news.ycombinator.com/item?id=10606910 Comments'
content: extracted
html: 2015-11-21-generating-unique-ids-an-easy-and-reliable-way.html
---

## [Generating unique IDs: an easy and reliable way](https://antirez.com/news/99)

[antirez](https://antirez.com/user/antirez) 3955 days ago. 468474 views.

```
Two days ago Mike Malone published an interesting post on Medium about the V8 implementation of Math.random(), and how weak is the quality of the PRNG used: http://bit.ly/1SPDraN.

The post was one of the top news on Hacker News today. It’s pretty clear and informative from the point of view of how Math.random() is broken and how should be fixed, so I’ve nothing to add to the matter itself. But since the author discovered the weakness of the PRNG in the context of generating large probably-non-colliding IDs, I want to share with you an alternative that I used multiple times in the past, which is fast and extremely reliable.

The problem of unique IDs
- - -

So, in theory, if you want to generate unique IDs you need to store some state that makes sure an ID is never repeated. In the trivial case you may use just a simple counter. However the previous ID generated must be stored in a consistent way. In case of restart of the system, it should never happen that the same ID is generated again because our stored counter was not correctly persisted on disk.

If we want to generate unique IDs using multiple processes, each process needs to make sure to prepend its IDs with some process-specific prefix that will never collide with another process prefix. This can be complex to manage as well. The simple fact of having to store in a reliable way the old ID is very time consuming when we want to generate an high number of IDs per second.

Fortunately there is a simple solution. Generate a random number in a range between 0 and N, with N so big that the probability of collisions is so small to be, for every practical application, irrelevant.
This works if the number we generate is uniformly distributed between 0 and N. If this prerequisite is true we can use the birthday paradox in order to calculate the probability of collisions.

By using enough bits it’s trivial to make the probability of a collision billions of times less likely than an asteroid centering the Earth, even if we generate millions of IDs per second for hundreds of years.
If this is not enough margin for you, just add more bits, you can easily reach an ID space larger than the number of atoms in the universe.

This generation method has a great advantage: it is completely stateless. Multiple nodes can generate IDs at the same time without exchanging messages. Moreover there is nothing to store on disk so we can go as fast as our CPU can go. The computation will easily fit the CPU cache. So it’s terribly fast and convenient.

Mike Malone was using this idea, by using the PRNG in order to create an ID composed of a set of characters, where each character was one of 64 possible characters. In order to create each character the weak V8 PRNG was used, resulting into collisions. Remember that our initial assumption is that each new ID must be selected uniformly in the space between 0 and N.

You can fix this problem by using a stronger PRNG, but this requires an analysis of the PRNG. Another problem is seeding, how do you start the process again after a restart in order to make sure you don’t pick the initial state of the PRNG again? Otherwise your real ID space is limited by the seeding of the PRNG, not the output space itself.

For all the above reasons I want to show you a trivial technique that avoids most of these problems.

Using a crypto hash function to generate unique IDs
- - -

Cryptographic hash functions are non invertible functions that transform a sequence of bits into a fixed sequence of bits. They are designed in order to resist a variety of attacks, however in this application we only rely on a given characteristic they have: uniformity of output. Changing a bit in the input of the hash function will result in each bit of the output to change with a 50% probability.

In order to have a reliable seed, we use some help from the operating system, by querying /dev/urandom. Seeding the generator is a moment where we really want some external entropy, otherwise we really risk of doing some huge mistake and generating the same sequence again.

As an example of crypto hash function we'll use the well known SHA1, that has an output of 160 bits. Note that you could use even MD5 sum for this application: the vulnerabilities it has have no impact in our usage here.

We start creating a seed, by reading 160 bits from /dev/urandom. In pseudocode it will be something like:

    seed = devurandom.read(160/8)

We also initialize a counter:

    counter = 0

Now this is the function that will generate every new ID:

    function get_new_id()
        myid = SHA1(string(counter) + seed)
        counter = counter + 1
        return myid
    end

Basically we have a fixed string, which is our seed, and we hash it with a progressive counter, so if our seed is “foo”, we output new IDs as:

    SHA1(“0foo”)
    SHA1(“1foo”)
    SHA1(“2foo”)

This is already good for our use case. However we may also need that our IDs are not easy to predict. In order to make the IDs very hard to predict instead of using SHA1 in the get_new_id() function, just use SHA1_HMAC() instead, where the seed is the secret, and the counter the message of the HMAC.

This method is fast, has guaranteed good distribution, so collisions will be as hard as predicted by the birthday paradox, there is no analysis needed on the PRNG, and is completely stateless.

I use it on my Disque project in order to generate message IDs among multiple nodes in a distributed system.

Hacker News thread here: https://news.ycombinator.com/item?id=10606910
```

[blog comments powered by Disqus](https://disqus.com)
