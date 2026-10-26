---
title: We have Traits at Home
link: https://www.axelmagn.com/blog/202610-zig-traits/
source: lobste-rs
published: 2026-10-22T21:52:10Z
updated: 2026-10-22T21:52:10Z
first_seen: 2026-10-26T19:53:35.807667070Z
authors:
- axelmagn.com via Defelo
labels:
- zig
summary: Comments
content: extracted
html: 2026-10-22-we-have-traits-at-home.html
remote_preview:
  url: https://www.axelmagn.com/blog/202610-zig-traits/so_good.png
  alt: Feels Good
---

I have had more time recently to write code in Zig [\[1\]](https://www.axelmagn.com/blog/202610-zig-traits/#fn-zig-0.17), and it has been good for my soul. I wanted to share some insights I have arrived at regarding abstractions in Zig, including some techniques that I have found useful but not seen employed more broadly.

Considerable [\[2\]](https://www.axelmagn.com/blog/202610-zig-traits/#fn-seguin2023) ink [\[3\]](https://www.axelmagn.com/blog/202610-zig-traits/#fn-figuratively) has [\[4\]](https://www.axelmagn.com/blog/202610-zig-traits/#fn-wong2025) been [\[5\]](https://www.axelmagn.com/blog/202610-zig-traits/#fn-mckay2025) spilled [\[6\]](https://www.axelmagn.com/blog/202610-zig-traits/#fn-cypert2026) on Zig Interfaces, and justifiably so. While not a language feature, interfaces are the de-facto way of implementing runtime polymorphism in Zig. Yet [\[7\]](https://www.axelmagn.com/blog/202610-zig-traits/#fn-kelley2021) more [\[8\]](https://www.axelmagn.com/blog/202610-zig-traits/#fn-redig2025) has been said about zig's comptime, and how it may be leveraged to avoid the performance woes of dynamic dispatch and write slick, data-oriented code.

To put it briefly, polymorphism in Zig is conventionally obtained in one of two ways:

2. **Dynamic Interfaces**: Use opaque pointers, virtual tables, and dynamic dispatch to achieve runtime polymorphism.
3. **Comptime Duck Typing**: Use type reflection, switch statements, and monomorphism at compile time to achieve static polymorhpism.

This state of affairs has proven to be quite effective so far, but it is not without its tradeoffs.

Interfaces offer the sort of type contracts that we are used to from other languages. A function that accepts an interface has a guarantee of its method signatures, but not its implementation. However because interfaces are, by design, modular at runtime, they also introduce a hard barrier for the compiler and thus a performance overhead. There is enough indirection that it often cannot effectively apply the optimizations that it would to monomorphic code.

For cases of performance critical code, Zig code typically relies on comptime duck typing. A function argument is either `anytype` or parameterized by a type argument `T`, and the function implementation will either perform compile-time reflection or make assumptions about the type's methods that can be checked at compile-time. Implementations can then be monomorphised, similar to C++ templating or C macros. This is slightly more safe than runtime duck typing, because any interface violations will be caught at compile time rather than runtime, but the caller and callee must still be careful to document and follow the contract of what types may be passed.

If you are coming from another strongly typed language, you would be forgiven for finding this arrangement rather simplistic. This is perhaps by design -- Zig aims to be simple. However if you have grown accustomed to Rust's traits or Haskell typeclasses, there appears to be no ready equivalent in Zig.

I will argue that, with a little adaptation, it is perfectly possible to express similar abstractions over types in Zig userland. I would propose a new **convention** for defining compile-time type contracts as **Traits** in Zig. It doesn't require any libraries or machinery - in fact if fits remarkably well into vanilla Zig. It is inspired by the functional programming world in general, and OCaml in particular.

> **Definition:** a Trait is a compile-time struct which contains related functions over a given type. [\[9\]](https://www.axelmagn.com/blog/202610-zig-traits/#fn-ocaml-inspired)

So as not to bury the lead, let me begin with a brief example of how we might define a trait for folding (reducing) on a type.

```zig
/// T forms a Reducer under an empty value and a combine operation
fn Reducer(T: type) type {
    return struct {
        empty: T,
        combine: fn (T, T) T,

        fn reduce(self: @This(), xs: []const T) T {
            var accum = self.empty;
            for (xs) |x| accum = self.combine(accum, x);
            return accum;
        }
    };
}

/// i32 forms a sum reducer under 0 and addition
const sum_trait = impl(Reducer(i32), struct {
    pub const empty = 0;
    pub fn combine(x: i32, y: i32) i32 { return x + y; }
});

/// i32 forms a product reducer under 1 and multiplication
const product_trait = impl(Reducer(i32), struct {
    pub const empty = 1;
    pub fn combine(x: i32, y: i32) i32 { return x * y; }
});

export fn run(buf: [*]i32, n: usize) i32 {
    const sum = sum_trait.reduce(buf[0..n]);
    const product = product_trait.reduce(buf[0..n]);
    return sum + product;
}
```

Ugh. Isn't that just buttery smooth?[\[10\]](https://www.axelmagn.com/blog/202610-zig-traits/#fn-so-good) The contract is clear, LSP autocompletion picks it up, the compiler even knows to inline it and unroll the loop.

Traits are just bags of functions and types that tell the receiving function how to execute subtasks that it's not responsible for defining. They behave a lot like vtables, except that they are evaluated at comptime rather than runtime.

## Conventional Polymorphism vs Traits

To justify traits, let's take a tour through Zig polymorphism with a common motivating example: arithmetic operations. I've seen a few complaints over the years about how awkward is it to do math in Zig over user-defined types. In its pursuit of explicit control flow, Zig has eschewed operator overloading, and this can be jarring if you're coming from other typed languages.

Let's imagine we have some user-defined types like `Rational` and `Complex`, and we want to define their basic arithmentic operations. We would probably end up with something like the following.

```zig
const Rational = struct {
    numerator: i32,
    denominator: i32,

    fn add(lhs: Rational, rhs: Rational) Rational {...}
    fn sub(lhs: Rational, rhs: Rational) Rational {...}
    fn mul(lhs: Rational, rhs: Rational) Rational {...}
    fn div(lhs: Rational, rhs: Rational) Rational {...}
};

const Complex = struct {
    real: f32,
    imaginary: f32,

    fn add(lhs: Complex, rhs: Complex) Complex {...}
    fn sub(lhs: Complex, rhs: Complex) Complex {...}
    fn mul(lhs: Complex, rhs: Complex) Complex {...}
    fn div(lhs: Complex, rhs: Complex) Complex {...}
};
```

For the sake of having something to do with them, let's say that we want to compute the [Möbius transformation](https://en.wikipedia.org/wiki/M%C3%B6bius_transformation): `(a * x + b) / (c * x + d)`, which is useful for complex numbers as well as rationals. To keep things simple, we'll assume that all arguments to the function are of the same type.

### Comptime Duck Typing

If we were to implement the Mobius transformation naively, we could just use built-in operators.

```zig
fn mobius(T: type, x: T, a: T, b: T, c: T, d: T) T {
    return (a * x + b) / (c * x + d);
}
```

This would work fine, as long as the inputs were all floats of the same type. Division would break for integers, and of course it wouldn't work with our user-defined types, but it's a starting point.

We could of course notice that both of our user-defined types have arithmetic functions with matching names and similar signatures, which would lend themselves to duck typing as well.

```zig
fn mobius(T: type, x: T, a: T, b: T, c: T, d: T) T {
    const numerator = a.mul(x).add(b);
    const denominator = c.mul(x).add(d);
    return numerator.div(denominator);
}
```

This will work for our user-defined types, but obviously not with Zig's primitive types. We would have to wrap those in our own user-defined types with their own functions. More troublingly, this sort of ties us to a sort of object-oriented pattern: the de-facto way to obey this abstraction contract is to define methods on the struct.

The good news is that it's all monomorphised at compile time, allowing the compiler to simplify and inline these function calls.

### Interfaces & VTables

If we wanted this to work dynamically at runtime, we could perhaps declare a `Num` interface that uses a VTable and opaque pointer to dynamically dispatch these math operations.

```zig
const Num = struct {
    value: [32]u8,
    vtable: *const VTable,

    const VTable = struct {
        add: *const fn([*]u8, [*]u8) Num,
        sub: *const fn([*]u8, [*]u8) Num,
        mul: *const fn([*]u8, [*]u8) Num,
        div: *const fn([*]u8, [*]u8) Num,
    };

    fn add(lhs: Num, rhs: Num) Num { ... }
    fn sub(lhs: Num, rhs: Num) Num { ... }
    fn mul(lhs: Num, rhs: Num) Num { ... }
    fn div(lhs: Num, rhs: Num) Num { ... }

    fn mobius(x: Num, a: Num, b: Num, c: Num, d: Num) {
        const numerator = a.mul(x).add(b);
        const denominator = c.mul(x).add(d);
        return numerator.div(denominator);
    }
}
```

We would then need to wire up `Complex` and `Rational` to expose `fn num(self: *Self) Num` (or some such) that wraps our arithmentic functions, which is a little too tedious to reproduce here, but it would work fine on the happy path. You would of course need to be a bit careful: there's nothing stopping you from mixing complex and rational numbers in your function calls, and this would lead to undefined behavior. You could probably add some saftey checks with a bit of overengineering, but we won't get into that here.

Not only is this clunkier than comptime duck typing, it will also be slower. We are effectively demanding dynamic dispatch whenever we want to do a math operation, which will trip up the compiler and have a significant performance impact if we're doing a lot of math.

### Traits

When we define the mobius function, we really want to say "I don't know what type I'm dealing with, but it needs to have arithmetic operations." In this sort of case, I think it makes the most sense to just pass the functions it needs to use at compile-time. This would remove any ambiguity, at the cost of being rather explicit.

```zig
fn mobius(
    comptime T: type,
    comptime add: fn (T, T) T,
    comptime mul: fn (T, T) T,
    comptime div: fn (T, T) T,
    x: T,
    a: T,
    b: T,
    c: T,
    d: T,
) T {
    const numerator = add(mul(a, x), b);
    const denominator = add(mul(c, x), d);
    return div(numerator, denominator);
}
```

This is effectively compile-time dependency injection at a really fine-grained level. The signature is rather verbose, but there's actually a lot less boilerplate to write for `Complex` or `Rational` compared to runtime interfaces.

```zig
mobius(Complex, Complex.add, Complex.mul, Complex.div, x, a, b, c, d);
mobius(Real, Real.add, Real.mul, Real.div, x, a, b, c, d);
```

From here, it's not a huge leap to traits. We're effectively just cleaning up the function signature by gathering together the arithmetic functions into one struct.[\[11\]](https://www.axelmagn.com/blog/202610-zig-traits/#fn-num-is-simplified)

```zig
fn Num(T: type) type {
    return struct {
        add: fn (T, T) T,
        sub: fn (T, T) T,
        mul: fn (T, T) T,
        div: fn (T, T) T,
    };
}

fn mobius(T: type, num: Num(T), x: T, a: T, b: T, c: T, d: T) T {
    const numerator = num.add(num.mul(a, x), b);
    const denominator = num.add(num.mul(c, x), d);
    return num.div(numerator, denominator);
}
```

From here, I usually just declare the trait implementation as part of my type.

```zig
const Complex = struct {
    // fields...
    const num = Num(Complex) { .add = add, .sub = sub, .mul = mul, .div = div };
    // functions (add, sub, mul, div...)
};

mobius(Complex, Complex.num, x, a, b, c, d);
```

What I really like about this approach is how flexible it is, while remaining very clear about the contract between the function and type. For example, imagine that we wanted to extend our definition of `Num` to the primitive types like `i32` and `f32`. There are a bunch of ways we could do this, but I would probably just define a function that produces something appropriate for integer and float types.

```zig
fn builtinNum(T: type) Num(T) {
    const inner = struct {
        fn add(lhs: T, rhs: T) T { return lhs + rhs; }
        fn sub(lhs: T, rhs: T) T { return lhs - rhs; }
        fn mul(lhs: T, rhs: T) T { return lhs * rhs; }
    };
    const div = switch (@typeInfo(T)) {
        .int, .comptime_int => struct {
            fn div(lhs: T, rhs: T) T { return @divTrunc(lhs, rhs); }
        }.div,
        .float, .comptime_float => struct {
            fn div(lhs: T, rhs: T) T { return lhs / rhs; }
        }.div,
        else => @compileError("unsupported type: " ++ @typeName(T)),
    };
    return .{ .add = inner.add, .sub = inner.sub, .mul = inner.mul, .div = div };
}

mobius(f32, builtinNum(f32), x, a, b, c, d);
```

Traits are also transparent to the compiler, which will happily monomorphize and inline these functions. I prefer this over runtime polymorphism most of the time, especially in hot loops or for library functions that are unopinionated on data layout.

### Limitations

You'll notice that traits have to be explicitly conditioned on a known type. Unlike interfaces, there's no way to say "give me a number, I don't care what kind." For some applications, it would be nice if we could have an associated type.

```zig
fn Iter(Elem: type) type {
    return struct {
        T: type,
        next: fn (T) ?Elem,
    };
}
```

Unfortunately, the above isn't valid Zig, and it's unclear how that would even work at a language level. Instead, traits insist that the types they are operating on are known in advance.

```zig
fn Iter(T: type, Elem: type) type {
    return struct {
        next: fn (T) ?Elem,
    };
}
```

I have not yet found an ergonomic way around this. Traits do not play very well with type erasure.

## Trait Helpers

Before proceeding, I want to introduce some small helper functions that have streamlined how I work with traits.

### impl

If you adopt traits as a programming pattern and convention, you'll quickly end up defining functions and passing them around. You will have already noticed above that I define a lot of throwaway inner struct types, just so that I can pull functions out of them. This has led to the following convenience function for declaring trait implementations, which removes some of the boilerplate.

```zig
pub fn impl(comptime Trait: type, comptime Impl: type) Trait {
    comptime {
        var result: Trait = undefined;
        for (@typeInfo(Trait).@"struct".field_names) |field_name| {
            @field(result, field_name) = @field(Impl, field_name);
        }
        return result;
    }
}
```

I use this all over the place to derive trait implementations from anonymous structs. For example, we could use it to rewrite `builtinNum` to be a little more concise and functional. Note that the declarations need to be public for the type reflection to work.

```zig
fn builtinNum(T: type) Num(T) {
    return impl(Num(T), struct {
        pub fn add(lhs: T, rhs: T) T { return lhs + rhs; }
        pub fn sub(lhs: T, rhs: T) T { return lhs - rhs; }
        pub fn mul(lhs: T, rhs: T) T { return lhs * rhs; }

        pub const div = switch (@typeInfo(T)) {
            .int, .comptime_int => struct {
                fn div(lhs: T, rhs: T) T { return @divTrunc(lhs, rhs); }
            }.div,
            .float, .comptime_float => struct {
                fn div(lhs: T, rhs: T) T { return lhs / rhs; }
            }.div,
            else => @compileError("unsupported type: " ++ @typeName(T)),
        };
    });
}
```

### getTrait

Most of the time, I don't mind passing trait implementations around explicitly. I've gotten into the habit, and just don't find it to be that big of a deal. Trait functors make it pretty easy to reduce the boilerplate, and I like having explicit control over how things are implemented.

However when I'm dealing with my own structs, 99% of the time the trait implementation I'm looking for is going to be declared as a const on the struct itself. I use the function below to to easily retrieve or construct the most common cases.

```zig
fn getTrait(Trait: type, Impl: type) Trait {
    comptime {
        // try to find the trait declared explicitly as a member
        for (@typeInfo(Impl).@"struct".decl_names) |decl_name| {
            if (@TypeOf(@field(Impl, decl_name)) == Trait) {
                return @field(Impl, decl_name);
            }
        }
        // otherwise throw an error
        @compileError("trait " ++ @typeName(Trait) ++ " not found on " ++ @typeName(Impl));
    }
}
```

This is basically equivalent to saying "eh, it's duck typed. figure it out". Just because we *are* explicit, doesn't mean that we *can't* be lazy. If we want to take this full circle back to duck typing, we can even define trait functors that rely on `getTrait` to extract traits from the types they pass.

```zig
fn duckMobius(T: type) fn(T, T, T, T) T {
    comptime {
        const num = getTrait(Num(T), T);
        return makeMobius(T, num);
    }
}
```

## Trait Functors

With some helpers defined, let's return to the `Num` example.

Traits in Zig resemble traits in Rust, typeclasses in Haskell, or modules in Ocaml. In Rust or Haskell, they would be resolved from a global registry managed by the compiler. This can feel very convenient, but it violates Zig's principle of explicit control flow. Rust and Haskell can require a fair bit of hunting for where a trait is defined, especially when something's wrong and the LSP isn't resolving it properly. It can also be a contributor to long compile times. Resolving a generic trait in these languages is often equivalent to evaluating a proof, and that can take time.

Ocaml modules, however, work quite a bit like Zig traits: modules are first-class objects, and they are passed as arguments to the functions that consume them. There is no inference of what modules apply to a type - you locate them and pass them explicitly. If we were to define `Num` as a module, it might look something like the following.

```ocaml
(* signature *)
module type NUM = sig
  type t
  val add : t -> t -> t
  val sub : t -> t -> t
  val mul : t -> t -> t
  val div : t -> t -> t
end

(* Implementation on float *)
module Float : NUM with type t = float = struct
  type t = float
  let add = ( +. )
  let sub = ( -. )
  let mul = ( *. )
  let div = ( /. )
end

(* function *)
let mobius (type a) (module N : NUM with type t = a) x a b c d =
  let numerator = N.add (N.mul a x) b in
  let denominator = N.add (N.mul c x) d in
  N.div numerator denominator

(* Usage *)
let res = mobius (module Float) x a b c d)
```

Because modules in Ocaml are first-class values, they can actually take it a step further. Rather than defining `mobius` as written above, it is common in Ocaml to make use of "module functors", which are simply functions that take modules as arguments and return new modules. If we wanted to define `mobius` in terms of a given numeric type, we might instead write it:

```ocaml
(* MakeMobius receives a NUM module and returns a new module with mobius function *)
module MakeMobius (N : NUM) = struct
  let mobius x a b c d =
    let numerator = N.add (N.mul a x) b in
    let denominator = N.add (N.mul c x) d in
    N.div numerator denominator
end

(* usage *)
module FloatMobius = MakeMobius (Float)
let res = FloatMobius.mobius x a b c d
```

This is effectively currying: we are applying the module arguments now, and the value arguments later. However it derives a mobius function that doesn't need to be taking a bunch of type arguments all the time, and that module can in turn be used in later constructions. This translates straightforwardly to Zig. If we wanted to adhere strictly to the traits pattern, it might look like the following.

```zig
fn Mobius(T: type) type {
    return struct {
        mobius: fn(T, T, T, T, T) T,
    };
}
fn makeMobius(T: type, num: Num(T)) Mobius(T) {
    return impl(Mobius(T), struct {
        fn mobius(x: T, a: T, b: T, c: T, d: T) T {
            const numerator = num.add(num.mul(a, x), b);
            const denominator = num.add(num.mul(c, x), d);
            return num.div(numerator, denominator);
        }
    });
}
const complex_mobius = MakeMobius(Complex, Complex.num);
complex_mobius.mobius(x, a, b, c, d);
```

This works fine, but I think it's even simpler to move the functor into `Mobius(T)`:

```zig
fn Mobius(T: type) type {
    return struct {
        mobius: fn(T, T, T, T, T) T,

        fn make(num: Num(T)) Mobius(T) {
            return impl(Mobius(T), struct {
                fn mobius(x: T, a: T, b: T, c: T, d: T) T {
                    const numerator = num.add(num.mul(a, x), b);
                    const denominator = num.add(num.mul(c, x), d);
                    return num.div(numerator, denominator);
                }
            });
        }
    };
}
const complex_mobius = Mobius(Complex).make(Complex.num);
complex_mobius.mobius(x, a, b, c, d);
```

However for single functions like this, unless I knew it would need to be consumed by another function as a trait, I would probably be tempted to just implement it as a type-curried function rather than a full-blown trait:

```zig
fn makeMobius(T: type, num: Num(T)) fn(T, T, T, T) T {
    return struct {
        fn mobius(x: T, a: T, b: T, c: T, d: T) T {
            const numerator = num.add(num.mul(a, x), b);
            const denominator = num.add(num.mul(c, x), d);
            return num.div(numerator, denominator);
        }
    }.mobius;
}

const complexMobius = makeMobius(Complex, Complex.num);
complexMobius(x, a, b, c, d);
```

The versatility of Zig's comptime makes all of these more or less equivalent to the compiler. It's just a stylistic choice, depending on what the situation calls for.

Also, notice that technically we have already used trait functors when we defined `builtinNum`. It's not a deep, technical concept. It's just a function that constructs a trait implementation for you.

## Useful Traits

Having thoroughly introduced traits, let's talk about what we can do with them.

In this section are a few traits that I find myself reaching for with enough regularity that I keep a file around containing them. All of these are fairly humdrum (but wildly applicable) abstractions that I lifted wholecloth from other languages.

### Into / From

I need to convert one type into another all the freaking time. Often there are multiple ways to do this, depending on how you want to interpret the upstream type. I use `Into` and `From` to canonicalize methods for converting one type into another.

```zig
/// T can be converted into Out
fn Into(T: type, Out: type) type {
    return struct {
        into: fn(T) Out,
    };
}

/// T can be derived from In
fn From(T: type, In: type) type {
    return struct {
        from: fn(In) T,
    };
}
```

### Index

Sometimes, rather than iterating, we need keyed access to a value. The container could be a slice or a hash table - we don't care. For this, I use `Index`.

```zig
/// T forms an index over Key and Value
fn Index(T: type, Key: type, Value: type) type {
    return struct {
        get: fn(T, Key) ?Value,
    };
}
```

### Iterator / Iterable

Another common case: we have a function that iterates over a type, but we don't care what sort of iterable data structure we're going to be working with. We usually need two traits: one for the `Iterable` collection, and another for the `Iterator`, which keeps track of our progress through the collection.

```zig
/// T can be used to iterate over Elem elements of type
fn Iterator(T: type, Elem: type) type {
    return struct {
        next: fn(*T) ?Elem,
    };
}

/// T can return an Iter iterator over Elem elements
fn Iterable(T: Type, Elem: type, Iter: type) type {
    return struct {
        iter: fn(T) IterPair,
        pub const IterPair = struct {impl: Iter, trait: Iterator(Iter, Elem)};
    };
}
```

You'll notice that this is a little awkward, because we don't usually care what the iterator implementation is. It's probably just a usize index over an array or something. It would be nice if we could just define `Iterable(T: type, Elem: type)`. I've chosen to just let it be awkward. It's usually not a big deal.

## Words of Warning

I really don't want you to walk away from this article thinking "I need to convert all my code to use traits". Traits should not be thought of as a totalizing pattern. The last thing I want is for Zig code to start looking like Rust or Haskell.

Traits are a sometimes treat. They are useful for functions that need abstractions inside of hot loops, where you know that you want monomorphization and static dispatch. They can become legible building blocks for some neat metaprogramming tricks. However most of the time I only reach for them in a pull-based manner: I have a function where it would be useful to consume a trait, because it's undecided on some implementation detail.

Used improperly, traits can explode the size of your binary and actually make it slower. Each function parameterized on comptime parameters gets its own templated assembly, and if you nest these a few layers deep, it's easy to end up with a combinatorial explosion. This can thrash your CPU cache, and depending on the situation, easily be outperformed by dynamic dispatch and vtables.

## Words of Encouragement

Traits are ultimately handy and very powerful. I wish I saw more codebases that made use of them, especially for abstraction inside of hot loops. I like that they are a design pattern more than a language feature, and this gives you a lot of explicit control over when and how you use them. I was looking for a solution like this for a long time, and I'm glad I found it.

I'm sure that other Zig users have stumbled onto this trick, but I have yet to see it in the wild. I think that part of the reason is that it doesn't really lend itself to a library. It's more of a convention that you can choose to use or ignore.

My hope is that for someone reading, traits are the right tool for the job.

1.  Zig 0.17 at time of writing. [↩](https://www.axelmagn.com/blog/202610-zig-traits/#fr-zig-0.17-1)

2.  [Zig Interfaces](https://www.openmymind.net/Zig-Interfaces/) by Karl Seguin [↩](https://www.axelmagn.com/blog/202610-zig-traits/#fr-seguin2023-1)

3.  figuratively [↩](https://www.axelmagn.com/blog/202610-zig-traits/#fr-figuratively-1)

4.  [Zig Interface Revisited](https://williamw520.github.io/2025/07/13/zig-interface-revisited.html) by William Wong [↩](https://www.axelmagn.com/blog/202610-zig-traits/#fr-wong2025-1)

5.  [Inside Zig's new Writer](https://joegm.github.io/blog/inside-zigs-new-writer-interface/) by Joe McKay [↩](https://www.axelmagn.com/blog/202610-zig-traits/#fr-mckay2025-1)

6.  [Interfaces in Zig](https://www.bradcypert.com/interfaces-in-zig/) by Brad Cypert [↩](https://www.axelmagn.com/blog/202610-zig-traits/#fr-cypert2026-1)

7.  [A Practical Guide to Applying Data-Oriented Design](https://vimeo.com/649009599) by Andrew Kelley [↩](https://www.axelmagn.com/blog/202610-zig-traits/#fr-kelley2021-1)

8.  [Zig Comptime is Bonkers Good](https://www.scottredig.com/blog/bonkers_comptime/) by Scott Redig [↩](https://www.axelmagn.com/blog/202610-zig-traits/#fr-redig2025-1)

9.  "Traits" are directly inspired from OCaml modules, but the term "module" already refers to something else in Zig. [↩](https://www.axelmagn.com/blog/202610-zig-traits/#fr-ocaml-inspired-1)

10. ![Feels Good](https://www.axelmagn.com/blog/202610-zig-traits/so_good.png) [↩](https://www.axelmagn.com/blog/202610-zig-traits/#fr-so-good-1)

11. This is a pretty simplistic definition of a numeric trait. In practice, you would probably want to look into [std::ops](https://doc.rust-lang.org/std/ops/index.html) from Rust or [Num](https://hackage.haskell.org/package/base-4.17.0.0/docs/GHC-Num.html#t:Num) from Haskell. [↩](https://www.axelmagn.com/blog/202610-zig-traits/#fr-num-is-simplified-1)
