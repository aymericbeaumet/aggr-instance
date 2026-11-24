---
title: Design for Understandability
link: https://jschuster.org/blog/2026/11/19/design-for-understandability/
source: lobste-rs
published: 2026-11-19T13:07:34Z
updated: 2026-11-19T13:07:34Z
first_seen: 2026-11-24T17:30:21.295672362Z
authors:
- jschuster.org by jschuster
labels:
- api
- plt
- practices
- slides
summary: Comments
content: extracted
html: 2026-11-19-design-for-understandability.html
remote_preview:
  url: https://jschuster.org/blog/wp-content/uploads/2026/10/page_6.png
---

*This is a talk I gave at Google’s internal Code Health Summit in November of 2025. I’m sharing it here in the hopes that it sparks further learning and discussion.*

## Abstract

In a world where LLMs generate much of our code, understanding that code is more important than ever. But with a system as large and complex as Google’s codebase, SWEs can easily fall into a pattern of learned helplessness where the best you can do is tweak the code and pray that it passes the tests. Without understanding, LLMs are little more than complexity-generating machines running at top speed.

There’s a better way to go about this. Just as our industry has learned to design software for testability, we can also design it for understandability. We can use ideas like design by contract, class invariants, modularity, and more to create systems that we can understand one piece at a time, rather than piling more onto the big ball of mud and hoping it doesn’t all collapse.

Is it easy? No. But is it worthwhile? Absolutely. These ideas aren’t new, but they’re not nearly as widely taught or well understood as they deserve to be, and mastering them leads to a world where we can have more confidence in our code and increase velocity at the same time.

## Slides and Transcript

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_1.png)

Hi everyone, thanks for having me! Let’s get started.

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_2.png)

I’d like to start this talk with a question: If LLMs can code, then what is left for human SWEs?

The tech industry is quickly looking to large language models to write all of our code. But you might say: isn’t writing code our job as software engineers? Where does this leave the responsibility of SWEs? What is the essential human element that SWEs add on top of LLMs?

Every one of you is going to have a different answer to this question, and that’s fine. But here’s my take.

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_3-1.png)

That necessary human element is judgment. Judgment of whether the code is maintainable. Judgment of whether it is healthy.

LLMs can follow patterns and generate code that looks like what it was trained on. Sometimes they can produce useful tips and critiques for a piece of code. But that critical piece of judgment, of deciding whether some code is suitable for the task it was designed for, is still up to you, the human.

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_4.png)

Because here’s the thing: LLMs today are still a sloppy intern. Sometimes the code they produce is good, but sometimes it’s completely off-track and needs correcting. You wouldn’t incorporate a sloppy intern’s code into your system without a close review first, so you shouldn’t do so for an LLM’s code either.

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_5.png)

YOU are responsible for the quality of your code, regardless of whether you had an LLM generate it for you or wrote it the old-fashioned way with fingers on a keyboard

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_6.png)

So where does that leave us? If LLMs are going to write a lot of the code for us, then we’re looking at a future where we humans are going to spend less time writing code, and more time reading code and understanding it, in order to make sure it passes our quality bar.

This means it’s more important than ever to get better at understanding code, and to speed up that reading and understanding process.

Now: if you want to get better at something, it’s helpful to set an aspirational goal for yourself. Something that’s above your current skill level, and drives you to get better and better every day.

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_7.png)

Here’s the goal I propose: we should be able to predict the behavior of every line of code, without running a single test.

Let me say that again: We should be able to predict the behavior of every line of code, without running a single test

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_8.png)

Now, I know you may be thinking: “Understand every line of code? Without testing? Are you serious?! There’s way too much complexity and legacy code for me to ever be able to do that!”

You might think so. And if we take our hands off the wheel and relinquish all control, you might be right. But I’m here to tell you there’s a better way.

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_9.png)

We can *design* our code to be understandable.

Consider that a decade or two ago, we used to think that writing multiple tests for every public method in an API would have been infeasible. It wasn’t because we didn’t want to, or that we weren’t trying. It was because we hadn’t designed our code to make testing possible at that level. But gradually, across the software industry, we changed our practices to design for testability. Today, designing our code for testability is so ingrained into our culture that doing anything else is practically unthinkable. We can do the same for understandability, so that our code enables us to understand it better.

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_10.png)

