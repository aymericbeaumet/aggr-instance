---
title: 'Selective Test Execution at Stripe: Fast CI for a 50M-line Ruby monorepo'
link: https://stripe.dev/blog/selective-test-execution-at-stripe-fast-ci-for-a-50m-line-ruby-monorepo
source: stripe-com
published: 2026-04-09T00:00:00Z
updated: 2026-04-09T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Aditya Anchuri
summary: Stripe's Selective Test Execution system employs some clever tricks to allow us to continue scaling our team and our codebase while only running around 5% of our tests on average. Find out how it works!
content: extracted
html: 2026-04-09-selective-test-execution-at-stripe-fast-ci-for-a-50m-line.html
remote_preview:
  url: https://stripe.dev/og/blog/selective-test-execution-at-stripe-fast-ci-for-a-50m-line-ruby-monorepo.png
  alt: 'Stripe Developer Blog: Selective Test Execution at Stripe: Fast CI for a 50M-line Ruby monorepo'
---

Stripe has an extremely large Ruby codebase: our monorepo is the order of 50 million lines of Ruby code, backed by a correspondingly huge test suite—roughly 100,000 Ruby test files and approximately 1.2 million test units. If we ran all of these tests sequentially end to end, a single build would take four months. Which is, of course, implausible.

To ensure builds finish within a reasonable time (a few minutes, not four months), we run them over a massively parallel set of workers. At our scale, we run around 50,000 builds a week, a number that is only increasing with AI adoption among our engineering teams accelerating commit rates and productivity.

**Selective Test Execution** (internally known as “STE”) is how we keep CI fast and affordable without giving up confidence. Simply put, we don’t run every test on every build. On average, we only run roughly 5% of our full Ruby test suite (with a median value of 0.5%) for a given build, spending <10% in compute of what an “always run everything” strategy would require. We are able to do this without sacrificing safety.

This means that for a typical change, we are able to select and execute a few hundred tests to safely validate it, as opposed to our entire test suite of 100,000 tests.

