---
title: oh, apparently it's not possible to portably check for string-to-float conversion errors in standard c
link: https://sebsite.pw/w/20261009-strtod.html
source: lobste-rs
published: 2026-10-10T05:37:18Z
updated: 2026-10-10T05:37:18Z
first_seen: 2026-10-11T09:16:02.600736576Z
authors:
- sebsite.pw by seb
labels:
- c
summary: Comments
content: extracted
html: 2026-10-10-oh-apparently-it-s-not-possible-to-portably-check-for.html
---

this is kinda a sequel-ish to my [previous post](https://sebsite.pw/w/20261004-math_errhandling.html), in which i go over the `math_errhandling` macro, and how math error handling is done in glibc and musl (as well as how it's specified in the standard).

to summarize: `math_errhandling` is a macro which indicates which error-handling mechanisms are supported by `math.h` functions: `errno` (`MATH_ERRNO`) and/or floating-point exceptions (`MATH_ERREXCEPT`).

there's one thing i didn't mention in my previous post: there's one family of functions which is affected by `math_errhandling` but which *isn't* in `math.h`: the string to float conversion functions `strtod`, `strtof`, `strtold`, `strtod32`, `strtod64`, and `strtod128`:

7.25.2.6p12:

> If the correct value overflows and default rounding is in effect (7.12.2), plus or minus `HUGE_VAL`, `HUGE_VALF`, or `HUGE_VALL` is returned (according to the return type and sign of the value); if the integer expression `math_errhandling & MATH_ERRNO` is nonzero, the integer expression `errno` acquires the value of `ERANGE`; if the integer expression `math_errhandling & MATH_ERREXCEPT` is nonzero, the "overflow" floating-point exception is raised.
>
> If the result underflows (7.12.2), the functions return a value whose magnitude is no greater than the smallest normalized positive number in the return type; if the integer expression `math_errhandling & MATH_ERRNO` is nonzero, whether `errno` acquires the value `ERANGE` is implementation-defined; if the integer expression `math_errhandling & MATH_ERREXCEPT` is nonzero, whether the "underflow" floating-point exception is raised is implementation-defined.

the linux man pages for these functions literally don't mention this *at all*, and there actually is a reason why which i'll get to in a sec, but what the standard is saying is, if `math_errhandling` doesn't advertise support for `errno` (as is the case on musl, for instance), the string to float conversion functions **don't set `errno` on error**. furthermore, if the result underflows, the function **isn't required to report an error at all**.

keep in mind that the return value alone isn't enough to determine if an error occurred, so to test for overflow, you *need* to use one of the two error handling mechanisms.

here is my attempt at a standards-blessed way of portably checking for overflow/underflow errors in the string to float functions:

```
feclearexcept(FE_OVERFLOW | FE_UNDERFLOW);
errno = 0;
double x = strtod(s, nullptr);
bool errored = math_errhandling & MATH_ERRNO
	? errno == ERANGE
	: fetestexcept(FE_OVERFLOW | FE_UNDERFLOW);
```

note that this still doesn't guarantee that underflow is detected, since reporting underflow is entirely optional for the implementation.

the reason you've never done this ever (and the reason the man pages don't mention this) is that posix specifies the functions differently:

> If the correct value is outside the range of representable values, ±HUGE\_VAL, ±HUGE\_VALF, or ±HUGE\_VALL shall be returned (according to the sign of the value), and `errno` shall be set to `[ERANGE]`.
>
> If the correct value would cause an underflow, a value whose magnitude is no greater than the smallest normalized positive number in the return type shall be returned and `errno` set to `[ERANGE]`.

so posix doesn't give a shit about `math_errhandling`; it always requires the implementation to set `errno` if overflow or underflow occurs. while it's not uncommon for posix to specify stricter requirements on functions than standard c, the fact that the man page never makes any note of this being an extension (neither `strtod(3)` nor the posix specification `strtod(3p)`) is really notable to me.

whether or not posix's behavior is even compatible with standard c is... unclear. at least for `math.h` functions, setting `errno` regardless of the value of `math_errhandling` is permitted:

7.12.2p8:

> If a domain, pole, or range error occurs and the integer expression `math_errhandling & MATH_ERRNO` is zero, then `errno` shall either be set to the value corresponding to the error or left unmodified.

but earlier on, when specifying `errno.h`, the standard says this:

7.5p3:

> \[...\] The value of `errno` may be set to nonzero by a library function call whether or not there is an error, provided the use of `errno` is not documented in the description of the function in this document.

`errno` *is documented* in the description for these functions, and that description makes no mention of setting `errno` when `math_errhandling & MATH_ERRNO` is zero. so that suggests that posix's behavior is non-conformant.

