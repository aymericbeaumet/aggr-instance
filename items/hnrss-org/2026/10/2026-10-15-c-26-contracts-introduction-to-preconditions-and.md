---
title: 'C++26 Contracts: Introduction to Preconditions and Postconditions'
link: https://www.cppstories.com/2026/cpp26_contracts/
source: hnrss-org
published: 2026-10-15T13:11:26Z
updated: 2026-10-15T13:11:26Z
first_seen: 2026-10-16T00:17:27.373653594Z
authors:
- ibobev
content: extracted
html: 2026-10-15-c-26-contracts-introduction-to-preconditions-and.html
extra:
  comments_url: https://news.ycombinator.com/item?id=50103164
  num_comments: 113
  points: 124
---

![](https://www.cppstories.com/2026/images/contracts.png)

Table of Contents

In the article about [Standard Library hardening](https://www.cppstories.com/2026/hardening-experiments/), we looked at checks for invalid Standard Library container access and other broken preconditions. I mentioned that those requirements are expressed in terms of “contracts”. But what about our code?

In this post, we’ll explore contracts from C++26. We’ll start with a simple `cassert`, move the requirement into the function itself, and see how to check results. We’ll also look at declarations and definitions, compiler options, and a few details that can surprise you.

## Starting with an Assertion

Let’s start with a small example:

A header file first:

```cpp
// foo.h
#include <vector>
int get_value(const std::vector<int>& values, std::size_t index);
```

And the implementation:

```cpp
// foo.cpp
#include "foo.h" // our function declaration...
#include <cassert>

int get_value(const std::vector<int>& values, std::size_t index) {
    assert(index < values.size());
    return values[index];
}
```

The function expects a valid index. If the condition is false, `assert` prints a diagnostic message and terminates the program. However, this only happens when assertions are enabled. Defining `NDEBUG` disables the check and basically removes the check from the code.

What are the issues with this simple approach? The requirement lives inside the implementation. Somebody reading only the declaration sees a vector and an index, without the relationship between them.

## Adding a Precondition

With C++26 contracts, we can move that requirement from an assertion inside the function body to the function itself:

```cpp
int get_value(const std::vector<int>& values, std::size_t index)
    pre(index < values.size())
{
    return values[index];
}
```

Notice the new `pre(...)` syntax after the parameter list. It introduces a *precondition assertion*: a condition expected to hold when entering the function.

What’s more, we can add this precondition to the declaration:

```cpp
int get_value(const std::vector<int>& values, std::size_t index)
    pre(index < values.size());
```

The feature was introduced through [P2900, Contracts for C++](https://wg21.link//p2900).

Let’s understand how it works and how it differs from `assert`.

### Why Use Contracts Instead of `assert`?

At first, `pre(index < values.size())` might look like another way to write `assert(index < values.size())`. Both express a condition that should be true. So what’s the benefit of using contracts?

There are a few important differences:

- Requirements in declarations: We can put `pre` and `post` directly in the function declaration. This makes the requirements visible to anyone reading the API, without looking at the implementation.
- More flexible checking: Traditional `assert` is enabled or disabled through `NDEBUG`. Contracts define several evaluation semantics, including ignoring a check, reporting a violation and continuing, or terminating the program. The compiler implementation determines which semantics are available and how they can be selected.
- Preconditions and postconditions: Contracts distinguish between what the caller must provide and what the function promises to return. With `assert`, we would have to place checks manually inside the function body, including checks before different return statements.

This also makes contracts useful for documenting APIs, even when runtime checking is disabled.

## A Complete Precondition Example

Here’s a complete example (no header file for simplicity for now):

```cpp
#include <print>
#include <vector>

int get_value(const std::vector<int>& values, std::size_t index)
    pre(index < values.size())
{
    return values[index];
}

int main() {
    std::vector<int> values { 10, 20, 30 };

    std::println("{}", get_value(values, 1));
    // get_value(values, 10); // violates the precondition
}
```

The valid call prints:

```text
20
```

And here’s the version for you to play with the problematic call uncommented:

See at [Compiler Explorer](https://godbolt.org/z/v9c4hr4va)

On GCC, I’m getting:

```
Program returned: 134
Program stdout
20
Program stderr
contract violation in function int get_value(const std::vector<int>&, std::size_t) at /app/example.cpp:6: index < values.size()
[assertion_kind: pre, semantic: enforce, mode: predicate_false, terminating: yes]
terminate called without an active exception
Program terminated with signal SIGABRT (6)
```

## Contracts on Declarations and Definitions

For clarity, contracts can appear on a declaration, in a definition, or both:

```cpp
int get_value(const std::vector<int>& values, std::size_t index)
    pre(index < values.size());
```

The definition can then omit the contract specifier:

```cpp
int get_value(const std::vector<int>& values, std::size_t index)
{
    return values[index];
}
```

The contract assertions are established by the function’s first declaration. A later declaration or definition may omit the contract specifiers, as above, or repeat the same sequence.

A later declaration cannot introduce a different set of preconditions or postconditions.

In practice, if a function is declared in a header, that first declaration is the natural place for its preconditions and postconditions. If you’re reading the API, it’s probably best to see all preconditions without looking inside the implementation.

What’s more, contracts are associated with the function, but they are not part of the function type. Overload resolution, for example, does not distinguish functions based on their contracts.

## Contract Evaluation Semantics

C++26 defines four evaluation semantics:

| Semantic      | What happens at runtime?                                                      |
| ------------- | ----------------------------------------------------------------------------- |
| Ignore        | The predicate is not evaluated.                                               |
| Observe       | A violation invokes the handler; execution continues if it returns normally.  |
| Enforce       | A violation invokes the handler; execution terminates if it returns normally. |
| Quick-enforce | A violation terminates execution without invoking the handler.                |

The implementation determines which semantic applies. C++26 has no portable syntax for forcing an individual ordinary contract assertion to use one specific policy. These rules appear in the [contract evaluation specification](https://eel.is/c++draft/basic.contract.eval).

With the ignore semantic, no runtime check is performed. With observe, a violation is reported, but execution continues if the handler returns normally. In our example, both policies can allow the program to reach `values[index]` with an invalid index, resulting in undefined behavior.

Thus, adding `pre(...)` expresses the requirement, while the selected policy determines how checking responds.

## Compiling with GCC

As of October 2026, only GCC 16 supports Contracts. We can use it with special compile flags:

```sh
g++ -std=c++26 -fcontracts \
    -fcontract-evaluation-semantic=enforce example.cpp
```

With that policy, the invalid call invokes the violation handler. If the handler returns normally, execution terminates before the function body performs the invalid access. GCC’s default handler emits diagnostic information. See the [GCC options documentation - contracts](https://gcc.gnu.org/onlinedocs/gcc/C_002b_002b-Dialect-Options.html#index-fcontracts).

GCC also allows us to replace the default contract-violation handler. For example, we can log a custom message when a violation is detected.

The handler receives a `std::contracts::contract_violation` object, which provides information about the failed contract, including the predicate, location, and evaluation semantic.

```cpp
#include <contracts>
#include <print>

void handle_contract_violation(
    const std::contracts::contract_violation& violation)
{
    std::println(stderr, "Contract violated: {}",
                 violation.comment());
}
```

See an example [@Compiler Explorer](https://godbolt.org/z/YT941W6vx)

Notice that replacing the handler doesn’t change what happens after it returns. With `observe`, execution continues. With `enforce`, the program terminates.

Not every implementation is required to support replacing the handler, so this feature is implementation-defined.

## Adding a Postcondition

So far, we’ve described what the caller must provide. We can also describe what a function promises when it returns normally.

See this example:

```cpp
int clamp_value(int value, const int low, const int high)
    pre(low <= high)
    post(result: result >= low && result <= high)
{
    if (value < low)
        return low;
    if (value > high)
        return high;
    return value;
}
```

There are two useful pieces here:

- `pre(low <= high)` checks that the bounds form a valid interval.
- `post(result: ...)` checks that the returned value lies inside that interval.

The name `result` is ours to choose. It refers to the function’s result within that postcondition.

What’s convenient is that the same postcondition covers every normal return path. We don’t have to repeat an assertion before each `return`.

Here’s an example where I deliberately violated the postcondition:

```cpp
#include <print>

int clamp_value(int value, const int low, const int high)
    pre(low <= high)
    post(result: result >= low && result <= high)
{
    if (value < low)
        return low-10; // <<
    if (value > high)
        return high;
    return value;
}

int main() {
    std::print("{}", clamp_value(10, 20, 30));
}
```

See [@Compiler Explorer](https://godbolt.org/z/c9asf75M5)

And on GCC I’m getting:

```text
contract violation in function int clamp_value(int, int, int) at /app/example.cpp:5: result >= low && result <= high
[assertion_kind: post, semantic: enforce, mode: predicate_false, terminating: yes]
terminate called without an active exception
Program terminated with signal SIGABRT (6)
```

The precondition is fine, but the postcondition is broken.

### Const Parameters in Postconditions

There’s also a detail in the parameter list: `low` and `high` are `const`.

You will get the following error if you try making them non-const:

```
source>:5:28: error: a value parameter used in a postcondition must be const
    5 |     post(result: result >= low && result <= high)
```

When a postcondition uses a parameter passed by value, that parameter must have `const` type on every declaration. Our comparisons use both bounds this way. The parameter `value` is only used in the body, so this requirement doesn’t apply to it.

Postconditions apply to normal exit; they do not check the result when the function body exits through an exception. See the [function contract rules](https://eel.is/c++draft/dcl.contract).

## Assertions Inside a Function

For conditions inside an implementation, we have `contract_assert`:

```cpp
#include <string_view>

std::size_t count_spaces(std::string_view text) {
    std::size_t count = 0;

    for (char ch : text) {
        if (ch == ' ')
            ++count;
    }

    contract_assert(count <= text.size());
    return count;
}
```

The condition is simple, but it expresses a useful property: the number of spaces cannot exceed the number of characters.

In a larger algorithm, such a check could describe a relationship between counters, an intermediate result, or the state after processing a block of data.

Together, `pre`, `post`, and `contract_assert` cover function entry, normal exit, and points inside a function.

## Avoiding Mutation in Predicates

One thing to watch during migration is mutation inside predicates:

```cpp
void process(int count)
    pre(++count > 0) // error
{
}
```

See [@Compiler Explorer](https://godbolt.org/z/9j4TMbfzn)

An error from GCC:

```text
error: increment of read-only location '(const int)count'
    2 |     pre(++count > 0) // error
```

Contract predicates use special constification rules. In this example, count is treated as const inside the predicate, so `++count` is rejected by the compiler.

However, this does not make predicates completely free of side effects. For example, a predicate can still call a function that modifies global state. Since contract evaluation may be ignored, and side effects are not guaranteed even under checking semantics, predicates should not modify program state.

## Contracts Are Not Input Validation

There’s another practical distinction: input validation.

Suppose an index comes from a configuration file. If an invalid value requires an error message, that handling belongs in ordinary control flow:

```cpp
if (index >= values.size()) {
    std::println("Invalid index: {}", index);
    return;
}

std::println("{}", get_value(values, index));
```

The validation handles an expected failure at the application boundary. The precondition documents the internal helper’s requirements.

P2900 explicitly discusses this idea in:

> Principle 12: Contract Assertions Are Not Flow Control: While a contract assertion provides an algorithm to validate correctness, nothing about a contract assertion guarantees any particular runtime behavior associated with that syntactic construct.

And more:

> Importantly, this aspect of Contracts is why contract assertions must not be used for error handling and input validation: If a function has in-contract requirements to report certain events as errors, that handling must be done with standard C++ control statements that are not optional, never with contract assertions.

## Considering Predicate Costs

Finally, one note about performance implications.

Our index comparison is cheap. A condition that verifies whether an entire collection is sorted needs to examine its elements. An expensive check may be useful during testing, but you should measure its effect on representative workloads.

## Summary

C++26 contracts give us three ways to express assumptions and guarantees directly in the language:

- `pre(...)` describes conditions expected to hold when a function is entered,
- `post(...)` describes conditions expected to hold when a function returns normally,
- `contract_assert(...)` checks properties at specific points inside a function.

Preconditions and postconditions are associated with the function through its declarations. In particular, the first declaration establishes the contract, which makes declarations in header files a natural place to document an API’s requirements.

A contract assertion does not necessarily mean that a runtime check will always be performed. The selected evaluation semantic can ignore, observe, enforce, or quick-enforce the assertion. Code therefore shouldn’t depend on contract predicates for required application logic or side effects.

Contracts also aren’t a replacement for normal input validation. Use regular control flow for errors that your program expects and needs to handle.

References

- [P2900, Contracts for C++](https://wg21.link//p2900)
- [Contract assertions (since C++26) - cppreference.com - C++ Reference](https://cppreference.dev/w/cpp/language/contracts)
- [How C++26 Contracts Improve Correctness Without Sacrificing Performance | Citadel Securities](https://www.citadelsecurities.com/careers/career-perspectives/how-cpp26-contracts-improve-correctness-without-sacrificing-performance/)
- [How C++26 contracts work | Train IT](https://train-it.eu/blog/how-cpp26-contracts-work/)
- [Test Driving C++26 Contracts with GCC 16.1 | Fabian Koehler](https://fkoehler.me/posts/2026/05/test-driving-cpp26-contracts-with-gcc-16-1/)
