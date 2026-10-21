---
title: Your C is not standard C
link: https://sebsite.pw/w/20261012-notstdc.html
source: hnrss-org
published: 2026-10-13T13:36:21Z
updated: 2026-10-13T13:36:21Z
first_seen: 2026-10-15T14:23:03.950687001Z
authors:
- dverlaeckt80
content: extracted
html: 2026-10-13-your-c-is-not-standard-c.html
extra:
  comments_url: https://news.ycombinator.com/item?id=50071546
  num_comments: 79
  points: 112
---

The committee has created an unreal language that no one can or will actually use.\
 \- [Dennis Ritchie](https://port70.net/~nsz/c/c89/dmr-on-noalias.html) (out of context)

this is an incomplete list of some incompatibilities between c as it's written in the standard and c as it exists in the real world for people who use gcc/clang + glibc/musl.

## `asm`

by default, gcc and clang recognize the `asm` keyword as an extension. this is not strictly conformant, because in standard c, `asm` is a valid non-reserved identifier. so code which conforms to the standard may not compile with gcc/clang with their default settings.

and like, whatever, that's fine, that's why `-std` exists, but 1. it's notable that compilers aren't conformant *by default*, and 2. why doesn't the standard just make `asm` a reserved word? c++ does that; i'm not sure why c doesn't.

## `FLT_ROUNDS`

the standard specifies the `FLT_ROUNDS` macro in `float.h` as follows:

5.3.5.3.3p23:

> The rounding mode for floating-point addition for standard floating types is characterized by the implementation-defined value of `FLT_ROUNDS`. Evaluation of `FLT_ROUNDS` correctly reflects any execution-time change of rounding mode through the function `fesetround` in `<fenv.h>`.

notably, the standard requires that the value dynamically changes with the runtime floating environment. gcc, however, always defines `FLT_ROUNDS` to 1 (rounds to nearest), regardless of the runtime rounding mode:

```
/* Addition rounds to 0: zero, 1: nearest, 2: +inf, 3: -inf, -1: unknown.  */
/* ??? This is supposed to change with calls to fesetround in <fenv.h>.  */
#undef FLT_ROUNDS
#define FLT_ROUNDS 1
```

clang defines a builtin for this (`__builtin_flt_rounds()`), but gcc doesn't, for some reason.

## standard pragmas

at some point in the future i'm gonna make an entire blog post dedicated to just this, because i kinda find it fascinating. but the super duper short version is this:

the c standard specifies five standard pragmas to control the behavior of floating point arithmetic:

```
// added in c99:
// switch can be ON, OFF, or DEFAULT
#pragma STDC FP_CONTACT ON
#pragma STDC FENV_ACCESS ON
#pragma STDC CX_LIMITED_RANGE ON

// added in c23:
// direction can be FE_DYNAMIC, FE_DOWNWARD, FE_TONEAREST,
// FE_TONEARESTFROMZERO, FE_TOWARDZERO, FE_UPWARD, or some
// other implementation-defined value
#pragma STDC FENV_ROUND FE_DYNAMIC
#pragma STDC FENV_DEC_ROUND FE_DYNAMIC
```

however, over 27 years after c99 was standardized, [gcc still doesn't support any of the standard pragmas](https://gcc.gnu.org/projects/c-status.html). they're just no-ops in gcc.

some (but not all) of the functionality can be replicated with command-line flags, but those operate on the entire translation unit, whereas the pragmas can apply to individual functions and blocks.

clang mostly supports the c99 pragmas, but still doesn't support the c23 pragmas.

there's actually *so much* more to say about these. i *will* be writing more about them at some point, but rn i don't really have the motivation cuz it's such a big thing lol

## qualifiers in function return types

the standard requires that qualifiers in function return types are discarded:

> If, in the declaration "`T D1`", `D1` has the form
>
> ```
> D ( parameter-type-listopt ) attribute-specifier-sequenceopt
> ```
>
> and the type specified for *ident* in the declaration "`T D`" is "*derived-declarator-type-list `T`*", then the type specified for *ident* is "*derived-declarator-type-list* function returning the unqualified, non-atomic version of `T`". The optional attribute specifier appertains to the function type.

but clang treats them as significant:

```
$ cat t.c
int f();
const int f();
```

```
$ clang t.c
t.c:2:11: error: conflicting types for 'f'
    2 | const int f();
      |           ^
t.c:1:5: note: previous declaration is here
    1 | int f();
      |     ^
1 error generated.
```

the two declarations should be identical, but clang thinks they're incompatible.

## outdated `va_start` definition on musl

for some reason, musl provides its own versions of some headers which the compiler already provides itself. one of these is `stdarg.h`, which is required to be supported by freestanding implementations and so is virtually guaranteed to already be provided by the compiler. but because it's provided by musl, compilers will use that header instead of the builtin one.

this is a problem because musl's header is strictly worse than the builtin header. here's how musl defines `va_start`:

```
#define va_start(v,l)   __builtin_va_start(v,l)
```

this definition is incorrect, because c23 allows the second argument to be omitted. it should be defined like this:

```
#if __STDC_VERSION__ >= 202311L
#define va_start(...) __builtin_c23_va_start(__VA_ARGS__)
#else
#define va_start(v,l) __builtin_va_start(v,l)
#endif
```

the result is that, when compiling for musl, c23's single-argument `va_start` can't be used, even if it's supported by the compiler.

## see also

- [notes on how glibc and musl handle math errors](https://sebsite.pw/w/20261004-math_errhandling.html) (`math_errhandling`)
- [oh, apparently it's not possible to portably check for string-to-float conversion errors in standard c](https://sebsite.pw/w/20261009-strtod.html): a case where posix arguably specifies a family of functions incompatibly from standard c
- [neither gcc nor clang are compliant with standard c++](https://sebsite.pw/w/20260708-badstdcxx.html): gcc and clang don't store language linkage information with function types in c++, contrary to what the standard requires
- [yet another place where a linux man page is inaccurate](https://sebsite.pw/w/20261014-initstate.html) (containing a deleted section from this post!)
