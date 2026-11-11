---
title: Announcing .NET 11
link: https://devblogs.microsoft.com/dotnet/announcing-dotnet-11/
source: hnrss-org
published: 2026-11-10T16:01:27Z
updated: 2026-11-10T16:01:27Z
first_seen: 2026-11-11T07:00:45.879812165Z
authors:
- runesoerensen
summary: 'https://learn.microsoft.com/en-us/dotnet/core/whats-new/dotn... Comments URL: https://news.ycombinator.com/item?id=50442225 Points: 144 # Comments: 206'
content: extracted
html: 2026-11-10-announcing-net-11.html
remote_preview:
  url: https://devblogs.microsoft.com/dotnet/wp-content/uploads/sites/10/2026/11/announcing-dotnet-11-scaled.webp
---

Today, we’re excited to announce the release of .NET 11! This year’s release makes building modern applications faster, more productive, and has improvements across C# 15, ASP.NET Core, .NET MAUI, and Windows development. With hundreds of runtime optimizations, more responsive SDK tooling, and less overhead in everyday commands, you can spend less time waiting and more time creating. Build richer web and cross-platform applications, take advantage of new C# language features, and explore AI-powered development with improved tools and agentic capabilities.

**.NET 11 support**

.NET 11 is a Standard Term Support (STS) release with **two years of support** under the [.NET support policy](https://dotnet.microsoft.com/platform/support/policy/dotnet-core). STS and Long Term Support (LTS) releases have the same quality and servicing policies; the difference is the length of support. .NET 10 remains a supported LTS option. Stay current with servicing updates whichever release you choose.

## Join us at .NET Conf, November 10-13

Explore .NET 11 with the product teams and community at [.NET Conf 2026](https://dot.net/conf). The conference includes product team sessions on November 10 and 11, followed by two days of community sessions.

Watch the [Day 1 livestream, including the keynote](https://www.youtube.com/watch?v=SIBdHRrk8os), or browse the [conference session playlist](https://www.youtube.com/playlist?list=PLJL9Ln_f6sGM).

[www.youtube.com](https://www.youtube.com/watch?v=SIBdHRrk8os)

## What’s new in .NET 11

Some of the changes we’re most excited for you to try:

- C# 15 union types and closed hierarchies help you express the possible states of your application and handle them with compiler-checked patterns.
- A Native AOT CLI host and a warm MSBuild server reduce overhead in everyday developer workflows.
- ASP.NET Core brings asynchronous validation, OpenAPI 3.2, and new capabilities for Blazor and SignalR applications. Experimental Blazor AI components integrate with the Agent User Interaction Protocol (AG-UI) for advanced agentic web experiences.
- Build AI-powered, agentic applications with Microsoft.Extensions.AI (MEAI) for model access, MCP for tool integration, and Microsoft Agent Framework (MAF) for agents and workflows.
- Put agentic development tools to work with .NET Skills for .NET-specific guidance, .NET MAUI DevFlow for inspecting and testing running apps, and the .NET Upgrade canvas for reviewing modernization progress.
- .NET MAUI adopts CoreCLR as its default runtime and improves XAML iteration and native controls.
- The libraries add built-in Zstandard compression, simpler process execution, richer JSON support, and new ways to work with in-memory data without copying it first.

The [what’s new in .NET 11 overview](https://learn.microsoft.com/dotnet/core/whats-new/dotnet-11/overview) links to the full details. Here are the changes behind those headlines and how to start using them.

## Languages

### C# 15

[C# 15](https://learn.microsoft.com/dotnet/csharp/whats-new/csharp-15) is the default language version for projects targeting .NET 11. [Union types](https://devblogs.microsoft.com/dotnet/csharp-15-union-types/) are one of its biggest additions: a value can be one of a declared set of case types, with conversions from those types and exhaustiveness checking in switch expressions.

Consider a product lookup that either finds a product or reports that it wasn’t found:

```csharp
ProductLookup result = new Product(42, "Mechanical keyboard");

string message = result switch
{
    Product product => $"Found {product.Name}",
    ProductNotFound missing => $"No product with ID {missing.Id}",
};

Console.WriteLine(message);

public sealed record Product(int Id, string Name);
public sealed record ProductNotFound(int Id);
public union ProductLookup(Product, ProductNotFound);
```

The result type tells a caller which outcomes it needs to handle. There’s no need to use `object` or an unrelated base class just to put those outcomes behind one return type.

C# 15 also adds **closed hierarchies**. A `closed` class limits direct derivation to its declaring assembly, giving the compiler the information it needs to check patterns over the hierarchy. Unions are useful when you want to combine existing types; closed hierarchies fit models where you own the inheritance relationship.

Smaller language features help with everyday code, too. Collection expression arguments let you pass constructor or factory arguments without giving up collection expression syntax:

```csharp
HashSet<string> names =
    [with(StringComparer.OrdinalIgnoreCase), "Alice", "ALICE", "Bob"];

Console.WriteLine(names.Count);
```

This prints `2`: the comparer controls equality, so `"Alice"` and `"ALICE"` are one entry.

Other additions include extension indexers and labeled `break` and `continue`. These features don’t require the preview language version. The separate memory safety work **remains in preview**; see the [C# 15 feature documentation](https://learn.microsoft.com/dotnet/csharp/whats-new/csharp-15) for its opt-in requirements.

### F# 11

F# 11 improves compiler memory use and reduces allocations in FSharp.Core collection operations. It also adds ways to bound concurrent asynchronous work, useful when a collection operation would otherwise start more work than a service or machine can handle comfortably.

The [F# release notes](https://github.com/dotnet/core/blob/main/release-notes/11.0/preview/rc2/fsharp.md) distinguish the default-language updates from preview capabilities, including Runtime Async support for custom computation-expression builders.

## Platform

### SDK

The SDK now enables a [Native AOT CLI host](https://learn.microsoft.com/dotnet/core/whats-new/dotnet-11/sdk#nativeaot-entry-point-for-the-dotnet-cli) by default. Commands such as help and version queries can run without starting the managed CLI, and tool launches can avoid that startup path too. Commands that need managed execution still use it.

The [MSBuild server](https://learn.microsoft.com/dotnet/core/whats-new/dotnet-11/sdk#msbuild-server-enabled-by-default) also stays warm between CLI invocations by default, reducing repeated startup work. SDK installers on Linux and macOS are smaller through assembly deduplication.

File-based apps can grow across source files with `#:include`, and solution filters can be created and edited from the CLI. `dotnet watch` adds Aspire app-host integration, crash recovery, and device selection for mobile projects.

For teams using Microsoft.Testing.Platform, `dotnet test` gains run-level timeouts, failure limits, clearer progress reporting, and per-module results directories. The test runner matters: these switches aren’t interchangeable with the VSTest command-line options.

Container publishing adds reproducible image digests through `SOURCE_DATE_EPOCH` and avoids redundant layer uploads when the target manifest already exists. NuGet source configuration gains a minimum package publication age setting, which lets teams wait before newly published package versions become eligible for selection. This is an additional policy control, not a replacement for vulnerability checks or reviewing dependencies.

See [what’s new in the SDK and tooling](https://learn.microsoft.com/dotnet/core/whats-new/dotnet-11/sdk) for command examples and configuration.

### Runtime and performance

Performance work in .NET 11 reaches both the code you deploy and the tools you use to develop it. Stephen Toub’s [Performance Improvements in .NET 11](https://devblogs.microsoft.com/dotnet/performance-improvements-in-net-11/) takes a detailed tour through hundreds of runtime and library optimizations, with benchmarks and code examples.

The [just-in-time (JIT) compiler improvements](https://learn.microsoft.com/dotnet/core/whats-new/dotnet-11/runtime#jit-improvements) remove more redundant bounds checks and branches, improve devirtualization, and fold more operations into constants. For example, the JIT can optimize additional `SequenceEqual` calls when it knows the inputs at compile time. These changes improve generated code without requiring you to rewrite the corresponding application logic.

[Native AOT interface dispatch](https://learn.microsoft.com/dotnet/core/whats-new/dotnet-11/runtime#nativeaot-faster-interface-dispatch) uses a shared helper that reduces the code needed at interface call sites and improves throughput for interface-heavy workloads. Hardware support also expands, including FP16 instructions for `Half` operations on supported x64 and Arm64 hardware and new vector construction and composition APIs.

There’s useful work on diagnostics, too. [In-process crash report logging](https://learn.microsoft.com/dotnet/core/whats-new/dotnet-11/runtime#in-process-crash-report-logging) can capture a managed stack trace, module information, and runtime state before a process exits. Previously available on mobile platforms, this reporting path now also supports Linux and macOS.

As always, measure the effect on your own application. Throughput, startup time, memory use, and deployment size depend on the workload and publishing options you choose.

Runtime Async has been a recurring theme throughout the .NET 11 previews. It moves asynchronous suspension and resumption into the runtime instead of relying on compiler-generated state machines.

One especially useful result is a clearer *live* call stack. Profilers and debuggers can show the asynchronous methods themselves instead of the infrastructure used to execute them. This is a distinction from exception stack traces, which already hide much of that infrastructure.

**Runtime Async remains a preview feature.** To try it in a project targeting `net11.0`, add:

```xml
<PropertyGroup>
  <Features>runtime-async=on</Features>
</PropertyGroup>
```

You don’t need to enable `EnablePreviewFeatures` for this scenario. The .NET runtime libraries themselves are built with Runtime Async, but opting your application’s code into the new compilation model is a separate choice. See the [Runtime Async documentation](https://learn.microsoft.com/dotnet/core/whats-new/dotnet-11/runtime#runtime-async) for its behavior and current guidance, and test your application’s dependencies and diagnostics before adopting it.

### AI libraries and tools

The .NET 11 SDK includes an [MCP server template](https://learn.microsoft.com/aspnet/core/release-notes/aspnetcore-11#mcp-server-template-ships-with-the-net-sdk). MCP gives AI applications a common way to discover and call tools and services. The template uses the [official MCP C# SDK](https://devblogs.microsoft.com/dotnet/announcing-v20-of-the-official-mcp-csharp-sdk/) and includes a working sample tool, so you can start exposing your own functionality without first installing a separate template package:

```console
dotnet new mcpserver -o MyMcpServer
```

For applications that consume AI services, [Microsoft.Extensions.AI (MEAI)](https://learn.microsoft.com/dotnet/ai/microsoft-extensions-ai) provides abstractions such as `IChatClient` and `IEmbeddingGenerator`. These libraries let you compose model access with dependency injection, caching, and OpenTelemetry using familiar .NET patterns. They’re available as NuGet packages, rather than features that require you to move all of your application code to a new framework version.

[Microsoft Agent Framework (MAF)](https://learn.microsoft.com/agent-framework/overview/agent-framework-overview) builds on these abstractions with agents that call tools and maintain conversation state, plus workflows that coordinate agents and application code. Connect agents to MCP tools to give them access to your application’s capabilities, and expose an agent through an ASP.NET Core AG-UI endpoint to build an interactive Blazor experience. The AG-UI hosting integration is currently prerelease.

For hands-on examples, start with [.NET AI Essentials: The Core Building Blocks Explained](https://devblogs.microsoft.com/dotnet/dotnet-ai-essentials-the-core-building-blocks-explained/) and continue with our [Microsoft Agent Framework walkthrough](https://devblogs.microsoft.com/dotnet/microsoft-agent-framework-building-blocks-for-ai-part-3/).

### Libraries

The .NET libraries add capabilities that previously required extra setup, intermediate buffers, or custom helpers.

The [`Process` API expansion](https://learn.microsoft.com/dotnet/core/whats-new/dotnet-11/libraries#process-api-expansion) includes one-shot methods to run a process and capture its output and exit status. More advanced APIs give you control over inherited handles, suspended starts, and process signaling. Build tools and developer utilities can spend less code on process plumbing while still inspecting how a child process finished. See [Process API Improvements in .NET 11](https://devblogs.microsoft.com/dotnet/process-api-improvements-in-dotnet-11/) for examples.

[Zstandard compression](https://learn.microsoft.com/dotnet/core/whats-new/dotnet-11/libraries#zstandard-compression) joins the existing compression formats in `System.IO.Compression`. New span-based compression APIs support callers that manage their own buffers, and ZIP improvements include CRC32 validation when reading entries.

Four new [in-memory stream adapters](https://learn.microsoft.com/dotnet/core/whats-new/dotnet-11/libraries#in-memory-stream-adapters) bridge existing data to APIs that accept a `Stream`: `ReadOnlyMemoryStream`, `WritableMemoryStream`, `ReadOnlySequenceStream`, and `StringStream`. For example, a pipeline’s segmented `ReadOnlySequence<byte>` can be read as a stream without first flattening it into a new contiguous buffer.

[`System.Text.Json`](https://learn.microsoft.com/dotnet/core/whats-new/dotnet-11/libraries#systemtextjson-improvements) adds support for C# unions, F# discriminated unions, and JSON Lines output from asynchronous sequences. F# union support uses reflection and doesn’t support source generation, trimming, or Native AOT, so check the documented limits before choosing it for a trimmed application.

LINQ adds [`FullJoin`](https://learn.microsoft.com/dotnet/core/whats-new/dotnet-11/libraries#linq-join-improvements), which preserves unmatched elements from both inputs. This is useful for reconciliation work, such as comparing two inventories where you need to see missing items on either side.

The libraries also expand numeric types, DNS resolution, cryptography, and tracing configuration. The [library overview](https://learn.microsoft.com/dotnet/core/whats-new/dotnet-11/libraries) covers the complete set of changes and marks APIs that remain experimental.

### Entity Framework Core 11

[Entity Framework Core 11](https://learn.microsoft.com/ef/core/what-is-new/ef-core-11.0/whatsnew) improves how your model becomes a query and how that query becomes SQL.

EF Core removes unnecessary joins in more reference-navigation and split-query scenarios, improves `GroupBy` translation, and translates the new LINQ `FullJoin` operator on relational providers that support it. These changes can simplify generated SQL while letting more queries stay in LINQ.

Complex types and JSON columns can now be used on entities mapped with table-per-type or table-per-concrete-type inheritance. Other improvements include migrations tooling and additional provider-specific JSON capabilities.

For SQL Server applications, EF Core adds support for vector indexes and approximate vector search. **The SQL Server features and corresponding EF APIs are experimental.** See the [vector search documentation and limitations](https://learn.microsoft.com/ef/core/what-is-new/ef-core-11.0/whatsnew#vector_search-and-vector-indexes) before evaluating them.

EF Core 11 requires .NET 11. When upgrading, check your database provider’s compatibility and the [EF Core 11 breaking changes](https://learn.microsoft.com/ef/core/what-is-new/ef-core-11.0/breaking-changes) as well as the runtime changes.

### Developer tools

The new [dotnetup](https://devblogs.microsoft.com/dotnet/introducing-dotnetup/) toolchain manager lets you install and update .NET SDKs and runtimes in your user account. It’s currently in public preview and helps you manage side-by-side versions and keep up with servicing updates.

[.NET Skills](https://github.com/dotnet/skills) gives coding agents task-specific guidance from the .NET team. Our [.NET Skills introduction](https://devblogs.microsoft.com/dotnet/extend-your-coding-agent-with-dotnet-skills/) explains how to add these skills to your coding agent.

The experimental [.NET MAUI DevFlow](https://learn.microsoft.com/dotnet/maui/developer-tools/devflow/) toolkit exposes a running app’s visual tree, screenshots, and interaction tools, so an agent can inspect and test the UI rather than work only from source code.

For modernization, the [.NET Upgrade canvas](https://learn.microsoft.com/dotnet/core/porting/github-copilot-upgrade/dashboard) gives you a visual view of a GitHub Copilot upgrade session. Review assessment findings and dependency compatibility, follow implementation tasks, and monitor failures and progress as the upgrade agent works. Our [modernization walkthrough in the GitHub Copilot app](https://devblogs.microsoft.com/dotnet/modernize-dotnet-in-github-copilot-app/) covers the workflow from assessment through execution.

## Workloads

### Web: ASP.NET Core and Blazor

Application types are most useful when the rest of the stack understands them. ASP.NET Core supports [C# unions](https://devblogs.microsoft.com/dotnet/unions-and-closed-hierarchies-in-aspnetcore/) wherever it uses `System.Text.Json`, including JSON request and response bodies in Minimal APIs and MVC. Generated OpenAPI documents describe the union’s alternatives with an `anyOf` schema.

This support applies to JSON bodies, not to route, query, header, or form binding. When multiple union cases have the same JSON shape, configure a classifier so deserialization can distinguish them. The [ASP.NET Core union documentation](https://learn.microsoft.com/aspnet/core/release-notes/aspnetcore-11#c-union-types) explains those boundaries.

Validation also becomes more practical for rules that need to do I/O. [Asynchronous validation](https://learn.microsoft.com/aspnet/core/release-notes/aspnetcore-11#async-validation-for-minimal-apis) works with the new asynchronous DataAnnotations APIs, so a validator can await a database lookup or a remote call instead of blocking a thread. Minimal APIs and Blazor forms can use this support.

[OpenAPI 3.2 is now the default](https://learn.microsoft.com/aspnet/core/release-notes/aspnetcore-11#openapi-32-by-default) for generated documents. Among other improvements, it lets the document describe the per-event payload of a server-sent event stream. If a client generator or another part of your toolchain doesn’t yet support 3.2, you can explicitly select an earlier document version.

For long-lived connections, [SignalR authentication refresh](https://learn.microsoft.com/aspnet/core/release-notes/aspnetcore-11#signalr-authentication-refresh) lets a connection refresh authentication without reconnecting. Blazor Interactive Server components can receive the updated identity and react when claims change.

Blazor adds capabilities for both statically rendered pages and interactive applications.

The new [`CacheView` component](https://learn.microsoft.com/aspnet/core/release-notes/aspnetcore-11#cache-rendered-output-of-a-component-subtree-during-static-ssr) caches the rendered output of a component subtree during static server-side rendering. On a cache hit, Blazor replays the markup without creating those child components or running their lifecycle methods. That gives you a smaller unit of caching for expensive, mostly static sections of a page.

Interactive Server apps can opt into [automatic circuit pause when a browser tab becomes inactive](https://learn.microsoft.com/aspnet/core/release-notes/aspnetcore-11#automatic-circuit-pause-on-tab-inactivity). After a configurable delay, pausing frees the server memory and SignalR connection held by an inactive user. The feature requires an additional package and explicit configuration; upgrading doesn’t silently change your application’s circuit behavior.

Forms gain client-side validation for static server rendering and asynchronous validation support. `QuickGrid` and `Virtualize` add ways to move to a particular item, useful when returning a user to their place in a large collection.

Blazor also adds components for agentic user interfaces. The new `Microsoft.AspNetCore.Components.AI` package includes components for streaming chat, tool rendering, approval flows, and shared UI state. Basic chat works with any `IChatClient`. For advanced experiences with a remote agent, the [AG-UI .NET integration](https://learn.microsoft.com/agent-framework/integrations/by-component/ui/ag-ui/) carries streamed responses, tool events, approval requests, and shared-state updates between an ASP.NET Core agent endpoint and the Blazor UI.

**The Blazor AI components package is experimental and remains prerelease throughout .NET 11.** It’s an opportunity to try the direction and give feedback, not a stable part of the ASP.NET Core shared framework. See the [Blazor AI components documentation](https://learn.microsoft.com/aspnet/core/release-notes/aspnetcore-11#experimental-blazor-ai-components-for-agentic-user-interfaces).

The [AG-UI .NET SDK introduction](https://devblogs.microsoft.com/dotnet/ag-ui-dotnet-sdk/) covers the protocol’s client and server APIs. [Build Agentic UI with the new Blazor AI components](https://devblogs.microsoft.com/dotnet/build-agentic-ui-blazor/) puts them together in a working app.

See [what’s new in ASP.NET Core](https://learn.microsoft.com/aspnet/core/release-notes/aspnetcore-11) for the Blazor features, configuration examples, and compatibility details.

### Client: .NET MAUI

[.NET MAUI in .NET 11](https://learn.microsoft.com/dotnet/maui/whats-new/dotnet-11?view=net-maui-11.0) makes CoreCLR the default runtime across its platforms. The [move to CoreCLR](https://devblogs.microsoft.com/dotnet/coreclr-progress-and-mono-timeline-dotnet-maui/) brings mobile and desktop apps onto the same runtime foundation as other .NET workloads. On Apple platforms that don’t allow JIT compilation, the runtime uses precompiled code and the interpreter rather than generating native code at runtime.

Native AOT is an opt-in publishing alternative; its Android support remains experimental. Review the platform-specific runtime guidance and test your app’s libraries when choosing a deployment model.

The UI development loop gets attention, too:

- XAML Incremental Hot Reload is a **preview feature** in .NET 11. It updates live instances of pages and controls, including supported structural edits, without requiring you to navigate away and back. It’s enabled by default in Debug builds.
- Implicit XAML namespace declarations remove the need to repeat the standard namespace declarations at the root of every file.
- Windows uses the new `CollectionView2` handler by default, while Android Shell adopts a handler-based architecture.
- New mobile test templates and `dotnet test` device selection bring testing into the CLI workflow.

These are meaningful runtime and handler changes, so exercise navigation, custom handlers, and trimmed publishing as part of your upgrade. The optional `Microsoft.Maui.Controls.Compatibility` package no longer ships; apps that still reference it need to migrate off it.

.NET MAUI follows its own [support policy](https://dotnet.microsoft.com/platform/support/policy/maui), separate from the .NET runtime’s STS and LTS lifecycle.

### Windows: WinUI, Windows Forms, and WPF

For new Windows desktop apps, [WinUI 3](https://learn.microsoft.com/windows/apps/winui/winui3/) is the recommended native UI framework, pairing C# and XAML with modern controls and the Fluent Design System. It’s delivered through the [Windows App SDK](https://learn.microsoft.com/windows/apps/windows-app-sdk/), which also makes modern Windows APIs available to existing WPF and Windows Forms apps.

To add UI tests, see [UWP and WinUI 3 apps: UI testing with MSTest](https://devblogs.microsoft.com/dotnet/testing-uwp-and-winui-3-apps-with-mstest/).

[Windows Forms in .NET 11](https://learn.microsoft.com/dotnet/desktop/winforms/whats-new/net110) adds an opt-in modern visual styles mode that you can select at the application or control level. It also adds ways to react to system visual and accessibility settings, defer revealing a form until it’s ready, and suspend painting during bulk updates.

These controls let you introduce refreshed rendering where it makes sense for your application instead of treating a framework upgrade as an automatic visual redesign.

[WPF updates](https://learn.microsoft.com/dotnet/desktop/wpf/whats-new/net110) focus on reliability, including fixes for Fluent window backdrops, window manager failures, and weak-event cleanup. Self-contained Windows publishing also includes a native dependency needed by apps that load mixed-mode WPF assemblies.

## Get started and plan your upgrade

Install the [.NET 11 SDK](https://dotnet.microsoft.com/download/dotnet/11.0). Use a compatible, up-to-date [Visual Studio 2026](https://visualstudio.microsoft.com/) installation, or [Visual Studio Code with C# Dev Kit](https://code.visualstudio.com/docs/csharp/get-started).

To create and run a console app:

```console
dotnet new console -n HelloDotnet11 -f net11.0
dotnet run --project HelloDotnet11
```

You can replace the generated program with either of the C# examples above to try the new language features.

For an existing application, plan the upgrade across your development and deployment environments:

1. Update the SDK selection in `global.json` if your repository pins it, and install the matching SDK on development machines and build agents.
2. Update target frameworks to .NET 11, preserving platform-specific target framework suffixes where applicable. Update packages and workloads to compatible versions.
3. Review the [.NET 11 breaking changes](https://learn.microsoft.com/dotnet/core/compatibility/11), including the [updated minimum CPU requirements](https://learn.microsoft.com/dotnet/core/whats-new/dotnet-11/runtime#updated-minimum-hardware-requirements). Installing a new SDK alone doesn’t retarget your app.
4. Run your tests and measure the application under a representative workload. Validate your actual publishing mode, container base images, native dependencies, and deployment hosts.

**Upgrading from .NET 8 or .NET 9**

.NET 8 and .NET 9 reach end of support on November 10, 2026. Move to a supported release to continue receiving fixes. Choose .NET 11 for the latest capabilities, or .NET 10 if its LTS lifecycle fits your requirements better. See the [.NET support policy](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) for dates and servicing requirements.

If you’re upgrading from a .NET 11 release candidate, update to the final SDK, runtime, and compatible package and workload versions rather than staying on a prerelease build.

## Thank you for building .NET with us

.NET 11 is the work of **over 7,000 contributors**. Thank you to everyone who contributed code, tested previews, reported issues, and helped us make better design decisions throughout the year. Your feedback across the previews and release candidates helped shape what we’re shipping today.

The .NET ecosystem continues to grow, with more than **589,000 unique packages** on [NuGet](https://www.nuget.org/) and over **1.1 trillion package downloads**.

![Collage of .NET 11 contributors' avatars](https://devblogs.microsoft.com/dotnet/wp-content/uploads/sites/10/2026/11/dotnet-11-contributors-scaled.webp)

[Download .NET 11](https://dotnet.microsoft.com/download/dotnet/11.0), try it with an application you know well, and join us at [.NET Conf](https://www.dotnetconf.net/) to learn directly from the people building it. Share your results and questions in the [.NET GitHub discussions](https://github.com/dotnet/core/discussions).

## Author

![.NET Team](https://devblogs.microsoft.com/dotnet/wp-content/uploads/sites/10/2023/01/dotnet-logo-96x96.png)

.NET is the free, open-source, cross-platform framework for building modern apps and powerful cloud services.
