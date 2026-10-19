---
title: The complement of true is true, except when it's false
link: https://dryperspective.github.io/posts/complement-of-true/
source: lobste-rs
published: 2026-10-04T12:30:26Z
updated: 2026-10-04T12:30:26Z
first_seen: 2026-10-08T13:19:06.446835656Z
authors:
- dryperspective.github.io via aw1621107
labels:
- c++
- enums
- undefined-behaviour
- wg21
summary: Comments
content: extracted
html: 2026-10-04-the-complement-of-true-is-true-except-when-it-s-false.html
---

I recently looked at [P4313R1](https://www.open-std.org/jtc1/sc22/wg21/docs/papers/2026/p4313r1.html), a standards proposal paper which adds a set of bitmask operations for enums, using a C++26 annotation to opt-in. The core idea being that, to use an example from the paper, given code like the below:

```cpp
enum class [[=std::bitmask_type]] Permission {
  None    = 0,
  Read    = 1 << 0,
  Write   = 1 << 1,
  Execute = 1 << 2,
};
```

The `[[=std::bitmask_type]]` annotation would automatically imbue `Permission` with an accessible set of bitwise operations so that you as a user needn’t write them out yourself. This got me thinking about the murky underbelly of C++ integer operations. Your C++ compiler will happily compute bitwise operations for any integral type as a builtin operation. This extends to types which we don’t traditionally think of as integers, such as `wchar_t`, the UTF character types `char8_t` through `char32_t`, and `bool`. But slightly more happens here than meets the eye. Because when you attempt to perform an operation like `a | b`, and the type of `a` and `b` is an integral type smaller than `int`, the language does not operate on the bit patterns of `a` and `b` directly. They undergo *integral promotion* - they are promoted up to `int` as an intermediate state, the bit patterns of these two `int`s are combined, and the result is returned to you, still as an `int`.[^1] Consider the below:

```cpp
//Two shorts
constexpr short perm_A {1 << 0};
constexpr short perm_B {1 << 1};

//And the result type of running a bitwise operation on them is int
static_assert(std::same_as<decltype(perm_A | perm_B), int>);
```

Now, for the most part this is harmless - if you cast the above `perm_A | perm_B` back to `short` then the unnecessary bytes are truncated away and you’re left with a `short` which contains exactly the value it would have held if you’d combined `perm_A` and `perm_B` as `short` directly. And if you want to be certain that you are keeping your types consistent, and avoid the pernicious bugs of silent narrowing conversions, you can get into the habit of `static_cast`-ing the result of your bitwise operation to its original type. The important thing here however is that integral promotion is not optional. Unlike most other areas of C++ the developer doesn’t get a choice - your types will unavoidably be promoted for these operations.

The second part of what makes this a hazard for `bool` specifically is *boolean conversion*, a special case which does *not* truncate but instead explicitly converts zero to `false` and non-zero values to `true`. For these non-zero values, whatever bit pattern was previously stored is discarded and replaced with `true`, which has an integer value of 1. So if we take code like this:

```cpp
int x{10};
bool b{static_cast<bool>(x)};
```

and look at the generated asm (in this case from unoptimised x86-64 gcc 16.2):

```asm
mov     DWORD PTR [rbp-4], 10   ;Store the value of 10
cmp     DWORD PTR [rbp-4], 0    ;Then compare to 0, set ZF if x is zero
setne   al                      ;Write 1 to AL if ZF is clear
mov     BYTE PTR [rbp-5], al    ;Store the result
```

The standard (specifically [\[conv.bool\]](https://eel.is/c++draft/conv.bool)) bases converting to `bool` on the only possible values which `bool` can hold - `true` and `false`, regardless of whatever bit pattern may have originally been used to create them.

## Putting it together

With all of that covered, let’s talk through what happens when you try to evaluate `~true` and then cast the result to a `bool`:

- The value `true` is promoted to an `int` with a value of `1`.
- The complement of `1` as an `int` is calculated as `-2`, as two’s complement behaviour is required as of C++20.
- The value of `-2` is then cast back down to `bool`, undergoes boolean conversion, and since `-2` is non-zero, becomes `true`.

There you have it, the complement of `true` is `true`, or spelled in C++ `static_cast<bool>(~true) == true`.

This brings us back to enums. An enum is permitted to use any integral type as its underlying type, including our good friend `bool`. So let’s define one:

```cpp
enum class [[=std::bitmask_type]] boolean : bool{
    FALSE,
    TRUE,
};
```

Let’s also look at the bitwise complement operator as laid out in P4313R1:

```cpp
template<bitmask-like T> constexpr T operator~ (T lhs) noexcept {
  return static_cast<T>(~to_underlying(lhs));
}
```

By now the workings of that operator should be a familiar shape - first we convert the enum to its underlying type (in our case `bool`), then integral promotion is applied if that type is lower ranked than `int`, then we perform the bitwise operation, then we cast back to the enum type. As we would expect:

```cpp
static_assert(~boolean::TRUE == boolean::TRUE);
```

Godbolt [here](https://godbolt.org/z/PW1EEvMex).

This has all the potential to be a slightly confusing corner case in the language.

## When it’s false

There is one exceptional case here which compounds the problem. If we try to run that same example in gcc, we get a different result:

```cpp
static_assert(~boolean::TRUE == boolean::FALSE);
```

Godbolt [here](https://godbolt.org/z/xWzjarEz8).

So what is going on with gcc? If we move the operation to runtime by defining two functions which depend on it as a runtime value:

```cpp
enum class boolean : bool{
    FALSE,
    TRUE,
};

void f(int x, bool& out) {
    out = static_cast<bool>(x);
}

void g(int x, boolean& out) {
    out = static_cast<boolean>(x);
}
```

and look at the asm (again unoptimised x86-64), then we get:

```asm
"f(int, bool&)":
        push    rbp
        mov     rbp, rsp
        mov     DWORD PTR [rbp-4], edi
        mov     QWORD PTR [rbp-16], rsi
        cmp     DWORD PTR [rbp-4], 0
        setne   dl                          ;same setne pattern from [conv.bool] earlier
        mov     rax, QWORD PTR [rbp-16]
        mov     BYTE PTR [rax], dl
        nop
        pop     rbp
        ret
"g(int, boolean&)":
        push    rbp
        mov     rbp, rsp
        mov     DWORD PTR [rbp-4], edi
        mov     QWORD PTR [rbp-16], rsi
        mov     eax, DWORD PTR [rbp-4]
        and     eax, 1                      ;store the low bit in eax
        mov     rdx, QWORD PTR [rbp-16]
        mov     BYTE PTR [rdx], al          ;and move al to the out-param
        nop
        pop     rbp
        ret
```

What we see is that when the type is not spelled `bool`, gcc will truncate down to the lowest bit rather than perform a boolean conversion. Any even value, when cast down, will produce `boolean::FALSE`, and any odd one will produce `boolean::TRUE`.

This is unique to enum types specifically, gcc generally performs consistently when handling `bool` directly:

```cpp
constexpr boolean b{boolean::TRUE};
static_assert(static_cast<bool>(~b) == false);

static_assert(static_cast<bool>(~true) == true);

static_assert(std::to_underlying(~b) == false);

static_assert(static_cast<bool>(~std::to_underlying(b)) == true);
```

In contrast, Clang and MSVC will evaluate the result of all four of these complement-and-cast combinations as `true`, which is what the standard says they must be. Full godbolt comparison [here](https://godbolt.org/z/5vK3zM7Y9). This seems to be a fairly long-lived conformance bug in gcc.

But there is one place in the standard where integral promotion rules go further and introduce a new vector for undefined behaviour to enter into your program when doing these bitmask operations. Consider this code:

```cpp
enum nums{
    zero,
    one,
    two,
    three,
};

//This is implementation defined but holds on gcc and Clang.
static_assert(std::is_same_v<std::underlying_type_t<nums>, unsigned int>);

//nums uses an unsigned int as its base, therefore the entire domain of representable values is >= 0.
//So let's test this:
static_assert(~one >= 0); //FAILS
```

Godbolt [here](https://godbolt.org/z/csW7GarKc)

Looking at the diagnostic, gcc gives us:

```
<source>:15:20: error: static assertion failed
   15 | static_assert(~one >= 0); //FAILS
      |               ~~~~~^~~~
  • the comparison reduces to '(-2 >= 0)'
```

So what happened here? Well, the passage in the standard which covers integral promotion, [\[conv.prom\]](https://eel.is/c++draft/conv.prom#3), carves out a special bullet for unscoped enumeration types with no fixed underlying type to behave differently from integer types when promoting. If the entire range of values can be stored in an `int`, then that is the type which it promotes to, then it tries `unsigned int`, and if that fails it repeats this signed-then-unsigned pattern for `long` and `long long` until it finds a suitable type. But, where this differs from plain integer types is that it only considers the effective range of representable values; and so an enum backed by `unsigned int` will promote to `int` regardless of the normal conversion ranking which would forbid it for integral types. As before, the fact that you are calling the builtin operator via `~one` will unavoidably promote it to `int`, its complement is calculated as `-2`, and then the comparison is performed between two `int`s, and fails. If instead you convert to the underlying type first then you really see the asymmetry - `static_assert(~std::to_underlying(one) >= 0);` succeeds, and is so vacuously true that gcc even warns about its redundancy.

But this is only half the battle, and we need to talk about converting the result of our bitwise operation back to the enum type, and this is where UB creeps in. An unscoped enum with no specified underlying type defines itself in terms of a valid range of values determined by its enumerators independently of the range of whatever actual type is used by the compiler to back it. [\[dcl.enum\]](https://eel.is/c++draft/dcl.enum) tells us the value range of such an enum is the value range of a hypothetical integer type of width M, where M is the minimum bits required to represent all enumerators. To demonstrate, consider a few examples:

```cpp
enum small{ //Range of enumerators is 0..1, so value range is that of an unsigned, one-bit integer.
    a = 0,
    b = 1,
};

enum medium{ //Range of enumerators is 0..7, so value range is that of an unsigned, three-bit integer
    c = 0,
    d = 4,
    e = 7
};

enum large{ //Range of enumerators is -1..100, so value range is that of a signed, eight-bit integer
    f = -1,
    g = 10,
    h = 100,
};
```

Even though your compiler will quite likely store these enums as `unsigned int` and `int`, only a subset of possible representable values is valid to use - that of the M-width integer. [\[expr.static.cast\]](https://eel.is/c++draft/expr.static.cast#8) in the standard makes it clear that it is undefined behaviour to cast a value outside of that range to the enum type. So, using the enumerators of the above example:

```cpp
static_cast<small>(1); //Well-defined
static_cast<small>(2); //UB

static_cast<medium>(3); //Well-defined: while not a value of an enumerator, it is within the range of an unsigned, three-bit integer
static_cast<medium>(8); //UB

static_cast<large>(-128); //Well-defined
static_cast<large>(128); //UB
```

This is our danger zone. The bitwise operators, in particular `operator~`, can produce values which are out of the well-defined value range for an `enum`, and user-defined operators which attempt to cast this value back down to the enum type can invoke UB.

But, I hear you cry, what about the example earlier with `~one`? We saw that give a value of `-2` which is outside the value range of `nums`. This is true, but here is where integral promotion actually saves us - `one` was promoted to `int` before we calculated its complement, and it was never cast back down again to invoke our UB. We only get into dangerous territory with a user-defined operator which converts the result of the operation back down to the original enum type.

It is also important to note that this UB risk only applies to unscoped “plain” enums with no fixed underlying type. All scoped enum types have a fixed underlying type, even if they don’t specify it (in that case, `int`); and all unscoped `enum` types which do specify a fixed underlying type inherit that type’s value range instead of inventing their own.

## Avoiding this in your own code

Let’s say that, like P4313, you are adding bitmask operations to enums for your own code. Now that we have more weirdness in this area of C++, how would you go about making sure that your code is correct and unconfusing? Avoiding the `bool` case is simple - just constrain away that the underlying type can’t be `bool`, either for all enums or for the complement operator. But the standard doesn’t come with a handy type trait or reflection metafunction to specifically detect an unscoped enum with no fixed underlying type. Fortunately we can make one:

```cpp
template<typename E>
concept unfixed_enum = std::is_enum_v<E> && !requires { E{0}; };
```

[\[dcl.init.list\]](https://eel.is/c++draft/dcl.init.list#3.8) only permits this initialization from a scalar for enums with a fixed underlying type, and 0 is the only possible value which is representable for all possible enums. To cover the edge cases, the standard defines an enum with no enumerators as having the value range of an unsigned, one-bit integer; and the thing which excludes 1 as a possible value is `enum E{ a = -1 };`, which has a range `[-1, 0]`. Be sure not to forget the `std::is_enum_v`. After all, there are many types for which `T{0}` is ill-formed, and most of them are not enums.

This post has mostly been focused on the bitwise complement operator, but the UB trap of an unfixed enum also applies to the bitwise left shift operator. If you take the route of constraining per operation, don’t let it slip your mind.

This brings us back once again to P4313R1. At time of writing, the concept which constrains these operators only constrains the type to be some enumeration type annotated with `[[=std::bitmask_type]]`. As such, it will allow confusing behaviour on enum types which are backed by `bool`, and potential UB on plain enums with no fixed underlying type. If the authors don’t want to standardise the above init-list trick, they could constrain it on `std::is_scoped_enum` as a conservative approach to prevent users from having easy access to the UB, since all unscoped enums can use the builtin bitwise operators anyway (albeit returning `int`). They could also constrain the underlying type to not be `bool` to minimise the confusion; particularly as the diagnostic which would normally get issued for complementing a `bool` in user code would be suppressed by default if it came from a system header. I do intend to contact the authors of P4313 about this to see if they want to add these constraints to their paper.

## Aside: What about floating point?

You might be wondering how the standard defines conversions of floating point numbers to these `bool`-backed `enum`s. It is perhaps unsurprising - [\[expr.static.cast\]](https://eel.is/c++draft/expr.static.cast#8) requires that the behaviour is equivalent to first converting the floating point value to the underlying type of the enum, and then converting it to the enum type; pointing the user to [\[conv.fpint\]](https://eel.is/c++draft/conv.fpint#note-1) which has an explicit note directing readers to our old friend \[conv.bool\]. Per the standard, this should mean that any floating point value other than exactly `0.0` or `-0.0` would convert to `boolean::TRUE`, otherwise we end up back at `boolean::FALSE`.

So let’s start small:

```cpp
enum class boolean : bool {
    FALSE,
    TRUE,
};

static_assert(static_cast<boolean>(0.0) == boolean::FALSE);
static_assert(static_cast<boolean>(0.5) == boolean::TRUE);
static_assert(static_cast<boolean>(2.5) == boolean::TRUE);
```

Godbolt [here](https://godbolt.org/z/1aK8Kfadv).

Both Clang and gcc reject this code. The errors are that `(boolean)2.5e+0` is not a constant expression; and `static_cast<boolean>(0.5) == boolean::TRUE` is wrong, and is instead equal to `boolean::FALSE`. MSVC accepts the above code.

But let’s go further and inspect what actually gets stored there. We write a function to examine the bit pattern generated from these casts, then try some values:

```cpp
void show(double d) {
    const boolean e = static_cast<boolean>(d);
    unsigned char byte {};
    std::memcpy(&byte, &e, 1);
    std::println("static_cast<boolean>({:7.1f})  stored byte {:3}   required {}",
               d, byte, (d != 0.0) ? 1 : 0);
}

int main() {
    show(0.0);
    show(0.5);
    show(1.0);
    show(2.5);
    show(3.0);
    show(256.0);
    show(-2.5);
}
```

Godbolt [here](https://godbolt.org/z/Phd8jvcjj).

The above code compiles without warning on all three compilers, but while MSVC again does the right thing in all cases, gcc and Clang’s behaviour is much more worrisome. Looking at the output, we see that gcc will always just truncate the value to an integer, then load that bit pattern into the resulting `boolean`. So `2.5` truncates to `2` and gives a `boolean` whose underlying bit pattern is `2`. Clang does the same thing on unoptimised builds, but when optimisation is turned on, the truncated value then goes through the \[conv.bool\] transformation as normal and produces the right answer for all non-zero values outside of the range (-1, 1).

This is more concerning than some funky bit patterns, however. The valid range of `boolean` is \[0, 1\]. It is UB to read objects with bit patterns outside of this range. But, I hear you ask, what happens if we take these `boolean`s and cast them back to `bool`, triggering a boolean conversion with no initial floating point state? MSVC again does the right thing; gcc doesn’t modify the bit patterns, leaving us with `bool` which are out of range of the type (and therefore UB to read); and Clang is where the fun happens again. Running the above code with a cast from `e` to a `bool`, and then memcpy-ing that `bool` into the `unsigned char`, we get this result for unoptimised builds:

|                    | 0.0 | 0.5 | 1.0 | 2.5 | 3.0 | 256.0 | -2.5 |
| ------------------ | --- | --- | --- | --- | --- | ----- | ---- |
| required           | 0   | 1   | 1   | 1   | 1   | 1     | 1    |
| gcc 14 / 15 / 16   | 0   | 0   | 1   | 2   | 3   | 0     | 254  |
| clang 19 / 20 / 21 | 0   | 0   | 1   | 0   | 1   | 0     | 0    |
| clang 23.1.1       | 0   | 0   | 1   | 1   | 1   | 0     | 1    |

Versions of Clang before 23 appear to simply perform `trunc(d) & 1`; meaning that after truncation, odd numbers are `true` and even numbers are `false`. Clang 23 performs `(trunc(d) % 256) != 0`, narrowing to a byte first. So `256.0` becomes `FALSE` and `257.0` becomes `TRUE`. Optimised builds act as before - truncating then doing the proper boolean conversion.

Ultimately this all comes to a rather absurd head. Consider the below code:

```cpp
#include <print>

enum class boolean : bool {
    FALSE,
    TRUE,
};

//Force this to be runtime
boolean make(double d) {
    return static_cast<boolean>(d);
}

int main() {
    const boolean e = make(2.5);
    std::println("e == boolean::TRUE  : {}", e == boolean::TRUE);
    std::println("e == boolean::FALSE : {}", e == boolean::FALSE);

    const bool b = static_cast<bool>(e);
    std::println("b == true  : {}", b == true);
    std::println("b == false : {}", b == false);
    std::println("b + 0      : {}", b + 0);
}
```

Godbolt [here](https://godbolt.org/z/T7zMqGo8j).

MSVC again leads the pack in giving the correct answer. Clang gets it exactly wrong and thinks that `e` is `FALSE` and `b` is `false`. gcc takes things up a notch, by providing an instance of an `enum` which compares equal to all of its enumerators, and a `bool` which is neither `true` nor `false`, until you turn on the optimiser and get a `bool` which is both `true` and `false`.

And to hit all the obligatory targets of any conversation on floating point, infinity and NaN show as `0` and become `FALSE` as `boolean` and so cast to `false` as `bool` on gcc and Clang; and give `1` and therefore `TRUE` and `true` on MSVC.

In lighter news, Clang trunk seems to have fixed the above example to behave correctly, so some of the issues described in this section should be patched out soon.

## Conclusion

What did we learn from all this? Perhaps that even sensible and uncontentious pure-library-level papers such as P4313 should be on their guard against a footgun from some exotic C++ edge case. Or perhaps, in more practical terms:

- Integral promotion is unavoidable. If you think you are operating on an integer which is smaller than `int`, there’s a good chance it became an `int` right under your nose.
- The bitwise complement of a `bool` is always `true`, even when wrapped in an enum. However you should not rely on this behaviour as gcc has a longstanding bug which gets this wrong (or right, if you prefer logic to C++).
- Unscoped enumeration types with no fixed underlying type have a range of valid values which may be smaller than that of whatever type actually underlies them.

[^1]: To be clear, the ranking order of integral promotion is more complex than just “convert to `int`”. For an integer type of lower conversion rank (not size) than `int`, if all values of that type can be represented as `int` then `int` is chosen. Otherwise `unsigned int` is chosen. In practice this will usually come out as `int`; but on implementations where `int` and `short` are the same size, `unsigned short` will promote directly to `unsigned int`. And for other types such as the charN\_t family, conversion can proceed to longer integer types such as `long` and `long long`. `bool` is singled out as a special case and always promotes to `int`.
