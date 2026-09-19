---
title: Private Methods in JavaScript
link: https://andrewkelley.me/post/js-private-methods.html
source: andrewkelley-me
published: 2013-07-17T02:20:54Z
updated: 2013-07-17T02:20:54Z
first_seen: 2026-09-19T21:30:23.395188495Z
summary: 'Private Methods in JavaScript In JavaScript, we don''t have private methods right? We must to resort to using this._somePrivateThing() right? Wrong. Before function Cell(x, y) { this.x = x; this.y = y; this.things = [1, 2, 3]; // I guess I''ll have to use an underscore to indicate that this // method is private. this._initializeSomethingElse(); } Cell.prototype._initializeSomethingElse = function() { this.dir = Math.atan2(this.y, this.x); this.total = 0; // hmm, I need a reference to this in that callback. I''ll have to // save a copy or use bind var self = this; self.things.forEach(function(thing) { self.total += thing; }); }; After function Cell(x, y) { this.x = x; this.y = y; this.things = [1, 2, 3]; // boom. private method. initializeSomethingElse(this); } function initializeSomethingElse(self) { self.dir = Math.atan2(self.y, self.x); self.total = 0; self.things.forEach(function(thing) { self.total += thing; }); } Notice that as an added benefit, the new private method is given an explicit reference to the instance, so if you need to use a callback you don''t have to shuffle around the this pointer. "But it will fool the optimizer!" Wrong again. Let''s benchmark the above examples: var PrivCell = require(''./priv''); var NoPrivCell = require(''./no-priv''); console.log("Test 1 - no priv method:", Math.round(test(NoPrivCell)) + "ms"); console.log("Test 1 - private method:", Math.round(test(PrivCell)) + "ms"); console.log("Test 2 - no priv method:", Math.round(test(NoPrivCell)) + "ms"); console.log("Test 2 - private method:", Math.round(test(PrivCell)) + "ms"); console.log("Test 3 - no priv method:", Math.round(test(NoPrivCell)) + "ms"); console.log("Test 3 - private method:", Math.round(test(PrivCell)) + "ms"); function test(Cell) { var start = new Date(); var total = 0; for (var i = 0; i < 20000000; i += 1) { var c = new Cell(); total += c.total; } return new Date() - start; } Running the benchmark on my machine: Test 1 - no priv method: 5525ms Test 1 - private method: 5537ms Test 2 - no priv method: 5537ms Test 2 - private method: 5571ms Test 3 - no priv method: 5572ms Test 3 - private method: 5595ms Makes no difference. It''s clean, it solves the problem, and it has no performance implications. Here''s a jsperf for further evidence. Browser Scoping Note that if you''re writing the code in an environment which does not provide scoping, such as a <script> tag in the browser, you''ll want to wrap the entire thing in an anonymous function call: var Cell = (function() { function Cell(x, y) { this.x = x; this.y = y; this.things = [1, 2, 3]; // boom. private method. initializeSomethingElse(this); } function initializeSomethingElse(self) { self.dir = Math.atan2(self.y, self.x); self.total = 0; self.things.forEach(function(thing) { self.total += thing; }); } return Cell; })();'
content: extracted
html: 2013-07-17-private-methods-in-javascript.html
---

In JavaScript, we don't have private methods right? We must to resort to using `this._somePrivateThing()` right?

Wrong.

## Before

```
function Cell(x, y) {
  this.x = x;
  this.y = y;
  this.things = [1, 2, 3];

  // I guess I'll have to use an underscore to indicate that this
  // method is private.
  this._initializeSomethingElse();
}

Cell.prototype._initializeSomethingElse = function() {
  this.dir = Math.atan2(this.y, this.x);
  this.total = 0;
  // hmm, I need a reference to this in that callback. I'll have to
  // save a copy or use bind
  var self = this;
  self.things.forEach(function(thing) {
    self.total += thing;
  });
};
```

## After

```
function Cell(x, y) {
  this.x = x;
  this.y = y;
  this.things = [1, 2, 3];

  // boom. private method.
  initializeSomethingElse(this);
}

function initializeSomethingElse(self) {
  self.dir = Math.atan2(self.y, self.x);
  self.total = 0;
  self.things.forEach(function(thing) {
    self.total += thing;
  });
}
```

Notice that as an added benefit, the new private method is given an explicit reference to the instance, so if you need to use a callback you don't have to shuffle around the `this` pointer.

## "But it will fool the optimizer!"

Wrong again.

Let's benchmark the above examples:

```
var PrivCell = require('./priv');
var NoPrivCell = require('./no-priv');

console.log("Test 1 - no priv method:", Math.round(test(NoPrivCell)) + "ms");
console.log("Test 1 - private method:", Math.round(test(PrivCell)) + "ms");

console.log("Test 2 - no priv method:", Math.round(test(NoPrivCell)) + "ms");
console.log("Test 2 - private method:", Math.round(test(PrivCell)) + "ms");

console.log("Test 3 - no priv method:", Math.round(test(NoPrivCell)) + "ms");
console.log("Test 3 - private method:", Math.round(test(PrivCell)) + "ms");

function test(Cell) {
  var start = new Date();
  var total = 0;
  for (var i = 0; i < 20000000; i += 1) {
    var c = new Cell();
    total += c.total;
  }
  return new Date() - start;
}
```

Running the benchmark on my machine:

```
Test 1 - no priv method: 5525ms
Test 1 - private method: 5537ms
Test 2 - no priv method: 5537ms
Test 2 - private method: 5571ms
Test 3 - no priv method: 5572ms
Test 3 - private method: 5595ms
```

Makes no difference. It's clean, it solves the problem, and it has no performance implications.

Here's a [jsperf](http://jsperf.com/public-vs-private-methods) for further evidence.

## Browser Scoping

Note that if you're writing the code in an environment which does not provide scoping, such as a \<script> tag in the browser, you'll want to wrap the entire thing in an anonymous function call:

```
var Cell = (function() {
  function Cell(x, y) {
    this.x = x;
    this.y = y;
    this.things = [1, 2, 3];

    // boom. private method.
    initializeSomethingElse(this);
  }

  function initializeSomethingElse(self) {
    self.dir = Math.atan2(self.y, self.x);
    self.total = 0;
    self.things.forEach(function(thing) {
      self.total += thing;
    });
  }

  return Cell;
})();
```

Thanks for reading my blog post.
