---
title: systemd loves Rust
link: https://amutable.com/blog/systemd-loves-rust
source: lobste-rs
published: 2026-11-08T12:55:05Z
updated: 2026-11-08T12:55:05Z
first_seen: 2026-11-11T15:04:08.373515189Z
authors:
- amutable.com via lorenzleutgeb
labels:
- rust
- systemd
summary: Comments
content: extracted
html: 2026-11-08-systemd-loves-rust.html
remote_preview:
  url: https://amutable.com/blog/systemd-loves-rust/opengraph-image?df4a8dd696d0f636
  alt: Amutable blog post
---

[systemd](https://systemd.io/) recently [merged support for Rust](https://github.com/systemd/systemd/pull/44324). Going forward it will be possible to write systemd components in Rust in addition to C.

The choice to integrate another core language into a large and established project is always contentious. We have seen the same when Rust was adopted as an additional language for the [Linux kernel](https://docs.kernel.org/rust/index.html). As we know by now, that has mostly been successful.

Rust support in systemd has two possible meanings:

1. leaf components (individual binaries)
2. root components (Rust in the fundamental libsystemd-shared.so shared library itself)

The project came to the conclusion that (1) is acceptable while (2) is (currently) not, as the scope and requirements are very different between these two options.

Option (2) is complicated. The details are best left to another blog post but the gist is that we require first-class shared library support in Rust. In the Rust community, this is often referred to as the “stable ABI problem,” which is a [well-known and long-standing issue](https://github.com/rust-lang/rfcs/issues/600). Until that is available we cannot integrate Rust into root components, otherwise every shared library and many binaries would be bloated by duplicated Rust code.

Static linking for a project like systemd that is split into many tiny components (> 180 binaries currently) is simply not practical or desirable. Generally, static linking works for the leaves of the OS but the core often has different requirements.

## Rust in systemd Binaries 😍

Option (1) means that the shared code remains C-only, and we wrap this functionality as Rust crates. The crates are then used for Rust binaries. This allows us to consume all the core API functions uniformly across all components independent of whether they are written in Rust or in C.

## Key Design Decisions

When Rust was accepted into systemd, the project adopted the following [principles](https://github.com/systemd/systemd/blob/main/docs/CODING_STYLE.md#rust):

- **Uniform Access to Core APIs.** The systemd shared library has a wide range of helpers that provide features and security that are far beyond what either libc or any Rust crate provide.

  The clearest example is systemd’s `chase*()` family of functions. It is a purely fd-based API for path lookup that the whole tree uses. Any new kernel feature that improves on the security and reliability of path lookup is immediately adopted and securely abstracted. There is no equivalent to any of this in core Rust.

- **Uniform Behavior.** Binaries in C and in Rust look and behave identically in how they parse options, display help, and output error messages.

  It is inherently unacceptable for binaries in Rust to spuriously differ from systemd’s C binaries. The user should never have to care about the specific language a binary is implemented in. It simply shouldn’t affect behavior.

- **Uniform Build System.** Like most contemporary programming languages, Rust comes with its own build and dependency management ecosystem: [cargo](https://doc.rust-lang.org/cargo/). Much like the kernel, systemd explicitly chose not to rely on cargo to build Rust.

  systemd uses [meson](https://mesonbuild.com/) as its build system and integrating a second build system next to it would be a bad idea in our opinion and a great way of complicating the lives of distribution maintainers.

  The security and supply-chain attack issues associated with cargo have been raised as a concern as well.

- **Controlled Dependencies.** systemd maintains strictly loose dependencies on other projects. Nowadays, systemd links exclusively against libc. Every other dependency is provided via [`dlopen()`](https://man7.org/linux/man-pages/man3/dlopen.3.html) using [ELF notes](https://uapi-group.org/specifications/specs/elf_dlopen_metadata/) to mark the dependencies.

  Accepting a new dependency is a careful process and involves vetting the individual projects in terms of maintenance and security.

  While systemd doesn’t oppose the usage of external crates in principle, it is intentionally conservative in not accepting any dependency on them until the project has had some experience with the wider Rust ecosystem and community.

- **No-std Rust.** systemd also doesn’t use the [Rust standard library](https://doc.rust-lang.org/reference/names/preludes.html#the-no_std-attribute). We currently don’t need to consume anything that the standard library provides. To reiterate an earlier point by way of example, none of the path lookup functionality provided by Rust is currently acceptable in the security sensitive system components provided by systemd.

- **Fallible Allocations.** Failing allocations in Rust cause panics. systemd always treats allocations as fallible and handles them gracefully whenever possible. So systemd provides its own allocator crate that makes memory handling across the whole systemd codebase uniform. Again, much like the kernel does.

- **No Gratuitous Rewrites.** Long-established and reliable parts of the codebase written in C will not be converted without good reason.

  If new languages have one downside it is that they cause rewrites of established and stable projects for the sake of using the language rather than solving an actual problem.

  One of the clearest examples is rewrites of components whose inherent attack surface is path-based lookup instead of memory safety. Projects ported to another language need more than a bug-for-bug compatible rewrite to actually benefit from the rewrite. It’s far more important to focus on removing their core attack surface.

- **Rust Remains Optional.** The feature set available to existing users will not regress. We’re aware that Rust does not (yet) support all possible architectures, and there are places where introducing Rust would be complicated, so for now at least, Rust remains optional, and systemd without Rust remains fully usable and supported.

## Closing Thoughts

Integrating Rust into systemd is an ongoing process. It is exciting to see where the many maintainers of the project are going to take it and how this will evolve. Rust is a good language. Its syntax is a matter of acquired taste and learning but it does bring a range of features to the table that an established project wants to use. One of the systemd maintainers recently discovered that his work on adopting [Fibers](https://github.com/systemd/systemd/pull/39771) in systemd naturally integrates with Rust in a surprisingly elegant way.

systemd would also like to see Rust adopted in core components and is willing to engage with the Rust community to make this a reality. The hope is that adopting Rust, even if only in leaf binaries for now, sends another strong signal to the Rust community that core components of the Linux stack are adopting Rust. It would be nice if fundamental blockers such as the lack of first-class support for shared libraries or the ability to interact with the file system primarily through file descriptors were addressed.

It was already an encouraging sign when the Rust community [quickly reverted a change in Rust 1.100](https://github.com/rust-lang/rust/pull/164855) that would’ve broken the newly merged Rust support in systemd. This is much appreciated and it is great to see that the community is paying attention. The systemd project is paying attention too!
