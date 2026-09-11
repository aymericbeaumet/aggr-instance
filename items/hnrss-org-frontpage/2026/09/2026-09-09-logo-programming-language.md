---
title: Logo Programming Language
link: https://el.media.mit.edu/logo-foundation/what_is_logo/logo_programming.html
source: hnrss-org-frontpage
published: 2026-09-09T07:07:26Z
updated: 2026-09-09T07:07:26Z
first_seen: 2026-09-11T17:43:44.917817170Z
authors:
- azhenley
summary: 'Article URL: https://el.media.mit.edu/logo-foundation/what_is_logo/logo_programming.html Comments URL: https://news.ycombinator.com/item?id=49622406 Points: 128 # Comments: 64'
content: extracted
html: 2026-09-09-logo-programming-language.html
---

The Logo Programming Language, a dialect of Lisp, was designed as a tool for learning. Its features - interactivity, modularity, extensibility, flexibility of data types - follow from this goal.

### Interactivity

Although there are some versions of Logo that compile, it is generally implemented as an interpreted language. The interactivity of this approach provides the user with immediate feedback on individual instructions, thus aiding in the debugging and learning process. Error messages are descriptive. For example

fowad

**I don't know how to fowad**

(The word **fowad** is not a primitive - one of Logo's built in words - nor a procedure that you've defined.)

forward

**Not enough inputs to forward**

(Now that you've spelled it correctly, Logo knows the word **forward**, but can't run your instruction because **forward** requires additional information.

forward 100

(Logo is happy. There's no error message. The turtle moves forward 100 steps.)

### Modularity and Extensibility

Logo programs are usually collections of small procedures. Generally, procedures are defined by writing them in a text editor. The special word **to** is followed by the name of the procedure. Subsequent lines form the procedure definition. The word **end** signals that you're finished.

In our [turtle graphics](https://el.media.mit.edu/logo-foundation/what_is_logo/logo_primer.html) example we defined a procedure to draw a square

to square\
 repeat 4 \[forward 50 right 90\]\
 end

and used it as a subprocedure of another procedure

to flower\
 repeat 36 \[right 10 square\]\
 end

Similarly, **flower** could be a building block of something larger

to garden\
 repeat 25 \[set-random-position flower\]\
 end

No, **set-random-position** is not a primitive, but **random** is and so is **setposition** (or **setpos** or **setxy**). Or you could write **set-random-position** using **forward** and **right** with **random**.

Once a Logo procedure is defined it works like the Logo primitives. In fact, when you look at Logo programs there's no way of knowing which words are primitives and which are user-defined unless you know that particular Logo implementation. In our [language](https://el.media.mit.edu/logo-foundation/what_is_logo/logo_and_natural_language.html) sample we used the procedure **pick** to randomly select an item from a list, for example in the procedure who.

to who\
 output pick \[Sandy Dale Dana Chris\]\
 end

In some versions of Logo **pick** is a primitive while in others you have to write it yourself. **Who** would look and work the same way in either case.

Logo allows you to build up complex projects in small steps. Programming in Logo is done by adding to its vocabulary, teaching it new words in terms of words it already knows. In this way it's similar to the way people learn spoken language.

### Flexibility

Logo works with words and lists. A Logo word is a string of characters. A Logo list is an ordered collection of words and or lists. Numbers are words, but they're special because you can do things like arithmetic with them.

Many programming languages are pretty strict about wanting to know exactly what kind of data you claim to be using. This makes things easier for the computer, but harder for the programmer. Before adding a couple of numbers you might have to specify whether they are integers or real numbers. The computer needs to know such things. But most people don't think about this so Logo takes care of it for you. When asked to do arithmetic Logo just does it.

print 3 + 4\
 **7**

print 3 / 4\
 **.75**

If you are unfamiliar with Logo but work in other programming languages, the following sequence may surprise you:

print word "apple "sauce\
 **applesauce**

print word "3 "4\
 **34**

print 12 + word "3 "4\
 **46**

Here's a recursive procedure that computes factorials:

to factorial :number\
 if :number = 1 \[output 1\]\
 output :number \* factorial :number - 1\
 end

print factorial 3\
 **6**

print factorial 5\
 **120**

Here's a procedure to reverse a list of words:

to reverse :stuff\
 ifelse equal? count :stuff 1\
 \[output first :stuff\]\
 \[output sentence reverse butfirst :stuff first :stuff\]\
 end

print reverse \[apples and pears\]\
 **pears and apples**

You might also want to take a look at Brian Harvey's interesting [Logo sample.](http://http.cs.berkeley.edu/%7Ebh/logo-sample.html)

### Enhancements

The features just illustrated are common to all versions of Logo. Some Logo implementations include enhanced language features.

There was an object-oriented Logo called Object Logo for the Macintosh.

MicroWorlds Logo includes multi-tasking so that several independent processes may be run simultaneously. The same capability is in the software for Control Lab, a LEGO Logo product. An even more massively parallel Logo is StarLogo.

In a traditional Logo the command to the turtle

repeat 9999 \[forward 1 right 1\]

would take a while to execute. The instruction

repeat 9999 \[forward 1 right 1\] print "HELLO

would cause the word **HELLO** to appear after the turtle was done moving.

In MicroWorlds Logo typing

launch \[repeat 9999 \[forward 1 right 1\]\] print "HELLO

would start the turtle going. The word **HELLO** would appear as soon as the first process is launched. Or

forever \[forward 1 right 1\] print "HELLO

would initiate a process that would continue until you stopped it. Again, the word **HELLO** would appear as soon as the turtle process is initiated.

### Find Out More

To find out more about the Logo programming language look at Brian Harvey's three-volume epic [Computer Science Logo Style](https://el.media.mit.edu/logo-foundation/resources/books.html#CSLS) and Michael Friendly's [Advanced Logo](https://el.media.mit.edu/logo-foundation/resources/books.html#advanced)\
[.](https://el.media.mit.edu/logo-foundation/resources/books.html#advanced)

If you do not have Logo and want to get started, you might want look at our [Logo software page](https://el.media.mit.edu/logo-foundation/resources/software_hardware.html). Or, you can just download [UCBLogo](http://www.eecs.berkeley.edu/%7Ebh/), [MSWLogo](http://www.softronix.com/logo.html), [FMSLogo](http://fmslogo.sourceforge.net/), [StarLogo TNG](https://education.mit.edu/project/starlogo-tng/), or [StarLogo Nova](https://www.slnova.org/) right now. \

A Hindi translation of this article [is available here](http://dealsqueen.co.uk/translations/logoprogramming/).

A Ukrainian translation of this article [is available here](https://fixgerald.com/blog/mova-programuvannia-logo).\
 A Serbo-Croatian translation of this article [is available here](https://studycrumb.com/translations/logo-programski-ezik)
