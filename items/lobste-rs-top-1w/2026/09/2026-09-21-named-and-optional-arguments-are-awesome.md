---
title: Named and Optional Arguments are Awesome
link: https://botahamec.dev/named-optional-args
source: lobste-rs-top-1w
published: 2026-09-21T21:16:53Z
updated: 2026-09-21T21:16:53Z
first_seen: 2026-09-22T22:00:05.825522006Z
authors:
- botahamec.dev via jado
labels:
- plt
- rust
summary: Comments
content: extracted
html: 2026-09-21-named-and-optional-arguments-are-awesome.html
---

Whenever someone asks what my least favorite part of Rust is, my answer is always the same: it doesn't have named or optional arguments.

I have a small list of programming languages I tolerate: Rust, C#, TypeScript, Dart, Python. Of those languages, Rust is the hardest to get named arguments out of. I'm going to start this post by going over the other languages and how they've achieved named arguments. Then, we'll look at Rust, the problems that have resulted from not having named arguments, and proposals for how Rust could get them in the future.

## How Other Languages Handle Named Parameters

### Dart

Dart is designed specifically for graphical user interfaces. GUI components tend to have lots of optional parameters, so Dart tried very hard to get them right. And I think it does the best job out of any language we're going to talk about today.

First, let's look at a normal function, just to familiarize ourselves with the language:

```
Widget Text(String text) {
	// ...
}
```

That's pretty normal as far as languages go. You can probably guess what it does, even if you don't know Dart. Dart was basically designed in a lab to be easy for programmers to learn.

Now, let's add a named parameter to set the size of the text, and default it to 16 pixels:

```
Widget Text(String text, {int fontSize = 16}) { /* ... */ }

// usage:
Text("Hello, world!", fontSize: 24)
```

Named parameters are wrapped in curly braces. This makes them look similar to a map. Named parameters can also be defaulted to null, or even required.

```
Widget Text({
	int fontSize = 16,
	Color? color,
	required String text,
});
```

Dart also supports optional positional arguments by wrapping the parameter in square brackets.

```
// From the Dart documentation
String say(String from, String msg, [String device = 'carrier pigeon']) {
  var result = '$from says $msg with a $device';
  return result;
}

assert(say('Bob', 'Howdy') == 'Bob says Howdy with a carrier pigeon');
assert(say('Bob', 'Howdy', 'smoke signal') == 'Bob says Howdy with a smoke signal');
```

### C#

C#'s handling of named parameters is also pretty good. In fact, any parameter can be named.

```
void ExampleMethod(string foo, string bar)
{
	Console.WriteLine($"{foo} {bar}")
}

ExampleMethod(bar: "Hello", foo: "world");
```

And any parameter can have a default value, as long as they're specified after the required parameters.

```
void ExampleMethod(string foo, string bar = "world")
{
	Console.WriteLine($"{foo} {bar}")
}

ExampleMethod("Howdy");
```

And that's all there is to it. I like the simplicity of it.

### TypeScript

TypeScript's handling of named parameters is not very good, in my opinion. But optional positional parameters aren't named, and they work pretty well, so let's start with that.

```
function example(required: string, optional?: string = "world"): string {
	return `${required} ${optional}`;
}

example("Hello") === "Hello world";
example("Hello", "TypeScript") === "Hello TypeScript";
```

That seems fine to me. If you don't specify the default value, then it gets set to `undefined`. The problem is that there isn't a good built-in syntax for named parameters, so we have to use other TypeScript features to hack it in.

```
function example(
	required: string,
	{
		requiredNamed,
		namedOptional = "world"
	}: { requiredNamed: string, namedOptional?: string }
): string {
	return `${required} ${requiredNamed} ${namedOptional}`;
}

example("Hello", { requiredNamed: "beautiful" });
```

Here, we're taking advantage of both anonymous types and destructuring. The second parameter of the function has an anonymous object type, which contains two fields: `requiredNamed` and `namedOptional`. Then we destructure this parameter so we can use the fields in the function.

The most annoying part of this is that we have to define the field names twice. Once for the type, and once for the function parameters. This is unnecessary verbosity that any good language should try to avoid. The function call is also more verbose than necessary, because of the curly braces.

Unfortunately, this syntax is used all of the time in React. Components are typically a function that takes one argument, which is an object. So when you define a component, you have to do this constantly.

### Rust

Some people have tried to do named parameters in Rust. This generally doesn't work very well. The most common approach is to define a struct for the function, and then have it implement the `Default` trait, so that we can end the struct initializer with `..Default::default()`.

```
struct ExampleParams {
	foo: String,
	bar: String,
}

impl Default for ExampleParams {
	fn default() -> Self {
		Self {
			foo: "Hello".into(),
			bar: "Hello".into(),
		}
	}
}

fn example(Example { foo, bar }) -> String {
	format!("{foo} {bar}")
}

assert_eq!(
	example(Example { foo: "Hello".into(), ..Default::default() }),
	"Hello world".to_string()
);
```

I complained before about how verbose TypeScript's version is, but this is even worse, because the types are not anonymous. And worse, if your default values are not the default of the types (e.g. empty string for `String`), then you need to write a whole function to set the default values. It also only works if every single parameter has a default value. If only some of the parameters have default values, then you need an even more verbose solution.

