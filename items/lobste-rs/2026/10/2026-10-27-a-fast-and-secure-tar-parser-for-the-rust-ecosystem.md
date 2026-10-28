---
title: A fast and secure tar parser for the Rust ecosystem
link: https://astral.sh/blog/tar-codec
source: lobste-rs
published: 2026-10-27T16:59:30Z
updated: 2026-10-27T16:59:30Z
first_seen: 2026-10-28T11:34:18.317588229Z
authors:
- astral.sh by yossarian
labels:
- python
- rust
summary: Comments
content: extracted
html: 2026-10-27-a-fast-and-secure-tar-parser-for-the-rust-ecosystem.html
remote_preview:
  url: https://astral.sh/static/OpenGraph/UV.jpg
---

Starting with 0.13[^1], [uv](https://github.com/astral-sh/uv) uses [tar-codec](https://github.com/astral-sh/astral-codec/tree/main/crates/tar-codec) to parse source distributions.

[tar-codec](https://github.com/astral-sh/astral-codec/tree/main/crates/tar-codec) is roughly twice as fast as our previous tar implementation ([astral-tokio-tar](https://crates.io/crates/astral-tokio-tar)) while also reducing uv's exposure to parser differentials and other security risks.

This blog post is about [tar-codec](https://github.com/astral-sh/astral-codec/tree/main/crates/tar-codec) itself: what it is, and what compelled us to build and optimize it. Read on if you, too, would like to know too much about tar archives.

![Performance of tar-codec relative to astral-tokio-tar, in uv's benchmarks](https://astral.sh/static/SVG/tar-codec-bench.svg)

## Background: uv and tar

*What follows is an extended digression on the tar format. If all you care about is `tar-codec`,* .

As a Python packaging tool, uv is in the business of resolving and installing Python packages. [uv](https://github.com/astral-sh/uv) requests *installable distributions* from various sources (principally package indices like [PyPI](https://pypi.org)) and installs them on the user's behalf. This is how you go from `uv add urllib3` to something you can actually import and use in your own Python codebase.

Installable distributions come in two primary forms:

- "Built"[^2] distributions, also known as *bdists* or (most commonly) *wheels*, are essentially ZIP archives containing structured metadata and a fixed layout. Installers handle these by (more or less) plopping their contents into the right destination directory.

  Many people think that *bdist* is short for "binary distribution," since wheels often contain compiled native code. This one common way to use wheels, but wheels can also contain pure Python. They're just another (and frequently better) way to package and install a distribution.

- "Source" distributions, also known as *sdists*, are essentially tar archives. Unlike built distributions, a source distribution is *essentially*[^3] just a rollup of a source tree's state at release time.

  By definition, source distributions execute arbitrary code at installation time. This makes them very powerful, but brittle (missing build tools or dependencies) and also a security risk (an attacker who controls the sdist can run code without the user needing to `import` anything).

Wheels are generally produced by running a [build backend](https://peps.python.org/pep-0517/) on a source distribution. Examples of that include uv's own [uv\_build](https://docs.astral.sh/uv/concepts/build-backend/), [maturin](https://github.com/pyo3/maturin), and [flit](https://flit.pypa.io/en/stable/).

Consequently, at a bare minimum, a Python package installer needs to understand at least two different archive formats: ZIP for wheels, and tar for sdists.

We've already [talked a bit before](https://astral.sh/blog/uv-security-advisory-cve-2025-54368) about ZIPs on the Astral blog, and we'll certainly be talking about them again soon enough. But today we'll be focusing on tar.

### Why a new tar parser?

It's fair to ask: why does [uv](https://github.com/astral-sh/uv) want a new tar parser?

In short: security and performance, but mostly security.

Up until 0.13, uv used [astral-tokio-tar](https://crates.io/crates/astral-tokio-tar) for its tar operations, including both parsing and creation (in `uv build`). [astral-tokio-tar](https://crates.io/crates/astral-tokio-tar) is itself a fork of [edera-dev/tokio-tar](https://github.com/edera-dev/tokio-tar), which is a fork of [vorot93/tokio-tar](https://github.com/vorot93/tokio-tar), which is a fork of [dignifiedquire/async-tar](https://github.com/dignifiedquire/async-tar), which is an async *adaptation* of [composefs/tar-rs](https://github.com/composefs/tar-rs).

Around a year ago, we began to receive a [number of security reports](https://github.com/astral-sh/tokio-tar/security/advisories) in [astral-tokio-tar](https://crates.io/crates/astral-tokio-tar). This spurred us into looking more into our compression and archive handling stack, where we found additional problems.

Many of these boiled down to differentials that could cause a full parser desynchronization[^4]: given an identical tarball, [uv](https://github.com/astral-sh/uv), [pip](https://github.com/pypa/pip), and other package tooling could be made to see different contents for the same package distribution. We found this particularly concerning in the context of archives that would appear "safe" to an index (like PyPI) but could extract dangerously when parsed by an installer like [uv](https://github.com/astral-sh/uv) or [pip](https://github.com/pypa/pip).

This could the take the form of an "append" differential (where the attacker can append new files or new contents to existing files for a specific victim parser), or even a "full" differential (where the attacker can fully separate the extraction contents for each parser):

In the context of [uv](https://github.com/astral-sh/uv), the attacker model for these kinds of vulnerabilities is somewhat subtle: tar is only used for source distributions which, per above and unlike wheels, are assumed to execute arbitrary code at build time. Consequently, Python packaging *mostly* considers tar differentials to be a minor concern, since any attacker who *would* use them has a much more direct and official route towards writing arbitrary contents to disk (or executing arbitrary code).

Still, these findings unnerved us, and we [embarked on a project](https://github.com/astral-sh/tokio-tar/pull/95) to eliminate every differential we could find in [astral-tokio-tar](https://crates.io/crates/astral-tokio-tar).

[tar-codec](https://github.com/astral-sh/astral-codec/tree/main/crates/tar-codec) grew directly out of that effort: we ultimately came to the conclusion that the tokio-tar family was *too lenient by design*, and that retrofitting constraints to it would be more troublesome and error-prone (including for existing non-Astral users of its APIs) than developing our own, ground-up implementation of tar. This would also give us the opportunity to systematically understand the tar format, and ensure that our assumptions around which aspects are (or aren't) differential risks hold true.

Similarly, we determined that our performance goals around tar parsing would be impossible to meet in the existing code base, at least not without refactors that would be tantamount to a rewrite.

## (t)archaeology

It's important to understand that one does not simply build a tar parser from scratch. One must first understand *how* the tar format evolved into its current state[^5].

tar is an *interesting* format. Or more precisely, it's an interesting *family* of formats.

To summarize, there are three (and a half) variants that matter in practice:

- There's UNIX V7 tar, which is the "original" tar. Very few tar implementations write this format by default anymore, but virtually all still read it. For example, Python's [`tarfile` module](https://docs.python.org/3/library/tarfile.html#supported-tar-formats) will happily parse V7 tar files.

  The UNIX V7 format is not widely used anymore because it comes with significant limitations on per-member file sizes (approximately 8 GiB) and path lengths (100 bytes).

- Then there's the UStar/pax tar family[^6]. This family is widely supported by tar implementations for both reading and writing. As far as tar goes, UStar and pax are what passes for a "gold standard" in terms of clarity of specification and documentation.

  At this juncture, the main thing to know about the UStar/pax family is that it's derived from the UNIX V7 format, but with certain restrictions lifted:

  - UStar keeps the per-member size limit at 8 GiB, but raises the path length limit to 255 bytes (except for link names, which remain 100 bytes),
  - pax makes per-member sizes and path lengths unlimited, *including* link names.
- Finally, there's GNU tar[^7].

  GNU tar is documented in the [GNU tar manual](https://www.gnu.org/software/tar/manual/html_node/Standard.html), but is not standardized *per se*. The source code of GNU `tar` (the CLI) is the *de facto* standard for GNU tar.

  Like UStar/pax, GNU tar is derived from UNIX V7 tar. It's *also* derived from an [early draft of UStar](https://www.gnu.org/software/tar/manual/html_node/gnu.html).

  Also like pax, GNU tar makes file sizes and path/link lengths (effectively) unlimited. It does so in a completely different and much funnier way than pax does.

That gives us the following genealogy:

If you've ever written a parser for a binary format before, then you probably have a preconception for what a binary format *should* look like. To first, second, and third approximations, binary formats tend to look *something* like this:

Within this, too, certain idioms appear: binary formats typically use some form of [TLV encoding](https://en.wikipedia.org/wiki/Type%E2%80%93length%E2%80%93value), use different magics to signal different endiannesses, &c.

The tar format does not look like this, regardless of the variant. tar is fundamentally a *block format*, owing to its origins as a tape archival format[^8].

As a block format, a tar archive is comprised of one or more fixed-length blocks. There is no file-level header; instead, many of its blocks *are* headers, while other blocks contain actual member[^9] data:

In all flavors of tar, all blocks (including the header blocks) are always 512 bytes. Each header block declares metadata for the *member* that follows it. This includes important things like the size of the member and its filepath.

Member contents don't always fit cleanly into multiples of 512 bytes, so a tar that wants to encode a file member of `N` bytes will allocate `ceil(N / 512)` blocks and pad out the remainder of the last block. This doesn't affect extraction, since the reader knows (via the header block) exactly how many bytes out of the allocated space are actual file content.

Conventionally, the padding or "holes" in a tar archive are filled with NUL bytes. However, nothing stops a tar producer from filling them with uninitialized memory, or lines from the Bee Movie, or anything else. A recurring theme with tar (of all flavors) is that it has no opinions about what goes in the holes of the format. This malleability makes it very prone to parser differentials, since it gives the attacker ample space to place surprising data.

tar's block design also makes it relatively space-inefficient[^10] by modern standards. For example, a 513-byte file occupies an absolute minimum of 1,536 bytes when encoded as a tar member: one 512-byte block for the header, and two blocks for the content (one of which has only 1 "real" byte, followed by 511 bytes of padding). Similarly, a tar archive containing 1000 one-byte files requires a minimum of 1025 KiB of encoding (1 MiB for the headers and contents, and then 1 KiB for the trailing zero blocks).

### tar problems

So far, this all sounds pretty sane. It's not a "normal" file format by modern standards, but there's a simple state machine one could write to formalize a parse for the tar example above:

Unfortunately, it's not that simple. The state machine above is accurate for a V7 tar, but not for anything else in the tar format family.

To understand why, we have to remember V7 tar's limitations. All of those limitations come down to the C structure that represents a V7 member header, which we've annotated here:

```c
struct v7_header {
    // The member's path. Observe that this can be no more than 100 bytes;
    // NUL termination isn't required, since it's a fixed-size field.
	char name[100];

    // The member's mode, as an octal string.
    // Note that only the first 4 octal digits (12 bits) have well-defined semantics in pax.
	char mode[8];

    // The member's owner's UID and GID, as octal strings.
	char uid[8];
	char gid[8];

    // The size of the member, as an octal string.
    // V7 treats this as NUL-terminated, so the largest
    // interoperable value in it is `77777777777\0`.
	char size[12];

    // The member's mtime, as an octal string.
	char mtime[12];

    // A checksum for the member header itself.
	char chksum[8];

    // '\0' for a regular file, '1' for a hard link,
    // and '2' for a symbolic link.
	char linkflag;

    // When `linkflag` is '1' or '2', this is the link's destination.
    // Like `name` this is limited to 100 bytes.
	char linkname[100];

    // 255 bytes of the header are unused.
	char pad[255];
};
```

Three big limitations stand out, and each member of the tar family tries to solve them in various different ways:

1. Member and link names cannot be longer than 100 bytes;
2. Members cannot be larger than `0o77777777777` bytes, i.e. one byte short of 8 GiB;
3. Only regular files and links are defined; there are no directory or other member types[^11].

Let's see how each family works around these.

### UStar and pax

UStar's half-solution is to preserve the existing physical header block, while [impressing](https://en.wikipedia.org/wiki/Impressment) some of its fields and padding into new roles.

This is the UStar header:

```c
struct ustar_header {
    // Unchanged.
	char name[100];
	char mode[8];
	char uid[8];
	char gid[8];
	char size[12];
	char mtime[12];
	char checksum[8];

    // `linkflag` is now `typeflag`, and encodes, inter alia,
    // '5' for a directory.
	char typeflag[1];

    // Unchanged.
	char linkname[100];

    // `"ustar\0"`
	char magic[6];

    // `"00"`
	char version[2];

    // The user and group names for the member.
	char uname[32];
	char gname[32];

    // For chardev/blockdev members, the device's major and minor numbers.
	char devmajor[8];
	char devminor[8];

    // If nonempty, a prefix to prepend to `name` (joined by a `/`).
	char prefix[155];

    // 12 remaining bytes of the header are unused.
	char pad[12];
};
```

This partially lifts the member name restriction (by raising it to 255 bytes), but retains the file size and linkname limitations.

pax fixes these remaining limitations by doing away with the single-block metadata design. Instead, pax has *extensions* that rely on two new UStar typeflags:

- Typeflag `'x'` indicates a *local* extended header block.
- Typeflag `'g'` indicates a *global* extended header block.

In both cases, an extended header block indicates its member's "size." But that size isn't for a file: instead, it's for the extension data itself.

So, what's in the extension data? pax calls it an *extended header*, and it's an entirely different encoding format. An *extended header* contains one or more pax "records," and each record is a string that looks like this:

```
<length> <keyword>=<value>\n
```

where:

- `<length>` is the byte-length of the record itself, *including* `<length>` itself[^12];
- `<keyword>` is a keyword that sets or overrides some pax parsing state;
- `<value>` is the value to apply to `<keyword>`.

This can be hard to visualize, so here's a concrete example:

```
13 size=8000↵
```

"↵" above indicates the linefeed.

Despite being a new encoding, the way that pax records work is *relatively* simple: if there is an *effective* pax record that corresponds to a tar member header's state, the pax record's value takes precedence. In the example above, this means that the subsequent member's size is `8000` bytes, even if the member's own header records a different size.

This is where the difference between global and local extensions comes in as well: global extended headers take precedence over *all* subsequent members in the archive, while local extended headers only affect the *next* file in the archive. Local extended headers also take precedence over any global headers, so we have the following precedence ordering:

(This, too, is simplified. More on that later.)

To visualize all of that:

For this example, a pax-conforming tar implementation should extract two members:

- `hasdrubal.txt`, with a payload of `10737418240` bytes and an `mtime` of `100`.

  Observe that this is both greater than the UStar size limit of 8 GiB and does *not* use the UStar header's own `name` (of `hannibal.txt`) or `size` (of `0`), since the member's header is preceded by an `x` extended header that overrides those values[^13].

  Moreover, the archive *also* contains a `g` extended header that overrides `mtime` for all subsequent members and `x` does not contain its own `mtime`, so the global extended header's `mtime=100` also takes effect.

- `hamilcar.txt`, with a payload of `1234` bytes and an `mtime` of `100`.

  Observe that, unlike `hasdrubal.txt`, there is no `x` extended header for this member. As a result, the UStar member states are used, with the exception of those overridden by the effective `g` extended header. Thus `hamilcar.txt` ends up with `mtime=100`, even if its UStar header sets a different value.

### GNU tar

GNU tar takes a very different approach. First, here's the GNU tar header:

```c
struct gnu_header {
    // Unchanged from UStar.
	char name[100];
	char mode[8];

    // Same layout as UStar, but potentially base-256.
	char uid[8];
	char gid[8];
	char size[12];
	char mtime[12];
	char checksum[8];

    // Same layout as UStar, but additional typeflags.
	char typeflag[1];

    // Unchanged.
	char linkname[100];

    // `"ustar "`
	char magic[6];

    // `" \0"`
	char version[2];

    // Unchanged from UStar.
	char uname[32];
	char gname[32];
	char devmajor[8];
	char devminor[8];

    // GNU extensions.
	char atime[12];
	char ctime[12];
	char offset[12];
	char longnames[4];
	char unused[1];
	struct {
		char offset[12];
		char numbytes[12];
	} sparse[4];
	char isextended[1];
	char realsize[12];

    // 17 remaining bytes of the header are unused.
	char pad[17];
};
```

Observe that, unlike the UStar format, there is no `char prefix[155]` field. Furthermore, instead of pax extended headers with a fully general keyword/value format, GNU tar does two things:

- It goes *way* harder than UStar on trying to re-use the existing 512-byte V7 member header. In particular, GNU tar adds its own base-256 encoding: if the first byte in a numeric field has its high bit set, then the remaining octets in that field are binary, rather than tar's normal octal form.

  This effectively expands the per-byte entropy in numeric fields from 3 bits to 8 bits. For a `char size[12]`, that makes the effective size limit `2**80`[^14] or one yobibyte.

- Instead of general `x` and `g` extended headers, GNU tar introduces a bunch of *specialized* typeflags for its own extensions.

  [Many of these](https://www.gnu.org/software/tar/manual/html_node/Standard.html) are not relevant to the post, or even to modern tar archives at all. But two are:

  - The `L` typeflag identifies the next sequence of content blocks as containing a filename.
  - The `K` typeflag identifies the next sequence of content blocks as containing a link name.

  These two typeflags effectively fill the role of the pax `path` and `linkpath` records, respectively.

To visualize that:

For this example, a GNU tar implementation should extract two members:

- `hasdrubal.txt` with a payload of `10737418240` bytes.

  Observe that this size, which exceeds 8 GiB, was encoded directly in the header. However, the effective filename of `hasdrubal.txt` comes from overriding the header's original filename of `hannibal.txt` via the preceding GNU `L` extension.

- `publius.txt`, which links to another file named `africanus.txt`.

  Observe that `publius.txt` originally declared `publius.txt -> scipio.txt` as its link, but this gets overridden to `africanus.txt` by the preceding GNU `K` extension.

Finally, while GNU tar does not have pax's notion of global vs. local precedence, GNU tar extensions can *stack*. For example, to override a member's `name` and `linkname` at the same time, you'd have a tar header sequence like this[^15]:

## tar-codec

### State machine [turducken](https://en.wikipedia.org/wiki/Turducken)

Now that we understand *very roughly* how the various tar family formats look and work, let's see what [tar-codec](https://github.com/astral-sh/astral-codec/tree/main/crates/tar-codec) has to do to parse them safely and efficiently.

First we'll revisit the state machine. Our nice and simple V7 state machine now understands the different pax and GNU states, and has become this monstrosity:

(Note that the error transitions in this state machine are dramatically simplified: in practice, any I/O or validation failure at any point also results in a `Failed` state.)

What in the world happened? A few different things:

- Our state machine begins with a "classification" stage, which effectively locks the remainder of the parse to a single family (either UStar/pax or GNU[^16]) based on the identity (magic and version) of the first block.

  This is done for differential reduction reasons: while researching [tar-codec](https://github.com/astral-sh/astral-codec/tree/main/crates/tar-codec)'s design, we realized that many other widely used tar parsers attempt to merge states across various tar families, with inconsistent results.

  For example, with some tar parsers, it's possible to stack headers like so:

  This has unclear semantics: GNU tar expects an `L` or `K` to apply to the next "file" member, but the next physical member in the stream is a pax local extension that *also* wants to modify the next "file" member. In practice we've seen other parsers do all kinds of things in this state, including attempting to merge the extensions, honoring one over the other(s), attempting to apply the `L` state to `x` instead of the file member, and so forth. Each of these is a potentially exploitable differential!

  Instead, [tar-codec](https://github.com/astral-sh/astral-codec/tree/main/crates/tar-codec) will reject a tar archive if its individual headers don't agree about what family of tar it is. Real-world tar encoders don't mix tar families (there's no good reason to), so this doesn't pose a significant breakage risk.

- The GNU part of the state machine is slightly more strict than the GNU documentation implies: the GNU documentation allows stacking of `L`/`K` extensions, including *multiple of the same kind*, in any order.

  [tar-codec](https://github.com/astral-sh/astral-codec/tree/main/crates/tar-codec) tightens that to allowing `L` and `K` in any order, but only one of each per underlying member.

- The UStar/pax part of the state machine models local and global extended headers rigidly.

  There's a significant amount of complexity beneath this: pax allows `x` and `g` *anywhere* in the tar stream. For example, this is a perfectly valid extension sequence in pax:

  This brings us to one of the most painful parts of pax that we glossed over before: extended headers don't *shadow* each other fully, they *compose* depending on what state the "stack" of effective pax records is in. The state of any given pax record can be:

  - *Undeclared*: no effective pax record exists anywhere in the current stack of local and global extensions, which means that the member's own UStar header's state takes precedence.

  - *Active*: there's an effective pax record in the stack, *and* it has a value.

  - *Tombstoned*: there's an effective pax record in the stack, but it has no value (for example, `8 path=\n` instead of `12 path=abc\n`).

    You would think that this would be the same as the *Undeclared* state, but it's not! pax considers this record **explicitly** deleted, and [prescribes](https://pubs.opengroup.org/onlinepubs/9799919799/utilities/pax.html#:~:text=If%20the%20%3Cvalue%3E%20field%20is%20zero%20length%2C%20it%20shall%20delete%20any%20header%20block%20field%2C%20previously%20entered%20extended%20header%20value%2C%20or%20global%20extended%20header%20value%20of%20the%20same%20name.):

    > If the `<value>` field is zero length, it shall delete any header block field, previously entered extended header value, or global extended header value of the same name.

    This has fascinating consequences for the state machine, because it means that an effective-but-deleted `path` record means...a file with no path? How are you supposed to extract that[^17]?

  Needless to say, [tar-codec](https://github.com/astral-sh/astral-codec/tree/main/crates/tar-codec) tries to follow this faithfully. In particular, it attempts to be diligent about rejecting logically-consistent-but-nonsense states that fall out of pax's strange record state machine, including empty paths and global extended headers that try to declare `size` and other key states for all members.

### From parsing to extraction

One of the early design choices we made with [tar-codec](https://github.com/astral-sh/astral-codec/tree/main/crates/tar-codec) was to isolate the implementation into a few layers. From lowest to highest:

- The [tar-framing](https://crates.io/crates/tar-framing) crate is responsible for taking a stream of blocks and turning it into a stream of frames. This happens in two refinement stages:

  - The stream or "physical" API honors the big state machine above and yields each block in a given state, one block at a time.

  - The "logical" API consumes the blocks produced by the "physical" API, and yields them as a stream of assembled members. Each assembled member is essentially its "header" (which is the physical header, merged with any effective extension state that affected the physical framing), a separate extension state for non-framing extensions (such as GNU `L` and `K`), and a readable payload for the member's actual contents.
- The [tar-codec](https://crates.io/crates/tar-codec) crate takes the "logical" API from [tar-framing](https://crates.io/crates/tar-framing) and turns it into something that can be extracted in memory or onto disk.

Extracting an archive (of any format) onto disk is its own can of worms, with its own differential risks[^18]. Another one of our early observations is that every archive implementation tends to have its own extract-to-disk implementation, with its own edge cases.

We built [archive-trait](https://crates.io/crates/archive-trait) to address that. [archive-trait](https://crates.io/crates/archive-trait) exposes `Archive` and `ArchiveBuilder` traits (along with associated types and machinery) that allow any format to expose uniform APIs for reading and creating archives. We then built [archive-trait](https://crates.io/crates/archive-trait)'s own filesystem extraction on top of [cap-std](https://crates.io/crates/cap-std), giving us the ability to "anchor" extraction into a single directory and avoid a large class of link and path traversal footguns.

## Lessons learned

- Writing a differential-resistant tar parser is *surprisingly* hard, given how ubiquitous tar files of all flavors are, and how simple the original (V7) format is.

  This post has only scratched the surface in terms of complexity. To recap:

  - Each tar family has its own state machine, which is not always clearly laid out in that family's documentation or reference implementation. It is *imperative* that these state machines do not mix, but real-world tar parsers regularly allow them to.

  - Each tar family has its own, non-trivial rules for determining the precedence of state (pathname, linkname, and size) attached to archive members.

    Some of this state affects the framing of the parser itself, meaning that faithfully handling e.g. pax archives means handling `size` and similar records up-front, rather than trying to abstract the parse across individual 512-byte header blocks.

  Separately, there were numerous places where we had to compromise on strict pax conformance because of observed behavior from other implementations:

  - The pax spec is explicit that pax record values must be UTF-8. However, both GNU tar and `star` historically emitted some vendor-specific records[^19] as raw binary. See [this bug-tar thread for more context](https://lists.gnu.org/r/bug-tar/2013-12/msg00012.html).

  - UStar and pax are explicit that `uname` and `gname` should be NUL-terminated in the UStar header, meaning that each can be at most 31 bytes long. However, CPython's `tarfile` (and probably others) will happily fill either to 32 bytes, only overflowing to the pax record at 33 bytes or more. See [CPython#159322](https://github.com/python/cpython/issues/159322) for more context.

  - More generally, tar implementations will frequently leave `mode`, `uid`, `gid`, and `mtime` as all NUL, rather than zeroed or similar. This doesn't pose a framing/parser synchronization risk, so [tar-codec](https://github.com/astral-sh/astral-codec/tree/main/crates/tar-codec) tracks them as "missing" rather than assigning an arbitrary default value.

  Finally, in the process of designing and implementing tar-codec, we found *many* parser differential primitives that cause various tar implementations to behave *very* differently on identical inputs. Some of these are currently being disclosed as security vulnerabilities. However, some of the general classes we saw:

  - pax/GNU state machine confusion, as mentioned above. In particular, some tar implementations can be fooled into applying a pax extension's states to a GNU extension (or vice versa) instead of to the subsequent member, often resulting in full desynchronization and different extraction results.

  - Mis-handling of pax extension precedence, in particular both global (`g`) extended headers and record "tombstoning." For example, some implementations will accept a `g` header with `size=1234` but ignore it, despite pax saying clearly that such a `size` record overrides any UStar `size` fields. This also typically results in a full desynchronization primitive.

  - Mis-handling of pax record parsing. For example, some implementations will ignore the `<length>` prefix and instead assume that pax records can be safely split on `\n`. This is not true, since the `<value>` of a pax record can contain a verbatim `\n`. As you may have guessed, this also typically results in a full desynchronization primitive.

  - More generally, we found a great deal of variety in how lax various tar implementations are when it comes to parsing integer fields: many use their language's native "integer from string" API, which can [behave surprisingly](https://yossarian.net/til/post/some-surprising-numeric-conversions-in-python/) in the presence of different (non-Western Arabic) numeral forms, leading and trailing whitespace, and so forth. These small variations can percolate into two different implementations parsing the same `size` field as different integers, once again resulting in desynchronization.
- Decode and encode performance was *not* the hard part: we found a lot of low-hanging fruit while optimizing both, including optimizing tar's checksum generation and calculating pax record length prefixes.

  We also picked up significant performance gains by ensuring that tar-codec reuses allocations and batches/buffers its work wherever possible (and where doing so does not weaken the API's internal consistency or cancellation correctness in an `async` setting).

- Conversely, *performant* secure extraction *is* somewhat hard: directory capability tracking (of the kind that [cap-std](https://crates.io/crates/cap-std) does) adds measurable overhead to many of our filesystem operations, particularly when extracting on hosts that lack [`openat2(2)`](https://man7.org/linux/man-pages/man2/openat2.2.html) or an equivalent system call[^20]. This negatively impacts extraction on macOS in particular.

All in all, [tar-codec](https://github.com/astral-sh/astral-codec/tree/main/crates/tar-codec)'s design rests on well-worn principles for secure parser design: we are [conservative rather than liberal](https://alexgaynor.net/2025/mar/25/postels-law-and-the-three-ring-circus/) in what we accept, and we enforce correctness by construction through increasingly constrictive layers (from member framing to streaming to extraction). These design choices have made us confident that [tar-codec](https://github.com/astral-sh/astral-codec/tree/main/crates/tar-codec) is more resistant to differentials (by correctly rejecting them) than other tar implementations.

The fractured and under-specified nature of tar makes it impossible for us to say that we are *entirely* devoid of differentials, or that new differentials won't be discovered in the future. However, we believe that [tar-codec](https://github.com/astral-sh/astral-codec/tree/main/crates/tar-codec)'s primitives are the correct ones if and when those contingencies occur.

## Looking forwards

This is not the end of our work, on several levels:

- While [tar-codec](https://github.com/astral-sh/astral-codec/tree/main/crates/tar-codec) is essentially feature-complete for our own purposes (within [uv](https://github.com/astral-sh/uv)), we're interested in making sure that it also solves problems that the larger Open Source community has around secure and performant tar parsing.

  To that end, we'll be looking at supporting other, more niche aspects of the various tar families (like sparse files, and more control over vendor-specific pax records) as the need arises.

  Separately, we'll be working with others in the community on improving the correctness of *other* tar implementations. In particular, we see Harmen Stoppels's nascent [tar-minefield](https://stoppels.ch/tar/) project as a promising way to test and re-align other implementations with stricter, more differential-resistant interpretations of the tar family formats.

- Separately, as you may have guessed from our decision to build a generic [archive-trait](https://crates.io/crates/archive-trait) library: tar is not the only format we're thinking about here!

  We're also actively looking into replacing uv's ZIP parsing, for similar reasons. We hope to share our lessons from that work soon, along with material benefits to uv itself.

[^1]: [tar-codec](https://github.com/astral-sh/astral-codec/tree/main/crates/tar-codec) has been available since 0.12, but was disabled by default behind the `--preview-features tar-codec` flag. As of 0.13, it is the default and can be disabled by setting `UV_LEGACY_TAR_BACKEND=1`.

[^2]: The term "built" is the source of a great deal of confusion for newcomers when they first encounter Python packaging. In the context of Python packaging, "built" refers to the fact that the distribution is produced by a "build backend", i.e. contains structured metadata that a source distribution is not guaranteed to have.

    It does **not** imply that the wheel contains shared libraries or other native code, although it can. Pure Python packages are also regularly distributed as built distributions, because installing them is faster and more reliable.

[^3]: Historically, a source distribution was *literally* just a source archive, with the convention of having a `pyproject.toml` or `setup.py` at the archive's root. More recent Python packaging standards state that source distributions must contain a `PKG-INFO` file containing standard metadata. See [PEP 643](https://peps.python.org/pep-0643/) for more on that.

    As a result, many source distributions contain no metadata other than the build system itself, which must be executed to access the distribution's metadata.

[^4]: We recommend Edera's "[TARmageddon](https://edera.dev/stories/tarmageddon)" blog post for a detailed treatment of one such differential.

[^5]: [Martin Dørum](https://mort.coffee) has a [great blog post](https://mort.coffee/home/tar/) on the original tar format and how it evolved over time into the constellation of variants that currently exist. We highly recommend reading his post.

[^6]: Whether or not UStar and pax should be clumped together in the same family is doubtless extremely controversial to the dozen-or-so other people on Earth who have thought about the tar format this much. I consider them to be in the same family, since pax is *effectively* a strict superset of UStar and comes from the same standards body (POSIX) that produced UStar.

[^7]: There are actually two different GNU tar formats: there's `oldgnu` i.e. "old GNU" and `gnu` i.e. "new GNU". The difference between these two is minor (relative to everything else) and GNU tar itself emits "new GNU" as of tar 1.12, which was released in 1997. So that's enough about "old GNU."

[^8]: "tar" of course being short for "Tape ARchive." Magnetic tapes (like all mass storage) store their contents in sequences of fixed-sized blocks, and tar seals 1979's glorious 512-byte blocks in amber.

    tar is also far from the only block-style file format. [cpio](https://en.wikipedia.org/wiki/Cpio) is another well-known one.

[^9]: Archive members are things like files and directories, but *also* things like sockets and device files. I'm not really clear on what it means to archive a socket, but both UStar/pax and GNU tar will happily let you express that you've done so!

[^10]: Before compression, of course. tar knows nothing about compression. It's just the format that encodes one or more files into a single file.

[^11]: V7 tar handles directories in two ways: it creates directories that are *implied* to exist (for example, a member whose name is `foo/makeme/bar` will create `foo` and `foo/makeme` if needed), and it will explicitly create a directory if a regular file member ends with a slash, e.g. `foo/makeme/`.

[^12]: In other words, it's a self-describing length. This is somewhat unusual when compared to a "normal" TLV/LTV encoding, where the length component is typically either a fixed length or something with only limited length variation, like [LEB128](https://en.wikipedia.org/wiki/LEB128).

[^13]: You might be thinking: why bother putting states in the member's UStar header at all, if they're just going to be overridden by the pax extended header? Why not just fill those parts of the block with NULs?

    The answer has to do with backwards compatibility: both GNU and pax do a disgusting hack interesting trick where the extension/extended header's *own* name field is also a valid path, conventionally a specially formatted one like `%d/PaxHeaders.%p/%f`, where `%d` is the dirname of the pax `path` record, `%p` is the current PID, and `%f` is the basename of the same `path` record.

    The theory behind this is (probably) that a subsequent tool can repair the extraction, by looking at the pax headers which have been extracted as a regular file instead. In practice I suspect this doesn't work well in the greater-than-8 GiB case, which is one of the cases where it matters the most.

[^14]: This is `2**80` instead of `2**96` because we lose `size[0]` to the base-256 sign bit and `size[11]` to the NUL byte. Implementations that don't require a NUL terminator for fields like `size` could in principle allow up to `2**88` instead.

[^15]: The order of GNU extensions before the member they affect does not appear to matter in any implementation. In other words, you can have `L -> K -> 1` or `K -> L -> 1`.

[^16]: That's correct: we don't support parsing V7-style tar files. An early review revealed that they're nonexistent in the Python packaging ecosystem, probably because Python's own `tarfile` never supported creating them. The gap between V7 and UStar is small, however, so adding it would not be difficult *if* needed.

[^17]: Many tar implementations do random things here, like trying to extract to an empty path (which of course fails), or picking *some* previous filename or extended header state.

[^18]: Extraction differentials themselves are out of this post's scope, but to summarize: extraction depends heavily on the target filesystem and its state, including any pre-existing symbolic and hard links.

    Nuances abound, including how to handle files that differ in casing inside the archive but not on the filesystem itself (macOS and Windows), how to handle symlinking on systems where symbolic links are privileged (Windows), and how to handle "funky but valid" filenames (non-UTF-8, UTF-8 with NULs, control characters, &c.).

[^19]: A pax "vendor record" is a normal pax record, but with a name like `VENDOR.keyword` instead of `keyword`. pax itself does not specify their semantics and various implementations do wildly different things with them, so tar-codec rejects them by default (with an option to skip/ignore them instead). `SCHILY.xattr.*` is one common example of a vendor record namespace. GNU also has their own vendor namespace, e.g. `GNU.sparse.*` for sparse members.

[^20]: Without `openat2(2)` or a similar source of `RESOLVE_BENEATH` semantics, `cap-std` needs to open each directory component in a path, which means that its system calls scale linearly with the depth of the paths it needs to track. This is on top of the system call overhead for the extraction I/O itself.