Our own Code Health guide makes a strong case for understandability. It says “For code to be sustainable, it must be understood. Change cannot be made unless a developer knows what the system currently provides, and what needs to change to adapt to new circumstances. As a result, key components of code health include good documentation, easy to understand error messages, and clear, simple code”

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_11.png)

Not only is designing for understandability important, but it’s a learnable skill. It’s not some magical ability that you either have or you don’t. There are specific, concrete actions you can take to improve the understandability of your code.

I’m going to introduce you to a few of those today. Think of it as a “starter kit” for designing for understandability. We’ll also talk more about a general theme that runs through all of these practices, so that at a certain point you can find even more ways to design your code for understandability.

The ideas I’m going to show you today are not new. But they’re not taught as well as they should be, when they’re taught at all, and most of us have only a rough understanding of how to apply them. My goal today is to sharpen that understanding just a little bit, and make it just a little more practical, so we can kickstart a process of making our code more understandable over time.

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_12.png)

So let’s get started on each of these Design for Understandability practices.

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_13.png)

The first one is Design by Contract.

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_14.png)

To motivate the idea, let’s ask the question: when calling a method, what behavior should you rely on?

Let’s say we have this binary search method, but its documentation is not terribly helpful (you’ve probably seen something like this before).

Binary search is reasonably well understood, but as a caller of the method, we have some questions we want to ask about this specific implementation. Such as:

- Will the returned index always be the first occurrence?
- What happens if t is not found?
- What happens if the list is not sorted?

The documentation doesn’t answer these questions. So what can we do?

First, we could guess. But we’ll probably be wrong.

Second, we could read the code. But that might take a long time, especially if the particular behavior we care about is buried several levels deep.

Third, we could test the code on some examples and see what happens. But just because it works a certain way on some examples doesn’t necessarily mean it works that way for all such inputs. Furthermore, even if it works that way now, there’s no guarantee that it will continue to work that way in the future.

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_15.png)

The point is that without a clear statement of what this code is intended to do, there’s no distinction between implementation and intent. The SWE who wants to use this code has to guess at which behavior the method *commits* to, versus which behavior is an implementation detail that might change over time.

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_16.png)

This is where contracts come in.

There are many ways to define a contract, but for the sake of this talk I’m going to define it as a formal-ish description of the observable behavior that each participant in an interaction commits to.

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_17.png)

In the real world, a basic contract is something like “If I cook dinner, then you will wash the dishes”.

In programming, an example might be: “If the caller provides a non-negative integer n, then the square-root function returns the square root of n”

Let’s see how contracts can help us with our binary-search example.

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_18.png)

Here’s a possible contract for this function. It says, “If the list is sorted, binarySearch returns the index of the first occurrence of t in the list, or -1 if not found. Behavior is undefined for unsorted lists.”

We can see that this contract answers the questions we asked about it:

- Will the returned index always be the first occurrence? Yes, the contract explicitly says this.
- What happens if t is not found? The contract says binarySearch will return -1 to signal that case.
- What happens if the list is not sorted? The contract doesn’t say anything, so this behavior is intentionally left undefined. And that’s a valid design choice! There may be performance reasons to not check whether the list is sorted at run-time. So this contract tells the user that they shouldn’t rely on any particular behavior if the list is not sorted. Instead, the obligation is on the caller to check whether the list is sorted before calling binarySearch.

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_19.png)

The power of a contract is that it defines a clear boundary between the author’s intent and the implementation. The caller doesn’t have to guess at the behavior anymore: they can just read the contract, and if a specific detail of the behavior isn’t there, they will know that it’s unspecified.

In reality, this isn’t always perfect. Whether intentionally or accidentally, we still tend to end up with code that relies on implementation details rather than specified behavior. But contracts at least give us a way to minimize this behavior.

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_22.png)

Now that we understand what a contract looks like, let’s talk about how contracts can help us reason about and understand our code better.

Suppose you just wrote a method. I’m going to draw that method as a little circle here.

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_23.png)

You want to know if it’s correct.

I’m going to use a purple circle to stand for methods we’ve read the code for

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_24.png)

Now, our method doesn’t live in isolation: it calls some other code.

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_25.png)

So, you think “Okay, well, I need to understand what those do. I’m going to read the code for those methods.”

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_26.png)

Well, those methods call other methods, and we start thinking about this, and figure out how we’re going to reason about those. And at this point you can maybe see where I’m going.

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_27.png)