![](https://stripe.dev/images/selective-test-execution-at-stripe-fast-ci-for-a-50m-line-ruby-monorepo/image3.png)

> **Fig. 1:** Percentage of total test suite executed on average. The average test selection percentage is around 5%, although at least half of our builds execute <0.5% of tests.

### Building a dependency graph by intercepting file opens

A common approach to test selection is static dependency analysis: figure out which code a test depends on by parsing source, building dependency graphs, following package imports, etc. That works well in ecosystems with explicit module boundaries and limited runtime dynamism (and this works for many of the codebases that we have at Stripe today).

Ruby is different: even with all of our internal restrictions and conventions, Ruby still retains escape hatches that are extremely hard to model perfectly with static analysis. For example:

- Metaprogramming and dynamic dispatch can change the shape of runtime behavior.
- Configuration and environment often determine which branches execute.
- Tests frequently depend on non-Ruby inputs: YAML and JSON config, templates, fixtures, generated artifacts, and more.

The “perfect” version of static selection—predicting exactly what executes at runtime and therefore exactly what the test depends on—is not something you can compute in the general case. Approximations tend to fail in one of two directions:

- **Underselection:** Miss a dependency and skip a test that should have run (dangerous).
- **Overselection:** Conservatively include too much and end up close to “run everything” (slow).

Thus, Stripe’s STE leans on dynamic observation: instead of guessing what a test might touch, we record what it did touch.

At a high level, Stripe’s STE works by instrumenting tests and recording which files were accessed during their execution. Then, for a new change, we rerun tests whose previously accessed files intersect with the changed files.

This “file-level dependency graph” has two properties we love:

- It naturally captures non-Ruby dependencies (config files, fixtures, templates, etc.).
- It can be implemented with very low overhead by focusing on file access, not code execution tracing.

The foundation of Stripe’s STE is a dynamically linked C++ shared library, built in-house and loaded using `LD_PRELOAD`, which we call `file_access_interceptor`.

In broad strokes, it does three things:

1. Provides an external API to tell the interceptor what’s currently running. Our test runner is able to set a “scope,” which essentially maps to the current test that is executing. Any files opened by the test are associated with the test’s “scope.”
2. Intercepts file opens at the operating system level. By intercepting `open` syscalls (and variants), the interceptor can record a mapping from a given opened file to the scope it is opened in.
3. Propagates `LD_PRELOAD` into child processes (analogous to a worm or virus). If a test forks or spawns subprocesses, we keep interception enabled, so file access in those processes is still attributed to the right test scope.

![](https://stripe.dev/images/selective-test-execution-at-stripe-fast-ci-for-a-50m-line-ruby-monorepo/image2.png)

> **Fig. 2:** File access interceptor runs as a low-level utility that monitors file I/O and records the list of files opened by the process.

#### Edge cases with instrumenting open syscalls

Observing the open syscall is a powerful approach in a dynamically interpreted language like Ruby. Every bit of code or configuration that a Ruby process depends on is necessarily accessed during the lifecycle of the process. Note that this is different from compiled languages (barring dynamic linking), where source file dependencies are statically built into compiled artifacts during the build process.

However, this can have edge cases. Some tests can access files without opening them. A good example is tests that use directory globbing or file discovery. For tests like this, the files will not necessarily be `open`-ed in the test. Adding a new file can change test behavior for these tests without any previously opened file signal. We have a small, limited set of tests at Stripe that have this behavior: typically these are codebase health tests that perform linting rather than verifying production behavior. To bypass the limitation with open, we call these tests mandatory and select them regardless of the results of the selectivity algorithm.

#### Making interception fast: Do almost nothing inside open

Interception only works if it’s cheap. Ruby tests at Stripe scale already consume massive CPU; adding heavyweight instrumentation would erase the benefit.

The design choice in the interceptor is: keep work inside the `open` interception path minimal.

While inside the intercepted `open`, we only append a record to a per-process file. These per-process logs are written under a temp directory. Everything more expensive—aggregation, indexing, inversion—is done outside the hot syscall path.

#### Scopes: Attributing file access to the right unit of selection

STE selection is fundamentally done at the test file level, so we model file access attribution with a simple scope stack.

- Every process starts in the root scope (we label this simply: `r`).
- When the test runner forks to execute tests for a particular test file, it pushes a new scope.
- Scopes are represented as strings separated by `//`. For example, a test file might correspond to a scope like: `//r/file-/cibot/test/tasks/cancel_build_test.rb`

Scopes are hierarchical: when deciding whether to run a test file, we consider files opened in that test file’s scope and any parent scope(s).

One important consequence: anything opened in root scope effectively becomes a global dependency. If a changed file was opened in root scope, it can force many (or all) tests to run. We view this behavior as a feature, not a bug; it’s the system being honest about shared global dependencies. In our CI test runner, we “preload” some key primitives that are effectively used by all tests: this ensures performant test execution. Any changes to these primitives are classified as in root scope, and trigger all tests.

![](https://stripe.dev/images/selective-test-execution-at-stripe-fast-ci-for-a-50m-line-ruby-monorepo/image4.png)

> **Fig. 3:** Root scope and test scopes. file1 and file2 are recorded as being accessed in root scope, whereas file3 and file4 are recorded in respective test scopes.

### Going from raw logs to a compact selection index

The STE pipeline turns “what files did tests open?” into something a build can use quickly. A simplified view of this process has the following steps:

1. Tests run under a wrapper script that enables tracing and interception.
2. Each process produces per-process “opened file → scope” logs.
3. Workers send test unit results to a build scheduler (a sidecar process responsible for scheduling all the tests over a massively parallel set of workers); during that, tooling reads the opened-file logs and sends indexed representations upstream.
4. The scheduler aggregates and inverts the information into a global map of opened file → scopes. We call this the “selection index.”
5. At the end of the build, we write out the compact selection index used for future selection.

In addition to the selection index, the scheduler also maintains an inventory of the current set of files in the repository source tree. This is used to detect what changed later. Note this isn’t just git-tracked code in the repository source tree, but also generated artifacts. We call this the “file inventory.” The file inventory is described in more detail in [the next section](https://stripe.dev/blog/selective-test-execution-at-stripe-fast-ci-for-a-50m-line-ruby-monorepo#detecting-file-changes-with-hashdeep).

The selection index is designed for fast lookup and low overhead:

- **Data format:** A bitmap, built with [roaring bitmaps](https://roaringbitmap.org/)
- **Key:** A global numeric index of the file in the inventory
- **Value:** A list of indexes of test files to run if that file changed

We use bitmaps as a compact representation that provides fast union or intersection operations—exactly what you want when you’re combining impacted tests across many changed files. In total, we store approximately three billion data points in this bitmap while being able to query it performantly.

![](https://stripe.dev/images/selective-test-execution-at-stripe-fast-ci-for-a-50m-line-ruby-monorepo/image1.png)

> **Fig. 4:** Distributed architecture of the build. A scheduler node distributes tests across multiple workers, which run with the file access interceptor and stream results back to the scheduler. The scheduler then aggregates all the results into a single indexed representation of opened files.

#### Selecting tests for a new build: Detect changes, then look up impacted tests

Now that we have a selection index and a file inventory, when future builds start, we can use them to select the right set of tests to execute. At build time, selection process looks like the following:

1. Determine which files changed (including generated files).
2. Translate “changed files” into file indexes.
3. For each changed file index, union the bitmap of impacted tests.
4. Apply safety and ergonomics rules (“special cases”).

#### Detecting file changes with hashdeep

We use hashdeep, a parallelized hash computation utility, to [detect file changes](https://github.com/jessek/hashdeep) between the current commit and the commit baseline associated with the STE data. Note that we can’t just simply use git, because we track both code that is checked into the source tree as well as code that is built by code-generation steps before we run tests.

In [the previous section](https://stripe.dev/blog/selective-test-execution-at-stripe-fast-ci-for-a-50m-line-ruby-monorepo#going-from-raw-logs-to-a-compact-selection-index), we mentioned that each build produces a file inventory. This inventory is essentially a list of files in the codebase, along with the hash of each file computed by hashdeep.

```
9d27e374ddab7b5af150ebaa203d180d,/path/to/file1.rb
bb5a16e1ba70ab96a2d84e1d43c2cb3d,/path/to/file2.rb
f87fdb23e667f0f055af660aadac4c60,/path/to/file3.rb
… other files
```

When we start a new build, we compute the file inventory first, and then compare the hashes from the file inventory built by a previous build. This allows us to figure out which files have changed relative to previous, and select tests to execute accordingly.

#### Special cases

Naively running only the tests correlated to changed files since the previous commit could lead to some unwanted behaviors, and so STE also includes explicit guardrails. Examples include:

- Always rerun previously failing tests (users expect failures to persist until fixed).
- As explained in a prior section, we always run tests that use directory globbing or file discovery because adding a new file can change behavior without any previously opened file signal.
- We include additional handling for linters ([RuboCop](https://github.com/rubocop/rubocop) or YAML lint–style tools), so they don’t force scanning the entire repo; instead, they receive changed-file data and lint selectively.

### Making it reliable at scale

The selection algorithm is only as good as our ability to fetch a recent, coherent baseline dataset quickly and reliably. Our current approach is to persist STE build completion state and metadata in a database (internally, we use MongoDB). We then order the build data using an internal primitive we call Monotonic Revision IDs (MRIs)—strictly increasing identifiers that are generated with the key property that ancestry implies ordering: if commit A is an ancestor of commit B, then `MRI(A) < MRI(B)`. This allows us to order build data by commit without needing to actually consult git history as part of the build.

![](https://stripe.dev/images/selective-test-execution-at-stripe-fast-ci-for-a-50m-line-ruby-monorepo/image5.png)

> **Fig. 5:** Leveraging a database to record build baseline metadata, which can be used to power subsequent builds. In this diagram, the build for the Head and M2 commits is powered by data from the prior M1 commit, whose build was completed in time before the subsequent builds began.

Then, at the start of a build, we do a single fast DB query keyed by MRI to find the latest completed STE dataset(s) required to make selection decisions. We call the build that corresponds to this dataset the “STE base.”

This matters for two reasons:

1. **Reliability and performance:** Selection does not depend on slow, failure-prone multistep discovery at build start.
2. **Reproducibility:** “What baseline did you select?” becomes a debuggable input. You can rerun with the same STE base and reproduce selection behavior deterministically.

### Wrapping up

Our Selective Test Execution system employs some clever tricks to allow us to continue scaling our team and our codebase while only running around 5% of our tests on average.

The strategy we used to handle the dynamism of Ruby is applicable to any similarly flexible language, and we hope the patterns we used to implement our solution are helpful to other organizations. To sum them up:

- Choose a dependency signal that’s robust to language dynamism (file access).
- Implement it at a boundary that’s efficient (syscall or file I/O), not at a boundary that’s noisy (per-line execution).
- Store and serve the data in a way that’s operationally scalable (single query, monotonic ordering, reproducible baselines).
- Layer in pragmatic guardrails for the edge cases real engineers hit every day.

If you’re interested in solving developer productivity problems like this, at scale, [we’re hiring](https://stripe.com/jobs).
