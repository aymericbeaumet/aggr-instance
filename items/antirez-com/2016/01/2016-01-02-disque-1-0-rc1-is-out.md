---
title: Disque 1.0 RC1 is out!
link: http://antirez.com/news/100
source: antirez-com
published: 2016-01-02T15:26:07Z
updated: 2016-01-02T15:26:07Z
first_seen: 2026-09-19T21:30:23.395188495Z
summary: 'Today I’m happy to announce that the first release candidate for Disque 1.0 is available. If you don''t know what Disque is, the best starting point is to read the README in the Github project page at http://github.com/antirez/disque. Disque is a just piece of software, so it has a material value which can be zero or more, depending on its ability to make useful things for people using it. But for me there is an huge value that goes over what Disque, materially, is. It is the value of designing and doing something you care about. It’s the magic of programming: where there was nothing, now there is something that works, that other people may potentially analyze, run, use. Distributed systems are a beautiful field. Thanks to Redis, and to the people that tried to mentor me in a way or the other, I got exposed to distributed systems. I wanted to translate this love to something tangible. A new, small system, designed from scratch, without prejudices and without looking too closely to what other similar systems were doing. The experience with Redis shown me that message brokers were a very interesting topic, and that in some way, they are the perfect topic to apply DS concepts. I even pretend message brokers can be fun and exciting. So I tried to design a new message queue, and Disque is the result. Disque design goal is to provide a system with a good user experience: to provide certain guarantees in the context of messaging, guarantees which are easy to reason about, and to provide extreme operational simplicity. The RC1 offers the foundation, but there is more work to do. For once I hope that Disque will be tested by Aphyr with Jepsen in depth. Since Disque is a system that provides certain kinds of guarantees that can be tested, if it fails certain tests, this translates directly to some bug to fix, that means to end with a better system. On the operational side there is to test it in the real world. AP and message queues IMHO are a perfect match to provide operational robustness. However I’m not living into the illusion that I got everything right in the first release, so it will take months (or years?) of iteration to *really* reach the operational simplicity I’m targeting. Moreover this is an RC1 that was heavily modified in the latest weeks, I expect it to have a non trivial amount of bugs. From the point of view of making a fun and exciting system, I tried to end with a simple and small API that does not force the user to think at the details of *this specific* implementation, but more generally at the messaging problem she or he got. Disque also has a set of introspection capabilities that should help making it a non-opaque system that is actually possible to debug and observe. Even with all the limits of new code and ideas, the RC release is a great first step, and I’m glad Disque is not in the list of side projects that we programmers start and never complete. I was not alone during the past months, while hacking with Disque and trying to figure out how to shape it, I received the help of: He Sun, Damian Janowski, Josiah Carlson, Michel Martens, Jacques Chester, Kyle Kingsbury, Mark Paluch, Philipp Krenn, Justin Case, Nathan Fritz, Marcos Nils, Jasper Louis Andersen, Vojtech Vitek, Renato C., Sebastian Waisbrot, Redis Labs and Pivotal, and probably more people I’m not remembering right now. Thank you for your help. The RC1 is tagged in the Disque Github repository. Have fun! Comments'
content: extracted
html: 2016-01-02-disque-1-0-rc1-is-out.html
---

## [Disque 1.0 RC1 is out!](https://antirez.com/news/100)

[antirez](https://antirez.com/user/antirez) 3913 days ago. 310414 views.

```
Today I’m happy to announce that the first release candidate for Disque 1.0 is available.

If you don't know what Disque is, the best starting point is to read the README in the Github project page at http://github.com/antirez/disque.

Disque is a just piece of software, so it has a material value which can be zero or more, depending on its ability to make useful things for people using it. But for me there is an huge value that goes over what Disque, materially, is. It is the value of designing and doing something you care about. It’s the magic of programming: where there was nothing, now there is something that works, that other people may potentially analyze, run, use.

Distributed systems are a beautiful field. Thanks to Redis, and to the people that tried to mentor me in a way or the other, I got exposed to distributed systems. I wanted to translate this love to something tangible. A new, small system, designed from scratch, without prejudices and without looking too closely to what other similar systems were doing. The experience with Redis shown me that message brokers were a very interesting topic, and that in some way, they are the perfect topic to apply DS concepts. I even pretend message brokers can be fun and exciting. So I tried to design a new message queue, and Disque is the result.

Disque design goal is to provide a system with a good user experience: to provide certain guarantees in the context of messaging, guarantees which are easy to reason about, and to provide extreme operational simplicity. The RC1 offers the foundation, but there is more work to do. For once I hope that Disque will be tested by Aphyr with Jepsen in depth. Since Disque is a system that provides certain kinds of guarantees that can be tested, if it fails certain tests, this translates directly to some bug to fix, that means to end with a better system.

On the operational side there is to test it in the real world. AP and message queues IMHO are a perfect match to provide operational robustness. However I’m not living into the illusion that I got everything right in the first release, so it will take months (or years?) of iteration to *really* reach the operational simplicity I’m targeting. Moreover this is an RC1 that was heavily modified in the latest weeks, I expect it to have a non trivial amount of bugs.

From the point of view of making a fun and exciting system, I tried to end with a simple and small API that does not force the user to think at the details of *this specific* implementation, but more generally at the messaging problem she or he got. Disque also has a set of introspection capabilities that should help making it a non-opaque system that is actually possible to debug and observe.

Even with all the limits of new code and ideas, the RC release is a great first step, and I’m glad Disque is not in the list of side projects that we programmers start and never complete.

I was not alone during the past months, while hacking with Disque and trying to figure out how to shape it, I received the help of: He Sun, Damian Janowski, Josiah Carlson, Michel Martens, Jacques Chester, Kyle Kingsbury, Mark Paluch, Philipp Krenn, Justin Case, Nathan Fritz, Marcos Nils, Jasper Louis Andersen, Vojtech Vitek, Renato C., Sebastian Waisbrot, Redis Labs and Pivotal, and probably more people I’m not remembering right now. Thank you for your help.

The RC1 is tagged in the Disque Github repository. Have fun!
```

[blog comments powered by Disqus](https://disqus.com)