Oh no. Now we have lots and lots of code we have to read across our entire code base.

![](https://jschuster.org/blog/wp-content/uploads/2026/10/Design-for-Understandability-public-blog-post-version-1.png)

Why do we need all this reasoning just to reason about one single method? That’s all we’re trying to do! How did we get so out of control?

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_29.png)

So, let’s back up and try again, using Design by Contract this time.

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_30.png)

You still have to read your own code. We can’t get around this part; that’s the change we just made.

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_31.png)

And we haven’t changed the code. It still calls those other methods.

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_32.png)

But now, the trick is that we can read just the *contract* on those called methods, not the code.

And that’s it! We can stop here. We don’t need to worry about all of that other code. The contracts tell us all we need to know about calling these other two methods.

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_33.png)

The rest of that code still exists, but we don’t have to think about it. The contract abstracts away all that other behavior that doesn’t matter to us as the caller of these two methods.

![](https://jschuster.org/blog/wp-content/uploads/2026/10/Design-for-Understandability-public-blog-post-version-2.png)

So that is the power that contracts give to us to reason about our code.

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_35.png)

Finally, I’ll wrap up this section with a few tips for writing contracts:

First, document the behavior that you’re willing to commit to. This is usually the same as what you want your callers to rely on. For example, you probably want to document the return value of your method. But the exact human-readable text in your error messages? Not so much.

Second, describe your error handling in your contracts. A contract does not and should not be just about the happy path. If your method might go wrong in some way, tell the caller under what conditions it might go wrong, and how you’re going to signal that to the caller, whether it be through a special return value, or an exception, or something else.

Finally, keep in mind that the more callers your method has, the harder it is to change the contract. If it’s a private method used only by your team? Sure, that’s probably an easy case to change both the contract and change the callers to match. If it’s a public method on a popular framework used by hundreds of other projects? Then renegotiating the contract is a much more expensive effort, so you should evaluate the costs and benefits carefully before deciding to do that.

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_36.png)

So that was the first element in our Design for Understandability starter kit.

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_37.png)

The next one is data invariants.

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_38.png)

Let me explain this idea with a little story.

Early in the COVID-19 pandemic, when my wife and I started working from home, we missed having free soda at work. So we got in the habit of keeping cans of Diet Coke in the fridge.

![](https://jschuster.org/blog/wp-content/uploads/2026/10/Design-for-Understandability-public-blog-post-version-3.png)

But there was a problem: we were great at taking cans out of the fridge, but not so great about putting new ones back in. So if you’re working hard and looking forward to taking a break with a nice cold beverage, it was a big disappointment to open a Coke-less fridge and resort to drinking room-temperature soda from the pantry instead.

![](https://jschuster.org/blog/wp-content/uploads/2026/10/Design-for-Understandability-public-blog-post-version-4.png)

The problem was that we hadn’t set any rules on how we retrieve Diet Coke and how we replenish the fridge.  Without any rules governing access to the fridge, we eventually got into an unwanted state: in this case, an empty fridge.

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_41.png)

This happens in code, too. For example, let’s say you have this proto to represent a customer’s order \[Note: “proto” is Google jargon for a Protocol Buffers message, similar to a JSON object\]. If you receive this proto, how much do you trust that the total actually indicates the total amount on the items?

Probably not much, right? This proto could have come from any part of your code, so it’s easy to imagine that someone forgot to set that field appropriately.

Protos give you completely uncontrolled access to the underlying representation of your data. Protos are great for transferring data back and forth across the network, but they’re poorly suited to situations where you want to rule out certain states.

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_42.png)

To solve these kinds of problems, we can use data invariants. A data invariant is a statement about a piece of data that is always true.

Data invariants are a principled way to enforce these kinds of valid states in your data. If you use them well, you can avoid a lot of bugs in your code.

Let’s see how we can use invariants to solve the Diet Coke problem, and similar problems you might come across in your own code

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_43.png)

First, identify the invariant you want to maintain.

Ask yourself: What’s the property you want to rely on for this piece of data? Where are the places in the code where you explicitly validate and return an error if invalid? Alternatively, where do you make the assumption that it’s invalid, but you’re not actually sure?

In this case, the invariant we want is “the fridge always contains a Diet Coke”.

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_44.png)

Second, you need to document the invariant.