## Why Rust Needs Optional Parameters

The standard library doesn't tend to follow the above pattern (for good reason, in my opinion). But there are still several cases where the standard library probably would have benefitted from it.

Take some of the factories of `HashMap`, for example.

```
impl<K, V> HashMap<K, V, RandomState> {
	pub fn new() -> HashMap<K, V, RandomState>;
	pub fn with_capacity(capacity: usize) -> HashMap<K, V, RandomState>;
}

impl<K, V, A: Allocator> HashMap<K, V, RandomState, A> {
	pub fn new_in(alloc: A) -> Self;
	pub fn with_capacity_in(capacity: usize, alloc: A) -> Self;
}

impl<K, V, S> HashMap<K, V, S> {
	pub const fn with_hasher(hash_builder: S) -> HashMap<K, V, S>;
	pub fn with_capacity_and_hasher(capacity: usize, hasher: S) -> HashMap<K, V, S>;
}

impl<K, V, S, A: Allocator> HashMap<K, V, S, A> {
	pub fn with_hasher_in(hash_builder: S, alloc: A) -> Self;
	pub fn with_capacity_and_hasher_in(capacity: usize, hasher: S, alloc: A) -> Self;
}
```

With just three optional parameters, we need eight functions to represent all of the overloads. Each one of them needs to have their own name, since Rust doesn't actually have overloads. And the programmer and/or code reviewer must either memorize or look up the order of the parameters when one of the more complex factories are used.

Another problem that comes up is the fact that, when the parameters are unnamed, it can be easier to make mistakes. Consider the following print function.

```
fn print(text: &str, bold: bool, italics: bool, underline: bool);
```

It's easy to mix up the parameters here because they don't have names. If I forget that the `italics` parameter is third and not second, then I can end up accidentally bolding my text instead.

Some functions just have lots of parameters as well. Imagine if I added color, underline color, background color, blinking text, hidden text, circled text, and fast blinking text above. Nobody wants to pass in 14 arguments to a function, of which several will have sensible defaults. And I don't see much reason to name a new struct if it's only going to be used for the one function.

## Proposals for Named Arguments

First I'm going to describe some proposals that I've seen for improving named arguments in Rust, that I don't like. Then I'll reveal what my preferred solution is.

### Default Field Values

This feature is already available in Nightly Rust. And the `syn` create even recently added support for it. The idea is to make implementing the `Default` trait easier by adding special syntax for it. It's not proposed that this will solve named arguments by itself, but might be part of a larger solution.

```
#[derive(Default)]
struct Pet {
	name: Option<String>, // impl Default for Pet will use Default::default() for name
	age: i128 = 42, // impl Default for Pet will use the literal 42 for age
}

// Pet { name: Some(""), age: 42 }
let _ = Pet { name: Some(String::new()), .. };
// Compilation error: `name` needs to be specified
let _ = Pet { .. };
// Pet { name: None, age: 42 }
let _ = Pet::default();
```

I don't actually have a problem with the proposal itself. I even created my own library to implement a polyfill for it ([feluments](https://www.lib.rs/feluments)). It seems perfect for structs. But it doesn't help very much for functions.

### Structural Records

The RFC for default field values mentions a closed RFC for structural records. They can be thought of as anonymous structs or tuples with named fields.

```
fn do_stuff_with(color: { red: u8, green: u8, blue: u8 }) { // More ergonomic!
	some_stuff(color.red); // *And* readable! :)
	...
	other_stuff(color.green);
	...
	yet_more_stuff(color.blue);
}

do_stuff_with({ red: 255, green: 127, blue: 63 });
```

The RFC notes that this emulates named arguments, although it does not emulate optional arguments. It wasn't a major motivation. The major motivation was to get the convenience of tuples with the readability of structs.

Let's say for the sake of argument that this RFC succeeded. What would the syntax of our "Hello, world" examples look like here? Let's assume default field values are also merged, and they work with structural records.

```
fn example(
	required: String,
	{requiredNamed, namedOptional}: {
		requiredNamed: Option<String>,
		namedOptional: String = "world".into()
	}
): String {
	format!("{required} {requiredNamed} {namedOptional}")
}

example("Hello", { requiredNamed: "beautiful", .. });
```

Wow, that looks familiar. In fact it's the same syntax that TypeScript uses, with all the problems that come with it. You need to specify the parameter names twice.

Structural records also come with many other challenges:

- What happens with records that have only one field? `{ x }` can be interpreted as either a block expression that returns `x`, or a structural record expression: `{ x: x, }`. To fix the ambiguity, the RFC proposed requiring an extra comma: `{ x, }`, like what tuples do. But this requires making the Rust parser much more complicated, to do several tokens of lookahead.
- It's impossible to implement any traits on these. The RFC proposed magically implementing several traits. And unlike for tuples, we can't just define some generic trait implementations, because there can be so much variation in the field names.
- A newcomer to the language may mistake this syntax for a Python dictionary or Dart's map type.

Interestingly, TypeScript actually has named tuple fields. But they only exist at the type declaration. You can't use the field names to access the fields.

```
type NewLocation = [lat: number, long: number]

const newLocations: NewLocation[] = [
    [52.3702, 4.8952],
    [53.3498, -6.2603]
]

const firstLat = newLocations[0][0]
const firstLong = newLocations[0][1]
```

