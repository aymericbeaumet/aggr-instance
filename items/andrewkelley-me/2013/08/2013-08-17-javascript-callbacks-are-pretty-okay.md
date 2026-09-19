---
title: JavaScript Callbacks are Pretty Okay
link: https://andrewkelley.me/post/js-callback-organization.html
source: andrewkelley-me
published: 2013-08-17T04:41:14Z
updated: 2013-08-17T04:41:14Z
first_seen: 2026-09-19T21:30:23.395188495Z
summary: 'JavaScript Callbacks are Pretty Okay I''ve seen a fair amount of callback bashing on Hacker News recently. Among the many proposed solutions, one of them strikes me as particularly clean: "asynchronous wait" or "wait and defer". iced-coffee-script had this a while ago. Kal just debuted with an identical solution, only differing in syntax. Supposedly LiveScript supports this with "backcalls". I must say, "asynchronous wait" or "monads" - whatever you want to call it - seems like an improvement over callbacks. But I''m here to say that actually... callbacks are pretty okay. Further, given how clean callbacks can be, the downsides of using a compile-to-js language often outweigh the benefits. I have 2 rules of thumb for code organization which makes clean callback based async code brainless: Avoid nontrivial anonymous functions. Put all function declarations after the code that actually does things. Example compile-to-js languages often show examples of deeply nested callback code to show how it can be refactored in the language. Let''s take one from Kal: function getUserFriends(userName, next) { db.users.findOne({name:userName}, function (err, user) { if (err != null) return next(err); db.friends.find({userId:user.id}, function (err, friends) { if (err != null) return next(err); return next(null, friends); }); }); } Yikes that does look a bit nested. But let''s apply the first rule and un-nest both of those anonymous functions. function getUserFriends(userName, next) { db.users.findOne({name:userName}, foundOne); function foundOne(err, user) { if (err != null) return next(err); db.friends.find({userId:user.id}, foundFriends); } function foundFriends(err, friends) { if (err != null) return next(err); return next(null, friends); } } It''s actually longer now, but it''s much easier to parse. When you want to learn what any given function does, you only have to understand 1-2 lines. For example, getUserFriends really only has 1 line which is the findOne part. The rest is a list of function declarations. Next you probably want to learn what the foundOne function does, so you jump to it and only have to read 2 lines to know what it does. Finally you probably want to learn what the foundFriends function does, so you jump to it, and again, only have to read 2 lines. Example Here''s another one taken from Kal (sorry to pick on you rzimmerman; you have good examples): var async = require(''async''); var getUserFriends = function (userName, next) { db.users.findOne({name:userName}, function (err, user) { if (err != null) return next(err); getFriendsById(user.id, function (err, friends) { if (err != null) return next(err); if (user.type == ''power user'') { async.map(friends, getFriendsById, function (err, friendsOfFriends) { for (var i = 0; i < friendsOfFriends.length; i++) { for (var j = 0; j < friendsOfFriends[i].length; j++) { if (friends.indexOf(friendsOfFriends[i][j]) != -1) { friends.push(friendsOfFriends[i][j]); } } } return next(null, friends); }); } else { return next(null, friends); } }); }); } var getFriendsById = function (userId, next) { db.friends.find({userId:userId}, function (err, friends) { if (err != null) return next(err); return next(null, friends); }); } Yep that is an eyesore. Let''s see what the 2 rules do. var async = require(''async''); function getUserFriends(userName, next) { db.users.findOne({name:userName}, foundUser); function foundUser(err, user) { if (err != null) return next(err); getFriendsById(user.id, gotFriends); function gotFriends(err, friends) { if (err != null) return next(err); if (user.type == ''power user'') { async.map(friends, getFriendsById, wtfFriendAction); } else { return next(null, friends); } function wtfFriendAction(err, friendsOfFriends) { for (var i = 0; i < friendsOfFriends.length; i++) { for (var j = 0; j < friendsOfFriends[i].length; j++) { if (friends.indexOf(friendsOfFriends[i][j]) != -1) { friends.push(friendsOfFriends[i][j]); } } } return next(null, friends); } } } } function getFriendsById(userId, next) { db.friends.find({userId:userId}, function (err, friends) { if (err != null) return next(err); return next(null, friends); }); } Okay actually while refactoring that code I realized it made no sense, hence my naming of wtfFriendAction. But let''s run with it. In this refactored code, we''ve only reduced the maximum nesting by 1 - from 8 to 7. But consider how much easier it is to follow. When you look at any given function, there are a few lines of synchronous code, followed by function declarations. Exception - I left getFriendsById alone since it is so short. Quick note on the downsides of compile-to-js languages First to note - Coffee-Script actually prohibits this kind of code organization, because all functions are necessarily assignments. Other compile-to-js languages solve this problem by providing function declarations. But all compile-to-js languages have some fundamental problems. For one, you increase the barrier to contributions to your code. People are a bazillion times more likely to create a pull request if they already know the language your module or app is written in. When you pick an obscure language to write your code in, you alienate a large number of potential contributors. It gets worse. Most people, when evaluating a module or app, will quickly scan the source code to see if the implementation looks reasonable. The depth of analyzation may not be too great; people are looking for obvious problems. When they see that it''s written in another language, it makes the codebase seem foreign; possibly even untrusted. At the very least it hampers their ability to judge quality. Finally, it means adding a build step to your code. Often this is not a big deal; you may already have a build step. But it is an additional moving part in your project that must be understood by you and any potential contributors, or even potential users. I probably sound like one of those grumps who scoffed at any programming language higher-level than assembly. But I actually really got into Coffee-Script for a while, then switched over to coco due to it solving some problems better. I have a nontrivial music player app written in coco. It was first in JavaScript, then Coffee-Script, then coco. But I''ve converted back to pure JavaScript in the active working branch. The first version of naught was written in coco but that''s now JavaScript as well. I learned the hard way about some of these tradeoffs. Conclusion I''ve outlined 2 simple rules for callback organization that I think make writing async code in pure JavaScript more than adequate. To review: Avoid nontrivial anonymous functions. Function declarations after the code that actually does things. Both principles aim for the same goal: A reader of your code should be able to look at the code that actually does things synchronously all together. This means placing all the stuff that happens later at the end. I''m not one to hinder progress in the world of programming languages, but I thought I''d share my perspective on why I still write pure JavaScript for Node.js and browser apps.'
content: extracted
html: 2013-08-17-javascript-callbacks-are-pretty-okay.html
---

