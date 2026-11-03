---
title: The Kluever Maneuver
link: https://jordi.inversethought.com/blog/the-kluever-maneuver/
source: lobste-rs
published: 2026-10-31T15:05:04Z
updated: 2026-10-31T15:05:04Z
first_seen: 2026-11-03T06:30:38.113210388Z
authors:
- jordi.inversethought.com by JordiGH
labels:
- testing
summary: Comments
content: extracted
html: 2026-10-31-the-kluever-maneuver.html
remote_preview:
  url: https://secure.gravatar.com/avatar/3d7505f1580f996cefb2df0ed70c6f6a3b0a5dec4679ff35ef830dd49a450f33?s=96&d=identicon&r=g
---

In brief, it’s this: whenever you are doing a bugfix, do it in two commits.

1. **Write a passing test that exemplifies the buggy behaviour.**
2. **Fix the bug and also fix the test we just wrote.**

For example,

```python
def add(a, b):
   return a + b + 1
```

and through your keen eye you observe that this addition function is buggy. You therefore write a test that records the bad behaviour,

```diff
Commit 1

diff --git a/test_arith.py b/test_arith.py
--- a/test_arith.py
+++ b/test_arith.py
@@ -0,0 +1,2 @@
+def test_add(a, b):
+   assert add(2, 2) == 5
```

Then comes the bugfix,

```diff
Commit 2

diff --git a/arith.py b/arith.py
--- a/arith.py
+++ b/arith.py
@@ -1,2 +1,2 @@
 def add(a, b):
-   return a + b + 1
+   return a + b
diff --git a/test_arith.py b/test_arith.py
--- a/test_arith.py
+++ b/test_arith.py
@@ -1,2 +1,2 @@
 def test_add(a, b):
-   assert add(2, 2) == 5
+   assert add(2, 2) == 4
```

## BUT Y THO

Three reasons:

1. Clearly shows exactly how the bug currently behaves
2. Demonstrable proof that the bugfix actually fixed the bug
3. CI still passes at every commit

The traditional way to do this is to do a bugfix and add a test in the same commit. But what if the test accidentally already passed before the alleged bugfix and the alleged bugfix didn’t actually work? In that case the added test gives false confidence that the bugfix worked.

## Alright, show me

Here’s a less obvious example.

Suppose we want to make sure users are not underage,

```python
def age_in_years(birthday, today):
    return today.year - birthday.year

def can_access(birthday, today):
    return 18 <= age_in_years(
        birthday,
        today
    )
```

You notice that this age check is buggy. It’s only considering the years. If it’s October 2026, but you were born on December 2008, you are not 18 yet.

To fix it, the problem is just an off-by-one.

```diff
diff --git a/dates.py b/dates.py
--- a/dates.py
+++ b/dates.py
@@ -4,7 +4,7 @@ def age_in_years(birthday, today):
     return today.year - birthday.year

 def can_access(birthday, today):
-    return 18 <= age_in_years(
+    return 18 < age_in_years(
         birthday,
         today
     )
diff --git a/test_date.py b/test_date.py
--- a/test_date.py
+++ b/test_date.py
@@ -0,0 +1,5 @@
+def test_can_access():
+   assert can_access(
+       date(2007, 1, 1),
+       date(2026, 1, 1)
+   )
```

The astute reader will observe the problem isn’t in the `can_access` function but in the age calculation. The bugfix isn’t an actual bugfix and the proposed test actually passes both before and after the proposed fix.

The *Kluever Maneuver* would have prevented this.

We wrongly check that someone born on New Year’s Eve in 2008 is eighteen on New Year’s Day in 2026.

```diff
commit 1

diff --git a/test_date.py b/test_date.py
--- a/test_date.py
+++ b/test_date.py
@@ -0,0 +1,5 @@
+def test_can_access():
+   assert can_access(
+       date(2008, 12, 31),
+       date(2026, 1, 1)
+   )
```

Then we fix the age calculation and flip the test.

```diff
commit 2

diff --git a/dates.py b/dates.py
--- a/dates.py
+++ b/dates.py
@@ -1,7 +1,10 @@
+from dateutil.relativedelta import relativedelta

 def age_in_years(birthday, today):
-    return today.year - birthday.year
+    return relativedelta(
+        today,
+        birthday
+    ).years

 def can_access(birthday, today):
     return 18 <= age_in_years(
diff --git a/test_date.py b/test_date.py
--- a/test_date.py
+++ b/test_date.py
@@ -1,5 +1,5 @@
 def test_can_access():
-   assert test_can_access(
+   assert not test_can_access(
        date(2008, 12, 31),
        date(2026, 1, 1)
    )
```

## Shouldn’t we use `XFAIL` instead?

Okay, first of all, not every testing framework has an `XFAIL` equivalent (sigh, Java…).

Setting that aside, a positive passing test isn’t completely logically equivalent to a positive failing test. Going back to our addition example, if we were to add this test,

```diff
Commit 1

diff --git a/test_arith.py b/test_arith.py
--- a/test_arith.py
+++ b/test_arith.py
@@ -0,0 +1,2 @@
+@pytest.mark.xfail
+def test_add(a, b):
+   assert add(2, 2) == 4
```

This test only tells us that the current addition isn’t four, but it doesn’t tell us exactly what the addition is. Depending on the nature of the bug, a positive failing test might not exemplify as clearly what exactly the bug is. Knowing that two plus two is five might make it easier to understand how to fix the bug.

That being said, using `XFAIL` is in general roughly equivalent. If for some reason we can’t currently fix the bug right now, only record it, an `XFAIL` is better, since we expect we’ll be carrying this around for a longer time.

## What about bug tracking?

Ah, yes, it’s also a good idea to log a bug in your tracker.

Reference the bug in your first commit.

```diff
add a test showing the bug

diff --git a/test_arith.py b/test_arith.py
--- a/test_arith.py
+++ b/test_arith.py
@@ -0,0 +1,2 @@
+FIXME: bug #1729
+def test_add(a, b):
+   assert add(2, 2) == 5
```

Record the fix in the second commit:

```diff
remove off-by-one error

fixes: #1729

diff --git a/arith.py b/arith.py
--- a/arith.py
+++ b/arith.py
@@ -1,2 +1,2 @@
 def add(a, b):
-   return a + b + 1
+   return a + b
diff --git a/test_arith.py b/test_arith.py
--- a/test_arith.py
+++ b/test_arith.py
@@ -1,2 +1,2 @@
-FIXME: bug #1729
 def test_add(a, b):
-   assert add(2, 2) == 5
+   assert add(2, 2) == 4
```

## Okay but why “Kluever Maneuver”?

Oh. Because my colleague [Kurt Kluever](https://github.com/kluever) taught it to me, and I immediately think back to him when I think of this technique. He usualy calls it “terrible tests”, as in “Jordi, can you please write a terrible test for this?” and off I go to write a test asserting something I know to be wrong.
