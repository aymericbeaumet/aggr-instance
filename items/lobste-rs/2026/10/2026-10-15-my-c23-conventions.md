---
title: My C23 Conventions
link: https://abner.page/post/c23-conventions-1/
source: lobste-rs
published: 2026-10-15T15:32:15Z
updated: 2026-10-15T15:32:15Z
first_seen: 2026-10-16T22:01:23.127136231Z
authors:
- abner.page by abnercoimbre
labels:
- c
summary: Comments
content: extracted
html: 2026-10-15-my-c23-conventions.html
---

This is one of a three-part mini series about how I’m using modern C as of late 2026.

Before C23, I was [taken in](https://www.youtube.com/watch?v=443UNeGrFoM) by *How I Program C* for years and years, and would even feel guilty for choosing C99 instead of `-ansi -pedantic`. But holy crap man I’m done being a doomsday prepper! These days I maintain a large-enough project with enough users where I could stand less foot-shooting. (I’ve also got bills to pay.)

So here’s three modern C conventions which started to rise naturally from adding `-std=c23` to my zig build:

## 1\. Unused? Maybe. (Yes)

Our first convention is centered around `[[nodiscard]]`. If I draw an app window or panel I require the caller to interrogate the rect of the thing that was just rendered:

```c
[[nodiscard]] rect_t draw_screen(/* ... */) {
    rect_t screen_r = { };
    // Draw stuff
    // ...
    return screen_r;
}
```

It’s a genuine business logic bug if the caller doesn’t capture the rectangle for later processing. Therefore `[[nodiscard]]` has been such a godsend for fixing bugs… Except there’s a choice few instances where I will draw something with *no real use* for the resulting rect. I’d rather ignore the return value:

```c
enum screen_opt popup_options = /* ... */ ;
draw_screen(popup_options, /* ... */); // ERROR: damn you [[nodiscard]]
```

Clang will error out here. I could maybe capture the var and never use it but…

```c
rect_t rect = draw_screen(/* ... */); // ERROR: unused variable (I have `-Werror` enabled)
```

Pre-C23 one might solve this through an `UNUSED()` macro:

```c
#ifndef UNUSED
#define UNUSED(x) (void)(x)
#endif

rect_t rect = draw_screen(/* ... */);
UNUSED(rect); // Silences the compiler, but now I'm sad.
```

But now with C23 we can delete the macro in favor of the native `[[maybe_unused]]`:

```c
[[maybe_unused]] rect_t rect = draw_screen(...); // OK
```

Phew I feel better. I’ve also noticed this is happening all over the place: lots of different routines returning different types that are marked with `[[nodiscard]]`. So for all the unused cases why not just go for:

```c
[[maybe_unused]] auto _ = foo(); // A new pattern emerges
```

Bonus: For any macro fans out there a fellow indie dev (Zardium) offered an additional improvement. From our chat server:

> Zardium: #define \_ \[\[maybe\_unused\]\] auto CONCAT\_MACRO(zz,**COUNTER**)

> Abner: ahh, unique varname. nice one

> Zardium: you could take the auto out if you don’t want it to look so weird. then it acts exactly like the \_ in other languages, where it can be redefined

> Abner: Yep totally. Although here I was celebrating my removal of a macro… XD

## 2\. More Perfect Unions

I’ve been cheated and lied to: designated initializers DO NOT clear my struct to zero and I can’t even trust `{0}` anymore. (Talk about a betrayal.) The problem is I’ll often have data types like this contrived timestamp:

```c
typedef struct {
    Type type;
    char buf[MAX];
    union {
        time_t time;
        float dt;
        date_t date;
    };
} stamp_t;
```

And if I partially initialize one:

```c
stamp_t stamp = { .type = STAMP_TYPE_BLA_BLA };
```

While it does mean we successfully zeroed out `buf` (thanks to [empty initialization](https://en.cppreference.com/c/language/initialization#Empty_initialization)), those union members are just a different beast. It also doesn’t matter if we go with `stamp_t stamp = {0}` instead. My dear reader: I’ve made the costly mistake of assuming `date` would clear to zero!

As I Spongebob-panicked in public, Lobsters member [accelbread](https://lobste.rs/~accelbread) offered a sage revelation:

> accelbread: It used to be that GCC would fully zero the union anyways, and clang would only zero the time\_t, though around a year ago they swapped behaviors iirc. Either way, standard only requires the time\_t be zeroed and not whole union. GCC has a flag to always zero whole union. Also C23 initialization always inits whole union

Wait WHAT? clang and gcc SWAPPED what now? I won’t investigate further. Everything about this is *dangerous af* unless we hug C23 tightly and never let go:

```c
stamp_t stamp = { }; // Fully clears to zero (C23)
stamp.type = STAMP_TYPE_BLA_BLA;
```

Accelbread went on to say two more things:

1. *“Note the C23 zero init whole thing is nothing in the brackets, not 0 in brackets; { 0 } does same thing as before C23”*
2. *“Also if you control union layout, putting largest member first is safest”*

Yessir.

## 3\. Enums In C Were Magic (And I Don’t Like It)

If I want to customize some dialog box I’ll use bit flags:

```c
enum {
    OPT_AUTOSCROLL = (1 << 0),
    OPT_NOCURSOR   = (1 << 1),
    OPT_CENTERED   = (1 << 2),
    OPT_NOCLOSE    = (1 << 3),
    // ...
};

[[nodiscard]] bool draw_warning(const char* msg, int opt);
```

Even if we disagree with this approach lots of relevant libraries or projects do it:

```c
_ = draw_warning("you done messed up A-A-RON", OPT_CENTERED | OPT_NOCLOSE);
```

Of course, erroneous bit shifting will land anyone into trouble and yield comical numbers. Thankfully C enums are a plain `int` underneath so the compiler will warn us if we defined a bad constant inside the enum that won’t fit.

:)

I’m kidding. Sigh. Well, I’m growing fearful of sage revelations but this one comes from [Modern C](https://gustedt.gitlabpages.inria.fr/modern-c/) by Jens Gustedt:

> Jens: If enumeration constants do not fit into signed int, if possible, the enumeration type is adjusted such that it can store all enumeration constants.

Noooooo!

> Jens: The property that an an enumeration type is adjusted such that it fits all its constants could have surprising effects

*sobs*

> Jens: So, it is preferable to specify the underlying integer type explicitly

THERE IS HOPE?

```c
enum options : int {
    OPT_AUTOSCROLL = (1 << 0),
    OPT_NOCURSOR   = (1 << 1),
    OPT_CENTERED   = (1 << 2),
    OPT_NOCLOSE    = (1 << 3),
    // ...
};
```

It compiles! We shoot. We score. We stick with C23.

## Epilogue

In part two of our mini series I’ll discuss three more conventions that I’ve fleshed out thanks to modern C: starting with safer macro usage (forgive me Zig folks.)

*P.S. If you want to shape this discussion [maybe join](https://handmadecities.com/chat) the Handmade Cities server!*

-Abner
