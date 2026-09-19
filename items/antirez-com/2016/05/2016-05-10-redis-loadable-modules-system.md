---
title: Redis Loadable Modules System
link: http://antirez.com/news/106
source: antirez-com
published: 2016-05-10T17:02:55Z
updated: 2016-05-10T17:02:55Z
first_seen: 2026-09-19T21:30:23.395188495Z
summary: 'It was a matter of time but it eventually happened. In the Redis 1.0 release notes, 7 years ago, I mentioned that one of the interesting features for the future was “loadable modules”. I was really interested in such a feature back then, but over the years I became more and more skeptic about the idea of adding loadable modules in Redis. And probably for good reasons. Modules can be the most interesting feature of a system and the most problematic one at the same time: API incompatibilities between versions, low quality modules crashing the system, a lack of identity of a system that is extendible are possible problems. SO, for years, I managed to avoided adding modules to Redis, and Lua scripting was a good tool in order to delay their addition. At the same time, years of experience with scripting, demonstrated that scripting is a way to “compose” existing features, but not a way to extend the capabilities of a system towards use cases it was not designed to cover. Previous attempts at modules also showed that one of the main pain points about mixing Redis and loadable modules is the way modules are bound with the Redis core. In may ways Redis resembles more a programming language than a database. To extend Redis properly, the module needs to have access to the internal API of the system. Directly exporting the Redis core functions to modules creates huge problems: the module starts to depend on the internal details of Redis. If the Redis core evolves, the module needs to be rewritten. This creates either a fragile modules ecosystem or stops the evolution of the Redis core. Redis internals can’t stop to evolve, nor the modules developers can keep modifying the module in order to stay updated with the internals (something that happened in certain popular systems in the past, with poor results). With all this lessons in mind, I was leaving Catania to fly to Tel Aviv, to have a meeting at Redis Labs to talk about the roadmap for the future months. One of the topics of our chat was loadable modules. During the flight I asked myself if it was possible to truly decouple the Redis core from the modules API, but still have a low level access to directly manipulate Redis data structure. So I started to immediately code something. What I wanted was an extreme level of API compatibility for the future, so that a module wrote today could work in 4 years from now with the same API, regardless of the changes to the Redis core. I also wanted binary compatibility so that the 4 years old module could even *load* in the new Redis versions and work as expected, without even the need to be recompiled. At the end of the flight I arrived in Tel Aviv with something already working in the “modules” branch. We discussed together how the API would work, and at the end everybody agreed that to be able to manipulate Redis internals directly was a fundamental feature. What we wanted to accomplish was to allow Redis developers to create commands that were as capable as the Redis native commands, and also as fast as the native commands. This cannot be accomplished just with an high level API that calls Redis commands, it’s too slow and limited. There is no point in having a Redis modules system that can just do what Lua can already do. You need to be able to say, get me the value associated with this key, what type is it? Do this low level operation on the value. Given me a cursor into the sorted set at this position, go to the next element, and so forth. To create an API that works as an intermediate layer for such low level access is tricky, but definitely possible. I returned home and started to work at the modules system immediately. After a couple of weeks I had a prototype that was already functional enough to develop interesting modules, featuring low level functions like data types low level access, strings DMA to poke with the internals of strings without wrappers when needed, a replication API, an interesting sorted set iterator API, and so forth. It looked was a very promising start, however the project was kinda of “secret” because it was not clear if it was viable initially. Also we wanted to avoid everybody to start developing modules while the API was extremely unstable and subject to changes. The result of this process, while not complete, is very promising, so today I’m announcing the new feature at Redis Conference 2016, and the code was just pushed into the “unstable” branch. But let’s check the API a bit… Here is a trivial example of what a module can do and how it works. It implements a “list splice” operation that moves elements from a list to another: int HelloListSpliceAuto_RedisCommand(RedisModuleCtx *ctx, RedisModuleString **argv, int argc) { if (argc != 4) return RedisModule_WrongArity(ctx); RedisModule_AutoMemory(ctx); RedisModuleKey *srckey = RedisModule_OpenKey(ctx,argv[1], REDISMODULE_READ|REDISMODULE_WRITE); RedisModuleKey *dstkey = RedisModule_OpenKey(ctx,argv[2], REDISMODULE_READ|REDISMODULE_WRITE); /* Src and dst key must be empty or lists. */ if ((RedisModule_KeyType(srckey) != REDISMODULE_KEYTYPE_LIST && RedisModule_KeyType(srckey) != REDISMODULE_KEYTYPE_EMPTY) || (RedisModule_KeyType(dstkey) != REDISMODULE_KEYTYPE_LIST && RedisModule_KeyType(dstkey) != REDISMODULE_KEYTYPE_EMPTY)) { return RedisModule_ReplyWithError(ctx,REDISMODULE_ERRORMSG_WRONGTYPE); } long long count; if ((RedisModule_StringToLongLong(argv[3],&count) != REDISMODULE_OK) || (count < 0)) { return RedisModule_ReplyWithError(ctx,"ERR invalid count"); } while(count-- > 0) { RedisModuleString *ele; ele = RedisModule_ListPop(srckey,REDISMODULE_LIST_TAIL); if (ele == NULL) break; RedisModule_ListPush(dstkey,REDISMODULE_LIST_HEAD,ele); } size_t len = RedisModule_ValueLength(srckey); RedisModule_ReplyWithLongLong(ctx,len); return REDISMODULE_OK; } int RedisModule_OnLoad(RedisModuleCtx *ctx) { if (RedisModule_Init(ctx,"helloworld",1,REDISMODULE_APIVER_1) == REDISMODULE_ERR) return REDISMODULE_ERR; if (RedisModule_CreateCommand(ctx,"hello.list.splice.auto", HelloListSpliceAuto_RedisCommand, "write deny-oom",1,2,1) == REDISMODULE_ERR) return REDISMODULE_ERR; } There was a big effort into providing a clean API, and an API that is not prone to misuses. For example there is support for automatic memory management, so that the context the command operates on, collects the objects that are not explicitly freed by the user, in order to free them when the command returns if needed. This makes writing modules a lot simpler. You can find the API documentation here (not perfect but there is enough to get familiar): https://github.com/antirez/redis/blob/unstable/src/modules/INTRO.md And the API reference here: https://github.com/antirez/redis/blob/unstable/src/modules/API.md And many simple examples of commands here: https://github.com/antirez/redis/blob/unstable/src/modules/helloworld.c The API is yet not complete nor stable, and will be released with the next stable version of Redis (likely 4.0). However it is already enough to do a lot of things, and my colleagues did very interesting things, from inverted indexes to authentication systems. In the next weeks we’ll fill all the holes, for example there is no low level Set API, so you’ll have to use Call() style API for now. Similarly the iterator is only provided for the sorted set type, and so forth. But the important point is that the process is now started, and Redis is becoming an extendible system. I think this will give more power to Redis users, power to go “faster” than the project itself, in using Redis to model their problems, with the big promise that, after Redis 4.0 RC is out, we’ll not break the API ever for years to come, so that modules work will not get wasted. Note that we *can improve* the API, since the module registration asks for a given API version, so it will be possible to maintain the backward compatibility while at the same time release new versions of the API. Soon there will be a Modules Directory were you can register your modules, using redis-cli, into a server that talks the Redis protocol. It was not possible to finish it in time unfortunately, but it is just a matter of weeks. I’m very very excited about what will happen now! Modules will have a Bazar model like clients, so there will not be “official modules”. The good ones will be used for sure, and all will get listed in the Redis site, probably ranked by Github stars or something like that. I hope many users will start to be part of the modules ecosystem and make Redis able to solve very specific use cases that was not wise to solve inside the core, but that are a good fit for modules. Now I need your feedbacks while the API is still malleable. Tell me what you think! Comments'
content: extracted
html: 2016-05-10-redis-loadable-modules-system.html
---

