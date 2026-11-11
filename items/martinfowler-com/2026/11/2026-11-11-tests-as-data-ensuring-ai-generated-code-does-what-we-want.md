---
title: 'Tests as data: ensuring AI-generated code does what we want'
link: https://martinfowler.com/articles/tests-as-data.html
source: martinfowler-com
published: 2026-11-11T15:17:00Z
updated: 2026-11-11T15:17:00Z
first_seen: 2026-11-11T21:51:32.649893059Z
authors:
- Martin Fowler
content: extracted
html: 2026-11-11-tests-as-data-ensuring-ai-generated-code-does-what-we-want.html
remote_preview:
  url: https://martinfowler.com/articles/tests-as-data/card.png
---

Ensuring AI-generated code does what we want

## Introduction

### Problems with AI-generated code

In early 2026, I conducted interviews with 26 developers working for an organization that was heavily invested in developing with AI coding agents. One of my questions was “do you trust our tests?” After all, the developers were from Thoughtworks, and Thoughtworks prides itself on the quality and thoroughness of our testing. Unfortunately, the developers invariably answered “*Well… I don’t trust our tests that much! Because even if I review the tests when they are initially generated, as new features get added, the AI refactors both existing code and existing tests, so I’m no longer confident that the tests are still ensuring the right behaviour”*.

This got me thinking. As we learn to get more work done from coding agents, it’s becoming natural that AI writes most, if not all the tests.

The practice of developers writing automated tests was a major breakthrough in software engineering in the late 20th century. The most practical way to do this is to write the tests before the code (test-first) and to write and make pass one test at a time (test-driven development). This practice allowed teams developing business software to deliver nearly bug-free code, at a fraction of the cost of formal specifications and proofs.

In 2026, a lot of code is written by AI coding agents, and those agents will generate tests even without asking, so one could hope that we can achieve the same level of correctness that human coders enjoyed with TDD. Unfortunately, it’s a common experience that this is not the case. In fact, there have been a number of reports published in early 2026 showing that the quality of code being deployed by organizations is getting worse.

I don’t know for certain what the causes are of this widely reported decrease in external quality, but I think it’s plausible that one of the causes may be a decline in quality of the automated functional tests.

### Problems with AI-generated tests

When we give high autonomy to AI to generate code, we need to trust the AI to write good tests, as the alternative of writing all the tests by hand is no longer realistic. Let’s get back to the problem reported by the developers who stopped trusting the tests when the AI refactored them. Why does the AI refactor the tests? Most likely, it needs to refactor them because the signature of a function changes as the code evolves, and this is expected to happen in the course of development, so the AI is not “wrong” when it does this. But reading AI-generated test code is even more boring than reading AI-generated production code, so it’s not realistic to expect developers to carefully re-read all changed test code at every step.

Another problem with AI-generated tests is that they might be tightly coupled to the implementation. If we ask a coding agent to “cover module X with unit tests”, it will happily generate a lot of tests, and often these tests will use lots of mocks and stubs. The purpose of tests is to break when the code returns the wrong result, or produces the wrong side effects. Unfortunately, tests that use mocks and stubs heavily tend to break whenever the implementation of the stubbed and mocked code changes, even when the change is a harmless refactoring that preserves correctness. So these tests produce a lot of false positives. What’s worse is that these tests might not give us confidence that they will protect us when the code does the wrong thing. Instead of proving that “when input is X, output must be Y”, these tests might just prove that “when we invoke function A, it will invoke function B”, which is not useful, because it’s not proving that the code will produce the correct results. In fact, when we ask AI to cover something with tests, we’d like the tests to specify the correct behaviour, but most often, the AI will write tests that confirm the current implementation, including any bugs it may contain.

So in summary, AI-generated tests are often problematic because:

- The volume of test code is large
- Test code is hard to read
- The tests are coupled to the implementation, and
- They might not even prove that the code is correct

We could mandate that human developers write all the tests, and write good tests, but this will negate most of the expected economic benefit of coding agents, and is not realistic given where software engineering seems to be heading. But what if there was a way to write tests that is much easier for humans to both read and write? This is what we’ll cover in what follows.

### Problems with AI-generated specs

