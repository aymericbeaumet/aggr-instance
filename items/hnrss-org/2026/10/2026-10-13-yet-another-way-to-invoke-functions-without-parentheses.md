---
title: Yet another way to invoke functions without parentheses
link: https://blog.ikaes.de/yet-another-way-to-invoke-functions-without-parentheses/
source: hnrss-org
published: 2026-10-13T19:25:18Z
updated: 2026-10-13T19:25:18Z
first_seen: 2026-10-16T14:45:31.206858038Z
authors:
- aragonite
content: extracted
html: 2026-10-13-yet-another-way-to-invoke-functions-without-parentheses.html
extra:
  comments_url: https://news.ycombinator.com/item?id=50077225
  num_comments: 63
  points: 175
---

i found yet another way to invoke arbitrary functions in javascript without using parentheses

![](https://blog.ikaes.de/static/01a10710-829b-7587-9e27-10e88ffa7349.png)

this payload works by overwriting the `prepareStackTrace` function of the `Error` object to generate arbitrary functions. it is usually used [to format stack traces](https://v8.dev/docs/stack-trace-api#customizing-stack-traces) and is invoked when the attribute `stack` of an error is accessed.

![](https://blog.ikaes.de/static/01a1073b-7a44-778f-96de-87762c1da0f6.png)

the parameters of this function are the error and "[a structured representation of the stack](https://v8.dev/docs/stack-trace-api#customizing-stack-traces)" , which is just an array of the code positions. it has the same function signature the `Function constructor` which is used to generate javascript functions, as long as the second argument is valid javascript code.

![](https://blog.ikaes.de/static/01a1071b-7b38-72c3-b3b9-3aa2be848dcf.png)

if we try to invoke the function which was generated using this method, we get a syntax error. it is to be expected, because a stack trace is not a valid javascript code, but an `Array` of code positions

![](https://blog.ikaes.de/static/01a10722-3e9d-7017-bfe9-5d5984187ccf.png)

in order to make the payload work, we need to control the output of the `Array.prototype.toString()` function such that it returns our desired javascript payload. this can be done in two ways

first we can simply overwrite it with a constant function which returns the payload

![](https://blog.ikaes.de/static/01a10724-7465-757c-add0-cc6b60390c92.png)

or we can use prototype pollution to sneak our payload into another function, and then overwrite the `Array` prototype. i found that we can stay in the `Error` realm and use its `toString` function which can be manipulated by polluting the `Object.prototype.message` variable. i am sure there are more ways to achieve this.

![](https://blog.ikaes.de/static/01a10726-a6f6-723f-9bb0-921d08f5d629.png)

now if we put everything together - the function generation using the `prepareStackTrace` function and way to convert arrays into arbitrary strings, we can generate arbitrary javascript functions

![](https://blog.ikaes.de/static/01a10728-85b1-7319-aa14-a5ffea0324a0.png)

last thing we have to do is to invoke our new function. luckily there are [many ways to do so](https://stackoverflow.com/questions/35949554/how-could-it-be-possible-to-invoke-a-function-without-using-parentheses) and i chose the `valueOf` trick. it works by overwriting the default `valueOf` function of our object and then invoking it as a primitive. alternatively, we can use `toString` and then concatenate the object with a string.

![](https://blog.ikaes.de/static/01a1072b-aab4-74f3-89ef-968ca7f7a062.png)

the final payload is as follows. feel free to try it out in the browser console.

```
Object.prototype.message='alert\x281337\x29';Array.prototype.toString=Error.prototype.toString;Error.prepareStackTrace = Function;const x = new Error;x.valueOf=x.stack;+x;
```