## [Redis Loadable Modules System](https://antirez.com/news/106)

[antirez](https://antirez.com/user/antirez) 3784 days ago. 299307 views.

```
It was a matter of time but it eventually happened. In the Redis 1.0 release notes, 7 years ago, I mentioned that one of the interesting features for the future was “loadable modules”. I was really interested in such a feature back then, but over the years I became more and more skeptic about the idea of adding loadable modules in Redis. And probably for good reasons.

Modules can be the most interesting feature of a system and the most problematic one at the same
time: API incompatibilities between versions, low quality modules crashing the system, a lack
of identity of a system that is extendible are possible problems. SO, for years, I managed to avoided adding modules to Redis, and Lua scripting was a good tool in order to delay their addition. At the same time, years of experience with scripting, demonstrated that scripting is a way to “compose” existing features, but not a way to extend the capabilities of a system towards use cases it was not designed to cover.

Previous attempts at modules also showed that one of the main pain points about mixing Redis
and loadable modules is the way modules are bound with the Redis core. In may ways Redis resembles more a programming language than a database. To extend Redis properly, the module needs to have access to the internal API of the system. Directly exporting the Redis core functions to modules creates huge problems: the module starts to depend on the internal details of Redis. If the Redis core evolves, the module needs to be rewritten. This creates either a fragile modules ecosystem or stops the evolution of the Redis core. Redis internals can’t stop to evolve, nor the modules developers can keep modifying the module in order to stay updated with the internals (something that happened in certain popular systems in the past, with poor results).

With all this lessons in mind, I was leaving Catania to fly to Tel Aviv, to have a meeting at
Redis Labs to talk about the roadmap for the future months. One of the topics of our chat was loadable modules. During the flight I asked myself if it was possible to truly decouple the Redis core from the modules API, but still have a low level access to directly manipulate Redis data structure. So I started to immediately code something. What I wanted was an extreme level of API compatibility for the future, so that a module wrote today could work in 4 years from now with the same API, regardless of the changes to the Redis core. I also wanted binary compatibility so that the 4 years old module could even *load* in the new Redis versions and work as expected, without even the need to be recompiled.

At the end of the flight I arrived in Tel Aviv with something already working in the “modules” branch. We discussed together how the API would work, and at the end everybody agreed that to be able to manipulate Redis internals directly was a fundamental feature. What we wanted to accomplish was to allow Redis developers to create commands that were as capable as the Redis native commands, and also as fast as the native commands. This cannot be accomplished just with an high level API that calls Redis commands, it’s too slow and limited. There is no point in having a Redis modules system that can just do what Lua can already do. You need to be able to say, get me the value associated with this key, what type is it? Do this low level operation on the value. Given me a cursor into the sorted set at this position, go to the next element, and so forth. To create an API that works as an intermediate layer for such low level access is tricky, but definitely possible.

I returned home and started to work at the modules system immediately. After a couple of weeks I had a prototype that was already functional enough to develop interesting modules, featuring low level functions like data types low level access, strings DMA to poke with the internals of strings without wrappers when needed, a replication API, an interesting sorted set iterator API, and so forth. It looked was a very promising start, however the project was kinda of “secret” because it was not clear if it was viable initially. Also we wanted to avoid everybody to start developing modules while the API was extremely unstable and subject to changes.

The result of this process, while not complete, is very promising, so today I’m announcing the new feature at Redis Conference 2016, and the code was just pushed into the “unstable” branch. But let’s check the API a bit…

Here is a trivial example of what a module can do and how it works. It implements a
“list splice” operation that moves elements from a list to another:

int HelloListSpliceAuto_RedisCommand(RedisModuleCtx *ctx, RedisModuleString **argv, int argc) {
    if (argc != 4) return RedisModule_WrongArity(ctx);

    RedisModule_AutoMemory(ctx);

    RedisModuleKey *srckey = RedisModule_OpenKey(ctx,argv[1],
        REDISMODULE_READ|REDISMODULE_WRITE);
    RedisModuleKey *dstkey = RedisModule_OpenKey(ctx,argv[2],
        REDISMODULE_READ|REDISMODULE_WRITE);

    /* Src and dst key must be empty or lists. */
    if ((RedisModule_KeyType(srckey) != REDISMODULE_KEYTYPE_LIST &&
         RedisModule_KeyType(srckey) != REDISMODULE_KEYTYPE_EMPTY) ||
        (RedisModule_KeyType(dstkey) != REDISMODULE_KEYTYPE_LIST &&
         RedisModule_KeyType(dstkey) != REDISMODULE_KEYTYPE_EMPTY))
    {
        return RedisModule_ReplyWithError(ctx,REDISMODULE_ERRORMSG_WRONGTYPE);
    }

    long long count;
    if ((RedisModule_StringToLongLong(argv[3],&count) != REDISMODULE_OK) ||
        (count < 0))
    {
        return RedisModule_ReplyWithError(ctx,"ERR invalid count");
    }

    while(count-- > 0) {
        RedisModuleString *ele;

        ele = RedisModule_ListPop(srckey,REDISMODULE_LIST_TAIL);
        if (ele == NULL) break;
        RedisModule_ListPush(dstkey,REDISMODULE_LIST_HEAD,ele);
    }

    size_t len = RedisModule_ValueLength(srckey);
    RedisModule_ReplyWithLongLong(ctx,len);
    return REDISMODULE_OK;
}

int RedisModule_OnLoad(RedisModuleCtx *ctx) {
    if (RedisModule_Init(ctx,"helloworld",1,REDISMODULE_APIVER_1)
        == REDISMODULE_ERR) return REDISMODULE_ERR;

    if (RedisModule_CreateCommand(ctx,"hello.list.splice.auto",
        HelloListSpliceAuto_RedisCommand,
        "write deny-oom",1,2,1) == REDISMODULE_ERR)
        return REDISMODULE_ERR;
}

There was a big effort into providing a clean API, and an API that is not prone to misuses.
For example there is support for automatic memory management, so that the context the command
operates on, collects the objects that are not explicitly freed by the user, in order to free them when the command returns if needed. This makes writing modules a lot simpler.

You can find the API documentation here (not perfect but there is enough to get familiar):
https://github.com/antirez/redis/blob/unstable/src/modules/INTRO.md

And the API reference here:
https://github.com/antirez/redis/blob/unstable/src/modules/API.md

And many simple examples of commands here:
https://github.com/antirez/redis/blob/unstable/src/modules/helloworld.c

The API is yet not complete nor stable, and will be released with the next stable version of Redis (likely 4.0). However it is already enough to do a lot of things, and my colleagues did very interesting things, from inverted indexes to authentication systems. In the next weeks we’ll fill all the holes, for example there is no low level Set API, so you’ll have to use Call() style API for now. Similarly the iterator is only provided for the sorted set type, and so forth.

But the important point is that the process is now started, and Redis is becoming an extendible system.
I think this will give more power to Redis users, power to go “faster” than the project itself, in using Redis to model their problems, with the big promise that, after Redis 4.0 RC is out, we’ll not break the API ever for years to come, so that modules work will not get wasted.
Note that we *can improve* the API, since the module registration asks for a given API version,
so it will be possible to maintain the backward compatibility while at the same time release new versions of the API.

Soon there will be a Modules Directory were you can register your modules, using redis-cli, into a server that talks the Redis protocol. It was not possible to finish it in time unfortunately, but it is just a matter of weeks.

I’m very very excited about what will happen now! Modules will have a Bazar model like clients, so there will not be “official modules”. The good ones will be used for sure, and all will get listed in the Redis site, probably ranked by Github stars or something like that.

I hope many users will start to be part of the modules ecosystem and make Redis able to solve very specific use cases that was not wise to solve inside the core, but that are a good fit for modules.

Now I need your feedbacks while the API is still malleable. Tell me what you think!
```

[blog comments powered by Disqus](https://disqus.com)
