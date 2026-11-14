---
title: Decompose Run-On Tests
link: https://newsletter.kentbeck.com/p/decompose-run-on-tests
source: tidyfirst-substack-com
published: 2026-10-13T13:31:10Z
updated: 2026-10-13T13:31:10Z
first_seen: 2026-11-14T02:15:15.124110058Z
authors:
- Kent Beck
summary: Run-On Tests
content: extracted
html: 2026-10-13-decompose-run-on-tests.html
remote_preview:
  url: https://substackcdn.com/image/fetch/$s_!glPN!,f_auto,q_auto:best,fl_progressive:steep/https%3A%2F%2Ftidyfirst.substack.com%2Ftwitter%2Fsubscribe-card.jpg%3Fv%3D1077263058%26version%3D9
---

*Originally posted March 2013*

> A RUN-ON SENTENCE has at least two parts, either one of which can stand by itself.\
> — [Guide to Grammar and Writing](http://grammar.ccc.commnet.edu/grammar/runons.htm)

Yesterday, a student brought me a test that looked like this:

```
set up
assert
operation
assert
operation
assert
operation
assert
```

I call this a *run-on test* (at least I do now that I’m writing this). I’ve written a few myself, so I know what I do about them.

The problem with run-on tests is that, when they fail, you don’t know what’s wrong (the “specific” property from the [Test Desiderata](https://testdesiderata.com/)). If one of the later assertions fails, in particular, you aren’t sure whether the culprit is the immediately preceding operation or an earlier operation that left the world in a wonky state. A failed run-on leaves you with a debugging problem.

I think I write run-ons when I am exploring. “I’d like an object like this.” Done. “Oh, I see, it needs an operation like that.” Done. “But that means it could have another operation like this.” Done. The result is the kind of test my student handed me.

When a design comes to me as an algebra (things and operations that compose) I don’t get run-ons. I write the tests and the code in little pieces. When every step is a revelation, though, the natural (in the sense of “natural law”) place to put the next bit of testing is with the largest existing bit of testing. This accounts for the power law distributions seen in test suites, in any case.

There’s nothing wrong with coding in discovery mode. It’s a useful skill to be able to make progress in a dark room without breaking anything. However, it’s not okay to leave the relics of past ignorance around to trip colleagues.

**1\. Don’t lose coverage, so make a copy of the test.** In the copy, delete everything after the first assertion:

```
set up
assert
```

Give this test a helpful name. Put yourself in the place of a reader. This test has just failed. What do you want to see in the test runner?

- **Bad:** `testInitialization`. Initialization of what? How could that go wrong?

- **Better:** `initializationProducesConsistentExperiments`. How could they be inconsistent?

- **Best:** `initializationProducesConsistentNamesAndExperiments`. Now I know where to look for the problem.

Names for tests don’t have to satisfy the same constraints as names for other functions. You only have to type the test name once and you never have to format it in calling code. When it’s read, though, the only context is the test failure. Put this together and it’s proper to give tests long, explanatory names (I learned this from BDD).

**2\. Copy the test again.** Delete the first assert and everything after the second assert:

```
set up
operation
assert
```

Is any of the set up irrelevant for this assertion? If so, delete it. Again, name the test what you want it to be named when you read it in a test failure.

**3\. Repeat until you get to the last assert.** At every stage, remove redundant preceding operations. If you see paths that need to be covered, make a note (“how about `opB(); opA();`?”) and write those tests when you’re done decomposing the original run-on. Now you have:

```
set up
assert
```

```
set up
operation
assert
```

```
set up
operation
operation
assert
```

```
set up
operation
operation
operation
assert
```

**4\. Delete the original test.** This is the fun part of the process for me.

You are left with a bunch of short tests, each of which is named after what could go wrong. Explanations in assertions would be redundant at this point, so I don’t write them. You can also extract the set up to a proper `setUp` function.

The suite resulting from this process has exactly the same coverage as the original run-on, but only because you commit to running the tests together and keeping them all working together. This took me a while to get and often surprises students.

The later tests lack all the assertions of the earlier tests. Deleting those assertions feels weird at first, but it’s okay. If something goes wrong, another test will break. Learning to trust your suite as a whole helps you simplify tests (the “composeable” property from the desiderata).

**Most teams don’t have a strategy problem. They have an adaptation problem.**\
Your plan was never going to survive contact with reality. The question is whether your organization bends or breaks when it doesn’t.

I help teams bend. *Adapt to Thrive.*

Booking a handful of **custom talks** and **advisory engagements** now. I interview your people, measure your real software flows, and hand you the truth plus what to do about it.

Curious whether it fits? [Tell me about your team.](https://kentbeck.com/kent-beck-sponsorship#inquire)
