---
title: You should try Gleam
link: https://nchagnet.eu/blog/you-should-try-gleam
source: lobste-rs
published: 2026-11-10T03:33:27Z
updated: 2026-11-10T03:33:27Z
first_seen: 2026-11-11T15:04:08.373515189Z
authors:
- nchagnet.eu via trousers
labels:
- gleam
summary: Comments
content: extracted
html: 2026-11-10-you-should-try-gleam.html
---

## Introduction

If you have read my recent [rant about programming languages](https://nchagnet.eu/blog/desert-island-languages/), you'll know I've been quite interested in [Gleam](https://gleam.run). Gleam is a functional language that targets BEAM, the Erlang VM,[^1] with a simple syntax and interoperability with the wider BEAM ecosystem (Erlang and Elixir). Interestingly, Gleam can *also* target JavaScript i.e., Gleam can be compiled into JavaScript and run through Node.js.

I've been mostly using Rust for hobby projects, which at the moment is the only language that really makes coding enjoyable for me, and Gleam has managed to scratch that itch too. If you're a Rust *connaisseur*, then Gleam will feel familiar to you, bar a few key differences. I feel like 80% of the code I write could be copy pasted to Rust (with a tinge of search-and-replace for syntax) and it'd compile.

This post won't give an exhaustive overview of the language, if you want to learn more, I recommend the [excellent interactive tour](https://tour.gleam.run). You'll get a good feel for Gleam in under half an hour.

## Gleam: the good

On its homepage, Gleam is described as "a **friendly** language for building **type-safe** systems that **scale**!" (emphasis theirs) which does a good job of conveying what Gleam is all about.

### Gleam is friendly

The syntax is extremely simple, most of the complex stuff is hidden by the Erlang runtime which you don't really have to interact with unless you want to! Of course, Gleam is a functional language, so if you have lived in a C-shaped imperative world your whole life, it might be a bit daunting at first. But if you compare it to other functional languages (OCaml, Haskell, Erlang), you'll quickly notice Gleam is much simpler. After using it a bit, it feels like all the functional parts of Rust, with a few missing ones, and none of the imperative parts.

Let's start from an example piece of code:

Gleam

```gleam
// Omitted imports
pub fn main() {
  let total =
    "1,X,5,6" // String
    |> string.split(on: ",") // List(String)
    |> list.map(int.parse) // List(Result(Int, Nil))
    |> result.values // List(Int), the invalid entries are dropped
    |> list.reduce(fn(acc, x) { acc + x }) // Result(Int, Nil)
    |> result.unwrap(0) // Int

  echo total // 12
}
```

Rust

```rust
fn main() {
    let total = "1,X,5,6" // &str
        .split(",")
        .map(|s| s.parse::<i64>())
        .filter_map(Result::ok)
        // Could have used .sum() here
        .reduce(|acc, x| acc + x)
        .unwrap_or(0);

    println!("{}", total); // 12
}
```

Dissecting this a bit, you can see a few specificities of Gleam:

- variables in Gleam are *immutable* (so no `mut` keyword or anything like that), chained functions here all return a new element.[^2] You can still *shadow* a binding with a new `let`, which is how you "update" a value.
- `|>` (typed |>) is the pipe operator used to chain functions on data. The main constraint is that the piped variable should be the first argument of the function in the pipe.
- just like Rust, errors in Gleam are just values as part of the `Result` [monad](https://en.wikipedia.org/wiki/Monad_\(functional_programming\)).[^3]

Comparing with the Rust code snippet, you can see that, up to a few naming choices, they look pretty similar. The main difference in this example is really the piping vs. chaining syntax. **Gleam types have no methods** so piping is used instead.

The piping operator `|>` is the first of the three mascot features of Gleam you will meet in this post, the other two being the `use` pattern and tail-call optimization.

#### The use pattern

If you've written a lot of JavaScript, you might be used to the indentation creep that arises when nesting many callbacks into one another

```javascript
someFunc(arg, function(param) {
  someOtherFunc(param, function(otherParam) {
    // ...
  })
})
```

In Gleam, many functions expect a callback, for example because as a way to control access to a resource and clean up when the process is done (closing a database connection, releasing a file handle, etc.). To avoid this kind of tower of callbacks, Gleam provides some syntactic sugar: `use param <- some_func(arg)` is equivalent to `some_func(arg, fn(param) { ... })`. The callback must always be the **last** argument of the function. That is the opposite of `|>`, which fills the first one.

With use

```gleam
use param <- some_func(arg)
use other_param <- some_other_func(param)
echo other_param
```

Without use

```gleam
some_func(arg, fn(param) {
  some_other_func(param, fn(other_param) {
    echo other_param
  })
})
```

#### Tail-call optimization

This one might genuinely trip you up if you're not familiar with it. The first thing you need to know is **there are no loops in Gleam**! Again, if you've written OCaml or Haskell before, you won't be surprised by this. In 98% of cases,[^4] loops are simple operations over bounded iterations. Think `for` loops on a list or a set. Exactly the kind of thing which in Rust you always have two ways of doing: one imperative with the exact `for` loop, one more functional with a combination of operations on iterators. Well, if you look at the code example in the previous section, you'll see Gleam uses the second kind and has a plethora of functions to iterate over lists, sets, tuples, strings, anything.

But what if you need something more complicated? What about the 2%? This is where you would instead use **recursion**! A simple algorithm which illustrates this is Euclid's algorithm to compute the Greatest Common Divisor (GCD). Below you can find a Rust iterative example, and a Gleam recursive example.

Rust

```rust
fn gcd(mut a: u64, mut b: u64) -> u64 {
    while b != 0 {
        let remainder = a % b;
        a = b;
        b = remainder;
    }
    a
}
```

Gleam

```gleam
fn gcd(a: Int, b: Int) -> Int {
  case b {
    0 -> a
    _ -> gcd(b, a % b)
  }
}
```

Of course, you can also write the recursive version in Rust, but this usually isn't such a great idea. Recursion in imperative languages can quickly get out of hand: every call adds a new stack frame, and a deeply recursive function can blow the stack (the infamous **stack overflow**).

On the BEAM there is no fixed-size stack, so runaway recursion eats heap memory until the process is killed instead, but the point remains: deep recursions equals trouble. To avoid that problem, compilers for functional languages implement a technique called Tail Call Optimization (TCO), which applies when the recursive call is the last thing the function does. The existing stack frame is then reused instead of stacking a new one, giving performance close to an imperative `while` loop.[^5]

### Gleam is type-safe

This piece of the slogan is mostly there to answer the obvious question "why Gleam and not Elixir?" (another Erlang-based language). While Elixir is a dynamically-typed language (with gradual typing now!), Gleam was *built* as a statically-typed language with a strong type system: strong types are its *raison d'être*.

A `type` in Gleam encompasses both the `enum` and the `struct` concepts in Rust:

Gleam

```gleam
// A usual enum
type Directions {
  Left
  Right
}

// A 'struct-like' type
type Person {
  Person(name: String, age: Int)
}

// A combination of both
type MyError {
  Missing
  InvalidQuery(String)
}
```

Rust

```rust
enum Directions {
  Left,
  Right
}

struct Person {
  name: String,
  age: i64
}

enum MyError {
  Missing,
  InvalidQuery(String)
}
```

Because Gleam types are algebraic data types, they are very useful to ensure undesired states are impossible. A great aspect of Gleam, which will be relevant a little bit later, is that it compiles to *both* the BEAM virtual machine *and* to JavaScript. So you can share these strong type definitions across different applications in both targets. This is particularly handy if you need a web application to communicate with a BEAM app.

### Gleam is made for systems that scale

That, of course, refers to BEAM, the Erlang VM. Unlike languages which target the native system (Rust, Go, C++), a VM provides an extra abstraction layer between the system and the program. In the case of BEAM, that layer is exceptionally good at handling concurrent processes and failures while remaining fast.

Gleam makes good use of that, and this is what a concurrent program would look like:

```gleam
// Omitted imports
pub fn main() {
  let order_number_and_amounts = [
    #(101, 100),
    #(102, 200),
    #(103, 300)
  ]
  
  order_number_and_amounts
  |> list.each(fn(x) {
    let #(order_no, amount) = x
    let order_description = "Order #" <> int.to_string(order_no)

    // Spawn unlinked: a crashing worker must not take the parent down.
    process.spawn_unlinked(fn() {
      // 101 and 103 will succeed, 102 will crash
      process_order(order_description, amount)
    })
  })

  io.println("All orders dispatched!")

  // Keep the parent alive until the children have had time to run.
  process.sleep(10)
}

fn process_order(order: String, amount: Int) {
  process.sleep(1)
  case amount {
    200 -> panic as "Payment failed!"
    _ -> io.println(order <> " processed successfully")
  }
}

// Stdout
// ----
// All orders dispatched!
// Order #101 processed successfully
// Order #103 processed successfully
```

What you should take away from this code is that it's the BEAM that handles processes.

The call to `process.spawn_unlinked` triggers a new process with no link to its creator, so when order 102 crashes, neither the parent nor siblings are affected. With a plain `process.spawn` the child would be linked to the caller, and that `panic` would have taken the whole program down. If you look closely, I also added `process.sleep(10)` so the parent wouldn't close *before* the children could finish: this is a hack, since the delay is only a guess and a slower child would still be cut off.

Thankfully, a great advantage of the BEAM is how easy inter-process communication is, which the following code piece illustrates well:

```gleam
// Omitted imports
pub type PaymentError {
  PaymentFailed
}

pub fn main() {
  let parent = process.new_subject()
  let orders = [
    #(101, False),
    #(102, True),
    #(103, False)
  ]

  orders
  |> list.each(fn(x) {
    let #(order_no, should_fail) = x

    process.spawn(fn() {
      // Simulate processing time
      process.sleep(1000)

      // 101 and 103 succeed, 102 fails
      let res = case should_fail {
        True -> Error(PaymentFailed)
        False -> Ok(Nil)
      }

      process.send(parent, #(order_no, res))
    })
  })

  io.println("All orders dispatched!")

  receive_results(list.length(orders), parent)
}

fn receive_results(
  remaining: Int,
  parent: process.Subject(#(Int, Result(Nil, PaymentError))),
) -> Nil {
  case remaining {
    0 -> Nil

    _ -> {
      let #(order_no, res) = process.receive_forever(parent)

      let status = case res {
        Ok(_) -> "successful"
        Error(_) -> "failed"
      }

      io.println(
        "Processing of order #" <> int.to_string(order_no) <> " " <> status <> "!",
      )

      receive_results(remaining - 1, parent)
    }
  }
}

// Stdout
// ----
// All orders dispatched!
// Processing of order #103 successful!
// Processing of order #102 failed!
// Processing of order #101 successful!
//
// The order of the last three lines depends on scheduling.
```

### OTP: the gift that keeps on giving

In the paragraph above, I briefly showed how the BEAM virtual machine is great for concurrent programs communicating by message passing. If you're familiar with Go, this is similar to how it handles the same problem with *goroutines*. But the OTP (Open Telecom Platform), the set of libraries that ships with Erlang, is more powerful than that. It provides you with a built-in supervisor-actor paradigm which enables you to create scalable, concurrent services which can crash without taking down the whole application.

In a nutshell, the OTP exposes *actors*: lightweight processes that hold state and communicate with each other via messages (like in the example above). When paired with a *supervisor*, the whole application becomes more resilient to any individual actor crashing: the supervisor will monitor processes and restart them as needed. It can also help start/stop groups of actors dynamically.

If you're reading this and thinking "uh that sounds a lot like how I used Kubernetes", you're not alone. The OTP model is extremely similar to the controller-pod architecture of Kubernetes and solves a lot of the same problems.[^6] However, in my experience, *a lot* of workflows using Kubernetes don't really need a cluster but still use one (and that's expensive!) because of the added bonus of the distributive experience. For those cases, the OTP provides a really interesting alternative.

Let me give you a concrete example. Without going in too much detail, we have a common setup in my team which involves a handwritten Go job scheduler handling some other processes.

The scheduler, each worker, and the user interface are all pods inside the cluster, managed by Kubernetes. To be honest, this works well, but it does introduce a lot of complexity. First of all, most of these services do not need to be so isolated from each other. The consequence here is that we need duplicate REST APIs in various places to enable communication between different services.

What if we wanted to reorganize this within a Gleam application? There are two parts where Gleam would shine here:

1. the OTP would essentially unify all scheduling services as actors within a single application,
2. the user interface (currently a Node.js app) can be written in Gleam and compiled to JavaScript.

Here is the revised architecture:

Within the BEAM application, all actors would communicate via messages:

- the HTTP server receives requests and saves them to the DB,
- the workers would pick up jobs and write the status back to the DB.

So instead of handling the deployment of multiple applications in separate pods, you can just let the OTP handle all process management within one VM (for the scheduling part).

#### What does it mean to reuse code between the frontend and the backend?

As mentioned before, one of the big advantages of Gleam is that it compiles to JavaScript. So you can bundle shared type definitions between the user interface and the job scheduler instead of keeping duplicate APIs in sync.

shared/job.gleam

```gleam
pub type JobStatus {
  Pending
  Running
  Completed
  Failed
}

pub type Job {
  Job(
    id: String,
    name: String,
    status: JobStatus,
  )
}

pub type CreateJobRequest {
  CreateJobRequest(name: String)
}

pub fn new_job(id: String, request: CreateJobRequest) -> Job {
  Job(
    id: id,
    name: request.name,
    status: Pending,
  )
}
```

frontend.gleam

```gleam
import gleam/json
import shared/job.{type CreateJobRequest, CreateJobRequest}

// `create_job_encoder` lives in shared/job; `client` is your HTTP client
// of choice (gleam_httpc on the BEAM, gleam/fetch in the browser).

pub fn submit_job(name: String) {
  let request = CreateJobRequest(name: name)

  let body = json.to_string(
    job.create_job_encoder(request),
  )

  // POST /api/jobs
  client.post(
    "/api/jobs",
    body,
  )
}
```

server.gleam

```gleam
import shared/job.{type CreateJobRequest, type Job}

// `Database`, `DatabaseError` and `generate_id` stand in for your
// database layer and ID generator.

pub fn create_job_handler(
  request: CreateJobRequest,
  db: Database,
) -> Result(Job, DatabaseError) {
  let id = generate_id()
  let created_job = job.new_job(id, request)

  // Persist as Pending in PostgreSQL.
  database.insert_job(db, created_job)
  |> result.map(fn(_) { created_job })
}
```

## Gleam: the bad

Every language has its flaws, and it's time for some honest feedback on Gleam. I don't pretend I have much experience with the language yet, so if any of these complaints feel unfair to you, feel free to [correct me](mailto:hi@nchagnet.eu). So far, these are the things I noticed:

- the ecosystem is small. This is not Gleam's fault per se, just a consequence of being a young language,[^7] and there are mitigating factors such as the possibility to use *external* functions quite easily (Erlang, Elixir on BEAM, JavaScript on its own runtime) directly from Gleam. Yet, the community itself favours Gleam-first packages and as of today, there are some gaps in package variety.
- The language is *too simple*. Well, to be honest I don't *agree* per se with this, I like it like this. But I totally understand that the lack of macros or other complex features can put a few people off (for example, I read that [currying](https://en.wikipedia.org/wiki/Currying) is dearly missed).
- The BEAM virtual machine is an *excellent* target, but it's also a peculiarity. I couldn't really name any big company/system using it. Yet, a quick search shows that WhatsApp, Discord and Pinterest, as well as Cisco, Nintendo, and even the NHS rely on it. Erlang and BEAM are quietly powering huge platforms due to their reliability, but it's not as flashy as "Made in Rust" marketing. Where Rust had Mozilla, Go had Google and Java had Oracle, there isn't a big tech player pushing for Gleam/Erlang right now.

Of course this is a very biased account. I think I'm missing a lot of flaws in the language itself, flaws that will show themselves once the honeymoon period is over. But for now, I'm having a blast with Gleam, and I can only encourage you to give it a try too! I'm sure just like me, you'll have a lot of fun!

[^1]: If like me this doesn't mean much to you, Erlang is *also* a functional language, albeit a much older one (about 40 years old), whose primary use has been for telecom systems. The BEAM shines with distributed, concurrent, fault-tolerant systems (so, telecom, obviously).

[^2]: One important thing about immutable languages is that when you do something like `let new_list = list.take([1, 2, 3], 2)`, you're not making a *copy* of the data (that'd be very wasteful), instead the common data in both variables comes from the same spot in the memory.