I've seen a fair amount of [callback bashing](http://tirania.org/blog/archive/2013/Aug-15.html) on [Hacker News](https://news.ycombinator.com/) recently.

Among the many proposed solutions, one of them strikes me as particularly clean: "asynchronous wait" or "wait and defer". [iced-coffee-script](http://maxtaco.github.io/coffee-script/) had this a while ago. [Kal](http://rzimmerman.github.io/kal/) just debuted with an identical solution, only differing in syntax. Supposedly [LiveScript supports this with "backcalls"](http://livescript.net/#backcalls).

I must say, "asynchronous wait" or "monads" - whatever you want to call it - seems like an improvement over callbacks. But I'm here to say that actually... callbacks are pretty okay. Further, given how clean callbacks can be, the downsides of using a compile-to-js language often outweigh the benefits.

I have 2 rules of thumb for code organization which makes clean callback based async code brainless:

1. Avoid nontrivial anonymous functions.
2. Put all function declarations *after* the code that actually does things.

### Example

compile-to-js languages often show examples of deeply nested callback code to show how it can be refactored in the language. Let's take one from [Kal](http://rzimmerman.github.io/kal/):

```
function getUserFriends(userName, next) {
    db.users.findOne({name:userName}, function (err, user) {
        if (err != null) return next(err);
        db.friends.find({userId:user.id}, function (err, friends) {
            if (err != null) return next(err);
            return next(null, friends);
        });
    });
}
```

Yikes that does look a bit nested. But let's apply the first rule and un-nest both of those anonymous functions.

```
function getUserFriends(userName, next) {
    db.users.findOne({name:userName}, foundOne);

    function foundOne(err, user) {
        if (err != null) return next(err);
        db.friends.find({userId:user.id}, foundFriends);
    }

    function foundFriends(err, friends) {
        if (err != null) return next(err);
        return next(null, friends);
    }
}
```

It's actually longer now, but it's much easier to parse. When you want to learn what any given function does, you only have to understand 1-2 lines. For example, `getUserFriends` really only has 1 line which is the `findOne` part. The rest is a list of function declarations. Next you probably want to learn what the `foundOne` function does, so you jump to it and only have to read 2 lines to know what it does. Finally you probably want to learn what the `foundFriends` function does, so you jump to it, and again, only have to read 2 lines.

### Example

Here's another one taken from Kal (sorry to pick on you [rzimmerman](https://github.com/rzimmerman); you have good examples):

```
var async = require('async');

var getUserFriends = function (userName, next) {
    db.users.findOne({name:userName}, function (err, user) {
        if (err != null) return next(err);
        getFriendsById(user.id, function (err, friends) {
            if (err != null) return next(err);
            if (user.type == 'power user') {
                async.map(friends, getFriendsById, function (err, friendsOfFriends) {
                    for (var i = 0; i < friendsOfFriends.length; i++) {
                        for (var j = 0; j < friendsOfFriends[i].length; j++) {
                            if (friends.indexOf(friendsOfFriends[i][j]) != -1) {
                                friends.push(friendsOfFriends[i][j]);
                            }
                        }
                    }
                    return next(null, friends);
                });
            } else {
                return next(null, friends);
            }
        });
    });
}
var getFriendsById = function (userId, next) {
    db.friends.find({userId:userId}, function (err, friends) {
        if (err != null) return next(err);
        return next(null, friends);
    });
}
```

Yep that is an eyesore. Let's see what the 2 rules do.

```
var async = require('async');

function getUserFriends(userName, next) {
    db.users.findOne({name:userName}, foundUser);

    function foundUser(err, user) {
        if (err != null) return next(err);
        getFriendsById(user.id, gotFriends);

        function gotFriends(err, friends) {
            if (err != null) return next(err);
            if (user.type == 'power user') {
                async.map(friends, getFriendsById, wtfFriendAction);
            } else {
                return next(null, friends);
            }

            function wtfFriendAction(err, friendsOfFriends) {
                for (var i = 0; i < friendsOfFriends.length; i++) {
                    for (var j = 0; j < friendsOfFriends[i].length; j++) {
                        if (friends.indexOf(friendsOfFriends[i][j]) != -1) {
                            friends.push(friendsOfFriends[i][j]);
                        }
                    }
                }
                return next(null, friends);
            }
        }
    }
}

function getFriendsById(userId, next) {
    db.friends.find({userId:userId}, function (err, friends) {
        if (err != null) return next(err);
        return next(null, friends);
    });
}
```

Okay actually while refactoring that code I realized it made no sense, hence my naming of `wtfFriendAction`. But let's run with it.

In this refactored code, we've only reduced the maximum nesting by 1 - from 8 to 7. But consider how much easier it is to follow. When you look at any given function, there are a few lines of synchronous code, followed by function declarations. Exception - I left `getFriendsById` alone since it is so short.

### Quick note on the downsides of compile-to-js languages

First to note - Coffee-Script actually *prohibits* this kind of code organization, because all functions are necessarily assignments. Other compile-to-js languages solve this problem by providing function declarations. But all compile-to-js languages have some fundamental problems.

For one, you increase the barrier to contributions to your code. People are a bazillion times more likely to create a pull request if they already know the language your module or app is written in. When you pick an obscure language to write your code in, you alienate a large number of potential contributors.

It gets worse. Most people, when evaluating a module or app, will quickly scan the source code to see if the implementation looks reasonable. The depth of analyzation may not be too great; people are looking for obvious problems. When they see that it's written in another language, it makes the codebase seem foreign; possibly even untrusted. At the very least it hampers their ability to judge quality.

Finally, it means adding a build step to your code. Often this is not a big deal; you may already have a build step. But it is an additional moving part in your project that must be understood by you and any potential contributors, or even potential users.

I probably sound like one of those grumps who scoffed at any programming language higher-level than assembly. But I actually really got into Coffee-Script for a while, then switched over to [coco](https://github.com/satyr/coco/) due to it solving some problems better. I have a [nontrivial music player app](https://github.com/andrewrk/groovebasin) written in coco. It was first in JavaScript, then Coffee-Script, then coco. But I've converted back to pure JavaScript in the active working branch. The first version of [naught](https://github.com/andrewrk/naught) was written in coco but that's now JavaScript as well. I learned the hard way about some of these tradeoffs.

### Conclusion

I've outlined 2 simple rules for callback organization that I think make writing async code in pure JavaScript more than adequate. To review:

1. Avoid nontrivial anonymous functions.
2. Function declarations *after* the code that actually does things.

Both principles aim for the same goal: A reader of your code should be able to look at the code that actually does things synchronously all together. This means placing all the stuff that happens later at the end.

I'm not one to hinder progress in the world of programming languages, but I thought I'd share my perspective on why I still write pure JavaScript for Node.js and browser apps.

Thanks for reading my blog post.