It’s important to note this is a separate step. It’s one thing to have an invariant in your own head. It’s another thing to write it down so that you communicate it to everyone else who might touch this code. Not to mention that if you come back to this code months or years later, you yourself might have forgotten which exact invariant you meant to maintain.

Write it down where people will see it: probably close to the relevant data members, or maybe in a class-level comment at the top.

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_45.png)

Third, establish the invariant at creation time. Usually this will be done in a constructor or factory method. In this case, we’re constructing the fridge with a few drinks already added.

[![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_46.png)](https://jschuster.org/blog/wp-content/uploads/2026/10/page_46.png)

Fourth, every time you change your the data, make sure you maintain the invariant.

This is the part my wife and I didn’t do for our fridge. What we should have done is established a rule where if you take a Diet Coke, you put another one back in the fridge to make sure there’s another for the next person.

You can maintain the invariant many ways – that part is up to you. The important thing is that you always maintain it. Without that, you have to think to yourself “Wait, are there going to be Diet Cokes in the fridge? Did I already take them out earlier? Let me go back and check all my callers to see if they did that already”. With an invariant, though, you just know: If I have a Fridge object, then it definitely has Diet Cokes, regardless of the sequence of operations that led to this point.

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_47.png)

Finally, one last step, and this might be the hardest one: you need to maintain the invariant as the code evolves. It’s relatively easy to maintain the invariant for a single version of code. But over the weeks, months, and years, complexity will creep in, new requirements will force you to change the code, and you might accidentally break the invariant or forget you even had one.

Establish the culture on your team that maintaining invariants is just a thing you do. If you have a mental checklist of things you look for in code review, make sure that maintaining the invariants is on that checklist.

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_48.png)

So how do data invariants help us design for understandability? Let’s first consider what we have to do when we don’t have an invariant.

Recall this representation of all the methods in your codebase, calling each other in various ways. Suppose you have a proto-based version of the fridge, and you want to know if it has any Diet Cokes in it. Because it’s a proto, you don’t have a way of knowing what state it’s in.

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_49.png)

So, you need to check all the callers of the current method.

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_50.png)

And you do the check, crawling across the codebase, and you find that there’s one particular case where you didn’t enforce it.

Of course you can fix these issues one at a time as they crop up, but all that searching sounds like a lot of work, right? What do you think are the chances you’re always going to find that one particular buggy spot? What are the chances that new methods added to this web of code are going to maintain that condition?

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_51.png)

In contrast, let’s try the version with our Fridge class and its invariant.

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_52.png)

In this case, the class has an invariant: the Fridge always has at least one Diet Coke, no matter what operations have been done previously or where we came from. By maintaining that invariant, we’ve removed the need to check what every single caller does.

This is what data invariants do for you: they let you focus on the state you’re in now, and not how you got there.

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_53.png)

That wraps up the section on data invariants.

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_54.png)

The final practice I want to discuss today is isolating side effects and dependencies on global state.

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_55.png)

To motivate this idea with an example. Let’s say there’s a supermarket point-of-sales system with this method to compute sales tax. Now ask yourself: what are the inputs and outputs for this method?

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_56.png)

Seems pretty obvious, right? It takes in a customer’s grocery order and returns the sales tax as an integer.

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_57.png)

But that’s not the whole story. If you dig into the implementation of this method, you’ll find that it reads in a set of sales tax rates from a separate file. That’s effectively a new input channel! The code depends on that data to compute the tax just as much as it depends on the content of the grocery order itself.

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_58.png)

And if you look further, you’ll find that the method also records the computed tax in the store’s account database for bookkeeping purposes. That’s a new output, but it’s nowhere to be found in the type signature!

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_59.png)

So functions have two kinds of input and output: the ones that are visible in the type signature, like the order and the computed sales tax, and the ones that are hidden, like the tax rates file and the entry in the accounting database.

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_60.png)

The hidden outputs are more commonly known as side effects. Side effects generally come in two flavors: mutation, so modifying something like a global variable or one of your input arguments, and outputs outside of your process. That would be things like database writes, remote procedure calls, printing to the console, etc.

I don’t know a common term for the hidden inputs, but I would call these dependencies on global state. Again, there are two flavors: dependency on the state of some global variable, or a dependency on the state of the system your program is running in. That’s things like permissions to access the database, the existence of a given file on the file system, and even the ability to read the system clock.

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_61.png)