but hang on! everything i've talked about so far is only for overflow and underflow. but if the string is malformed and can't be parsed as a number, then the standard doesn't specify any error at all:

7.25.2.6p11:

> The functions return the converted value, if any. If no conversion could be performed, positive or unsigned zero is returned.

instead, you're supposed to use the `endptr` parameter, and check afterward if `endptr == nptr` (i.e. the end pointer is the same as the start pointer, so no data was parsed):

7.25.2.6p8:

> If the subject sequence is empty or does not have the expected form, no conversion is performed; the value of `nptr` is stored in the object pointed to by `endptr`, provided that `endptr` is not a null pointer.

the man page `strtod(3)` says something similar:

> If no conversion is performed, zero is returned and (unless `endptr` is null) the value of `nptr` is stored in the location referenced by `endptr`.

but check out what posix says:

> Upon successful completion, these functions shall return the converted value. If no conversion could be performed, 0 shall be returned, and `errno` may be set to `[EINVAL]`.

the word "may" basically means that it's implementation-defined. but this is a big deal, because it's pretty common to check for errors by doing something like this:

```
errno = 0;
double x = strtod(s, nullptr);
if (errno != 0) {
	// ...
}
```

`strtod(3)` suggests doing exactly this:

> Since 0 can be legitimately returned on both success and failure, the calling program should set `errno` to 0 before the call, and then determine if an error occurred by checking whether `errno` has a nonzero value after the call.

but even for posix-compatible libcs, this isn't portable! different conforming libcs may behave differently if no conversion can be performed. in fact...

```
errno = 0;
strtod("x", nullptr);
printf("%d\n", errno);
```

on glibc, this prints 0, because glibc's `strtod` never sets `errno` to `EINVAL`. musl, on the other hand, *does* set `errno` to `EINVAL`, so this prints "22". **this is completely undocumented on the linux man page**.

it's also, from my reading of the standard, not conformant to standard c, because it's setting `errno` to a nonzero value in a function with other documented error conditions (and as far as standard c is concerned, invalid input isn't an error condition). musl *could* defend itself by saying that, because it doesn't set `errno` in its `math.h` functions, the `errno` condition in `strtod`'s description no longer applies, therefore there's no documented use for `errno` (since the use of `errno` is dependent on the value of `math_errhandling`). but that's clearly a stretch.

either way, it's clear to me that the standard needs clearer wording here.

## conclusion

just for fun, i figured i'd conclude with a list of all the "correct" ways to check for errors in the string to float conversion functions, just to hammer the point home:

### overflow

- if you're targeting posix, set `errno` to 0 before the call, and afterward check `copysign(result, 1.0) == HUGE_VAL && errno == ERANGE`.

- otherwise, set `errno` to 0 *and* call `feclearexcept(FE_OVERFLOW)` before the call, and, if `copysign(result, 1.0) == HUGE_VAL`, either check `errno == ERANGE` or `fetestexcept(FE_OVERFLOW)` after the call, depending on the value of `math_errhandling`.

  if you're targeting just one implementation, and you know which error-reporting methods it supports beforehand, you can skip the `math_errhandling` check and only support one of the two methods.

  if you're using `feclearexcept`/`fetestexcept`, make sure that the compiler knows you may be accessing the floating point environment: on gcc the relevant flag is `-ftrapping-math`, which is the default (unless you're using `-ffast-math`). there's also a standard pragma for this purpose: `#pragma STDC FENV_ACCESS ON`. this pragma isn't supported by gcc though.

### underflow

- if you're targeting posix, set `errno` to 0 before the call, and afterward check `result == 0.0 && errno == ERANGE`.

  be sure to check specifically that `errno` is `ERANGE`, not just that it's nonzero. otherwise you'll end up with nonportable behavior.

- otherwise, if you're just targeting one implementation, check if it documents its behavior on underflow in `math.h` functions. the behavior is implementation-defined, so technically it's required to document it. but in practice, even clang doesn't bother documenting its implementation-defined shit, so don't hold your breath.

  but if it is documented, and it reports underflow errors, then, depending on the implementation's value of `math_errhandling`, either set `errno` to 0 before the call and check `result == 0.0 && errno == ERANGE` after the call, or call `feclearexcept(FE_UNDERFLOW)` before the call and check `fetestexcept(FE_UNDERFLOW)` after the call.

- otherwise you're SOL.

  if you *reeally* need to check for underflow, and you *need* to use the libc function for some reason, here's the only option i can think of: check if `result == 0.0`, and if so, check yourself if the input string contains any nonzero digits before the exponent. if so, underflow occurred. this is difficult to get right though; read the description of `strtod` et al from the standard and make sure you cover all edge-cases.

### invalid input (no conversion)

- pass in `&endptr` as a second argument to the function, and then check if `endptr == nptr`. `errno` can't be relied on.
