---
title: Stabilizing Rust's Never Type
link: https://lwn.net/SubscriberLink/1091015/d9e48318ed242b41/
source: hnrss-org-frontpage
published: 2026-09-09T11:57:45Z
updated: 2026-09-09T11:57:45Z
first_seen: 2026-09-12T23:18:46.813688217Z
authors:
- cjd8
summary: 'Article URL: https://lwn.net/SubscriberLink/1091015/d9e48318ed242b41/ Comments URL: https://news.ycombinator.com/item?id=49625056 Points: 108 # Comments: 18'
content: extracted
html: 2026-09-09-stabilizing-rust-s-never-type.html
preview:
  file: 2026-09-09-stabilizing-rust-s-never-type.preview-058f572d61e1.webp
  width: 70
  height: 81
  alt: LWN.net Logo
  color: '#948d76'
images:
- source: https://static.lwn.net/images/logo/barepenguin-70.webp
  original:
    file: 2026-09-09-stabilizing-rust-s-never-type.image-d3c04bff3171.webp
    width: 70
    height: 81
  color: '#d6d7d8'
---

\[+\] Proceed to the article

A function's return type is supposed to indicate the kind of data that it produces. Rust's "never" type, which is [denoted by an exclamation mark](https://doc.rust-lang.org/stable/std/primitive.never.html) ("!"), is the type the language uses to mark a function that never returns and other places where a value can never occur. For a long time, the never type was used internally by the compiler, but was considered an unstable feature. On [August 24](https://github.com/rust-lang/rust/pull/155499), after more than two years of work, Rust-compiler-contributor "waffle" finally managed to stabilize the type. It took so long, in part, because it involved a small breaking change to previous Rust editions, which the compiler maintainers needed to ensure did not impact much real code.

(Note: Rust also uses exclamation marks to indicate calls to macros. The way the syntax is constructed, a place where it is valid to use the never type is not a valid place to put a macro invocation and vice versa.)

#### Why a never type?

There are two reasons that Rust has a never type, one practical and one philosophical. The practical reason is that it allows for more efficient generic code. For example, consider the [FromStr](https://doc.rust-lang.org/std/str/trait.FromStr.html) trait in the standard library, which is used for types that can be instantiated from a string:

```
    trait FromStr: Sized {
        type Err;
        fn from_str(s: &str) -> Result<Self, Self::Err>;
    }
```

FromStr::from\_str() either returns a converted result, or a custom error type. For example, attempting to convert "foo" into an integer will return a [ParseIntError](https://doc.rust-lang.org/std/num/struct.ParseIntError.html). But some types have an infallible conversion. For example, it is always possible to convert a string into a [ByteString](https://doc.rust-lang.org/std/bstr/struct.ByteString.html). That implementation of FromStr could set Err to be the never type. Then the compiler would know that the error branch of the returned Result is never present, and could optimize out all of the code that touches it or checks for it.

```
    impl FromStr for ByteString {
        type Err = !;
        fn from_str(s: &str) -> Result<Self, !> { ... }
        // Keeps the same generic interface,
        // but generates code equivalent to:
        // fn from_str(s: &str) -> Self { ... }
    }
```

The philosophical reason involves correct type inference. In Rust, constructs such as if statements and while loops are expressions; their results can be assigned to a variable. The compiler needs a type to infer for the result of an infinite loop, if the programmer writes one. That shouldn't come up often in real code, but it turns out to simplify type inference to be able to treat that case uniformly, rather than adding special rules to handle it.

In particular, the never type has a useful property for simplifying code: it automatically coerces to any other type. This sounds strange, but it is safe, since the never type represents the "result" of a computation that will never produce a value. So, anywhere that the code claims to have a value of the never type, the compiler knows that it can't possibly *reach* that code, and therefore it's safe to ignore it. This is a form of type-system-driven dead-code elimination.

For both of these reasons, Rust programmers have wanted to be able to use the never type in stable versions of the language. Making that happen required resolving a particularly thorny corner case.

#### Never fallback

Because of the way that conversions from the never type to other types are implemented, the compiler can sometimes end up in a situation where it cannot naively infer the concrete type of an expression. Consider this example, which defines an anonymous function (using ||, which is like Python or LISP's lambda) that never returns, and then calls it in a way that expects a concrete error type (using the [? operator](https://doc.rust-lang.org/book/ch09-02-recoverable-errors-with-result.html?highlight=error#the--operator-shortcut)):

```
    let function_that_never_returns = || { loop {} };
    function_that_never_returns()?;
```

That infinite loop is given a type of ! which is then implicitly converted to whatever the function is supposed to return. But since the function is defined locally and not given an explicit type, the compiler does not have sufficient information to say what that type is. The problem could be fixed by giving the function an explicit return type:

```
    let function_that_never_returns = || -> Foo { loop {} };
```

Since such an annotation would only be required in cases where the function cannot return anything, however, it would be a bit pointless to require the programmer to assign a fictitious type to it. So, the compiler includes a special rule: if, after all other type inference has been done, there is still an ambiguous type that cannot be determined, just assume that it should be the designated fallback type. Prior to the 2024 [edition](https://doc.rust-lang.org/edition-guide/editions/index.html) of Rust, that fallback type was () (the unit type, which has exactly one possible value). In the 2024 edition, the fallback type was changed to ! itself, essentially canceling out the implicit conversion. In the compiler internals, the never type still gets converted to an unknown type and then falls back, but from the programmer's perspective the behavior is identical to having the never type only undergo implicit conversion when required for the types to make sense.

That change of behavior was, technically, a breaking change. Type inference for some code could change, which could in turn cause compilation errors. That is the purpose of Rust's edition system: allow breaking changes in the front-end design of the language without breaking older code or requiring the whole ecosystem to update at once. In this case, however, there were reasons to want the new behavior backported to old editions.

#### Never infallible

For many years, the standard library has had an [Infallible](https://doc.rust-lang.org/std/convert/enum.Infallible.html) type to work around the unstable nature of the never type. It served the same semantic purpose as the never type, but did not have any special compiler support. Therefore, code using it would be technically correct but suboptimal (such as having an extra layer of tags in an enumeration or emitting dead code), because the optimizer would not always be able to remove references to Infallible. It was planned that, when the never type was eventually stabilized, Infallible would become a type alias for ! and all that old code would silently become more efficient. However, people pointed out a handful of ways that redefining Infallible had accidentally been made into a breaking change. Since ! has implicit conversions, changing the definition of Infallible could result in existing code needing additional type specifiers in order to type check.

Luckily, changing the definition of Infallible and changing the default fallback type, while both breaking changes, nearly cancel out. Any code that refers to the standard library's Infallible type by name would continue to work; it is only places where type inference is implicitly expected to produce Infallible that pose a risk of breaking existing code. With Rust's lack of implicit conversions in most cases, that will most often come up in places where the never type used to be implicitly converted to Infallible. If Infallible is made to be a type alias of the never type, then those places may experience never-type fallback, which would, in turn, change the inferred type and cause a compilation error if the never fallback type were not updated at the same time.

With both changes occurring simultaneously, the Rust maintainers believed that almost all existing Rust code would continue to compile — but "almost all" is not a reassuring qualifier when dealing with backward-incompatible changes. The Rust community does have a solution to this in the form of [crater](https://github.com/rust-lang/crater#crater), which can download and compile all publicly available Rust libraries from [crates.io](https://crates.io/) in search of code that is broken by a compiler change.

#### Never say never

Waffle ran crater in April and [found](https://github.com/rust-lang/rust/pull/155499#issuecomment-4290467341) that, while there were 3,300 crates negatively impacted by the change, only seven were fully broken, with the rest broken by depending on old versions of libraries that had since been fixed. In the latter case, the problem would theoretically be fixable by releasing backported fixes for a handful of core libraries. This is not an accident; Rust has been emitting a warning whenever code triggers never-type fallback in a way that will break with the new change since 2024, so most libraries had plenty of time to update of their own initiative. The most common remaining error observed by crater is code that calls a generic function without enough type information for the compiler to pick a specific return type. Consider this function:

```
    fn foo<T: Default>() -> Result<T, Error> { ... }
```

It returns either a value of some caller-chosen type T that must implement the Default trait, or an error. If it is called without specifying a value for T, however, then type fallback can kick in:

```
    // No type specified.
    foo()?;
```

Previously, this would have made the compiler assume that T should be (), which implements Default, and so the code compiles. After this change (and on the 2024 edition), the compiler assumes that T should be !, which doesn't implement Default, and therefore causes a compilation error. The fix is to explicitly specify the type that foo() should return, either in the call or by pattern-matching assignment.

```
    foo::<()>()?;
    // or
    () = foo()?;
```

Even though it's not a complicated change, the Rust maintainers were not willing to break 3,300 crates. Waffle was [asked](https://github.com/rust-lang/rust/pull/155499#issuecomment-4307446212) to work with the maintainers of common libraries to backport simple changes like the above (making a new [patch version](https://semver.org/), which many Rust build environments will pick up automatically), in order to reduce the number of libraries depending on broken dependencies. Several library authors were [willing](https://github.com/rust-lang/rust/pull/155499#issuecomment-4400482314) to make the backports, but some refused on the grounds that those old versions were past their end of life. Those maintainers pointed out that users could stay on an older version of Rust or update to the maintained version of the library. Even so, the successful backports addressed 1,553 of the failing crates.

After fixing [a handful of related problems](https://github.com/rust-lang/rust/issues/155924) to reduce the number of broken crates even further, the Rust maintainers eventually agreed that even though there would still be some broken code it was worth making the change to simplify the language. So, starting in Rust 1.99, the never type will be stable and Infallible will be a type alias for the never type. Users who find that this breaks their code have a few options:

- Stay on Rust version 1.98.
- Update their dependencies to supported versions that include a fix for the problem.
- Add a patch to explicitly specify the return types of affected function calls.

On the one hand, this is a breaking change, and people may see code that had remained stable and working suddenly fail to compile. That could be seen as a violation of Rust's commitment to backward compatibility. On the other hand, the problem is relatively rare, there are multiple simple ways to fix it, it has been warned about for years, and it has always been part of the plan for the language. Additionally, the Rust maintainers worked directly with the community to find and address the breakage, even going so far as to help backport fixes to long-dead versions of popular libraries. So, the whole process could also be seen as an affirmation of Rust's commitment to backward compatibility.

In the future, people learning the language will hopefully find the never type just a little less special. Either way, most users of Rust will probably not be affected at all, but never say "never".

\
 \

> **Did you like this article?**? [Subscribe now](https://lwn.net/Promo/slink-d-obnoxious/claim) at the special discounted rate to get a lot more like it.

* * *