So why are these hidden inputs and outputs a problem? They make code hard to test, hard to reuse, and hard to reason about.

Hard to test because you have to make sure you have all the necessary dependencies are set up before you can even run the method in your test. And then once you do run the method, side effects often force the test to rely more on interaction verification than state verification, which can be brittle.

Hard to reuse because the code now makes assumptions about what kinds of resources you have available to you. If you need to reuse some method in another part of the codebase, but the input data needs to come from a different source, or you need to store the results in a different place, those side effects and global states are going to create a tight coupling between your computation code and the specific sources and destinations for that computation, which will make your life harder.

Hard to reason about because, by definition, these inputs and outputs are hidden! If you don’t even know that your method is making these kinds of assumptions on the system that it’s running in, then you’re going to make some faulty conclusions about how your code will behave.

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_62.png)

Now I’m not saying all side effects or global state are bad. And I’m not saying we should all immediately switch to a pure functional programming language like Haskell.

Effects have to happen somewhere. Without side effects, computation is just the process of making a metal box hot for a little while\
(and even that heat is a side effect! although not usually one we care about).

But by default, we tend to sprinkle them everywhere throughout the code. Need some input? Just read the file at whatever point you happen to be at in the code. Need to store your result? Go ahead and do it 4 layers deep into your business logic.

Without a principled approach to side effects and global state, we’re constantly running into the kinds of problems we just described.

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_63.png)

There’s a better way, which is to isolate the side effects and the dependencies on global state. Much of our code is just logic on top of the data (once we get it) and creating new data based on that. Divide your code into the computational parts on one hand, and the parts that deal with side effects and global state on the other.

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_64.png)

There are a few common patterns for this.

One of them is Command/Query Separation. It’s a pattern where each method is either a command, which performs some side effect, or a query, which is a pure function that returns its output as the return value. But you maintain a strict separation so that no function does both of those things.

Another pattern is called “Functional Core, Imperative Shell”. This is a pattern at the architectural level. You focus the inner modules of your system on the business logic and computation, not performing side effects – this is the “functional core”. The outer modules coordinate the effects of your system with that computation, so that the complexity of the side effects is isolated to just that outer layer, and the majority of your system is simpler due to the functional purity.

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_65.png)

As with the other two practices, side effects and global state have a big impact on how we think about code.

For example, you might be working on a method and need to delete a file. Your assumption is that it should be okay, you think no one is using it.

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_66.png)

But then it turns out, some other part of the codebase was relying on it, and you just broke them. The code was so far away and unrelated that you didn’t even think to check there.

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_67.png)

On the other hand, if you isolate your side effects and global state such that they only happen in a known region of the code, then you can have confidence that you really aren’t going to break anyone else.

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_68.png)

So, those are the three starter practices for Design for Understandability: Design by Contract, Data Invariants, and Isolating Side Effects and Dependencies on Global State.

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_69.png)

Now, I’ve presented these as independent practices, but in fact they’re not so distinct. There’s a unifying theme to them. That theme is local reasoning.

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_70.png)

What do I mean by local reasoning?

Local reasoning means you can come to conclusions about some chunk of code by looking only at that chunk of code, plus just a small amount of the neighboring code, like the specifications or contracts of the components that you directly interact with.

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_71.png)

This is as opposed to global reasoning. That’s the kind of reasoning where to answer even a simple question, you have to read a large chunk of the codebase, because the answer could be in any method, at any level of the call stack.

The benefit of local reasoning is that it puts a hard boundary on how far you have to look in order to figure something out about your system: there’s always a limit to how far you have to search.

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_72.png)

So let’s talk about how each of the practices we talked about enables local reasoning. You may have noticed we covered this already, but let’s review.

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_73.png)

Design by Contract defines the expected behavior for a caller and a called method. That means that to understand what behavior a caller can rely on, or what behavior a called method is obligated to implement, the only place we need to look is the defined interface for that called method. Without that, we would revert to global reasoning to find all the parts of the code that might depend on some aspect of this method’s behavior.

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_74.png)

Data invariants often go hand in hand with contracts, but they usually say something about an entire object rather than a single method on that object.

If you have a piece of data and you want to know something about its validity, like “does this fridge have any diet cokes?”, an invariant is what lets you immediately conclude yes, without looking at the callers that produced that data.

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_75.png)

