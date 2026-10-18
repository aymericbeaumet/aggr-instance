---
title: Java sealed classes and exhaustive pattern matching
link: https://neilmadden.blog/2026/04/24/java-sealed-classes-and-exhaustive-pattern-matching/
source: hnrss-org
published: 2026-10-18T14:47:37Z
updated: 2026-10-18T14:47:37Z
first_seen: 2026-10-18T23:27:08.867657739Z
authors:
- hnisjafx40
content: extracted
html: 2026-10-18-java-sealed-classes-and-exhaustive-pattern-matching.html
remote_preview:
  url: https://s0.wp.com/i/blank.jpg?m=1383295312i
---

Java 17 introduced [sealed classes](https://openjdk.org/jeps/409), which allow you to explicitly list the allowed sub-types of an interface or base class. For example, here’s a toy example using a sealed interface and records (inner classes are implicitly added to the permitted sub-types if an explicit list is not given):

```java
public sealed interface SealedType {
    record TypeA() implements SealedType {}
    record TypeB() implements SealedType {}

    static SealedType of(String type) {
        return switch (type) {
            case “A” -> new TypeA();
            case “B” -> new TypeB();
            default -> throw new IllegalArgumentException();
        };
    }
}
```

If you are familiar with functional programming languages with algebraic datatypes, you can view this as similar to a datatype declaration in Haskell or ML:

```haskell
data SealedType = TypeA | TypeB
```

We can then use this in a simple Main class:

```java
void main(String[] args) {
    var val = SealedType.of(args[0]);
    System.out.println(switch (val) {
        case SealedType.TypeA() -> “A”;
        case SealedType.TypeB() -> “B”;
    });
}
```

OK, not so exciting. But one thing to note here is that we didn’t have to add a `default` clause to the switch expression in our main method. This is because sealed classes (and enums) enable *exhaustiveness checking*: the compiler knows exactly what the possible cases are, and so can check if you have covered them all. If you have, then you don’t need a `default` clause. If you forget one (and don’t have a default clause), then you get a compile-time error.

This is great when you want to ensure that all uses of some type do cover all of the cases, but it does introduce a new type of breaking change: adding a new sub-type to a sealed class/interface may break consumers of that code. For example, adding a new `TypeC` case to our example will cause the main method to fail to compile due to the missing case. So if you export a sealed type in your API then adding a new subtype is a breaking change that would require a major version bump (if you’re following SemVer).

## Compile-time or runtime error?

Although Java will produce a compile-time error for a non-exhaustive switch when you compile the consumer (main in this case), it cannot do so if the consumer is not recompiled when the sealed type changes. For example, suppose that we extend our SealedType with another case:

```java
public sealed interface SealedType {
    record TypeA() implements SealedType {}
    record TypeB() implements SealedType {}
    record TypeC() implements SealedType {}

    static SealedType of(String type) {
        return switch (type) {
            case “A” -> new TypeA();
            case “B” -> new TypeB();
            case “C” -> new TypeC();
            default -> throw new IllegalArgumentException();
        };
    }
}
```

If we just recompiled SealedType.java and don’t recompile Main, then we end up with a **runtime exception** if we trigger the new case:

Here we have the new [MatchException](https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/lang/MatchException.html) being thrown. The Javadoc notes this potential issue with separate compilation, and also some corner-cases with nulls in patterns. So even if you were hoping that using sealed classes would statically ensure that you update all consumers when a new case is added, this is not the case unless you recompile everything.

## Conclusions

I think for me the conclusion is that sealed types are probably most useful *within* the implementation of a component, and are less useful when exposed in the public API that a component offers to other components (eg a library). For internal use, where you typically *are* going to recompile everything together, you get the nice properties of exhaustiveness checking and higher compile-time safety guarantees. But when used across module boundaries, you may just be introducing new ways to break code, often only detectable at runtime.

(I discovered these subtleties when reviewing the [preview support for PEM-encoded cryptographic objects](https://openjdk.org/jeps/524), which makes exactly this mistake of baking a sealed interface into a public API and recommend clients to pattern match against that type. A predict a very high chance of breakage if they ever want to add a new case).