[^3]: Gleam also has the `Option` monad via `gleam/option`, but unlike Rust, it's somewhat discouraged and a simple `Result(T, Nil)` is preferred when possible. This is why `reduce` in Gleam returns `Result(T, Nil)` while Rust returns `Option<T>`.

[^4]: This is a completely made-up statistics, but I feel it's not too far from reality.

[^5]: Rust does not guarantee TCO. LLVM sometimes applies sibling-call optimisation, but it is not something you can rely on, and explicit tail calls are still unstable. Since Rust *also* gives you loops, the imperative style is generally the safer choice. Compilers for functional languages are usually much more aggressive here, since TCO is crucial to their performance.

[^6]: The main difference is really about isolation: an OTP node is a single OS process, and all the actors on it share the same memory, while Kubernetes gives each pod its own container and spreads them over the machines of the cluster. Erlang does know how to cluster several nodes together, but OTP has no cluster scheduler, no placement and no container isolation: you still have to deploy and place the nodes yourself.

[^7]: Gleam's exact age is debatable. According to [Wikipedia](https://en.wikipedia.org/wiki/Gleam_\(programming_language\)), Gleam was initially created in 2016, but it would only be considered a "serious" endeavour starting from 2019 (when it got numbered versioning) and only reached version 1.0.0 in 2024.