A lot of AI-assisted development is done using some form of [spec-driven development](https://martinfowler.com/articles/exploring-gen-ai/sdd-3-tools.html): vendors [recommend it](https://claude.com/resources/articles/the-ai-native-sdlc-playbook) and some SDD frameworks are very popular. While it seems clear that aligning with AI before development is very useful, e.g., by getting the AI to ask clarifying questions, there are problems with the generation of “specifications” written as prose in Markdown documents.

One problem is that, again, the volume of generated prose makes it hard for developers to read it all. After a point, fatigue kicks in and we risk accepting the AI-generated plans after little more than a cursory glance, in what someone called [*cognitive surrender*](https://addyosmani.com/blog/cognitive-surrender/). Another problem is maintenance: once a feature is complete, what do we do with all the specs? Some frameworks support maintaining a set of durable specs, but the risk, again, is that over time the volume of these specs grow too large for maintenance. A third problem is that large-language models do not comply with all of the detailed specifications; it can happen that the AI gives up on some parts of the spec in order to claim “done” sooner.

Finally, and most fundamentally, English is ambiguous: while we can try to specify what we want as precisely as we can, natural language does not afford the level of precision that we get with programming languages. In short, prose specs can't be executed; tests can.

### What I am proposing

Our goal is to have tests that exhibit these properties:

1. They [predict success in production](https://coding-is-like-cooking.info/2025/12/test-desiderata-2-0/): when all the tests pass, it gives us confidence that the functionality is implemented correctly and it will behave as expected in production
2. They are resistant to refactoring
3. They are as easy to read as possible

To achieve this, we propose these techniques:

- **Customer tests**: write in the language of the customer.
- **Black box**: test the system, or components of the system from the outside, without mentioning implementation details of the production code.
- **Tests-as-data**: focus on the important details: the input, the expected output or side effects, and the test setup. In other words, the test should capture *just the facts*, without mentioning implementation details in the test code.
- **Test DSL**: find the most straightforward ways to represent the input, the output and the system state in text that can be understood by a domain expert, and produces a clean *diff* when the test fails.

When AI writes the code, the developers become in a way customers. The tests are a contract that establishes what the code should do for us, and moving tests to data files, interpreted by a relatively smaller test runner, makes them easier to protect from unexpected manipulation by AI. The same AI makes it easy to write test runners that run our test DSLs.

Where does this leave TDD? Given how much humans benefit from TDD when coding, can we expect AI to do a better job when we ask it, or force it, to use TDD in its inner coding loop? Getting AI to follow [canon TDD](https://newsletter.kentbeck.com/p/canon-tdd) is not easy, because AI usually generates lots of code before writing tests. On the other hand, it seems that coding agents perform better when we just tell them what we want and how to validate the result: adding instructions on how to achieve the result [seems to make them perform worse](https://danluu.com/agentic-testing/).

The techniques discussed here provide the “how to validate the result” that coding agents need. In a way, we could see this as a way to do test-first, with larger steps. Generally speaking, taking smaller steps makes development safer; however, the goal of this article is to discuss ways to enable taking larger steps safely.

## Core techniques

### Customer tests

Developers writing tests was one of the original practices of Extreme Programming. The resulting tests are sometimes called “developer tests” because they support developers in ensuring that their code does what they think it should. (Most developers call them *unit tests*.) Another one of the original XP practices was *Customer tests*, also known as *Acceptance tests*.

A customer test is a test written in the language of the customer; it describes what the software should do, from the point of view of the users.

The idea is that a customer can understand what a customer test is trying to prove, so that these tests help bring alignment between customer and developers. The interesting part is that when a machine is writing most of the code, the human developers start playing more and more the part of the “customers” of the code. We are interested in specifying what the software should do, in terms that are as free of implementation details as we can, because we are delegating those implementation details to the AI.

![The XP practices](https://martinfowler.com/articles/tests-as-data/circles.jpg)

The circles of Extreme Programming practices. Image by [Ron Jeffries](https://ronjeffries.com/articles/017-02ff/new-framework-0/), used with permission.

That's why I think that **customer tests are the primary kind of tests that we should rely on; especially when production code is AI-generated.** Unit tests (developer tests) can still be useful, but we don’t treat them as the primary source of truth where specifying behaviour is concerned.

Customer tests:

- Define the functional content of a user story, so that when they pass, we know that the story is functionally complete
- Protect us against regressions: once a customer test passes, it must always pass, until the business rule that it’s testing changes
- Support refactoring, because they test behaviour, not implementation
- Last but probably most important: they are a tool for ensuring alignment between developers and customers. The customers can read and understand the test, and can provide feedback to ensure that we are testing the right thing.

On the other hand, customer tests might prove difficult to work with. Especially when they are implemented as end-to-end tests, they can be slow and flaky. There are ways to overcome these problems, and we will discuss them in the coming sections.

### Focus on what matters

Let's see how we can fix the problem of hard-to-read tests. Consider the following simple example:

```
@Test  
void defaultGreeting() throws IOException, InterruptedException {  
    HttpRequest request = HttpRequest.newBuilder()  
            .uri(URI.create(baseUrl + "/hello"))  
            .GET()  
            .build();

    HttpResponse<String> response =  
            client.send(request, HttpResponse.BodyHandlers.ofString());

    assertEquals(200, response.statusCode());  
    assertEquals("Hello, world!", response.body());  
}

@Test  
void personalisedGreeting() throws IOException, InterruptedException {  
    HttpRequest request = HttpRequest.newBuilder()  
            .uri(URI.create(baseUrl + "/hello?name=Joe"))  
            .GET()  
            .build();

    HttpResponse<String> response =  
            client.send(request, HttpResponse.BodyHandlers.ofString());

    assertEquals(200, response.statusCode());  
    assertEquals("Hello, Joe!", response.body());  
}
```

Can you immediately spot what the tests are trying to prove? A trained eye will focus on the important bits, that are highlighted below:

```
@Test  
void defaultGreeting() throws IOException, InterruptedException {  
    HttpRequest request = HttpRequest.newBuilder()  
            .uri(URI.create(baseUrl + "/hello"))  
            .GET()  
            .build();

    HttpResponse<String> response =  
            client.send(request, HttpResponse.BodyHandlers.ofString());

    assertEquals(200, response.statusCode());  
    assertEquals("Hello, world!", response.body());  
}

@Test  
void personalisedGreeting() throws IOException, InterruptedException {  
    HttpRequest request = HttpRequest.newBuilder()  
            .uri(URI.create(baseUrl + "/hello?name=Joe"))  
            .GET()  
            .build();

    HttpResponse<String> response =  
            client.send(request, HttpResponse.BodyHandlers.ofString());

    assertEquals(200, response.statusCode());  
    assertEquals("Hello, Joe!", response.body());  
}
```

By focusing on the *test name*, the *input* and the *expected output*, it becomes clear that the tests are trying to prove: the standard greeting is “Hello, world!”, and the greeting when a name is provided is “Hello, \<name>!”. Every other bit of code is boilerplate that makes seeing the test intent harder.

So why don't we focus on just those three things? What if we just wrote something like

```
suite: greetings
tests:
  - name: "default greeting"
    request:
      resource: "/hello"
      name: null
    greeting: "Hello, world!"
  - name: "personalised greeting"
    request:
      resource: "/hello"
      name: "Joe"
    greeting: "Hello, Joe!"
```

This yaml file can be used as a source of test cases by a parameterized test that is very easy for the AI to write. A normal process would be:

1. Draft a sample of the test *domain-specific language* ([DSL](https://martinfowler.com/bliki/DomainSpecificLanguage.html)) that you’d like to have
2. Get the AI to write a test runner for it
3. Both human and AI integrate any other useful test cases; for instance

```
- name: "name must not be entirely whitespace"  
  request:  
    resource: "/hello"  
    name: " "  
  error: "Name must not be entirely whitespace"
```

We can make this even more concise by extracting a table that can be stored in a CSV file

```
test_name,             resource, name, greeting,         error
default greeting,      /hello,   null, "Hello, world!",
personalised greeting, /hello,   Joe,  "Hello, Joe!",
not whitespace,        /hello,   " ",  null,             Name must not be entirely whitespace
```

Why tables? Because a **table makes it easy to see at a glance what cases are covered, and what cases may be missing. In addition, a table makes it easier to add new test cases**.

This, in essence, is tests-as-data. How does this help with the problem of developers not trusting tests? It helps because the tests are (1) focused on the part that is of most interest to us as developer-customers, (2) more resistant to refactoring, and (3) when the tests are written in text files, it's easier to spot when AI is changing them.

Tables work well when the input to the application, the setup state and the expected output are single, scalar values. When they are structured data, we need to use structured scenarios, covered below.

### Not all tests can fit in tables

When the inputs of the test are not a simple value, the tabular format does not suffice. We can then turn to more structured scenarios. My first exposure to the idea of customer tests came from Ron Jeffries' book *Extreme Programming Adventures in C#*, published in 2004. He describes the development of an editor for articles with XML markup, and starts with this customer test (rewritten for clarity):

`---- before ----
<p>Fo|o</p>
<p>Bar</p>
----------------
* press enter
---- after ----
<p>Foo</p>
<p>|</p>
<p>Bar</p>
----------------`

The intent of the test is to show what should happen when the user presses the *enter* key when the cursor, represented by the “|” character, is in the middle of a paragraph. Jeffries continues showing how you could write a simple runner for this test. From the beginning, this customer test lives in a text file.

Another example: when developing programming language interpreters or compilers, I always thought that the waterfall approach of designing the grammar first and then each processing stage is less effective than starting with the simplest meaningful program you can define, which for most languages is a “Hello, world!”, and then building language features one test at a time. Working this way with coding agents makes it possible to write very complicated interpreters and compilers safely. Here’s an example from a BASIC interpreter experiment, and this approach scales to much more complicated languages.

```
tests:  
  - name: "LEN function basic tests"  
    program: |  
      10 PRINT LEN("HELLO WORLD")  
      20 PRINT LEN("")  
      30 PRINT LEN("A")  
      40 END  
    expected:  
      - "11\n"  
      - "0\n"  
      - "1\n"  
  - name: "LEFT$ function basic tests"  
    program: |  
      10 PRINT LEFT$("HELLO WORLD", 5)  
      20 PRINT LEFT$("HI", 10)  
      30 PRINT LEFT$("TEST", 0)  
      40 PRINT LEFT$("HELLO", 3)  
      50 END  
    expected:  
      - "HELLO\n"  
      - "HI\n"  
      - "\n"  
      - "HEL\n"
```

How would we test a web application? Here’s an idea for testing a simple todo-list app:

```
## Unchecking a completed todo reactivates it
GIVEN model:
    [x] buy milk  
    [ ] walk the dog
WHEN user:
    click toggle of "buy milk"
THEN page:
    > ""
    [ ] buy milk  
    [ ] walk the dog  
    -- **2** items left
THEN model:
    [ ] buy milk  
    [ ] walk the dog
```

The display of the application, that renders like this:

![Screenshot from the todo-list app](https://martinfowler.com/articles/tests-as-data/todo.png)

is simplified from the original HTML to this:

```
> ""
[ ] buy milk  
[ ] walk the dog  
-- **2** items left
```

This textual representation is easy to insert in tests, it produces a clear “diff” when a test fails, and is robust against changes in the visual appearance of the UI. The function that transforms the HTML, or the DOM, to this textual representation is called a “projection”, or more simply, a *printer*.

In this example we use another printer, that shows the contents of the internal state of the application. It's in the `THEN model` section and it uses a syntax similar to the one produced by the UI printer. It was added because when testing an operation that is supposed to change the state, like in this case unchecking a todo item, we should not be satisfied with just seeing the change in the UI: we'd like to ensure that the persistent state of the application was changed as well.

Alongside the two printers, in this example we also employ two *parsers*: the first one is used in the `GIVEN model` section, where the starting state for the test is represented, and is used to set up the application state the way the test needs it to be. The second parser is used in the `WHEN user` section, to parse the command that we send to the application. Its job is to recognize a vocabulary of operations that can be applied.

![A sequence diagram of how the todo-list test works](https://martinfowler.com/articles/tests-as-data/http-seq.svg)

Sequence diagram showing how the todo-list test works.

The printer and parser for the application state must have some way to access and manipulate the application state itself; this can be achieved through a *side channel*.

All of the above mechanisms are used together in a test runner that a coding agent can easily generate.

## Building on existing traditions

### Fit and *specification by example*

Ward Cunningham is credited as originator of many of the ideas that later were popularized in Extreme Programming. Cunningham tells about how around the year 2002, his customers tended to provide their own specifications in Excel documents. Tired of copying the test data from Excel to test code, he wrote a program that read the test data from Excel and executed the tests automatically. This made it very easy for his customers to extend and refine the specification, and for him to verify that his programs matched the specification itself. He [called this tool Fit](http://fit.c2.com/) and it has implementations for various programming languages.

![Fit screenshot](https://martinfowler.com/articles/tests-as-data/fit.png)

A screenshot of Fit showing the results of a test. Image by [James Shore](https://www.jamesshore.com/), used with permission.

In 2004, Martin Fowler published his article on [Specification by Example](https://martinfowler.com/bliki/SpecificationByExample.html), that pretty much sums up the idea: complicated requirements can be clarified by getting developers and business people to agree on *examples of what should happen*. Later, in 2011, Gojko Adzic published [a book with the same title](https://gojko.net/books/specification-by-example/), and by then a number of similar approaches were emerging, the most popular being [Cucumber](https://cucumber.io/) and its language [Gherkin](https://cucumber.io/docs/gherkin/).

The *Specification by Example* tradition popularized customer tests; but the approach was not without costs. Automating the extraction of tables from document can be slow and fragile, and Cucumber requires a layer of interpretation code that sometimes can be a source of bugs and misunderstandings.

James Shore was an early adopter of Fit, and maintained many of its implementations. He later [became disillusioned with the approach](https://www.jamesshore.com/v2/blog/2010/the-problems-with-acceptance-testing), lamenting that, while the primary purpose of the tool is to improve communication with customers, most customers did not want to write the test tables, which led him to have the conversations with the customers and then write the Fit tests himself. Later, usage of Fit (and Gherkin) was commonly delegated to QA teams to test software after-the-fact, which defeats the original purpose of facilitating a conversation with the customers. For these reasons, he reckoned that if the customers won't write the tests, then the maintenance overhead of the Fit or Gherkin runners is not worth the value, and writing tests in code makes more sense.

The fact that customers don't want to write customer tests is often quoted against the practice of customer tests. I think that the situation we're in today changes this fact, because:

1. When the AI writes most production code, then human developers are in the position of customers: we describe what we want, and provide feedback on what we get, just like customers have always done.
2. Writing and maintaining the machinery that runs the customer tests is something that coding agents can do quickly and cheaply.

Specification by example can help with the problems with spec-driven development described in the introduction, by clarifying the acceptance criteria with examples, so that their description can be much shorter. By making these tests an integral part of the specification, we make the specification executable and impossible to bypass by the coding agents. And these specs cannot go stale, because they run in CI.

### Testing practices in Go

A related approach comes from the tradition of the Go programming language, where most tests are tabular. For example, here is how the “fmt” package is tested:

```
var fmtTests = []struct {  
    fmt string  
    val any  
    out string  
}{  
    {"%d", 12345, "12345"},  
    {"%v", 12345, "12345"},  
    {"%t", true,  "true"},

    // basic string  
    {"%s",    "abc",              "abc"},  
    {"%q",    "abc",              `"abc"`},  
    {"%x",    "abc",              "616263"},  
    {"%x",    "\xff\xf0\x0f\xff", "fff00fff"},  
    {"%X",    "\xff\xf0\x0f\xff", "FFF00FFF"},  
    {"%x",    "",                 ""},  
    {"% x",   "",                 ""},  
    {"%#x",   "",                 ""},  
    {"%# x",  "",                 ""},  
    {"%x",    "xyz",              "78797a"},  
    {"%X",    "xyz",              "78797A"},  
    {"% x",   "xyz",              "78 79 7a"},  
    {"% X",   "xyz",              "78 79 7A"},  
    {"%#x",   "xyz",              "0x78797a"},      
    {"%#X",   "xyz",              "0X78797A"},  
    {"%# x",  "xyz",              "0x78 0x79 0x7a"},  
    {"%# X",  "xyz",              "0X78 0X79 0X7A"},  
    //[...]
```

The above creates an array of structs composed of: the format being tested, eg “%x”, the input string, eg “abc”, and the resulting formatted text, in this case “616263”. A short test runner executes all the rows in this table against the fmt function. Many functions in the Go standard library are tested in a similar way.

Again in Go, there is an established practice of writing scenarios in text files, particularly when the test depends on the existence of files, or checks that files are created. Here is [an example](https://go.dev/src/cmd/go/testdata/script/run_hello.txt) of how the `go` command line tool is tested:

```
env GO111MODULE=off

# hello world
go run hello.go
stderr 'hello world'

-- hello.go --
package main
func main() { println("hello world") }
```

We see that [a custom DSL](https://go.dev/src/cmd/go/testdata/script/README) was developed for the purpose of testing the central Go utility program. The test exercises the `go run` subcommand. The DSL allows the test writer to define an environment variable, setup a file, execute a command and check the resulting output on the standard error channel.

### Approval testing

A third relevant tradition is that of [Approval Tests](https://approvaltests.com/), popularized by Llewellyn Falco, where it is common to get a system to generate a file that may represent a sequence of computations, or what the UI looks like at a particular moment, or a readable representation of the internal state of the system; the main point is that the representation we get is *understandable by humans,* who can then *approve* it. Once a file is approved, it can be regenerated in future runs and whenever the newly generated file differs from the approved one, it signals that the behaviour of the system has changed. The mechanism for running these tests involves a test runner, that can come from an approval testing library, or can be generated ad hoc by a coding agent. This approach is relevant for our interests here, because it stresses that what humans look at is not test code, but test data.

One key idea to take from approval testing is to find ways to make application state and output readable and “diffable”, meaning that when a test fails, we can see a clean textual diff between what was expected and what we got.

Llewellyn Falco and Lada Kesseler give [a great explanation of Approval Testing in practice](https://www.youtube.com/watch?v=QEdpE0chA-s) (video)

### Approved Scenarios and Approved Logs

Ivett Ördög has been promoting the techniques I'm describing in this article for some time. She published the [Approved Scenarios](https://lexler.github.io/augmented-coding-patterns/patterns/approved-scenarios/) and [Approved Logs](https://lexler.github.io/augmented-coding-patterns/patterns/approved-logs/) patterns, where she advocates using a domain-specific language to express customer tests. The first pattern is for when we specify behaviour upfront; the second relies on the running system to generate a textual representation of its state, that can be tweaked and used to generate new tests, especially when testing existing systems, or writing a failing test as the first step towards bug fixing. Her [video](https://www.youtube.com/watch?v=GyI5qU9MNJU&t=2250s) explains the techniques in more detail.

A typical Approved Scenario or Approved Log DSL is composed of three parts: the setup of the state of the system, the action or actions to be executed, and a representation of the state of the system after the actions. The process by which one would get started is to:

1. Decide on a DSL format, likely in collaboration with the AI, and write a first test
2. Have a coding agent generate the DSL parser and test runner
3. The developer checks that the runner works correctly, by checking it against production code
4. The coding agent adds scenarios, using the first test as a template

My interpretation is that the Approved Scenarios pattern is applied to a system that has already enough structure to make a meaningful first test possible, in a way similar to the [Spike and Stabilize](https://dannorth.net/blog/the-art-of-misdirection/) pattern described by Dan Terhorst-North. An alternative way, when starting from scratch, could be that the first test is used to generate [a Walking Skeleton](https://www.thoughtworks.com/insights/blog/first-story-every-project).

The two patterns are instances of what Ördög calls the [Constrained Tests](https://lexler.github.io/augmented-coding-patterns/patterns/constrained-tests/) pattern, where a coding agent cannot easily “cheat” by writing tests that are weaker than expected. Approved Scenarios and Logs make cheating less likely, by making the assertion part of the test mandatory in the DSL syntax, and this requires that the DSL is implemented with a parser, i.e. an [external DSL](https://martinfowler.com/dsl.html).

Ördög argues that if we implement an [internal testing DSL](https://martinfowler.com/bliki/InternalDslStyle.html), which is simply a collection of functions defined in the programming language we are using, then an agent could write tests with no assertions, or weak assertions, thus faking a high test coverage score while the tests are not testing the system as expected. An external DSL, where the assertions are an integral and mandatory part of the DSL syntax, makes this problem less likely.

So, one purpose of the Constrained Tests pattern is to ensure that test coverage is a reliable indication of what parts of the code are tested, and this makes the overhead and indirection of an external DSL worthwhile.

## Implementation hints

### Not always end-to-end

It is common to implement customer tests as end-to-end tests passing through the UI. This can be OK, but it is not the only way. One problem with all customer tests being end-to-end is that the tests become slower, and more fragile. A good way to avoid this is to execute most tests bypassing the UI. Many business applications have a [service layer](https://martinfowler.com/eaaCatalog/serviceLayer.html), that is a set of classes, or functions, that are invoked by the UI and by other entry points in the system, to execute business operations. The tests can invoke the service layer, bypassing the UI entirely. This makes sense in many domains where logic is much more complicated and difficult to get right than the UI, for example in the financial domain. An example from my experience was when, in the insurance domain, we were given a complicated set of rules for matching a claim with a lawyer. Writing the customer tests helped us make sure we understood exactly what the customer wanted to achieve, and we implemented it as a service-level test.

Another problem is when the entry point of your test is far, in terms of number of function calls, from the logic that you want to exercise.

![a diagram where a test talks to component A that talks to component B, that talks to component C](https://martinfowler.com/articles/tests-as-data/deep-test.png)

In this example, we want to test some interesting logic in a component “C”, but in order to do so, our test must “persuade” the intervening component “A” to set up appropriately component “B” in a way that it sets up appropriately component “C”. This can be awkward, and may lead to a feeling of trying to build a ship in a bottle, trying to set up tiny details using sticks through the opening of the bottle.

We don’t have to do this; when we have an important component that we want to specify in detail, we can write customer tests for it directly. Remember that the most important thing about a customer test is that it supports alignment between customers and developers. An example is this test for a component that checks if the user’s IP address matches an allowlist. The allowlist was sufficiently complicated that we didn’t feel confident testing it with end-to-end tests alone, so we wrote a focused customer test, implemented as a unit test, complete with its own custom test DSL.

```
tests:  
  - name: "is_ip_allowed respects enabled flag"  
    allowlist:  
      - { id: 1, ip_or_cidr: "10.0.0.1/32", is_enabled: true }  
      - { id: 2, ip_or_cidr: "10.0.0.2/32", is_enabled: false }  
    is_ip_allowed:  
      - { ip: "10.0.0.1", expected: true }  
      - { ip: "10.0.0.2", expected: false }

  - name: "is_ip_allowed fails closed on empty allowlist"  
    allowlist: []  
    is_ip_allowed:  
      - { ip: "10.0.0.1", expected: false }

  # --- IPv4 MATCHING ---  
  - name: "is_ip_allowed matches /32 range"  
    allowlist:  
      - { id: 1, ip_or_cidr: "192.168.1.100/32", is_enabled: true }  
    is_ip_allowed:  
      - { ip: "192.168.1.100", expected: true }  
      - { ip: "192.168.1.101", expected: false }  
      - { ip: "192.168.1.99",  expected: false }
```

Using a test DSL for every important thing that we want to test is cheap now, because the AI can build test runners very cheaply.

### Customer tests for the UI

Testing UIs is notoriously challenging, but at the same time, UIs often contain important logic that we want to test. The challenges come from the slowness and flakiness of most tests that use tools like Selenium. Tools like Playwright can help reduce test flakiness by automating the wait for components to be rendered. [Browser-based component testing](https://www.thoughtworks.com/radar/techniques/browser-based-component-testing) was recently promoted to *Trial* in the Thoughtworks Technology Radar.

There are also approaches that do not require running tests through a browser; JSDOM can be used by test runners to mount UI components into a simulated DOM and allow tests to interact with the UI (JSDOM) in the same way as a human user would. Popular frontend frameworks use JSDOM to run component tests in-process.

Additional hints on how to get [sub-millisecond end-to-end tests](https://www.youtube.com/watch?v=sUclXYMDI94) come from Aslak Hellesøy.

### Dealing with external systems

Business applications frequently depend on external services; an ecommerce application will likely need to talk to payment gateways, mail servers, inventory services, and many others. This makes testing more complicated. If we tried to test the application end-to-end, connected to external services, we would run into many problems. The external services might not be always available, they might be slow and unreliable, and they might not have a test version that we can use. And even if a test environment of an external service exists, it might not be convenient to use for other reasons.

I once worked with a test payment gateway, using a test credit card, and we had to periodically call the people behind this service to replenish the credit limit of the test card by a few million Euros, because even if our tests tried to “spend” as little as possible, they were running all the time and they would regularly bring the available credit limit to zero. Another example is a test version of a stock exchange API that respects the real market opening and closing phases, so that we can’t run the tests off hours. Or they might have rate limiters.

It’s clear that one of the success criteria with agentic coding is fast and reliable feedback, so it is important to be able to run our tests, even end-to-end tests, locally with no connection to external services.

We can run local databases and local email servers, but what about custom third party services? The traditional solution is to setup a tool like *Wiremock* or *Mountebank*, with specific instructions to mock the desired behaviour. This works, but it is a lot of work to specify how the external system should behave in the test. An emerging approach to this solution is to get coding agents to generate local simulators of the external services. If the external service has a published API, we can just ask a coding agent to implement it. This approach was popularized by Justin McCarthy, who calls them [Digital Twins](https://factory.strongdm.ai/techniques/dtu) (also see [this interview video with McCarthy](https://www.youtube.com/watch?v=Ge9HvXha6XM)). This approach was also mentioned in [this video presentation by Clare Liguori](https://www.youtube.com/watch?v=pqlWNihgdjI), who calls them *mock services*, in the context of how teams manage to get a step improvement in productivity with coding agents.

In this context, it's often useful to run the same test as an [in-process test for rapid feedback](https://martinfowler.com/articles/microservice-testing/#testing-component-in-process-diagram), and as an [out-of process integration test](https://martinfowler.com/articles/microservice-testing/#testing-component-out-of-process-diagram) with more realistic dependencies. Tests-as-data makes this easy to do by having AI generate different test runners for the same test. A similar technique was [explained by Uberto Barbini](https://medium.com/javarevisited/beyond-traditional-acceptance-tests-79cbcee63eda) using tests-as-code, which makes it a bit less straightforward to adopt.

The topic of how to do black box tests with external dependencies is rich and deep and we can but skim it in this article. More details can be found in [Martin Fowler's](https://martinfowler.com/bliki/TestPyramid.html) or [Ham Vocke's](https://martinfowler.com/articles/practical-test-pyramid.html) articles.

### Protecting the test harness

One problem with coding agents is that they may sometimes “cheat”; in the rush to claim task completion, they might take shortcuts, such as weakening or disabling tests. This is one of the reasons why developers, as we mentioned in the introduction, might lose confidence in the protection afforded by the tests, as they are changed due to refactoring or due to new features being implemented, and they are afraid that the tests might be unexpectedly weakened in the process.

The approach described in this article helps with this problem, because when the important part of the tests is in text files, the developers can protect those files from modification, when no changes to the spec are expected, or at least, they can check that those files are not unexpectedly modified in source control. So it's harder for the developers to miss a modification in the tests, and if such a modification happens, they can inspect the tests-as-data to see what's changed, with hopefully less fatigue than would happen if the tests were tests-as-code.

| When                           | Expect production code changes? | Expect test data changes? | Expect test runner changes? |
| ------------------------------ | ------------------------------- | ------------------------- | --------------------------- |
| Implementing new functionality | Yes                             | Yes                       | No                          |
| Refactoring production code    | Yes                             | No                        | No                          |
| Improving test coverage        | No                              | Yes                       | No                          |
| Improving test DSL             | No                              | Yes                       | Yes                         |

Depending on the type of task, it's possible to protect the files that we're not expecting the AI to change, by making them read-only in a way that the agent cannot bypass, or by running a check in CI that verifies what files were changed depending on the type of task. The hooks facility of coding harnesses can provide the model fast feedback when it attempts to change something that it shouldn't, but is not bullet-proof.

Even then, there is still some room for AI to play surprises on us. The test runner is code, and could be unexpectedly modified. The DSL implementation also provides room for AI-induced confusion. To further improve our confidence that the tests are still protecting us, we might look into additional techniques, such as those mentioned in the next section.

### Validating the tests

When the AI writes some, or most of the tests, we still have a review problem. The techniques described above make test review cheaper, by increasing the expressiveness and conciseness of the tests. However, the volume of tests is still going to be significant, so we incur cognitive fatigue in test review, and run the risk of cognitive surrender.

Let's see two strategies for dealing with this. One is finding ways to validate the tests; the other is finding additional ways to validate the product itself.

The first thing we can do is **mutation testing**: we inject intentional faults in production code, and see if any test breaks, and if it breaks, check that the error message is correct and informative. The simplest way to do mutation testing is to manually change something in the production code, e.g., turn a `+` into a `-`, or comment out one branch of an `if`, and see if any test breaks. Then we might ask AI to do some mutation testing itself. Finally, we might adopt a mutation testing library, to automate this process and make it part of a CI pipeline.

Mutation testing proves that the tests are effective in detecting faults, but it does not protect us against the risk that the tests themselves are specifying something that is not correct. In order to mitigate this risk, we should look for ways to validate the tests themselves, i.e., to validate that the expected output of the tests is correct. An *oracle* is someone or something that is able to tell what the correct output should be for any given input. Sometimes the only available oracle is a person: the domain expert, but we can find often other oracles. One example is official documentation: official regulations, internal compliance documents, technical handbooks, API docs.

For instance, in the development of a COBOL interpreter, I provided the official IBM manuals, in pdf and text format, so that the agents had an authoritative source for the dialect of COBOL that runs on IBM i machines.

Another type of oracle is an alternative implementation: sometimes you can use a simple, brute force algorithm to compare the outputs of a complex, more efficient algorithm. I have used the oracle technique in the development of the COBOL interpreter mentioned above, where all the customer tests were required to pass in the same way on a real IBM i system, as they were passing on my laptop. I used it to validate that the customer tests, that were mostly generated by AI, were specifying the same behaviour as the IBM machine.

This is an additional advantage of tests-as-data: you may run the same test in more than one environment, by getting AI to write another test runner. This is handy e.g., when rewriting a system, as we can write characterization tests that can be run against both the old and new environment.

### Beyond example-based tests

The tests we have been discussing so far are all example-based, meaning that they exercise the system by checking a specific example of behaviour. Property-based testing is a completely different and complementary approach. *Property-based* tests (PBT) exercise the system through a large, randomized variety of inputs, checking that no sequence of inputs pushes the system into an unwanted state. At the very least we can check that no input produces a crash; more sophisticated approaches to PBT involve checking system invariants, that is, properties that must always hold, like for instance *customer balance must never be negative*, or compare the results of the system with an oracle.

Depending on the nature of your application, you may find creative ways to further test and increase confidence in its correctness. If it's a game, have the AI play hundreds of games against itself. If it's a programming language compiler, have the AI implement a complex program in it and see how well it runs. If it's a web application, have the AI simulate users and try workflows by running the browser through a tool like [agent-browser](https://agent-browser.dev/). Ask your coding agent; it might come up with some more ways.

## Limits of applicability

The techniques described in this article are appropriate for testing production systems. Not all development efforts require, or benefit from, this sort of treatment: when starting something new, it can be good to generate a prototype and explore the space of possibilities, and observe what the coding agent produces and then ask for changes. Prototypes, proofs-of-concepts, experiments can all be generated quickly without too much concern for tests, because in an early stage, learning quickly can be more important than being able to safely evolve the system. Once we have a better understanding of what we want, we can either throw away the prototype and rebuild it from scratch with tests, or we can [stabilize](https://dannorth.net/blog/the-art-of-misdirection/) the prototype and add test coverage after the fact.

The extra machinery and indirection of an external DSL requires extra work to ensure that the DSL expresses well the things that we want to say, and that the test runner and the printers and parser are correct. This work makes sense if we are going to review those tests, at least a sample of them, to ensure that they express what we want. But sometimes it might make more sense to just let the model write tests in whichever way it wants, because it is going to be faster and cheaper. This might make sense especially when we have other ways to validate that the system does what we want.

The large steps that these techniques enable make it harder for humans to keep a good understanding of how the system works. An understanding of the internal architecture of the product is still important, and other techniques are needed for us to gain and maintain this understanding.

The approaches discussed in this article are often not sufficient to achieve assurance that the system is correct. Other automated testing techniques can be used, as discussed above. Most importantly, human exploratory testing is always recommended.

## Conclusion

We started from the problem that many developers who give high autonomy to AI do not think that the test coverage they get is trustworthy, chiefly because AI-generated tests are hard to read and tend to change whenever the production code is changed, even for refactorings.

We proposed a solution that consists of:

- Customer tests that speak the language of the customer (which nowadays includes the developers)
- Black box tests at the system or component level
- Tabular parameterized tests where applicable,
- Ad hoc test DSLs where tables won't work
- Moving the test data (tables or scenarios) to text files that are easy to tell apart from test code
- An AI-generated test runner that runs test data files

This helps the problem of test trustworthiness, because the tests become more expressive, devoid of irrelevant details, robust with respect to refactoring and easier to protect from unexpected changes by the coding agents. It also helps to write specs that rely on more than Markdown prose to ensure the behaviour we want is implemented correctly.

These techniques help establish clarity on the functional specification of the system; they are a part of an engineering harness, and they should be paired with other complementary techniques, including: mutation testing, property-based tests, lints and scanners and manual exploratory testing by humans.

This is relevant now, because it seems that going forward, most code will be written by machines, and that includes test code. We need to shape these tests in a way that helps people stay in control.

We can get started applying these ideas by casting our tests in the language of the customer and following the process [outlined above for Constrained Tests](https://martinfowler.com/articles/tests-as-data.html#ivetts-process).
