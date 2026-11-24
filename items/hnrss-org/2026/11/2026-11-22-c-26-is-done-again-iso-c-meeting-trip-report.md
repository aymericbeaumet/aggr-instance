---
title: 'C++26 is done (again): ISO C++ meeting trip report'
link: https://herbsutter.com/2026/11/21/c26-is-done-again-trip-report-november-2026-iso-c-standards-meeting-buzios-brazil/
source: hnrss-org
published: 2026-11-22T15:59:59Z
updated: 2026-11-22T15:59:59Z
first_seen: 2026-11-24T05:54:25.686228927Z
authors:
- dverlaeckt80
labels:
- c++
- c++26
content: extracted
html: 2026-11-22-c-26-is-done-again-iso-c-meeting-trip-report.html
remote_preview:
  url: https://secure.gravatar.com/blavatar/fe7f5b0b83dba894afe26c42bcab33fe2d1246d9730ef182c72c0bff0dc26acb?s=200&ts=1795499521
---

## tl;dr: A few highlights

- C++26 technical work, originally completed in March, was reopened and is now recompleted: The anticipated plan is to reissue the updated C++26 document for a second approval ballot.
- Most of the six-day meeting was devoted to addressing a second set of international comments for C++26, primarily on the contracts language feature.
- Adopted this week for draft C++29: `break label; continue label;`, `std::ranges::any_view`, `views::slice`, and more…

## Overview

Today, the ISO C++ committee completed technical work on C++26 for a second time in beautiful Búzios, Rio de Janeiro, Brazil (hybrid online via Zoom).

