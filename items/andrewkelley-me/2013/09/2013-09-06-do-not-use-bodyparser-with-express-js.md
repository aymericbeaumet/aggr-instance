---
title: Do Not Use bodyParser with Express.js
link: https://andrewkelley.me/post/do-not-use-bodyparser-with-express-js.html
source: andrewkelley-me
published: 2013-09-06T22:37:07Z
updated: 2013-09-06T22:37:07Z
first_seen: 2026-09-19T21:30:23.395188495Z
summary: 'Do Not Use bodyParser with Express.js Note: this post has been edited to take into account TJ''s diligent work in response to this. I came across this Google+ post mentioning this StackOverflow post in which someone is quite wisely asking whether the express.js framework is secure enough to use for production applications. This reminds me of one "gotcha" in particular that you could be bitten by if you''re not careful. All servers using express.bodyParser are vulnerable to an attack which creates an unlimited number of temp files on the server, potentially filling up all the disk space, which is likely to cause the server to hang. Demonstration This problem is extremely easy to demonstrate. Here''s a simple express app: var express = require(''express''); var app = express(); app.use(express.bodyParser()); app.post(''/test'', function(req, resp) { resp.send(''ok''); }); app.listen(9001); Seems pretty innocuous right? Now check how many temp files you have with something like this: $ ls /tmp | wc -l 33 Next simulate uploading a multipart form: $ curl -X POST -F foo=@tmp/somefile.c http://localhost:9001/test ok Go back and check our temp file count: $ ls /tmp | wc -l 34 That''s a problem. Solutions Always delete the temp files when you use bodyParser or multipart middleware You can prevent this attack by always checking whether req.files is present for endpoints in which you use bodyParser or multipart, and then deleting the temp files. Note that this is every POST endpoint if you did something like app.use(express.bodyParser()). This is suboptimal for several reasons: It is too easy to forget to do these checks. It requires a bunch of ugly cleanup code. Why have code when you could not have code? Your server is still, for every POST endpoint that you use bodyParser, processing every multipart upload that comes its way, creating a temp file, writing it to disk, and then deleting the temp file. Why do all that when you don''t want to accept uploads? As of express 3.4.0 (connect 2.9.0) bodyParser is deprecated. It goes without saying that deprecated things should be avoided. Use a utility such as tmpwatch or reap jfromaniello pointed out that using a utility such as tmpwatch can help with this issue. The idea here is to, for example, schedule tmpwatch as a cron job. It would remove temp files that have not been accessed in a long enough period of time. It''s usually a good idea to do this for all servers, just in case. But relying on this to clean up bodyParser''s mess still suffers from issue #3 outlined above. Plus, server hard drives are often small, especially when you didn''t realize you were going to have temp files in the first place. If you ran your cron job every 8 hours for instance, given a hdd with 4 GB of free space, an attacker would need an Internet connection with 145 KB/s upload bandwidth to crash your server. TJ pointed out that he also has a utility for this purpose called reap. Avoid bodyParser and explicitly use the middleware that you need If you want to parse json in your endpoint, use express.json() middleware. If you want json and urlencoded endpoint, use [express.json(), express.urlencoded()] for your middleware. If you want users to upload files to your endpoint, you could use express.multipart() and be sure to clean up all the temp files that are created. This would still stuffer from problem #3 previously mentioned. Use the defer option in the multipart middleware When you create your multipart middleware, you can use the defer option like this: express.multipart({defer: true}) According to the documentation: defers processing and exposes the multiparty form object as `req.form`. `next()` is called without waiting for the form''s "end" event. This option is useful if you need to bind to the "progress" or "part" events, for example. So if you do this you will use multiparty''s API assuming that req.form is an instantiated Form instance. Use an upload parsing module directly bodyParser depends on multipart, which behind the scenes uses multiparty to parse uploads. You can use this module directly to handle the request. In this case you can look at multiparty''s API and do the right thing. There are also alternatives such as busboy, parted, and formidable.'
content: extracted
html: 2013-09-06-do-not-use-bodyparser-with-express-js.html
---