Rust's language team decided that although something resembling this feature seems useful, it would be very hard to implement and not worth the effort.

### Copy C#

If we're already agreeing on a syntax for optional fields in structs, why not just copy that over to functions? First, we can allow the arguments to be specified in any order, as long as they are named, and no out-of-order positional arguments are following a named argument.

```
print_order_details(order_num: 31, product_name: "Red Mug", seller_name: "Gift Shop");
print_order_details(seller_name: "Gift Shop", product_name: "Red Mug", order_num: 31);
print_order_details("Gift Shop", 31, product_name: "Red Mug");
print_order_details(seller_name: "Gift Shop", 31, product_name: "Red Mug");

// this would cause an error
print_order_details(product_name: "Red Mug", 31, "Gift Shop");
```

Then, we could define default values, and allow parameters with default values to be unspecified.

```
fn print_order_details(product_name: &str, order_num: 31, seller_name: &str = "Gift Shop");

print_order_details("Red Mug", 31);
```

Unlike for the default field values proposal, we can't use a `..` at the end here, because it is also a valid expression.

The biggest problem with this idea is that it makes parameter names part of the public API of the function, without functions opting into it. There was previously an objection on the grounds of it conflicting with the proposed type ascription feature. But the RFC for it has since been deleted, so I don't think we need to worry about that anymore.

### Public Arguments

As far as I know, a formal RFC was never made for this, but a pre-RFC was posted to the Rust Internals Forum several years ago, and it's the most thorough proposal for named arguments that I know of. I'll avoid copy-pasting the entire RFC here, but I'll give a high level overview.

Here's an example of a function with named arguments from the RFC:

```
pub struct Database;
pub struct RegistrationError;

pub fn register(
	pub name: String,
	pub surname: String,
	to db: Database
) -> Result<(), RegistrationError> {
	/* ... */
}

register(name: "Alexis".into(), surname: "Poliorcetics".into(), to: my_db);
```

In the case of `to db`, the name of the argument is `to`, but it's referred to as `db` within the body of the function. For the other two parameters, the `pub` keyword is used to say that that both the name of the argument when calling the function, and the name of the parameter in the function body, are the same.

The RFC goes into way more detail, proposing function overloads, using named arguments with the `Fn` trait, exposing names from patterns, method overloads, and explaining the documentation aspect.

Here are the criticisms I saw in the comments:

- Unordered named arguments was ruled out. I don't like the justification RustyFrog gave for this, but it seems easy to support with this model.
- Default parameter values also weren't proposed, even though it's the main reason for wanting named parameters
- Despite not proposing default parameter values, the pre-RFC did propose overloads, which are very difficult to implement in Rust, and they're not as useful as optional arguments. It's not proposing type-based overloads, which is a bit better, but still.
- It's a very big RFC, which might make it difficult to get it approved.
- There are some syntax ambiguities, like `fn foo(name (a, b): name)`, which is currently valid syntax. There was also a proposed type acription feature that it conflicted with, but as I said before, that RFC has since been removed.
- Using `pub` might be confusing, given how it's used currently. I don't think I mind that personally, because its effect is to make the name of the parameter part of a function's public API. But I get how that's a bit weird.
- The proposal for changing the `Fn` trait seemed underbaked. The `Fn(f32, f32) -> String` trait gets desugared to `Fn<(f32, f32), Output = String>`, but it's not clear what the desugaring for named parameters should look like.
- Despite explaining the usage for `Fn` implementations, the `fn` type was not mentioned at all.
- The pre-RFC was not clear about if named parameters were required to be used by name, or if having the correct position was technically sufficient.
- The handling of extern functions with named arguments is fairly complicated to the point where it would probably be better to not handle them.

My understanding is that the author got busy and didn't address these concerns, but I think the pre-RFC serves as a good starting point.

### Struct-style function calls

This idea was proposed back in 2016 by nixpulvis. I've also seen it floating around in some other places.

```
// 1. Tuple style method call (current functions).
fn foo(a: u32) {}
foo(2);

// 2. Struct style method call.
fn foo { a: u32 }
foo { a: 2 };
// SomeType { a: 1 } -> SomeType
// SomeFunc { a: 1 } -> Codomain
// generally_lowercase { a: 1 } -> Codomain

// 3. Together.
fn foo(a: u32) { a: u32 } {}
foo(1) { a: 2 };
```

I personally don't like it because it makes struct constructions and function calls look too similar. I also have no idea how the compiler would parse this. But then again, most languages use `new Foo()` syntax for constructors, so maybe somebody will like it.

### Copy Dart

Another proposal that's come up, including from myself, is to copy the Dart syntax, like so:

```
fn foo({ foo: &str = "Hello", bar: &str = "World" }) -> String {
	format!("{foo} {bar}")
}
```

To me, this looks too much like existing pattern syntax. But I do like that it doesn't require an extra keyword on each argument.

### Edition Separation

Some people have proposed solving the backwards-compatibility problem by only allowing named arguments to be used with functions that were written in a future edition of Rust. To me, this feels like too large of a breaking change to be worth considering.

### Dot Prefixes

An RFC was opened in 2020 proposing using a dot prefix to distinguish between named and unnamed variables. It was not merged, because the RFC was a very incremental step that didn't explain the future plans for how the named arguments would work. Here's an example from the PR:

