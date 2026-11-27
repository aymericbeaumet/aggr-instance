---
title: Asserting Implications
link: https://tigerbeetle.com/blog/2025-05-26-asserting-implications
source: tigerbeetle-com
published: 2025-05-26T00:00:00Z
updated: 2025-05-26T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- matklad
summary: A short one today!
content: extracted
html: 2025-05-26-asserting-implications.html
remote_preview:
  url: https://tigerbeetle.com/blog/2025-05-26-asserting-implications/banner-6a69b893.webp
---

A short one today!

When using assertions heavily, a common pattern is asserting an implication:

```
assert(a implies b);
```

Most programming languages don’t have special syntax for implication, as you don’t often *branch* based on implications. But in my experience, you want to *assert* implications all the time!

Recall that `and`, `or`, and `not` logical operators form a basis, so an implication can be expressed in terms of disjunction and negation:

A⇒B ⇔ ¬A∨B

This tautology is how asserting that `a` implies `b` gets expressed by default:

```
assert(!a or b);
```

I find this form hard to read, and suggest using the `if` instead:

```
if (a) assert(b);
```

From a recent code change:

```
// Before:
assert(header_b != null or replica.commit_min == replica.op_checkpoint);

// After:
if (header_b == null) assert(replica.commit_min == replica.op_checkpoint);
```