Note: this post has [been edited](https://github.com/andrewrk/andrewkelley.me/commit/a5cbf4a7815391dbcc57d6f30f8d330a67487167) to take into account [TJ](https://github.com/visionmedia/)'s [diligent work](https://groups.google.com/forum/#!topic/express-js/iP2VyhkypHo) in response to this.

I came across [this Google+ post](https://plus.google.com/106706438172517329683/posts/4kQiD8L1D36) mentioning [this StackOverflow post](http://stackoverflow.com/questions/14612143/node-js-express-framework-security-issues) in which someone is quite wisely asking whether the [express.js framework](http://expressjs.com/) is secure enough to use for production applications.

This reminds me of one "gotcha" in particular that you could be bitten by if you're not careful.

All servers using [express.bodyParser](http://expressjs.com/api.html#bodyParser) are vulnerable to an attack which creates an unlimited number of temp files on the server, potentially filling up all the disk space, which is likely to cause the server to hang.

## Demonstration

This problem is extremely easy to demonstrate. Here's a simple express app:

```
var express = require('express');
var app = express();

app.use(express.bodyParser());
app.post('/test', function(req, resp) {
  resp.send('ok');
});

app.listen(9001);
```

Seems pretty innocuous right?

Now check how many temp files you have with something like this:

```
$ ls /tmp | wc -l
33
```

Next simulate uploading a multipart form:

```
$ curl -X POST -F foo=@tmp/somefile.c http://localhost:9001/test
ok
```

Go back and check our temp file count:

```
$ ls /tmp | wc -l
34
```

That's a problem.

## Solutions

### Always delete the temp files when you use bodyParser or multipart middleware

You can prevent this attack by always checking whether `req.files` is present for endpoints in which you use `bodyParser` or `multipart`, and then deleting the temp files. Note that this is *every POST endpoint* if you did something like `app.use(express.bodyParser())`.

This is suboptimal for several reasons:

1. It is too easy to forget to do these checks.
2. It requires a bunch of ugly cleanup code. Why have code when you could not have code?
3. Your server is still, for every POST endpoint that you use bodyParser, processing every multipart upload that comes its way, creating a temp file, writing it to disk, and then deleting the temp file. Why do all that when you don't want to accept uploads?
4. As of express 3.4.0 (connect 2.9.0) bodyParser is deprecated. It goes without saying that deprecated things should be avoided.

### Use a utility such as tmpwatch or reap

[jfromaniello](https://github.com/jfromaniello) [pointed out](https://groups.google.com/d/msg/nodejs/6KOlfk5cpcM/SCJ9jZZfP-UJ) that using a utility such as [tmpwatch](http://linux.die.net/man/8/tmpwatch) can help with this issue. The idea here is to, for example, schedule tmpwatch as a cron job. It would remove temp files that have not been accessed in a long enough period of time.

It's usually a good idea to do this for all servers, just in case. But relying on this to clean up bodyParser's mess still suffers from issue #3 outlined above. Plus, server hard drives are often small, especially when you didn't realize you were going to have temp files in the first place.

If you ran your cron job every 8 hours for instance, given a hdd with 4 GB of free space, an attacker would need an Internet connection with 145 KB/s upload bandwidth to crash your server.

TJ pointed out that he also has a utility for this purpose called [reap](https://github.com/visionmedia/reap).

### Avoid bodyParser and explicitly use the middleware that you need

If you want to parse json in your endpoint, use `express.json()` middleware. If you want json and urlencoded endpoint, use `[express.json(), express.urlencoded()]` for your middleware.

If you want users to upload files to your endpoint, you could use `express.multipart()` and be sure to clean up all the temp files that are created. This would still stuffer from problem #3 previously mentioned.

### Use the defer option in the multipart middleware

When you create your multipart middleware, you can use the `defer` option like this:

```
express.multipart({defer: true})
```

According to the documentation:

> defers processing and exposes the multiparty form object as \`req.form\`.\
>  \`next()\` is called without waiting for the form's "end" event.\
>  This option is useful if you need to bind to the "progress" or "part" events, for example.\

So if you do this you will use [multiparty's API](https://github.com/andrewrk/node-multiparty/blob/master/README.md#api) assuming that `req.form` is an instantiated `Form` instance.

### Use an upload parsing module directly

`bodyParser` depends on `multipart`, which behind the scenes uses [multiparty](https://github.com/andrewrk/node-multiparty) to parse uploads.

You can use this module directly to handle the request. In this case you can look at [multiparty's API](https://github.com/andrewrk/node-multiparty/blob/master/README.md#api) and do the right thing.

There are also alternatives such as [busboy](https://github.com/mscdex/busboy), [parted](https://github.com/chjj/parted), and [formidable](https://github.com/felixge/node-formidable).

Thanks for reading my blog post.