Finally, by isolating side effects and global state, you minimize the number of places in your code where a change in one part of the code can cause a problem in a completely unrelated part. So it lets you say things like “this is the only part of the code that’s allowed to touch this file, so I know for sure that it’s safe to delete”.

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_76.png)

Another way of looking at local reasoning is to say that it minimizes the amount of context we need in order to reason about the system.

With global reasoning, you might need to know about any arbitrary part of your codebase, because the part that breaks your assumptions could be anywhere. You need to have the whole system loaded into your head in order to understand it.

Local reasoning keeps the amount of stuff you need loaded into your head to a reasonable size.

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_77.png)

And guess what else benefits from small contexts: LLMs!

A key portion of making an LLM useful for your work is giving an LLM the right context to give good suggestions. But contexts costs tokens, and your tokens are limited. So LLMs give us another great motivation to use local reasoning and minimize our context.

In other words, if you want your LLMs to be useful to you, then design your systems for understandability. Keep the necessary context small to make it easier for both you and your LLM to work with your code.

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_78.png)

At the beginning of this talk, I compared “Design for Understandability” to “Design for Testability”. I’d like to revisit that for a moment.

Before designing for testability was the norm, the default testing approach was application-level testing. Our code wasn’t set up for practices like dependency injection, so there was no way to run your code without setting up a huge set of dependencies as well. It wasn’t until we started designing and building our systems with testability in mind that we were able to make unit testing practical.

That’s how it is with understandability and local reasoning, as well. If you don’t design for it ahead of time, it’s somewhere between hard and impossible to use local reasoning. But if you design for it along the way, you can make it work.

If you’ve never heard of unit testing, then application-level testing seems obvious, and straightforward. But it’s not sufficient for modern software engineering. The same is true for global reasoning. I say it’s time we made designing for understandability a required part of our work, just like we did for testability.

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_79.png)

So you may be thinking: that all sounds good, and these practices sound good on paper, but is designing for understandability feasible in practice? You’re probably not working in a brand-new greenfield project. You’re probably working on old code, with lots of old warts and complexity, and the situation may seem hopeless. Can you really do this?

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_80.png)

I’m here to say yes, this is feasible, if you take it one small step at a time. I’ll share some tips for getting started.

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_81.png)

First off, take little bits at a time. Don’t try to boil the ocean. Don’t say “We’re going to take two weeks off from our normal work, make all of our code perfectly understandable, and then we’re done!”. That’s not going to work; you’re going to have a bad time.

Instead, try applying some of these practices to just new methods that you write. You might have some old code out there that’s hard to change. So be it. Start with just the new methods and build some momentum.

Teach your teammates, as well. Maybe share this talk, or find other ways to help your teammates design for understandability.

And finally, you might consider group programming sessions where you all get together and discuss: what does designing for understandability mean in your context? The Monarch team at Google did this with their Design by Contract practice and it worked well for them.

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_83.png)

Here are some further resources to learn more about Design for Understandability. I’m not going to go into them in detail, but please feel free to go revisit the slides after the talk and check these out.

\[Public links reproduced here for the blog post:\]

- [Intellectual Control](https://ieeexplore.ieee.org/document/8611447)
- [The Humble Programmer](https://www.cs.utexas.edu/~EWD/transcriptions/EWD03xx/EWD340.html)
- [Parse, Don’t Validate](https://lexi-lambda.github.io/blog/2019/11/05/parse-don-t-validate/)
- [Functional Core, Imperative Shell](https://www.destroyallsoftware.com/screencasts/catalog/functional-core-imperative-shell)
- [The Power of Composition](https://fsharpforfunandprofit.com/composition)

![](https://jschuster.org/blog/wp-content/uploads/2026/10/page_84.png)

In conclusion: design your code for understandability. Whenever you’re working on some code and realize that you’re falling back to global reasoning, pause for a moment. Ask yourself: can I do this with local reasoning instead? Would a contract or an invariant help here? Or what if I moved the side effects and global state somewhere else? Or, maybe you’ll think of some other practice that fits here – you probably know a lot of local-reasoning practices, even if you didn’t have the terminology to call it that until today.

It’s time to make design for understandability just as much a part of software engineering as designing for testability is today. Understandability is not just helpful – it’s crucial, especially with Google-size systems. And not just that, but making your code understandable is doable if you design for it along the way.

Trust me: you’ll be glad you did.