```
fn split(string: &str, .at: char, .limit: usize, .case_sensitive: bool) {}

split("hello world", .at = ' ', .limit = 2, .case_sensitive = true);
```

The dot was chosen because it's only one character, (as opposed to four characters, with the `pub` keyword plus a space). But I honestly don't think it's intuitive enough. The dot is also reminds the user of struct fields, which is a completely unrelated concept.

## My Proposal

After reading all of these proposals, I feel that all of the pieces exist for a complete named arguments proposal that lack any of the downsides. I get the impression that some people are going to be opposed to the concept no matter what, but I tried to address as many counter-arguments for named arguments as possible in this post. The rest of this post will be a pre-RFC that think will satisfy as many people as possible. I'll monitor the comments anywhere this gets posted to see if there are any major criticisms. I can't guarantee that I'll actually make an RFC, but permission is granted to create an RFC if I don't. I think it's been a while since named argument were last seriously proposed, so I think now is a good time for a new proposal.

### Summary

Add named and optional arguments to functions. Functions can have both positional and named arguments. In function calls, named arguments may be referred to by name, to increase readability and maintainability. Named arguments may also have default values, avoiding the need to specify them in the function call.

### Motivation

#### Improve safety and readability

Misremembering which parameter is at which position is a major source of bugs in many languages, including Rust. Take the function from the standard library: `[hard_link](https://doc.rust-lang.org/stable/std/fs/fn.hard_link.html)`.

```
pub fn hard_link<P: AsRef<Path>, Q: AsRef<Path>>(
	original: P,
	link: Q,
) -> Result<()>
```

Both parameters accept the same types, so there's no way to know which one is which without looking at the documentation. The consequence of misremembering is a runtime error or a bug.

With named parameters, it is easy to see what is happening at the call-site. This also makes the resulting code more readable.

```
hard_link(original: "a.txt", link: "b.txt")?;
```

The effect is more pronounced for functions which have more parameters.

```
// Without named arguments: what are these numbers?
solar_elevation(1787517098.0, 38.897957, -77.036560)

// With named arguments, it's obvious
solar_elevation(timestamp: 1787517098.0, latitude: 38.897957, longitude: -77.036560);
```

A similar effect is possible in Rust today, by creating a new struct and having that be the function's only parameter. But it's very verbose to create, so in practice, few libraries take advantage of it. Importantly, the standard library rarely uses this pattern.

#### Boilerplate reduction

Many functions have reasonable defaults that should be passed into many of their parameters, but because all positional arguments are required, all of the parameters must be specified. Passing `None` into every parameter comes with readability issues.

```
repository.checkout_index(None, None)?;
```

