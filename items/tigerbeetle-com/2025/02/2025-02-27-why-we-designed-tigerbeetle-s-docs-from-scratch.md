---
title: Why We Designed TigerBeetle's Docs from Scratch
link: https://tigerbeetle.com/blog/2025-02-27-why-we-designed-tigerbeetles-docs-from-scratch
source: tigerbeetle-com
published: 2025-02-27T00:00:00Z
updated: 2025-02-27T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Fabian Rühle
- Fabio Arnold
- matklad
summary: We recently rebuilt TigerBeetle’s docs site from scratch. We wanted to apply TigerStyle and first principles thinking, not only to our database but also to our docs. To give users the fastest possible experience, given how much time they spend reading, and to do this without the dependency of Docusaurus. At TigerBeetle, how we build things is as important as what we build, because methodology has a second order effect on motivation.
content: extracted
html: 2025-02-27-why-we-designed-tigerbeetle-s-docs-from-scratch.html
remote_preview:
  url: https://tigerbeetle.com/blog/2025-02-27-why-we-designed-tigerbeetles-docs-from-scratch/banner-4521ba4c.webp
---

We recently rebuilt TigerBeetle’s docs site from scratch. We wanted to apply TigerStyle and first principles thinking, not only to our database but also to our docs. To give users the fastest possible experience, given how much time they spend reading, and to do this without the dependency of Docusaurus. At TigerBeetle, *how* we build things is as important as *what* we build, because methodology has a second order effect on motivation.

