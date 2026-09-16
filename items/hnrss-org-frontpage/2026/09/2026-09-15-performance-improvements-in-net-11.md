---
title: Performance Improvements in .NET 11
link: https://devblogs.microsoft.com/dotnet/performance-improvements-in-net-11/
source: hnrss-org-frontpage
published: 2026-09-15T12:18:16Z
updated: 2026-09-15T12:18:16Z
first_seen: 2026-09-16T23:14:32.037729522Z
authors:
- soheilpro
summary: 'Article URL: https://devblogs.microsoft.com/dotnet/performance-improvements-in-net-11/ Comments URL: https://news.ycombinator.com/item?id=49711424 Points: 109 # Comments: 12'
content: extracted
html: 2026-09-15-performance-improvements-in-net-11.html
preview:
  file: 2026-09-15-performance-improvements-in-net-11.preview-ba9c7d225d31.webp
  width: 256
  height: 144
  color: '#4826c4'
images:
- source: https://devblogs.microsoft.com/dotnet/wp-content/uploads/sites/10/2026/09/net11perf.webp
  original:
    file: 2026-09-15-performance-improvements-in-net-11.image-39ebb554b9ad.webp
    width: 1920
    height: 1080
  color: '#3736ea'
- source: https://devblogs.microsoft.com/dotnet/wp-content/uploads/sites/10/2022/03/stoub_square-96x96.jpg
  original:
    file: 2026-09-15-performance-improvements-in-net-11.image-4676e8208fb7.jpg
    width: 96
    height: 96
  color: '#454548'
html_truncated: true
---

Before television shows like *The Office* and *Parks and Recreation* cemented the mockumentary in the minds of millions, there was Christopher Guest. He didn’t invent the genre, but he’s widely recognized as one of its most influential practitioners, and for my money, there’s none better. I’ve watched *Waiting for Guffman* and *Best in Show* more times than I can count. But the one that has stuck with me the most, the one I quote at the slightest provocation, is *This Is Spinal Tap*.

If you’ve seen it you already know where this is going (and if you haven’t, you now have weekend plans). The film is a fictional documentary about an aging English rock band named Spinal Tap, whose members are everything we picture when we picture over-the-top rock stars. In one of its more memorable scenes, the guitarist (Nigel) gives the filmmaker (Marty) a tour of his most prized gear, in particular showing off an amplifier unlike any other: its dials don’t stop at ten. That leads to what might be the single most quoted exchange in the entire movie:

> **Nigel:** “You see, most blokes, you know, will be playing at ten. You’re on ten here, all the way up, all the way up, all the way up, you’re on ten on your guitar. Where can you go from there? Where?”
>
> **Marty:** “I don’t know.”
>
> **Nigel:** “Nowhere. Exactly. What we do is, if we need that extra push over the cliff, you know what we do?”
>
> **Marty:** “Put it up to eleven?”
>
> **Nigel:** “Eleven. Exactly. One louder.”

This is .NET 11. It’s one louder, with another year’s worth of performance work having gone into making the runtime and libraries that much faster. Of course, the premise of Nigel’s special amplifier is ludicrous, as is exemplified in the subsequent few lines of dialog:

> **Marty:** “Why don’t you just make ten louder and make ten be the top number and make that a little louder?”
>
> **Nigel:** (pauses) “…these go to eleven.”

In contrast, .NET 11 is actually one higher, one louder. The sections that follow are full of real improvements. A bounds check removed, an allocation that no longer happens, a lock that isn’t taken, a loop that runs in fewer cycles than it did a year ago, a comparison folded to a constant here, a redundant check hoisted out of a loop there, a couple of instructions fused into one, a syscall sidestepped, an array copy handed off to SIMD, and on and on. That’s how real performance work goes, accumulating gain after gain, each compounding on the last, until the whole thing is measurably, provably louder. And so, in this post, as I’ve done in past years with [.NET 10](https://devblogs.microsoft.com/dotnet/performance-improvements-in-net-10/), [.NET 9](https://devblogs.microsoft.com/dotnet/performance-improvements-in-net-9/), [.NET 8](https://devblogs.microsoft.com/dotnet/performance-improvements-in-net-8/), [.NET 7](https://devblogs.microsoft.com/dotnet/performance_improvements_in_net_7/), [.NET 6](https://devblogs.microsoft.com/dotnet/performance-improvements-in-net-6), [.NET 5](https://devblogs.microsoft.com/dotnet/performance-improvements-in-net-5), [.NET Core 3.0](https://devblogs.microsoft.com/dotnet/performance-improvements-in-net-core-3-0), [.NET Core 2.1](https://devblogs.microsoft.com/dotnet/performance-improvements-in-net-core-2-1), and [.NET Core 2.0](https://devblogs.microsoft.com/dotnet/performance-improvements-in-net-core) before it, we’ll take an unhurried tour through hundreds of them.

This is a long one. It’s meant to be. Grab your hot beverage of choice, settle in, and let’s turn it up.

## Benchmarking Setup

As in previous years, the post is chock full of micro-benchmarks that demonstrate the individual improvements. Almost all of them use [BenchmarkDotNet](https://www.nuget.org/packages/BenchmarkDotNet), and each is written to be self-contained so you can try it out yourself.

Start by ensuring you have both [.NET 10](https://dotnet.microsoft.com/download/dotnet/10.0) and [.NET 11](https://dotnet.microsoft.com/download/dotnet/11.0) installed (most of the benchmarks compare the same code running on both versions) and create a new console project in a fresh `benchmarks` directory:

```
dotnet new console -o benchmarks
cd benchmarks
```

Replace the contents of the generated `benchmarks.csproj` with the following, which multi-targets both versions so that BenchmarkDotNet can build for each:

```
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFrameworks>net11.0;net10.0</TargetFrameworks>
    <LangVersion>preview</LangVersion>
    <ImplicitUsings>enable</ImplicitUsings>
    <Nullable>enable</Nullable>
    <AllowUnsafeBlocks>true</AllowUnsafeBlocks>
    <ServerGarbageCollection>true</ServerGarbageCollection>
    <SystemPackageVersion Condition="'$(TargetFramework)' == 'net10.0'">10.0.12</SystemPackageVersion>
    <SystemPackageVersion Condition="'$(TargetFramework)' == 'net11.0'">11.0.0-rc.1.26425.128</SystemPackageVersion>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="BenchmarkDotNet" Version="0.16.0-preview.1" />
    <PackageReference Include="System.IO.Hashing" Version="$(SystemPackageVersion)" />
    <PackageReference Include="System.Runtime.Caching" Version="$(SystemPackageVersion)" />
    <PackageReference Include="System.Numerics.Tensors" Version="$(SystemPackageVersion)" />
  </ItemGroup>

</Project>
```

For a given benchmark to test, copy its complete contents over everything in `Program.cs` and then run it. Each benchmark includes as a comment at the top the exact command to use. In most cases, it’s:

```
dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0
```

which builds in Release and runs the benchmark against both .NET 10 and .NET 11, emitting a side-by-side comparison. The other common form, used when a benchmark is comparing two coding approaches on a single runtime (rather than the same code across two runtimes) is:

```
dotnet run -c Release -f net11.0 --filter "*"
```

The usual disclaimer applies: these are micro-benchmarks, many measuring operations so short that a blink would miss them. Your results will vary with your hardware, OS, runtime configuration, what else your machine happens to be doing at that exact moment, and whether Mercury is in retrograde.

Every line of managed code ultimately ends up at the just-in-time compiler, so let’s start there.

## JIT

Of all the places to improve .NET’s performance, few have as broad an impact as the just-in-time (JIT) compiler. C#, F#, and Visual Basic are typically compiled first to intermediate language (IL), and the JIT ultimately turns that IL into the native instructions the CPU executes. A JIT improvement can therefore benefit application and library code wherever the optimized pattern occurs, often with no source changes or recompilation of the application itself. Even removing a single instruction or proving one check unnecessary can add up when the code is on a very hot path.

### Deabstraction

We as developers love our abstractions. They let us write clean, reusable, object-oriented code, but we don’t want to pay for every abstraction at run time. The runtime can often undo an abstraction when it proves the effects aren’t observable. It can look at a virtual call and determine which concrete method it’ll invoke, look at a heap allocation and recognize that the object never leaves the current stack frame, or look at an interface cast and reuse a type fact already established earlier in the method. This process is called “deabstraction.” .NET has improved steadily in this area for years, and that continues in .NET 11.

Every time you write `interface` in C#, you’re creating a contract, a promise that any type implementing that interface can be substituted for any other. That flexibility is enormously valuable because, for example, it’s what lets us write `IEnumerable<T>` and have it work equally well over arrays, lists, other collections, LINQ, custom iterators, and so on. But the CPU doesn’t know anything about these contracts; it just knows how to execute instructions. Turning “call whatever method this interface reference points to” into actual machine instructions requires special machinery. Consider this example:

```
// dotnet run -c Release -f net11.0 --filter "*"

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;
using System.Runtime.CompilerServices;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[DisassemblyDiagnoser, HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private Animal _animal = Environment.TickCount >= 0 ? new Dog() : new Cat();

    [Benchmark]
    public int Speak() => _animal.Speak();

    public abstract class Animal
    {
        public abstract int Speak();
    }

    private sealed class Dog : Animal
    {
        [MethodImpl(MethodImplOptions.NoInlining)]
        public override int Speak() => 1;
    }

    private sealed class Cat : Animal
    {
        [MethodImpl(MethodImplOptions.NoInlining)]
        public override int Speak() => 2;
    }
}
```

At compile time, all else equal, the JIT doesn’t know whether `_animal` is a `Dog` or a `Cat`. It generates code that loads the instance’s “method table pointer” (its object type handle), sometimes called a “vtable pointer”, stored at the beginning of every .NET object, indexes into the method table at the known slot for `Speak`, and calls the function pointer found there:

```
; x64
mov     rcx, [rcx+8]   ; load _animal
mov     rax, [rcx]     ; load method table
mov     rax, [rax+40]  ; load vtable chunk
call    qword ptr [rax+20]
```

For this one call to `Speak`, we pay three dependent memory dereferences and an indirect call because the processor doesn’t know for certain in advance where the call is going (it might guess, or “speculatively execute”, but it has to be prepared for the possibility it was wrong), and because the call target is indirect, the JIT can’t inline the callee. Whatever `Speak` does, its code can’t be folded into the calling method.

That’s a performance problem. Those indirections have overhead, but the bigger cost is the lost opportunity to inline. Inlining not only saves function call overhead, more importantly it opens the callee’s code up to the same optimizations that are operating on the caller, such as constant propagation, dead code elimination, bounds check elimination, further devirtualization, etc. That means a series of small virtual calls that each look innocent can, when devirtualized and inlined, collapse into a handful of instructions that would be unrecognizable and way cheaper when compared to the original source code. Without inlining, each callee is an opaque box; with it, the JIT can see through the layers.

We as .NET developers constantly rely on the JIT’s sophisticated heuristics for inlining that weigh the IL size of the callee, the exact work the callee is performing, the call frequency of the method, the expected benefit from constant arguments, and dozens of other factors. For virtual calls, the JIT needs to know what the actual target of the call will be; it needs to “devirtualize”. In some cases, it can determine that statically, where it has exact-type knowledge. For example, if the JIT can prove that `animal` is always a `Dog`, whether because it was just allocated with `new Dog()`:

```
Animal animal = GetSomeAnimal();
animal.Speak();
...
static Animal GetSomeAnimal() => new Dog(); // inlineable
```

or because the variable’s type is a sealed class:

```
Dog animal = GetSomeAnimal();
animal.Speak();
...
sealed class Dog { ... } // impossible for `animal` to be anything other than a `Dog`
```

or with NativeAOT and whole-program compilation, if it sees that `Animal` is abstract and the only type in the whole application that derives from `Animal` is `Dog`:

```
Animal animal = GetSomeAnimal();
animal.Speak();
...
abstract class Animal { ... }
class Dog : Animal { ... } // no other such derived type
```

or other such validation, it can emit a call to `Dog.Speak()` directly, and the inliner can take its shot.

But for other cases where it can’t prove this with static analysis, the JIT turns to profile-guided optimization (PGO). PGO sounds fancy, but it’s conceptually simple. With “tiered compilation”, when a method is first invoked, it can be compiled “just in time” with few-to-no optimizations (this is referred to as Tier 0). The JIT can include in this compilation additional probes (think “printf debugging”) that let it track a bunch of interesting information about the nature of the code, recording what actually happens when it runs: which branches are taken, what are the concrete types that show up at virtual call sites or cast attempts, and so on. If the method is invoked enough or loops enough times, the runtime can ask the JIT to produce a new optimized version (referred to as Tier 1). That compilation can then factor in all of the learnings gathered as part of that profiling.

The JIT, of course, still needs to generate code that’s always correct. Even if a dynamic profile says `animal` was `Dog` 100% of the time, that doesn’t guarantee it’ll always be `Dog` in the future; it could be that the first 1000 calls passed in a `Dog` but the 1001st call is going to pass in `Dolphin`. How can the JIT incorporate this learning then? By emitting a run-time check. The `Dog` path can get a direct call, which may then be inlinable, and the other path keeps the original virtual call as the fallback. The speed comes from making the common case tiny, while correctness comes from leaving the uncommon case intact.

```
// Approximately what the JIT generates
if (animal?.GetType() == typeof(Dog))
{
    ((Dog)animal).Speak();  // devirtualized, inlinable
}
else
{
    animal.Speak(); // original virtual call, hopefully rare
}
```

This “guess and verify” pattern, called “guarded devirtualization” (GDV), accounts for many of the biggest throughput wins in real workloads. It’s applicable not only to virtual dispatch but also to interface dispatch, which also happens to be a bit more expensive than virtual dispatch because a type can implement any number of interfaces and that means the interface slots don’t simply map to fixed vtable positions.

Deabstraction can also make object creation more efficient when it reveals what kind of object is involved. In general, objects in .NET are allocated on the garbage collected heap, tracked by the garbage collector (GC), and collected when no longer reachable. Heap allocation is typically fast, often effectively just bumping a pointer. However, when there’s not enough space available to bump the pointer, it can get much more expensive, including needing to incur a garbage collection. Every allocated object also effectively incurs the amortized cost of all collections, as every allocated object eventually needs to be cleaned up.

“Escape analysis” is the compiler technique that lets us ask whether this object ever “escapes” the current method. If an object reference to a newly allocated object provably doesn’t escape, then the JIT can more efficiently allocate it. It needn’t store it on the GC heap, because nothing could possibly need to reference that object again, so it can instead allocate the object on the stack, making both allocation and cleanup essentially free. Stack allocation is even faster than heap bump-pointer allocation; it’s just decrementing the stack pointer, which is typically already in a register. And more importantly it means zero GC impact, because the stack frame is freed atomically on function return.

The JIT’s been progressively expanding escape analysis over the past several .NET releases, with .NET 9 and 10 seeing significant investments in stack-allocating delegates and closures, `Nullable<T>` temporaries, and small helper objects. The key theme is that every false positive escape, every time the JIT incorrectly concludes an object may escape when it really doesn’t, represents a heap allocation that could have been avoided, and we want to whittle away at that false positive list. In .NET 11, the JIT trims that list in several ways.

We’ll start with nullable boxing. Consider this benchmark:

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[MemoryDiagnoser(false), HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private int? _nullableNull;
    private int? _nullableValue = 42;

    [Benchmark]
    public object? BoxNullableNull() => (object?)_nullableNull;

    [Benchmark]
    public object? BoxNullableValue() => (object?)_nullableValue;

    [Benchmark]
    public string? FormatNullableInt() => Format(_nullableValue);

    private static string? Format<T>(T value)
    {
        if (value is IFormattable formattable)
            return formattable.ToString(null, null);

        return null;
    }
}
```

| Method            | Runtime   | Mean     | Ratio | Allocated | Alloc Ratio |
| ----------------- | --------- | -------- | ----- | --------- | ----------- |
| BoxNullableNull   | .NET 10.0 | 2.095 ns | 1.00  | –         | –           |
| BoxNullableNull   | .NET 11.0 | 1.764 ns | 0.84  | –         | –           |
| BoxNullableValue  | .NET 10.0 | 9.213 ns | 1.00  | 24 B      | 1.00        |
| BoxNullableValue  | .NET 11.0 | 4.126 ns | 0.45  | 24 B      | 1.00        |
| FormatNullableInt | .NET 10.0 | 9.583 ns | 1.00  | 24 B      | 1.00        |
| FormatNullableInt | .NET 11.0 | 1.987 ns | 0.21  | –         | 0           |

[dotnet/runtime#122167](https://github.com/dotnet/runtime/pull/122167) expands nullable boxing inside the JIT, exposing the temporary box to escape analysis; previously, a runtime helper hid it. For a `null` input, there’s no allocation on either version, because nothing gets boxed. And on both versions, `BoxNullableValue` returns the boxed object, meaning the object escapes, so the 24-byte allocation remains. However, for `FormatNullableInt`, the JIT in .NET 11 can now see that the temporary 24-byte box doesn’t escape and eliminates that heap allocation entirely.

Escape analysis improved further for enumerators, through a mechanism called Conditional Escape Analysis (CEA). Support for CEA was introduced in .NET 10, but .NET 11 extends the set of patterns that this analysis can safely recognize. The existing escape analysis asks whether a reference created by an allocation can flow somewhere the JIT can no longer track, such as an unknown call. If it can, the object must remain on the heap. That analysis is necessarily conservative and largely flow-insensitive: if an object might be passed to an interface call on any path, it doesn’t try to prove that the path containing that call is mutually exclusive with the path containing the allocation.

Unfortunately, that’s exactly what GDV produces when it optimizes a `foreach` over an `IEnumerable<T>`. As noted earlier, GDV turns an interface call into a type check with two branches: a fast branch for the likely collection type and a fallback branch containing the original interface call. Devirtualization and inlining along the fast branch will often reveal an enumerator allocation for the collection type, while later enumerator guards retain fallback calls such as `IEnumerator<T>.MoveNext`. The existing analysis sees those calls and concludes that the locally allocated enumerator might escape. CEA instead records the relationship between the fast-path allocation and the enumerator local tested by the later guards. If every apparent escape occurs only behind a failed type check, the JIT can clone the region into a hot version where those checks are known to succeed. In that clone, the object can’t reach the fallback calls, so it can be stack-allocated and often promoted into separate scalar locals. The original region remains as the general slow path.

One case .NET 10 didn’t handle, though, was a `GetEnumerator()` implementation that returns the result of another `GetEnumerator()` call. A collection expression converted to `IEnumerable<int>`, for example, uses a compiler-generated read-only-array wrapper with exactly this structure: the wrapper’s `GetEnumerator()` delegates to the underlying array’s `GetEnumerator`. With [dotnet/runtime#122946](https://github.com/dotnet/runtime/pull/122946), the JIT in .NET 11 handles this “chaining”:

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[MemoryDiagnoser(false), HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private static readonly IEnumerable<int> s_readOnlyStatic = [1, 2, 3, 4, 5];
    private readonly IEnumerable<int> _readOnlyInstance = [1, 2, 3, 4, 5];

    [Benchmark]
    public int ReadOnlyStatic()
    {
        int sum = 0;
        foreach (int item in s_readOnlyStatic) sum += item;
        return sum;
    }

    [Benchmark]
    public int ReadOnlyInstance()
    {
        int sum = 0;
        foreach (int item in _readOnlyInstance) sum += item;
        return sum;
    }
}
```

| Method           | Runtime   | Mean      | Ratio | Allocated | Alloc Ratio |
| ---------------- | --------- | --------- | ----- | --------- | ----------- |
| ReadOnlyStatic   | .NET 10.0 | 2.665 ns  | 1.00  | –         | –           |
| ReadOnlyStatic   | .NET 11.0 | 2.666 ns  | 1.00  | –         | –           |
| ReadOnlyInstance | .NET 10.0 | 13.874 ns | 1.00  | 32 B      | 1.00        |
| ReadOnlyInstance | .NET 11.0 | 2.674 ns  | 0.19  | –         | 0           |

`ReadOnlyStatic`, whose `static readonly` field the JIT can effectively treat as a constant, was already optimized in .NET 10. In .NET 11, the instance-field case also loses its 32-byte enumerator allocation and converges on the same throughput.

[dotnet/runtime#121918](https://github.com/dotnet/runtime/pull/121918) from [@MichalPetryka](https://github.com/MichalPetryka) fixes another way an address could unnecessarily make an object appear to escape. The IL `constrained.` prefix lets one generic `callvirt` sequence work for both value types and reference types: it can avoid boxing a value type, while for a reference type it dereferences the receiver and performs normal virtual dispatch. `ObjectEqualityComparer<T>.Equals`, used in the following benchmark by `EqualityComparer<T>.Default`, contains such a call to `value.Equals(other)`. The receiver was represented as an indirect read through the address of a local. Merely taking that address marked the local as exposed, preventing the newly allocated `Value` from being considered for stack allocation. The receiver is now represented as a direct value load instead, and the 24-byte heap allocation disappears.

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;
using System.Collections.Generic;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[MemoryDiagnoser(false), HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private static readonly Value s_other = new(42);

    [Benchmark]
    public bool Equals() => EqualityComparer<Value>.Default.Equals(new Value(42), s_other);

    private sealed class Value(int value)
    {
        private readonly int _value = value;

        public override bool Equals(object? obj) => obj is Value other && _value == other._value;

        public override int GetHashCode() => _value;
    }
}
```

| Method | Runtime   | Mean     | Ratio | Allocated | Alloc Ratio |
| ------ | --------- | -------- | ----- | --------- | ----------- |
| Equals | .NET 10.0 | 3.874 ns | 1.00  | 24 B      | 1.00        |
| Equals | .NET 11.0 | 1.786 ns | 0.46  | –         | 0           |

While CEA can move a non-escaping object off the GC heap, sometimes the JIT can go further and prove an allocation need not exist at all. Generic code provides a common source of such opportunities through boxing. For example, the `ArgumentNullException.ThrowIfNull` method accepts an `object value`. That means when you have a method like this:

```
static void Test<T>(T value)
{
    ArgumentNullException.ThrowIfNull(value);
    ...
}
```

when `T` is constrained to a non-nullable struct, boxing is incurred, in order to pass `value` as `object`. `ThrowIfNull` here is a nop if `value` is non-`null` (since the method is simply `if (value is null) Throw();`), and previous releases successfully optimized away that boxing in optimized code. However, in Tier 0, that optimization wasn’t applied, and `ThrowIfNull` would end up allocating. While this wouldn’t negatively impact steady-state throughput, it would lead to annoying noise in profiling, as well as additional overhead during startup, where such use wasn’t yet promoted out of Tier 0. In .NET 11, [dotnet/runtime#129392](https://github.com/dotnet/runtime/pull/129392) adds support for this in Tier 0 as well.

On the virtual-dispatch side, multiple PRs contribute to improving generic virtual methods (GVMs). [dotnet/runtime#120866](https://github.com/dotnet/runtime/pull/120866) from [@hez2010](https://github.com/hez2010) stops eagerly spilling `ldvirtftn` call targets into a temporary, and lets generic virtual target resolution move ahead of argument setup when legal. [dotnet/runtime#122023](https://github.com/dotnet/runtime/pull/122023) from [@hez2010](https://github.com/hez2010) then enables the JIT to devirtualize non-shared GVMs, carrying the generic context needed to turn the indirect dispatch into a direct, and potentially inlineable, call. And [dotnet/runtime#128702](https://github.com/dotnet/runtime/pull/128702) from [@hez2010](https://github.com/hez2010) extends that support to shared GVMs and default interface implementations that require an instantiating stub. These optimizations can increase total code size when the newly direct calls are inlined, but that’s generally the desired trade: more of the actual work becomes visible to the optimizer. Consider the following benchmark:

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;
using System.Runtime.CompilerServices;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[MemoryDiagnoser(false), HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    [Benchmark]
    public int NonShared() => ((IProcessor)new Processor()).SizeOf(42);

    [Benchmark]
    public int Shared() => ((IProcessor)new Processor()).SizeOf("hello");

    private interface IProcessor
    {
        int SizeOf<T>(T value);
    }

    private sealed class Processor : IProcessor
    {
        public int SizeOf<T>(T value) => Unsafe.SizeOf<T>();
    }
}
```

Casting a freshly allocated `Processor` to `IProcessor` incurs an interface generic virtual call in the IL, but the JIT is now able to see the receiver’s exact type, even in the shared `string` case, such that .NET 11 devirtualizes and inlines both calls. That in turn exposes `Unsafe.SizeOf<T>()` as a constant and proves that the short-lived `Processor` doesn’t need to be allocated at all.

| Method    | Runtime   | Mean     | Ratio | Allocated | Alloc Ratio |
| --------- | --------- | -------- | ----- | --------- | ----------- |
| NonShared | .NET 10.0 | 6.678 ns | 1.00  | 24 B      | 1.00        |
| NonShared | .NET 11.0 | 1.764 ns | 0.26  | –         | 0           |
| Shared    | .NET 10.0 | 7.166 ns | 1.00  | 24 B      | 1.00        |
| Shared    | .NET 11.0 | 1.764 ns | 0.25  | –         | 0           |

Building on that, [dotnet/runtime#123183](https://github.com/dotnet/runtime/pull/123183) from [@hez2010](https://github.com/hez2010) enables ReadyToRun compilation to resolve and devirtualize more non-shared generic virtual calls that would otherwise remain indirect, and [dotnet/runtime#130202](https://github.com/dotnet/runtime/pull/130202) from [@hez2010](https://github.com/hez2010) extends that support to NativeAOT. NativeAOT represents some generic virtual targets as “fat pointers” (pointers that are more than just an address, typically an address and associated metadata, and that in this case carry both a code address and generic context); by deferring that transformation until after exact-type devirtualization has had a chance to run, the JIT can turn an interface call site with a single known target to a non-shared GVM into a direct call that may then be inlined.

Type information also needs to survive the transformations the JIT performs internally. If the JIT spills a reference expression into a temporary while restructuring a tree, losing the expression’s exact class information can turn a call that was devirtualizable back into an opaque virtual call. That’s what happens here in .NET 10: `Value` gets boxed and `SetValue` is invoked through `IValue`. [dotnet/runtime#128485](https://github.com/dotnet/runtime/pull/128485) from [@hez2010](https://github.com/hez2010) preserves the class handle and exactness on the temporary. With that information still available, .NET 11 devirtualizes and inlines the call, eliminating the box and its 24-byte allocation.

Separately, [dotnet/runtime#127433](https://github.com/dotnet/runtime/pull/127433) relaxes the inliner’s budget heuristics for callees on `[Intrinsic]` types like `Span` and `Vector`. These types intentionally expose many small, composable methods that serve as gateways to JIT-recognized operations. If a wrapper remains as a call, the caller pays the call overhead and optimizations around it see an opaque boundary. If it inlines, the importer can replace its body with an intrinsic node and optimize that node together with the surrounding indexing, bounds checks, and vector operations. Giving such wrappers more favorable budgeting therefore keeps more of them inlineable and exposes more of the actual operation to the rest of the optimizer.

One of the core abstraction-enabling mechanisms in .NET is delegates: they let us pass around objects representing functions to be invoked, carrying with them associated required state. Deabstraction enables avoiding paying for the overheads associated with delegates in some cases. For the rest, we still want those delegates to be as cheap as possible. [dotnet/runtime#99200](https://github.com/dotnet/runtime/pull/99200) from [@MichalPetryka](https://github.com/MichalPetryka) simplifies CoreCLR’s delegate representation, removing one pointer-sized field from every delegate object. That saves 8 bytes per delegate in a 64-bit CoreCLR process. [dotnet/runtime#129304](https://github.com/dotnet/runtime/pull/129304) from [@MichalPetryka](https://github.com/MichalPetryka) improves Native AOT’s delegate layout separately by reordering its existing four fields so related values are adjacent. The updated layouts also give equality and hash-code operations more direct access to the method identity they need.

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[MemoryDiagnoser(false), HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private static readonly Target s_target = new();
    private static readonly Func<int> s_first = s_target.GetValue;
    private static readonly Func<int> s_second = s_target.GetValue;

    [Benchmark]
    public Func<int> ClosedInstance() => s_target.GetValue;

    [Benchmark]
    public bool DelegateEquals() => s_first.Equals(s_second);

    [Benchmark]
    public int DelegateGetHashCode() => s_first.GetHashCode();

    private sealed class Target
    {
        public int GetValue() => 42;
    }
}
```

| Method              | Runtime   | Mean     | Ratio | Allocated | Alloc Ratio |
| ------------------- | --------- | -------- | ----- | --------- | ----------- |
| ClosedInstance      | .NET 10.0 | 7.395 ns | 1.00  | 64 B      | 1.00        |
| ClosedInstance      | .NET 11.0 | 6.844 ns | 0.93  | 56 B      | 0.88        |
| DelegateEquals      | .NET 10.0 | 3.254 ns | 1.00  | –         | –           |
| DelegateEquals      | .NET 11.0 | 2.215 ns | 0.68  | –         | –           |
| DelegateGetHashCode | .NET 10.0 | 5.623 ns | 1.00  | –         | –           |
| DelegateGetHashCode | .NET 11.0 | 3.741 ns | 0.67  | –         | –           |

[dotnet/runtime#129410](https://github.com/dotnet/runtime/pull/129410) from [@MichalPetryka](https://github.com/MichalPetryka) follows up on the CoreCLR layout by placing the target object and method pointer next to each other. Those are commonly consumed together during invocation, and the adjacency enables paired loads on architectures such as Arm64.

### Runtime Async

For more than a decade, `async` and `await` have let us write asynchronous code that looks remarkably similar to synchronous code: we can put a `try`/`catch` around an `await`, use local variables on either side of it, return a value and generally reason about the method in source order. When execution reaches an `await` for something that isn’t yet complete, however, the method can’t simply leave its current stack frame in place and wait for the operation to finish. The thread needs to be freed up to do other work, while the work after the `await`, including whatever local state it will need later, must survive somewhere. In C#, the compiler has traditionally been responsible for transforming the method into a representation that enables that continuation.

I went into the history and mechanics of that transformation in [How async/await really works](https://devblogs.microsoft.com/dotnet/how-async-await-really-works/). The very short version is that the compiler traditionally replaces an `async` method with a small entry method and a generated state machine whose `MoveNext` method contains the transformed user code. Parameters, locals that need to survive an incomplete await, spilled expression values, awaiters, the current state number, and a method builder all become fields on a heap-allocated object. The generated `MoveNext` method runs the user’s code until an awaiter reports that it isn’t yet complete. It stores enough information to know where and with what values to resume, registers `MoveNext` as the continuation, and returns. When the operation completes, `MoveNext` is invoked again, jumps to the right location based on the saved state number (think `goto` and a label), retrieves the result from a value-producing awaiter, and continues. If every awaiter is already complete, `MoveNext` can run all the way through synchronously. When the method completes or throws, the builder publishes the result, cancellation, or exception through the returned `Task`, `Task<T>`, `ValueTask`, or `ValueTask<T>` (or, in the rare case, a custom task-like type).

For example, consider this tiny method:

```
static async Task<int> ReadLengthAsync(Stream stream, CancellationToken cancellationToken)
{
    var buffer = new byte[4096];
    int bytesRead = await stream.ReadAsync(buffer, 0, buffer.Length, cancellationToken);
    return bytesRead;
}
```

While the code that gets generated for this changes over time and differs between debug and release builds, the lowering by the C# compiler has looked something like this:

```
[AsyncStateMachine(typeof(<ReadLengthAsync>d__0))]
static Task<int> ReadLengthAsync(Stream stream, CancellationToken cancellationToken)
{
    <ReadLengthAsync>d__0 stateMachine = default;
    stateMachine.builder = AsyncTaskMethodBuilder<int>.Create();
    stateMachine.state = -1;
    stateMachine.stream = stream;
    stateMachine.cancellationToken = cancellationToken;
    stateMachine.builder.Start(ref stateMachine);
    return stateMachine.builder.Task;
}

struct <ReadLengthAsync>d__0 : IAsyncStateMachine
{
    public int state;
    public AsyncTaskMethodBuilder<int> builder;
    public Stream stream;
    public CancellationToken cancellationToken;

    private TaskAwaiter<int> awaiter;

    public void MoveNext()
    {
        int result;
        try
        {
            TaskAwaiter<int> localAwaiter;

            if (state != 0)
            {
                byte[] buffer = new byte[4096];
                localAwaiter = stream.ReadAsync(buffer, 0, buffer.Length, cancellationToken).GetAwaiter();
                if (!localAwaiter.IsCompleted)
                {
                    state = 0;
                    awaiter = localAwaiter;
                    builder.AwaitUnsafeOnCompleted(ref localAwaiter, ref this);
                    return;
                }
            }
            else
            {
                localAwaiter = awaiter;
                awaiter = default;
                state = -1;
            }

            result = localAwaiter.GetResult();
        }
        catch (Exception e)
        {
            state = -2;
            builder.SetException(e);
            return;
        }

        state = -2;
        builder.SetResult(result);
    }
}
```

That’s quite a lot of generated code for three lines of C#. The compiler has to make decisions before the program runs about the state-machine layout, which values might need to survive, how many awaiter fields are required, and how all the suspension points fit into one `MoveNext` dispatch. The runtime and JIT have optimized the resulting pattern heavily over the years, including combining the task, state machine, continuation, and `ExecutionContext` into a single allocation, but by the time the JIT sees the IL, the transformation has already happened, leaving it with a very complicated system to try to optimize.

.NET 11 introduces a new way to split that responsibility, a reimplementation of the `async`/`await` infrastructure referred to as “runtime async”. Rather than the C# compiler being responsible for the transformation, the JIT is. The C# compiler emits a much smaller suspension-aware IL contract for each eligible `async` method and marks the method as `async` in metadata. The runtime and JIT then do the work that depends on runtime knowledge: creating the externally visible `Task` or `ValueTask`, recognizing direct async calls, deciding which values are actually alive at each suspension point, laying out continuation objects, and generating the control flow that suspends and resumes the method. Effectively, the transformation moves from C# to the runtime, where more information is available to optimize it.

The programming model hasn’t changed. This is still C# `async`/`await`; `await` still obeys the awaiter pattern, exceptions and cancellation still surface through the returned task-like object, `ConfigureAwait` still has its usual meaning, synchronous completion is still synchronous completion, and on and on. An explicit goal for the feature has been 100% behavioral compatibility: whether an `async` method is lowered by the language compiler or by the runtime is an implementation detail, and any observable semantic difference is a bug.

In .NET 11, application code opts in with a compiler feature switch:

```
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <TargetFramework>net11.0</TargetFramework>
    <Features>$(Features);runtime-async=on</Features>
  </PropertyGroup>
</Project>
```

Note that there’s no new C# syntax involved, so `LangVersion=preview` isn’t required, nor is `EnablePreviewFeatures`. While this is opt-in at the application layer, most of the in-box shared framework is already built this way for .NET 11. The `async`/`await` performance goal for .NET 11 is parity with .NET 10, and in general runtime async is already as good as or better than the older implementation in many important paths. It isn’t yet fully optimized, though, and there are known cases where it still produces less efficient code. I’d encourage you to experiment in .NET 11 with opting-in your applications and services; just make sure to measure. My hope is that it’ll be on by default starting in .NET 12.

Moving the transformation from the C# compiler to the runtime has the added benefit of reducing binary size. As noted, the traditional lowering emits an entry method, a generated state-machine type, fields for captured state, and a `MoveNext` body, for every async method. Runtime async leaves a much smaller method body for the runtime to transform. The following tiny app contains ten `Task<int>`-returning async methods, each awaiting the next, and compiles the same source once with compiler lowering and once with runtime async:

```
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>net11.0</TargetFramework>
    <AssemblyName>SizeProbe</AssemblyName>
    <ImplicitUsings>enable</ImplicitUsings>
    <Nullable>enable</Nullable>
    <Features Condition="'$(RuntimeAsync)' == 'true'">$(Features);runtime-async=on</Features>
  </PropertyGroup>
</Project>
```

```
// dotnet build -c Release -p:RuntimeAsync=false -o classic --no-incremental; dotnet build -c Release -p:RuntimeAsync=true -o runtime --no-incremental; Get-Item .\classic\SizeProbe.dll, .\runtime\SizeProbe.dll | Select-Object Directory, Length

Console.WriteLine(await Benchmarks.Layer0());

public class Benchmarks
{
    public static async Task<int> Layer0() => await Layer1();
    private static async Task<int> Layer1() => await Layer2();
    private static async Task<int> Layer2() => await Layer3();
    private static async Task<int> Layer3() => await Layer4();
    private static async Task<int> Layer4() => await Layer5();
    private static async Task<int> Layer5() => await Layer6();
    private static async Task<int> Layer6() => await Layer7();
    private static async Task<int> Layer7() => await Layer8();
    private static async Task<int> Layer8() => await Layer9();

    private static async Task<int> Layer9()
    {
        await Task.Yield();
        return 42;
    }
}
```

| Lowering      | SizeProbe.dll | Ratio |
| ------------- | ------------- | ----- |
| Compiler      | 10,752 bytes  | 1.00  |
| Runtime async | 5,632 bytes   | 0.52  |

For a method such as:

```
static async Task<int> CallerAsync() => await CalleeAsync();
```

with runtime async enabled, the C# compiler generates IL like the following:

```
; MSIL
.method private hidebysig static
    class System.Threading.Tasks.Task`1<int32> CallerAsync() cil managed async
{
    call class System.Threading.Tasks.Task`1<int32> CalleeAsync()
    call int32 System.Runtime.CompilerServices.AsyncHelpers::Await<int32>(
        class System.Threading.Tasks.Task`1<int32>)
    ret
}
```

There is no generated `<CallerAsync>d__0` type, no `IAsyncStateMachine`, no `MoveNext`, no `AsyncTaskMethodBuilder<int>`, and no `AsyncStateMachineAttribute`. Previously, `async` on a C# method evaporated at compile time. Now, the method has a new `MethodImpl` `async` bit, represented in IL assembly syntax by that `async` modifier, and the body calls helpers in `System.Runtime.CompilerServices.AsyncHelpers`.

At first glance the `ret` looks impossible because the declared signature returns `Task<int>` while the value on the IL evaluation stack is an `int`. This clearly isn’t a normal calling convention. The VM can give a `Task`-returning method two related identities, or MethodDescs, where one has the normal signature the rest of managed code sees, `Task<int> CallerAsync()`. The other is the AsyncCall variant, which effectively returns `int` and has an implicit channel for a continuation. Both refer to the same logical method and metadata token, but they have different calling conventions and different jobs. If regular managed code invokes `CallerAsync`, the VM-generated outer thunk preserves the public contract and returns a `Task<int>`. If another runtime async method directly awaits it, the JIT can instead call the AsyncCall variant and receive the result directly when the call completes synchronously, or a continuation when it suspends. In other words, it can hand back the `T` directly and avoid allocating a `Task<T>`.

That pairing works in both directions. For a method compiled with runtime async, the AsyncCall variant owns the generated (newly compact) IL while the public `Task`-returning entry point is an adapter thunk; for a traditionally compiled method, the public method owns its usual IL while the VM can create an AsyncCall adapter around it. That means runtime async code remains able to await existing libraries and code compiled by older compilers, a critical capability for our goal of 100% compat. The largest wins naturally appear as more of an async call chain is compiled with runtime async.

This is where the JIT gets an opportunity that simply didn’t exist when every boundary was already expressed as a task and a generated state machine. Suppose `A` awaits `B`, which awaits `C`:

```
static async Task<int> A(bool yield) => await B(yield);
static async Task<int> B(bool yield) => await C(yield);
static async Task<int> C(bool yield)
{
    if (yield)
        await Task.Yield();

    return 42;
}
```

Traditionally, each method has its own compiler-generated state machine and its own task-like result. `C` suspends and eventually completes its task, which wakes `B`‘s state machine; `B` then completes its task, which wakes `A`‘s state machine; and `A` completes the root task observed by the caller. There has been an enormous amount of work done over the years to reduce the costs of those objects and transitions.

With runtime async, the importer recognizes the adjacent pattern of “call a Task-returning method, then await that task.” In the simple case it can call the callee’s AsyncCall variant instead. When `yield` is false and `C` completes synchronously, the `int` flows back through `B` and `A` as a plain value, and only the outermost boundary needs to turn it into the `Task<int>` promised to the original caller. When `yield` is true and `C` suspends, the runtime links continuation state for the chain and eventually resumes it without requiring an intermediate `Task<int>` at every directly fused edge. The `Task` contract hasn’t vanished, it just moved to the place where a `Task` is actually needed.

Runtime async doesn’t make every asynchronous operation allocation-free, though. Rather, it gives the JIT enough information to avoid materializing some task objects that existed only to carry a result from one async method directly into the next. If a consumer stores the task in a collection, manually hooks up a continuation, or otherwise observes the task as an object, that object is still needed. The optimization is about not paying for boundaries that aren’t observably boundaries.

The impact is already visible with just two layers:

```
// dotnet run -c Release -f net11.0 --filter "*"
// The project also needs the `runtime-async=on` feature switch set.

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Configs;
using BenchmarkDotNet.Running;
using System.Runtime.CompilerServices;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[MemoryDiagnoser(false)]
[GroupBenchmarksBy(BenchmarkLogicalGroupRule.ByCategory)]
[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private static readonly Task<int> s_completed = Task.FromResult(42);

    [Benchmark(Baseline = true), BenchmarkCategory("Completed")]
    public Task<int> ClassicCompleted() => ClassicCompletedOuter();

    [Benchmark, BenchmarkCategory("Completed")]
    public Task<int> RuntimeCompleted() => RuntimeCompletedOuter();

    [Benchmark(Baseline = true), BenchmarkCategory("Yielding")]
    public Task<int> ClassicYielding() => ClassicYieldingOuter();

    [Benchmark, BenchmarkCategory("Yielding")]
    public Task<int> RuntimeYielding() => RuntimeYieldingOuter();

    [RuntimeAsyncMethodGeneration(false)]
    private static async Task<int> ClassicCompletedOuter() => await ClassicCompletedInner();

    [RuntimeAsyncMethodGeneration(false)]
    private static async Task<int> ClassicCompletedInner() => await s_completed;

    private static async Task<int> RuntimeCompletedOuter() => await RuntimeCompletedInner();

    private static async Task<int> RuntimeCompletedInner() => await s_completed;

    [RuntimeAsyncMethodGeneration(false)]
    private static async Task<int> ClassicYieldingOuter() => await ClassicYieldingInner();

    [RuntimeAsyncMethodGeneration(false)]
    private static async Task<int> ClassicYieldingInner()
    {
        await Task.Yield();
        return 42;
    }

    private static async Task<int> RuntimeYieldingOuter() => await RuntimeYieldingInner();

    private static async Task<int> RuntimeYieldingInner()
    {
        await Task.Yield();
        return 42;
    }
}

namespace System.Runtime.CompilerServices
{
    [AttributeUsage(AttributeTargets.Method)]
    internal sealed class RuntimeAsyncMethodGenerationAttribute(bool runtimeAsync) : Attribute
    {
        public bool RuntimeAsync => runtimeAsync;
    }
}
```

| Method           | Mean       | Ratio | Allocated | Alloc Ratio |
| ---------------- | ---------- | ----- | --------- | ----------- |
| ClassicCompleted | 21.221 ns  | 1.00  | 144 B     | 1.00        |
| RuntimeCompleted | 6.151 ns   | 0.29  | 0 B       | 0.00        |
| ClassicYielding  | 254.139 ns | 1.00  | 248 B     | 1.00        |
| RuntimeYielding  | 116.927 ns | 0.46  | 168 B     | 0.68        |

The synchronously completing chain is more than 3x faster and avoids both intermediate task allocations. Even after a real suspension, the same two-layer chain takes less than half the time and allocates 80 fewer bytes.

Exception handling amplifies the difference. Again consider an async method `A` calling an async method `B` calling an async method `C`. The transformation generated by the C# compiler of each method results in a `try`/`catch` block around the whole body of the `MoveNext` method so that any unhandled exception can be stored into the returned `Task`. Let’s say code in `C` throws an unhandled exception. That’s then caught by this manufactured `catch` block and stored into the `Task` returned to `B`. The awaiter in `B` then retrieves that exception from the `Task` object and throws it. It’s then caught by `B`‘s generated catch and stored into its `Task`. And so on. An exception crossing ten such async helpers can therefore be thrown, caught, and stored ten times even though none of the source methods has an explicit handler. That is super expensive. But runtime async doesn’t need to re-enter a pass-through frame with no handler. On the synchronous path the exception unwinds through the fused calls normally, and after a real suspension, one dispatch-loop catch walks past continuation records that have no handler and faults the observable root task once.

The following benchmark measures both a fully synchronous throw and an exception after one real `Task.Yield` suspension. It uses a compiler-recognized per-method escape hatch (`RuntimeAsyncMethodGeneration`) so that the classic and runtime async methods run in the same process on the same .NET 11 runtime and differ only in how the compiler lowers them. (Note that this attribute is experimental and isn’t a public API exposed from the core libraries; as with other attributes known to the C# compiler, it recognizes them by name and signature.)

```
// dotnet run -c Release -f net11.0 --filter "*"
// The project also needs the `runtime-async=on` feature switch set.

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;
using System.Runtime.CompilerServices;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[MemoryDiagnoser(false), HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    [Params(1, 10, 30)]
    public int Depth;

    [Params(false, true)]
    public bool Yield;

    [Benchmark(Baseline = true)]
    public int Classic() => Invoke(ClassicThrowAsync(Depth));

    [Benchmark]
    public int Runtime() => Invoke(RuntimeThrowAsync(Depth));

    private static int Invoke(Task<int> task)
    {
        try
        {
            return task.GetAwaiter().GetResult();
        }
        catch (InvalidOperationException)
        {
            return -1;
        }
    }

    [RuntimeAsyncMethodGeneration(false)]
    private async Task<int> ClassicThrowAsync(int depth)
    {
        if (depth == 0)
        {
            if (Yield) await Task.Yield();
            throw new InvalidOperationException("uh oh");
        }

        return await ClassicThrowAsync(depth - 1);
    }

    private async Task<int> RuntimeThrowAsync(int depth)
    {
        if (depth == 0)
        {
            if (Yield) await Task.Yield();
            throw new InvalidOperationException("uh oh");
        }

        return await RuntimeThrowAsync(depth - 1);
    }
}

namespace System.Runtime.CompilerServices
{
    [AttributeUsage(AttributeTargets.Method)]
    internal sealed class RuntimeAsyncMethodGenerationAttribute(bool runtimeAsync) : Attribute
    {
        public bool RuntimeAsync => runtimeAsync;
    }
}
```

| Depth | Yield | Method  | Mean      | Ratio | Allocated | Alloc Ratio |
| ----- | ----- | ------- | --------- | ----- | --------- | ----------- |
| 1     | False | Classic | 4.727 μs  | 1.00  | 1.6 KB    | 1.00        |
| 1     | False | Runtime | 3.558 μs  | 0.75  | 1.16 KB   | 0.72        |
| 1     | True  | Classic | 6.308 μs  | 1.00  | 1.68 KB   | 1.00        |
| 1     | True  | Runtime | 8.211 μs  | 1.30  | 1.42 KB   | 0.85        |
| 10    | False | Classic | 19.469 μs | 1.00  | 15.13 KB  | 1.00        |
| 10    | False | Runtime | 5.885 μs  | 0.30  | 2.13 KB   | 0.14        |
| 10    | True  | Classic | 24.923 μs | 1.00  | 15.63 KB  | 1.00        |
| 10    | True  | Runtime | 6.122 μs  | 0.25  | 2.88 KB   | 0.18        |
| 30    | False | Classic | 51.302 μs | 1.00  | 84.2 KB   | 1.00        |
| 30    | False | Runtime | 10.721 μs | 0.21  | 5.71 KB   | 0.07        |
| 30    | True  | Classic | 65.974 μs | 1.00  | 85.53 KB  | 1.00        |
| 30    | True  | Runtime | 11.254 μs | 0.17  | 7.72 KB   | 0.09        |

Runtime async supports `Task`, `Task<T>`, `ValueTask`, and `ValueTask<T>` as method return types, but as of today it doesn’t support `async void`, async iterators, or arbitrary custom task-like return types with custom builders; those continue to use the traditional compiler transformation. For `ValueTask<T>`, the existing reasons to use the type still apply. A `ValueTask<T>` can carry a result directly, wrap a `Task<T>`, or refer to an `IValueTaskSource<T>`. That’s made it useful for APIs where synchronous completion is common enough that avoiding a `Task` allocation outweighs the larger return value and the more restrictive consumption rules, or where asynchronous completion can have its costs amortized via a reusable backing object. Runtime async then addresses some of the scenarios that would have led developers to use `ValueTask<T>`. Does that mean everyone should stop using `ValueTask<T>`? No. Choosing `Task` versus `ValueTask` remains an API design decision based on completion patterns, allocation sensitivity, call frequency, and how consumers need to use the result. Write the return type that makes sense for the API, then let the compiler, VM, and JIT optimize it as best they can.

Workloads with many layers of small async methods can benefit the most from runtime async, because those layers are exactly where intermediate tasks and state machines often accumulate. Shared framework code, for example, is full of this pattern: a public method validates arguments and awaits a private helper, which awaits a transport helper, which awaits an operating-system operation. Application services similarly compose authentication, retry, logging, serialization, and I/O helpers. Runtime async can make the source-level decomposition cheaper without asking the developer to flatten the code into one giant method in order to avoid “implementation detail” costs.

The work required to reach this point has been extensive. A GitHub search of the [runtime async tracking label](https://github.com/dotnet/runtime/issues?q=label%3Aruntime-async%20is%3Apr) on September 14, 2026 returned 235 pull requests, far too many for me to enumerate one by one. So I won’t try; you can peruse that label in your spare time. The work is also not only about direct performance improvements but also about improvements to diagnostics and performance tooling that help you to make better use of async in your own code. When an async method suspends, its physical thread stack unwinds. That method’s continuation might later run on a different thread whose physical stack begins in the thread pool, with the methods that led to the original `await` nowhere to be found. A sampling CPU profiler can see where the processor is spending time, but without additional information, it can’t reliably connect those traces back through the logical async call chain, making it hard to answer questions about what async call paths were actually costing. Profiling tools like the async profiler in Visual Studio have traditionally reconstructed those chains from events emitted by `Task`‘s infrastructure, but async-heavy applications can generate enormous volumes of those very chatty events. The resulting overhead easily perturbs the workload being measured, making it all but unusable in production. [dotnet/runtime#127238](https://github.com/dotnet/runtime/pull/127238) added a new lightweight async-profiler event stream for .NET 11 and runtime async. Rather than sending every small transition through the eventing system as its own full event, the runtime writes compact records into per-thread buffers, delta-encoding timestamps and instruction pointers and flushing the data in batches. It also puts a small identifiable wrapper frame into the physical stack when invoking a continuation. A profiler can use that frame as an anchor, joining ordinary CPU samples to the logical async call stack represented by the event stream. In some measurements, this new approach added less than 1% overhead and shrank the traced data by an order of magnitude. [dotnet/runtime#129043](https://github.com/dotnet/runtime/pull/129043) and a few follow-up PRs extended the same approach to the compiler-generated state machines used by existing async code. Thus this isn’t useful only to applications that opt into runtime async; tooling gets one consistent representation across both implementations.

What should you as a developer do differently with runtime async in the picture? Mostly nothing. Keep writing asynchronous code the way you want it to read, and break a large operation into helpers when that makes the code clearer. Use `Task` by default and choose `ValueTask` where its API and usage tradeoffs genuinely fit. And don’t contort source code to remove a clean `await` just because today’s implementation might allocate an intermediate `Task`. The lowering strategy should “just work” as an implementation detail, preserve behavior, and make existing source get better as the runtime improves.

### Bounds Checks

C# is a memory-safe language. Accesses to arrays, strings, and spans are guaranteed by the runtime to be in-bounds; if you try to access `someArray[i]`, `someString[i]`, or `someSpan[i]` with an index less than 0 or greater than or equal to the length of the array/string/span, you’ll get an exception, not silently corrupted memory or a process crash. The runtime guarantees that all permitted accesses are within bounds, and that means it needs to be able to prove the access is in bounds. The main method the JIT has for achieving that is by injecting code that performs a bounds check, as if instead of:

```
int[] array = ...;
int value = array[i];
```

you’d written:

```
int[] array = ...;
if ((uint)i >= array.Length) throw new IndexOutOfRangeException();
int value = array[i];
```

At the assembly level, a bounds check looks something like:

```
; x64
cmp ecx, dword ptr [rax+8]        ; compare index with array length
jae THROW                         ; unsigned index >= length
mov edx, dword ptr [rax+rcx*4+16] ; load the element
```

The JIT could just inject such code on every access and call it a day, but such code adds overhead, so the JIT works to elide those checks and that overhead wherever it can prove the index is valid. Proving an index is valid means the JIT needs to be able to see from other evidence that it couldn’t possibly be out of bounds.

The quintessential example of that is a `for` loop over the full contents of an array or span:

```
for (int i = 0; i < array.Length; i++)
{
    Use(array[i]);
}
```

The JIT recognizes from this idiom that, within the loop body, `i` is guaranteed to be in the range `[0, array.Length)`, and avoids emitting the bounds check for the `array[i]` access. The JIT has long handled this particular case. Other cases, not so much. Bounds-check elimination has improved in virtually every .NET release; more recent releases added range propagation for derived expressions (`.NET 7` and `.NET 8` saw significant improvements here), SSA-based reasoning (`.NET 9`), and better handling of `Span<T>`, whose length sits in a field rather than an object header, complicating tracking. Each year, the developers contributing to the JIT find new patterns that were being missed, that show up in the wild, and that are fixable. .NET 11 improves several such patterns.

Range analysis in the JIT tracks intervals for each variable, an upper bound and a lower bound. For example, taking the true branch of `x < 5` gives the range for `x` in that branch an upper bound of 4 while taking the true branch of `x > 2` makes the lower bound 3. What about `x != 5`? On the true edge, we know `x` isn’t 5, and if the current range for `x` is `[5, 10]`, then we know the range must actually be `[6, 10]`… the lower bound can be tightened because the only value at the lower end is excluded. Similarly, if the range is `[0, 5]`, an `x != 5` assertion tells us the range is actually the narrower `[0, 4]`. Or, at least, that’s what you’d hope it would do. The JIT had this relevant comment:

```
// We have a != assertion, but it doesn't tell us much about the interval. So just skip it.
continue;
```

In .NET 11, [dotnet/runtime#121273](https://github.com/dotnet/runtime/pull/121273) replaces that logic with productive reasoning. It checks whether the excluded constant is at either edge of the currently tracked range, adding in the new insights if so. C# list patterns, introduced in C# 11, generate just such comparison sequences. For example, the pattern `name is [] or [':'] or [':', not ':', ..]` lowers to something like this:

```
if (name != null)
{
    int num = name.Length;

    if (num == 0) return true;

    if (num == 1)
    {
        if (name[0] == ':') return true;
    }
    else if (name[0] == ':' && name[1] != ':')
    {
        return true;
    }

    return false;
}
```

Range analysis then proceeds with something like this:

1. We know that `Array.Length` is never negative, so it has a range of `[0, Array.MaxLength]`.
2. On the false edge of `num == 0`, we know that `num != 0`, so the range is narrowed now to `[1, Array.MaxLength]`.
3. Similarly, on the false edge of `num == 1`, we know that `num != 1`, so the range is narrowed now to `[2, Array.MaxLength]`.
4. We then access `name[0]` and `name[1]`, both of which are guaranteed in bounds based on the lower bound of 2 that was established.

Without the `!= constant` tightening, that narrowing wouldn’t happen, and the bounds checks in step 4 couldn’t be elided. Thankfully, they now can be in .NET 11. Consider this example:

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;
using System.Runtime.CompilerServices;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[DisassemblyDiagnoser, HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private string[] _inputs = ["", ":", ":x", "abc", ":ab", "x", "ab:cd"];

    [Benchmark]
    public int ClassifyAll()
    {
        int total = 0;
        foreach (string s in _inputs) total += Classify(s);
        return total;
    }

    [MethodImpl(MethodImplOptions.NoInlining)]
    private static int Classify(ReadOnlySpan<char> name) =>
        name switch
        {
            [] => 0,
            [':'] => 1,
            [':', not ':', ..] => 10 + name[0] + name[1],
            _ => 3
        };
}
```

In .NET 10, we can see the call to `CORINFO_HELP_RNGCHKFAIL` at the bottom of the method. That’s the tell-tale sign there was at least one bounds check in the method. With .NET 11, that sign is removed.

```
; Arm64
--- .NET 10
+++ .NET 11
@@ -10,17 +10,15 @@
             beq     G_M000_IG08

 G_M000_IG04:
-            ldrh    w2, [x0]
-            cmp     w2, #58
+            ldrh    w1, [x0]
+            cmp     w1, #58
             bne     G_M000_IG06

 G_M000_IG05:
-            cmp     w1, #1
-            bls     G_M000_IG11
             ldrh    w0, [x0, #0x02]
             cmp     w0, #58
             beq     G_M000_IG06
-            add     w0, w2, w0
+            add     w0, w1, w0
             add     w0, w0, #10
             b       G_M000_IG07

@@ -44,8 +42,4 @@
             mov     w0, wzr
             b       G_M000_IG07

-G_M000_IG11:
-            bl      CORINFO_HELP_RNGCHKFAIL
-            brk     #0
-
-; Total bytes of code 112
+; Total bytes of code 96
```

“Assertion” machinery in the JIT propagates learned facts (like the aforementioned range information) between “basic blocks” (a sequence of instructions with one entry point, one exit point, and no branches into or out of the middle of it), so information established in block A flows to block B if A “dominates” B (meaning the only way to get to B is through A). But what about facts established earlier within the same block? That’s the gap that [dotnet/runtime#121527](https://github.com/dotnet/runtime/pull/121527) addresses. Consider this code:

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;
using System.Runtime.CompilerServices;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[DisassemblyDiagnoser, HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private int[] _arr = new int[512];

    [Benchmark]
    public int RunMany()
    {
        int touched = 0;
        for (int i = 0; i < _arr.Length - 2; i++)
        {
            Test(_arr, i);
            touched++;
        }
        return touched;
    }

    [MethodImpl(MethodImplOptions.NoInlining)]
    private static void Test(int[] arr, int i)
    {
        arr[i] = 0;  // 1: establishes 'i >= 0 && i < arr.Length'
        i++;         // 2: same block
        if (i < arr.Length) arr[i] = 0;  // 3: proven safe from 1's assertion
    }
}
```

Statements 1, 2, and 3 are all in the same basic block, up to the conditional; after statement 1 executes, if we reach statement 2, the bounds check on statement 1 passed, we know `i >= 0` and `i < arr.Length`, and after statement 2, `i` becomes `i + 1`. After the `if` guard `i < arr.Length` we know the incremented `i` is still within bounds. But when the range check pass in the .NET 10 JIT examined statement 3’s bounds check, it saw the assertions propagated from predecessor blocks. Since the assertion from statement 1 is generated within the current block, the range check couldn’t see it. The PR fixed it to walk the current block’s tree in execution order, accumulating assertions as it went. When we reach statement 3’s bounds check, we’ve already walked past statement 1 and picked up its `i >= 0 && i < arr.Length` assertion.

```
; Arm64
--- .NET 10
+++ .NET 11
@@ -13,8 +13,6 @@
             ble     G_M000_IG04

 G_M000_IG03:
-            cmp     w1, w2
-            bhs     G_M000_IG05
             str     wzr, [x0, w1, UXTW #2]

 G_M000_IG04:
@@ -25,4 +23,4 @@
             bl      CORINFO_HELP_RNGCHKFAIL
             brk     #0

-; Total bytes of code 68
+; Total bytes of code 60
```

There are almost an infinite number of things the JIT could look for and special-case. But every special case requires code, maintenance, and, most importantly, compilation time. A “just-in-time” compiler typically runs while the application is running, so the JIT itself must be optimized and spend its limited budget only where there’s a likely payoff. That pushes the developers building it toward patterns that occur in real workloads. One such pattern, often seen in libraries like format decoders, builds a table index with bitwise operations on a byte, for example `((b & 0x03) << 4) | ((b & 0xf0) >> 4)`. Each masked piece has a tiny upper bound, so the OR of those pieces is always in `[0..63]`, safely in range for e.g. a Base64 alphabet table. Until [dotnet/runtime#122263](https://github.com/dotnet/runtime/pull/122263), the JIT often failed to prove that combined bound and left a bounds check on the index. Existing range-check code understood the upper bounds produced by bitwise AND and shifts, but not OR; the change lets the JIT combine the known bounds of both OR operands and remove the remaining array check.

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;
using System.Runtime.CompilerServices;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[DisassemblyDiagnoser, HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private readonly byte[] _input = new byte[4096];

    [GlobalSetup]
    public void Setup() => new Random(42).NextBytes(_input);

    [Benchmark]
    public int Base64LikeIndex() => Sum(_input);

    [MethodImpl(MethodImplOptions.NoInlining)]
    private static int Sum(ReadOnlySpan<byte> input)
    {
        int sum = 0;
        foreach (byte b in input)
        {
            int index = ((b & 0x03) << 4) | ((b & 0xF0) >> 4);
            sum += "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/="u8[index];
        }

        return sum;
    }
}
```

The .NET 10 assembly checks the computed index against the 65-byte lookup table on every iteration. In .NET 11, range analysis proves the index is at most 63, so both the comparison and the branch to the range-check failure helper disappear:

```
; x64
 M01_L00:
        movzx    r9d, byte ptr [rdx+r8]
        mov      r11d, r9d
        and      r11d, 3
        shl      r11d, 4
        and      r9d, 0F0
        sar      r9d, 4
        or       r9d, r11d
-       cmp      r9d, 41
-       jae      short M01_L02
        movzx    r9d, byte ptr [r10+r9]
        add      eax, r9d
        inc      r8d
        cmp      r8d, ecx
        jl       short M01_L00

-M01_L02:
-       call     CORINFO_HELP_RNGCHKFAIL
-       int      3
-
-; Total bytes of code 95
+; Total bytes of code 79
```

As another example, [dotnet/runtime#125056](https://github.com/dotnet/runtime/pull/125056) improves the handling of guards like `(uint)i < span.Length` that are pervasive in performance-sensitive code. Consider this benchmark:

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;
using System.Runtime.CompilerServices;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[DisassemblyDiagnoser, HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private int[] _data = Enumerable.Range(0, 512).ToArray();

    [Benchmark]
    public int RunMany()
    {
        int sum = 0;
        for (int i = 0; i < _data.Length; i++)
            sum += Test(_data, i);
        return sum;
    }

    [MethodImpl(MethodImplOptions.NoInlining)]
    private static int Test(Span<int> span, int i)
    {
        if ((uint)i < (uint)span.Length)
        {
            if (i != 0)
                return span[i - 1] + span[i];

            return span[i];
        }

        return 0;
    }
}
```

Because the comparison is unsigned, `(uint)i` would be a large positive number if `i` were negative, making it impossible for `(uint)i < (uint)span.Length` to be true (since a span’s length is never negative, `(uint)span.Length` is at most `int.MaxValue`). Inside the true branch, `i` is therefore in `[0, span.Length - 1]`. Previously, the JIT wasn’t always recording the lower bound `i >= 0` when it processed the `(uint)i < span.Length` assertion, and that could leave bounds checks on expressions like `i - 1` in place. The fix adds the `[0, int.MaxValue - 1]` lower bound deduction for the index variable upon entering the true arm of a `(uint)i < span.Length` check. Combined with the existing range tracking for the upper bound, this gives the JIT a complete picture of `i`‘s range inside the guarded block.

```
; Arm64
--- .NET 10
+++ .NET 11
@@ -8,10 +8,8 @@
             cbz     w2, G_M000_IG05

 G_M000_IG03:
-            sub     w3, w2, #1
-            cmp     w3, w1
-            bhs     G_M000_IG09
-            ldr     w1, [x0, w3, UXTW #2]
+            sub     w1, w2, #1
+            ldr     w1, [x0, w1, UXTW #2]
             ldr     w0, [x0, w2, UXTW #2]
             add     w0, w1, w0

@@ -33,8 +31,4 @@
             ldp     fp, lr, [sp], #0x10
             ret     lr

-G_M000_IG09:
-            bl      CORINFO_HELP_RNGCHKFAIL
-            brk     #0
-
-; Total bytes of code 84
+; Total bytes of code 68
```

Bounds check elision is generally based on forms of range analysis, where the JIT needs to prove that a given index is guaranteed to be within the range of the data structure. But the same range analysis-based facts can prove that other checks are unnecessary. For example, once the JIT knows that an integer is in `[0..100]`, it can prove both that converting it to `byte` can’t lose data and that multiplying it by 10 can’t overflow. [dotnet/runtime#124147](https://github.com/dotnet/runtime/pull/124147) enables the JIT to use such facts to avoid unnecessary branches as part of `checked` operations. When range analysis proves that the operands are in ranges whose result can’t overflow, making `checked` a nop, the backend can now emit plain add/multiply/subtract instructions, without the jump to failure, as in the following example:

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;
using System.Runtime.CompilerServices;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[DisassemblyDiagnoser, HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private readonly int[] _array = new int[99];

    [Benchmark]
    public int ArrayLengthPlusConstant() => AddToLength(_array);

    [MethodImpl(MethodImplOptions.NoInlining)]
    private static int AddToLength(int[] array) => checked(array.Length + 10);

    [Benchmark]
    public int GuardedLengthTimesConstant() => Multiply(_array);

    [MethodImpl(MethodImplOptions.NoInlining)]
    private static int Multiply(Span<int> span)
    {
        if (span.Length >= 100) return 0;
        return checked(span.Length * 10);
    }
}
```

```
; Arm64
--- .NET 10
+++ .NET 11
 G_M000_IG02:
             cmp     w1, #100
             bge     G_M000_IG05

 G_M000_IG03:
             mov     w0, #10
-            smull   x0, w1, w0
-            lsr     x2, x0, #32
-            cmp     w2, w0, ASR #31
+            mul     w0, w1, w0
-            bne     G_M000_IG07

 G_M000_IG04:
             ldp     fp, lr, [sp], #0x10
             ret     lr

-G_M000_IG07:
-            bl      CORINFO_HELP_OVERFLOW
-            brk     #0
-
-; Total bytes of code 64
+; Total bytes of code 44
```

That makes the change broadly applicable: any time you write `checked` arithmetic on quantities that are inherently bounded, such as collection counts, lengths, or indices constrained by prior comparisons, the JIT now has a chance to prove at compile time that the overflow can’t happen and thus eliminate the run-time check entirely. Building on that range-check work, [dotnet/runtime#124184](https://github.com/dotnet/runtime/pull/124184) teaches the JIT to eliminate “narrowing casts” under the same kinds of guards:

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;
using System.Runtime.CompilerServices;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[DisassemblyDiagnoser, HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private uint _value = 100;

    [Benchmark]
    public byte GuardedNarrowingCast() => Narrow(_value);

    [MethodImpl(MethodImplOptions.NoInlining)]
    private static byte Narrow(uint value)
    {
        if (value > 100) return 0;
        return checked((byte)value);
    }
}
```

```
; Arm64
--- .NET 10
+++ .NET 11
@@ -4,23 +4,10 @@

 G_M000_IG02:
             cmp     w0, #100
-            bhi     G_M000_IG04
-            cmp     w0, #255
-            bhi     G_M000_IG06
+            csel    w0, w0, wzr, ls

 G_M000_IG03:
             ldp     fp, lr, [sp], #0x10
             ret     lr

-G_M000_IG04:
-            mov     w0, wzr
-
-G_M000_IG05:
-            ldp     fp, lr, [sp], #0x10
-            ret     lr
-
-G_M000_IG06:
-            bl      CORINFO_HELP_OVERFLOW
-            brk     #0
-
-; Total bytes of code 52
+; Total bytes of code 24
```

Such use of `checked` is common in serialization and protocol code where you validate a value’s range prior to truncating it. In this benchmark I’ve used `checked` explicitly, but the more common form is with the whole project compiled with `<CheckForOverflowUnderflow>true</CheckForOverflowUnderflow>` in the .csproj, such that this `checked` becomes implicit. After the change, the range analysis sees that `value` is in the range `[0, 100]`, knows `byte` fits values up to 255, and elides the check.

[dotnet/runtime#128620](https://github.com/dotnet/runtime/pull/128620) further teaches range analysis the possible results of leading-zero count, trailing-zero count, and population count instructions. Those results are often used to index small lookup tables… knowing their bounds lets the JIT remove the bounds check.

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;
using System.Numerics;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private static readonly int[] s_lookup =
        Enumerable.Range(0, 33).Select(i => i * i).ToArray();
    private uint[] _values;

    [GlobalSetup]
    public void Setup()
    {
        Random rng = new(42);
        _values = Enumerable.Range(0, 1024).Select(i => (uint)rng.Next(1, int.MaxValue)).ToArray();
    }

    [Benchmark]
    public int SumLookupByLeadingZeroCount()
    {
        int sum = 0;
        foreach (var v in _values)
            sum += s_lookup[BitOperations.LeadingZeroCount(v)];

        return sum;
    }
}
```

The lookup improves because the JIT now knows `LeadingZeroCount(uint)` is between 0 and 32 and can remove the bounds check.

| Method                      | Runtime   | Mean     | Ratio |
| --------------------------- | --------- | -------- | ----- |
| SumLookupByLeadingZeroCount | .NET 10.0 | 516.1 ns | 1.00  |
| SumLookupByLeadingZeroCount | .NET 11.0 | 438.5 ns | 0.85  |

The JIT is also able to conditionally apply range check-based elision via “cloning”. Cloning is a mechanism where the JIT takes one piece of code and duplicates it. One of the copies it leaves as it was originally, and the other copy it special cases. So, for example, if you had code like:

```
int value = array[i];
```

the JIT could theoretically clone that in order to avoid the implicit bounds check, e.g.

```
int value;
if ((uint)i < array.Length)
{
    // no bounds check emitted by JIT, e.g.
    value = Unsafe.Add(ref MemoryMarshal.GetArrayDataReference(array), i);
}
else
{
    // bounds check emitted
    value = array[i];
}
```

That particular code looks silly, as we’re just trading an implicit bounds check for an explicit one. It becomes less silly when the JIT is able to elide multiple bounds checks with a single branch, e.g.

```
int sum;
if (4 < array.Length)
{
    // zero bounds checks
    ref int startRef = ref MemoryMarshal.GetArrayDataReference(array);
    sum =
        startRef +
        Unsafe.Add(ref startRef, 1) +
        Unsafe.Add(ref startRef, 2) +
        Unsafe.Add(ref startRef, 3);
}
else
{
    // potentially four bounds checks
    sum =
        array[0] +
        array[1] +
        array[2] +
        array[3];
}
```

Such optimizations are already handled in the JIT, via its `optRangeCheckCloning` phase. It groups bounds checks from a basic block, emits one guard for the largest required range, and duplicates the affected code into a fast path where the individual checks can be removed and a fallback path where they remain. However, one long-standing limitation of range-check cloning is that it refused to process the last statement of any “terminator” block, a block that ends with a jump or return instruction. For a method like:

```
static int ArrayAccess(int[] abcd) => abcd[0] + abcd[1] + abcd[2] + abcd[3];
```

all four array accesses live in the return statement, the last statement of a return block, so nothing got cloned and the hot path retained four separate bounds checks. In .NET 11, [dotnet/runtime#124705](https://github.com/dotnet/runtime/pull/124705) removes that restriction, making the return statement eligible for range-check cloning and allowing a single fast-path guard to cover all four accesses.

But even without range-check cloning, there’s really no reason such accesses should require four bounds checks: the JIT should be able to see that the array or span needs to have a length of at least 4 and guard all accesses by that single check. If there were intervening operations that had side effects, the JIT would need to maintain order of operations, at least enough to maintain the observable behavior of those effects, but that’s not the case here. With [dotnet/runtime#127439](https://github.com/dotnet/runtime/pull/127439) in .NET 11, the JIT will now coalesce those checks within a basic block, strengthening the first check to the largest constant index and removing the rest.

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private readonly int[] _values = Enumerable.Range(0, 16).ToArray();

    [Benchmark]
    public int Sum16()
    {
        int[] values = _values;
        return
            values[0] + values[1] + values[2] + values[3] +
            values[4] + values[5] + values[6] + values[7] +
            values[8] + values[9] + values[10] + values[11] +
            values[12] + values[13] + values[14] + values[15];
    }
}
```

In previous releases, you’d sometimes see a proactive developer doing a similar optimization manually, e.g. reordering the accesses in an example like that to put the largest read first. That’s no longer necessary.

| Method | Runtime   | Mean     | Ratio |
| ------ | --------- | -------- | ----- |
| Sum16  | .NET 10.0 | 2.958 ns | 1.00  |
| Sum16  | .NET 11.0 | 1.828 ns | 0.62  |

Another bounds checking improvement comes in [dotnet/runtime#127488](https://github.com/dotnet/runtime/pull/127488), which actually targets explicitly-implemented bounds checks (rather than the implicit ones we’ve been discussing) and targets code that reads a fixed-size value from the end of a span, such as `BinaryPrimitives.ReadInt32BigEndian(span.Slice(span.Length - 4))` behind a `span.Length >= 4` guard, e.g.

```
if (span.Length >= 4)
{
    // Parse an int from the end of the span
    ... = ReadInt32BigEndian(span.Slice(span.Length - 4));
    ...
}
```

There shouldn’t be any additional bounds checking required here. However, `Span.Slice` begins with:

```
if ((uint)start > (uint)_length)
    ThrowHelper.ThrowArgumentOutOfRangeException();
```

and `ReadInt32BigEndian` begins with:

```
if (sizeof(T) > source.Length)
    ThrowHelper.ThrowArgumentOutOfRangeException();
```

so even though our `span.Length >= 4` check should have been sufficient, we’re still ending up with two additional checks. To address that, the JIT needed two things.

First, it needed to be able to identify that `x - (x + a)` is the same as `-a`. Without this identity, `length - (length - 4)` is just an opaque subtraction of two expressions with no obvious constant result. With the identity, the JIT can recognize the inner expression `(length - 4)` as `length + (-4)`, apply `x - (x + a) == -a` with `x == length` and `a == -4`, and end up with `-(-4) == 4`. Now `ReadInt32BigEndian`‘s check against 4 becomes `4 >= 4`, which the JIT can trivially see is true.

Second, `Slice(start)` must establish that `start` is between zero and the span’s length. When `start` is `length - 4`, the existing `length >= 4` guard proves the result is non-negative, while subtracting a positive constant means the result can’t exceed `length`. The improved range analysis connects that guard to the subtraction and removes `Slice`‘s check.

Both fixes together mean the above example now elides both extra bounds checks. That’s useful in particular for libraries like parsers, network protocol implementations, and cryptographic code, all of which frequently on hot paths do things like “read the last N bytes of a buffer.”

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;
using System.Buffers.Binary;
using System.Runtime.CompilerServices;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[DisassemblyDiagnoser]
[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private readonly byte[] _buffer = new byte[64];

    [Benchmark]
    public int ReadLastInt32() => ReadLastInt32(_buffer);

    [MethodImpl(MethodImplOptions.NoInlining)]
    private static int ReadLastInt32(ReadOnlySpan<byte> span)
    {
        if (span.Length >= sizeof(int))
        {
            return BinaryPrimitives.ReadInt32BigEndian(span.Slice(span.Length - sizeof(int)));
        }

        return -1;
    }
}
```

In .NET 10, the helper is 73 bytes and includes both additional checks and their throw paths:

```
; x64
cmp       ecx,4
jl        RETURN_MINUS_ONE
lea       edx,[rcx-4]
cmp       edx,ecx
ja        THROW_SLICE
mov       r8d,edx
add       rax,r8
sub       ecx,edx
cmp       ecx,4
jl        THROW_READ
movbe     eax,[rax]
```

In .NET 11, the helper is 28 bytes, and only the original length guard remains:

```
; x64
cmp       ecx,4
jl        RETURN_MINUS_ONE
add       ecx,-4
add       rax,rcx
movbe     eax,[rax]
```

[dotnet/runtime#122040](https://github.com/dotnet/runtime/pull/122040) and [dotnet/runtime#127117](https://github.com/dotnet/runtime/pull/127117) similarly help to remove bounds checks involving `span.Slice`. Vectorized loops often work through a span a chunk at a time, slicing off the elements they’ve already processed. The JIT hasn’t always been able to keep track of how those progressively smaller slices relate to the original span, so it could end up checking the same limits again on each iteration. These changes improve that tracking, enabling more of those repeated checks to be removed.

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using System.Runtime.CompilerServices;
using System.Runtime.Intrinsics;
using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[DisassemblyDiagnoser, HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private readonly int[] _data = Enumerable.Repeat(1, 1_024).ToArray();

    [Benchmark]
    public Vector256<int> CreateFromSlice() => CreateFromSlice(_data);

    [Benchmark]
    public int SumSliced() => SumSliced(_data);

    [MethodImpl(MethodImplOptions.NoInlining)]
    private static Vector256<int> CreateFromSlice(Span<int> values)
    {
        if (values.Length < 16)
            return default;

        return Vector256.Create(values.Slice(8));
    }

    [MethodImpl(MethodImplOptions.NoInlining)]
    private static int SumSliced(ReadOnlySpan<int> data)
    {
        Vector128<int> sum = default;
        while (data.Length >= Vector128<int>.Count)
        {
            sum += Vector128.Create(data);
            data = data.Slice(Vector128<int>.Count);
        }

        int result = Vector128.Sum(sum);
        foreach (int value in data)
            result += value;

        return result;
    }
}
```

In .NET 10, the loop condition proves that at least one vector remains, but the construction of the vector from the current span performs the same check again. .NET 11 retains the length relationship, so the loop body begins directly with the vector addition:

```
; x64, vector loop
-cmp       esi, 4
-jl        THROW_ARGUMENT_OUT_OF_RANGE
-vpaddd    xmm6, xmm6, [rbx]
-add       rbx, 10
-add       esi, 0FFFFFFFC
-cmp       esi, 4
+vpaddd    xmm0, xmm0, [rax]
+add       rax, 10
+add       ecx, 0FFFFFFFC
+cmp       ecx, 4
 jge       LOOP
```

We saw earlier how range-check cloning enables duplicating a sequence of instructions in order to eliminate bounds checks. “Loop cloning” extends that to a whole loop. Consider a loop that processes the first `count` elements of an array:

```
for (int i = 0; i < count; i++)
    sum += values[i];
```

The test `i < count` doesn’t by itself prove that `i < values.Length`, so by default the compilation would need a bounds check in the body, which would mean a bounds check for every `values[i]` access. Loop cloning gives the JIT another option. Instead of generating the equivalent of:

```
for (int i = 0; i < count; i++)
    sum += values[i]; // bounds check!
```

it can generate the equivalent of:

```
if ((uint)count <= (uint)values.Length)
{
    // no bounds checks
    ref int startRef = ref MemoryMarshal.GetArrayDataReference(values);
    for (int i = 0; i < count; i++)
    {
        sum += Unsafe.Add(ref startRef, i);
    }
}
else
{
    // bounds check per iteration
    for (int i = 0; i < count; i++)
    {
        sum += values[i];
    }
}
```

For the common case where the iteration is in bounds, execution proceeds through a cloned loop with no per-iteration bounds checks, whereas the original checked loop remains as the fallback that preserves exceptional behavior for invalid inputs. The normal path pays for one guard and avoids a check on every iteration, but that comes at the expense of duplicating code. The JIT therefore needs to apply the optimization selectively.

The JIT has long employed loop cloning, but it didn’t always kick in even in cases it seemed applicable. The previous example showed loop cloning with `<` in the iteration condition. For whatever reason, however, some developers used `!=`, and loop cloning didn’t apply (I’m guessing they used `!=` because they thought it was more efficient, and they actually end up deoptimizing). Thanks to [dotnet/runtime#129268](https://github.com/dotnet/runtime/pull/129268), in .NET 11 `!=` is now also handled, as long as specific conditions are met, such as the stride being exactly 1 or -1, e.g. `i++` qualifies, while `i += 2` doesn’t. [dotnet/runtime#129303](https://github.com/dotnet/runtime/pull/129303) also improves loops that terminate with `i != bound`, giving the JIT a tighter understanding of the values `i` can take and allowing it to remove some bounds checks even when it can’t clone the whole loop.

Lookahead in arrays and spans is another recurring pattern, especially in parsers. [dotnet/runtime#124242](https://github.com/dotnet/runtime/pull/124242) and [dotnet/runtime#125235](https://github.com/dotnet/runtime/pull/125235) recognize conditions such as `(uint)(i + 2) < (uint)span.Length` and use that relation to remove the follow-on checks for `span[i + 1]` and `span[i + 2]`. Consider this benchmark:

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;
using System;
using System.Linq;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private readonly string _text = string.Concat(Enumerable.Repeat("%FE", 128));

    [Benchmark]
    public bool ContainsPercentFF()
    {
        ReadOnlySpan<char> span = _text;
        for (int i = 0; i < span.Length; i++)
        {
            if (span[i] == '%' &&
                (uint)(i + 2) < (uint)span.Length &&
                span[i + 1] == 'F' &&
                span[i + 2] == 'F')
            {
                return true;
            }
        }

        return false;
    }
}
```

| Method            | Runtime   | Mean     | Ratio |
| ----------------- | --------- | -------- | ----- |
| ContainsPercentFF | .NET 10.0 | 232.1 ns | 1.00  |
| ContainsPercentFF | .NET 11.0 | 194.9 ns | 0.84  |

Several smaller changes broaden the range of code from which the JIT can remove bounds checks:

- [dotnet/runtime#121640](https://github.com/dotnet/runtime/pull/121640) helps in a situation where once an access using a chosen index has been checked, a later access to the same array at that index need not be checked again.
- [dotnet/runtime#121683](https://github.com/dotnet/runtime/pull/121683) enables the JIT to trace an array’s length through calculations performed earlier in the method, exposing more redundant checks, including some involving index-from-end expressions.
- [dotnet/runtime#124387](https://github.com/dotnet/runtime/pull/124387) and [dotnet/runtime#130326](https://github.com/dotnet/runtime/pull/130326) teach the optimizer to rely on a span’s length always being non-negative.
- [dotnet/runtime#124571](https://github.com/dotnet/runtime/pull/124571) improves sequences of index-from-end accesses: once an access like `arr[^4]` establishes that the array has at least four elements, the JIT reuses that information for nearby accesses such as `arr[^3]`.
- [dotnet/runtime#129101](https://github.com/dotnet/runtime/pull/129101) improves how the JIT combines and carries forward the possible ranges of arithmetic expressions, including expressions involving bitwise OR and unsigned division. Those tighter ranges can show that more values are non-negative or within bounds.

Bounds-check elimination is only one payoff from understanding a loop’s structure. The JIT analyzes induction variables (values like loop counters that change predictably each iteration) and puts loops into standard forms so that later optimizations can reason about them. .NET 11 broadens the range of loops for which that works:

- [dotnet/runtime#122184](https://github.com/dotnet/runtime/pull/122184) recognizes another representation of a 32-to-64-bit zero extension. That lets pointer loops using expressions such as `data[(uint)i]` replace the repeated index extension and address calculation with a pointer increment.
- [dotnet/runtime#119537](https://github.com/dotnet/runtime/pull/119537) follows simple control-flow predecessors when finding an induction variable’s initialization and zero-trip test, while [dotnet/runtime#128303](https://github.com/dotnet/runtime/pull/128303) gives loops with multiple backedges a single canonical latch block.
- [dotnet/runtime#128532](https://github.com/dotnet/runtime/pull/128532) makes loop cloning tolerate more statements around the update and test.
- [dotnet/runtime#129309](https://github.com/dotnet/runtime/pull/129309) extends cloning to more span loops with non-unit strides and offset limits.
- [dotnet/runtime#129349](https://github.com/dotnet/runtime/pull/129349) handles large strides in array loops with an explicit safety guard rather than rejecting them outright.
- [dotnet/runtime#129472](https://github.com/dotnet/runtime/pull/129472) allows loop inversion to spend more of its budget on likely cloning candidates.
- [dotnet/runtime#130205](https://github.com/dotnet/runtime/pull/130205) removes comparisons that are redundant given the induction variable’s known range.
- [dotnet/runtime#131362](https://github.com/dotnet/runtime/pull/131362) corrects profile weights after inversion changes a loop’s exit.

Much of this work wasn’t motivated by contrived benchmarks containing nothing but array indexing as I’m prone to use in these posts. Rather, many of the improvements stemmed from an ongoing audit of unsafe code throughout the .NET libraries, part of a broader [effort to improve memory safety in .NET](https://github.com/dotnet/designs/blob/main/accepted/2025/memory-safety/memory-safety.md). .NET and C# are memory safe, but as with other memory safe languages like Rust, it provides escape hatches that enable turning off the guardrails provided by the compiler and runtime. This effort is about reducing where and when developers feel compelled to use those escape hatches, since every occurrence is an opportunity for increased risk. Unsafe code was often introduced years earlier to manually avoid bounds checks, typically by walking a buffer with pointers, byrefs, or `Unsafe.Add`. Sometimes the audit found that the unsafe code was no longer needed and could simply be removed. Sometimes a “safe” rewrite (meaning not using `unsafe` and friends) was already just as fast or even faster. And sometimes the rewrite exposed an optimization the JIT was missing, in which case the answer was to improve the JIT and then rewrite the library code to use normal, bounds-checked C#. Several of the optimizations discussed in this section are the result of exactly that feedback loop. [dotnet/runtime#127429](https://github.com/dotnet/runtime/pull/127429) is a particularly nice example. The vectorized implementation of `Enumerable.Sum` used `MemoryMarshal.GetReference`, `Vector.LoadUnsafe`, and `Unsafe.Add` to walk its input without bounds checks. With the span-slicing improvements described earlier, it could instead use `Vector.Create(span)`, `span.Slice(...)`, and a `foreach` for the tail. That’s easier to reason about, removes the unchecked indexing, and ended up being faster. [dotnet/runtime#114757](https://github.com/dotnet/runtime/pull/114757) similarly replaced an unsafe pointer-based header-name accessor with a generic `ReadOnlySpan<T>` implementation without loss of performance. Similarly, [dotnet/runtime#121270](https://github.com/dotnet/runtime/pull/121270) removed more unsafe code from `Uri` parsing and actually improved performance of the cited code measurably.

There’s a useful “go do” here for libraries outside of dotnet/runtime, as well. Unsafe code written to work around the JIT is a snapshot of what the JIT could do at the time that code was written. If you own code that has hand-written pointer or `Unsafe`-based loops whose purpose is to avoid bounds checks, it’s worth rewriting them with safe, bounds-checked C# and measuring again on .NET 11. Chances are, you’ll find the gap at this point is either non-existent or small enough that it’s not worth the increased maintenance and risk for managing the safety yourself. And if the revised version is still slower, that’s a great opportunity for you to share a repro in the dotnet/runtime repo, hopefully serving as inspiration for one of the first performance improvements to go into the JIT for .NET 12. `unsafe` code is still necessary for scenarios like interop, but performance alone shouldn’t be a permanent reason to eschew all the valuable guardrails .NET provides.

The [C# 15 memory-safety preview](https://learn.microsoft.com/dotnet/csharp/whats-new/csharp-15#memory-safety) pushes in the same direction and is part and parcel of this effort. Historically, C# has largely equated pointers with unsafe code: simply declaring or manipulating a pointer generally required an `unsafe` context, even if the code never accessed the memory to which it points. In the preview, pointer plumbing such as declaring a pointer, taking an address with `&`, using `fixed`, converting `stackalloc` to a pointer, and applying `sizeof` to an unmanaged type no longer requires an `unsafe` context. Operations that actually access the pointed-to memory, including `*p`, `p->member`, and `p[i]`, still do. C# 15 also adds an `unsafe(expression)` form, analogous to `checked(expression)`, so an unsafe context can cover one precise expression rather than a larger statement block. Those changes are the first preview slice of a larger, multi-release [unsafe evolution](https://github.com/dotnet/csharplang/blob/main/proposals/unsafe-evolution.md). The end goal is to make unsafe regions smaller, make their assumptions visible through the call graph, and make them easier for reviewers and tools to find. Pairing that with a JIT that makes idiomatic safe code fast removes a lot of the historical pressure to use unsafe code in the first place.

### Assertion Propagation

As discussed earlier, the JIT continually learns facts while compiling a method: a value equals a constant, a reference isn’t null, an integer falls within a particular range, and so on. “Assertion propagation” carries those facts forward so they can simplify later code. “Value numbering” complements it by letting the JIT recognize when two expressions compute the same value, even if they appear in different places or use different variables. Together, these mechanisms enable optimizations such as removing redundant null and bounds checks, folding conditions to constants, and reusing repeated computations. .NET 11 improves assertion propagation primarily by fixing places where useful facts were either never recorded or weren’t recognized later.

For example, reading an array’s length normally carries an implicit null-check: if the array reference is `null`, the read must throw. Once global assertion propagation already knows the reference is non-null, however, we should be able to avoid the implicit null check. In .NET 11, [dotnet/runtime#124291](https://github.com/dotnet/runtime/pull/124291) takes care of that for `Array.Length`:

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;
using System.Runtime.CompilerServices;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[DisassemblyDiagnoser, HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private readonly int[] _values = new int[1024];

    [Benchmark]
    public void DeadLength() => Test(_values);

    [MethodImpl(MethodImplOptions.NoInlining)]
    private static void Test(int[]? values)
    {
        if (values is not null)
            _ = values.Length;
    }
}
```

The .NET 10 code still tests the reference and reads the length. In .NET 11, the guard proves the read can’t throw, and since its result isn’t used, the access disappears:

```
; Arm64
--- .NET 10
+++ .NET 11
 G_M000_IG01:
             stp     fp, lr, [sp, #-0x10]!
             mov     fp, sp

 G_M000_IG02:
-            cbz     x0, G_M000_IG04
-
-G_M000_IG03:
-            ldr     wzr, [x0, #0x08]
-
-G_M000_IG04:
             ldp     fp, lr, [sp], #0x10
             ret     lr

-; Total bytes of code 24
+; Total bytes of code 16
```

[dotnet/runtime#119474](https://github.com/dotnet/runtime/pull/119474) improves the starting point for integer range analysis. The JIT now uses facts inherent in a value itself, e.g. a constant has one exact value, while a value converted to `byte`, for example, must be between 0 and 255. That can eliminate bounds checks and conditions even when no preceding `if` explicitly established the range. [dotnet/runtime#124415](https://github.com/dotnet/runtime/pull/124415) further refines this handling of casts, combining what is known about both the source value and the destination type to derive the tightest useful range.

Those improvements derive ranges from facts inherent in a value, but ranges can also come from control flow. After `if ((uint)x < 10)`, for example, the JIT knows that `x` is between 0 and 9 on the true path, which may be enough to remove a later comparison or array bounds check. [dotnet/runtime#123624](https://github.com/dotnet/runtime/pull/123624) derives tighter ranges from assertions and casts, including proving that some comparisons are always true or false. [dotnet/runtime#129390](https://github.com/dotnet/runtime/pull/129390) preserves range information more accurately when control-flow paths merge.

Other changes make better use of the ranges once known. [dotnet/runtime#129354](https://github.com/dotnet/runtime/pull/129354) traces values back through their definitions to fold more span- and slice-related comparisons, and [dotnet/runtime#126917](https://github.com/dotnet/runtime/pull/126917) uses narrowed ranges to remove more relational branches.

[dotnet/runtime#124711](https://github.com/dotnet/runtime/pull/124711) teaches the JIT to learn implicit facts from operations that have already completed successfully. For example:

- Creating an array proves its requested length wasn’t negative.
- A reference-array store may need a runtime covariance check, because a value typed as `object[]` can actually refer to a `string[]`; the helper that performs that type check also validates the index, so if it returns successfully, the index was in range.
- Integer division or modulo proves the divisor wasn’t zero.

And so on. Those facts can then remove redundant checks and conditions later in the method.

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private readonly object?[] _objArr = new object?[8];
    private readonly object _value = new();

    [Benchmark]
    public object? CovariantArrayStore()
    {
        object?[] objArr = _objArr;
        objArr[3] = _value;
        return objArr[2];
    }
}
```

A successful store to element 3 proves that particular array has at least four elements; since an array’s length can’t change, the subsequent read of element 2 doesn’t need another bounds check.

| Method              | Runtime   | Mean     | Ratio |
| ------------------- | --------- | -------- | ----- |
| CovariantArrayStore | .NET 10.0 | 3.565 ns | 1.00  |
| CovariantArrayStore | .NET 11.0 | 2.985 ns | 0.84  |

[dotnet/runtime#128522](https://github.com/dotnet/runtime/pull/128522) simplifies how the global assertion pass identifies values, making it less likely to miss a fact learned earlier. One practical impact of this is better propagation of a static `string`‘s known length, which can turn a general string comparison into a fixed-size vectorized comparison.

[dotnet/runtime#127810](https://github.com/dotnet/runtime/pull/127810) improves null-check elimination where control flow merges. With `??=`, which is a very common operator used for lazy initialization, the resulting value is non-null whether it came from the existing field or from the newly allocated object. The JIT now combines the facts from both paths and recognizes that the subsequent call doesn’t need another null check.

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;
using System.Runtime.CompilerServices;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[DisassemblyDiagnoser, HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    Inner? _inner;

    [Benchmark]
    [Arguments(42)]
    public int Invoke(int n) => (_inner ??= new()).Increment(n);

    private sealed class Inner
    {
        [MethodImpl(MethodImplOptions.NoInlining)]
        public int Increment(int n) => n + 1;
    }
}
```

The generated code consequently loses the null check on the merged value:

```
; x64
 M00_L00:
        mov      edx, esi
-       cmp      [rcx], ecx
        call     qword ptr [...] ; Inner.Increment(Int32)

-; Total bytes of code 75
+; Total bytes of code 73
```

[dotnet/runtime#128701](https://github.com/dotnet/runtime/pull/128701) removes similarly redundant null checks from copies of structs that contain object references. Such copies use a runtime helper so the garbage collector is correctly notified about the reference writes, but lowering had been adding probes for both source and destination without preserving whether either address could actually fault. It now emits only the probes that are needed.

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[DisassemblyDiagnoser, HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private FourRefs _src = new()
    {
        A = new(),
        B = new(),
        C = new(),
        D = new()
    };
    private FourRefs _dst;

    [Benchmark]
    public void BulkStructCopy() => _dst = _src;

    private struct FourRefs
    {
        public object? A;
        public object? B;
        public object? C;
        public object? D;
    }
}
```

Both `_src` and `_dst` are fields of the same object, so after probing the source address has established that the object isn’t null, probing the destination address can’t provide any additional information. .NET 11 removes that second probe:

```
; Arm64
 G_M000_IG02:
             add     x1, x0, #8
             ldrsb   wzr, [x1]
             add     x0, x0, #40
-            ldrsb   wzr, [x0]
             movz    x2, ...
             ldr     x3, [x2]
             mov     x2, #32
             blr     x3      // CORINFO_HELP_BULK_WRITEBARRIER

-; Total bytes of code 56
+; Total bytes of code 52
```

Additionally, [dotnet/runtime#125215](https://github.com/dotnet/runtime/pull/125215) lets the JIT retain and efficiently find more assertions in larger methods, increasing the opportunities for the same kinds of simplification. And [dotnet/runtime#129312](https://github.com/dotnet/runtime/pull/129312) removes unnecessary temporary variables when the same simple field address is used multiple times, enabling more efficient loads and stores.

### Simplification

Assertion propagation is largely about proving things to help the generated code. Once the JIT knows enough about an operation’s inputs, it can often replace the operation with something simpler and cheaper.

“Constant folding” is a fancy way of saying the compiler does work once so it doesn’t need to be repeated at run time. If the compiler has everything it needs to compute an answer when building, it can bake that answer in to the generated code and avoid needing the code to re-compute it. That answer can then be further used by other computations at build time, potentially folding further. The C# compiler handles constant folding expressions composed entirely of language constants, while the JIT compiler can go further after inlining and after learning things about values and control flow. The JIT already does a ton of folding, and as with every release, it goes further in .NET 11.

One straightforward example is the offset of a field within a struct. [dotnet/runtime#122297](https://github.com/dotnet/runtime/pull/122297) recognizes more cases where two addresses refer to the same struct and replaces their difference with the known field offset. Here, the second `int` field begins four bytes into the struct:

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;
using System.Runtime.CompilerServices;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[DisassemblyDiagnoser, HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public unsafe class Benchmarks
{
    private struct MyStruct
    {
        public int A;
        public int Field;
    }

    [MethodImpl(MethodImplOptions.AggressiveInlining)]
    private static nint OffsetOfFieldInline()
    {
        MyStruct dummy;
        return (nint)((byte*)&dummy.Field - (byte*)&dummy);
    }

    [Benchmark]
    [Arguments(1_000)]
    public nint OffsetOfFieldLoop(int n)
    {
        nint sum = 0;
        for (int i = 0; i < n; i++)
            sum += OffsetOfFieldInline();

        return sum;
    }

}
```

Without the fold, the loop repeatedly computes the field offset. With the fold, each iteration simply adds the constant `4`.

```
; Arm64
--- .NET 10
+++ .NET 11
@@ -1,7 +1,6 @@
 G_M000_IG01:
-            stp     fp, lr, [sp, #-0x20]!
+            stp     fp, lr, [sp, #-0x10]!
             mov     fp, sp
-            str     xzr, [fp, #0x18]

 G_M000_IG02:
             mov     x0, xzr

@@ -9,22 +8,18 @@
             ble     G_M000_IG05

 G_M000_IG03:
-            add     x2, fp, #0x1C
-            add     x3, fp, #24
-            sub     x2, x2, x3
             align   [0 bytes for IG04]
             align   [0 bytes]
             align   [0 bytes]
             align   [0 bytes]

 G_M000_IG04:
-            str     xzr, [fp, #0x18]
-            add     x0, x2, x0
+            add     x0, x0, #4
             sub     w1, w1, #1
             cbnz    w1, G_M000_IG04

 G_M000_IG05:
-            ldp     fp, lr, [sp], #0x20
+            ldp     fp, lr, [sp], #0x10
             ret     lr

-; Total bytes of code 60
+; Total bytes of code 40
```

[dotnet/runtime#121985](https://github.com/dotnet/runtime/pull/121985) from [@hez2010](https://github.com/hez2010) enables the JIT to evaluate `SequenceEqual` at compile time when both inputs are known. `SequenceEqual` normally walks two sequences element by element, stopping at the first mismatch. But if inlining exposes both sequences as constants, there’s nothing useful left to do at run time: the JIT can compare them while compiling and replace the whole operation with a constant `true` or `false`. This intrinsic underpins APIs including `MemoryExtensions.SequenceEqual`, `ReadOnlySpan<T>.SequenceEqual`, and `string.Equals`.

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[DisassemblyDiagnoser, HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private static string AlphaLower => "abcdefghijklmnopqrstuvwxyz";
    private static string AlphaUpper => "ABCDEFGHIJKLMNOPQRSTUVWXYZ";

    [Benchmark]
    public bool CompareEqual() => AlphaLower.Equals(AlphaLower);

    [Benchmark]
    public bool CompareDistinct() => AlphaLower.Equals(AlphaUpper);
}
```

Because these properties aren’t `const`, the C# compiler can’t evaluate the comparisons. The JIT, however, can see the string literals after inlining. It now folds comparisons of the same input whose contents are available at the time of compilation. `CompareDistinct` therefore becomes a constant `false`.

```
; x64
--- .NET 10
+++ .NET 11
-mov       rax,LOWER_STRING
-mov       rcx,UPPER_STRING
-add       rax,0C
-vmovups   ymm0,[rax]
-vmovups   ymm1,[rax+14]
-vmovups   ymm2,[rcx]
-vpxor     ymm0,ymm2,ymm0
-vpxor     ymm1,ymm1,[rcx+14]
-vpor      ymm0,ymm1,ymm0
-vptest    ymm0,ymm0
-sete      al
-movzx     eax,al
-vzeroupper
+xor       eax,eax
 ret

-; Total bytes of code 65
+; Total bytes of code 3
```

Folding an operation is only the first step, though. The result can then simplify later code, even when it’s a vector. [dotnet/runtime#127124](https://github.com/dotnet/runtime/pull/127124) extends assertion propagation to 128-bit integer vector constants. If a branch establishes that a vector is zero, uses of that vector within the branch can now be replaced with zero and simplified just like scalar values.

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;
using System.Runtime.CompilerServices;
using System.Runtime.Intrinsics;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[DisassemblyDiagnoser, HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private int _selector;

    [MethodImpl(MethodImplOptions.NoInlining)]
    private Vector128<int> Compute() => _selector == 0 ? Vector128<int>.Zero : Vector128.Create(7);

    [Benchmark]
    public int AndNotIfZero()
    {
        Vector128<int> v = Compute();
        if (v == Vector128<int>.Zero)
        {
            Vector128<int> masked = Vector128.AndNot(v, Vector128.Create(0x00FF00FF));
            return masked[0];
        }

        return -1;
    }
}
```

In the benchmark’s zero branch, the JIT can now fold away the mask creation, `AndNot`, and lane extraction, reducing the Arm64 method from 68 bytes to 56 bytes. This currently applies to integer vectors up to 128 bits (floating-point equality has additional NaN and signed-zero semantics that prevent the same reasoning at present).

```
; Arm64
--- .NET 10
+++ .NET 11
@@ -11,14 +11,11 @@
             umaxp   v16.4s, v0.4s, v0.4s
             umov    x0, v16.d[0]
             movn    w1, #0
-            movi    v16.8h, #0xFF,  LSL #8
-            and     v16.4s, v0.4s, v16.4s
-            smov    x2, v16.s[0]
             cmp     x0, #0
-            csel    w0, w1, w2, ne
+            cinc    w0, w1, eq
G_M000_IG03:
             ldp     fp, lr, [sp], #0x10
             ret     lr
-; Total bytes of code 68
+; Total bytes of code 56
```

Two backend cleanups take advantage of simpler expressions. [dotnet/runtime#124332](https://github.com/dotnet/runtime/pull/124332) from [@jonathandavies-arm](https://github.com/jonathandavies-arm) removes an unnecessary negation when Arm64 code compares a negated value with zero. And [dotnet/runtime#124642](https://github.com/dotnet/runtime/pull/124642) from [@yykkibbb](https://github.com/yykkibbb) lets short-circuit Boolean returns fold even when inlining has left unused writes in the same block; those stores previously obscured the simple Boolean expression from the optimizer.

Branches offer another opportunity for simplification. Modern processors work on several instructions at different stages at the same time. When a processor encounters a conditional branch, it predicts which path will be taken so that it can continue fetching and executing instructions speculatively. A correct prediction hides much of the branch’s cost. A misprediction throws away that speculative work, redirects instruction fetch to the correct path, and refills the processor’s execution pipeline. That can make the predictability of a branch as important as the work in either branch. The JIT can sometimes avoid that variability, particularly inside small hot loops, by replacing a branch with a conditional move instruction or by recognizing that several branches describe one simpler condition. This isn’t always profitable: branchless code may evaluate work that a predictable branch would skip, making the branching code less expensive in the majority case. But it can be valuable for small, data-dependent choices.

[dotnet/runtime#124567](https://github.com/dotnet/runtime/pull/124567) recognizes zero-based equality chains, e.g. `value == 0 || value == 1 || value == 2`. Such chains can be replaced with an unsigned range check, e.g. `(uint)value <= 2`, producing a branchless result. The unsigned comparison also handles negative inputs: when interpreted as unsigned, any negative `int` is larger than the upper bound.

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[DisassemblyDiagnoser]
[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private int _value = 2;

    [Benchmark]
    public bool IsLetterCategory() =>
        _value == 0 ||
        _value == 1 ||
        _value == 2 ||
        _value == 3 ||
        _value == 4;
}
```

The .NET 10 JIT already combines the first four comparisons, but still needs a branch and a separate comparison for `4`:

```
; x64
mov       ecx,[rcx+8]
cmp       ecx,3
ja        CHECK_FOUR
mov       eax,1
ret

CHECK_FOUR:
cmp       ecx,4
sete      al
movzx     eax,al
ret
```

.NET 11 recognizes the whole chain as one unsigned range check, reducing the method from 24 bytes to 13:

```
; x64
mov       eax,[rcx+8]
cmp       eax,5
setb      al
movzx     eax,al
ret
```

[dotnet/runtime#128524](https://github.com/dotnet/runtime/pull/128524) from [@BoyBaykiller](https://github.com/BoyBaykiller) extends the same optimization to contiguous ranges that don’t start at zero. For example, `x == 3 || x == 4 || x == 5` can become `(uint)(x - 3) <= 2`.

Casts can obscure an equally simple comparison. [dotnet/runtime#128091](https://github.com/dotnet/runtime/pull/128091) from [@BoyBaykiller](https://github.com/BoyBaykiller) broadens cast-comparison optimization to equality and inequality. In this benchmark, converting a `uint` to `ulong` adds no information needed to compare it with `uint.MaxValue`, so the JIT can keep the comparison at 32 bits:

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;
using System.Linq;
using System.Runtime.CompilerServices;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[DisassemblyDiagnoser, HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private readonly int[] _values = Enumerable.Range(0, 128).ToArray();

    [Benchmark]
    public int CastEquality()
    {
        int matches = 0;
        foreach (int value in _values)
            if ((ulong)(uint)value == uint.MaxValue)
                matches++;

        return matches;
    }
}
```

The widening cast disappears, reducing the Arm64 method from 80 bytes to 76 bytes.

```
; Arm64
--- .NET 10
+++ .NET 11
@@ -18,8 +18,7 @@

 G_M000_IG04:
             ldr     w3, [x0]
-            mov     x4, #0xFFFFFFFF
-            cmp     x3, x4
+            cmn     w3, #1
             beq     G_M000_IG08

 G_M000_IG05:
@@ -38,4 +37,4 @@
             add     w1, w1, #1
             b       G_M000_IG05

-; Total bytes of code 80
+; Total bytes of code 76
```

The examples thus far simplify individual comparisons. [dotnet/runtime#127181](https://github.com/dotnet/runtime/pull/127181) also combines multiple comparisons in the same expression. For example, `(x >= c) && (x <= c)` can only be true when `x == c`; corresponding OR forms can be simplified similarly.

Once the JIT can reason about one comparison in terms of another, it can apply the same idea across branches. [dotnet/runtime#126587](https://github.com/dotnet/runtime/pull/126587) removes an earlier test when a later, stronger test subsumes it. For example, `if (x > 0) if (x > 1)` needs only the `x > 1` test, as reaching the nested body with `x > 1` necessarily also means `x > 0`.

Rather than simply removing a test, the JIT can sometimes use the outcome of an earlier branch to choose the destination of a later one. This is known as “jump threading”: the JIT threads a control-flow path through the intervening jumps directly to its eventual destination. For example, consider:

```
int value = condition ? 1 : 2;
if (value == 1)
{
    One();
}
else
{
    Two();
}
```

The path where `condition` is true can go directly to `One`, while the false path can go directly to `Two`, eliminating the second test, effectively:

```
int value;
if (condition)
{
    value = 1;
    One();
}
else
{
    value = 2;
    Two();
}
```

[dotnet/runtime#126812](https://github.com/dotnet/runtime/pull/126812) lets this continue through more places where paths rejoin, and [dotnet/runtime#127103](https://github.com/dotnet/runtime/pull/127103) ensures the rewritten values remain correct in more of those cases. [dotnet/runtime#127950](https://github.com/dotnet/runtime/pull/127950) carries relationships between values further, so facts like `a > 10` and `b > a` can simplify later branches or bounds. The same reasoning can apply to type information. [dotnet/runtime#128500](https://github.com/dotnet/runtime/pull/128500) combines the known types of instances arriving from multiple paths; if every value derives from the tested base type, the JIT can remove the `is` test after the paths merge. And [dotnet/runtime#127434](https://github.com/dotnet/runtime/pull/127434) from [@hez2010](https://github.com/hez2010) lets redundant-branch elimination look through empty jump blocks. Such a block contains no work of its own and exists only to redirect control elsewhere, but it could still hide the relationship between two conditions from the optimizer. Consider this benchmark:

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;
using System.Runtime.CompilerServices;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[DisassemblyDiagnoser, HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private static object? s_sink;

    private int _x = 20;
    private int _y = 30;
    private bool _flag = true;
    private int _count = 5;

    [Benchmark]
    public bool TransitiveComparison() => TransitiveComparison(_x, _y);

    [Benchmark]
    public bool MergedTypeCheck() => MergedTypeCheck(_flag);

    [Benchmark]
    public int NestedThresholds() => NestedThresholds(_count);

    [MethodImpl(MethodImplOptions.NoInlining)]
    private static bool TransitiveComparison(int x, int y)
    {
        if (x > 10 && x < 100 && y > x)
            return y > 0;

        return false;
    }

    [MethodImpl(MethodImplOptions.NoInlining)]
    private static bool MergedTypeCheck(bool flag)
    {
        object shape = flag ? new Circle() : new Rectangle();
        s_sink = shape;
        return shape is Shape;
    }

    [MethodImpl(MethodImplOptions.NoInlining)]
    private static int NestedThresholds(int count)
    {
        if (count > 1)
            if (count > 2)
                if (count > 3)
                    if (count > 4)
                        return 1;

        return 3;
    }

    private abstract class Shape;
    private sealed class Circle : Shape;
    private sealed class Rectangle : Shape;
}
```

In `TransitiveComparison`, reaching `y > 0` means the JIT already knows that `x > 10` and `y > x`, which together prove that `y` is positive. The final comparison disappears, reducing the Arm64 method from 40 bytes to 36 bytes:

```
; Arm64
--- .NET 10
+++ .NET 11
             cmp     w1, w0
             ccmp    w2, w3, c, gt
-            ccmp    w1, #0, nzc, ls
-            cset    x0, gt
+            cset    x0, ls

-; Total bytes of code 40
+; Total bytes of code 36
```

In `MergedTypeCheck`, each path creates a different concrete type, but both derive from `Shape`. .NET 11 keeps the allocations and the store that make the example observable, but replaces the `is` helper call and its result test with the constant `true`, reducing the method from 112 bytes to 88 bytes:

```
; Arm64
--- .NET 10
+++ .NET 11
             bl      CORINFO_HELP_ASSIGN_REF
-            movz    x0, #0xEA30
-            movk    x0, #0x4EB LSL #16
-            movk    x0, #0x7FFF LSL #32
-            bl      CORINFO_HELP_ISINSTANCEOFCLASS
-            cmp     x0, #0
-            cset    x0, ne
+            mov     w0, #1

-; Total bytes of code 112
+; Total bytes of code 88
```

For `NestedThresholds`, reaching the `return 1` requires `count` to be greater than all four constants, which is equivalent to just `count > 4`. Once redundant-branch elimination can see through the empty jump blocks left behind while simplifying the nested conditions, the other three comparisons disappear:

```
; Arm64
--- .NET 10
+++ .NET 11
             mov     w1, #3
             mov     w2, #1
-            cmp     w0, #1
-            ccmp    w0, #2, nzc, gt
-            ccmp    w0, #3, nzc, gt
-            ccmp    w0, #4, nzc, gt
+            cmp     w0, #4
             csel    w0, w1, w2, le

-; Total bytes of code 44
+; Total bytes of code 32
```

Removing a redundant branch is ideal; why do work when it’s provably unnecessary? Often, however, the branch is necessary, as both outcomes are possible (or at least not provably impossible). In such cases, the JIT may still be able to avoid branching via specialized instructions that bake the choice into the instruction. “If-conversion” replaces a small `if`/`else` with a conditional-move instruction or another branchless form when both alternatives are cheap. The JIT has been able to do this for several releases, and improves in .NET 11. [dotnet/runtime#124738](https://github.com/dotnet/runtime/pull/124738) from [@BoyBaykiller](https://github.com/BoyBaykiller) recognizes an earlier default assignment as the implicit `else`, so `bool x = false; if (cond) x = true;` can become the same branchless form as an explicit `else`. [dotnet/runtime#127915](https://github.com/dotnet/runtime/pull/127915) from [@BoyBaykiller](https://github.com/BoyBaykiller) handles the opposite cleanup, removing a conditional selection when both outcomes are the same constant while preserving any side effects from evaluating the condition. [dotnet/runtime#128533](https://github.com/dotnet/runtime/pull/128533) from [@BoyBaykiller](https://github.com/BoyBaykiller) also helps these Boolean optimizations meet in the middle by normalizing power-of-two bit tests. A power of two has exactly one bit set, in which case `(A & bit) == bit` is equivalent to `(A & bit) != 0`; putting both forms into the same canonical representation makes them easier to combine with surrounding conditions. All three improvements are visible in the following benchmarks:

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;
using System.Runtime.CompilerServices;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[DisassemblyDiagnoser, HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private int _left = 1;
    private int _right = 2;
    private double _double = 0.0;
    private int _bits = 4;

    [Benchmark]
    public bool ImplicitElse() => ImplicitElse(_left, _right);

    [Benchmark]
    public bool IsDefaultValue() => IsDefaultValue(_double);

    [Benchmark]
    public bool HasEitherBit() => HasEitherBit(_bits);

    [MethodImpl(MethodImplOptions.NoInlining)]
    private static bool ImplicitElse(int left, int right)
    {
        bool leftIsSmaller = false;
        if (left < right)
            leftIsSmaller = true;

        return leftIsSmaller;
    }

    [MethodImpl(MethodImplOptions.NoInlining)]
    private static bool IsDefaultValue(double value) => 0.0.Equals(value);

    [MethodImpl(MethodImplOptions.NoInlining)]
    private static bool HasEitherBit(int value) =>
        ((value & 4) == 4) || ((value & 8) == 8);
}
```

For `ImplicitElse`, .NET 10 already avoids a branch, but it still materializes both Boolean values and selects between them. In .NET 11, the method becomes just the comparison and a `cset`, shrinking from 36 bytes to 24 bytes:

```
; Arm64
--- .NET 10
+++ .NET 11
-            mov     w2, wzr
-            mov     w3, #1
             cmp     w0, w1
-            csel    w2, w2, w3, ge
-            mov     w0, w2
+            cset    x0, lt

-; Total bytes of code 36
+; Total bytes of code 24
```

`0.0.Equals(value)` needs to account for `NaN`, but because the left operand is zero, the case where both operands are `NaN` can never apply. Removing the conditional selection for that case leaves one floating-point comparison and one `cset`, reducing `IsDefaultValue` from 40 bytes to 24 bytes:

```
; Arm64
--- .NET 10
+++ .NET 11
             fcmp    d0, #0.0
-            beq     G_M000_IG04
-
-G_M000_IG03:
-            fcmp    d0, d0
-            csel    w0, wzr, wzr, eq
-            b       G_M000_IG05
-
-G_M000_IG04:
-            mov     w0, #1
-
-G_M000_IG05:
+            cset    x0, eq
+
+G_M000_IG03:
             ldp     fp, lr, [sp], #0x10
             ret     lr

-; Total bytes of code 40
+; Total bytes of code 24
```

Finally, normalizing both power-of-two comparisons lets the JIT combine their results. The short-circuit branch in `HasEitherBit` is replaced by two masks and an `or`, reducing the method from 40 bytes to 36 bytes:

```
; Arm64
--- .NET 10
+++ .NET 11
-            tbz     w0, #2, G_M000_IG05
-
-G_M000_IG03:
-            mov     w0, #1
-
-G_M000_IG04:
-            ldp     fp, lr, [sp], #0x10
-            ret     lr
-
-G_M000_IG05:
-            tst     w0, #8
+            and     w1, w0, #4
+            and     w0, w0, #8
+            orr     w0, w1, w0
+            cmp     w0, #0
             cset    x0, ne

-G_M000_IG06:
+G_M000_IG03:
             ldp     fp, lr, [sp], #0x10
             ret     lr

-; Total bytes of code 40
+; Total bytes of code 36
```

Not every simplification depends on broader control-flow reasoning. “Peephole optimizations” instead replace a short, recognizable pattern with an equivalent cheaper one. Each may save only an instruction or expose a form that another optimization understands, but these patterns can occur very frequently on hot paths throughout generated code. For example, [dotnet/runtime#126529](https://github.com/dotnet/runtime/pull/126529) from [@BoyBaykiller](https://github.com/BoyBaykiller) recognizes that `255 - x` for a `byte` is equivalent to `x ^ 255`: both simply flip all eight bits, but the latter can remove an instruction if it’s able to replace a negation and add with an xor. Similarly, `-1 - x` can turn into the equivalent of `~x`.

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[DisassemblyDiagnoser, HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private readonly byte[] _data = new byte[4096];

    [GlobalSetup]
    public void Setup() => new Random(42).NextBytes(_data);

    [Benchmark]
    public int InvertBytes()
    {
        int sum = 0;
        foreach (byte b in _data) sum += 255 - b;
        return sum;
    }
}
```

In .NET 11, the loop loses a separate negate and add:

```
; Arm64
--- .NET 10
+++ .NET 11
@@ -19,9 +19,8 @@

 G_M000_IG04:
             ldrb    w4, [x0, w2, UXTW]
-            neg     w4, w4
+            eor     w4, w4, #255
             add     w1, w4, w1
-            add     w1, w1, #255
             add     w2, w2, #1
             cmp     w3, w2
             bgt     G_M000_IG04
@@ -33,4 +32,4 @@
             ldp     fp, lr, [sp], #0x10
             ret     lr

-; Total bytes of code 76
+; Total bytes of code 72
```

[dotnet/runtime#129361](https://github.com/dotnet/runtime/pull/129361) removes another unnecessary instruction when comparing an `sbyte` with a constant that fits in eight bits. The JIT can compare the byte directly, with no sign extension:

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;
using System.Runtime.CompilerServices;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[DisassemblyDiagnoser, HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private sbyte _value = -65;

    [Benchmark]
    public bool IsLow() => IsLow(_value);

    [MethodImpl(MethodImplOptions.NoInlining)]
    private static bool IsLow(sbyte value) => value < -64;
}
```

The optimized codegen then compares the byte directly, removing the `movsx` sign-extension instruction (though the JIT still retains it in the few comparison forms that require a full-width sign bit for correctness).

```
; x64
--- .NET 10
+++ .NET 11
-movsx  rax,cl
-cmp    eax,0FFFFFFC0
+cmp    cl,0C0
 setl   al
 movzx  eax,al
 ret

-; Total bytes of code 14
+; Total bytes of code 10
```

[dotnet/runtime#125180](https://github.com/dotnet/runtime/pull/125180) from [@saucecontrol](https://github.com/saucecontrol) improves non-overflowing `float` and `double` conversions to `long` and `ulong` on x86 machines with AVX-512 or AVX10.2. These casts have defined behavior for NaN and out-of-range values, so older code used a helper to preserve those semantics. The newer instruction set lets the JIT keep the normal path inline and register-based, avoiding the helper call; machines that don’t support these instructions retain the existing fallback.

```
// Run with 32-bit x86 dotnet on a machine with AVX-512 or AVX10.2:
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[DisassemblyDiagnoser, HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private float _single = 123_456.75f;
    private double _double = 123_456.75;

    [Benchmark] public long SingleToInt64() => (long)_single;
    [Benchmark] public ulong SingleToUInt64() => (ulong)_single;
    [Benchmark] public long DoubleToInt64() => (long)_double;
    [Benchmark] public ulong DoubleToUInt64() => (ulong)_double;
}
```

| Method         | Runtime   | Mean     | Ratio | Code Size |
| -------------- | --------- | -------- | ----- | --------- |
| SingleToInt64  | .NET 10.0 | 5.103 ns | 1.00  | 31 B      |
| SingleToInt64  | .NET 11.0 | 2.093 ns | 0.41  | 54 B      |
|                |           |          |       |           |
| SingleToUInt64 | .NET 10.0 | 4.810 ns | 1.00  | 31 B      |
| SingleToUInt64 | .NET 11.0 | 1.366 ns | 0.28  | 34 B      |
|                |           |          |       |           |
| DoubleToInt64  | .NET 10.0 | 4.834 ns | 1.00  | 31 B      |
| DoubleToInt64  | .NET 11.0 | 2.101 ns | 0.43  | 54 B      |
|                |           |          |       |           |
| DoubleToUInt64 | .NET 10.0 | 4.663 ns | 1.00  | 31 B      |
| DoubleToUInt64 | .NET 11.0 | 1.363 ns | 0.29  | 34 B      |

### Vectorization

SIMD, or “single instruction, multiple data”, is the concept of one instruction applying the same operation to several values at once. A “scalar” `add`, for example, might combine one pair of 32-bit integers, while a 128-bit SIMD `add` can combine “vectors” of four pairs in the same instruction; 256- and 512-bit variants can handle vectors of eight and sixteen pairs, respectively. When the iterations of an operation are independent, “vectorizing” a loop can therefore replace several scalar iterations with one, improving the throughput of the loop significantly.

.NET exposes portable (they work on any machine) variable-width vector type `Vector<T>` (which can represent different counts of `T` depending on the current hardware), fixed-width `Vector64<T>` through `Vector512<T>` types (which always represent the same count of `T`), and architecture-specific intrinsics (performing operations on such vector types which the JIT then maps to the right underlying hardware instructions). Each element in a vector is often referred to as a “lane”. Because the JIT recognizes these operations directly, it can fold constants, select instructions, and remove unsupported paths without treating them as normal method calls.

A variety of PRs in .NET 11 improve AVX-512 broadcasting and masking. Embedded broadcasting lets an instruction load a single scalar value and replicate it across all vector lanes, avoiding the need to materialize a full-width vector constant in memory to feed into the instruction. For example, this bitwise AND instruction:

```
; x64
vpandd  zmm0, zmm1, dword ptr [reloc @RWD00] {1to16}
```

can replace this one:

```
; x64
vpandd  zmm0, zmm1, zmmword ptr [reloc @RWD00]
```

storing only 4 bytes in the read-only data section rather than 64. Because the broadcast is handled as part of the load, there’s no additional instruction-level latency; the primary benefit is reduced data size and cache footprint.

Embedded masking similarly lets an instruction update only a subset of the lanes. A mask is one bit per vector lane, where each bit indicates whether and how the operation should affect the corresponding lane. Without embedded masking, code often needs to compute every lane and then blend that result with the old value, so folding the mask into the operation can remove both the separate blend and a zero-vector setup. [dotnet/runtime#117700](https://github.com/dotnet/runtime/pull/117700) from [@saucecontrol](https://github.com/saucecontrol) improves broadcast selection when an intrinsic’s natural element size differs from its managed vector type. VNNI, the Vector Neural Network Instructions used for small-integer multiply-accumulate operations, and bitwise operations can now use the smallest valid repeated constant, avoiding a full-vector load.

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0
// Requires AVX-VNNI and AVX-512F.

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;
using System.Runtime.Intrinsics;
using System.Runtime.Intrinsics.X86;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[DisassemblyDiagnoser, HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private readonly Vector128<byte> _bytes = Vector128.Create((byte)1);
    private readonly Vector128<ulong> _u64 = Vector128.Create(1UL);
    private readonly Vector128<uint> _u32 = Vector128.Create(1U);
    private readonly Vector512<int> _v512 = Vector512.Create(1);
    private int _n = 42;

    [Benchmark]
    public Vector128<int> VnniBroadcast() =>
        AvxVnni.MultiplyWideningAndAdd(
            Vector128<int>.Zero, _bytes, Vector128<sbyte>.One);

    [Benchmark]
    public Vector128<uint> MaskAnd() =>
        Vector128.ConditionalSelect(
            Vector128.GreaterThan(_u32, Vector128<uint>.Zero),
            (_u64 & Vector128<uint>.One.AsUInt64()).AsUInt32(),
            Vector128<uint>.Zero);

    [Benchmark]
    public Vector512<int> BlendMaskAllOnes() =>
        Avx512F.BlendVariable(
            Vector512.Create(_n),
            _v512,
            Vector512.Create(-1));

    [Benchmark]
    public Vector512<int> MultiInsert() =>
        Vector512.ConditionalSelect(
            Vector512.Create(0, -1, 0, 0, 0, -1, 0, 0, 0, -1, 0, 0, 0, -1, 0, 0),
            _v512,
            Vector512.Create(_n));

    [Benchmark]
    public Vector512<int> MultiInsertZero() =>
        Avx512F.BlendVariable(
            _v512,
            Vector512<int>.Zero,
            Vector512.Create(0, -1, 0, 0, 0, -1, 0, 0, 0, -1, 0, 0, 0, -1, 0, 0));
}
```

In .NET 11, this results in 12 fewer bytes in the read-only data section, and 12 fewer bytes of constant-pool cache footprint.

```
; x64
-C4E279503500000000   vpdpbusd xmm6, xmm0, xmmword ptr [reloc @RWD00]
+62F27D18503500000000 vpdpbusd xmm6, xmm0, dword ptr [reloc @RWD00] {1to4}

-RWD00  dq 0101010101010101h, 0101010101010101h
+RWD00  dd 01010101h
```

The fix also impacts embedded masking. For example, with `MaskAnd` previously, the AND used a qword broadcast, `{1to2}`, and a separate blend then moved the masked result, meaning two instructions. Now that `Vector128<uint>.One` can be broadcast at dword granularity, the mask’s element size and the AND’s element size agree, unlocking using the single merged-masked form. This pattern shows up throughout vectorized algorithms that do lots of bitwise manipulation and hashing, including implementations in System.Numerics.Tensors, System.IO.Hashing, and System.Private.CoreLib.

```
; x64
-       vpandq   xmm0, xmm0, qword ptr [reloc @RWD00] {1to2}
-       vpblendmd xmm0 {k1}{z}, xmm0, xmm0
+       vpandd   xmm0 {k1}{z}, xmm0, dword ptr [reloc @RWD00] {1to4}

; Code: 45 → 39 bytes; data: 8 bytes → 4 bytes
```

That `MaskAnd` example starts as an AND followed by a blend, an operation that chooses independently for each vector lane whether to take its value from one input or the other, with the JIT able to fold those two operations together. Similar opportunities arise with blends more generally. Sometimes the mask or one of the inputs makes the choice trivial, e.g. an all-ones mask always selects the same input, so the blend is just a move. If one input is zero, it can often become an `AND` or `ANDN`. AVX-512 provides more options still, as constant masks and zeroing can be encoded directly in the instruction. [dotnet/runtime#123146](https://github.com/dotnet/runtime/pull/123146) from [@saucecontrol](https://github.com/saucecontrol) makes these simplifications consistently across the portable and hardware-specific APIs. A blend with an all-ones mask provides a particularly clear example:

```
// Run on x64 with AVX-512:
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using System.Runtime.Intrinsics;
using System.Runtime.Intrinsics.X86;
using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[DisassemblyDiagnoser]
[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private readonly Vector512<int> _values = Vector512.Create(1);
    private int _n = 42;

    [GlobalSetup]
    public void Setup()
    {
        if (!Avx512F.IsSupported)
            throw new PlatformNotSupportedException();
    }

    [Benchmark]
    public Vector512<int> BlendMaskAllOnes() =>
        Avx512F.BlendVariable(Vector512.Create(_n), _values, Vector512.Create(-1));
}
```

The generated code no longer needs to create the first input, load the mask, or perform the blend. It simply loads the input the all-ones mask would always select:

```
; x64
-vpbroadcastd zmm0, dword ptr [rcx+8]
-kmovq       k1, qword ptr [RWD00]
-vpblendmd   zmm0 {k1}, zmm0, [rcx+48]
+vmovups     zmm0, [rcx+48]
 vmovups     [rdx], zmm0
 mov         rax, rdx
 vzeroupper
 ret

; 39 bytes → 23 bytes
```

### Intrinsics

An intrinsic is a managed API that the JIT recognizes and special-cases. Often that special-casing involves actually replacing calls to the method with custom code that’s behaviorally equivalent but better in some way (faster, smaller, etc.)

As an example, [dotnet/runtime#128678](https://github.com/dotnet/runtime/pull/128678) improves recognition of generic-math calls to `IBinaryNumber<T>.Log2`. The method computes the base-2 logarithm of an integer, equivalent to the index of the number’s highest set bit; for example, `Log2(16)` is `4`. Previously, the JIT’s normalized integer type lost the signedness needed to import the operation directly as an intrinsic. Inlining the managed implementation could still produce the same optimized code, but when inlining didn’t happen, the managed call remained. In .NET 11, the JIT consults the precise type and imports the operation directly: unsigned and non-negative signed inputs can become leading-zero-count or bit-scan arithmetic, while a negative signed value retains the managed fallback and its exact exception behavior.

Sometimes the JIT has a perfectly good intrinsic lowering but doesn’t recognize a call that should use it. `Enum.Equals` from a generic `T : Enum` context was a good example. Even though both arguments to the generic helper are strongly typed as `T`, an enum doesn’t provide an `Equals(T)` method; it inherits the virtual `Enum.Equals(object)` implementation. The second argument therefore needs to be boxed to pass it as `object`. The receiver is invoked with a constrained virtual call, but because the concrete enum doesn’t override the method itself, it too needs to be boxed to invoke the implementation on `System.Enum`. Thus, what looks like a strongly-typed comparison can end up allocating two boxes and making a virtual call. In .NET 11, [dotnet/runtime#122779](https://github.com/dotnet/runtime/pull/122779) eliminates this overhead by teaching the JIT to recognize the call and fold it to a direct comparison of the enum’s underlying integer values. For example:

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;
using System.Runtime.CompilerServices;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[MemoryDiagnoser(false), HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private static readonly StringComparison[] s_values =
    {
        StringComparison.Ordinal, StringComparison.OrdinalIgnoreCase, 
        StringComparison.CurrentCulture, StringComparison.CurrentCultureIgnoreCase,
        StringComparison.InvariantCulture, StringComparison.Ordinal,
    };

    [Benchmark]
    public int CountOrdinal_Generic()
    {
        int count = 0;
        foreach (var v in s_values)
            if (EqualsGeneric(v, StringComparison.Ordinal))
                count++;

        return count;
    }

    [MethodImpl(MethodImplOptions.NoInlining)]
    private static bool EqualsGeneric<T>(T a, T b) where T : Enum => a.Equals(b);
}
```

Once the JIT knows the callee is `Enum.Equals` and knows the exact enum type, it asks the runtime for the underlying integer type and replaces the virtual call with a direct comparison. That in turn makes both box/unbox pairs redundant, and the generated code contains neither allocation. For the six comparisons performed here, .NET 10 creates twelve boxes, totaling 288 bytes. In .NET 11, the helper becomes just the integer comparison, eliminating both the allocations and the virtual dispatch.

| Method                | Runtime   | Mean     | Ratio | Allocated |
| --------------------- | --------- | -------- | ----- | --------- |
| CountOrdinal\_Generic | .NET 10.0 | 59.24 ns | 1.00  | 288 B     |
| CountOrdinal\_Generic | .NET 11.0 | 10.01 ns | 0.17  | –         |

NativeAOT had been carrying an equivalent optimization for years, implemented as IL rewriting in ILCompiler that patches `Enum.Equals` to use typed comparisons. With the JIT now handling it, including in NativeAOT’s own use of the JIT (NativeAOT uses the JIT ahead of time rather than just in time), [dotnet/runtime#123086](https://github.com/dotnet/runtime/pull/123086) deletes that rewriting and its supporting machinery.

[dotnet/runtime#127329](https://github.com/dotnet/runtime/pull/127329) improves the `Vector256.Sum` and `Vector512.Sum` intrinsics. The JIT now performs most of the reduction at full width and combines the per-lane results at the end, avoiding the extracts and duplicate shuffle sequences needed when splitting wide vectors into 128-bit pieces. And [dotnet/runtime#127402](https://github.com/dotnet/runtime/pull/127402) extends vector-constant propagation from 128-bit vectors to `Vector256` and `Vector512`. Code that compares a wide vector with a known sentinel can now simplify subsequent uses just as narrower vectors already could. The following benchmark exemplifies both:

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0
// Requires AVX2 for the assembly shown below.

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;
using System.Runtime.CompilerServices;
using System.Runtime.Intrinsics;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[DisassemblyDiagnoser, HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private readonly Vector256<float> _floats =
        Vector256.Create(1.0f, 2.0f, 3.0f, 4.0f, 5.0f, 6.0f, 7.0f, 8.0f);
    private int _selector;

    [Benchmark]
    public float Sum() => Vector256.Sum(_floats);

    [Benchmark]
    public int TransformWhenKnown()
    {
        Vector256<int> value = GetVector();
        if (value == Vector256.Create(0, 1, 2, 3, 4, 5, 6, 7))
            return (value + Vector256.Create(10)).GetElement(6);

        return -1;
    }

    [MethodImpl(MethodImplOptions.NoInlining)]
    private Vector256<int> GetVector() =>
        _selector == 0 ?
            Vector256.Create(0, 1, 2, 3, 4, 5, 6, 7) :
            Vector256.Create(7);
}
```

For `Sum`, .NET 10 separately reduces each 128-bit half and then adds the two scalar results. In .NET 11, the permutes and adds operate on both halves in parallel as 256-bit instructions, after which only the two already-reduced halves need to be combined:

```
; x64
 vmovups   ymm0, [rcx+28]
-vmovaps   ymm1, ymm0
-vpermilps xmm2, xmm1, 0B1
-vaddps    xmm1, xmm2, xmm1
-vpermilps xmm2, xmm1, 4E
-vaddps    xmm1, xmm2, xmm1
-vextractf128 xmm0, ymm0, 1
-vpermilps xmm2, xmm0, 0B1
-vaddps    xmm0, xmm2, xmm0
-vpermilps xmm2, xmm0, 4E
-vaddps    xmm0, xmm2, xmm0
-vaddss    xmm0, xmm1, xmm0
+vpermilps ymm1, ymm0, 0B1
+vaddps    ymm0, ymm1, ymm0
+vpermilps ymm1, ymm0, 4E
+vaddps    ymm0, ymm1, ymm0
+vextractf128 xmm1, ymm0, 1
+vaddps    xmm0, xmm1, xmm0

; 63 bytes → 39 bytes
```

`TransformWhenKnown` uses a deliberately non-repeating constant across its eight lanes. On the branch where the comparison succeeds, .NET 11 can replace `value` with that constant, fold the vector addition, and determine that element 6 is `16`. The `vpaddd`, extraction, second 32-byte constant, and associated control flow all disappear:

```
; x64
-cmp      eax, 0FFFFFFFF
-jne      M00_L00
-vmovups  ymm0, [rsp+20]
-vpaddd   ymm0, ymm0, [RWD32]
-vextracti128 xmm0, ymm0, 1
-vpextrd  eax, xmm0, 2
-vzeroupper
-add      rsp, 58
-ret
-
-M00_L00:
-mov      eax, 0FFFFFFFF
+mov      ecx, 0FFFFFFFF
+mov      edx, 10
+cmp      eax, 0FFFFFFFF
+mov      eax, edx
+cmovne   eax, ecx
 vzeroupper
 add      rsp, 58
 ret

; 85 bytes → 59 bytes
```

One of the goals of .NET is that you can write code once and have it run anywhere, optimized for whatever that “anywhere” has to offer. For vectorization, that means providing portable operations whenever the intent is common across instruction sets, while retaining architecture-specific APIs for algorithms that really do need to target a particular machine.

Whenever possible, we want to enable developers to express their algorithms using the portable APIs, and each release of .NET fills additional gaps there. Including .NET 11. [dotnet/runtime#129627](https://github.com/dotnet/runtime/pull/129627) from [@hez2010](https://github.com/hez2010) adds portable APIs for constructing common lane sequences (e.g. `[1, 2, 4, 8]` or `[a, b, a, b]`), concatenating half-vectors (the lower halves of `[a, b, c, d]` and `[w, x, y, z]` producing `[a, b, w, x]`), interleaving (`[a, b]` and `[x, y]` producing `[a, x, b, y]`), de-interleaving (`[a, x, b, y]` producing `[a, b]` and `[x, y]`), and reversal (`[a, b, c, d]` producing `[d, c, b, a]`), along with their JIT intrinsification. These operations were already expressible, but only verbosely and only if you knew which hardware instruction to reach for, e.g. writing `Zip` by hand meant targeting a platform-specific API like `AdvSimd.Arm64.ZipLow`. The new APIs let the code state the transformation and leave instruction selection to the JIT.

Once the intrinsic operation has been recognized, the backend still needs to keep it in a useful vector form while assigning registers and selecting instructions. Vector values are structs, and the JIT will often apply “struct promotion,” tracking a struct’s fields as independent locals so that each can be optimized separately. That’s useful for ordinary structs, but counterproductive when a value is meant to remain in a vector or mask register: splitting it can introduce extra moves and obscure what should be a single whole-value store, particularly after inlining introduces more local stores. [dotnet/runtime#128013](https://github.com/dotnet/runtime/pull/128013) consistently marks SIMD and mask stores as intrinsic-related across platforms, including 32-bit x86 and x64 mask stores, so those locals remain intact. [dotnet/runtime#129563](https://github.com/dotnet/runtime/pull/129563) extends that principle to user-defined structs that are bitcast to SIMD types. This trades away struct promotion for those locals, but enables the JIT to preserve their vector representation.

This matters for user-defined numerical types that store the same data as a hardware vector but expose named fields or domain-specific operations. The following `Vector2Double` is laid out as two adjacent `double` values, so it can be bitcast to `Vector128<double>`, operated on with SIMD, and bitcast back:

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using System.Runtime.CompilerServices;
using System.Runtime.InteropServices;
using System.Runtime.Intrinsics;
using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

public struct Vector2Double(double x, double y)
{
    public double X = x;
    public double Y = y;

    public static Vector2Double operator +(Vector2Double left, Vector2Double right)
    {
        Vector128<double> simdLeft = Unsafe.BitCast<Vector2Double, Vector128<double>>(left);
        Vector128<double> simdRight = Unsafe.BitCast<Vector2Double, Vector128<double>>(right);
        return Unsafe.BitCast<Vector128<double>, Vector2Double>(simdLeft + simdRight);
    }
}

[DisassemblyDiagnoser, HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private readonly Vector2Double _a = new(1.0, 2.0);
    private readonly Vector2Double _b = new(3.0, 4.0);
    private readonly Vector2Double _c = new(5.0, 6.0);

    [Benchmark]
    public Vector2Double Add() => Add(_a, _b, _c);

    [MethodImpl(MethodImplOptions.NoInlining)]
    private static Vector2Double Add(Vector2Double a, Vector2Double b, Vector2Double c) =>
        a + b + c;
}
```

In .NET 10, promotion of the intermediate struct sends the first SIMD result through two stack locations before the second addition. .NET 11 keeps that value in `xmm0`, reducing the helper from 52 bytes to 22 bytes:

```
; x64
-sub       rsp, 28
 vmovups   xmm0, [rdx]
 vaddpd    xmm0, xmm0, [r8]
-vmovaps   [rsp], xmm0
-vmovups   xmm0, [rsp]
-vmovups   [rsp+18], xmm0
-vmovups   xmm0, [rsp+18]
 vaddpd    xmm0, xmm0, [r9]
 vmovups   [rcx], xmm0
 mov       rax, rcx
-add       rsp, 28
 ret

; 52 bytes → 22 bytes
```

[dotnet/runtime#128350](https://github.com/dotnet/runtime/pull/128350) gives the xarch register allocator more freedom around fused multiply-add (FMA) and AVX-512 ternary-logic operations. These instructions can read and overwrite operands in several equivalent arrangements; choosing the arrangement that already matches the surrounding registers avoids otherwise necessary moves.

Generic vector code introduces another wrinkle. Operators like `Vector128<T>.operator ==` return `bool`, so the return type doesn’t reveal the vector’s element type. The JIT instead needs to obtain that type from the operands in order to select the right comparison instruction. In some generic contexts, including helpers built on the internal `ISimdVector` abstraction, the JIT was consulting the wrong type information and failed to import the operator as an intrinsic. It then executed the managed fallback, which compares the lanes individually. [dotnet/runtime#130086](https://github.com/dotnet/runtime/pull/130086) marks these operators so their element type is taken from the first argument. As an example, the generic helpers used internally by ordinal-ignore-case string comparer benefit from this.

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private readonly string _lower = new('a', 256);
    private readonly string _upper = new('A', 256);

    [Benchmark]
    public bool OrdinalIgnoreCase() => string.Equals(_lower, _upper, StringComparison.OrdinalIgnoreCase);
}
```

| Method            | Runtime   | Mean      | Ratio |
| ----------------- | --------- | --------- | ----- |
| OrdinalIgnoreCase | .NET 10.0 | 27.794 ns | 1.00  |
| OrdinalIgnoreCase | .NET 11.0 | 21.861 ns | 0.79  |

.NET 11 adds support for newer x86 capabilities while also improving code generated for existing hardware. These changes benefit both direct users of hardware intrinsics and portable vector code selected by the JIT. For example, [dotnet/runtime#124114](https://github.com/dotnet/runtime/pull/124114) from [@saucecontrol](https://github.com/saucecontrol) improves 32-bit x86 without AVX-512, where converting `uint` to `float` or `double` previously required a runtime helper. Older x86 conversion instructions accept signed integers, and half of the `uint` range doesn’t fit in a signed 32-bit value, which is why the helper existed. The JIT now emits an inline vector-instruction sequence that handles the high bit explicitly, avoiding the call and its register and stack overhead.

```
// Run with 32-bit x86 dotnet and AVX-512 disabled (DOTNET_EnableAVX512=0)
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[DisassemblyDiagnoser, HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private uint _value = 0xF123_4567;

    [Benchmark] public float UInt32ToSingle() => _value;
    [Benchmark] public double UInt32ToDouble() => _value;
}
```

| Method         | Runtime   | Mean     | Ratio | Code Size |
| -------------- | --------- | -------- | ----- | --------- |
| UInt32ToSingle | .NET 10.0 | 4.752 ns | 1.00  | 37 B      |
| UInt32ToSingle | .NET 11.0 | 2.403 ns | 0.51  | 43 B      |
|                |           |          |       |           |
| UInt32ToDouble | .NET 10.0 | 4.727 ns | 1.00  | 39 B      |
| UInt32ToDouble | .NET 11.0 | 2.402 ns | 0.51  | 41 B      |

[dotnet/runtime#124804](https://github.com/dotnet/runtime/pull/124804) from [@alexcovington](https://github.com/alexcovington) adds the AVX-512 Bit Matrix Multiply APIs. A binary matrix treats each bit as an element and combines rows and columns with bitwise operations, not integer multiplication. The instructions are useful in areas such as error correction and CRC computation. Each replaces a much longer sequence of shifts, masks, and exclusive-ORs. And [dotnet/runtime#128365](https://github.com/dotnet/runtime/pull/128365) from [@jamesburton](https://github.com/jamesburton) adds `AvxVnni.V512`, extending the AVX-VNNI APIs from 256-bit to 512-bit operands so the small-integer dot products used by quantized machine-learning models can process 64 bytes per operation instead of 32.

[dotnet/runtime#126062](https://github.com/dotnet/runtime/pull/126062) from [@saucecontrol](https://github.com/saucecontrol) also avoids converting a vector selector into an AVX-512 mask register when the eventual operation still needs the vector form. In such cases, the older-looking vector blend is actually shorter and uses fewer resources:

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;
using System.Runtime.Intrinsics;
using System.Runtime.Intrinsics.X86;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[DisassemblyDiagnoser, HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private readonly Vector128<float> _v1 = Vector128.Create(-1.0f, 2.0f, -3.0f, 4.0f);
    private readonly Vector128<float> _v2 = Vector128.Create(10.0f);

    [GlobalSetup]
    public void Setup()
    {
        if (!Sse41.IsSupported)
            throw new PlatformNotSupportedException();
    }

    [Benchmark]
    public Vector128<float> AddToNegative() =>
        Sse41.BlendVariable(_v1, _v1 + _v2, _v1);
}
```

In .NET 11, you get the simpler `vblendvps` form that avoids an unnecessary k-register operation.

```
; x64
  vmovups   xmm0, [rcx+8]
- vpmovd2m  k1, xmm0
- vaddps    xmm0 {k1}, xmm0, [rcx+18]
+ vaddps    xmm1, xmm0, [rcx+18]
+ vblendvps xmm0, xmm0, xmm1, xmm0
  vmovups   [rdx], xmm0

; 29 bytes → 24 bytes
```

The masked EVEX form looks more modern, but when the mask originates from a vector anyway, the vector-blend sequence is five bytes shorter and avoids writing a mask register. There are only 8 k-registers, and some microarchitectures have port contention for instructions that write them.

A compiler’s cost model assigns estimates to operations and instructions, such as their execution cost or throughput and their impact on code size, and uses those estimates to choose between otherwise legal transformations or instruction sequences. Wrong estimates can still produce semantically correct code, just slower or larger code. With [dotnet/runtime#127048](https://github.com/dotnet/runtime/pull/127048), which updates the JIT’s xarch floating-point and SIMD cost model, the JIT’s cost model reflects modern instruction throughput and encoded size, replacing old x87 assumptions and a flat cost for every intrinsic. That leads to better decisions about common-subexpression elimination and loop unrolling, particularly for 512-bit operations.

[dotnet/runtime#130422](https://github.com/dotnet/runtime/pull/130422) folds a vector lane extraction followed by `WithElement` into one `insertps` that reads the source lane directly. Code such as `destination.WithElement(0, source.GetElement(2))` conceptually extracts a scalar and then inserts it elsewhere. `insertps`, however, has an immediate operand whose bits select both the source lane and destination lane. The JIT can therefore pass the original source vector to the instruction and encode lane 2 in that immediate, instead of first shuffling lane 2 into the scalar position and then inserting it.

Three more xarch changes tighten public SIMD operations on the hardware where they apply. [dotnet/runtime#125666](https://github.com/dotnet/runtime/pull/125666) from [@alexcovington](https://github.com/alexcovington) replaces the dedicated AVX dot-product instruction with a multiply, add, and permute reduction that has better throughput on contemporary cores:

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using System.Numerics;
using System.Runtime.Intrinsics;
using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[DisassemblyDiagnoser, HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private readonly Plane _plane = new(new Vector3(1.0f, 2.0f, 3.0f), 4.0f);
    private readonly Vector4 _vector4 = new(5.0f, 6.0f, 7.0f, 8.0f);
    private readonly Quaternion _quaternion1 = new(1.0f, 2.0f, 3.0f, 4.0f);
    private readonly Quaternion _quaternion2 = new(5.0f, 6.0f, 7.0f, 8.0f);
    private readonly Vector128<float> _vector1 = Vector128.Create(1.0f, 2.0f, 3.0f, 4.0f);
    private readonly Vector128<float> _vector2 = Vector128.Create(5.0f, 6.0f, 7.0f, 8.0f);

    [Benchmark]
    public float PlaneDot() => Plane.Dot(_plane, _vector4);

    [Benchmark]
    public float QuaternionDot() => Quaternion.Dot(_quaternion1, _quaternion2);

    [Benchmark]
    public float Vector128Dot() => Vector128.Dot(_vector1, _vector2);
}
```

| Method        | Runtime   | Mean     | Ratio | Code Size |
| ------------- | --------- | -------- | ----- | --------- |
| PlaneDot      | .NET 10.0 | 2.616 ns | 1.00  | 13 B      |
| PlaneDot      | .NET 11.0 | 1.365 ns | 0.52  | 31 B      |
|               |           |          |       |           |
| QuaternionDot | .NET 10.0 | 2.640 ns | 1.00  | 13 B      |
| QuaternionDot | .NET 11.0 | 1.326 ns | 0.50  | 31 B      |
|               |           |          |       |           |
| Vector128Dot  | .NET 10.0 | 2.597 ns | 1.00  | 13 B      |
| Vector128Dot  | .NET 11.0 | 1.366 ns | 0.53  | 31 B      |

Multiplying vectors of bytes is more involved than multiplying vectors of larger integer types because x86 doesn’t provide a packed byte-multiply instruction. The implementation needs to combine wider 16-bit multiplications while retaining only the low byte of each product. When it couldn’t widen the whole operation to the next vector size, .NET 10 split the input into two halves, widened and multiplied each half, narrowed both results, and joined them again. [dotnet/runtime#126348](https://github.com/dotnet/runtime/pull/126348) from [@saucecontrol](https://github.com/saucecontrol) instead separates the even and odd bytes with masks and shifts, performs two 16-bit multiplications over the full vector width, and recombines the low bytes:

```
// Run on x64 with AVX-512:
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using System.Runtime.Intrinsics;
using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[DisassemblyDiagnoser, HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private readonly Vector512<byte> _left = Vector512.Create((byte)17);
    private readonly Vector512<byte> _right = Vector512.Create((byte)19);

    [Benchmark]
    public Vector512<byte> Multiply() => _left * _right;
}
```

| Method   | Runtime   | Mean     | Ratio | Code Size |
| -------- | --------- | -------- | ----- | --------- |
| Multiply | .NET 10.0 | 3.752 ns | 1.00  | 114 B     |
| Multiply | .NET 11.0 | 2.174 ns | 0.58  | 73 B      |

The .NET 11 sequence no longer extracts, widens, narrows, and reinserts both 256-bit halves:

```
; x64
 vmovups     zmm0, [rcx+8]
-vmovaps     zmm1, zmm0
-vpmovzxbw   zmm1, ymm1
-vmovups     zmm2, [rcx+48]
-vmovaps     zmm3, zmm2
-vpmovzxbw   zmm3, ymm3
-vpmullw     zmm1, zmm3, zmm1
-vpmovwb     ymm1, zmm1
-vextracti32x8 ymm0, zmm0, 1
-vpmovzxbw   zmm0, ymm0
-vextracti32x8 ymm2, zmm2, 1
-vpmovzxbw   zmm2, ymm2
-vpmullw     zmm0, zmm2, zmm0
-vpmovwb     ymm0, zmm0
-vinserti32x8 zmm0, zmm1, ymm0, 1
+vmovups     zmm1, [rcx+48]
+vpmullw     zmm2, zmm0, zmm1
+vpsrlw      zmm0, zmm0, 8
+vpandd      zmm1, zmm1, dword bcst [RWD00]
+vpmullw     zmm0, zmm1, zmm0
+vpternlogd  zmm0, zmm2, dword bcst [RWD04], 0F8
 vmovups     [rdx], zmm0

; 114 bytes → 73 bytes
```

[dotnet/runtime#127094](https://github.com/dotnet/runtime/pull/127094) lets scalar conversions between `Half` and `float` use F16C’s `vcvtps2ph` and `vcvtph2ps` instructions when AVX2 is enabled:

```
// Run on x64 with AVX2 enabled:
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[DisassemblyDiagnoser, HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private Half _half = (Half)123.5f;
    private float _single = 123.5f;

    [Benchmark] public float HalfToSingle() => (float)_half;
    [Benchmark] public Half SingleToHalf() => (Half)_single;
}
```

| Method       | Runtime   | Mean     | Ratio | Code Size |
| ------------ | --------- | -------- | ----- | --------- |
| HalfToSingle | .NET 10.0 | 2.506 ns | 1.00  | 104 B     |
| HalfToSingle | .NET 11.0 | 1.380 ns | 0.55  | 14 B      |
|              |           |          |       |           |
| SingleToHalf | .NET 10.0 | 2.598 ns | 1.00  | 134 B     |
| SingleToHalf | .NET 11.0 | 1.351 ns | 0.52  | 19 B      |

Finally, [dotnet/runtime#127536](https://github.com/dotnet/runtime/pull/127536) from [@Ruihan-Yin](https://github.com/Ruihan-Yin) completes support for APX, Intel’s Advanced Performance Extensions. In addition to expanding the general-purpose register set, APX adds forms of many instructions that don’t overwrite the processor’s condition flags. That gives the register allocator and instruction scheduler more freedom to keep values and pending conditions alive at the same time. Its `CTEST` and `CFCMOV` instructions can also represent chained conditions without branches and replace some compare-with-zero forms with shorter encodings. Applications don’t need to call APX-specific APIs to benefit; when the hardware and operating system expose APX, the JIT is able to utilize the additional instructions automatically.

On Arm64, the work in .NET 11 spans both conventional code generation and continued support for SVE (Scalable Vector Extension). Unlike 128-bit AdvSimd vectors, an SVE vector doesn’t have one width fixed by the instruction set; each processor chooses a supported width, and the same compiled loop uses predicate masks to operate on however many elements fit. That makes SVE well suited to loops whose trip counts are not exact multiples of a particular vector size.

[dotnet/runtime#121986](https://github.com/dotnet/runtime/pull/121986) improves zeroing for larger stack allocations on Arm64. The JIT can store two zeroed 128-bit vector registers at a time, doubling the amount cleared by each instruction:

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;
using System.Runtime.CompilerServices;
using System.Runtime.Intrinsics.Arm;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    [Benchmark] public void Stackalloc512() => Consume(stackalloc byte[512]);
    [Benchmark] public void Stackalloc1024() => Consume(stackalloc byte[1024]);
    [Benchmark] public void Stackalloc16384() => Consume(stackalloc byte[16384]);

    [MethodImpl(MethodImplOptions.NoInlining)]
    private static void Consume(Span<byte> x) { }
}
```

| Method          | Runtime   | Mean       | Ratio |
| --------------- | --------- | ---------- | ----- |
| Stackalloc512   | .NET 10.0 | 13.65 ns   | 1.00  |
| Stackalloc512   | .NET 11.0 | 9.557 ns   | 0.70  |
| Stackalloc1024  | .NET 10.0 | 25.35 ns   | 1.00  |
| Stackalloc1024  | .NET 11.0 | 14.332 ns  | 0.57  |
| Stackalloc16384 | .NET 10.0 | 312.97 ns  | 1.00  |
| Stackalloc16384 | .NET 11.0 | 162.656 ns | 0.52  |

A wave of smaller Arm64 changes improves instruction selection. In .NET 11, [dotnet/runtime#119758](https://github.com/dotnet/runtime/pull/119758) from [@jonathandavies-arm](https://github.com/jonathandavies-arm) lets a comparison with zero consume condition flags set as a side effect of the preceding arithmetic or logical instruction, avoiding a separate `cmp`. [dotnet/runtime#123138](https://github.com/dotnet/runtime/pull/123138) from [@jonathandavies-arm](https://github.com/jonathandavies-arm) recognizes bit-extraction idioms such as `(value >> 6) & 0x3F` and maps them to the dedicated `ubfx` instruction. And [dotnet/runtime#123546](https://github.com/dotnet/runtime/pull/123546) from [@jonathandavies-arm](https://github.com/jonathandavies-arm) removes a non-overflowing `int`-to-`long` widening cast when the result is immediately truncated to a smaller integer type.

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[DisassemblyDiagnoser, HideColumns("Job", "Error", "StdDev", "Median", "RatioSD", "left", "right", "value")]
public class Benchmarks
{
    [Benchmark]
    [Arguments(-1, 2)]
    public bool CompareWithZero(int left, int right) => (left & right) <= 0;

    [Benchmark]
    [Arguments(0x7F65_4321)]
    public int ExtractBits(int value) => (value >> 6) & 0x3F;

    [Benchmark]
    [Arguments(0x1122_3344)]
    public sbyte TruncateAfterWidening(int value) => (sbyte)(long)value;
}
```

Each example removes one instruction. `CompareWithZero` changes `and` to its flag-setting `ands` form and drops the subsequent `cmp`; `ExtractBits` replaces a shift and mask with `ubfx`; and `TruncateAfterWidening` drops the `sxtw` that widened the value to 64 bits only for `sxtb` to immediately truncate it again:

```
; Arm64
; CompareWithZero: 28 bytes → 24 bytes
-            and     w0, w1, w2
-            cmp     w0, #0
+            ands    w0, w1, w2
             cset    x0, le

; ExtractBits: 24 bytes → 20 bytes
-            asr     w0, w1, #6
-            and     w0, w0, #63
+            ubfx    w0, w1, #6, #6

; TruncateAfterWidening: 24 bytes → 20 bytes
-            sxtw    x0, w1
-            sxtb    w0, w0
+            sxtb    w0, w1
```

Instruction selection also improves where values move between registers and memory. In .NET 11, [dotnet/runtime#126803](https://github.com/dotnet/runtime/pull/126803) changes `ToScalar` on a vector of 64-bit integers to use `fmov Xd, Dn` rather than the lane-extract instruction `umov`; in both cases lane zero moves to a general-purpose register, but `fmov` is the more direct form. For ReadyToRun code, [dotnet/runtime#129589](https://github.com/dotnet/runtime/pull/129589) folds relocatable indirection-cell loads from `adrp + add + ldr` into `adrp + ldr #:lo12:`, removing the separate address addition. And [dotnet/runtime#129932](https://github.com/dotnet/runtime/pull/129932) re-enables `ldp`/`stp` formation for negative unscaled offsets, letting two adjacent loads or stores become one paired instruction.

The first and third changes are easy to see with small methods:

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;
using System.Runtime.CompilerServices;
using System.Runtime.Intrinsics;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[DisassemblyDiagnoser, HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private Vector128<long> _vector = Vector128.Create(42L, 84L);
    private nint[] _storage = new nint[8];

    [Benchmark]
    public long ToScalar() => ToScalarCore(_vector);

    [Benchmark]
    public void ClearPrevious() => ClearPreviousCore(ref _storage[4]);

    [MethodImpl(MethodImplOptions.NoInlining)]
    private static long ToScalarCore(Vector128<long> value) => value.ToScalar();

    [MethodImpl(MethodImplOptions.NoInlining)]
    private static void ClearPreviousCore(ref nint value)
    {
        Unsafe.Add(ref value, -1) = 0;
        Unsafe.Add(ref value, -2) = 0;
        Unsafe.Add(ref value, -3) = 0;
        Unsafe.Add(ref value, -4) = 0;
    }
}
```

The `ToScalar` change is a direct instruction substitution, while the negative-offset stores collapse from four instructions to two, reducing the helper from 32 bytes to 24 bytes:

```
; Arm64
; ToScalarCore
-            umov    x0, v0.d[0]
+            fmov    x0, d0

; ClearPreviousCore
-            str     xzr, [x0, #-0x08]
-            str     xzr, [x0, #-0x10]
-            str     xzr, [x0, #-0x18]
-            str     xzr, [x0, #-0x20]
+            stp     xzr, xzr, [x0, #-0x10]
+            stp     xzr, xzr, [x0, #-0x20]
```

Bit-counting operations benefit as well. `PopCount` counts the one bits in a value, while `TrailingZeroCount` counts the zero bits below its least-significant one bit. [dotnet/runtime#128677](https://github.com/dotnet/runtime/pull/128677) imports both as dedicated Arm64 intrinsics, making their intent visible to later optimization. On processors with the FEAT\_CSSC extension, [dotnet/runtime#130332](https://github.com/dotnet/runtime/pull/130332) can then lower them directly to the scalar `cnt` and `ctz` instructions.

Comparison masks are another place where spelling out the intent enables much better code. Portable SIMD code often compares vectors, calls `ExtractMostSignificantBits`, and then asks whether any lane matched, counts matching lanes, or finds the first or last match. [dotnet/runtime#129688](https://github.com/dotnet/runtime/pull/129688) from [@jonathandavies-arm](https://github.com/jonathandavies-arm) recognizes those consumers on Arm64 and avoids materializing the full scalar mask: it can horizontally reduce the vector mask directly.

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;
using System.Numerics;
using System.Runtime.CompilerServices;
using System.Runtime.Intrinsics;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[DisassemblyDiagnoser, HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private Vector128<int> _value = Vector128.Create(1, -2, 3, -4);

    [Benchmark]
    public bool AnyLessThan() => AnyLessThanCore(_value, 0);

    [Benchmark]
    public int CountLessThan() => CountLessThanCore(_value, 0);

    [MethodImpl(MethodImplOptions.NoInlining)]
    private static bool AnyLessThanCore(Vector128<int> value, int limit) =>
        Vector128.LessThan(value, Vector128.Create(limit))
            .ExtractMostSignificantBits() != 0;

    [MethodImpl(MethodImplOptions.NoInlining)]
    private static int CountLessThanCore(Vector128<int> value, int limit) =>
        BitOperations.PopCount(
            Vector128.LessThan(value, Vector128.Create(limit))
                .ExtractMostSignificantBits());
}
```

In .NET 10, both helpers first pack the most-significant bit from every comparison lane into a scalar. .NET 11 instead keeps the comparison as a vector.

```
; Arm64
; AnyLessThanCore
             cmgt    v16.4s, v16.4s, v0.4s
-            movi    v17.4s, #0x80, LSL #24
-            and     v16.4s, v16.4s, v17.4s
-            ldr     q17, [@RWD00]
-            ushl    v16.4s, v16.4s, v17.4s
-            addv    s16, v16.4s
-            smov    x0, v16.s[0]
+            umaxv   s16, v16.4s
+            umov    w0, v16.s[0]
             cmp     w0, #0
             cset    x0, ne

; CountLessThanCore
             cmgt    v16.4s, v16.4s, v0.4s
-            movi    v17.4s, #0x80, LSL #24
-            and     v16.4s, v16.4s, v17.4s
-            ldr     q17, [@RWD00]
-            ushl    v16.4s, v16.4s, v17.4s
-            addv    s16, v16.4s
-            movi    v17.2s, #0
-            smov    x0, v16.s[0]
-            ins     v17.s[0], w0
-            cnt     v16.8b, v17.8b
-            addv    b16, v16.8b
-            umov    w0, v16.b[0]
+            ushr    v16.4s, v16.4s, #31
+            addv    s16, v16.4s
+            umov    w0, v16.s[0]
```

On the SVE and SVE2 side, [dotnet/runtime#129852](https://github.com/dotnet/runtime/pull/129852) from [@snickolls-arm](https://github.com/snickolls-arm) removes the old 128-bit size ceiling for `Vector<T>` on Arm64 and lets the runtime size the type from the process’s actual SVE vector length. (Scalable `Vector<T>` remains experimental and disabled by default in .NET 11, so this expands what the experimental mode can do; it doesn’t speed up the default `Vector<T>` configuration.)

The public intrinsic surface also grows. In .NET 11, [dotnet/runtime#118957](https://github.com/dotnet/runtime/pull/118957) from [@SwapnilGaikwad](https://github.com/SwapnilGaikwad) exposes odd-lane floating-point conversions; “odd lane” here means converting elements 1, 3, 5, and so on, which is useful when widening or narrowing interleaved data. [dotnet/runtime#123890](https://github.com/dotnet/runtime/pull/123890) from [@ylpoonlg](https://github.com/ylpoonlg) and [dotnet/runtime#123892](https://github.com/dotnet/runtime/pull/123892) from [@ylpoonlg](https://github.com/ylpoonlg) add non-temporal gather loads and scatter stores, which read from or write to multiple non-contiguous addresses (the “gather” part) while hinting that the data need not remain in cache (the “non-temporal” part).

Other changes improve the predicates that make scalable loops work. [dotnet/runtime#127538](https://github.com/dotnet/runtime/pull/127538) adds hardware-generated predicate masks for more loop and memory-access patterns, while [dotnet/runtime#126398](https://github.com/dotnet/runtime/pull/126398) from [@ylpoonlg](https://github.com/ylpoonlg) reduces setup moves for masked operations. And [dotnet/runtime#128326](https://github.com/dotnet/runtime/pull/128326) from [@snickolls-arm](https://github.com/snickolls-arm) improves how SVE masks flow through the JIT, allowing zeroing forms of instructions to replace separate constant setup. [dotnet/runtime#127520](https://github.com/dotnet/runtime/pull/127520) from [@a74nh](https://github.com/a74nh) enables scalable vector and mask constants, and [dotnet/runtime#128148](https://github.com/dotnet/runtime/pull/128148) from [@snickolls-arm](https://github.com/snickolls-arm) uses vector stores to initialize scalable vector locals, replacing scalar loops.

### Register Allocation

Generated code constantly moves values between the CPU’s limited set of fast registers and temporary stack slots. Register allocation in a compiler decides which values stay in registers and which are “spilled” to the stack; avoiding one spill can remove both the store and the later reload.

Some small structs are passed with multiple fields packed into one register. In .NET 11, [dotnet/runtime#112740](https://github.com/dotnet/runtime/pull/112740) lets the JIT extract those fields directly, avoiding a “spill” to a temporary stack slot followed by a reload of each field:

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;
using System.Drawing;
using System.Runtime.CompilerServices;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private readonly Memory<int>[] _memories = CreateMemories();

    private static Memory<int>[] CreateMemories()
    {
        Random rng = new(42);
        var memories = new Memory<int>[4096];
        for (int i = 0; i < memories.Length; i++)
            memories[i] = new int[rng.Next(0, 20)];

        return memories;
    }

    [MethodImpl(MethodImplOptions.NoInlining)]
    private static bool Test(Memory<int> mem) => mem.Length > 10;

    [Benchmark]
    public int MemoryLengthExtract_Loop()
    {
        int count = 0;
        for (int i = 0; i < _memories.Length; i++)
            if (Test(_memories[i]))
                count++;

        return count;
    }
}
```

The measured row uses `Memory<int>` because its length arrives packed into part of an argument register on Arm64. The new extraction avoids a stack round-trip on every call.

| Method                    | Runtime   | Mean     | Ratio |
| ------------------------- | --------- | -------- | ----- |
| MemoryLengthExtract\_Loop | .NET 10.0 | 27.15 μs | 1.00  |
| MemoryLengthExtract\_Loop | .NET 11.0 | 23.95 μs | 0.88  |

Two broader register-allocation changes reduce unnecessary copies and spills: [dotnet/runtime#125214](https://github.com/dotnet/runtime/pull/125214) handles more conflicts directly, while [dotnet/runtime#125219](https://github.com/dotnet/runtime/pull/125219) steers short-lived values away from registers an upcoming operation will overwrite. [dotnet/runtime#126552](https://github.com/dotnet/runtime/pull/126552) from [@SingleAccretion](https://github.com/SingleAccretion) removes an old restriction on method prologs, eliminating jumps that existed only to satisfy that encoding rule.

### Write Barriers and Garbage Collection

The .NET garbage collector is generational: new objects start in gen0, while objects that survive collections are promoted to gen1 and gen2. That enables the GC to collect younger generations without having to scan the whole heap. Of course, a reference to a younger object could get written to a field of an older one, in which case only scanning the younger generation would lead to problems. To ensure such references aren’t missed, whenever a write could create one, the JIT emits a small piece of code to update the GC’s bookkeeping; that code is known as a GC write barrier. Reference writes happen a lot, so it’s really important for performance that those barriers be as cheap as possible, and elided if they’re provably not needed at all.

Managed reference stores may require both an array covariance check and a GC write barrier. Arrays in .NET are covariant, meaning a `TDerived[]` can be used as a `TBase[]`, e.g. a `string[]` can be used as an `object[]`; consequently, storing an instance into an `object[]` must validate that the instance is actually of the right type (otherwise, you could have a `TDerived1[]` masquerading as a `TBase[]` and try to store a `TDerived2` into it, which would cause badness if it were to store successfully). [dotnet/runtime#126547](https://github.com/dotnet/runtime/pull/126547) expands calls to the runtime’s array-store helper into the individual operations it performs, exposing both the covariance check and write barrier to the JIT. When the JIT knows the array’s exact type, it can then eliminate the covariance check and optimize the barrier:

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;
using System.Runtime.CompilerServices;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private readonly object[] _array = new object[4096];
    private object _value = new();

    [MethodImpl(MethodImplOptions.NoInlining)]
    private static void StoreAll(object[] arr, object value)
    {
        for (int i = 0; i < arr.Length; i++)
            arr[i] = value;
    }

    [Benchmark]
    public object[] CovariantStore_Loop()
    {
        StoreAll(_array, _value);
        return _array;
    }
}
```

| Method               | Runtime   | Mean     | Ratio |
| -------------------- | --------- | -------- | ----- |
| CovariantStore\_Loop | .NET 10.0 | 10.85 μs | 1.00  |
| CovariantStore\_Loop | .NET 11.0 | 6.042 μs | 0.56  |

Sometimes writes are done one at a time, but sometimes they can be batched, as happens when copying structs. [dotnet/runtime#128238](https://github.com/dotnet/runtime/pull/128238) extends the JIT’s heap-destination analysis from individual stores to whole-struct copies. [dotnet/runtime#128542](https://github.com/dotnet/runtime/pull/128542) then replaces a specialized helper that copied one reference field at a time with reference stores and vector stores for the non-reference data. Together, they let the JIT choose more efficient write barriers and copy the rest of a mixed struct with SIMD.

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;
using System.Runtime.CompilerServices;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    [InlineArray(4)]
    public struct InlineArray4Long
    {
        private long _element0;
    }

    public struct MyStruct
    {
        public string A;
        public InlineArray4Long G;
        public string B;
    }

    private MyStruct _src;
    private MyStruct _dst;

    [GlobalSetup]
    public void Setup()
    {
        _src = new MyStruct { A = "hello", B = "world" };
        _src.G[0] = 1;
        _src.G[1] = 2;
        _src.G[2] = 3;
        _src.G[3] = 4;
    }

    [Benchmark]
    public void HeapStructCopy() => _dst = _src;
}
```

| Method         | Runtime   | Mean     | Ratio |
| -------------- | --------- | -------- | ----- |
| HeapStructCopy | .NET 10.0 | 4.132 ns | 1.00  |
| HeapStructCopy | .NET 11.0 | 3.071 ns | 0.74  |

[dotnet/runtime#130535](https://github.com/dotnet/runtime/pull/130535) handles the equivalent case for small structs that don’t contain object references. Once the JIT has turned the copy into several writes to adjacent fields, it can combine them into fewer, wider writes.

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;
using System.Runtime.CompilerServices;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[DisassemblyDiagnoser, HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private Int128 _value;

    [Benchmark]
    public void StoreInt128() => _value = 123456789;
}
```

.NET 10 stores the low and high halves separately. .NET 11 loads the value into a vector register and writes all 16 bytes at once.

```
; x64
; StoreInt128
-       mov      qword ptr [rcx+8], 75BCD15
-       xor      eax, eax
-       mov      [rcx+10], rax
+       vmovss   xmm0, dword ptr [RWD00]
+       vmovups  [rcx+8], xmm0

-; Total bytes of code 15
+; Total bytes of code 14
```

The same idea applies when the source code assigns neighboring fields individually. [dotnet/runtime#126562](https://github.com/dotnet/runtime/pull/126562) enables this for promoted struct locals, while [dotnet/runtime#130107](https://github.com/dotnet/runtime/pull/130107) extends it to adjacent fields at constant static addresses:

```
// dotnet run -c Release -f net11.0 --filter "*"

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;
using System.Runtime.CompilerServices;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[DisassemblyDiagnoser, HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private static Point s_point;

    [Benchmark]
    public void SetPoint() => Set();

    [MethodImpl(MethodImplOptions.NoInlining | MethodImplOptions.AggressiveOptimization)]
    private static void Set()
    {
        s_point.X = 1;
        s_point.Y = 2;
    }

    private struct Point
    {
        public int X;
        public int Y;
    }
}
```

The referenced .NET 11 x64 build combines the two 32-bit constants and writes both fields with one 64-bit store:

```
; x64
mov     rax, 200000001
mov     rcx, <address of s_point>
mov     [rcx], rax
```

[dotnet/runtime#127487](https://github.com/dotnet/runtime/pull/127487) applies a related improvement when stack protection requires a struct parameter to be copied. It uses consistently sized writes so a subsequent wider read doesn’t need to wait for the processor to reconcile overlapping stores.

Write barriers are only one part of the interaction between generated code and the garbage collector. During a compacting collection, the GC needs to plan where surviving objects will move and then update references to them. To do that efficiently, it records their addresses, sorts those addresses, and groups adjacent survivors into regions called “plugs.” With enough live objects, sorting these mark lists becomes a meaningful part of the collection. Recent x86/x64 runtimes use a vectorized `vxsort` implementation for sufficiently large lists. In .NET 11, [dotnet/runtime#110692](https://github.com/dotnet/runtime/pull/110692) from [@a74nh](https://github.com/a74nh) extends that support to Arm64.

The generation assigned to GC metadata matters just as much as the speed of one collection. .NET’s generational GC is based on the observation that most objects die young: generation 0 and generation 1 collections, collectively called ephemeral collections, run frequently and should avoid revisiting state that has already survived into generation 2. A dependent handle associates a primary object with a secondary object, keeping the secondary alive while the primary remains reachable; `ConditionalWeakTable<TKey, TValue>` is built on this mechanism. Previously, the handle itself didn’t age with its referents, so every ephemeral collection continued scanning it even after both objects had become long-lived. [dotnet/runtime#78746](https://github.com/dotnet/runtime/pull/78746) ages dependent handles accordingly and moves a handle back to a younger generation when necessary. Old handles can therefore be skipped by young collections without compromising reachability.

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using System.Runtime.CompilerServices;
using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private ConditionalWeakTable<object, object> _table = new();
    private object[] _keys = [];

    [Params(100_000, 1_000_000)]
    public int Handles { get; set; }

    [GlobalSetup]
    public void Setup()
    {
        _table = new();
        _keys = new object[Handles];

        for (int i = 0; i < _keys.Length; i++)
        {
            object key = new();
            _keys[i] = key;
            _table.Add(key, new object());
        }

        GC.Collect(2, GCCollectionMode.Forced, blocking: true, compacting: true);
    }

    [Benchmark]
    public void CollectGen0() =>
        GC.Collect(0, GCCollectionMode.Forced, blocking: true, compacting: false);
}
```

| Method      | Runtime   | Handles | Mean      | Ratio |
| ----------- | --------- | ------- | --------- | ----- |
| CollectGen0 | .NET 10.0 | 100000  | 1.522 ms  | 1.00  |
| CollectGen0 | .NET 11.0 | 100000  | 255.5 μs  | 0.17  |
| CollectGen0 | .NET 10.0 | 1000000 | 10.737 ms | 1.00  |
| CollectGen0 | .NET 11.0 | 1000000 | 310.7 μs  | 0.029 |

### Runtime Knowledge and Frozen Data

The JIT can optimize only the facts it knows. Some facts come from its own analysis; others are contracts supplied by the runtime, such as which helpers have side effects, the length of a newly allocated string, or whether a data object will ever move.

A generic virtual call such as `baseReference.Foo<string>()` may need help from the runtime to find the implementation for both the object’s actual type and the generic argument. If that lookup appears to have arbitrary side effects, the JIT has to perform it exactly where it occurs, rather than possibly resulting on a cached answer from a previous lookup. In .NET 11, [dotnet/runtime#122017](https://github.com/dotnet/runtime/pull/122017) teaches the JIT more precisely which exceptions these runtime helpers can throw and whether they otherwise have side effects. The JIT can then share repeated lookups or move an unchanging lookup out of a loop:

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;
using System.Runtime.CompilerServices;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    public abstract class Base
    {
        public abstract void Foo<T>();
    }

    public class Derived : Base
    {
        public override void Foo<T>() { }
    }

    private Base _b = new Derived();

    [Benchmark]
    public void GvmCseHoist()
    {
        Base b = _b;
        b.Foo<string>();
        b.Foo<int>();
        b.Foo<string>();
        b.Foo<int>();

        for (int i = 0; i < 10; i++)
            b.Foo<double>();
    }
}
```

In .NET 11, the repeated lookups outside the loop are shared and the loop’s lookup is performed once, not ten times.

| Method      | Runtime   | Mean     | Ratio |
| ----------- | --------- | -------- | ----- |
| GvmCseHoist | .NET 10.0 | 45.75 ns | 1.00  |
| GvmCseHoist | .NET 11.0 | 24.06 ns | 0.53  |

Profile data is another way the JIT learns what matters. Inlining could previously hide important work from the instrumentation used to gather that data. [dotnet/runtime#119658](https://github.com/dotnet/runtime/pull/119658) allows the inlined code to be instrumented as well, giving later PGO-driven compilation a more complete picture of the hot paths.

### JIT Throughput and Cleanup

The quality of the generated code isn’t the only concern; the time spent producing it matters too. Every analysis the JIT performs has a cost. [dotnet/runtime#123856](https://github.com/dotnet/runtime/pull/123856) removes checks and maps from Global Assertion Propagation whose bookkeeping wasn’t paying for itself. This is the recurring balancing act in the development of the JIT: retaining the information that enables meaningful optimizations while avoiding analysis overhead whose code-quality benefit is negligible.

[dotnet/runtime#127363](https://github.com/dotnet/runtime/pull/127363) makes profile-guided optimization more resilient with OSR (on-stack replacement), which replaces a method while one of its loops is already running. Because that execution begins in the middle of the method rather than at its normal entry, reconstructed profile data doesn’t always line up perfectly with the paths actually available. The JIT now estimates the likelihood of those paths rather than asserting or abandoning the profile.

Optimizations can leave behind code that’s no longer reachable, so the JIT also needs to be good at dead code removal. [dotnet/runtime#126223](https://github.com/dotnet/runtime/pull/126223) runs another sweep whenever the method’s branching structure changes, catching blocks made obsolete by earlier transformations.

And [dotnet/runtime#128515](https://github.com/dotnet/runtime/pull/128515) from [@BoyBaykiller](https://github.com/BoyBaykiller) repeatedly combines equivalent return and throw endings, removing duplicate exit paths and sometimes exposing more code that can be shared.

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[DisassemblyDiagnoser, HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    [Benchmark]
    [Arguments((byte)9)]
    public bool IsLinearWhiteSpace(byte value) =>
        value <= 32 &&
        (value == 32 || value == 10 || value == 13 || value == 9);
}
```

In .NET 10, tail merging combines the paths that return `false`, but not both paths that return `true`. As a result, the JIT’s bit test covers three of the four values, with a separate comparison for `9`. In .NET 11, the true returns are merged as well, enabling all four values to be handled by the same bit test:

```
; x64
-       movzx    ecx, dl
-       cmp      ecx, 20
-       jg       M00_L02
-       cmp      ecx, 20
-       ja       M00_L01
-       mov      eax, 0FFFFDBFF
-       bt       rax, rcx
-       jae      M00_L00
-       mov      eax, 1
-       ret
-M00_L00:
-       cmp      ecx, 9
-       sete     al
-       movzx    eax, al
-       ret
-M00_L01:
+       movzx    eax, dl
+       cmp      eax, 20
+       jg       M00_L00
+       cmp      eax, 20
+       ja       M00_L00
+       mov      ecx, 0FFFFD9FF
+       bt       rcx, rax
+       jb       M00_L00
+       mov      eax, 1
+       ret
+M00_L00:
        xor      eax, eax
        ret

-; Total bytes of code 43
+; Total bytes of code 33
```

Also related to dead code, a call that never returns, such as one that always throws, makes everything after it unreachable. In .NET 11, after inlining, [dotnet/runtime#128513](https://github.com/dotnet/runtime/pull/128513) removes the remaining statements and outgoing paths from such a block and marks it as ending in a throw, exposing the dead code early enough for the cleanup passes above to remove it.

## Startup and Deployment

Before managed `Main` can run, the native host needs to locate the application’s dependencies, CoreCLR needs to load enough types and code to begin execution, and various pieces of framework infrastructure need to initialize themselves. Work removed from any of those stages helps the application get going sooner, improving startup time.

The host starts by reading the application’s `.deps.json`, turning its entries into paths, and building the trusted platform assembly (TPA) list. That list tells CoreCLR which framework and application assemblies it can resolve by simple name. Several costs in this process scaled with the number of assets rather than with the amount of useful work. [dotnet/runtime#123568](https://github.com/dotnet/runtime/pull/123568) in .NET 11 avoids checking every asset against a servicing directory unless the resolver is actually probing that directory. [dotnet/runtime#123919](https://github.com/dotnet/runtime/pull/123919) avoids repeatedly comparing the servicing-directory name and copying every dependency asset while constructing the TPA list, avoiding a lot of allocation. [dotnet/runtime#125251](https://github.com/dotnet/runtime/pull/125251) removes more allocation by normalizing each asset’s directory separators once when parsing the `.deps.json`, rather than normalizing the path again every time it is used.

Once the host hands off to CoreCLR, ReadyToRun (R2R) code helps avoid compiling methods before they can execute. However, initializing `Comparer<T>.Default` and `EqualityComparer<T>.Default` called a reflection-based helper whose resulting concrete comparer type wasn’t known when the R2R image was built. The comparer constructor and operations could consequently fall back to being interpreted. In .NET 11, [dotnet/runtime#126204](https://github.com/dotnet/runtime/pull/126204) uses specialized helpers that R2R can compile ahead of time and ensures the required comparer types are included in the image.

Even better than making initialization faster is avoiding it altogether. An `EventSource` normally discovers its event metadata and computes its provider GUID when it is initialized. [dotnet/runtime#121180](https://github.com/dotnet/runtime/pull/121180) adds an internal source generator that performs this work when the framework is built and emits the result for its `EventSource` implementations, including the ones for core runtime tracing. Applications then don’t need to pay the reflection and setup costs when those event sources are first used.

Startup also has a memory footprint outside the managed heap. Native AOT’s `AllocHeap` typically holds only small amounts of runtime metadata. On Windows, however, its virtual-memory allocator reserved a 64 KB region for each block even when it initially needed only 4 KB. In .NET 11, [dotnet/runtime#122822](https://github.com/dotnet/runtime/pull/122822) instead uses ordinary `new` and `delete` for these small blocks, matching the allocation strategy to the amount of memory normally involved.

Note that the aforementioned R2R work wasn’t motivated only by desktop and server startup. It was also part of the substantial effort to make CoreCLR the runtime for .NET on mobile. Starting with .NET 11, [.NET MAUI moved to CoreCLR](https://devblogs.microsoft.com/dotnet/dotnet-maui-moves-to-coreclr-in-dotnet-11/) for Android, iOS, and Mac Catalyst, the last .NET MAUI platforms that had still been using Mono. This is much more than swapping one execution engine for another. Those apps now use the same runtime as ASP.NET Core, cloud services, and desktop .NET, with the same JIT, garbage collector, diagnostics infrastructure, performance improvements, and bug fixes. It also brings CoreCLR’s tiered compilation, ReadyToRun, and profile-guided optimization to mobile, while providing a common foundation for NativeAOT. That combination is important: R2R and packaged profiles can precompile the code most important to startup, while the optimizing JIT can produce higher-quality code for hot methods on platforms where dynamic compilation is available. Improvements like the comparer specialization mentioned earlier keep more code on the compiled path instead of falling back to interpretation.

## Threading

Threading is a cross-cutting concern that impacts almost every application and service. Whether code is protecting shared state, queueing work, or coordinating asynchronous operations, small costs in the underlying machinery can quickly add up. As such, it’s something that’s revisited in every release of .NET.

`Monitor` is the synchronization primitive historically used to implement `lock`, providing the most pervasively used support for mutual exclusion. It also supports sending signals, such that one thread can wait on a `Monitor` with `Monitor.Wait` for another thread to `Pulse` it. The internal object that tracks these waiters is a “condition variable.” [dotnet/runtime#129083](https://github.com/dotnet/runtime/pull/129083) stores that condition directly on the lock, removing a separate `ConditionalWeakTable` lookup from this already synchronization-heavy path.

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private const int RoundTripsPerInvoke = 2_000;

    private readonly object _gate = new();
    private int _ping;
    private int _pong;
    private bool _stop;
    private Thread _responder = null!;

    [GlobalSetup]
    public void Setup()
    {
        _responder = new Thread(ResponderLoop) { IsBackground = true };
        _responder.Start();
    }

    [GlobalCleanup]
    public void Cleanup()
    {
        lock (_gate)
        {
            _stop = true;
            Monitor.PulseAll(_gate);
        }

        _responder.Join();
    }

    private void ResponderLoop()
    {
        lock (_gate)
        {
            int seen = 0;
            while (true)
            {
                while (_ping == seen && !_stop)
                    Monitor.Wait(_gate);

                if (_stop)
                    return;

                seen = _ping;
                _pong = seen;
                Monitor.PulseAll(_gate);
            }
        }
    }

    [Benchmark(OperationsPerInvoke = RoundTripsPerInvoke)]
    public int PingPong_MonitorWaitPulse()
    {
        lock (_gate)
        {
            for (int i = 0; i < RoundTripsPerInvoke; i++)
            {
                _ping++;
                int expected = _ping;
                Monitor.PulseAll(_gate);
                while (_pong != expected)
                    Monitor.Wait(_gate);
            }

            return _pong;
        }
    }
}
```

| Method                     | Runtime   | Mean     | Ratio |
| -------------------------- | --------- | -------- | ----- |
| PingPong\_MonitorWaitPulse | .NET 10.0 | 4.194 μs | 1.00  |
| PingPong\_MonitorWaitPulse | .NET 11.0 | 3.517 μs | 0.84  |

In the case of `Monitor`, that improvement targeted the specific shared implementation. In other cases, the costs are spread out in a more peanut butter manner across lots of code. [dotnet/runtime#125274](https://github.com/dotnet/runtime/pull/125274) removes some of that peanut butter by removing unnecessary `volatile` annotations from a wide range of library fields whose correctness already comes from locks, `Interlocked`, or one-time initialization. On x86/x64 hardware, which already provides a strong memory model, those annotations generally don’t result in extra instructions, though they can still constrain compiler optimizations. Arm, however, permits more reordering, so the JIT often needs to emit memory fences to provide `volatile`‘s guarantees. Removing the annotations where they’re redundant therefore can end up removing unnecessary fences from Arm’s generated code.

Similar considerations apply to code in the runtime. [dotnet/runtime#125259](https://github.com/dotnet/runtime/pull/125259) replaces full memory barriers in the runtime’s `HashMap` with the narrower acquire and release operations actually required. On top of that, many VM hash tables, including its `EEHashTable`, are read constantly but updated only occasionally. [dotnet/runtime#124822](https://github.com/dotnet/runtime/pull/124822) adds epoch-based reclamation, enabling readers to avoid entering cooperative GC mode simply to keep an old set of buckets alive. And [dotnet/runtime#129640](https://github.com/dotnet/runtime/pull/129640) replaces the previous byte-at-a-time hash used by these tables with an xxHash implementation that consumes four bytes at a time.

Along the same lines, in .NET 11 [dotnet/runtime#122726](https://github.com/dotnet/runtime/pull/122726) reduces the scheduling overhead around small thread-pool work items. It removes unnecessary memory fences and shared-state updates, checks in with the thread-pool controller once per batch rather than once per item, spends less time spinning on a semaphore, and requests another worker only when the queued work shows one is needed. The result is less coordination overhead and fewer workers woken just as the queue becomes empty.

Earlier in this post, we talked about runtime async, which can have a significant impact on the performance of `async`/`await` code, how they produce `Task`s, and so on. They’re not the only improvements in .NET 11 related to `Task`s, though.

One fun one is a new analyzer, CA2027, introduced in [dotnet/sdk#51452](https://github.com/dotnet/sdk/pull/51452). With that, the SDK can point out problematic usage of `Task.Delay` that I’ve seen on multiple occasions to lead to non-trivial performance issues in large scale services. Consider this code:

```
Task someTask = ...;
if (await Task.WhenAny(someTask, Task.Delay(timeout)) != someTask) // oops!
{
    throw new TimeoutException();
}
```

The developer that wrote this is obviously trying to implement a timeout. The problem, however, is that this leaks. In the hopefully common case where `someTask` completes really quickly, the `Task.Delay` will still be pending. That `Delay` has associated with it a `System.Threading.Timer` that’s consuming valuable resources, as well as other data in memory, and if this `timeout` is long and this code is on a hotter path, we could accumulate thousands upon thousands of those timers. That in turn can increase memory use and slow down other calls that interact with timers.

The fix is to instead use the `Task.WaitAsync` method, introduced all the way back in .NET 6. It provides a much more efficient mechanism for doing this same kind of timed waiting, and it correctly handles all the relevant cleanup. CA2027 will detect common forms of this issue and recommend the replacement.

## Numerics

`BigInteger` is one of those types that many applications may never need, but for those that do, there’s often no practical substitute. It powers workloads ranging from cryptography and number theory to compilers and applications that need to parse, format, or compute with integers larger than the fixed-width primitives can hold. Despite that need, however, `BigInteger` hasn’t received the same steady stream of performance investment as many of .NET’s other core types. Thankfully, in .NET 11 it gets a makeover.

[dotnet/runtime#125799](https://github.com/dotnet/runtime/pull/125799) rewrote significant portions of `BigInteger`‘s implementation, changing its limbs (the fixed-size pieces stored in its backing array) from `uint` to `nuint` (`UIntPtr`). That makes no effective difference on a 32-bit machine. On a 64-bit machine, however, each limb grows from 32 to 64 bits; since most arithmetic on a 64-bit value on a 64-bit platform costs no more than the corresponding 32-bit operation, each step can therefore process twice as many bits in the same number of cycles. The implementation also improves the algorithms around those wider limbs, including Montgomery multiplication and sliding-window exponentiation in `ModPow`, fused bitwise steps, additional hardware intrinsics, loop unrolling, and caching. That all builds on top of other optimizations that were done previously in the release, such as faster conversion of huge values to decimal text in [dotnet/runtime#112178](https://github.com/dotnet/runtime/pull/112178) from [@kzrnm](https://github.com/kzrnm), [dotnet/runtime#112876](https://github.com/dotnet/runtime/pull/112876) from [@kzrnm](https://github.com/kzrnm) using Toom-Cook multiplication for sufficiently large operands, and improved shifts and rotations thanks to [dotnet/runtime#113005](https://github.com/dotnet/runtime/pull/113005) from [@kzrnm](https://github.com/kzrnm). Toom-Cook splits each operand into several chunks and combines smaller products, doing less work than the straightforward every-limb-by-every-limb algorithm once the operands are large enough.

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;
using System.Numerics;
using System.Globalization;
using System.Text;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    [Params(64, 512)]
    public int Limbs;

    private BigInteger _a;
    private BigInteger _b;
    private BigInteger _shiftSubject;
    private BigInteger _hugeValueForToString;
    private string _decimalDigits100000 = "";
    private byte[] _utf8Digits1000 = [];
    private byte[] _utf8FormatBuffer = new byte[120_000];

    private BigInteger _divideDividendBelowThreshold;
    private BigInteger _divideDivisorBelowThreshold;
    private BigInteger _divideDividendAboveThreshold;
    private BigInteger _divideDivisorAboveThreshold;

    [GlobalSetup]
    public void Setup()
    {
        _a = MakeDeterministicBigInteger(Limbs, seed: 1);
        _b = MakeDeterministicBigInteger(Limbs, seed: 2);
        _shiftSubject = MakeDeterministicBigInteger(Limbs, seed: 3);

        _decimalDigits100000 = MakeDeterministicDecimalDigits(100_000);
        _hugeValueForToString = BigInteger.Parse(_decimalDigits100000, CultureInfo.InvariantCulture);

        string decimalDigits1000 = MakeDeterministicDecimalDigits(1_000);
        _utf8Digits1000 = Encoding.UTF8.GetBytes(decimalDigits1000);

        _divideDivisorBelowThreshold = MakeDeterministicBigInteger(16, seed: 4);
        _divideDividendBelowThreshold = MakeDeterministicBigInteger(16 + 96, seed: 5);

        _divideDivisorAboveThreshold = MakeDeterministicBigInteger(128, seed: 6);
        _divideDividendAboveThreshold = MakeDeterministicBigInteger(128 + 96, seed: 7);
    }

    private static BigInteger MakeDeterministicBigInteger(int limbCount, int seed)
    {
        Random rng = new(seed);
        byte[] bytes = new byte[(limbCount * 4) + 1]; // trailing 0 byte keeps the value positive
        rng.NextBytes(bytes);
        bytes[^1] = 0;
        return new BigInteger(bytes);
    }

    private static string MakeDeterministicDecimalDigits(int digitCount)
    {
        StringBuilder sb = new(digitCount);
        sb.Append('9'); // avoid a leading zero, which would shorten the effective digit count
        Random rng = new(42);
        for (int i = 1; i < digitCount; i++)
            sb.Append((char)('0' + rng.Next(0, 10)));

        return sb.ToString();
    }

    [Benchmark]
    public BigInteger Divide_BelowBurnikelZieglerThreshold() => _divideDividendBelowThreshold / _divideDivisorBelowThreshold;

    [Benchmark]
    public BigInteger Divide_AboveBurnikelZieglerThreshold() => _divideDividendAboveThreshold / _divideDivisorAboveThreshold;

    [Benchmark]
    public BigInteger Multiply() => _a * _b;

    [Benchmark]
    public BigInteger ShiftLeft() => _shiftSubject << 12345;

    [Benchmark]
    public BigInteger ParseLargeDecimal() => BigInteger.Parse(_decimalDigits100000, CultureInfo.InvariantCulture);

    [Benchmark]
    public string ToStringLargeDecimal() => _hugeValueForToString.ToString(CultureInfo.InvariantCulture);
}
```

| Method                                | Runtime   | Limbs | Mean             | Ratio |
| ------------------------------------- | --------- | ----- | ---------------- | ----- |
| Divide\_BelowBurnikelZieglerThreshold | .NET 10.0 | 64    | 2,954.3 ns       | 1.00  |
| Divide\_BelowBurnikelZieglerThreshold | .NET 11.0 | 64    | 1,493.8 ns       | 0.51  |
| Divide\_AboveBurnikelZieglerThreshold | .NET 10.0 | 64    | 10,766.7 ns      | 1.00  |
| Divide\_AboveBurnikelZieglerThreshold | .NET 11.0 | 64    | 6,303.4 ns       | 0.59  |
| Multiply                              | .NET 10.0 | 64    | 2,359.5 ns       | 1.00  |
| Multiply                              | .NET 11.0 | 64    | 1,328.2 ns       | 0.56  |
| ShiftLeft                             | .NET 10.0 | 64    | 217.1 ns         | 1.00  |
| ShiftLeft                             | .NET 11.0 | 64    | 121.6 ns         | 0.56  |
| ParseLargeDecimal                     | .NET 10.0 | 64    | 9,111,926.1 ns   | 1.00  |
| ParseLargeDecimal                     | .NET 11.0 | 64    | 3,899,478.0 ns   | 0.43  |
| ToStringLargeDecimal                  | .NET 10.0 | 64    | 135,894,135.4 ns | 1.00  |
| ToStringLargeDecimal                  | .NET 11.0 | 64    | 7,868,359.3 ns   | 0.058 |
| Divide\_BelowBurnikelZieglerThreshold | .NET 10.0 | 512   | 2,894.6 ns       | 1.00  |
| Divide\_BelowBurnikelZieglerThreshold | .NET 11.0 | 512   | 1,482.0 ns       | 0.51  |
| Divide\_AboveBurnikelZieglerThreshold | .NET 10.0 | 512   | 10,751.7 ns      | 1.00  |
| Divide\_AboveBurnikelZieglerThreshold | .NET 11.0 | 512   | 6,317.8 ns       | 0.59  |
| Multiply                              | .NET 10.0 | 512   | 68,063.6 ns      | 1.00  |
| Multiply                              | .NET 11.0 | 512   | 35,443.5 ns      | 0.52  |
| ShiftLeft                             | .NET 10.0 | 512   | 673.0 ns         | 1.00  |
| ShiftLeft                             | .NET 11.0 | 512   | 309.4 ns         | 0.46  |
| ParseLargeDecimal                     | .NET 10.0 | 512   | 9,149,793.0 ns   | 1.00  |
| ParseLargeDecimal                     | .NET 11.0 | 512   | 3,891,313.0 ns   | 0.43  |
| ToStringLargeDecimal                  | .NET 10.0 | 512   | 135,829,594.6 ns | 1.00  |
| ToStringLargeDecimal                  | .NET 11.0 | 512   | 7,880,631.1 ns   | 0.058 |

In addition to internal changes, `BigInteger` also gained new public APIs that avoid transcoding. Protocols and storage formats increasingly expose text as UTF-8 bytes, but the previous parsing and formatting APIs required UTF-16 characters. Callers therefore had to decode the input into a temporary string before parsing, or format into characters and encode the result back to bytes. [dotnet/runtime#117745](https://github.com/dotnet/runtime/pull/117745) adds direct UTF-8 parsing and formatting to both `BigInteger` and `Complex`, sharing the generic numeric machinery used for UTF-16 and letting those consumers operate on their original representation.

[dotnet/runtime#130721](https://github.com/dotnet/runtime/pull/130721) improves a different `BigInteger` boundary: casting to `double` and `float`. The general conversion needs to inspect the arbitrary-width magnitude, locate its highest set bits, and perform the rounding required by the target floating-point format. But many `BigInteger` instances are much smaller than that machinery is designed for… the implementation now recognizes values that fit in 64 bits and routes them through the hardware’s native integer conversion support.

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;
using System.Numerics;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private readonly BigInteger _small = (BigInteger.One << 63) + 123;
    private readonly BigInteger _large = (BigInteger.One << 1023) + (BigInteger.One << 511) + 123;

    [Benchmark] public double SmallToDouble() => (double)_small;
    [Benchmark] public float SmallToSingle() => (float)_small;
    [Benchmark] public double LargeToDouble() => (double)_large;
    [Benchmark] public float LargeToSingle() => (float)_large;
}
```

| Method        | Runtime   | Mean     | Ratio |
| ------------- | --------- | -------- | ----- |
| SmallToDouble | .NET 10.0 | 2.873 ns | 1.00  |
| SmallToDouble | .NET 11.0 | 1.764 ns | 0.61  |
| SmallToSingle | .NET 10.0 | 3.548 ns | 1.00  |
| SmallToSingle | .NET 11.0 | 1.764 ns | 0.50  |
| LargeToDouble | .NET 10.0 | 2.863 ns | 1.00  |
| LargeToDouble | .NET 11.0 | 2.797 ns | 0.98  |
| LargeToSingle | .NET 10.0 | 3.559 ns | 1.00  |
| LargeToSingle | .NET 11.0 | 2.849 ns | 0.80  |

The same limb-widening advantages given to `BigInteger` in .NET 11 were also extended to the core floating-point types. Parsing a very long decimal input and formatting a floating-point value with many requested digits both need temporary arbitrary-precision arithmetic once the value no longer fits in the normal mantissa. .NET uses a separate internal `Number.BigInteger` for that work. [dotnet/runtime#132577](https://github.com/dotnet/runtime/pull/132577) applies the same native-width limb representation to that type, reducing the amount of per-limb work in floating-point parsing, formatting, and rounding.

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using System.Globalization;
using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private readonly string _longFraction = "0." + new string('1', 768);

    [Benchmark]
    public double ParseLongFraction() => double.Parse(_longFraction, CultureInfo.InvariantCulture);

    [Benchmark]
    public string FormatSubnormal() => double.Epsilon.ToString("G99", CultureInfo.InvariantCulture);
}
```

| Method            | Runtime   | Mean     | Ratio |
| ----------------- | --------- | -------- | ----- |
| ParseLongFraction | .NET 10.0 | 8.592 μs | 1.00  |
| ParseLongFraction | .NET 11.0 | 3.569 μs | 0.42  |
| FormatSubnormal   | .NET 10.0 | 6.884 μs | 1.00  |
| FormatSubnormal   | .NET 11.0 | 1.237 μs | 0.18  |

The .NET 11 improvements aren’t limited to the scalar representations underlying `BigInteger` and floating-point parsing and formatting. Other numerical types improve as well. Consider `Matrix4x4`. A 4×4 matrix determinant combines products of many independent matrix elements, making it a natural fit for SIMD. [dotnet/runtime#123954](https://github.com/dotnet/runtime/pull/123954) from [@alexcovington](https://github.com/alexcovington) adds an SSE implementation of `Matrix4x4.GetDeterminant`, evaluating several of those products in parallel:

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;
using System.Numerics;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private readonly Matrix4x4 _matrix =
        Matrix4x4.CreateFromYawPitchRoll(0.4f, 0.8f, 1.1f) *
        Matrix4x4.CreateTranslation(1.5f, -2.5f, 3.25f) *
        Matrix4x4.CreateScale(1.1f, 0.9f, 1.05f);

    [Benchmark]
    public float GetDeterminant() => _matrix.GetDeterminant();
}
```

| Method         | Runtime   | Mean     | Ratio |
| -------------- | --------- | -------- | ----- |
| GetDeterminant | .NET 10.0 | 3.836 ns | 1.00  |
| GetDeterminant | .NET 11.0 | 2.645 ns | 0.69  |

The `System.Numerics.Tensors` APIs are designed to perform the same numerical operation over many values, making them a natural fit for SIMD. [dotnet/runtime#126052](https://github.com/dotnet/runtime/pull/126052) adds vector implementations of inverse sine to the portable vector types and uses them in `TensorPrimitives.Asin`. The tensor loop now evaluates a polynomial approximation for several inputs together, with special handling near the ends of the function’s `[-1, 1]` domain, rather than calling `MathF.Asin` or `Math.Asin` separately for every element:

```
// Run separately so each target uses its matching System.Numerics.Tensors package:
// dotnet run -c Release -f net10.0 --filter "*"
// dotnet run -c Release -f net11.0 --filter "*"

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;
using System.Numerics.Tensors;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private const int Length = 4096;

    private float[] _floatsIn = new float[Length];
    private float[] _floatsOut = new float[Length];
    private double[] _doublesIn = new double[Length];
    private double[] _doublesOut = new double[Length];

    [GlobalSetup]
    public void Setup()
    {
        Random rng = new(42);
        for (int i = 0; i < Length; i++)
        {
            float v = (float)((rng.NextDouble() * 2.0) - 1.0); // Asin's domain is [-1, 1]
            _floatsIn[i] = v;
            _doublesIn[i] = v;
        }
    }

    [Benchmark]
    public float AsinFloat()
    {
        TensorPrimitives.Asin(_floatsIn, _floatsOut);
        return _floatsOut[0];
    }

    [Benchmark]
    public double AsinDouble()
    {
        TensorPrimitives.Asin(_doublesIn, _doublesOut);
        return _doublesOut[0];
    }
}
```

| Method     | Runtime   | Mean      | Ratio |
| ---------- | --------- | --------- | ----- |
| AsinFloat  | .NET 10.0 | 33.72 μs  | 1.00  |
| AsinFloat  | .NET 11.0 | 8.240 μs  | 0.24  |
| AsinDouble | .NET 10.0 | 35.80 μs  | 1.00  |
| AsinDouble | .NET 11.0 | 10.975 μs | 0.31  |

`TensorPrimitives` also picked up a few more targeted SIMD improvements. For floating-point values, `BitIncrement` and `BitDecrement` move to the immediately adjacent representable value; despite their names, they can’t simply add or subtract one, as they also need to handle signed zero, infinities, and NaNs correctly. [dotnet/runtime#123610](https://github.com/dotnet/runtime/pull/123610) and [dotnet/runtime#123754](https://github.com/dotnet/runtime/pull/123754) process multiple `float`/`double` and `Half` values at once, respectively. The `Half` path works directly with the raw `ushort` bit patterns, avoiding conversion to `float` and back, and both paths use vector masks and conditional selection rather than calling a scalar helper for every element.

```
// Run separately so each target uses its matching System.Numerics.Tensors package:
// dotnet run -c Release -f net10.0 --filter "*"
// dotnet run -c Release -f net11.0 --filter "*"

using System.Numerics.Tensors;
using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private const int Length = 4096;
    private readonly float[] _floats = new float[Length];
    private readonly float[] _floatDestination = new float[Length];
    private readonly double[] _doubles = new double[Length];
    private readonly double[] _doubleDestination = new double[Length];
    private readonly Half[] _halves = new Half[Length];
    private readonly Half[] _halfDestination = new Half[Length];

    [GlobalSetup]
    public void Setup()
    {
        for (int i = 0; i < Length; i++)
        {
            float value = (i & 7) switch
            {
                0 => 0,
                1 => -0.0f,
                2 => float.PositiveInfinity,
                3 => float.NegativeInfinity,
                4 => float.NaN,
                _ => i / 7.0f,
            };
            _floats[i] = value;
            _doubles[i] = value;
            _halves[i] = (Half)value;
        }
    }

    [Benchmark]
    public void BitIncrementFloat() => TensorPrimitives.BitIncrement(_floats, _floatDestination);

    [Benchmark]
    public void BitIncrementDouble() => TensorPrimitives.BitIncrement(_doubles, _doubleDestination);

    [Benchmark]
    public void BitIncrementHalf() => TensorPrimitives.BitIncrement(_halves, _halfDestination);
}
```

| Method             | Runtime   | Mean       | Ratio |
| ------------------ | --------- | ---------- | ----- |
| BitIncrementFloat  | .NET 10.0 | 4.223 μs   | 1.00  |
| BitIncrementFloat  | .NET 11.0 | 1,071.1 ns | 0.25  |
| BitIncrementDouble | .NET 10.0 | 4.223 μs   | 1.00  |
| BitIncrementDouble | .NET 11.0 | 2,140.6 ns | 0.51  |
| BitIncrementHalf   | .NET 10.0 | 3.918 μs   | 1.00  |
| BitIncrementHalf   | .NET 11.0 | 573.6 ns   | 0.15  |

[dotnet/runtime#124280](https://github.com/dotnet/runtime/pull/124280) removes a more mechanical cost from `TensorPrimitives.Round`: for `digits == 0`, the old code invoked a full-span rounding kernel and then continued through another full-span pass. Returning immediately removes that redundant traversal and overwrite of the destination.

```
// Run separately so each target uses its matching System.Numerics.Tensors package:
// dotnet run -c Release -f net10.0 --filter "*"
// dotnet run -c Release -f net11.0 --filter "*"

using System.Numerics.Tensors;
using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private const int Length = 4096;
    private readonly float[] _source = new float[Length];
    private readonly float[] _destination = new float[Length];

    [Benchmark]
    public void RoundZero() => TensorPrimitives.Round(_source, 0, MidpointRounding.ToEven, _destination);
}
```

| Method    | Runtime   | Mean     | Ratio |
| --------- | --------- | -------- | ----- |
| RoundZero | .NET 10.0 | 882.7 ns | 1.00  |
| RoundZero | .NET 11.0 | 205.5 ns | 0.23  |

`Half` comparisons are faster as well. Previously, `Half.CompareTo` separately asked whether one value was less than, greater than, or equal to the other, repeating the special handling required for NaN and signed zero each time. [dotnet/runtime#131297](https://github.com/dotnet/runtime/pull/131297) performs that work once and then arranges the underlying bits into a form that can be compared directly, while still treating `+0` and `-0` as equal. On x64 with AVX2, it also makes `CompareTo`, `<`, and `<=` faster by converting the operands to `float`, which the hardware can do very efficiently. Equality remains bit-based, as that’s already the cheaper approach.

```
// Run on x64 with AVX2:
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private readonly Half[] _left = Enumerable.Range(0, 4096).Select(i => (Half)(i - 2048)).ToArray();
    private readonly Half[] _right = Enumerable.Range(0, 4096).Select(i => (Half)(2048 - i)).ToArray();

    [Benchmark]
    public int CompareTo()
    {
        int sum = 0;
        for (int i = 0; i < _left.Length; i++)
            sum += _left[i].CompareTo(_right[i]);

        return sum;
    }

    [Benchmark]
    public int LessThan()
    {
        int count = 0;
        for (int i = 0; i < _left.Length; i++)
            count += _left[i] < _right[i] ? 1 : 0;

        return count;
    }
}
```

| Method    | Runtime   | Mean     | Ratio |
| --------- | --------- | -------- | ----- |
| CompareTo | .NET 10.0 | 9.340 μs | 1.00  |
| CompareTo | .NET 11.0 | 5.745 μs | 0.62  |
|           |           |          |       |
| LessThan  | .NET 10.0 | 7.514 μs | 1.00  |
| LessThan  | .NET 11.0 | 5.672 μs | 0.75  |

Multiplying two 64-bit integers produces a 128-bit result, and x64 has instructions that provide both 64-bit halves directly. [dotnet/runtime#117261](https://github.com/dotnet/runtime/pull/117261) from [@Daniel-Svensson](https://github.com/Daniel-Svensson) exposes those signed and unsigned forms through an `X86Base.X64.BigMul` intrinsic. `Math.BigMul` can then map directly to `imul` or `mul` and return both halves in registers, avoiding the extra instructions and register shuffling required by the previous paths.

```
// Run on x64:
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[DisassemblyDiagnoser, HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private readonly long _signedLeft = 0x1234_5678_9ABC_DEF;
    private readonly long _signedRight = 0x0FED_CBA9_8765_432;
    private readonly ulong _unsignedLeft = 0xFEDC_BA98_7654_3210;
    private readonly ulong _unsignedRight = 0x1234_5678_9ABC_DEF0;

    [Benchmark]
    public long Signed()
    {
        long high = Math.BigMul(_signedLeft, _signedRight, out long low);
        return high ^ low;
    }

    [Benchmark]
    public ulong Unsigned()
    {
        ulong high = Math.BigMul(_unsignedLeft, _unsignedRight, out ulong low);
        return high ^ low;
    }
}
```

| Method   | Runtime   | Mean     | Ratio | Code Size |
| -------- | --------- | -------- | ----- | --------- |
| Signed   | .NET 10.0 | 2.210 ns | 1.00  | 65 B      |
| Signed   | .NET 11.0 | 1.344 ns | 0.61  | 12 B      |
|          |           |          |       |           |
| Unsigned | .NET 10.0 | 1.446 ns | 1.00  | 39 B      |
| Unsigned | .NET 11.0 | 1.323 ns | 0.92  | 12 B      |

Fixed-format numeric and identifier helpers benefit from a much simpler technique: establish the exact span length once, then let the JIT reuse that fact. [dotnet/runtime#119254](https://github.com/dotnet/runtime/pull/119254) from [@xtqqczze](https://github.com/xtqqczze) applies that pattern in `Decimal`, `Guid`, and `IPAddress`, removing repeated bounds checks.

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private const string Value = "a8098c1a-f86e-11da-bd1a-00112444be1e";

    [Benchmark]
    public bool TryParseExactD() => Guid.TryParseExact(Value, "D", out _);
}
```

| Method         | Runtime   | Mean     | Ratio |
| -------------- | --------- | -------- | ----- |
| TryParseExactD | .NET 10.0 | 15.94 ns | 1.00  |
| TryParseExactD | .NET 11.0 | 12.60 ns | 0.79  |

`Guid` has been improving every .NET release, and sees several improvements in .NET 11. Whenever possible, .NET tries to maintain similar performance and behaviors across operating systems, but low-level functionality often simply delegates to the operating system, exposing that OS’ characteristics. When it comes to random number generation, historically cryptographically-secure random number generation, as is used in `Guid.NewGuid`, has been a bit slower on Linux than on Windows due to using `/dev/urandom` as the source of entropy. [dotnet/runtime#123540](https://github.com/dotnet/runtime/pull/123540) from [@reedz](https://github.com/reedz) moves `Guid.NewGuid()` off of that file-descriptor path to the `getrandom()` syscall, avoiding descriptor setup and reads through the file abstraction.

And on the subject of randomness, [dotnet/runtime#119890](https://github.com/dotnet/runtime/pull/119890) from [@hamarb123](https://github.com/hamarb123) removes two pieces of work from `Random.Shuffle`: an unnecessary copy of the span length and a branch that skipped swapping an element with itself. A self-swap is harmless and uncommon, while testing for it adds an unpredictable branch to every iteration. The difference is most visible for short arrays and small value types, where the swap itself is cheap:

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    [Params(16, 4096)]
    public int Length;

    private readonly Random _random = new(42);
    private int[] _values = [];

    [GlobalSetup]
    public void Setup() => _values = Enumerable.Range(0, Length).ToArray();

    [Benchmark]
    public int ShuffleSmallValueType()
    {
        _random.Shuffle(_values);
        return _values[0] + _values[^1];
    }
}
```

| Method                | Runtime   | Length | Mean         | Ratio |
| --------------------- | --------- | ------ | ------------ | ----- |
| ShuffleSmallValueType | .NET 10.0 | 16     | 138.9 ns     | 1.00  |
| ShuffleSmallValueType | .NET 11.0 | 16     | 89.34 ns     | 0.64  |
| ShuffleSmallValueType | .NET 10.0 | 4096   | 25,925.1 ns  | 1.00  |
| ShuffleSmallValueType | .NET 11.0 | 4096   | 14,151.08 ns | 0.55  |

`Random` itself picked up a small but pointed code-generation fix. `Random.InternalSample` contains a condition that’s inherently hard for the processor to predict, so it’s better implemented with conditional instructions than with a branch. The JIT’s if-conversion support we previously discussed would have been able to do that transformation, except it doesn’t currently support if-conversion inside of loops, which is a pretty common place to find an inlined `Random.Next` call. [dotnet/runtime#131714](https://github.com/dotnet/runtime/pull/131714) marks the helper as `[MethodImpl(MethodImplOptions.NoInlining)]` to preserve the branch-free form; once the JIT can perform if-conversion inside loops, that annotation can be reconsidered.

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private readonly Random _random = new(42);

    [Benchmark]
    public int Next()
    {
        int sum = 0;
        for (int i = 0; i < 1024; i++)
            sum += _random.Next();

        return sum;
    }
}
```

| Method | Runtime   | Mean     | Ratio |
| ------ | --------- | -------- | ----- |
| Next   | .NET 10.0 | 5.954 μs | 1.00  |
| Next   | .NET 11.0 | 3.275 μs | 0.55  |

## Globalization

Many globalization-related APIs sit atop data that can be expensive to locate and interpret. `DateTime.Now`, for example, depends on time-zone transition data, while casing and parsing depend on native globalization services and culture-specific tables.

[dotnet/runtime#119662](https://github.com/dotnet/runtime/pull/119662) substantially reworks `TimeZoneInfo` around that observation. Determining an offset isn’t always a fixed arithmetic operation: daylight-saving rules can vary by year, and historical rules can contain multiple transitions and exceptional cases. Once the transitions for a zone and year have been interpreted, however, other conversions in that year can reuse them. Similarly, the local offset used by `DateTime.Now` can’t change between transition instants. Conversions now reuse cached per-year transition data rather than repeatedly walking adjustment rules, while `DateTime.Now` caches the active UTC offset together with the instant at which it next needs to be recomputed.

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private readonly DateTime _utc = new(2026, 7, 15, 12, 0, 0, DateTimeKind.Utc);
    private readonly DateTime _local = new(2026, 7, 15, 5, 0, 0, DateTimeKind.Unspecified);
    private readonly TimeZoneInfo _zone = TimeZoneInfo.FindSystemTimeZoneById(
        OperatingSystem.IsWindows() ? "Pacific Standard Time" : "America/Los_Angeles");

    [Benchmark]
    public DateTime ConvertTimeFromUtc() => TimeZoneInfo.ConvertTimeFromUtc(_utc, _zone);

    [Benchmark]
    public DateTime ConvertTimeToUtc() => TimeZoneInfo.ConvertTimeToUtc(_local, _zone);

    [Benchmark]
    public DateTime GetLocalNow() => DateTime.Now;
}
```

| Method             | Runtime   | Mean     | Ratio |
| ------------------ | --------- | -------- | ----- |
| ConvertTimeFromUtc | .NET 10.0 | 45.13 ns | 1.00  |
| ConvertTimeFromUtc | .NET 11.0 | 19.44 ns | 0.43  |
| ConvertTimeToUtc   | .NET 10.0 | 51.97 ns | 1.00  |
| ConvertTimeToUtc   | .NET 11.0 | 20.23 ns | 0.39  |
| GetLocalNow        | .NET 10.0 | 76.41 ns | 1.00  |
| GetLocalNow        | .NET 11.0 | 34.39 ns | 0.45  |

[dotnet/runtime#120685](https://github.com/dotnet/runtime/pull/120685) separates two costs in invariant casing. With the normal globalization configuration, `ToUpperInvariant` and `ToLowerInvariant` now try a managed ASCII path first, so casing ASCII text can avoid or delay initialization of ICU, the native library .NET uses for culture-aware globalization. In invariant-globalization mode, where ICU isn’t loaded at all, that managed path also improves ASCII casing throughput. Non-ASCII input still needs the appropriate globalization path.

```
// DOTNET_SYSTEM_GLOBALIZATION_INVARIANT=1 dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private readonly string _short = "runtime";
    private readonly string _long = new('a', 139);

    [Benchmark]
    public string ShortAscii() => _short.ToUpperInvariant();

    [Benchmark]
    public string LongAscii() => _long.ToUpperInvariant();
}
```

| Method     | Runtime   | Mean      | Ratio | Allocated |
| ---------- | --------- | --------- | ----- | --------- |
| ShortAscii | .NET 10.0 | 18.12 ns  | 1.00  | 40 B      |
| ShortAscii | .NET 11.0 | 14.62 ns  | 0.81  | 40 B      |
| LongAscii  | .NET 10.0 | 248.38 ns | 1.00  | 304 B     |
| LongAscii  | .NET 11.0 | 39.22 ns  | 0.16  | 304 B     |

Several smaller changes remove setup around date and culture data. [dotnet/runtime#123886](https://github.com/dotnet/runtime/pull/123886) allocates the `DateTimeFormatInfo` date-word table only for cultures that actually contain such words. And [dotnet/runtime#122918](https://github.com/dotnet/runtime/pull/122918) replaces synchronized, boxing `Hashtable` caches used by time-zone and encoding tables with typed `ConcurrentDictionary` instances.

The round-trip `"O"` date format always contains exactly seven fractional-second digits, matching the 10,000,000 ticks in a second. [dotnet/runtime#129005](https://github.com/dotnet/runtime/pull/129005) parses those digits directly as ticks, avoiding a conversion through `double` followed by division, multiplication, and rounding. Formatting benefits from specialization as well. [dotnet/runtime#129374](https://github.com/dotnet/runtime/pull/129374) routes invariant `DateTime.ToString("G")` through the existing fixed-format fast path, bypassing the general culture-aware formatter. `DateTimeOffset` retains the general path because its offset changes the output:

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Running;

using System.Globalization;
using BenchmarkDotNet.Attributes;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private readonly DateTime _dateTime = new(2024, 3, 15, 13, 45, 30, DateTimeKind.Utc);

    [Benchmark]
    public string DateTime_ToString_G() => _dateTime.ToString("G", CultureInfo.InvariantCulture);
}
```

| Method                | Runtime   | Mean     | Ratio |
| --------------------- | --------- | -------- | ----- |
| DateTime\_ToString\_G | .NET 10.0 | 65.54 ns | 1.00  |
| DateTime\_ToString\_G | .NET 11.0 | 29.76 ns | 0.45  |

## Strings and Spans

UTF-8 is everywhere, from web protocols and JSON payloads to files on disk. Since .NET strings use UTF-16, applications frequently need to convert between the two, making it especially important for those conversions to be fast. UTF-8 encoding must validate UTF-16 surrogate pairs as it counts and converts them. On Arm64, the vectorized implementation in .NET 10 still examined individual elements when counting the resulting UTF-8 bytes and checking that surrogates were correctly paired. That gets expensive for text containing many supplementary characters, as every surrogate-heavy vector falls back to this element-by-element work. [dotnet/runtime#121981](https://github.com/dotnet/runtime/pull/121981) from [@ylpoonlg](https://github.com/ylpoonlg) instead performs the counting and surrogate checks with vector-wide operations. As part of that work, it also unifies most of the x86 and Arm64 implementations, retaining small platform-specific helpers where the instruction sets differ:

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;
using System.Text;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private const int Length = 4096;

    private string _validWithSurrogatePairs = string.Empty;

    [GlobalSetup]
    public void Setup()
    {
        Random rng = new(42);
        StringBuilder sb = new(Length);
        while (sb.Length < Length - 2)
        {
            sb.Append((char)('A' + rng.Next(0, 26)));
            sb.Append("\U0001F600"); // emoji -> surrogate pair
        }

        _validWithSurrogatePairs = sb.ToString();
    }

    [Benchmark]
    public int ValidWithSurrogatePairs() => Encoding.UTF8.GetByteCount(_validWithSurrogatePairs);
}
```

This input deliberately contains a surrogate pair for every ASCII character, making the removed per-element work especially visible.

| Method                  | Runtime   | Mean     | Ratio |
| ----------------------- | --------- | -------- | ----- |
| ValidWithSurrogatePairs | .NET 10.0 | 2.994 μs | 1.00  |
| ValidWithSurrogatePairs | .NET 11.0 | 856.8 ns | 0.29  |

The byte-to-char direction was also improved on Arm. UTF-8 decoding can copy ASCII bytes directly to UTF-16 characters, but as soon as we find the first non-ASCII byte, we need the full multi-byte decoder. The vector loop therefore needs both a fast test for whether any lane is non-ASCII and, only when one is found, its exact position. Calculating that position for every all-ASCII vector wastes work on the overwhelmingly common fast path. [dotnet/runtime#121382](https://github.com/dotnet/runtime/pull/121382) from [@ylpoonlg](https://github.com/ylpoonlg) first performs the cheap vector-wide test and then computes the lane index only after that test succeeds.

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;
using System.Text;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private readonly byte[] _ascii = Enumerable.Repeat((byte)'a', 16_384).ToArray();

    [Benchmark]
    public int Utf8GetCharCount() => Encoding.UTF8.GetCharCount(_ascii);
}
```

With all-ASCII input, every vector can stay on the cheap path:

| Method           | Runtime   | Mean     | Ratio |
| ---------------- | --------- | -------- | ----- |
| Utf8GetCharCount | .NET 10.0 | 443.5 ns | 1.00  |
| Utf8GetCharCount | .NET 11.0 | 210.6 ns | 0.47  |

Base64 is commonly used when binary data needs to travel through text-oriented formats and protocols. Its encoder naturally works in groups of three input bytes and four output characters, but the line-breaking option also needs to stop at the MIME-style 76-character boundary and insert `\r\n`. The older implementation handled that formatting through a separate scalar path. [dotnet/runtime#123403](https://github.com/dotnet/runtime/pull/123403) brings the optimized span-based Base64 encoder to `Convert.ToBase64String` with `Base64FormattingOptions.InsertLineBreaks`, processing each line with the same vectorized core and handling the separators around it:

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Running;

using BenchmarkDotNet.Attributes;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    [Params(57, 570)]
    public int ByteLength { get; set; }

    private byte[] _bytes = [];

    [GlobalSetup]
    public void Setup()
    {
        _bytes = new byte[ByteLength];
        new Random(42).NextBytes(_bytes);
    }

    [Benchmark]
    public string ToBase64String_InsertLineBreaks() => Convert.ToBase64String(_bytes, Base64FormattingOptions.InsertLineBreaks);
}
```

| Method                           | Runtime   | ByteLength | Mean      | Ratio |
| -------------------------------- | --------- | ---------- | --------- | ----- |
| ToBase64String\_InsertLineBreaks | .NET 10.0 | 57         | 60.95 ns  | 1.00  |
| ToBase64String\_InsertLineBreaks | .NET 11.0 | 57         | 23.91 ns  | 0.39  |
| ToBase64String\_InsertLineBreaks | .NET 10.0 | 570        | 560.66 ns | 1.00  |
| ToBase64String\_InsertLineBreaks | .NET 11.0 | 570        | 194.99 ns | 0.35  |

Base64 decoding got the same treatment from the other direction. `Base64.DecodeFromUtf8InPlace` decodes in place, overwriting the encoded input with the decoded bytes. In .NET 10, it still employed a scalar loop, long after the out-of-place `DecodeFromUtf8` had acquired AVX-512, AVX2, AdvSimd, and SSSE3 paths. In-place decoding turns out to be safe to vectorize precisely because of Base64’s ratio: 4 bytes read produce 3 bytes written, so the write cursor always trails the read cursor, and each vector store, including its zero-padded overshoot, ends at or before the next vector load and never clobbers source that hasn’t been read yet. [dotnet/runtime#131333](https://github.com/dotnet/runtime/pull/131333) therefore reuses the existing decode helpers for the in-place path.

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using System.Buffers;
using System.Buffers.Text;
using System.Text;
using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private readonly byte[] _encoded = Encoding.ASCII.GetBytes(Convert.ToBase64String(new byte[16_384]));
    private byte[] _buffer = [];

    [IterationSetup]
    public void Setup() => _buffer = (byte[])_encoded.Clone();

    [Benchmark]
    public OperationStatus Decode() => Base64.DecodeFromUtf8InPlace(_buffer, out _);
}
```

| Method | Runtime   | Mean     | Ratio |
| ------ | --------- | -------- | ----- |
| Decode | .NET 10.0 | 10.70 μs | 1.00  |
| Decode | .NET 11.0 | 2.256 μs | 0.21  |

`MemoryExtensions.CommonPrefixLength` compares two spans and returns how many elements they share at the beginning (“hello” and “help”, for example, have a common prefix length of 3). Internally, it utilizes a helper that slices whichever input was longer to the length of the shorter one. [dotnet/runtime#121104](https://github.com/dotnet/runtime/pull/121104) from [@xtqqczze](https://github.com/xtqqczze) simplifies that helper: after shortening the second span if necessary, it always slices the first span to the second’s length. That gives the JIT the same explicit relationship between the two lengths regardless of which input started out longer.

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;
using System;
using System.Linq;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private readonly string[] _shorter = Enumerable.Repeat("value", 64).ToArray();
    private readonly string[] _longer = Enumerable.Repeat("value", 128).ToArray();

    [Benchmark]
    public int ShorterFirst() => _shorter.AsSpan().CommonPrefixLength(_longer);

    [Benchmark]
    public int LongerFirst() => _longer.AsSpan().CommonPrefixLength(_shorter);
}
```

The longer-first case was already efficient. The change improves the shorter-first case, bringing the two orderings to essentially the same throughput:

| Method       | Runtime   | Mean     | Ratio |
| ------------ | --------- | -------- | ----- |
| ShorterFirst | .NET 10.0 | 45.16 ns | 1.00  |
| ShorterFirst | .NET 11.0 | 25.43 ns | 0.56  |
| LongerFirst  | .NET 10.0 | 26.40 ns | 1.00  |
| LongerFirst  | .NET 11.0 | 26.31 ns | 1.00  |

Text processing often starts by obtaining an `Encoding`. Properties such as `Encoding.UTF8` provide fast access to popular encodings, while legacy code pages can be made available by registering `CodePagesEncodingProvider`. In .NET 10, that provider’s tables, including the name lookup used by `Encoding.GetEncoding(string)` once the provider is registered, used reader-writer locks. [dotnet/runtime#125001](https://github.com/dotnet/runtime/pull/125001) replaces those caches with `ConcurrentDictionary` instances, allowing warmed-up provider lookups to proceed without acquiring the reader lock.

On `string` itself, [dotnet/runtime#130361](https://github.com/dotnet/runtime/pull/130361) from [@prozolic](https://github.com/prozolic) recognizes when `string.Concat(IEnumerable<string?>)` receives a `string[]` or `List<string?>` and passes its contiguous storage directly to the span-based implementation:

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using System.Collections.Generic;
using System.Linq;

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[MemoryDiagnoser(false), HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private readonly IEnumerable<string?> _array = Enumerable.Range(0, 1_000).Select(i => i.ToString()).ToArray();
    private readonly IEnumerable<string?> _list = Enumerable.Range(0, 1_000).Select(i => i.ToString()).ToList();

    [Benchmark]
    public string Array() => string.Concat(_array);

    [Benchmark]
    public string List() => string.Concat(_list);
}
```

| Method | Runtime   | Mean     | Ratio | Allocated |
| ------ | --------- | -------- | ----- | --------- |
| Array  | .NET 10.0 | 4.230 μs | 1.00  | 5.7 KB    |
| Array  | .NET 11.0 | 3.285 μs | 0.78  | 5.67 KB   |
| List   | .NET 10.0 | 6.703 μs | 1.00  | 5.71 KB   |
| List   | .NET 11.0 | 3.253 μs | 0.49  | 5.67 KB   |

Some of my favorite improvements in .NET are the tiny ones that show up everywhere. A good example of that is in [dotnet/roslyn#82729](https://github.com/dotnet/roslyn/pull/82729). Previously, when you wrote `span[start..]`, the compiler would lower that to the equivalent of `span.Slice(start, span.Length - start)`. The JIT has made strides towards compiling this exactly how it would `span.Slice(start)`, but everyone is better off if the C# compiler just emits that in the first place. And it now does. The difference is clear in the IL for a method that returns `span[start..]`:

```
; Platform-independent IL
-// Before: 21 bytes
+// After: 9 bytes
-.locals init ([0] System.Span<char>&, [1] int32)
 ldarga.s span
-stloc.0
 ldarg.1
-stloc.1
-ldloc.0
-ldloc.1
-ldloc.0
-call instance int32 System.Span<char>::get_Length()
-ldloc.1
-sub
-call instance System.Span<char> System.Span<char>::Slice(int32, int32)
+call instance System.Span<char> System.Span<char>::Slice(int32)
 ret
```

### Searching and Comparing

Searching in one way, shape, or form is one of the most common things programs do. And when it comes to searching text, regular expressions are an extremely common and helpful way to specify and perform said search. .NET’s regex support has improved by leaps and bounds over the years, with significant investments in .NET 5 and .NET 7 and then every release since, including .NET 11.

When a `Regex` instance is created, it needs to parse the incoming regular expression pattern and turn it into a form it can utilize for performing the actual searches. The regex language is very expressive and enables multiple ways of specifying the same pattern, some more efficient to process than others, so as part of parsing, `Regex` applies a variety of simplifications and optimizations over the parsed tree in order to put it into an ideal form, as well as to learn facts about the pattern to further optimize later processing (such as discovering a minimum and maximum length of any possible match). Each of these transformations can in turn expose more opportunity for other transformations, but based on the order the transformations are applied, sometimes those opportunities can be missed. In .NET 11, [dotnet/runtime#125289](https://github.com/dotnet/runtime/pull/125289) gives compiled and source-generated regexes one final cleanup pass after the whole-pattern optimizations have reshaped the pattern. Consider the pattern `[ab]+c[ab]+|[ab]+`. On input containing a long run of `a`s with no `c`, the .NET 10 source-generated matcher first scans the whole run for the first alternative, fails when it doesn’t find the `c`, and then scans the same run again for the second alternative. The final cleanup pass factors out the common `[ab]+`, leaving `c[ab]+` as an optional suffix:

```
// dotnet run -c Release -f net10.0 --filter "*"
// dotnet run -c Release -f net11.0 --filter "*"

using System.Text.RegularExpressions;

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public partial class Benchmarks
{
    private readonly string _input = new('a', 4096);

    [Benchmark]
    public bool SharedPrefix() => SharedPrefixRegex().IsMatch(_input);

    [GeneratedRegex("[ab]+c[ab]+|[ab]+")]
    private static partial Regex SharedPrefixRegex();
}
```

| Method       | Runtime   | Mean     | Ratio |
| ------------ | --------- | -------- | ----- |
| SharedPrefix | .NET 10.0 | 550.9 ns | 1.00  |
| SharedPrefix | .NET 11.0 | 282.8 ns | 0.51  |

Beyond doing additional passes, several other changes improve what those analysis passes can see. For example, for a pattern like `(http|https)` with ordinal ignore-case matching, for uninteresting reasons previously the engine would extract a prefix of `"htt"`, even though it could have extracted `"http"`. [dotnet/runtime#124881](https://github.com/dotnet/runtime/pull/124881) improves that, enabling the engine to skip far more false candidates. The input here contains 25,000 `"htt"` prefixes that aren’t followed by a `p` before the final match:

```
// dotnet run -c Release -f net10.0 --filter "*"
// dotnet run -c Release -f net11.0 --filter "*"

using System.Text.RegularExpressions;

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public partial class Benchmarks
{
    private readonly string _input = string.Concat(Enumerable.Repeat("httx", 25_000)) + "https";

    [Benchmark]
    public bool IgnoreCaseAlternation() => Http.IsMatch(_input);

    [GeneratedRegex("(http|https)", RegexOptions.IgnoreCase)]
    private static partial Regex Http { get; }
}
```

| Method                | Runtime   | Mean     | Ratio |
| --------------------- | --------- | -------- | ----- |
| IgnoreCaseAlternation | .NET 10.0 | 415.0 μs | 1.00  |
| IgnoreCaseAlternation | .NET 11.0 | 7.012 μs | 0.017 |

When those transformation passes are looking for various patterns, sometimes small things obscure what they’re trying to see, and they miss optimizations. [dotnet/runtime#124842](https://github.com/dotnet/runtime/pull/124842) improves a case where captures were getting in the way of identifying a searchable prefix. For a pattern like `\b(in)\b` with `RegexOptions.IgnoreCase`, it will now discover it can search for ordinal-ignore-case `"in"`.

```
// dotnet run -c Release -f net10.0 --filter "*"
// dotnet run -c Release -f net11.0 --filter "*"

using System.Text.RegularExpressions;

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public partial class Benchmarks
{
    private readonly string _input = string.Concat(Enumerable.Repeat("xn ", 33_333)) + "in";

    [Benchmark]
    public bool IgnoreCaseCapturedPrefix() => CapturedPrefix.IsMatch(_input);

    [GeneratedRegex(@"\b(in)\b", RegexOptions.IgnoreCase)]
    private static partial Regex CapturedPrefix { get; }
}
```

| Method                   | Runtime   | Mean     | Ratio |
| ------------------------ | --------- | -------- | ----- |
| IgnoreCaseCapturedPrefix | .NET 10.0 | 277.7 μs | 1.00  |
| IgnoreCaseCapturedPrefix | .NET 11.0 | 7.286 μs | 0.026 |

As these cases highlight, one of the most impactful things we can do for regular expression processing is improve the engine’s ability to find things to search for as the next possible place a match could apply, and to optimize that search. [dotnet/runtime#124736](https://github.com/dotnet/runtime/pull/124736) does that. For compiled, source-generated, and `NonBacktracking` regexes, it improves how the engine is able to search for one of several literal prefixes. For `agggtaaa|tttaccct`, for example, the .NET 10 source generator first searched for `[ag]` at offset 3 and then checked nearby characters for `[gt]`. That’s a weak filter for an input full of `a` characters, where almost every position becomes a candidate. The .NET 11 generator instead searches for the complete `agggtaaa` and `tttaccct` strings with `SearchValues<string>`. A frequency heuristic selects this approach only for case-sensitive alternatives where whole-string searching is expected to reject more false candidates than the available character-set filter.

```
// dotnet run -c Release -f net10.0 --filter "*"
// dotnet run -c Release -f net11.0 --filter "*"

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

using System.Text.RegularExpressions;

BenchmarkSwitcher.FromAssembly(typeof(RegexPrefixBenchmarks).Assembly).Run(args);

[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public partial class RegexPrefixBenchmarks
{
    private const string Pattern = "agggtaaa|tttaccct";

    private readonly string _match = new string('a', 100_000) + "tttaccct";
    private readonly string _miss = new('a', 100_000);

    [Benchmark]
    public bool Match() => Generated.IsMatch(_match);

    [Benchmark]
    public bool Miss() => Generated.IsMatch(_miss);

    [GeneratedRegex(Pattern)]
    private static partial Regex Generated { get; }
}
```

| Method | Runtime   | Mean     | Ratio |
| ------ | --------- | -------- | ----- |
| Match  | .NET 10.0 | 861.9 μs | 1.00  |
| Match  | .NET 11.0 | 9.251 μs | 0.011 |
| Miss   | .NET 10.0 | 861.4 μs | 1.00  |
| Miss   | .NET 11.0 | 9.647 μs | 0.011 |

Of course, searching for the next place to match isn’t the only opportunity for improvement. Once you’ve found that place, you need to try to match, and we want to optimize that further, too.

Consider the pattern `\b\w+n\b`. The `\w+` can match `n`, which means we can’t automatically treat this loop as being atomic. Normally, after matching the loop greedily and failing to match `n`, we’d need to backtrack looking for the next viable place to match the `n`. But if what comes after the `n` (in this case, a boundary) can’t possibly match the loop, we can avoid doing that search. [dotnet/runtime#125636](https://github.com/dotnet/runtime/pull/125636) teaches the compiled and source-generated engines to prove that and test the final position directly rather than searching backward through the loop’s existing match. The same idea applies to other loops followed by a literal when the engine can prove that trying earlier positions can’t change the result.

```
// dotnet run -c Release -f net10.0 --filter "*"
// dotnet run -c Release -f net11.0 --filter "*"

using BenchmarkDotNet.Running;

using System.Text.RegularExpressions;
using BenchmarkDotNet.Attributes;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public partial class Benchmarks
{
    private const int WordLength = 5000;
    private readonly string _matchingWord = new string('a', WordLength - 1) + "n";
    private readonly string _nonMatchingWord = new string('a', WordLength - 1) + "b";

    [GeneratedRegex(@"\b\w+n\b")]
    private static partial Regex Generated { get; }

    [Benchmark]
    public bool Matching() => Generated.IsMatch(_matchingWord);

    [Benchmark]
    public bool NonMatching() => Generated.IsMatch(_nonMatchingWord);
}
```

| Method      | Runtime   | Mean      | Ratio |
| ----------- | --------- | --------- | ----- |
| Matching    | .NET 10.0 | 3.441 μs  | 1.00  |
| Matching    | .NET 11.0 | 3.118 μs  | 0.91  |
| NonMatching | .NET 10.0 | 83.656 μs | 1.00  |
| NonMatching | .NET 11.0 | 69.984 μs | 0.84  |

A match can sometimes be ruled out before examining any of the input’s characters. When matching starts at position zero, a fixed-length pattern with a leading `\A` or non-multiline `^` and a trailing `\z` can match only when the whole input has exactly that length. [dotnet/runtime#120916](https://github.com/dotnet/runtime/pull/120916) emits that length check up front for the compiled and source-generated engines when the computed maximum length equals the minimum required length. Here, the pattern requires exactly 512 characters while the input contains 513:

```
// dotnet run -c Release -f net10.0 --filter "*"
// dotnet run -c Release -f net11.0 --filter "*"

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

using System.Text.RegularExpressions;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public partial class Benchmarks
{
    private readonly string _tooLong = new('a', 513);

    [GeneratedRegex(@"\A[a-z]{512}\z")]
    private static partial Regex Generated { get; }

    [Benchmark]
    public bool AnchoredReject() => Generated.IsMatch(_tooLong);
}
```

| Method         | Runtime   | Mean     | Ratio |
| -------------- | --------- | -------- | ----- |
| AnchoredReject | .NET 10.0 | 41.06 ns | 1.00  |
| AnchoredReject | .NET 11.0 | 16.05 ns | 0.39  |

In general, we’ve tried to keep the compilers behind `RegexOptions.Compiled` (which emits IL) and the source generator (which emits C#) as close to 1:1 as possible. There are a few cases, however, where they have diverged from each other, generally where one was able to easily utilize some feature of the target language the other didn’t have. A good example is with alternations. If several left-to-right atomic branches each begin with a different literal character, the engine can read that character and jump straight to the matching branch rather than testing each branch in order. With C#, we emitted a `switch`, which the C# compiler could then lower to IL using various strategies. For IL, in .NET 10 and earlier, without the C# compiler to provide those optimizations, we just skipped the optimization. Now in .NET 11, [dotnet/runtime#122959](https://github.com/dotnet/runtime/pull/122959) emits a similar implementation to what the C# compiler would have, bringing this optimization to `RegexOptions.Compiled`.

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using System.Text.RegularExpressions;

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private readonly string _input = string.Concat(Enumerable.Repeat("p15", 10_000));
    private readonly Regex _regex = new(@"(?>a0|b1|c2|d3|e4|f5|g6|h7|i8|j9|k10|l11|m12|n13|o14|p15)", RegexOptions.Compiled);

    [Benchmark]
    public int DispatchToFinalBranch() => _regex.Count(_input);
}
```

| Method                | Runtime   | Mean     | Ratio |
| --------------------- | --------- | -------- | ----- |
| DispatchToFinalBranch | .NET 10.0 | 233.9 μs | 1.00  |
| DispatchToFinalBranch | .NET 11.0 | 159.5 μs | 0.68  |

Another of the few differences between compiled and source-generated regexes had to do with backreferences. A case-sensitive backreference, such as the `\1` in `([a-z]+)-\1`, asks whether the next input equals text that was previously captured in the match. Source-generated regexes were using the optimized `SequenceEqual` to do that comparison, whereas `RegexOptions.Compiled` wasn’t. With [dotnet/runtime#123914](https://github.com/dotnet/runtime/pull/123914) in .NET 11, now it does.

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

using System.Text.RegularExpressions;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private readonly string _input = new string('a', 256) + "-" + new string('a', 256);
    private readonly Regex _regex = new(@"^([a-z]{256})-\1$", RegexOptions.Compiled);

    [Benchmark]
    public bool Backreference() => _regex.IsMatch(_input);
}
```

| Method        | Runtime   | Mean     | Ratio |
| ------------- | --------- | -------- | ----- |
| Backreference | .NET 10.0 | 168.4 ns | 1.00  |
| Backreference | .NET 11.0 | 49.59 ns | 0.29  |

Searching isn’t limited to `Regex`, of course. Many other methods in .NET help finding things and comparing things, some of which get notable bumps in .NET 11.

The `Ascii` class provides optimized helpers for validating and manipulating ASCII text. Members like `Equals` are already vectorized in .NET 10, but in .NET 11, [dotnet/runtime#123115](https://github.com/dotnet/runtime/pull/123115) improves that implementation by ensuring that inputs of length 8 through 15 can be vectorized, as well.

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Running;

using BenchmarkDotNet.Attributes;
using System.Text;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    [Params(8, 15)]
    public int Length { get; set; }

    private byte[] _bytes = [];
    private char[] _charsMatching = [];

    [GlobalSetup]
    public void Setup()
    {
        _bytes = new byte[Length];
        _charsMatching = new char[Length];
        for (int i = 0; i < Length; i++)
        {
            byte b = (byte)('a' + (i % 26));
            _bytes[i] = b;
            _charsMatching[i] = (char)b;
        }
    }

    [Benchmark]
    public bool Equals_Matching() => Ascii.Equals(_bytes, _charsMatching);
}
```

| Method           | Runtime   | Length | Mean     | Ratio |
| ---------------- | --------- | ------ | -------- | ----- |
| Equals\_Matching | .NET 10.0 | 8      | 3.834 ns | 1.00  |
| Equals\_Matching | .NET 11.0 | 8      | 1.966 ns | 0.51  |
| Equals\_Matching | .NET 10.0 | 15     | 6.177 ns | 1.00  |
| Equals\_Matching | .NET 11.0 | 15     | 2.398 ns | 0.39  |

[dotnet/runtime#130644](https://github.com/dotnet/runtime/pull/130644) also improves equality performance, in this case with `SequenceEqual` over a span of `Guid` or `Int128`. Previously, `SequenceEqual` treated these as arbitrary structures and compared them one element at a time. The PR teaches the runtime that their fixed bitwise representations are suitable for comparison as raw bytes. That enables the same optimized memory-comparison path used for primitive types, including JIT unrolling and vectorization:

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private readonly Guid[] _guids1 = new Guid[2];
    private readonly Guid[] _guids2 = new Guid[2];
    private readonly Int128[] _int128s1 = new Int128[2];
    private readonly Int128[] _int128s2 = new Int128[2];

    [Benchmark]
    public bool GuidEqual() => _guids1.AsSpan().SequenceEqual(_guids2);

    [Benchmark]
    public bool Int128Equal() => _int128s1.AsSpan().SequenceEqual(_int128s2);
}
```

| Method      | Runtime   | Mean     | Ratio |
| ----------- | --------- | -------- | ----- |
| GuidEqual   | .NET 10.0 | 2.960 ns | 1.00  |
| GuidEqual   | .NET 11.0 | 2.077 ns | 0.70  |
| Int128Equal | .NET 10.0 | 3.547 ns | 1.00  |
| Int128Equal | .NET 11.0 | 2.077 ns | 0.59  |

Another improvement in .NET 11 is to `string.Split`. Before `string.Split` can produce the resulting strings, it first needs to find the characters that separate them and record their positions. In .NET 10, that search is already vectorized: rather than examine one UTF-16 character at a time, it loads a vector’s worth, compares all of its lanes against the separator in parallel, and turns the comparison result into a mask identifying any matches. It then advances to the next vector, or uses the mask to record the matching positions. In .NET 11, on x86/x64, [dotnet/runtime#125379](https://github.com/dotnet/runtime/pull/125379) from [@hamarb123](https://github.com/hamarb123) makes the no-match path cheaper for ASCII separators. It loads two vectors of UTF-16 characters, packs their 16-bit elements into one vector of bytes, and checks that combined vector for the separator. If there isn’t a match, it has skipped twice as much input with one packed comparison; only a possible match requires the full 16-bit comparisons needed to determine its exact position. (This same packing technique is already employed elsewhere, such as in various `SearchValues<T>` implementations.)

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private readonly string _input = new('a', 16_384);

    [Benchmark]
    public int SplitNoSeparators() => _input.Split(',').Length;
}
```

| Method            | Runtime   | Mean     | Ratio |
| ----------------- | --------- | -------- | ----- |
| SplitNoSeparators | .NET 10.0 | 786.0 ns | 1.00  |
| SplitNoSeparators | .NET 11.0 | 404.7 ns | 0.51  |

A related Arm64 text-search improvement comes from [dotnet/runtime#126678](https://github.com/dotnet/runtime/pull/126678). A vector comparison produces a vector whose elements are all zero for non-matches and all one bits for matches. Finding the first or last match then requires condensing those bits into a scalar value and counting its leading or trailing zeros. On x86, the runtime can use a movemask instruction for that condensing step. Arm64 has no direct equivalent, and the old implementation needed a sequence of shifts, widening operations, and a horizontal add to achieve it. The .NET 11 implementation now uses `shrn`, Arm64’s shift-right-and-narrow instruction, to pack the relevant bits directly. `SearchValues<char>` uses these helpers, so the following benchmark reaches the affected code while searching for a match at the end of the input.

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;
using System.Buffers;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[DisassemblyDiagnoser(maxDepth: 3)]
[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private const int Length = 8_192;
    private static readonly SearchValues<char> s_vowels = SearchValues.Create("aeiouAEIOU");
    private static readonly string s_input = new string('x', Length - 1) + 'e';

    [Benchmark]
    public int IndexOfAny() => s_input.AsSpan().IndexOfAny(s_vowels);
}
```

The .NET 10 match-index path requires this sequence:

```
; Arm64
; .NET 10
cmeq    v16.16b, v16.16b, #0
movi    v17.16b, #0x80
and     v16.16b, v16.16b, v17.16b
ldr     q17, [MASK]
ushl    v16.16b, v16.16b, v17.16b
uxtl2   v17.8h, v16.16b
shl     v17.8h, v17.8h, #8
uaddw   v16.8h, v17.8h, v16.8b
addv    h16, v16.8h
umov    w2, v16.h[0]
mvn     w2, w2
rbit    w2, w2
clz     w2, w2
```

In .NET 11, the equivalent work is simpler:

```
; Arm64
; .NET 11
cmeq    v16.16b, v16.16b, #0
mvn     v16.16b, v16.16b
shrn    v16.8b, v16.8h, #4
fmov    x2, d16
rbit    x2, x2
clz     x2, x2
lsr     w2, w2, #2
```

`MemoryExtensions` already provides span-based searches for one or more values with `IndexOfAny`, and for contiguous ranges with `IndexOfAnyInRange`, along with `Except`, `Contains`, and last-index variants of these operations. For example, `span.IndexOfAnyInRange('0', '9')` finds the next ASCII digit. Whitespace is also common to search for, but the characters recognized by `char.IsWhiteSpace` are spread across multiple parts of Unicode rather than forming one contiguous range. To avoid requiring every caller to construct the same `SearchValues<char>`, [dotnet/runtime#111439](https://github.com/dotnet/runtime/pull/111439) from [@AlexRadch](https://github.com/AlexRadch) adds `ContainsAnyWhiteSpace`, `IndexOfAnyWhiteSpace`, `IndexOfAnyExceptWhiteSpace`, `LastIndexOfAnyWhiteSpace`, and `LastIndexOfAnyExceptWhiteSpace` for `ReadOnlySpan<char>`. Their shared `SearchValues<char>`-based implementation vectorizes these searches for parsers, validators, trimming code, and other text-processing code.

This is, however, a good example of how vectorization isn’t always a win. Take trimming. To trim leading whitespace, code needs to find the first character that isn’t whitespace. That character could be deep into the string, but in the most common case, there’s little or nothing to trim. A scalar loop can then return after inspecting just one or two characters, whereas the vectorized helper has fixed setup cost. It’s still worth vectorizing, because that overhead is small and the benefits when there is a lot to scan can be significant. Something to keep in mind.

```
// dotnet run -c Release -f net11.0 --filter "*"

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private readonly string _input = new(' ', 256);

    [Benchmark(Baseline = true)]
    public int Scalar()
    {
        ReadOnlySpan<char> input = _input;
        for (int i = 0; i < input.Length; i++)
        {
            if (!char.IsWhiteSpace(input[i]))
                return i;
        }

        return -1;
    }

    [Benchmark]
    public int Vectorized() => _input.AsSpan().IndexOfAnyExceptWhiteSpace();
}
```

| Method     | Mean      | Ratio |
| ---------- | --------- | ----- |
| Scalar     | 127.87 ns | 1.00  |
| Vectorized | 13.14 ns  | 0.10  |

This method is a particularly good fit when needing to validate that input does not contain any whitespace; that requires searching the entirety of input, which is where the vectorization in these methods shines. As an example of this, [dotnet/runtime#127123](https://github.com/dotnet/runtime/pull/127123) uses it to accelerate the parsing of the `"X"` GUID format:

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private static readonly string s_noWhitespace =
        Guid.Parse("a8098c1a-f86e-11da-bd1a-00112444be1e").ToString("X");

    [Benchmark]
    public Guid ParseExactX() => Guid.ParseExact(s_noWhitespace, "X");
}
```

| Method      | Runtime   | Mean     | Ratio |
| ----------- | --------- | -------- | ----- |
| ParseExactX | .NET 10.0 | 120.6 ns | 1.00  |
| ParseExactX | .NET 11.0 | 87.10 ns | 0.72  |

Closely related to searching is sorting. Years ago, sorting methods for `Span<T>` were added to `MemoryExtensions`. Interestingly, the method wasn’t added as `Sort<T>` but rather as `Sort<T, TComparer>` where `TComparer : IComparer<T>`. That signature enables a caller to provide a struct comparer without allocating a delegate or class-based comparer. Because the comparer is a constrained value type, the JIT should also be able to inline the comparison into the hot sorting loop. In practice, the implementation boxed the struct into an `IComparer<T>`, both allocating and turning every comparison back into an interface call. This was known at the time, but avoiding the box used generic implementation techniques that then carried too much runtime and code-size cost. Those supporting costs have since been addressed, so [dotnet/runtime#116109](https://github.com/dotnet/runtime/pull/116109) from [@2A5F](https://github.com/2A5F) now carries a value-type comparer through `Span<T>.Sort` without boxing it. The JIT can specialize the sorting routine for that comparer and inline the comparison.

The generic specialization does increase generated code and very large comparer structs can be more expensive to copy; this optimization is aimed at the small stateless or lightly stateful structs for which the API was designed.

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[MemoryDiagnoser(false), HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private readonly int[] _source = Enumerable.Range(0, 512).Select(i => (i * 257) % 512).ToArray();
    private int[] _values = [];

    [IterationSetup]
    public void Setup() => _values = (int[])_source.Clone();

    [Benchmark]
    public void Sort() => _values.AsSpan().Sort(new DescendingComparer());

    private readonly struct DescendingComparer : IComparer<int>
    {
        public int Compare(int x, int y) => y.CompareTo(x);
    }
}
```

| Method | Runtime   | Mean     | Ratio | Allocated |
| ------ | --------- | -------- | ----- | --------- |
| Sort   | .NET 10.0 | 11.62 μs | 1.00  | 88 B      |
| Sort   | .NET 11.0 | 3.533 μs | 0.30  | –         |

## Collections and LINQ

Much of the collection and LINQ work in .NET 11 comes from taking better advantage of information that’s already available. A collection often knows much more than an `IEnumerable<T>` can express: its count, its contiguous storage, its comparer, or the layout of its hash table. Similarly, a LINQ iterator can know how many elements it represents or how its operations were composed. Preserving that information can avoid enumeration, temporary storage, repeated hashing, and other work a general-purpose implementation would otherwise need to perform.

[dotnet/runtime#119896](https://github.com/dotnet/runtime/pull/119896) from [@prozolic](https://github.com/prozolic) changes `ImmutableArray.Create` to use `Array.Copy` rather than a hand-written element loop. A general element-by-element copy repeatedly performs indexing and assignment, while the runtime can specialize `Array.Copy` for the element type and size. For blittable data, it can use optimized bulk memory copies, and for reference types, which need GC write barriers, it performs the required write barriers in the runtime’s tuned copy helpers. The change therefore both simplifies the managed code and gives `ImmutableArray` access to those optimized implementations.

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Running;

using System.Collections.Immutable;
using BenchmarkDotNet.Attributes;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[HideColumns("Job", "Error", "StdDev", "RatioSD", "Median")]
public class Benchmarks
{
    private readonly int[] _source = Enumerable.Range(0, 1_000).ToArray();

    [Benchmark]
    public ImmutableArray<int> CreateSlice() => ImmutableArray.Create(_source, 0, _source.Length);
}
```

This in particular makes larger copies much faster.

| Method      | Runtime   | Mean     | Ratio |
| ----------- | --------- | -------- | ----- |
| CreateSlice | .NET 10.0 | 552.5 ns | 1.00  |
| CreateSlice | .NET 11.0 | 277.7 ns | 0.50  |

[dotnet/runtime#118932](https://github.com/dotnet/runtime/pull/118932) from [@prozolic](https://github.com/prozolic) similarly keeps `ImmutableArrayExtensions.SequenceEqual` on optimized paths when the other sequence is an array, list, or another `ICollection<T>`.

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Running;

using System.Collections.Immutable;
using BenchmarkDotNet.Attributes;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[HideColumns("Job", "Error", "StdDev", "RatioSD", "Median")]
public class Benchmarks
{
    private ImmutableArray<int> _immutable;
    private List<int> _list = [];

    [GlobalSetup]
    public void Setup()
    {
        int[] values = Enumerable.Range(0, 1_000).ToArray();
        _immutable = ImmutableArray.Create(values);
        _list = [.. values];
    }

    [Benchmark]
    public bool SequenceEqual() => _immutable.SequenceEqual(_list);
}
```

| Method        | Runtime   | Mean     | Ratio |
| ------------- | --------- | -------- | ----- |
| SequenceEqual | .NET 10.0 | 925.8 ns | 1.00  |
| SequenceEqual | .NET 11.0 | 122.2 ns | 0.13  |

`Array.FindAll` has the opposite job: it produces a new collection. For a small result, its temporary storage used to cost more than the result itself. [dotnet/runtime#120336](https://github.com/dotnet/runtime/pull/120336) from [@Henr1k80](https://github.com/Henr1k80) has `Array.FindAll` collect its first four matches in an inline stack buffer rather than an intermediate `List<T>`:

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Running;

using BenchmarkDotNet.Attributes;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[MemoryDiagnoser(false), HideColumns("Job", "Error", "StdDev", "RatioSD", "Median")]
public class Benchmarks
{
    private int[] _data = [];

    [Params(4, 5)]
    public int Size { get; set; }

    [GlobalSetup]
    public void Setup() => _data = Enumerable.Range(0, Size).ToArray();

    [Benchmark]
    public int[] FindAllMatch() => Array.FindAll(_data, static _ => true);
}
```

| Method       | Runtime   | Size | Mean      | Ratio | Allocated | Alloc Ratio |
| ------------ | --------- | ---- | --------- | ----- | --------- | ----------- |
| FindAllMatch | .NET 10.0 | 4    | 27.61 ns  | 1.00  | 112 B     | 1.00        |
| FindAllMatch | .NET 11.0 | 4    | 9.212 ns  | 0.33  | 40 B      | 0.36        |
| FindAllMatch | .NET 10.0 | 5    | 36.93 ns  | 1.00  | 176 B     | 1.00        |
| FindAllMatch | .NET 11.0 | 5    | 11.102 ns | 0.30  | 48 B      | 0.27        |

`Dictionary<TKey, TValue>.Remove` had also missed an optimization already used by lookup and insertion. [dotnet/runtime#125884](https://github.com/dotnet/runtime/pull/125884) gives value-type keys a streamlined loop for the common default-comparer case. Because that path doesn’t need a virtual comparer call, the JIT can keep more of the operation’s state in registers; reference-type keys and custom comparers continue to use the general path.

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private readonly Guid[] _keys = Enumerable.Range(0, 512).Select(i => new Guid(i, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0)).ToArray();
    private Dictionary<Guid, int> _dictionary = [];

    [IterationSetup]
    public void Setup() => _dictionary = _keys.ToDictionary(key => key, key => key.GetHashCode());

    [Benchmark(OperationsPerInvoke = 512)]
    public void Remove()
    {
        foreach (Guid key in _keys)
            _dictionary.Remove(key);
    }
}
```

| Method | Runtime   | Mean     | Ratio |
| ------ | --------- | -------- | ----- |
| Remove | .NET 10.0 | 5.285 ns | 1.00  |
| Remove | .NET 11.0 | 4.321 ns | 0.82  |

[dotnet/runtime#125893](https://github.com/dotnet/runtime/pull/125893) changes `HashSet<T>`‘s internal chain walks to test the entry index against the array length with an unsigned comparison. That proves the subsequent array access is in range, allowing the JIT to remove its bounds check.

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private readonly HashSet<int> _set = Enumerable.Range(0, 4096).ToHashSet();
    private readonly int[] _probes = Enumerable.Range(0, 4096).ToArray();

    [Benchmark]
    public int ContainsHits()
    {
        int count = 0;
        foreach (int value in _probes)
            count += _set.Contains(value) ? 1 : 0;

        return count;
    }
}
```

| Method       | Runtime   | Mean     | Ratio |
| ------------ | --------- | -------- | ----- |
| ContainsHits | .NET 10.0 | 7.870 μs | 1.00  |
| ContainsHits | .NET 11.0 | 7.388 μs | 0.94  |

[dotnet/runtime#128988](https://github.com/dotnet/runtime/pull/128988) from [@prozolic](https://github.com/prozolic) removes a second hash-table lookup when removing a matching key-value pair from `OrderedDictionary<TKey, TValue>` through `ICollection<KeyValuePair<TKey, TValue>>`. That interface operation must first find the key and verify that its stored value equals the supplied value. Once both checks have succeeded, the implementation already has the entry index needed for removal. Looking up the key again unnecessarily repeats its hash computation and collision-chain walk, so the updated path removes the known entry directly.

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Running;

using System.Collections.Generic;
using BenchmarkDotNet.Attributes;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[HideColumns("Job", "Error", "StdDev", "RatioSD", "Median")]
public class Benchmarks
{
    private const int N = 10_000;

    private OrderedDictionary<string, int> _dict = [];
    private KeyValuePair<string, int>[] _pairs = Enumerable.Range(0, N)
        .Select(i => new KeyValuePair<string, int>($"key{i}", i))
        .ToArray();

    [IterationSetup]
    public void IterationSetup() => _dict = new OrderedDictionary<string, int>(_pairs);

    [Benchmark]
    public int Remove_ExplicitInterface()
    {
        ICollection<KeyValuePair<string, int>> col = _dict;
        int removed = 0;
        foreach (var pair in _pairs)
            if (col.Remove(pair))
                removed++;

        return removed;
    }
}
```

For 10,000 entries:

| Method                    | Runtime   | Mean     | Ratio |
| ------------------------- | --------- | -------- | ----- |
| Remove\_ExplicitInterface | .NET 10.0 | 220.7 ms | 1.00  |
| Remove\_ExplicitInterface | .NET 11.0 | 179.0 ms | 0.81  |

[dotnet/runtime#122952](https://github.com/dotnet/runtime/pull/122952) goes further when two hash tables have compatible layouts. Normally, `UnionWith` enumerates the source and inserts every element independently, recomputing hashes, checking for duplicates, and potentially resizing the destination along the way. If the destination is empty and both sets use compatible comparers, every source entry is already unique under exactly the equality rules the destination needs. `UnionWith` can therefore use the existing `HashSet<T>` copy-constructor fast path to clone the populated storage rather than rebuilding the same table entry by entry.

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Running;

using System.Collections.Generic;
using System.Linq;
using BenchmarkDotNet.Attributes;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[MemoryDiagnoser(false), HideColumns("Job", "Error", "StdDev", "RatioSD", "Median")]
public class Benchmarks
{
    private readonly HashSet<int> _source = new(Enumerable.Range(0, 4_096));

    [Benchmark]
    public HashSet<int> FreshDestinationUnionWith()
    {
        HashSet<int> destination = [];
        destination.UnionWith(_source);
        return destination;
    }
}
```

| Method                    | Runtime   | Mean      | Ratio | Allocated | Alloc Ratio |
| ------------------------- | --------- | --------- | ----- | --------- | ----------- |
| FreshDestinationUnionWith | .NET 10.0 | 46.207 μs | 1.00  | 252.27 KB | 1.00        |
| FreshDestinationUnionWith | .NET 11.0 | 2.433 μs  | 0.05  | 76.07 KB  | 0.30        |

[dotnet/runtime#128300](https://github.com/dotnet/runtime/pull/128300) from [@AndrewP-GH](https://github.com/AndrewP-GH) also helps with collection construction. Building a `FrozenDictionary<TKey, TValue>` first requires collecting the input elements into a regular `Dictionary<TKey, TValue>` if they’re not already in one. That temporary dictionary resolves duplicate keys before the final frozen representation is chosen, but in .NET 10 it was growing incrementally even when the source’s count was readily available. This PR uses that count as the dictionary’s initial capacity, avoiding repeated allocation, copying, and rehashing as it’s populated.

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0
using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;
using System.Collections.Concurrent;
using System.Collections.Frozen;
using System.Collections.Generic;
using System.Collections.Immutable;
using System.Linq;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[MemoryDiagnoser(false), HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private readonly KeyValuePair<int, int>[] _array =
        Enumerable.Range(0, 4096).Select(i => new KeyValuePair<int, int>(i, i)).ToArray();

    [Benchmark]
    public FrozenDictionary<int, int> FromArray() => _array.ToFrozenDictionary();

}
```

| Method    | Runtime   | Allocated | Alloc Ratio |
| --------- | --------- | --------- | ----------- |
| FromArray | .NET 10.0 | 347.17 KB | 1.00        |
| FromArray | .NET 11.0 | 127.16 KB | 0.37        |

`SetEquals` asks whether two sets contain the same values, regardless of insertion order. The general implementation needs a temporary mutable set so it can account for duplicates and arbitrary enumeration order. When the other input is already a hash set with a compatible comparer, though, that reconstruction is unnecessary. [dotnet/runtime#126309](https://github.com/dotnet/runtime/pull/126309) from [@aw0lid](https://github.com/aw0lid) adds to `ImmutableHashSet<T>.SetEquals` direct zero-allocation paths for compatible `ImmutableHashSet<T>` and `HashSet<T>` inputs; with an identical comparer, the sets can be considered equal if they have the same count and if every element from one is found in the other.

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using System.Collections.Immutable;
using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[MemoryDiagnoser(false)]
[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private ImmutableHashSet<int> _set = ImmutableHashSet<int>.Empty;
    private ImmutableHashSet<int> _immutable = ImmutableHashSet<int>.Empty;
    private HashSet<int> _mutable = [];

    [GlobalSetup]
    public void Setup()
    {
        int[] items = Enumerable.Range(0, 10_000).ToArray();
        _set = ImmutableHashSet.CreateRange(items);
        _immutable = ImmutableHashSet.CreateRange(items);
        _mutable = new(items);
    }

    [Benchmark]
    public bool EqualImmutableHashSet() => _set.SetEquals(_immutable);

    [Benchmark]
    public bool EqualHashSet() => _set.SetEquals(_mutable);
}
```

| Method                | Runtime   | Mean     | Ratio | Allocated | Alloc Ratio |
| --------------------- | --------- | -------- | ----- | --------- | ----------- |
| EqualImmutableHashSet | .NET 10.0 | 775.6 μs | 1.00  | 158.16 KB | 1.00        |
| EqualImmutableHashSet | .NET 11.0 | 559.7 μs | 0.72  | –         | 0           |
| EqualHashSet          | .NET 10.0 | 478.6 μs | 1.00  | 157.99 KB | 1.00        |
| EqualHashSet          | .NET 11.0 | 230.9 μs | 0.48  | –         | 0           |

Sorted sets have a related case. `SetEquals` can be passed any `IEnumerable<T>`. That sequence might be unordered and might contain duplicate values, so `ImmutableSortedSet<T>` previously copied it into a temporary `SortedSet<T>` before performing the comparison. However, when the input is another sorted set using the same ordering comparer, both sets contain unique values and enumerate those values in the same order. Equality can then be determined by first comparing their counts and, if those match, advancing both enumerators together. The first unequal pair proves the sets are different, and reaching the end without finding a difference proves they’re equal. [dotnet/runtime#126549](https://github.com/dotnet/runtime/pull/126549) from [@aw0lid](https://github.com/aw0lid) recognizes this case for `ImmutableSortedSet<T>`, avoiding the temporary `SortedSet<T>` and comparing the two sorted sequences directly in one linear pass:

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Running;

using System.Collections.Generic;
using System.Collections.Immutable;
using BenchmarkDotNet.Attributes;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[MemoryDiagnoser(false), HideColumns("Job", "Error", "StdDev", "RatioSD", "Median")]
public class Benchmarks
{
    private const int N = 10_000;
    private ImmutableSortedSet<int> _set = ImmutableSortedSet<int>.Empty;
    private ImmutableSortedSet<int> _equalSet = ImmutableSortedSet<int>.Empty;

    [GlobalSetup]
    public void Setup()
    {
        var items = new int[N];
        for (int i = 0; i < N; i++) items[i] = i;
        _set = ImmutableSortedSet.CreateRange(items);
        _equalSet = ImmutableSortedSet.CreateRange(items);
    }

    [Benchmark]
    public bool SetEquals_EqualImmutableSortedSet() => _set.SetEquals(_equalSet);
}
```

For 10,000 elements:

| Method                             | Runtime   | Mean     | Ratio | Allocated | Alloc Ratio |
| ---------------------------------- | --------- | -------- | ----- | --------- | ----------- |
| SetEquals\_EqualImmutableSortedSet | .NET 10.0 | 767.7 μs | 1.00  | 430.02 KB | 1.00        |
| SetEquals\_EqualImmutableSortedSet | .NET 11.0 | 117.6 μs | 0.15  | –         | 0           |

`SortedSet<T>` already enjoyed an optimization for that case in .NET 10, but it’s not left out of .NET 11 improvements. `SortedSet<T>.GetViewBetween` returns a `SortedSet<T>` view, effectively a slice of another `SortedSet<T>`, a live window onto a range of another set: changes through the view affect the original set. Clearing a view therefore can’t replace the view with an empty collection; it must find and remove every original node in that range. [dotnet/runtime#126410](https://github.com/dotnet/runtime/pull/126410) from [@prozolic](https://github.com/prozolic) reduces the temporary storage used for that operation. The implementation pre-sizes the list of elements to remove and walks it by index rather than repeatedly removing from and shrinking the temporary list.

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Running;

using BenchmarkDotNet.Attributes;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[MemoryDiagnoser(false), HideColumns("Job", "Error", "StdDev", "RatioSD", "Median")]
public class Benchmarks
{
    private const int N = 10_000;
    private SortedSet<int> _fullSet = [];

    [IterationSetup]
    public void Setup() => _fullSet = new SortedSet<int>(Enumerable.Range(0, N));

    [Benchmark]
    public int GetViewBetweenThenClear()
    {
        SortedSet<int> view = _fullSet.GetViewBetween(0, N - 1);
        view.Clear();
        return _fullSet.Count;
    }
}
```

| Method                  | Runtime   | Allocated | Alloc Ratio |
| ----------------------- | --------- | --------- | ----------- |
| GetViewBetweenThenClear | .NET 10.0 | 193.15 KB | 1.00        |
| GetViewBetweenThenClear | .NET 11.0 | 103.93 KB | 0.54        |

Collections are frequently consumed through LINQ. Although its operators work in terms of the general `IEnumerable<T>` abstraction, LINQ’s internal iterators can preserve useful facts about their sources and the operations already applied. Those facts can sometimes answer a query without enumerating the source at all.

For example, consider `source.Append(x).Skip(10).LastOrDefault()`. LINQ queries are lazy, so the actual search begins only when `LastOrDefault` asks the `Skip` iterator for its last element. If `source.Append(x)` contains ten or fewer elements, `Skip(10)` necessarily removes all of them, leaving an empty sequence from which `LastOrDefault` must return the default value. `Append`, `Prepend`, and `Concat` iterators can cheaply report their total count when their underlying sources can do so. [dotnet/runtime#123306](https://github.com/dotnet/runtime/pull/123306) from [@prozolic](https://github.com/prozolic) teaches the last-element path for `Skip` to compare that count with the number being skipped and immediately report that there is no element, rather than searching a sequence it already knows is empty.

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Running;

using System.Linq;
using BenchmarkDotNet.Attributes;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[MemoryDiagnoser(false), HideColumns("Job", "Error", "StdDev", "RatioSD", "Median")]
public class Benchmarks
{
    private readonly int[] _source = [1, 2, 3, 4, 5];

    [Benchmark]
    public int AppendSkipLastOrDefault() => _source.Append(6).Skip(10).LastOrDefault();
}
```

| Method                  | Runtime   | Mean     | Ratio | Allocated | Alloc Ratio |
| ----------------------- | --------- | -------- | ----- | --------- | ----------- |
| AppendSkipLastOrDefault | .NET 10.0 | 44.89 ns | 1.00  | 144 B     | 1.00        |
| AppendSkipLastOrDefault | .NET 11.0 | 16.32 ns | 0.36  | 112 B     | 0.78        |

.NET 11 also improve’s LINQ’s `Enumerable.Sum`. `Sum` already uses SIMD. The main loop processes four vectors at a time, alternating between two accumulators so that the additions don’t require extra moves. However, `Sum` also promises to throw if the result overflows. Alongside each vector addition, the implementation uses the signs of the two inputs and the result to update another vector that tracks whether any lane overflowed. After every group of four vectors, the loop tests that tracking vector and branches to the throwing path if needed. Overflow is rare, though, so on the common path that test and branch almost always just confirm that nothing happened. [dotnet/runtime#127429](https://github.com/dotnet/runtime/pull/127429) removes that repeated work in .NET 11. It accumulates the overflow information across all of the vector processing and tests it once after the vector loops have completed. The checked-overflow behavior remains the same, but the normal path no longer needs to stop and check after every four vectors. The PR also simplifies how the method walks the input, replacing unsafe reference and index arithmetic with span-based vector loads, progressively slicing off the elements already processed, and using a `foreach` for the final scalar elements.

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Running;

using System.Linq;
using BenchmarkDotNet.Attributes;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[HideColumns("Job", "Error", "StdDev", "RatioSD", "Median")]
public class Benchmarks
{
    private const int N = 32;
    private int[] _intData = [];

    [GlobalSetup]
    public void Setup()
    {
        Random rng = new(42);
        _intData = Enumerable.Range(0, N).Select(_ => rng.Next(-1_000, 1_000)).ToArray();
    }

    [Benchmark]
    public int SumInt() => _intData.Sum();
}
```

| Method | Runtime   | Mean     | Ratio |
| ------ | --------- | -------- | ----- |
| SumInt | .NET 10.0 | 5.609 ns | 1.00  |
| SumInt | .NET 11.0 | 4.731 ns | 0.84  |

`Enumerable`‘s `Min` and `Max` already examined many values at once with SIMD, but they still finished `byte`, `sbyte`, `short`, and `ushort` inputs by copying the final vector to the stack and checking its values one by one. [dotnet/runtime#127995](https://github.com/dotnet/runtime/pull/127995) keeps that final step in vector instructions, using shuffles to combine the lanes. Smaller element types pack more values into each vector, so they benefit most from no longer finishing the search one value at a time.

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private byte[] _bytes = [];

    [Params(16, 64)]
    public int Length { get; set; }

    [GlobalSetup]
    public void Setup() => _bytes = Enumerable.Range(0, Length).Select(i => (byte)i).ToArray();

    [Benchmark]
    public byte MaxByte() => _bytes.Max();
}
```

| Method  | Runtime   | Length | Mean     | Ratio |
| ------- | --------- | ------ | -------- | ----- |
| MaxByte | .NET 10.0 | 16     | 7.546 ns | 1.00  |
| MaxByte | .NET 11.0 | 16     | 2.176 ns | 0.29  |
| MaxByte | .NET 10.0 | 64     | 7.827 ns | 1.00  |
| MaxByte | .NET 11.0 | 64     | 2.059 ns | 0.26  |

Since its inception, LINQ has had `Join` and `GroupJoin`, and .NET 10 introduced the long-requested `LeftJoin` and `RightJoin`. In .NET 11, [dotnet/runtime#127236](https://github.com/dotnet/runtime/pull/127236) adds `FullJoin`. The operators differ in which unmatched elements they retain and how they represent the matches:

- `Join` emits only pairs whose keys match.
- `GroupJoin` emits every left element together with a sequence containing its matching right elements; that sequence is empty when there are no matches.
- `LeftJoin` emits the matching pairs and also unmatched left elements, paired with a default value for the right.
- `RightJoin` does the inverse, emitting the matching pairs and also unmatched right elements, paired with a default value for the left.
- `FullJoin` emits the matching pairs and the unmatched elements from both inputs, using a default value for whichever side is missing.

Before .NET 11, applications typically approximated it by combining `GroupJoin`, `SelectMany`, and `Concat`, then searching the first input again to find right-side elements without a match. The built-in operator avoids both that composition of iterators and the repeated search.

```
// dotnet run -c Release -f net11.0 --filter "*"

using BenchmarkDotNet.Running;

using System.Collections.Generic;
using System.Linq;
using BenchmarkDotNet.Attributes;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[MemoryDiagnoser(false), HideColumns("Job", "Error", "StdDev", "RatioSD", "Median")]
public class Benchmarks
{
    private const int N = 10_000;

    private List<(int Id, string Name)> _left = Enumerable.Range(0, N).Select(i => (i, $"item{i}")).ToList();
    private List<(int Id, decimal Amount)> _right = Enumerable.Range(N / 4, N).Select(i => (i, (decimal)i * 1.5m)).ToList();

    [Benchmark(Baseline = true)]
    public int FullJoin_Manual() =>
        _left.GroupJoin(_right, l => l.Id, r => r.Id, (l, rs) => (l, rs))
             .SelectMany(x => x.rs.DefaultIfEmpty(), (x, r) => (x.l, r))
             .Concat(_right
                 .Where(r => !_left.Any(l => l.Id == r.Id))
                 .Select(r => (l: default((int Id, string Name)), r)))
             .Count();

    [Benchmark]
    public int FullJoin_New() => _left.FullJoin(_right, l => l.Id, r => r.Id).Count();
}
```

For 10,000 elements in each input:

| Method           | Mean      | Ratio | Allocated | Alloc Ratio |
| ---------------- | --------- | ----- | --------- | ----------- |
| FullJoin\_Manual | 27.615 ms | 1.00  | 3.23 MB   | 1.00        |
| FullJoin\_New    | 1.702 ms  | 0.06  | 1.56 MB   | 0.48        |

The earlier `Skip` example showed how many LINQ optimizations come from one operator flowing information to subsequent operators that can then be used for additional optimization. This is typically done by adding that additional information to properties on the concrete internal `IEnumerable<T>` implementations used by `System.Linq`. Synchronous `Enumerable` has accumulated many such specialized iterators over many releases, focusing in particular on places where algorithmic complexity could be significantly reduced. `AsyncEnumerable`, introduced “in the box” in .NET 10, initially had much less of that machinery; for asynchronous sequences dominated by I/O, it often wouldn’t matter.

Concatenation is an important exception. Prior to .NET 11, every call to `AsyncEnumerable.Append` created a new iterator around the sequence produced by the previous call. Consider a chain with just three appended values:

```
var sequence = AsyncEnumerable.Empty<int>()
    .Append(0)
    .Append(1)
    .Append(2);
```

To produce `0`, enumeration needs to pass through all three nested iterators. Producing `1` passes through two, and producing `2` passes through one. Thus, yielding three values involves roughly `3 + 2 + 1` iterator steps. With 1,000 appends, that grows to `1,000 + 999 + ... + 1`, or approximately 500,000 steps, rather than approximately 1,000. In general, enumerating N values requires O(N^2) work. Several years back `Enumerable` addressed this by special-casing the various concatenation enumerables to flow enough information through to make iterating the chain O(N) rather than O(N^2), and in .NET 11, [dotnet/runtime#122389](https://github.com/dotnet/runtime/pull/122389) applies that to `AsyncEnumerable` as well. The operators accumulate the extra elements or sequences in one flat representation instead of adding another wrapper for each LINQ operator.

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Running;

using System.Linq;
using System.Threading.Tasks;
using BenchmarkDotNet.Attributes;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[MemoryDiagnoser(false), HideColumns("Job", "Error", "StdDev", "RatioSD", "Median")]
public class Benchmarks
{
    [Benchmark]
    public async Task<int> AppendChain()
    {
        var seq = AsyncEnumerable.Empty<int>();
        for (int i = 0; i < 1_000; i++) seq = seq.Append(i);
        return await seq.SumAsync();
    }
}
```

For a chain of 1,000 appended elements:

| Method      | Runtime   | Mean     | Ratio   | Allocated | Alloc Ratio |
| ----------- | --------- | -------- | ------- | --------- | ----------- |
| AppendChain | .NET 10.0 | 8.253 ms | 1.00    | 187.57 KB | 1.00        |
| AppendChain | .NET 11.0 | 28.49 μs | 0.00345 | 136.77 KB | 0.73        |

## I/O

I/O performance is often equated with the speed of the underlying device, but the transfer itself is only one part of an operation. A cached file read may complete in microseconds, many redirected pipes may be active concurrently, and compression may operate entirely on data already in memory. In cases like these, the managed overhead around the operation can be as important as the time spent moving the data.

That overhead includes setting up the appropriate synchronous or asynchronous OS mechanism, keeping state alive until an operation completes, allocating and copying temporary buffers, and adapting between the caller’s data and stream-based APIs. .NET 11 removes work from each of these layers.

On Windows, “overlapped I/O” is the asynchronous model in which an operation begins now and the operating system posts its completion later. Any time .NET performs I/O as part of an asynchronous operation on Windows, it strives to use a corresponding overlapped I/O API rather than using a synchronous API asynchronously (i.e. queuing a work item that blocks a thread pool thread doing the I/O). However, there have been some stragglers. Redirected child-process output previously used synchronous pipe handles, so `ReadToEndAsync` on the stream from a `Process`‘s stdout or stderr `Stream` still needed a thread-pool thread blocked in a native read for each stdout or stderr pipe. In .NET 11, [dotnet/runtime#125643](https://github.com/dotnet/runtime/pull/125643) instead opens the parent’s stdout and stderr ends for overlapped reads, while leaving the child ends synchronous (as console applications expect).

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using System.Diagnostics;
using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Jobs;
using BenchmarkDotNet.Running;

if (args is ["--emit"])
{
    Console.Write(new string('x', 8 * 1024 * 1024));
    return;
}

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[SimpleJob(launchCount: 1, warmupCount: 3, iterationCount: 10)]
[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    [Benchmark]
    public Task ReadOutputConcurrently() =>
        Task.WhenAll(Enumerable.Range(0, 16).Select(_ => RunProcess()));

    private static async Task RunProcess()
    {
        var psi = new ProcessStartInfo("dotnet")
        {
            RedirectStandardOutput = true,
            UseShellExecute = false,
        };
        psi.ArgumentList.Add(typeof(Benchmarks).Assembly.Location);
        psi.ArgumentList.Add("--emit");

        using Process process = Process.Start(psi)!;
        _ = await process.StandardOutput.ReadToEndAsync();
        await process.WaitForExitAsync();
    }
}
```

| Method                 | Runtime   | Mean     | Ratio |
| ---------------------- | --------- | -------- | ----- |
| ReadOutputConcurrently | .NET 10.0 | 587.8 ms | 1.00  |
| ReadOutputConcurrently | .NET 11.0 | 541.8 ms | 0.92  |

We often refer to the mechanism being fixed as “async over sync.” The opposite case, “sync over async,” can be even worse, as it means blocking one thread while waiting for another to do some work; that provides one of the necessary ingredients for cycles and deadlocks, and is a leading cause of scalability bottlenecks in services, so we try to stamp out “sync over async” whenever possible. In cases where we can’t avoid it, though, we can at least make it better.

`RandomAccess.Read` provides one such opportunity when it’s used with a Windows file handle opened for asynchronous I/O. Windows requires specifying at the time of file opening whether I/O will be overlapped or not, and if it is, Windows still requires a read to use its `OVERLAPPED` mechanism, even in a synchronous `Read` case where the API’s caller is going to block until that read completes. While we can’t avoid that overlapped I/O, we can still make the operation cheaper. Previously, .NET both gave the operation an event for the calling thread to wait on and registered an I/O-completion callback. Instead, [dotnet/runtime#126845](https://github.com/dotnet/runtime/pull/126845) uses a documented Windows convention: setting the low bit of `OVERLAPPED.hEvent` instructs Windows to signal the event when the operation completes but not to also queue a completion packet to the I/O completion port. The calling thread can wait on an event cached by the file handle, retrieve the result, and perform the cleanup itself. This removes the callback, its coordination, and the per-operation allocation while still performing the same synchronous wait.

```
// Windows:
// dotnet run -c Release -f net10.0 --filter "*RandomAccessBenchmarks*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;
using Microsoft.Win32.SafeHandles;

BenchmarkSwitcher.FromAssembly(typeof(RandomAccessBenchmarks).Assembly).Run(args);

[MemoryDiagnoser(false), HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class RandomAccessBenchmarks
{
    private string _path = "";
    private SafeFileHandle _handle = null!;
    private readonly byte[] _buffer = new byte[4_096];

    [GlobalSetup]
    public void Setup()
    {
        _path = Path.Combine(Path.GetTempPath(), $"net11-random-access-{Guid.NewGuid():N}.tmp");
        File.WriteAllBytes(_path, new byte[1024 * 1024]);
        _handle = File.OpenHandle(_path, FileMode.Open, FileAccess.Read, FileShare.Read, FileOptions.Asynchronous | FileOptions.RandomAccess);
    }

    [GlobalCleanup]
    public void Cleanup()
    {
        _handle.Dispose();
        File.Delete(_path);
    }

    [Benchmark]
    public int Read4K() => RandomAccess.Read(_handle, _buffer, fileOffset: 0);
}
```

| Method | Runtime   | Mean     | Ratio | Allocated | Alloc Ratio |
| ------ | --------- | -------- | ----- | --------- | ----------- |
| Read4K | .NET 10.0 | 4.564 μs | 1.00  | 176 B     | 1.00        |
| Read4K | .NET 11.0 | 2.747 μs | 0.60  | –         | 0           |

There are smaller allocation wins at higher layers as well. For example, [dotnet/runtime#121508](https://github.com/dotnet/runtime/pull/121508) makes assigning `TextWriter.NewLine` to its existing value a no-op and shares arrays for the standard `"\n"` and `"\r\n"` values, avoiding a fresh `char[]` conversion.

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[MemoryDiagnoser(false), HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private readonly StringWriter _writer = new();

    [Benchmark]
    public void SetSameValue() => _writer.NewLine = Environment.NewLine;
}
```

| Method       | Runtime   | Mean     | Ratio | Allocated |
| ------------ | --------- | -------- | ----- | --------- |
| SetSameValue | .NET 10.0 | 8.281 ns | 1.00  | 32 B      |
| SetSameValue | .NET 11.0 | 2.168 ns | 0.26  | –         |

Archive and file APIs remove similar temporary allocations. A GNU tar header has fixed-size fields for an entry’s name and link target. When either doesn’t fit, `TarWriter` emits an additional metadata record containing the long value. In .NET 10, `TarWriter` first encoded that value into a newly allocated byte array, then wrote the bytes and a null terminator into a new `MemoryStream`. Because the stream didn’t know the final size, it allocated and grew its own backing array as the data was written. With [dotnet/runtime#123835](https://github.com/dotnet/runtime/pull/123835), .NET 11 instead computes the exact UTF-8 size including the terminator, allocates one array of that size, encodes directly into it, and constructs the `MemoryStream` over that array. This removes both the temporary encoded array and the stream’s growth and copying.

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using System.Formats.Tar;
using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[MemoryDiagnoser(false)]
[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private readonly MemoryStream _destination = new();
    private readonly GnuTarEntry _entry = new(TarEntryType.RegularFile, new string('a', 256));

    [Benchmark]
    public long WriteLongName()
    {
        _destination.SetLength(0);
        using TarWriter writer = new(_destination, TarEntryFormat.Gnu, leaveOpen: true);
        writer.WriteEntry(_entry);
        return _destination.Length;
    }
}
```

| Method        | Runtime   | Mean     | Ratio | Allocated | Alloc Ratio |
| ------------- | --------- | -------- | ----- | --------- | ----------- |
| WriteLongName | .NET 10.0 | 731.1 ns | 1.00  | 1.36 KB   | 1.00        |
| WriteLongName | .NET 11.0 | 616.7 ns | 0.84  | 608 B     | 0.44        |

`ZipArchive` similarly eliminates temporary buffers. ZIP archives end with a central directory describing their entries. Reading that directory allocated a new 4 KB buffer for every `ZipArchive`, along with additional arrays in a few paths that needed to combine or slice data. With [dotnet/runtime#123836](https://github.com/dotnet/runtime/pull/123836), .NET 11 now rents the central-directory buffer from `ArrayPool<byte>` and uses spans and memory in place of the additional arrays and several open-coded loops.

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using System.IO.Compression;
using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[MemoryDiagnoser(false)]
[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private byte[] _archive = [];

    [GlobalSetup]
    public void Setup()
    {
        using MemoryStream destination = new();
        using (ZipArchive archive = new(destination, ZipArchiveMode.Create, leaveOpen: true))
        {
            ZipArchiveEntry entry = archive.CreateEntry("entry.txt");
            using Stream stream = entry.Open();
            stream.WriteByte(42);
        }

        _archive = destination.ToArray();
    }

    [Benchmark]
    public int ReadCentralDirectory()
    {
        using MemoryStream source = new(_archive, writable: false);
        using ZipArchive archive = new(source, ZipArchiveMode.Read);
        return archive.Entries.Count;
    }
}
```

| Method               | Runtime   | Mean     | Ratio | Allocated | Alloc Ratio |
| -------------------- | --------- | -------- | ----- | --------- | ----------- |
| ReadCentralDirectory | .NET 10.0 | 553.5 ns | 1.00  | 5.05 KB   | 1.00        |
| ReadCentralDirectory | .NET 11.0 | 258.1 ns | 0.47  | 1.13 KB   | 0.22        |

As a final example, `FileInfo.MoveTo` performs a source-directory existence check before moving the file. It had been constructing a `DirectoryInfo` solely to read its `Exists` property, which is wasteful when `Directory.Exists` exists and can do it without the allocation. [dotnet/runtime#123893](https://github.com/dotnet/runtime/pull/123893) in .NET 11 switches to use that.

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[MemoryDiagnoser(false)]
[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private string _directory = "";
    private string _firstPath = "";
    private string _secondPath = "";
    private FileInfo _file = null!;
    private bool _atFirstPath;

    [GlobalSetup]
    public void Setup()
    {
        _directory = Path.Combine(Path.GetTempPath(), $"net11-file-move-{Guid.NewGuid():N}");
        Directory.CreateDirectory(_directory);
        _firstPath = Path.Combine(_directory, "first.tmp");
        _secondPath = Path.Combine(_directory, "second.tmp");
        File.WriteAllBytes(_firstPath, [42]);
        _file = new(_firstPath);
        _atFirstPath = true;
    }

    [GlobalCleanup]
    public void Cleanup() => Directory.Delete(_directory, recursive: true);

    [Benchmark]
    public string MoveTo()
    {
        _file.MoveTo(_atFirstPath ? _secondPath : _firstPath, overwrite: true);
        _atFirstPath = !_atFirstPath;
        return _file.FullName;
    }
}
```

| Method | Runtime   | Allocated | Alloc Ratio |
| ------ | --------- | --------- | ----------- |
| MoveTo | .NET 10.0 | 332 B     | 1.00        |
| MoveTo | .NET 11.0 | 236 B     | 0.71        |

.NET provides great high-level abstractions for working with all manner of data and I/O. One of the most prominent is `Stream`, which provides a simple, flexible mechanism for reading and writing many different data sources and formats: `MemoryStream`, `FileStream`, `CryptoStream`, `ZLibStream`, `SslStream`, and on and on. For folks that really care about maximizing performance, however, sometimes you want to go a bit lower-level and deal directly with the underlying primitives. For example, the various compression streams, `ZLibStream`, `DeflateStream`, `GZipStream`, and `BrotliStream`, all maintain buffers that hold input and output data waiting to be read or written. But what if the caller already owns its input and output buffers, or wants to use a pool for them? In .NET 11, [dotnet/runtime#123145](https://github.com/dotnet/runtime/pull/123145) exposes the underlying `DeflateEncoder`/`DeflateDecoder`, `ZLibEncoder`/`ZLibDecoder`, and `GZipEncoder`/`GZipDecoder` types, following the existing `BrotliEncoder` and `BrotliDecoder` pattern. The stream types are wrappers around these encoders and decoders, and in .NET 11 you can now use them directly. They support chunked `Compress`, `Decompress`, and `Flush` operations as well as one-shot `TryCompress` and `TryDecompress`, enabling callers to supply and reuse their own buffers rather than going through adapter streams and their buffers.

## Networking

Networking is the bread-and-butter of many applications and sits directly on the hot path of scalable services. Improvements throughout the stack add up quickly.

At the bottom of the stack, connections and sockets establish and carry the byte stream. A socket doesn’t actually connect to a host name; it connects to an IP address and port. Resolving a host name may produce multiple candidate addresses, including one or more IPv4 addresses from DNS A records and one or more IPv6 addresses from AAAA records. When `SocketAsyncEventArgs.RemoteEndPoint` is a `DnsEndPoint`, the existing `Socket.ConnectAsync` implementation performs that resolution and tries the resulting addresses in sequence. It starts a connection to the first address, and only if that attempt fails does it move on to the next. This works well when the first address is reachable. A failed TCP connection isn’t always reported quickly, however. If packets sent over that route are simply dropped, the attempt may remain pending until a timeout even though another address for the same host could have connected immediately.

This problem is especially visible on machines with both IPv4 and IPv6. Clients generally want to prefer IPv6 when it works, but a broken or misconfigured IPv6 path can make an application wait through a long timeout before trying IPv4. The technique commonly known as [Happy Eyeballs](https://www.rfc-editor.org/rfc/rfc8305.html) addresses this by overlapping connection attempts. Rather than putting all the latency of one candidate in front of the next, it starts another attempt after a short delay and uses the first connection that succeeds. The remaining attempts are then canceled or discarded. That consumes some additional resources, but it can greatly reduce the long tail of connection establishment.

In .NET 11, [dotnet/runtime#106374](https://github.com/dotnet/runtime/pull/106374) adds an opt-in, Happy-Eyeballs-like strategy to the static `Socket.ConnectAsync` overload that accepts a `SocketAsyncEventArgs`. The new overload accepts a `ConnectAlgorithm`, where `ConnectAlgorithm.Default` preserves the existing sequential behavior, while `ConnectAlgorithm.Parallel` requests the new strategy. When parallel connection is requested for an address-family-unspecified `DnsEndPoint` on a machine that supports both IPv4 and IPv6, .NET starts separate IPv4 and IPv6 DNS queries and runs a connection loop for each family concurrently. Addresses within each family are still tried sequentially, but the two families no longer wait on each other. The first successful connection becomes the `ConnectSocket`, and a connection subsequently established by the other family is disposed. If one family fails, the other is allowed to continue; the operation reports failure only after neither can connect. Parallel mode can briefly establish two connections, while the default remains cheaper when the first candidate connects promptly.

Given the nature of the change, it’s a little hard to create a real benchmark for this, but we can get creative. Here I’ve created IPv4 and IPv6 listeners on the same port, but arranged the benchmark to only accept from the IPv4 listener. On my machine, `localhost` resolves to `::1` before `127.0.0.1`, so the client sockets by default would first try the IPv6 address and only when it fails try the IPv4 one. The benchmark setup fills the IPv6 listener’s accept backlog, such that additional client connect requests will stall.

```
// Windows
// dotnet run -c Release -f net11.0 --filter "*"

using System.Net;
using System.Net.Sockets;
using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Engines;
using BenchmarkDotNet.Running;

BenchmarkRunner.Run<Benchmarks>();

[SimpleJob(RunStrategy.Throughput, launchCount: 1, warmupCount: 2, iterationCount: 8, invocationCount: 1)]
[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private Socket _ipv6Listener = null!;
    private Socket _ipv4Listener = null!;
    private List<Socket> _backlogClients = [];
    private CancellationTokenSource _cancellation = null!;
    private Task _ipv4AcceptLoop = null!;
    private Task? _releaseOne;
    private int _port;

    [IterationSetup(Target = nameof(Default))]
    public void SetupDefault() => Setup(releaseIPv6: true);

    [IterationSetup(Target = nameof(Parallel))]
    public void SetupParallel() => Setup(releaseIPv6: false);

    [IterationCleanup]
    public void Cleanup()
    {
        _releaseOne?.GetAwaiter().GetResult();
        _cancellation.Cancel();
        _ipv4Listener.Dispose();
        _ipv6Listener.Dispose();

        try
        {
            _ipv4AcceptLoop.GetAwaiter().GetResult();
        }
        catch { }

        foreach (Socket socket in _backlogClients)
        {
            socket.Dispose();
        }

        _cancellation.Dispose();
    }

    [Benchmark(Baseline = true)]
    public async Task Default()
    {
        using Socket socket = await ConnectAsync(ConnectAlgorithm.Default);
    }

    [Benchmark]
    public async Task Parallel()
    {
        using Socket socket = await ConnectAsync(ConnectAlgorithm.Parallel);
    }

    private void Setup(bool releaseIPv6)
    {
        if (Dns.GetHostAddresses("localhost")[0].AddressFamily != AddressFamily.InterNetworkV6)
        {
            throw new InvalidOperationException("This benchmark requires localhost to prefer IPv6.");
        }

        _ipv6Listener = new(AddressFamily.InterNetworkV6, SocketType.Stream, ProtocolType.Tcp)
        {
            DualMode = false,
        };
        _ipv6Listener.Bind(new IPEndPoint(IPAddress.IPv6Loopback, 0));
        _port = ((IPEndPoint)_ipv6Listener.LocalEndPoint!).Port;
        _ipv6Listener.Listen(1);

        _ipv4Listener = new(AddressFamily.InterNetwork, SocketType.Stream, ProtocolType.Tcp);
        _ipv4Listener.Bind(new IPEndPoint(IPAddress.Loopback, _port));
        _ipv4Listener.Listen(128);
        _cancellation = new();
        _ipv4AcceptLoop = AcceptLoopAsync(_ipv4Listener, _cancellation.Token);

        _backlogClients = [];
        bool stalled = false;
        for (int i = 0; i < 128; i++)
        {
            Socket socket = new(AddressFamily.InterNetworkV6, SocketType.Stream, ProtocolType.Tcp);
            Task connect = socket.ConnectAsync(new IPEndPoint(IPAddress.IPv6Loopback, _port));

            if (!connect.Wait(TimeSpan.FromMilliseconds(100)))
            {
                socket.Dispose();
                stalled = true;
                break;
            }

            connect.GetAwaiter().GetResult();
            _backlogClients.Add(socket);
        }

        if (!stalled)
        {
            throw new InvalidOperationException("Unable to saturate the IPv6 accept backlog.");
        }

        _releaseOne = releaseIPv6 ?
            Task.Run(async () =>
            {
                await Task.Delay(100);
                using Socket socket = await _ipv6Listener.AcceptAsync();
            }) :
            null;
    }

    private Task<Socket> ConnectAsync(ConnectAlgorithm algorithm)
    {
        TaskCompletionSource<Socket> completion = new(TaskCreationOptions.RunContinuationsAsynchronously);
        var args = new SocketAsyncEventArgs
        {
            RemoteEndPoint = new DnsEndPoint("localhost", _port),
        };

        args.Completed += Complete;
        if (!Socket.ConnectAsync(SocketType.Stream, ProtocolType.Tcp, args, algorithm))
        {
            Complete(null, args);
        }

        return completion.Task;

        void Complete(object? sender, SocketAsyncEventArgs e)
        {
            e.Completed -= Complete;
            if (e.SocketError == SocketError.Success)
            {
                completion.SetResult(e.ConnectSocket!);
            }
            else
            {
                completion.SetException(new SocketException((int)e.SocketError));
            }

            e.Dispose();
        }
    }

    private static async Task AcceptLoopAsync(Socket listener, CancellationToken cancellationToken)
    {
        while (true)
        {
            using Socket socket = await listener.AcceptAsync(cancellationToken);
        }
    }
}
```

With this setup, the parallel algorithm isn’t held up by the stalled IPv6 attempt. It connects to the IPv4 listener in just over a millisecond, whereas the default algorithm spends approximately half a second waiting for the IPv6 connection to make progress:

| Method   | Mean       | Ratio |
| -------- | ---------- | ----- |
| Default  | 511.054 ms | 1.000 |
| Parallel | 1.060 ms   | 0.002 |

A bit synthetic, but it conveys the idea.

Another interesting improvement around sockets has to do with `Socket.Blocking`. “Berkeley sockets”, which is what all modern stacks implement, implement the notion of blocking / non-blocking modes. Typically by default, as is the case with .NET, sockets are in blocking mode. That means, for example, a `recv()` / `Socket.Receive` operation will synchronously block until data is available (or the socket closes). The other option is non-blocking; a socket in non-blocking mode will always return immediately from a `recv` operation, regardless of whether there’s data to read or not. If the operation would have blocked in blocking mode, in non-blocking mode it’ll instead return an error code, EAGAIN or EWOULDBLOCK, which the consuming application can then use to, for example, decide to try again later.

Enter .NET asynchronous operations. On Windows, the Windows sockets APIs provided overlapped APIs that the .NET `Socket` APIs can and do employ. But on Unix, we have the standard `recv` and friends functions. We also have mechanisms like `epoll` (Linux) and `kqueue` (macOS) that let us efficiently and synchronously wait for large numbers of file descriptors to have some activity. As such, on Unix, .NET implements asynchronous socket operations by putting a `Socket` into a non-blocking state, trying the synchronous operation (e.g. `recv` for a `Socket.ReceiveAsync`), and then if the operation couldn’t complete yet and we get back an EAGAIN/EWOULDBLOCK, data about the operation gets queued into `epoll`/`kqueue`-based machinery that will signal when the operation should be retried. This is very similar conceptually to how overlapped I/O works on Windows with I/O completion ports.

Now here’s the rub. To implement asynchronous operations on sockets, we need to flip the socket into non-blocking mode… what do we then do if, say, someone does `Socket.ReceiveAsync` but then follows that up with `Socket.Send`. The socket was flipped into non-blocking mode for the first operation… do we flip it back for the second? It turns out that’s really risky to do, with race conditions making it hard and expensive to get right due to multi-threaded use (expensive because we’d need extra synchronization). When first bringing up .NET on Linux, we made the decision that the flip would be a one-way trip: once non-blocking, always non-blocking. We flip the first time an asynchronous operation is performed, and we leave it there.

What, then, do we do if someone does in fact issue a synchronous `Receive`/`Send` after it’s already been flipped? We simulate the blocking ourselves with sync over async, basically doing the asynchronous operation and blocking on it to complete. Internally we’re able to do it cheaper than actually creating a task and blocking on it, but as a mechanism it’s basically the same.

We were comfortable with this approach in the early days on the theory that if someone starts using asynchronous operations, they’re likely to continue to, and for the odd synchronous operation here and there after that, it’s not a big deal. That has largely proven out over the many years since… except for one case.

Turns out in some systems it’s reasonably common for the initial connect to be asynchronous but then followed only by synchronous sends and receives. This ends up paying that overhead on all operations: you do `ConnectAsync`, we flip the socket to be non-blocking, and then every `Receive`/`Send` after that ends up paying the emulation costs. But there’s good news. It turns out this is also a case where we can easily and safely flip back: we can flip back to blocking before handing back control from `ConnectAsync`. For the static `ConnectAsync` overloads, the caller won’t even have a reference to the connected `Socket` until `ConnectAsync` gives it to them, and for the instance overloads, it’s defined to be erroneous to use such send/receive operations on the `Socket` concurrent with `ConnectAsync`. As such, in all cases, we can just flip it back to blocking before completing the task representing the operation. That’s exactly what [dotnet/runtime#124200](https://github.com/dotnet/runtime/pull/124200) does now in .NET 11.

```
// Linux:
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using System.Net;
using System.Net.Sockets;
using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private const int OperationsPerInvoke = 1_000;
    private readonly byte[] _buffer = new byte[1];
    private Socket _listener = null!;
    private Socket _client = null!;
    private Socket _server = null!;
    private Task _echoLoop = null!;

    [GlobalSetup]
    public async Task Setup()
    {
        _listener = new(AddressFamily.InterNetwork, SocketType.Stream, ProtocolType.Tcp);
        _listener.Bind(new IPEndPoint(IPAddress.Loopback, 0));
        _listener.Listen(1);

        Task<Socket> accept = _listener.AcceptAsync();
        _client = new(AddressFamily.InterNetwork, SocketType.Stream, ProtocolType.Tcp);
        await _client.ConnectAsync(_listener.LocalEndPoint!);
        _server = await accept;
        _echoLoop = Task.Run(EchoLoop);
    }

    [GlobalCleanup]
    public async Task Cleanup()
    {
        _client.Dispose();

        try
        {
            await _echoLoop;
        }
        catch { }

        _server.Dispose();
        _listener.Dispose();
    }

    [Benchmark(OperationsPerInvoke = OperationsPerInvoke)]
    public void SynchronousRoundTripAfterConnectAsync()
    {
        for (int i = 0; i < OperationsPerInvoke; i++)
        {
            _client.Send(_buffer);
            _client.Receive(_buffer);
        }
    }

    private void EchoLoop()
    {
        var buffer = new byte[1];
        while (_server.Receive(buffer) != 0)
            _server.Send(buffer);
    }
}
```

The background socket echoes each byte. When the client calls `Receive` before the reply is available, .NET 10 must emulate the wait with the Unix socket poller, whereas .NET 11 can wait in the native blocking `recv` call.

| Method                                | Runtime   | Mean     | Ratio |
| ------------------------------------- | --------- | -------- | ----- |
| SynchronousRoundTripAfterConnectAsync | .NET 10.0 | 199.2 μs | 1.00  |
| SynchronousRoundTripAfterConnectAsync | .NET 11.0 | 161.6 μs | 0.81  |

Windows has a different concern around its asynchronous socket operations. I mentioned that Windows supplies functions that utilize overlapped I/O. These APIs, e.g. `AcceptEx`, `ConnectEx`, `DisconnectEx`, and `WSARecvMsg`, are extension functions supplied by the installed Winsock provider. .NET looks up their function pointers dynamically and caches them based on the socket’s address family, socket type, and protocol. Each `Socket` instance consults that cache the first time it needs one of these functions. In .NET 10, that cache was a small global `List<T>` guarded by a lock. The list rarely contains more than a handful of entries and almost every lookup finds an entry that was initialized earlier, but even those read-only hits acquired the same lock. When many sockets began their first asynchronous operation concurrently, all of those lookups were forced through the lock one at a time. [dotnet/runtime#124997](https://github.com/dotnet/runtime/pull/124997) changes the cache to a copy-on-write array. The common read path takes a snapshot of the array and scans it without locking. A miss still acquires a lock, double-checks the latest array, and publishes a new array containing the additional entry. Because entries are added only when a new combination of address family, socket type, and protocol is encountered, misses are rare and the warmed path no longer serializes.

Once you have the socket connection, often the next step is to layer in TLS, with `SslStream`. During client-certificate negotiation, a server can include in its `CertificateRequest` message the distinguished names of certificate authorities whose certificates it will accept. `SslStream` turns each encoded X.500 name into an `X500DistinguishedName`, ultimately making the names available to certificate-selection logic. In .NET 10, that involved allocating a `byte[]` for every name. On Windows, the implementation created a span over the native SSPI buffer and then called `ToArray`; on macOS, it copied each Core Foundation `CFData` value into a new managed array. The `X500DistinguishedName(ReadOnlySpan<byte>)` constructor has existed since .NET 5, but both of these `SslStream` paths predated it and weren’t updated when it was added. With [dotnet/runtime#123904](https://github.com/dotnet/runtime/pull/123904), .NET 11 removes those intermediate arrays. Both implementations instead pass a `ReadOnlySpan<byte>` over the native encoding directly to the `X500DistinguishedName` constructor. The macOS implementation keeps the `CFData` handle alive while that span is in use, but the per-authority managed copy is no longer needed.

Once a client certificate has been selected, macOS requires `SslStream` to package the native handles for the leaf certificate and its intermediate certificates into a Core Foundation array. In .NET 10, `SslStream` first allocated an `IntPtr[]` large enough for the entire chain, populated it with those handles, and then used the array to create the native `CFArray`. [dotnet/runtime#123905](https://github.com/dotnet/runtime/pull/123905) in .NET 11 changes the interop layer to accept a `ReadOnlySpan<IntPtr>` instead. `SslStream` builds the handle list in a `Span<IntPtr>`, using `stackalloc` for chains of up to 128 certificates and falling back to a managed array only for larger chains. Typical certificate chains are far smaller than that, so the usual setup path no longer allocates the temporary `IntPtr[]` at all.

A larger Linux change removes copies from the steady-state encrypted-data path. `SslStream` uses OpenSSL, and OpenSSL traditionally exchanges data with its caller through in-memory buffers known as BIOs. In .NET 10, encryption first wrote ciphertext into an OpenSSL memory BIO, after which .NET copied it into the buffer to send. Decryption went in the other direction: .NET copied received ciphertext into a memory BIO, and after OpenSSL decrypted it, `SslStream` copied the plaintext from its own buffer into the caller’s buffer.

[dotnet/runtime#128245](https://github.com/dotnet/runtime/pull/128245) replaces those memory BIOs with a custom BIO that can point directly at managed buffers. In .NET 11, OpenSSL can write encrypted output directly into the buffer `SslStream` will send and, in the common case, write decrypted plaintext directly into the buffer supplied by the caller. The change also combines the setup, OpenSSL operation, and cleanup into one native call rather than four. OpenSSL still performs its own internal TLS processing, and `SslStream` retains a fallback buffer for unusual cases such as TLS alerts or output that doesn’t fit, but the normal application-data path avoids the extra staging copies.

The following benchmark provides a way to reproduce the impact using only public APIs. It establishes a TLS 1.3 connection once, outside the measurement, and then sends one 16-KB TLS record in each direction:

```
// Linux:
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using System.Net;
using System.Net.Security;
using System.Net.Sockets;
using System.Security.Authentication;
using System.Security.Cryptography;
using System.Security.Cryptography.X509Certificates;

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private const int MessageSize = 16 * 1024;

    private readonly byte[] _sendBuffer = new byte[MessageSize];
    private readonly byte[] _receiveBuffer = new byte[MessageSize];
    private RSA _rsa = null!;
    private X509Certificate2 _certificate = null!;
    private SslStream _client = null!;
    private SslStream _server = null!;

    [GlobalSetup]
    public async Task Setup()
    {
        Random.Shared.NextBytes(_sendBuffer);

        _rsa = RSA.Create(2048);
        var request = new CertificateRequest("CN=localhost", _rsa, HashAlgorithmName.SHA256, RSASignaturePadding.Pkcs1);
        using X509Certificate2 temporary = request.CreateSelfSigned(DateTimeOffset.UtcNow.AddDays(-1), DateTimeOffset.UtcNow.AddDays(1));
        _certificate = X509CertificateLoader.LoadPkcs12(temporary.Export(X509ContentType.Pfx), password: null, X509KeyStorageFlags.Exportable);

        using TcpListener listener = new(IPAddress.Loopback, 0);
        listener.Start();

        Socket clientSocket = new(AddressFamily.InterNetwork, SocketType.Stream, ProtocolType.Tcp)
        {
            NoDelay = true,
        };
        Task<Socket> accept = listener.AcceptSocketAsync();
        await clientSocket.ConnectAsync(listener.LocalEndpoint);
        Socket serverSocket = await accept;
        serverSocket.NoDelay = true;

        _client = new(new NetworkStream(clientSocket, ownsSocket: true), leaveInnerStreamOpen: false, (_, _, _, _) => true);
        _server = new(new NetworkStream(serverSocket, ownsSocket: true), leaveInnerStreamOpen: false);

        using CancellationTokenSource timeout = new(TimeSpan.FromSeconds(30));
        Task clientAuthentication = _client.AuthenticateAsClientAsync(
            new SslClientAuthenticationOptions
            {
                TargetHost = "localhost",
                EnabledSslProtocols = SslProtocols.Tls13,
            },
            timeout.Token);
        Task serverAuthentication = _server.AuthenticateAsServerAsync(
            new SslServerAuthenticationOptions
            {
                ServerCertificate = _certificate,
                EnabledSslProtocols = SslProtocols.Tls13,
            },
            timeout.Token);

        await Task.WhenAll(clientAuthentication, serverAuthentication);
    }

    [Benchmark]
    public async Task RoundTrip()
    {
        await _client.WriteAsync(_sendBuffer);
        await _server.ReadExactlyAsync(_receiveBuffer);

        await _server.WriteAsync(_sendBuffer);
        await _client.ReadExactlyAsync(_receiveBuffer);
    }

    [GlobalCleanup]
    public void Cleanup()
    {
        _client.Dispose();
        _server.Dispose();
        _certificate.Dispose();
        _rsa.Dispose();
    }
}
```

On Ubuntu 24.04 x64 under WSL 2, the 16-KB round trip improves by 16%:

| Method    | Runtime   | Mean     | Ratio |
| --------- | --------- | -------- | ----- |
| RoundTrip | .NET 10.0 | 68.04 μs | 1.00  |
| RoundTrip | .NET 11.0 | 57.39 μs | 0.84  |

Moving up the stack, `HttpClient`‘s core HTTP implementation in `SocketsHttpHandler` layers protocol processing on top of TLS and the underlying sockets. With `AutomaticDecompression` enabled, `SocketsHttpHandler` advertises supported encodings on the request, checks the response’s final `Content-Encoding`, and, when it recognizes gzip, deflate, or Brotli, presents an `HttpContent` whose stream decodes the compressed transport bytes as the caller reads them. [dotnet/runtime#122676](https://github.com/dotnet/runtime/pull/122676) precomputes the combined `Accept-Encoding` value when the handler is created. In the common case where the caller hasn’t supplied that header, it adds the combined value directly, avoiding an `HttpHeaderValueCollection`, its backing list and header-storage object, and an enumeration of the collection for each enabled algorithm. On the response side, `TryGetValues` avoids materializing a collection when there is no `Content-Encoding`. When decompression is needed, the wrapper takes ownership of the original content-header collection, removes the now-invalid `Content-Length` and the encoding it consumes, and retains any preceding encodings without copying every header into a new collection.

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using System.IO.Compression;
using System.Net;
using System.Net.Sockets;
using System.Text;
using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[MemoryDiagnoser(false)]
[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD","Mean","Ratio")]
public class Benchmarks
{
    private TcpListener _listener = new(IPAddress.Loopback, 0);
    private CancellationTokenSource _cts = new();
    private Task _server = null!;
    private HttpClient _client = null!;

    [GlobalSetup]
    public async Task Setup()
    {
        _listener.Start();
        _server = ServeAsync(_cts.Token);
        _client = new(new SocketsHttpHandler { AutomaticDecompression = DecompressionMethods.GZip })
        {
            BaseAddress = new Uri($"http://127.0.0.1:{((IPEndPoint)_listener.LocalEndpoint).Port}")
        };

        await GetAsync();
    }

    [Benchmark]
    public async Task<int> GetAsync()
    {
        using HttpResponseMessage response = await _client.GetAsync("/", HttpCompletionOption.ResponseHeadersRead);
        await response.Content.CopyToAsync(Stream.Null);
        return (int)response.StatusCode;
    }

    [GlobalCleanup]
    public async Task Cleanup()
    {
        _client.Dispose();
        _cts.Cancel();

        try
        {
            await _server;
        }
        catch { }

        _listener.Stop();
        _cts.Dispose();
    }

    private async Task ServeAsync(CancellationToken cancellationToken)
    {
        byte[] body = Compress(new byte[1024]);
        byte[] headers = Encoding.ASCII.GetBytes(
            $"HTTP/1.1 200 OK\r\nContent-Encoding: gzip\r\n" +
            $"Content-Length: {body.Length}\r\n\r\n");

        while (true)
        {
            using TcpClient connection = await _listener.AcceptTcpClientAsync(cancellationToken);
            NetworkStream stream = connection.GetStream();
            byte[] request = new byte[4096];

            while (await ReadRequestAsync(stream, request, cancellationToken))
            {
                await stream.WriteAsync(headers, cancellationToken);
                await stream.WriteAsync(body, cancellationToken);
            }
        }
    }

    private static async Task<bool> ReadRequestAsync(Stream stream, byte[] buffer, CancellationToken cancellationToken)
    {
        int length = 0;
        while (length < buffer.Length)
        {
            int read = await stream.ReadAsync(buffer.AsMemory(length), cancellationToken);
            if (read == 0)
                return false;

            length += read;
            if (buffer.AsSpan(0, length).IndexOf("\r\n\r\n"u8) >= 0)
            {
                return true;
            }
        }

        throw new InvalidOperationException("Request headers are too large.");
    }

    private static byte[] Compress(byte[] data)
    {
        using MemoryStream output = new();
        using (GZipStream gzip = new(output, CompressionLevel.SmallestSize, leaveOpen: true))
        {
            gzip.Write(data);
        }

        return output.ToArray();
    }
}
```

| Method   | Runtime   | Allocated | Alloc Ratio |
| -------- | --------- | --------- | ----------- |
| GetAsync | .NET 10.0 | 3.44 KB   | 1.00        |
| GetAsync | .NET 11.0 | 2.87 KB   | 0.83        |

`SocketsHttpHandler` saw other improvements. HTTP content often consists of a single value, but sometimes a request or response needs to carry several independent pieces together in one body. Multipart content provides that packaging. For example, an HTML form submission might contain a few text fields and a file; each becomes a separate part with its own headers and content, while the collection of parts is sent as one HTTP message body. The receiver needs to know where one part ends and the next begins, so the message uses a boundary: a token chosen to be unlikely to occur in the content itself. In .NET 10, `MultipartContent` retained the boundary as a string. Each time the content was serialized, it rebuilt the opening and closing delimiter strings, encoded them into bytes, and separately wrote the pieces of the delimiters between parts. In .NET 11, [dotnet/runtime#124963](https://github.com/dotnet/runtime/pull/124963) instead constructs and encodes the opening and closing delimiters once, when the `MultipartContent` is created. The serialization paths can then reuse and directly write those cached bytes.

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using System.Net.Http;
using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[MemoryDiagnoser(false), HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private readonly MultipartContent _content = new("mixed", "net11-boundary");

    [Benchmark]
    public Task Serialize() => _content.CopyToAsync(Stream.Null);
}
```

| Method    | Runtime   | Mean     | Ratio | Allocated | Alloc Ratio |
| --------- | --------- | -------- | ----- | --------- | ----------- |
| Serialize | .NET 10.0 | 78.49 ns | 1.00  | 296 B     | 1.00        |
| Serialize | .NET 11.0 | 41.90 ns | 0.53  | 64 B      | 0.22        |

Several allocation reductions remove collections created only to populate or inspect another collection. For example, [dotnet/runtime#122677](https://github.com/dotnet/runtime/pull/122677) writes HTTP/3 trailers directly into the final `HttpResponseHeaders` collection, eliminating a temporary `List` of tuples. Trailers are headers sent after the response body, commonly carrying information such as checksums that isn’t known when the initial headers are written.

Other paths only need a transient view over existing storage. [dotnet/runtime#131142](https://github.com/dotnet/runtime/pull/131142) has `SocketsHttpHandler` inspect available HTTP/2 and HTTP/3 connections through spans, avoiding a copy of each list to an array during idle-connection eviction. [dotnet/runtime#123034](https://github.com/dotnet/runtime/pull/123034) similarly changes `HeaderUtilities.DumpHeaders`, which is used as part of `ToString` on header collections, to take a `params ReadOnlySpan<HttpHeaders?>`, removing a small array allocation from `HttpRequestMessage.ToString()` and `HttpResponseMessage.ToString()`.

Improvements in .NET 11 also show up for `Uri`. Consider `https://user@example.com:8443/files/report%20Q3?q=%E4%BD%A0%E5%A5%BD#summary`. Before `Uri` can expose `Scheme`, `UserInfo`, `Host`, `Port`, `AbsolutePath`, `Query`, and `Fragment`, it first locates delimiters such as `:`, `/`, `@`, `?`, and `#`. It then validates each delimited component. ASCII can usually remain as-is, `%20` needs unescaping or preservation according to the component, and the percent-encoded UTF-8 in the query needs decoding and Unicode-aware canonicalization. With [dotnet/runtime#124433](https://github.com/dotnet/runtime/pull/124433), .NET 11 uses `IndexOfAny` and `SearchValues` for more of the delimiter-finding work, examining long spans a vector at a time rather than character by character. And once the component boundaries are known, [dotnet/runtime#119435](https://github.com/dotnet/runtime/pull/119435) replaces repeated reserved-character and unsafe-character tests with a single optimized `SearchValues` lookup.

```
// dotnet run -c Release -f net10.0 --filter "*UriScanningBenchmarks*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

BenchmarkSwitcher.FromAssembly(typeof(UriScanningBenchmarks).Assembly).Run(args);

[MemoryDiagnoser(false)]
[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class UriScanningBenchmarks
{
    private readonly string _longHost = $"https://{new string('a', 64)}.example.com/path";
    private readonly string _escapedAscii =
        "https://example.com/" +
        string.Concat(Enumerable.Range('a', 26).Select(i => $"%{i:X2}"));

    [Benchmark]
    public Uri LongHost() => new(_longHost);

    [Benchmark]
    public Uri EscapedAscii() => new(_escapedAscii);
}
```

| Method       | Runtime   | Mean     | Ratio | Allocated | Alloc Ratio |
| ------------ | --------- | -------- | ----- | --------- | ----------- |
| LongHost     | .NET 10.0 | 218.1 ns | 1.00  | 56 B      | 1.00        |
| LongHost     | .NET 11.0 | 112.9 ns | 0.52  | 56 B      | 1.00        |
| EscapedAscii | .NET 10.0 | 442.6 ns | 1.00  | 448 B     | 1.00        |
| EscapedAscii | .NET 11.0 | 208.1 ns | 0.47  | 368 B     | 0.82        |

After finding a component, `Uri` checks whether its text is already in canonical form or needs to be escaped or normalized. Letters and digits are by far the most common characters, but in .NET 10 they still flowed through the more general character tests. Some callers could also repeat a canonicalization check whose answer parsing had already established. [dotnet/runtime#121270](https://github.com/dotnet/runtime/pull/121270) adds a fast path for ASCII letters and digits and records the earlier result so that .NET 11 can avoid performing the same check again.

```
// dotnet run -c Release -f net10.0 --filter "*UriCanonicalizationBenchmarks*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

BenchmarkSwitcher.FromAssembly(typeof(UriCanonicalizationBenchmarks).Assembly).Run(args);

[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class UriCanonicalizationBenchmarks
{
    private const string Address = "https://example.com/api/items/42?view=summary#details";

    [Benchmark]
    public Uri Parse() => new(Address);
}
```

| Method | Runtime   | Mean     | Ratio | Allocated | Alloc Ratio |
| ------ | --------- | -------- | ----- | --------- | ----------- |
| Parse  | .NET 10.0 | 61.86 ns | 1.00  | 56 B      | 1.00        |
| Parse  | .NET 11.0 | 44.64 ns | 0.72  | 56 B      | 1.00        |

Non-ASCII input can require several parts of a URI to be normalized. For example, Unicode characters may need to be preserved or percent-encoded differently depending on whether they occur in the path, query, or fragment. In .NET 10, parsing and rebuilding were interleaved: `Uri` normalized each of those components separately and repeatedly extended its stored string as it went. In addition to making the offset bookkeeping complicated, those individual normalization results and string concatenations could create roughly five temporary strings. In .NET 11, [dotnet/runtime#122038](https://github.com/dotnet/runtime/pull/122038) separates that rebuilding work from the subsequent validation. `Uri` normalizes the path, query, and fragment into one builder, creates the final string once, and then validates the component boundaries in that completed string. The host is still handled separately, but the remaining components no longer each produce intermediate strings.

```
// dotnet run -c Release -f net10.0 --filter "*UriNormalizationBenchmarks*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

BenchmarkSwitcher.FromAssembly(typeof(UriNormalizationBenchmarks).Assembly).Run(args);

[MemoryDiagnoser(false)]
[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class UriNormalizationBenchmarks
{
    private const string Address =
        "https://dot.net/abc/defghijklmno/pqrstuv/wxyz" +
        "?arch=x64&os=linux&type=release#hello\uD83C\uDF49";

    [Benchmark]
    public Uri Parse() => new(Address);
}
```

| Method | Runtime   | Mean     | Ratio | Allocated | Alloc Ratio |
| ------ | --------- | -------- | ----- | --------- | ----------- |
| Parse  | .NET 10.0 | 547.6 ns | 1.00  | 936 B     | 1.00        |
| Parse  | .NET 11.0 | 386.9 ns | 0.71  | 432 B     | 0.46        |

## JSON

JSON readers and writers spend much of their time scanning text: writers look for characters that need escaping, while readers look for whitespace and token boundaries. .NET 11 makes several of those scans more efficient.

When using the default encoder, `Utf8JsonWriter` needs to locate characters such as quotation marks and control characters that can’t be copied directly into JSON. In .NET 10, that search was routed through `JavaScriptEncoder.Default`. [dotnet/runtime#129781](https://github.com/dotnet/runtime/pull/129781) instead gives .NET 11 precomputed `SearchValues` sets for the default escaping rules, allowing the writer to search the input directly. Once it finds a character to escape, the writer must emit a sequence such as `\"` or `\u0022`. In .NET 10, the escaping helper received the entire remaining destination and performed repeated bounds checks as it wrote each byte or character. [dotnet/runtime#129803](https://github.com/dotnet/runtime/pull/129803) passes only the range known to be writable. The JIT can then prove once that the escape fits and remove the checks from the individual stores.

```
// dotnet run -c Release -f net10.0 --filter "*JsonWriterBenchmarks*" --runtimes net10.0 net11.0

using System.Text.Json;
using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

BenchmarkSwitcher.FromAssembly(typeof(JsonWriterBenchmarks).Assembly).Run(args);

[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class JsonWriterBenchmarks
{
    private static readonly string s_fullyEscaped = new('"', 2_048);

    [Benchmark]
    public byte[] Write() => JsonSerializer.SerializeToUtf8Bytes(s_fullyEscaped);
}
```

| Method | Runtime   | Mean     | Ratio |
| ------ | --------- | -------- | ----- |
| Write  | .NET 10.0 | 30.83 μs | 1.00  |
| Write  | .NET 11.0 | 7.875 μs | 0.26  |

On the reading side, insignificant whitespace is allowed between JSON tokens. Indented documents can contain long runs of spaces and newlines, and in .NET 10 `Utf8JsonReader` examined those bytes one at a time. [dotnet/runtime#129701](https://github.com/dotnet/runtime/pull/129701) changes `SkipWhiteSpace` to use `IndexOfAnyExcept` with a `SearchValues` set containing the four JSON whitespace bytes. .NET 11 can therefore skip a whole run at once, stopping at the next byte that might begin a token.

```
// dotnet run -c Release -f net10.0 --filter "*JsonReaderBenchmarks*" --runtimes net10.0 net11.0

using System.Text.Json;
using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

BenchmarkSwitcher.FromAssembly(typeof(JsonReaderBenchmarks).Assembly).Run(args);

[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class JsonReaderBenchmarks
{
    private static readonly Payload s_value = new(new string('a', 2_048), Enumerable.Range(0, 256).ToArray());
    private static readonly byte[] s_json = JsonSerializer.SerializeToUtf8Bytes(s_value, new JsonSerializerOptions { WriteIndented = true });

    [Benchmark]
    public int Read()
    {
        Utf8JsonReader reader = new(s_json);
        int tokens = 0;
        while (reader.Read()) tokens++;
        return tokens;
    }

    private sealed record Payload(string Message, int[] Values);
}
```

| Method | Runtime   | Mean     | Ratio |
| ------ | --------- | -------- | ----- |
| Read   | .NET 10.0 | 7.418 μs | 1.00  |
| Read   | .NET 11.0 | 5.945 μs | 0.80  |

## Diagnostics

Creating an `Activity`, polling metrics, and logging all add overhead beyond what the application is otherwise trying to accomplish. That cost is deliberately paid to make production systems understandable, but observability code also sits on paths that can execute for every request, dependency call, or log event. Small fixed costs there can really add up, and disabled or unobserved instrumentation needs to be “pay for play” so applications don’t incur meaningful costs for diagnostics they aren’t currently collecting.

Let’s start with distributed tracing. A trace follows a request as it travels through an application and potentially across multiple services. Each operation along the way can be represented by an `Activity`; the activities have their own span IDs, but share a trace ID that lets a tracing system correlate them as parts of the same request. The W3C Trace Context standard defines how those identifiers are carried between services, including in an HTTP `traceparent` header. Its trace ID is represented as 32 lowercase hexadecimal characters, and it can’t be all zeroes. Applications may need to parse and validate that identifier for every request. In .NET 10, `DiagnosticSource` did so with a loop that checked each character both for whether it was hexadecimal and whether it was non-zero. In .NET 11, [dotnet/runtime#119673](https://github.com/dotnet/runtime/pull/119673) replaces that loop with two `ContainsAnyExcept` searches: one detects a character outside `0`–`9` and `a`–`f`, while the other determines whether the entire ID is zeroes. Those searches can examine multiple characters at a time. `W3CPropagator` had similar hand-written loops for validating trace-state and baggage characters. In addition to replacing those loops with `SearchValues<char>`, the same PR changes baggage encoding to search for the first character that requires escaping. If there isn’t one, as is common, it can append the whole value at once rather than checking and appending every character individually.

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using System.Diagnostics;
using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private static readonly DistributedContextPropagator s_propagator =
        DistributedContextPropagator.CreateW3CPropagator();
    private readonly Activity _activity = new("Test");

    [GlobalSetup]
    public void Setup()
    {
        _activity.AddBaggage("a", "aaaaabbbbbcccccddddd");
        _activity.Start();
    }

    [Benchmark]
    public void ExtractTraceParent() =>
        s_propagator.ExtractTraceIdAndState(
            null,
            static (object? carrier, string name, out string? value, out IEnumerable<string>? values) =>
            {
                value = name == "traceparent" ? "00-0af7651916cd43dd8448eb211c80319c-b9c7c989f97918e1-01" : null;
                values = null;
            },
            out _, out _);

    [Benchmark]
    public void InjectBaggage() => s_propagator.Inject(_activity, null, static (object? carrier, string name, string value) => { });
}
```

| Method             | Runtime   | Mean      | Ratio |
| ------------------ | --------- | --------- | ----- |
| ExtractTraceParent | .NET 10.0 | 45.41 ns  | 1.00  |
| ExtractTraceParent | .NET 11.0 | 9.137 ns  | 0.20  |
| InjectBaggage      | .NET 10.0 | 96.69 ns  | 1.00  |
| InjectBaggage      | .NET 11.0 | 47.895 ns | 0.50  |

Process APIs present a different kind of diagnostics overhead. Launching a process requires translating managed arguments and environment variables into the representation expected by the operating system, while inspection often crosses into native APIs to retrieve only a small piece of information.

At the lowest level, a new process on Unix receives its command-line arguments and environment as `argv` and `envp`. Each is a null-terminated array of pointers to null-terminated strings; the entries in `argv` are the executable and its arguments, while each entry in `envp` has the form `key=value`. `ProcessStartInfo`, however, exposes managed strings and a managed environment dictionary, so `Process.Start` needs to marshal all of that data into the native representation. In .NET 10, building `envp` first concatenated every key and value into a new managed `key=value` string and collected those strings into an intermediate array. Both `argv` and `envp` were then constructed with a native allocation for the pointer array and another allocation for each UTF-8 string. With [dotnet/runtime#126201](https://github.com/dotnet/runtime/pull/126201), .NET 11 instead makes one pass to count the pointers and calculate the total number of UTF-8 bytes required. It then allocates one native block for `argv` and one for `envp`, with each block containing both its pointer table and all of its string data, and writes the data directly into those blocks. That avoids the intermediate managed strings and array, as well as all of the per-string native allocations and frees.

There’s then the question of how the operating system actually creates the process. The traditional Unix model uses `fork` to create a child that is initially a logical copy of the parent, followed by `exec` in the child to replace that copy with the requested executable. Copy-on-write means `fork` doesn’t immediately copy all of the parent’s memory, but the operating system still needs to duplicate process state and page tables, work that can become significant for a large, multithreaded application. In .NET 10, `Process.Start` used this `fork`-then-`exec` path on macOS. With [dotnet/runtime#126063](https://github.com/dotnet/runtime/pull/126063), .NET 11 uses `posix_spawn` for the common case. `posix_spawn` asks the operating system to create the new process and load its executable as one operation, while still describing the required standard-input/output/error redirection, working directory, and signal state. A launch that requests different user or group credentials still uses `fork` and `exec`, as macOS’s `posix_spawn` facilities can’t perform the required `setuid` and `setgid` operations.

```
// Run on Linux and macOS:
// dotnet run -c Release -f net10.0 --filter "*ProcessLaunchBenchmarks*" --runtimes net10.0 net11.0

using System.Diagnostics;
using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

BenchmarkSwitcher.FromAssembly(
    typeof(ProcessLaunchBenchmarks).Assembly).Run(args);

[MemoryDiagnoser(false), HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class ProcessLaunchBenchmarks
{
    private readonly ProcessStartInfo _plain = CreateStartInfo();
    private readonly ProcessStartInfo _withEnvironment = CreateStartInfo(includeEnvironment: true);

    [Benchmark]
    public void StartWithEnvironment() => StartAndWait(_withEnvironment);

    [Benchmark]
    public void StartAndWaitForExit() => StartAndWait(_plain);

    private static ProcessStartInfo CreateStartInfo(bool includeEnvironment = false)
    {
        ProcessStartInfo psi = new("whoami")
        {
            RedirectStandardOutput = true,
            UseShellExecute = false,
        };

        if (includeEnvironment)
        {
            for (int i = 0; i < 256; i++)
                psi.Environment[$"NET11PERF_{i}"] = new string('x', 32);
        }

        return psi;
    }

    private static void StartAndWait(ProcessStartInfo psi)
    {
        using Process process = Process.Start(psi)!;
        process.WaitForExit();
    }
}
```

Process creation dominates the elapsed time in this benchmark, but the environment-marshalling allocation reduction is clear.

| Method               | Runtime   | Mean     | Ratio | Allocated | Alloc Ratio |
| -------------------- | --------- | -------- | ----- | --------- | ----------- |
| StartWithEnvironment | .NET 10.0 | 1.484 ms | 1.00  | 48.16 KB  | 1.00        |
| StartWithEnvironment | .NET 11.0 | 1.435 ms | 0.97  | 14.48 KB  | 0.30        |
|                      |           |          |       |           |             |
| StartAndWaitForExit  | .NET 10.0 | 1.371 ms | 1.00  | 16.95 KB  | 1.00        |
| StartAndWaitForExit  | .NET 11.0 | 1.362 ms | 0.99  | 14.48 KB  | 0.85        |

Once a process is running, a `Process` instance can expose a bunch of information about it. Much of that OS data is gathered and cached together in an internal `ProcessInfo` object so that properties needing it can share the work. In .NET 10 on Linux and macOS, however, asking only for `ProcessName` triggered the machinery to populate the whole object and everything on it, which was unnecessarily costly if you only needed the name. `Process.ToString()` includes the process name, so it incurred the same cost. In .NET 11, [dotnet/runtime#126449](https://github.com/dotnet/runtime/pull/126449) from [@tmds](https://github.com/tmds) adds a narrower operating-system query for the name. `ProcessName` and `ToString()` can use that to query without collecting the rest of the process metadata.

```
// Run on Linux and macOS:
// dotnet run -c Release -f net10.0 --filter "*ProcessNameBenchmarks*" --runtimes net10.0 net11.0

using System.Diagnostics;
using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

BenchmarkSwitcher.FromAssembly(typeof(ProcessNameBenchmarks).Assembly).Run(args);

[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class ProcessNameBenchmarks
{
    [Benchmark]
    public string GetProcessName()
    {
        using Process process = Process.GetProcessById(Environment.ProcessId);
        return process.ProcessName;
    }
}
```

| Method         | Runtime   | Mean      | Ratio |
| -------------- | --------- | --------- | ----- |
| GetProcessName | .NET 10.0 | 332.13 μs | 1.00  |
| GetProcessName | .NET 11.0 | 11.90 μs  | 0.04  |

`Process` can also query processes on another Windows machine. APIs such as `GetProcesses(string machineName)` accept a machine name, and the remote path uses Windows performance-counter infrastructure to retrieve the information. That support in turn depends on additional components, including remote Registry access. None of that should be necessary for an application that only starts or inspects processes on its own machine. In .NET 10, however, several local-only APIs delegated to overloads that also supported remote machines. For example, `GetProcessById(int)` called the machine-name overload with `"."`, and other helpers selected between local and remote implementations at run time. Even when the application always took the local branch, the trimmer saw a call path to both implementations and needed to preserve the remote-process and `PerformanceCounter` code. As a result, even a Native AOT application that did little more than call `Process.Start` could carry that unused support in its executable. In .NET 11, [dotnet/runtime#126338](https://github.com/dotnet/runtime/pull/126338) gives the local APIs dedicated paths that don’t reference the remote implementation. The remote implementation is instead reached through a delegate initialized only when an API is actually asked to operate on another machine. Remote process inspection continues to work, but if an application uses only local process APIs, .NET 11’s trimmer can now prove that the remote machinery and its dependencies are unreachable and remove them, resulting in significantly smaller binary size.

```
// Add to the csproj's PropertyGroup:
//     <PublishAot>true</PublishAot>
//     <InvariantGlobalization>true</InvariantGlobalization>
//     <AssemblyName>ProcessSize</AssemblyName>

using System.Diagnostics;

using Process process = Process.Start(new ProcessStartInfo("cmd.exe", "/c exit")
{
    UseShellExecute = false
})!;
process.WaitForExit();
```

You can then publish both targets and inspect the resulting executable:

```
# dotnet publish -c Release -f net10.0 -r win-x64 -o publish-net10
# dotnet publish -c Release -f net11.0 -r win-x64 -o publish-net11
# Get-Item .\publish-net10\ProcessSize.exe, .\publish-net11\ProcessSize.exe |
#     ForEach-Object { "$($_.Directory.Name): $($_.Length) bytes" }
```

| Runtime   | Executable size |
| --------- | --------------- |
| .NET 10.0 | 1,599,488 bytes |
| .NET 11.0 | 1,326,080 bytes |

Metrics report numerical information about an application, such as the number of requests processed or the current depth of a queue. With `System.Diagnostics.Metrics`, a `Meter` creates instruments that produce those measurements, and a listener such as an OpenTelemetry provider consumes them. Some instruments are updated by the application whenever an event occurs. An observable instrument instead registers a callback that computes its current value when a listener asks to collect it. That pull model is useful for values like queue depth: the application doesn’t need to record every change, only to report the depth when it’s observed. The callback for an `ObservableGauge<T>`, `ObservableCounter<T>`, or `ObservableUpDownCounter<T>` can return a `T`, a `Measurement<T>`, or an `IEnumerable<Measurement<T>>`. A `Measurement<T>` pairs the value with any associated tags, and the enumerable form allows one callback to report multiple tagged values. The first two forms always produce exactly one measurement. In .NET 10, `ObservableInstrument<T>` nevertheless normalized those single-value forms into the enumerable model. Every time a listener collected the instrument, it invoked the callback, put the result into a new one-element `Measurement<T>[]`, and then enumerated that array to report the value. With [dotnet/runtime#128039](https://github.com/dotnet/runtime/pull/128039) from [@unsafePtr](https://github.com/unsafePtr), .NET 11 recognizes the built-in single-value forms and sends their result directly to `MeterListener.NotifyMeasurement`, avoiding both the array and its enumeration. The enumerable form retains its existing path: the application owns that sequence, and it may legitimately contain any number of measurements.

```
// dotnet run -c Release -f net10.0 --filter "*ObservableBenchmarks*" --runtimes net10.0 net11.0

using System.Diagnostics.Metrics;
using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

BenchmarkSwitcher.FromAssembly(typeof(ObservableBenchmarks).Assembly).Run(args);

[MemoryDiagnoser(false), HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class ObservableBenchmarks
{
    private int _queueLength = 42;
    private Meter _meter = new("Sample.Service");
    private ObservableGauge<int> _gauge = null!;
    private MeterListener _listener = new();

    [GlobalSetup]
    public void Setup()
    {
        _gauge = _meter.CreateObservableGauge("queue.length", () => _queueLength);
        _listener.InstrumentPublished = (instrument, listener) =>
        {
            if (instrument.Meter == _meter)
                listener.EnableMeasurementEvents(instrument);
        };
        _listener.SetMeasurementEventCallback<int>( static (instrument, measurement, tags, state) => { });
        _listener.Start();
    }

    [GlobalCleanup]
    public void Cleanup()
    {
        _listener.Dispose();
        _meter.Dispose();
    }

    [Benchmark]
    public void Record() => _listener.RecordObservableInstruments();
}
```

| Method | Runtime   | Mean     | Ratio | Allocated | Alloc Ratio |
| ------ | --------- | -------- | ----- | --------- | ----------- |
| Record | .NET 10.0 | 17.16 ns | 1.00  | 72 B      | 1.00        |
| Record | .NET 11.0 | 4.511 ns | 0.26  | –         | 0           |

In previous iterations of Performance Improvements in .NET, I’ve discussed “false sharing.” Modern processors move data between memory and their caches in fixed-size chunks known as cache lines, commonly 64 bytes. Before a core can write to a location, it needs exclusive ownership of the cache line containing that location, invalidating copies of the same line held by other cores. That matters even when the cores aren’t updating the same value. Imagine two `long` fields next to each other in memory, with one core repeatedly updating the first and another core repeatedly updating the second. The fields are logically independent, but if they occupy the same cache line, each core’s write invalidates the line for the other. Ownership of the line continually bounces between the cores, limiting scalability despite there being no sharing conceptually. Hence, “false sharing.” `System.Runtime.Caching.MemoryCache` maintains performance counters for operations such as gets, hits, misses, adds, removes, and trims. In .NET 10, those counters were stored as elements in a small `long[]`. The array header, including its length, and several unrelated counters could all occupy the same cache line. Under load, cores performing different cache operations would therefore contend for ownership of that line as they updated different counters. Accessing a counter through the array also meant loading the array length for a bounds check. [dotnet/runtime#131470](https://github.com/dotnet/runtime/pull/131470) addresses this in .NET 11 by replacing the array with named fields and laying those fields out across separate cache lines. Counters that an operation naturally updates together can remain together, while unrelated counters are kept apart. That deliberately spends a small amount of additional memory on padding in order to reduce cache-line bouncing under contention, while the named fields also avoid the array bounds checks.

```
// Run separately so each target uses its matching System.Runtime.Caching package:
// dotnet run -c Release -f net10.0 --filter "*"
// dotnet run -c Release -f net11.0 --filter "*"

using System.Runtime.Caching;
using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private const int ThreadCount = 32;
    private const int TotalOperations = 256_000;
    private readonly MemoryCache _cache = new("Benchmark");

    [GlobalSetup]
    public void Setup() => _cache.Set("key", 42, DateTimeOffset.MaxValue);

    [GlobalCleanup]
    public void Cleanup() => _cache.Dispose();

    [Benchmark(OperationsPerInvoke = TotalOperations)]
    public void Get()
    {
        Parallel.For(0, ThreadCount, new ParallelOptions
        {
            MaxDegreeOfParallelism = ThreadCount
        }, _ =>
        {
            for (int i = 0; i < TotalOperations / ThreadCount; i++)
                _cache.Get("key");
        });
    }
}
```

| Method | Runtime   | Mean     | Ratio |
| ------ | --------- | -------- | ----- |
| Get    | .NET 10.0 | 75.83 ns | 1.00  |
| Get    | .NET 11.0 | 51.76 ns | 0.68  |

Logging is another per-event diagnostics path. Microsoft.Extensions.Logging’s EventSource provider shrank its cost when the `JsonMessage` keyword is on. [dotnet/runtime#131229](https://github.com/dotnet/runtime/pull/131229) reuses a `[ThreadStatic]` `MemoryStream` and `Utf8JsonWriter` in `EventSourceLogger.ToJson`, avoiding both allocations on every logged event, leaving primarily the returned JSON string. Buffers larger than 1 KB aren’t retained on the thread.

```
// Add a FrameworkReference to Microsoft.AspNetCore.App.
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using System.Diagnostics.Tracing;
using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;
using Microsoft.Extensions.Logging;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[MemoryDiagnoser(false), HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private JsonLoggingListener _listener = null!;
    private ILoggerFactory _factory = null!;
    private ILogger _logger = null!;

    [GlobalSetup]
    public void Setup()
    {
        _listener = new JsonLoggingListener();
        _factory = LoggerFactory.Create(builder => builder.AddEventSourceLogger());
        _logger = _factory.CreateLogger("Sample");
    }

    [GlobalCleanup]
    public void Cleanup()
    {
        _factory.Dispose();
        _listener.Dispose();
    }

    [Benchmark]
    public void Log() => _logger.LogInformation("Processed {Count} items for {Customer}", 42, "Contoso");

    private sealed class JsonLoggingListener : EventListener
    {
        protected override void OnEventSourceCreated(EventSource eventSource)
        {
            if (eventSource.Name == "Microsoft-Extensions-Logging")
                EnableEvents(eventSource, EventLevel.LogAlways, (EventKeywords)8); // JsonMessage
        }
    }
}
```

| Method | Runtime   | Mean     | Ratio | Allocated | Alloc Ratio |
| ------ | --------- | -------- | ----- | --------- | ----------- |
| Log    | .NET 10.0 | 506.4 ns | 1.00  | 1.92 KB   | 1.00        |
| Log    | .NET 11.0 | 400.5 ns | 0.79  | 1.15 KB   | 0.60        |

## Cryptography

ASN.1 is the binary data-description format used by certificates, public and private keys, and many other cryptographic structures. Its encodings are nested: reading a sequence produces another reader over the sequence’s contents, which may itself contain more sequences. [dotnet/runtime#125254](https://github.com/dotnet/runtime/pull/125254) adds `ValueAsnReader`, a span-based `ref struct` counterpart to `AsnReader`. [dotnet/runtime#125346](https://github.com/dotnet/runtime/pull/125346) further applies that representation to selected RSA, PKCS/CMS, ECC, and X.509 decoders. And [dotnet/runtime#125528](https://github.com/dotnet/runtime/pull/125528) carries it through generated key loaders so parsing layers can pass views of the original data by reference rather than wrapping the same bytes in new reader objects.

```
// dotnet run -c Release -f net11.0 --filter "*"

using BenchmarkDotNet.Running;

using System.Formats.Asn1;
using System.Runtime.CompilerServices;
using BenchmarkDotNet.Attributes;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[MemoryDiagnoser(false), HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private static readonly byte[] s_der =
    [
        0x30, 0x0F,
        0x30, 0x03, 0x02, 0x01, 0x01,
        0x30, 0x03, 0x02, 0x01, 0x02,
        0x30, 0x03, 0x02, 0x01, 0x03,
    ];

    [Benchmark(Baseline = true)]
    public int ReadWithEscapingAsnReader()
    {
        AsnReader outer = CreateReader();
        AsnReader sequence = ReadSequence(outer);
        int count = 0;

        while (sequence.HasData)
        {
            AsnReader child = ReadSequence(sequence);
            _ = child.ReadIntegerBytes();
            child.ThrowIfNotEmpty();
            count++;
        }

        outer.ThrowIfNotEmpty();
        return count;
    }

    [Benchmark]
    public int ReadWithValueAsnReader()
    {
        ValueAsnReader outer = new(s_der, AsnEncodingRules.DER);
        ValueAsnReader sequence = outer.ReadSequence();
        int count = 0;

        while (sequence.HasData)
        {
            ValueAsnReader child = sequence.ReadSequence();
            _ = child.ReadIntegerBytes();
            child.ThrowIfNotEmpty();
            count++;
        }

        outer.ThrowIfNotEmpty();
        return count;
    }

    [MethodImpl(MethodImplOptions.NoInlining)]
    private static AsnReader CreateReader() => new(s_der, AsnEncodingRules.DER);

    [MethodImpl(MethodImplOptions.NoInlining)]
    private static AsnReader ReadSequence(AsnReader reader) => reader.ReadSequence();
}
```

| Method                    | Mean     | Ratio | Allocated | Alloc Ratio |
| ------------------------- | -------- | ----- | --------- | ----------- |
| ReadWithEscapingAsnReader | 79.81 ns | 1.00  | 240 B     | 1.00        |
| ReadWithValueAsnReader    | 31.05 ns | 0.39  | –         | 0.00        |

Some ASN.1 values add text validation to that parsing work. ASN.1 defines several text types with restricted character sets. `IA5String` is ASCII, while `VisibleString` permits the printable ASCII characters from space through `~`. Encoding or decoding one must both copy the data and reject characters outside the allowed range. [dotnet/runtime#131109](https://github.com/dotnet/runtime/pull/131109) vectorizes that validation and transcoding for `IA5String` and `VisibleString`, checking and copying multiple characters at once. [dotnet/runtime#131170](https://github.com/dotnet/runtime/pull/131170) then applies the same approach to big-endian UCS-2 `BMPString`.

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using System.Formats.Asn1;

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private static readonly string s_text = new('A', 1024);

    private readonly char[] _destination = new char[1024];
    private readonly AsnWriter _writer = new(AsnEncodingRules.DER);
    private readonly byte[] _encoded = EncodeText();

    [Benchmark]
    public int Read()
    {
        AsnDecoder.TryReadCharacterString(
            _encoded,
            _destination,
            AsnEncodingRules.DER,
            UniversalTagNumber.VisibleString,
            out _,
            out int charsWritten);
        return charsWritten;
    }

    [Benchmark]
    public int Write()
    {
        _writer.Reset();
        _writer.WriteCharacterString(UniversalTagNumber.VisibleString, s_text);
        return _writer.GetEncodedLength();
    }

    private static byte[] EncodeText()
    {
        AsnWriter writer = new(AsnEncodingRules.DER);
        writer.WriteCharacterString(UniversalTagNumber.VisibleString, s_text);
        return writer.Encode();
    }
}
```

| Method | Runtime   | Mean     | Ratio |
| ------ | --------- | -------- | ----- |
| Read   | .NET 10.0 | 1.243 μs | 1.00  |
| Read   | .NET 11.0 | 112.7 ns | 0.091 |
| Write  | .NET 10.0 | 1.556 μs | 1.00  |
| Write  | .NET 11.0 | 152.8 ns | 0.098 |

[dotnet/runtime#131616](https://github.com/dotnet/runtime/pull/131616) takes that further and extends the approach to the non-contiguous character sets of `PrintableString` and `NumericString`. The validation has more than one accepted range, but it can still classify a vector of characters at a time and fall back to the scalar checks only where necessary:

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using System.Formats.Asn1;
using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private static readonly string s_text = new('A', 1024);

    private readonly char[] _destination = new char[1024];
    private readonly AsnWriter _writer = new(AsnEncodingRules.DER);
    private readonly byte[] _encoded = EncodeText();

    [Benchmark]
    public int Read()
    {
        AsnDecoder.TryReadCharacterString(
            _encoded,
            _destination,
            AsnEncodingRules.DER,
            UniversalTagNumber.PrintableString,
            out _,
            out int charsWritten);
        return charsWritten;
    }

    [Benchmark]
    public int Write()
    {
        _writer.Reset();
        _writer.WriteCharacterString(UniversalTagNumber.PrintableString, s_text);
        return _writer.GetEncodedLength();
    }

    private static byte[] EncodeText()
    {
        AsnWriter writer = new(AsnEncodingRules.DER);
        writer.WriteCharacterString(UniversalTagNumber.PrintableString, s_text);
        return writer.Encode();
    }
}
```

| Method | Runtime   | Mean     | Ratio |
| ------ | --------- | -------- | ----- |
| Read   | .NET 10.0 | 1.242 μs | 1.00  |
| Read   | .NET 11.0 | 263.9 ns | 0.21  |
| Write  | .NET 10.0 | 1.555 μs | 1.00  |
| Write  | .NET 11.0 | 428.3 ns | 0.28  |

Once all input is available, hashing needn’t retain reusable state. SHA-1 is no longer suitable for security decisions such as signing new content, but .NET still needs it for compatibility identifiers such as an assembly’s public-key token. [dotnet/runtime#120674](https://github.com/dotnet/runtime/pull/120674) adds a one-shot path to the internal implementation used for those non-secret purposes. Its hash state, work area, and padding buffer can live on the stack. `AssemblyName.GetPublicKeyToken()` now uses the one-shot path as it has the complete public key available for a single operation:

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using System.Reflection;
using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[MemoryDiagnoser(false), HideColumns("Job", "Error", "StdDev", "RatioSD", "Median")]
public class Benchmarks
{
    private static readonly AssemblyName s_an = typeof(object).Assembly.GetName();

    [Benchmark]
    public byte[]? GetPublicKeyToken() => ((AssemblyName)s_an.Clone()).GetPublicKeyToken();
}
```

| Method            | Runtime   | Mean     | Ratio | Allocated | Alloc Ratio |
| ----------------- | --------- | -------- | ----- | --------- | ----------- |
| GetPublicKeyToken | .NET 10.0 | 1.458 μs | 1.00  | 664 B     | 1.00        |
| GetPublicKeyToken | .NET 11.0 | 721.4 ns | 0.49  | 296 B     | 0.45        |

AES key wrap is used to encrypt cryptographic keys before they’re stored or sent elsewhere. Wrapping or unwrapping one key requires applying AES many times. In .NET 10 on Windows and Apple platforms, the implementation performed each of those steps through a general-purpose helper that created a native AES cipher, processed one block, and then destroyed the cipher. The public `Aes` object could be reused, but internally a single key-wrap operation still repeated that native setup and cleanup, with the number of repetitions growing with the size of the key material. [dotnet/runtime#129921](https://github.com/dotnet/runtime/pull/129921) from [@vcsjones](https://github.com/vcsjones) changes the Windows implementation to create one native cipher and reuse it for the entire wrap or unwrap operation. [dotnet/runtime#129911](https://github.com/dotnet/runtime/pull/129911) does the same for Apple’s implementation.

```
// dotnet run -c Release -f net10.0 --filter "*" --runtimes net10.0 net11.0

using BenchmarkDotNet.Running;

using System.Security.Cryptography;
using BenchmarkDotNet.Attributes;

BenchmarkSwitcher.FromAssembly(typeof(Benchmarks).Assembly).Run(args);

[MemoryDiagnoser(false), HideColumns("Job", "Error", "StdDev", "Median", "RatioSD")]
public class Benchmarks
{
    private const int PlaintextLength = 4096;
    private static readonly byte[] s_key = new byte[32]; // Fixed AES-256 key.

    private readonly Aes _aes = Aes.Create();
    private byte[] _plaintext = [];
    private byte[] _ciphertext = [];
    private byte[] _encryptDestination = [];
    private byte[] _decryptDestination = [];

    [GlobalSetup]
    public void Setup()
    {
        _aes.Key = s_key;

        _plaintext = new byte[PlaintextLength];
        new Random(42).NextBytes(_plaintext);

        int wrappedLength = Aes.GetKeyWrapPaddedLength(PlaintextLength);
        _ciphertext = new byte[wrappedLength];
        _aes.EncryptKeyWrapPadded(_plaintext, _ciphertext);
        _encryptDestination = new byte[wrappedLength];
        _decryptDestination = new byte[PlaintextLength];
    }

    [Benchmark]
    public byte[] EncryptKeyWrapPadded()
    {
        _aes.EncryptKeyWrapPadded(_plaintext, _encryptDestination);
        return _encryptDestination;
    }

    [Benchmark]
    public int DecryptKeyWrapPadded()
    {
        _aes.TryDecryptKeyWrapPadded(_ciphertext, _decryptDestination, out int written);
        return written;
    }

    [GlobalCleanup]
    public void Cleanup() => _aes.Dispose();
}
```

| Method               | Runtime   | Mean     | Ratio | Allocated | Alloc Ratio |
| -------------------- | --------- | -------- | ----- | --------- | ----------- |
| EncryptKeyWrapPadded | .NET 10.0 | 2.497 ms | 1.00  | 264 KB    | 1.00        |
| EncryptKeyWrapPadded | .NET 11.0 | 111.3 μs | 0.045 | 88 B      | 0.00033     |
| DecryptKeyWrapPadded | .NET 10.0 | 2.473 ms | 1.00  | 264 KB    | 1.00        |
| DecryptKeyWrapPadded | .NET 11.0 | 123.5 μs | 0.050 | 88 B      | 0.00033     |

Certificate validation can be dominated by work outside the signature math. For example, during revocation checking on Linux, a downloaded certificate revocation list (CRL) is persisted to disk. A later chain build could therefore avoid the network, but it still needed to open the file, read it, parse the encoded CRL, and create a new native handle. For .NET 11, [dotnet/runtime#123562](https://github.com/dotnet/runtime/pull/123562) adds a bounded in-memory cache of parsed CRLs. A repeated lookup can reuse the native CRL handle directly, while least-recently-used eviction and GC-assisted aging prevent the cache from retaining entries indefinitely.

Authority Information Access (AIA) presents a related problem. A certificate can name a URL from which a missing issuer certificate may be downloaded, and multiple concurrent chain builds may all discover the same missing issuer. [dotnet/runtime#130456](https://github.com/dotnet/runtime/pull/130456) reuses the cache infrastructure so those builds share one asynchronous download rather than issuing duplicate requests. Failed downloads aren’t cached, old successful responses are refreshed in the background, and Linux now limits each chain build to two AIA downloads, matching Windows and bounding the amount of network work one chain can trigger.

## What’s Next?

Whew! Several hundred performance improvements later, .NET 11 is indeed one louder. If any of the examples in this post look like code in your applications, please try the latest [.NET 11 release candidate](https://dotnet.microsoft.com/download/dotnet/11.0) and measure your own workloads. If something got faster, we’d love to hear about it. If something got slower, we’d also love to hear about it. And if you have ideas for how .NET 12 can be turned up even louder, we’re all ears.

Happy coding!

## Author

![Stephen Toub - MSFT](https://devblogs.microsoft.com/dotnet/wp-content/uploads/sites/10/2022/03/stoub_square-96x96.jpg)

Distinguished Engineer

Stephen Toub is a Distinguished Engineer at Microsoft.