As I [reported in March](https://herbsutter.com/2026/03/29/c26-is-done-trip-report-march-2026-iso-c-standards-meeting-london-croydon-uk/), the ISO C++ committee completed technical work on C++26 at our March meeting; following that, we held [the first meeting of C++29 in June](https://herbsutter.com/2026/06/13/brno-trip-report/). Since then, the approval (Draft International Standard, or DIS) ballot for C++26 was held but, for the first time, did not pass unanimously and was sent back to the ISO C++ committee with additional international comments to address, mostly about the contracts feature (`pre`, `post`, and `contract_assert`). So this week we addressed the additional comments on C++26, including by adopting an extension to contracts and providing a detailed rationale and timeline to inform national bodies of the history and process by which contracts were developed, concerns heard repeatedly, and decisions made. The committee is now producing an updated final document that is anticipated to be sent out for its second international approval (DIS) ballot and final editorial work, anticipated to be published in the near future by ISO.

This meeting was hosted by **C++ Alliance**. Thank you to everyone involved and especially to **Matheus Izvekov**, who took the lead in making the arrangements! Our hosts arranged for high-quality facilities for our six-day meeting from Monday through Saturday. We had about 220 attendees, split about 35%/65% in-person/remote-Zoom, formally representing 27 nations. (The higher-than-usual percentage of remote participation was partly because of the U.S. aviation ground stop that disrupted air travel just as experts were trying to fly to the ISO C++ meeting.) At each meeting we have new guest attendees, and this time there were 13 new guests, mostly in person, in addition to new attendees who are official national body representatives. To all of them, once again welcome!

[The committee](https://isocpp.org/std/the-committee) currently has 22 active subgroups, but this time fewer of them met throughout the week than usual as we focused on the additional C++26 comments.

## C++26, take 2

The most controversial feature in C++26 is contracts: `pre`, `post`, and `contract_assert`. The committee already had strong consensus, and as of the end of this meeting has still stronger consensus, on including the feature in C++26, but we spent most of this meeting rehearing contracts concerns and working hard again to address them.

Here is some history (much of this repeated from [my March trip report](https://herbsutter.com/2026/03/29/c26-is-done-trip-report-march-2026-iso-c-standards-meeting-london-croydon-uk/)): In February 2025, when we took the plenary poll to adopt contracts in the C++26 working draft (“merge it to trunk”), the vote was:

**87.7%** in favor:opposed — 100 in favor, 14 opposed, and 12 abstaining

After that, the concerns were all deeply rediscussed for the last four meetings of the original C++26 cycle thanks to thoughtful and high-quality technical papers explaining the objections; all of those papers continued to be fully heard, often for multiple days and at many telecons between meetings, with some contracts fixes adopted.

At our meeting in March 2026, when we took the plenary poll to finalize and ship the C++26 standard, the vote was non-unanimous primarily because of the sustained concerns about contracts:

**90.5%** in favor:opposed — 114 in favor, 12 opposed, and 3 abstaining

As I reported in March: After extensive scrutiny, the ISO C++ committee still wanted C++26 with contracts, and so contracts stayed in C++26 for the ISO DIS ballot.

Our past ISO C++ DIS approval ballots have always been unanimous. No national bodies (NBs) objected to balloting this C++26 DIS document that included contracts. However, during the ISO DIS process, additional late concerns were raised about contracts, including from five NBs that raised contracts concerns for the first time and voted Disapprove on C++26 because of them (see also my paper [P4454R0](https://isocpp.org/files/papers/P4454R0.pdf) section 2.3). The result, which surprised nearly everyone, was that the first DIS was not approved.

Therefore we spent most of the week in Búzios rehearing the NB comments on the DIS, which were mostly about contracts. One requested extension to contracts was adopted in response to NB comments (see next section). Other suggested changes were not adopted, notably the request by multiple comments to remove contracts from C++26; the committee discussed, revoted, and reaffirmed it wants to retain contracts in C++26.

This week, the vote to reship the C++26 standard with the changes to C++26 adopted at this meeting, including an additional extension to contracts in response to the international comments and retaining contracts as part of C++26, was:

**92.5%** in favor:opposed — 111 in favor, 9 opposed, and 8 abstaining

Thank you again to everyone in the committee who has been working hard together to discuss and address these concerns, including all week long again this week!

## Changes to C++26 adopted this week (and for draft C++29)

For C++26, we adopted four main papers and fixed some editorial issues. All of these changes were also adopted for draft C++29, which keeps the standards work in sync rather than forking.

The primary change to contracts was to adopt **[P4451R2 “Early Adoption of an Always-Enforced Label”](http://isocpp.org/files/papers/P4451R2.pdf)** by **Joshua Berne** (in a similar spirit to Romania’s paper [P3911R2 “Make Contracts Reliably Non-Ignorable”](https://isocpp.org/files/papers/P3911R2.html)). Thanks, Romania and Josh!

This paper addresses what several NBs told the committee was their primary request for contracts: It is a pure extension that gives programmers a way to express in source code that a specific contract is always enforced, guaranteeing that “program execution will not continue past the contract assertion if it is violated, regardless of the semantics of other contract assertions” that various parts of the program may be built with. This is forward-compatible with proposals like [P3400R5](http://isocpp.org/files/papers/P3400R5.pdf) being considered for C++29.

From the paper:

```plain
#include <contracts>

int f(int x)
    pre<std::contracts::controls::enforced>(x > 0)
    post<std::contracts::controls::enforced>(r : r > 0)
{
    contract_assert<std::contracts::controls::enforced>(x > 0);
    return x;
}

// Because the label is found by ordinary name lookup,
// a namespace alias or a using-directive works as well:

namespace scc = std::contracts::controls;

int h(int x)
    pre<scc::enforced>(x > 0)
    post<scc::enforced>(r : r > 0)
{
    contract_assert<scc::enforced>(x > 0);
    return x;
}

using namespace std::contracts::controls;

int g(int x)
    pre<enforced>(x > 0)
    post<enforced>(r : r > 0)
{
    contract_assert<enforced>(x > 0);
    return x;
}
```

We also adopted the following for C++26:

**[P4466R2 “Selectively transparent *using-declarators* (response to FI-067)”](https://isocpp.org/files/papers/P4466R2.html)** by **Dan Katz and Tomasz Kamiński** addresses a ballot comment to make reflection work more consistently with modules (especially module `std`), while leaving room for intended fuller support for `using`-alias reflection in C++29. Thank you, Dan and Tomasz!

**[P4453R1 “Memory allocation with senders/receivers; a response to IE-133”](https://isocpp.org/files/papers/P4453R1.html)** by **Dietmar Kühl and Lucian Radu Teodorescu** addresses a ballot comment to offer more flexible control over memory allocation with std::execution. Much appreciated, Dietmar and Lucian!

**[P3861R1 “Pragmatic approach to standard structural types”](https://isocpp.org/files/papers/P3861R0.pdf)** by **Corentin Jabot** makes reflection more useful. Thanks, Corentin! This paper allows more use of `string_view`, `span`, and `tuple`, including to use them as template parameters and to create type-safe strings at compile time that can be used at execution time.

## Adopted for C++29: Core language changes/features

We actually also did some work on C++29! Here are some highlights.

> *These links are to the most recent public version of each paper. If a paper was tweaked at the meeting before being approved, the link tracks and will automatically find the updated version as soon as it’s uploaded to the public site.*
>
> *Several of the adopted language and library additions are already supported today in major C++ implementations. “Adopted for C++29” doesn’t mean “have to wait until 2029 or later to start seeing some support in real compilers / standard libraries.”*

In addition to fixing a list of issues, we adopted five papers for the C++29 core language, including the following…

**[P0085R5 “Oo… adding a coherent character sequence to begin octal-literals”](http://wg21.link/p0085)** by **Jolly Chen, Axel Naumann, Michael Jonker** (all of CERN) gets the award for “most patient paper adopted at this meeting” — thanks, Jolly, Axel, and Michael! Its initial revision was published in 2015, and this week we adopted it: Finally, portable C++ will support “`0o`” (zero, lowercase-O) and “`0O`” (zero, uppercase-O) as a way to introduce octal literals, and the old prefix “`0`” is deprecated for that purpose.

**[P3568R4 “`break label;` and `continue label;`”](http://wg21.link/p3568)** by **Jan Schultke** adds labeled `break` and `continue` to draft standard C++. Thanks, Jan! A loop can now be preceded by a “`label:`” (or more than one) that names it, and can be a target for `break` and `continue` to say which of a set of nested loops is intended. Here is an example from the updated standard wording itself (lightly edited):

```plain
a: b: while (/* ... */) {
  a: a: c: for (/* ... */) {
    break;          // OK, terminates enclosing for loop
    break a;        // OK, same
    break b;        // OK, terminates enclosing while loop
    y: { break y; } // error: y is not a breakable statement
  }
  break c;          // error: c is not an enclosing statement
}
```

We also added several other language extensions and improvements.

## Adopted for C++29: Standard library changes/features

In addition to fixing a list of issues, we adopted seven papers for the C++29 standard library, including the following…

**[P3411R7 “`any_view`”](http://wg21.link/p3411)** by **Hui Xie, Louis Dionne, S. Levent Yilmaz, and Patrick Roberts** adds the type-erased `std::ranges::any_view` to the standard library. Thank you, authors! The paper’s abstract summarizes it well: “That type-erased view allows customizing the traversal category of the view, its value type and a few other properties. For example:”

```plain
class MyClass {
    std::unordered_map<Key, Widget> widgets_;
public:
    std::ranges::any_view<Widget> getWidgets();
};

std::ranges::any_view<Widget> MyClass::getWidgets() {
    return widgets_ | std::views::values
                    | std::views::filter(myFilter);
}
```

**[P3216R6 “`views::slice`”](http://wg21.link/p3216)** by **Hewill Kang** adds the `views::slice` adaptor to the draft C++29 ranges library. Thanks, Hewill! This provides a standard way to extract a contiguous subrange from a sequence by specifying a start index and an end index. It is also the first adaptor that takes two parameters (start and end).

**[P4189R0 “get()ing the pointer from optional”](http://wg21.link/p4189)** by **Nevin Liber** provides a portable way to get a pointer to the contained object of a `std::optional` by adding the `.get_ptr()` member function. Thank you, Nevin!

We also added several other library extensions and improvements.

## Wrapping up

Thank you again to the about 220 experts who attended in person and online at this week’s meeting, and the many more who participate in standardization through their national bodies!

But we’re not slowing down… we’ll continue to have subgroup Zoom meetings, and then our next full meeting will be in [Istanbul, Türkiye, in March](https://isocpp.org/files/papers/N5057.pdf), where we’ll continue adding features to C++29. I look forward to seeing many of you there.

Thank you to everyone reading this for your interest and support for C++ and its standardization.