When TigerBeetle was founded, [Docusaurus](https://docusaurus.io/) was used to prototype our docs site. It was quick to start, and we could maintain [our docs in our main GitHub repository](https://github.com/tigerbeetle/tigerbeetle/tree/main/docs).

There were, however, some downsides:

- Violation of our zero dependency principle: as Docusaurus is based on NodeJS, it added [a lot of dependencies](https://github.com/tigerbeetle/tigerbeetle/blob/0.16.23/src/docs_website/package-lock.json) to our code base. We have a check that no large files are committed to git, and package-lock.json failed this check.
- Complexity: Docusaurus is fundamentally a complex piece of software – a React app. While it’s good at what it does, we believe that an equally good result can be achieved with simpler methods.
- Unwanted additional code: Docusaurus adds [extra artifacts](https://github.com/tigerbeetle/tigerbeetle/blob/2fc953a7c788c6f28e23bb65c5a16c1c782ffbcc/docs/README.md?plain=1#L1-L4) within the Markdown files which also show up visibly on GitHub (i.e. metadata and extra `_category_.json` files) and we wanted GitHub’s representation of our docs to be pure.
- Sub-optimal search: the search experience provided through an external service, was a bit fuzzy and not optimally integrated.
- But, most importantly, we wanted to treat our documentation as a part of the database itself. That means that docs must use the same standard of craftsmanship we put into the database proper, or they won’t *feel* like an integral part of the database.

Docusaurus served well as a prototype but the time for change had arrived. We set out to create the experience we’d prefer when browsing and reading documentation: simple, clear, and fast. With not only a great “external” user experience but also a great “internal” user experience (the implementation of the code itself, when you pop the hood, e.g. the footprint over the network). So the path we took was shaped not only by our design but also our engineering considerations.

![Docs using Docusaurus with annotions on what to change](https://tigerbeetle.com/blog/2025-02-27-why-we-designed-tigerbeetles-docs-from-scratch/before-7030edc4.png)\
Docs using Docusaurus with annotions on what to change

How could we provide the best reading experience for our readers? The reading experience should be as clean and simple as a book. We wanted to remove as many distractions as possible, to keep the design minimalistic. We gave the content more space by removing elements like breadcrumbs, table of contents, footer and by integrating the top navigation into the left side navigation. We switched dark/light mode automatically based on your system settings, so you wouldn’t have to toggle that manually (and so we wouldn’t bring in any chrome for that) and made it possible to hide the side navigation or adjust its width for maximum focus.

![](https://tigerbeetle.com/blog/2025-02-27-why-we-designed-tigerbeetles-docs-from-scratch/nav-scale-d3abec9a.webp)

Given that TigerBeetle’s only dependency is the Zig compiler, we started exploring what a static site generator (SSG) based on Zig could look like. At the time, our friend [Loris Cro](https://kristoff.it/) was building his own SSG, [Zine](https://zine-ssg.io/), and [some companies were already using it to generate their docs](https://x.com/gwenzek/status/1835983748761817433).

We got excited and considered Zine. However, Zine uses SuperMD, described as an extension to Markdown and fundamentally a different type of Markdown. It’s important to understand that our docs are content first, and can be viewed in three different forms:

1. As raw text in your editor of choice
2. On GitHub
3. On our [website](https://docs.tigerbeetle.com)

For this reason, it is required that the content is GitHub-flavored Markdown ([GFM](https://github.github.com/gfm/)). And unfortunately, we found some incompatibilities between SuperMD and GFM. For example, by design, SuperMD does not support raw inline HTML while GFM does. Instead of rewriting our docs using only the subset that is supported by both GFM and SuperMD, we decided to explore more options…

To be honest, the hard part of static site generation is parsing the Markdown, since Markdown is a complex language. Everything around it is simple scripting, which we can easily do ourselves. Luckily, there is [Pandoc](https://pandoc.org) – a rock solid tool created by the editor of the CommonMark specification himself. So we made an exception to depend on it. We use Zig’s built-in package manager to pull in Pandoc as a single statically built executable (verifying its hash of course) and then use Zig’s build system to parallelize the translation of Markdown source files into the final HTML. (Fun fact: this blog is generated using the same setup!)

You can find the full implementation [on GitHub](https://github.com/tigerbeetle/tigerbeetle/tree/0.16.29/src/docs_website), but, for the purposes of this blog post, we’ll actually write a tiny prototype right on the spot. This is a useful exercise, as this tiny program teaches *a lot* about the philosophy behind `build.zig`!

The big picture is that we cast the entire static site generation as just a build task for the Zig build system. This will get us incremental updates for free. That is, if you change a single Markdown file and re-build the website, only this single file will be updated. Using build.zig for this is a brilliant idea. Sadly, it’s not ours — we shamelessly stole it from Zine (thanks, Loris!).

As described above, we want to use Pandoc to do the heavy lifting of parsing Markdown and converting it to HTML. We have a *dependency* on a system utility, Pandoc! This is the place where people usually reach out for heavy artillery of Docker or Nix, which solves the problem of “works on my machine” by turning *your* machine into mine!

But with Zig, you often can handle system dependencies directly and without headache, provided that a static build is available for download. This is true of Pandoc. Its [GitHub releases](https://github.com/jgm/pandoc/releases/tag/3.6.3) contain prebuilt binaries for all operating systems and CPU architectures relevant for us. To use these binaries, we add them to `build.zig.zon` (`zon` stands for Zig Object Notation):

```
.{
  .name = "Build Zig Docs",
  .version = "0.0.0",
  .dependencies = .{
    .pandoc_macos_arm64 = .{
      .url = "https://github.com/jgm/pandoc/releases/download/3.4/pandoc-3.4-arm64-macOS.zip",
      .hash = "1220c2506a07845d667e7c127fd0811e4f5f7591e38ccc7fb4376450f3435048d87a",
      .lazy = true,
    },
    .pandoc_linux_amd64 = .{
      .url = "https://github.com/jgm/pandoc/releases/download/3.4/pandoc-3.4-linux-amd64.tar.gz",
      .hash = "1220139a44886509d8a61b44d8b8a79d03bad29ea95493dc97cd921d3f2eb208562c",
      .lazy = true,
    },
  },
  .paths = .{"."},
}
```

Here, we have two dependencies, one for MacOS and one for Linux. Crucially, dependencies are specified with the content hash! This fixes reproducibility, and severely diminishes the risk of a supply-chain attack — every user of our static-site generator is guaranteed to get the bit-for-bit identical version of Pandoc!

Importantly, the hash isn’t the *physical* hash of the `.zip` archive itself. It is a *logical* hash of the contents of the archive. To compute the hash, Zig unpacks the archive, and then recursively walks the resulting file tree, hashing file contents. In other words, the actual *identity* of the dependency is its hash, and the url is just a suggestion for how to get the right content. If, for whatever reason, I already have a directory on my local drive with the content with a matching hash, I can substitute it in place of the `.zip` archive because the hash is the same, and only the hash matters.

Of course, extracting random archives downloaded from the internet is a security nightmare, as archive formats are poorly specified, redundant, and contain ample negative space to hide various nastiness in it. But Zig *doesn’t* actually support the full generality of various archive formats. It doesn’t shell out to 3rd party utilities for unarchiving, and it doesn’t support *any* file attributes. It only extracts the *content*, which can be done relatively safely: while it is still hard to do robustly, it is a *much* narrower scope than supporting everything a typical archive format supports.

Content-only extraction creates a puzzle: how to handle executables like our Pandoc? As attributes are not preserved, the file won’t be marked as `+x`. The answer is that content is king! If a file has a hash-bang, or an ELF header (or, [after our PR](https://github.com/ziglang/zig/pull/21555), a Mach-O header), it is marked as executable automatically.

Using this dependency specification, we can gain access to a Pandoc binary from our build:

```
const std = @import("std");
const builtin = @import("builtin");
const os = builtin.os;
const cpu = builtin.cpu;

pub fn build(b: *std.Build) !void {
    const pandoc_dependency = if (os.tag == .linux and cpu.arch == .x86_64)
        b.lazyDependency("pandoc_linux_amd64", .{}) orelse return
    else if (os.tag == .macos and cpu.arch == .aarch64)
        b.lazyDependency("pandoc_macos_arm64", .{}) orelse return
    else
        return error.UnsupportedHost;
    const pandoc = pandoc_dependency.path("bin/pandoc");
}
```

Here, we make use of so-called lazy dependencies. Depending on the host OS, only *one* of the binaries needs to be downloaded. To avoid downloading both, it would have been possible to add some extra platform metadata to the `build.zig.zon`, but Zig implements a more elegant and general solution.

Zig’s `lazyDependency` function returns a dependency by name, but can also return `null` if the dependency isn’t fetched yet. In that case, the build is simply re-run after the dependency is downloaded.

So, the above code is run twice! On the first run, we bail out from `orelse return`, but Zig also learns which of the two dependencies is needed. On the second iteration, we get our Pandoc. This is [Capturing the Future by Replaying the Past](https://arxiv.org/abs/1710.10385)!

Next, we need to get the list of Markdown files to convert. We can write code to walk the file system, using `Dir.Iterator`, but, for illustrative purposes, let’s cheat and make `git` do all the heavy lifting here:

```
const markdown_files = b.run(&.{ "git", "ls-files", "content/*.md" });
var lines = std.mem.tokenizeScalar(u8, markdown_files, '\n');
while (lines.next()) |file_path| {

}
```

`b.run` is a convenience function which directly runs an external command, and returns its output as `[]const u8`, which we can split line-by-line and iterate.

We *can* use `b.run` to run Pandoc as well, but then we won’t be able to take advantage of the build system caching. `b.run` runs the command every time you invoke `zig build`, and should be avoided, if possible. Instead, the work should be delegated to build tasks with tracked inputs. That’s what we do for Pandoc:

```
fn markdown2html(
    b: *std.Build,
    pandoc: std.Build.LazyPath,
    markdown: std.Build.LazyPath,
) std.Build.LazyPath {
    const pandoc_step = std.Build.Step.Run.create(b, "run pandoc");
    pandoc_step.addFileArg(pandoc);
    pandoc_step.addArgs(&.{ "--from=markdown", "--to=html5" });
    pandoc_step.addFileArg(markdown);
    return pandoc_step.captureStdOut();
}
```

The `markdown2html` function doesn’t run Pandoc directly, and instead just creates a recipe for how to run it. The build system can then decide if it actually needs to be run, or if a cached result can be re-used. That’s why the inputs and outputs of the function are `LazyPath`s. A `LazyPath` is a *logical* path pointing at some file on the file system, but the file doesn’t necessarily exist already, it might be created in the future.

In other words, if some other step would require the result of `markdown2html`, then Zig will run Pandoc, passing the input file in, and capturing Pandoc’s stdout. If the file will be needed again, the old result will be re-used. If the input file or Pandoc itself changes, the result will be regenerated.

Now, we can create a recipe for the entire website — we start with an empty directory and add all markdown-processed files:

```
const website = b.addWriteFiles();

const markdown_files = b.run(&.{ "git", "ls-files", "content/*.md" });
var lines = std.mem.tokenizeScalar(u8, markdown_files, '\n');
while (lines.next()) |file_path| {
    const markdown = b.path(file_path);
    const html = markdown2html(b, pandoc, markdown);

    // map `content/pure-awesomeness.md` to `pure-awesomeness.html`
    var html_path = file_path;
    html_path = cut_prefix(html_path, "content/").?;
    html_path = cut_suffix(html_path, ".md").?;
    html_path = b.fmt("{s}.html", .{html_path});

    _ = website.addCopyFile(html, html_path);
}

fn cut_prefix(text: []const u8, prefix: []const u8) ?[]const u8 {
    if (std.mem.startsWith(u8, text, prefix))
        return text[prefix.len..];
    return null;
}

fn cut_suffix(text: []const u8, suffix: []const u8) ?[]const u8 {
    if (std.mem.endsWith(u8, text, suffix))
        return text[0 .. text.len - suffix.len];
    return null;
}
```

To emphasize it again, this loop doesn’t actually run Pandoc. Rather, it creates a graph of tasks, which looks like this:

> To get the directory `website`, you need to run Pandoc for all these input `.md` files, and write Pandoc’s stdout to the corresponding `.html` files.

Finally, to kick everything off, we ask to *install* the directory into `zig-out`:

```
b.installDirectory(.{
    .source_dir = website.getDirectory(),
    .install_dir = .prefix,
    .install_subdir = ".",
});
```

Putting everything together:

```
const std = @import("std");
const builtin = @import("builtin");
const os = builtin.os;
const cpu = builtin.cpu;
const assert = std.debug.assert;

pub fn build(b: *std.Build) !void {
    const pandoc_dependency = if (os.tag == .linux and cpu.arch == .x86_64)
        b.lazyDependency("pandoc_linux_amd64", .{}) orelse return
    else if (os.tag == .macos and cpu.arch == .aarch64)
        b.lazyDependency("pandoc_macos_arm64", .{}) orelse return
    else
        return error.UnsupportedHost;
    const pandoc = pandoc_dependency.path("bin/pandoc");

    const website = b.addWriteFiles();

    const markdown_files = b.run(&.{ "git", "ls-files", "content/*.md" });
    var lines = std.mem.tokenizeScalar(u8, markdown_files, '\n');
    while (lines.next()) |file_path| {
        const markdown = b.path(file_path);
        const html = markdown2html(b, pandoc, markdown);

        var html_path = file_path;
        html_path = cut_prefix(html_path, "content/").?;
        html_path = cut_suffix(html_path, ".md").?;
        html_path = b.fmt("{s}.html", .{html_path});

        _ = website.addCopyFile(html, html_path);
    }

    b.installDirectory(.{
        .source_dir = website.getDirectory(),
        .install_dir = .prefix,
        .install_subdir = ".",
    });
}

fn markdown2html(
    b: *std.Build,
    pandoc: std.Build.LazyPath,
    markdown: std.Build.LazyPath,
) std.Build.LazyPath {
    const pandoc_step = std.Build.Step.Run.create(b, "run pandoc");
    pandoc_step.addFileArg(pandoc);
    pandoc_step.addArgs(&.{ "--from=markdown", "--to=html5" });
    pandoc_step.addFileArg(markdown);
    return pandoc_step.captureStdOut();
}

fn cut_prefix(text: []const u8, prefix: []const u8) ?[]const u8 {
    if (std.mem.startsWith(u8, text, prefix)) return text[prefix.len..];
    return null;
}

fn cut_suffix(text: []const u8, suffix: []const u8) ?[]const u8 {
    if (std.mem.endsWith(u8, text, suffix)) return text[0 .. text.len - suffix.len];
    return null;
}
```

Now, when we run `zig build`, Zig will evaluate our build-script, which will return a task graph, whose `install` task will contain the description of the website directory. The build system will then notice that the website isn’t built yet, and will execute the task, running Pandoc. The end result will be copied to the `./zig-out` directory. If we change one Markdown file and re-run `zig build`, the build system will notice that only one Pandoc tasks needs to be re-run.

You can *also* run `zig build --watch`, and get live rebuild for free! How cool is that!

As an aside, you might be wondering where’s all the memory management? How come we use a language with manual resource management and don’t have a single `defer` in sight? The secret of happy resource management is to avoid managing resources. What we are writing is the “configure” part of a build script, which has a very clear, short lifetime, doesn’t need to allocate a lot, and doesn’t need much scratch space. So all the allocations we do go into the build’s local arena (a field of `b`), and are cleaned up in a batch, when the entire `b: *std.Build` is destroyed.

Our docs site is built with ‘vanilla’ web technology: plain HTML and CSS. And if JavaScript is enabled, you get interactive elements such as [the easter egg](https://tigerbeetle.com/blog/2025-02-27-why-we-designed-tigerbeetles-docs-from-scratch#in-closing-paying-homage) and of course our new search.

We started out with implementing a search similar in behavior to the external search service, which was fuzzy and which would accept minor typos ([Levenshtein distance](https://en.wikipedia.org/wiki/Levenshtein_distance) <= 1) and slightly different word ordering. However, we realized we preferred a more precise search as if you had all docs on a single page and could use CTRL+F on that page.

To achieve an experience as close to this concept as possible we decided to integrate the search results into the left-hand navigation, to eliminate the need for a search result page or the old modal the external service used. This would also enable the user to browse all search results directly in the content, making search simpler and faster to navigate.

We added keyboard shortcuts to make search faster. You can now search by pressing the ‘/’ key. While typing you can immediately browse results using the up and down arrow keys. The entire navigation menu can be toggled using the ‘M’ key.

![Showing new search navigation using arrow keys](https://tigerbeetle.com/blog/2025-02-27-why-we-designed-tigerbeetles-docs-from-scratch/search-c47eb965.webp)\
Showing new search navigation using arrow keys

It’s worth mentioning that the site stays usable even if you disable JavaScript. You can collapse and expand parts of the navigation tree, but of course you won’t be able to use the search without JS. We optimize the footprint as much as possible, while making sure the code remains readable (again, for a great “pop the hood” experience). For example, we don’t use minification but instead, rely on compression during transport.

Another progressive enhancement is that we preload and cache all doc pages in the background (of which there are around 60, for a total footprint of ~500 KiB of compressed data) using a [Service Worker](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API/Using_Service_Workers). This way, when a user clicks on a link, it opens instantly. We also made the deliberate choice to turn off any UI animations because we believe that docs shouldn’t make you wait artificially but should feel as ‘snappy’ as possible.

In removing Docusaurus, we reduced the deployment to a single \`zig build\` command that does all the work. This makes our CI less flaky because we don’t have to pull in all of Docusaurus’ dependencies over the network.

We employed a [Content Security Policy](https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP) to prevent Cross Site Scripting (XSS) as defense-in-depth, in case a seemingly friendly PR contains some innocent looking MathML. This [MathML could contain obfuscated code that would run in the user’s browser](https://issues.chromium.org/issues/40090296). CSP prevents any unwanted inline scripts from running and keeps our users safer.

When comparing bytes, the entire statically built output of our docs site with Docusaurus clocked in total at 20.2 MiB and for the new version we get 3.2 MiB or 2.3 MiB depending on whether we include the local search index. It’s not an apples to apples comparison, because with Docusaurus we still had to use an external search service, but it is an indication of the (roughly 10x) reduction in footprint.

![](https://tigerbeetle.com/blog/2025-02-27-why-we-designed-tigerbeetles-docs-from-scratch/performance-cf1f4ffd.svg)

Of course, with a little reduction in footprint, we had some bytes to add a little something special. We always loved the original network error game in Chrome (the T-Rex that has to jump over an endless series of cacti), which you can see by going to `chrome://dino` in Chrome, and figured that a new rendition would be a great way to pay homage to Docusaurus for all its service over the years. Our resident artist, Joy Machs, drew the graphics, and the backgrounds depict scenarios from our [simulator game](https://sim.tigerbeetle.com/): City Breeze, Red Desert, and Radioactive. Can you guess what’s running at top speed in our version?

You can get to the game through any [404 error](https://docs.tigerbeetle.com/404) (which we hope isn’t encountered too often, thanks [link checker](https://github.com/tigerbeetle/tigerbeetle/blob/61949eecf3e98eeefec3e7ff6c2a65d0b05a3464/src/docs_website/src/file_checker.zig#L155)!).

We have more docs changes planned, so if you have suggestions (or think you might have clocked a docs high score), then we’d love to hear from you.