Passing a struct into the parameters does give us names, but still requires us to specify `None` for the optional values. Sometimes this boilerplate can be reduced by adding `..Default::default()` to the end of the constructor, `Default` cannot be implemented if any of the fields are not optional. Implementing `Default` is also a lot of boilerplate if it cannot be implemented simply by using the derive macro. Some of these problems can be addressed by [RFC 3681](https://github.com/rust-lang/rfcs/pull/3681), but it still requires a new struct to be defined specifically for one function.

```
let instance = Instance::new(InstanceDescriptor {
	backends: Backends::default(),
	flags: InstanceFlags::default(),
	memory_budget_thresholds: MemoryBudgetThresholds::default(),
	backend_options: BackendOptions::default(),
	display: None,
});
```

To help reduce the boilerplate of calling such a function, many libraries implement builder types that don't require every value to be specified. This comes at the cost of even more boilerplate to create the builder, which becomes part of the library's public API. There are crates, such as [bon](https://www.crates.io/crates/bon) to make declaring a builder for a struct easier, but it still requires a struct be declared for a single function. Libraries don't always use these crates either, as doing so increases compile times. The `Command` struct uses 60 SLOCs (and many more lines of documentation) to allow us to do this:

```
Command::new("printenv")
	.stdin(Stdio::null())
	.stdout(Stdio::inherit())
	.env_clear()
	.envs(&filtered_env)
	.spawn()
	.expect("printenv failed to start");
```

If we had named arguments, the following function would be easy to define:

```
spawn(
	command: "printenv",
	stdin: Stdio::null(),
	stdout: Stdio::inherit(),
	env_clear: true,
	envs: &filtered_env
).expect("printenv failed to start");
```

Bon also provides utilities for emulating named arguments in functions, but calling such a function involves some boilerplate as well.

```
// Example from the bon README

#[builder]
fn greet(name: &str, level: Option<u32>) -> String {
	let level = level.unwrap_or(0);

	format!("Hello {name}! Your level is {level}")
}

// This could be shortened to greet(name: "Bon", level: 24)
let greeting = greet()
	.name("Bon")
	.level(24)
	.call();

assert_eq!(greeting, "Hello Bon! Your level is 24");
```

In practice, the added complexity means that these patterns are only ever used in public APIs, so private functions have no worthwhile workaround to improve readability.

#### Extensibility without breaking changes

Currently, adding a new parameter to an existing function is a breaking change. If you want to add a new field to a struct, your existing factory must implement a reasonable default value, and a new function must be created to enable the new functionality.

This leads to some APIs having many functions which do similar things, but with different parameters. For example, `HashMap` from the standard library has the following factories.

```
impl<K, V> HashMap<K, V, RandomState> {
	pub fn new() -> HashMap<K, V, RandomState>;
	pub fn with_capacity(capacity: usize) -> HashMap<K, V, RandomState>;
}

impl<K, V, A: Allocator> HashMap<K, V, RandomState, A> {
	pub fn new_in(alloc: A) -> Self;
	pub fn with_capacity_in(capacity: usize, alloc: A) -> Self;
}

impl<K, V, S> HashMap<K, V, S> {
	pub const fn with_hasher(hash_builder: S) -> HashMap<K, V, S>;
	pub fn with_capacity_and_hasher(capacity: usize, hasher: S) -> HashMap<K, V, S>;
}

impl<K, V, S, A: Allocator> HashMap<K, V, S, A> {
	pub fn with_hasher_in(hash_builder: S, alloc: A) -> Self;
	pub fn with_capacity_and_hasher_in(capacity: usize, hasher: S, alloc: A) -> Self;
}
```

All of these functions are shown in the documentation, polluting the API. The number of functions is exponential with respect to the number of parameters. If we ignore the differences in generic parameters (a possible solution to which is discussed in the "Future possibilities" section), then we could imagine having a single function.

```
pub fn new(
	pub capacity: usize = 0,
	pub alloc: A = Global,
	pub hasher: S = RandomState,
);
```

If we wanted to some day add a `fill` parameter, we'd be able to add it to this existing function without it being a breaking change, and without doubling the number of factories for `HashMap`.

### Guide-level explanation

Parameters may be named or unnamed. By default, all parameters to a function are unnamed, and cannot be referred to by name. We can create a named parameter by adding the `pub` keyword before its name.

```
fn print_labeled_measurement(pub value: i32, pub unit_label: char) {
	println!("The measurement is: {value}{unit_label}");
}

fn main() {
	print_labeled_measurement(5, 'h');
}
```

At first, this may not seem very different from unnamed parameters. But when a parameter is named, we may use its name when calling the function. This makes it easier to tell, at a glance, what the values being passed into the function are meant to do.

```
print_labeled_measurement(value: 5, unit_label: 'h');
```

When parameters are named, they may also be re-ordered, so you don't need to remember the position of each one.

```
print_labeled_measurement(unit_label: 'h', value: 5);
```

There are some caveats to the ordering. A function may contain both named and unnamed parameters. But if it does, all named arguments must be specified after the positional arguments, both in the function declaration and in the function body.

```
// unit_label is named, value is not
fn print_labeled_measurement(value: i32, pub unit_label: char) {
	println!("The measurement is: {value}{unit_label}");
}

// forbidden: all named parameters must be after all unnamed parameters
fn _print_labeled_measurement(pub value: i32, unit_label: char) {
	// ...
}

fn main() {
	// forbidden: the parameter that 'h' is being passed into must be named
	print_labeled_measurement(value: 5, 'h');
	// forbidden
	print_labeled_measurement(unit_label: 'h', 5);
}
```

Named parameters may also have default values. This means that the caller does not need to specify the argument value when calling the function. The default value is using automatically.

```
fn print_labeled_measurement(pub value: i32, pub unit_label: char = 'm') {
	println!("The measurement is: {value}{unit_label}");
}

fn main() {
	// unit_label is unspecified and is defaulted to 'm'
	print_labeled_measurement(5)
}
```

Named parameters cannot be used if the parameter does not have a name and is defined as a pattern instead. For example, the following is not valid.

```
fn print_coordinates(pub &(x, y): &(i32, i32)) {
	// ...
}
```

However, a name can be given to this parameter by using an `@` binding.

```
fn print_coordinates(pub point @ &(x, y): &(i32, i32)) {
	// ...
}
```

Traits may also choose to use named parameters, in which case the implementation must use the same name. However, trait function parameters cannot have default values.

```
trait Trait {
	fn f(pub a: i32, pub b: i32);
}

impl Trait for () {
	// This is okay, because the parameter names match
	// unused parameter warnings can be ignored using @
	fn f(pub a: i32, pub b @ _: i32) {}
}

impl Trait for i32 {
	// invalid: the second argument is named `b` in the definition
	fn f(pub a: i32, pub c: i32) {}
}

impl Trait for bool {
	// invalid: `b` is not public
	fn f(pub a: i32, b: i32) {}
}

impl Trait for X {
	// invalid: trait methods cannot have default parameter values
	fn f(pub a: i32, pub b: i32 = 0) {}
}
```

Because the parameter names are optional, function pointers may be created for functions with named parameters. However, the name will not be accessible on the function pointer, and default values cannot be used. This also applies for the `Fn*` family of traits.

```
fn foo(pub a: i32, pub b: i32) {}

let f: fn(i32, i32) = foo;

f(4, 2);       // ok
f(a: 4, b: 2); // ERROR! `f` can't be called with named arguments

fn higher_order(f: Fn(i32, i32)) {
	f(4, 2);        // ok
	f(a: 4, b: 2);  // ERROR! `f` can't be called with named arguments
}
higher_order(foo);       // ok
higher_order(|_, _| {}); // ok
```

### Reference-level explanation

#### Grammar

The only needed change to support public parameters in function declarations is to function parameters. First, we add the `pub` keyword, which must be followed by an identifier pattern. Such a pattern may also include a default value.

```
FunctionParamPattern = PatternNoTopAlt ":" ( Type | "..." )
                     | "pub" IdentifierPattern ":" ( Type | "..." ) ( "=" Expr )?
```

For function calls, the syntax is slightly more complicated to parse, but only affects `CallParams`.

```
CallParam  = Expression ( ":" Expression )?
CallParams = CallParam ( "," CallParam )* ","?
```

This is slightly more complicated to parse, because an identifier is also a valid expression. To solve this, we'll just parse an entire expression, and then check to see if there's a colon following it. The compiler can later give an error if the argument name is anything other than an identifier.

#### Static semantics

##### Function parameters

Given a `FunctionParamPattern` where the default is specified, i.e.:

```
FunctionParamPattern = "pub" pat:IdentifierPattern ":" ( ty:Type | "..." ) ( "=" expr:Expr )
```

Similar rules to the rules for default field values apply. Namely,

- If the function is `const`, then the expression, `expr` must be a constant expression.
- The expression `expr` must coerce to the type `ty`
- Generic parameters of the current items are accessible.

  ```
  fn foo<const A: usize>(bar: usize = A) {}
  ```

- Default const expressions are not evaluated at definition time, only during instantiation. This means that the following will not fail to compile:

  ```
  fn foo(a: usize = panic!(), b: usize = 42) {}

  foo(a: 0);
  ```

- The function's generic parameters are properly propagated, meaning the following is possible:

  ```
  fn foo<T>(bar: Vec<T> = Vec::new()) {}

  foo::<T>();
  ```

- When lints check attributes such as `#[allow(lint_name)]` are placed on a `FunctionParam`, it also applies to the expression, `expr`.
- Unused parameter names will not emit a warning if they are bound to a different pattern using `@`. The bound pattern may still emit a warning

For public parameters in general, all public parameters must be strictly after all non-public parameters.

##### Function calls

The semantics of function calls change significantly. In general, all named arguments must be specified after all positional arguments. For positional arguments, all existing semantics apply.

In the case of the following production of a named argument:

```
CallParam  = name:Expression ":" value:Expression
```

the following rules apply

- The expression, `name` must be a plain identifier.
- The identifier, `name` must be the name of a public parameter of the function.
- The expression, `value` must coerce the the type of the aforementioned public parameter.

##### Trait declarations

For trait declarations all the same rules apply from function declarations. In addition:

- Trait methods cannot have default parameter values.

##### Trait implementations

The semantics of trait implementations vary slightly from the semantics for trait declarations.

- Any parameter which was declared public on the trait declaration must also be public in the trait implementation.
- Public parameters must have the same name that their declaration has.

### Drawbacks

#### Added complexity

The language would be slightly more complex as a result of this feature. But this proposal is fairly minimal as far as named arguments proposals go. The syntax for named arguments resembles the syntax for struct fields, and the default value syntax is pretty similar to the syntax proposed for default field values.

One concern that can come up in named argument proposals is the problem of it only applying to new functions, which makes it hard to remember which libraries. This proposal can be backported to the standard library, making the language feel more consistent.

#### Named arguments aren't mandatory

The fact that names are optional could be seen as a compromise on the safety benefits. Some people may lazily choose to leave them out, in which case, no safety benefit is provided.

However, the fact that they aren't mandatory allows more library authors to use them without worrying about breaking changes. Some authors may also be more inclined to support named arguments if they felt confident that it would not increase the verbosity of calling the function.

This drawback does present a potential bug. If the names are removed during a refactor, then they may be in the wrong position after the refactor, causing a bug.

```
// before refactor
coordinates(y: 45.6, x: 6784.0);

// after refactor: incorrect order
coordinates(y, x);
```

##### Remedy: Add a clippy lint

A clippy lint can be used to enforce the use of named arguments when possible.

##### Remedy: Require named arguments in a future edition

Although it may be a large breaking change, future editions could likely require the use of named parameters, if it were deemed desirable to do so.

#### Limiting type ascription

Many older proposals for named arguments noted that the syntax chosen here: `name: value` conflicts with the proposed type ascription feature. However, since then, the RFC for type ascription has been removed. And another type ascription RFC was rejected, partially on the basis of it potentially conflicting with named arguments. So this is not likely to be a concern anymore.

### Rationale and alternatives

#### Why make named arguments opt-in?

Although many languages, such as Kotlin and C#, allow any argument to be named, this is not practical for Rust. Changing an argument is currently not a breaking change, but would become one as soon as named arguments are introduced. The solution is to allow authors to decide if their argument names are an implementation detail or not.

#### The `pub` keyword

Using the `pub` keyword may seem like a strange choice, given the semantics. But it has essentially the effect of making the parameter name public, so this is defensible. Other visibility modifiers are not proposed by this RFC, because it's hard to imagine a situation where a public function would want to allow named arguments internally, but not externally.

Other modifiers have been proposed. One could imagine adding an `ext` keyword to define named parameters. A previous RFC proposed using dots, but this was deemed to be too reminiscent of field access. Other symbols, such as `@` may be more acceptable. Another proposed idea is to copy Swift syntax and use two identifiers.

```
fn foo(bar bar: &str) {}
```

Re-typing the same parameter name twice is verbose and usually unnecessary. It's also unnecessary since syntax exists for rebinding a variable already.

Dart wraps named arguments in curly braces. This has the benefit of not requiring the `pub` keyword to be specified multiple times. However, in the context of Rust, this looks like a destructuring pattern, which isn't semantically correct.

```
// In Dart
void enableFlags({bool? bold, bool? hidden})
enableFlags(bold: true)

// Similar idea, in Rust
fn enable_flags({ bold: Option<bool> = None, hidden: Option<bool> = None })
enable_flags(bold: Some(true))
```

Another proposal is to make function calls with named arguments look more like struct constructions by using curly braces instead of parentheses. This seems like it could be very confusing and make the distinction between the two features unclear.

```
fn foo (a: u32) { b: u32 } {}
foo(1) { b: 2 };
```

#### Colon vs equal sign

Some have proposed using an equal sign (`=`) instead of a colon (`:`). This would make the syntax more closely resemble assignment, which is semantically true. It also would not possibly conflict with type ascription.

The colon syntax was chosen for this RFC to be more consistent with struct initializers.

#### Argument re-ordering

Some proposals for named arguments have omitted re-ordering of arguments for simplicity. For the sake of thoroughness, this RFC proposes allowing arguments to be re-ordered. A possible alternative would be to require arguments to still be in the original order. The drawback would be unnecessary friction when the user sorts the arguments out of order.

Some languages allow for more flexible re-ordering than this RFC proposes. C# allows named arguments to appear before unnamed arguments, as long as the arguments are still in order. This RFC omits this feature, in order to make named arguments seem less confusing to users. The reordering proposed here still allows for more flexible argument-reordering in the future.

#### No default values in traits

This RFC currently prohibits traits from making use of default parameter values. This is because it's unclear if default values for trait parameters should be defined at the trait definition or the trait implementation. It would likely be more predictable if default values could be defined by the implementation. But it's hard to imagine how a default value would allow the trait to be dyn-compatible. If the default values were defined at the trait definition, then they could stay dyn-compatible.

#### On const contexts

In the default field values RFC, it was decided that default values for structs must be const. The reasoning is that functions with side-effects may be confusing for the compiler and for users. However, these justifications don't apply to functions, because non-const functions don't carry an expectation of being cheap or consistent. It is possible to always require default values to be const, but there's seemingly no strong reason to do so.

However, it would be confusing if default parameters could be non-const in const functions, which is why they are required to be const in that case.

#### Named arguments aren't mandatory

See the section above in "Drawbacks".

#### Alternative: Do nothing

The "Motivation" section explains thoroughly how the current state is very verbose, to the point where existing alternatives, such a struct parameters and builders are often not used. The existence of these patterns is evidence that users want some way to emulate named arguments. The fact that these alternatives are not used in private APIs is evidence that the status quo is annoyingly verbose.

#### Alternative: Structural Records

It has been proposed many times that structural records, alongside default field values, could emulate named parameters. This is a pattern that is often used in languages like TypeScript. The syntax for declaring such a function would look like this.

```
fn foo({ a, b, c }: { a: f64, b: f64, c: f64 }) {}
```

There are several criticisms I have of this pattern:

- It cannot be backported to older functions without causing a breaking change, so the standard library could not use it.
- Every identifier in this declaration needs to be typed twice. It might be possible to introduce syntactic sugar for this, but not such syntax has been proposed.
- Structural records have had a proposed RFC in the past, but was rejected, mainly due to the complexity of implementing it.

Structural records could serve as a temporary compromise for named arguments, if they are ever implemented. I'm not currently convinced that they ever will be implemented. If the compromise solution is harder to implement than the real solution, then we might as well just use the real solution.

#### Alternative: Struct name inference

One proposal to make named arguments easier is to allow the name of a struct to be inferred when it is constructed. This would make the call sites of functions using struct parameters to look mostly simple.

```
foo(_ { a: 1, b: "2", c: [] });
```

However, this still requires the function to support this function explicitly, by defining a struct to be passed in. It also still cannot be backported to existing functions. The only benefit this provides over the status quo is to avoid typing the name of the struct at the call site, which is not the main criticism of this pattern. It would still be unlikely for private functions to use this pattern.

#### Alternative: Implementing `Fn` traits

I've seen this come up a couple of times, but I think most of the people suggesting this are doing so as a joke. Implementing `Fn` would be a more verbose form of the method overload feature that some other languages have. Overloads are difficult and controversial to implement in Rust, so it's unlikely this pattern would be adopted.

### Prior art

Several previous attempts at named arguments, both in Rust and in other languages, have been described throughout this RFC.

A previous RFC for named arguments exists for Rust. It was rejected mainly because it did not describe what the future of named arguments would look like with optional arguments. This RFC attempts to address as many future uses of named arguments as possible in order to give a full picture.

A pre-RFC was proposed by forum user Azerupi, but no RFC seemed to come as a result of it. Several complications were pointed out by commenters. This RFC attempts to address syntactic ambiguity, the type system, and opt-in naming.

Another pre-RFC proposed using a more Swift-style syntax, which was avoided in this RFC. The pre-RFC also proposed overloading but not default arguments. Arguably the latter is more important than the former.

As was mentioned before, bon offers a procedural macro for creating builders from functions. The existence of this feature shows that there is a desire for named arguments, that would be better handled as a language feature.

Other lanaguages with named arguments have also been discussed. The feature proposed here is similar to named arguments from C# or Kotlin, with the difference being that thhe named arguments are opt-in. The syntaxes used by Dart and Swift have also been discussed. Dart uses the following syntax, which looks closer to destructuring than named arguments.

```
void enableFlags({bool? bold, bool? hidden})
enableFlags(bold: true)
```

Swift makes all arguments named by default, allowing authors to opt-out by using an underscore before the variable name. This is impossible to port to Rust without making a breaking change to the language. Various proposals have tried to use slightly different syntaxes, but many of them present syntactic ambiguities.

### Unresolved questions

- Should we require named arguments to be given in position order?
- Is `pub` the best keyword, or should a different keyword be used?
- Should default values be required to always be const?

### Future possibilities

#### Default parameter values for trait methods

Trait methods were forbidden from having default values due to the question of if the default value should be on the trait declaration or the trait implementation. Having it be on the implementation would be consistent with how associated types and constants work. But this would make it hard for such a trait to be dyn-compatible.

One solution would be to only allow default values to be used in non-dyn contexts.

```
trait Foo {
	fn bar(&self, baz: i32 = 0);
}

impl Foo for i32 {
	fn bar(&self, baz: i32 = i32::MAX) {}
}

fn do_bar<T: Foo>(x: &T) {
	x.bar(); // baz doesn't need to be specified, since we're using generics
}

fn do_bar_2(x: &dyn T) {
	x.bar(); // ERROR: baz must be specified when using a method on a dyn object
}
```

#### Extending traits

Theoretically, there's nothing preventing the following, as long as a default parameter value is provided:

```
impl<T> Default for Vec<T> {
	fn default(capacity: usize = 0) -> Self {
		Self::with_capacity(capacity)
	}
}
```

Although, in addition to making the behavior of the `default` function inconsistent, this is even harder to resolve in a `dyn` context. The likely remedy would be to forbid `Vec` from being converted into a `dyn Default`, which would make introducing the named parameter a breaking change.

#### Requiring named arguments in a future edition

Allowing named arguments to be optional is necessary in order to backport them into existing functions without creating breaking changes. This comes with some compromises, as has been described in the "Drawbacks" section. However, the way this RFC is drafted makes it possible to require named arguments in a future edition of Rust. Specifying a named argument would not give a warning in either the previous edition or the new edition.

Doing this too early would likely hurt adoption of named arguments, as backporting them into existing functions would become a breaking API change. This would also increase the verbosity of calling functions which make use of named parameters, hurting adoption further.

#### More flexible re-ordering

As mentioned before, re-ordering of named arguments is allowed by this RFC, in a limited way. The limitations are kept in order to keep the rules easy to explain and reason about. However, some languages allow arguments to be re-ordered more flexibly, and this would be possible to do in the future.

C#'s main rule is that out-of-order named arguments cannot be followed by positional arguments.

```
print_order_details(order_num: 31, product_name: "Red Mug", seller_name: "Gift Shop");
print_order_details(seller_name: "Gift Shop", product_name: "Red Mug", order_num: 31);
print_order_details("Gift Shop", 31, product_name: "Red Mug");
print_order_details(seller_name: "Gift Shop", 31, product_name: "Red Mug");

// this would cause an error
print_order_details(product_name: "Red Mug", 31, "Gift Shop");
```

#### Defaults Affect Inference

In 2022, Gankra proposed using default parameter values as part of inference for generic type parameters. We could imagine something like this.

```
fn default_hasher() -> RandomState { ... }

impl <K, V, S: BuildHasher> HashMap<K, V, S> {
    fn new(hasher: S = RandomState::default()) { ... }
}

// uses S=RandomState
let map = HashMap::new();
```

The downside of this particular approach, as Gankra pointed out, is that this would only allow the argument to be omitted if the type of `S` is `RandomState`. Although, that's already true with the current set of `HashMap` factories, unless you use `default` which doesn't have any parameters at all.

#### Generic const

In the case of `Vec`, adding an optional `capacity` parameter is not possible with this iteration of the RFC, because `Vec::new` is a `const` function, and allocating memory cannot be done at compile-time. This is a very difficult problem to solve, and it's possible that it never will. However, if keyword generics are added to the language, it may be possible to do something like this:

```
const<C> trait Capacity: Copy {
	const<C> fn allocate<T>(self) -> Option<Box<[MaybeUninit<T>]>>;

	const fn as_usize(self) -> usize;
}

const impl Capacity for () {
	const fn allocate<T>(self) -> Option<Box<[MabyUninit<T>]>> {
		None
	}

	const fn as_usize(self) -> usize {
		0
	}
}

impl Capacity for usize {
	fn allocate<T>(self) -> Option<Box<[MaybeUninit<T>]>> {
		Some(Box::default())
	}

	const fn as_usize(self) -> usize {
		self
	}
}

const<C> impl<T, Cap: const<C> Capacity> Vec<T> {
	pub const<C> fn new(capacity: Cap = ()) -> Self {
		Self {
			len: 0,
			capacity: capacity.as_usize(),
			buffer: capacity.allocate(),
		}
	}
}
```

If no argument value is provided, then the function would be `const`. But when a `usize` is provided, then it would no longer be `const`.
