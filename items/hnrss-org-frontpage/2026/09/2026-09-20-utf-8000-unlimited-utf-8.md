---
title: 'UTF-8000: Unlimited UTF-8'
link: https://utf-8000.jb2170.com/
source: hnrss-org-frontpage
published: 2026-09-20T05:15:00Z
updated: 2026-09-20T05:15:00Z
first_seen: 2026-09-20T17:28:39.170422635Z
authors:
- vismit2000
summary: 'Article URL: https://utf-8000.jb2170.com Comments URL: https://news.ycombinator.com/item?id=49772677 Points: 106 # Comments: 77'
content: extracted
html: 2026-09-20-utf-8000-unlimited-utf-8.html
preview:
  file: 2026-09-20-utf-8000-unlimited-utf-8.preview-c9b1d330040c.webp
  width: 256
  height: 134
  alt: preview
  color: '#f7e4c5'
images:
- source: https://utf-8000.jb2170.com/og-image.png
  original:
    file: 2026-09-20-utf-8000-unlimited-utf-8.image-b7cbdf606697.png
    width: 1200
    height: 630
  variants:
  - file: 2026-09-20-utf-8000-unlimited-utf-8.image-bf12db913f2b.webp
    width: 320
    height: 168
  - file: 2026-09-20-utf-8000-unlimited-utf-8.image-e01d7686e861.webp
    width: 640
    height: 336
  - file: 2026-09-20-utf-8000-unlimited-utf-8.image-ae9f929a5a28.webp
    width: 1200
    height: 630
  color: '#fee7c8'
- source: https://utf-8000.jb2170.com/code-unit-anatomy.blanchedalmond.png
  original:
    file: 2026-09-20-utf-8000-unlimited-utf-8.image-bb7ec7558cca.png
    width: 960
    height: 320
  color: '#feeacc'
- source: https://utf-8000.jb2170.com/overlong-window.gif
  original:
    file: 2026-09-20-utf-8000-unlimited-utf-8.image-e2397a318acb.gif
    width: 800
    height: 320
  color: '#feeacc'
- source: https://utf-8000.jb2170.com/utf-8000-info-example.png
  original:
    file: 2026-09-20-utf-8000-unlimited-utf-8.image-fe88b86ee9d7.png
    width: 785
    height: 174
  color: '#f6f6f6'
- source: https://utf-8000.jb2170.com/logo.blanchedalmond.png
  original:
    file: 2026-09-20-utf-8000-unlimited-utf-8.image-7c5f063ba514.png
    width: 1280
    height: 1280
  variants:
  - file: 2026-09-20-utf-8000-unlimited-utf-8.image-590e878107a6.webp
    width: 320
    height: 320
  - file: 2026-09-20-utf-8000-unlimited-utf-8.image-d9e7401fc1c3.webp
    width: 640
    height: 640
  - file: 2026-09-20-utf-8000-unlimited-utf-8.image-8c297ec9180c.webp
    width: 1280
    height: 1280
  color: '#feeacc'
html_truncated: true
---

Unlimited UTF-8! ASCII ⊆ UTF-8 ⊆ UTF-8000.

No special cases introduced. All [properties](https://utf-8000.jb2170.com/#sec-properties) preserved.

Try out the [reference implementation](https://utf-8000.jb2170.com/#sec-reference-implementation) with `$ pipx install UTF-8000`.

UTF-8000 is in no way endorsed by or representative of the [Unicode Consortium](https://home.unicode.org/). \
 This is a fun standalone project / proposal.

TLDR / Examples

|          |               |              |              |              |              |             |             |             |             |             |             |
| -------- | ------------- | ------------ | ------------ | ------------ | ------------ | ----------- | ----------- | ----------- | ----------- | ----------- | ----------- |
| ASCII    |               |              |              |              |              |             |             |             |             |             |             |
| 1        | `0 xxxxxxx`   |              |              |              |              |             |             |             |             |             |             |
| UTF-8    |               |              |              |              |              |             |             |             |             |             |             |
| 2        | `11 0 xxxx x` | `10 xxxxxx`  |              |              |              |             |             |             |             |             |             |
| 3        | `11 10 xxxx`  | `10 x xxxxx` | `10 xxxxxx`  |              |              |             |             |             |             |             |             |
| 4        | `11 110 xxx`  | `10 xx xxxx` | `10 xxxxxx`  | `10 xxxxxx`  |              |             |             |             |             |             |             |
| UTF-8000 |               |              |              |              |              |             |             |             |             |             |             |
| 5        | `11 1110 xx`  | `10 xxx xxx` | `10 xxxxxx`  | `10 xxxxxx`  | `10 xxxxxx`  |             |             |             |             |             |             |
| 6        | `11 11110 x`  | `10 xxxx xx` | `10 xxxxxx`  | `10 xxxxxx`  | `10 xxxxxx`  | `10 xxxxxx` |             |             |             |             |             |
| 7        | `11 111110`   | `10 xxxxx x` | `10 xxxxxx`  | `10 xxxxxx`  | `10 xxxxxx`  | `10 xxxxxx` | `10 xxxxxx` |             |             |             |             |
| 8        | `11 111111`   | `10 0 xxxxx` | `10 xxxxxx`  | `10 xxxxxx`  | `10 xxxxxx`  | `10 xxxxxx` | `10 xxxxxx` | `10 xxxxxx` |             |             |             |
| 9        | `11 111111`   | `10 10 xxxx` | `10 x xxxxx` | `10 xxxxxx`  | `10 xxxxxx`  | `10 xxxxxx` | `10 xxxxxx` | `10 xxxxxx` | `10 xxxxxx` |             |             |
| 10       | `11 111111`   | `10 110 xxx` | `10 xx xxxx` | `10 xxxxxx`  | `10 xxxxxx`  | `10 xxxxxx` | `10 xxxxxx` | `10 xxxxxx` | `10 xxxxxx` | `10 xxxxxx` |             |
| ...      |               |              |              |              |              |             |             |             |             |             |             |
| 22       | `11 111111`   | `10 111111`  | `10 111111`  | `10 110 xxx` | `10 xx xxxx` | `10 xxxxxx` | `10 xxxxxx` | `10 xxxxxx` | `10 xxxxxx` | ...         | `10 xxxxxx` |
| ...      |               |              |              |              |              |             |             |             |             |             |             |

There is nothing special-case-y about the example 22-byte code unit here. It is just a good prototypical example, demonstrating the power of UTF-8000 with multiple start bytes.

There are only two special cases, both of which are inherited from UTF-8: ASCII as is, and 2-byte UTF-8 having 4 mandatory content bits to check against overlong encoding as opposed to 5 for all longer length code units.

Anatomy

Here is anatomical diagram of the example 22-byte code unit from the [tldr](https://utf-8000.jb2170.com/#sec-tldr).

See the [glossary](https://utf-8000.jb2170.com/#sec-glossary) for more information on the definitions of the terms.

Byte number four is exciting! It is a continuation byte, a start byte, the final start byte, has content bits, and has only some of the mandatory content bits, which are straddled across the final start byte and first non-start byte.

![](https://utf-8000.jb2170.com/code-unit-anatomy.blanchedalmond.png)

The main contribution of UTF-8000's specification is clarity on splitting the highest bits of the first byte of UTF-8 code units into self-synchronization bits and start bits, and then making it clear how to stripe the start bits across the continuation bytes if needed, to achieve arbitrarily large code units.

Glossary

These terms are ordered somewhat by chronology of first requirement, rather than alphabetically, for convenience.

Terms used within definitions are [underlined](https://utf-8000.jb2170.com/#sec-glossary) clickable hyperlinks.

| Term                        | Definition                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| --------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Codepoint                   | A non-negative integer, aka an unsigned integer.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| Code Unit                   | A sequence of UTF-8000 bytes that encode a single [codepoint](https://utf-8000.jb2170.com/#def-codepoint).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| First Byte                  | The first, one and only, byte that begins a UTF-8000 [code unit](https://utf-8000.jb2170.com/#def-code-unit).  The [self-synchronization prefix](https://utf-8000.jb2170.com/#def-self-synchronization-prefix) of a [first byte](https://utf-8000.jb2170.com/#def-first-byte) is either `0` for ASCII or `11` for multi-byte [code units](https://utf-8000.jb2170.com/#def-code-unit).   This term is **not** synonymous with [start byte](https://utf-8000.jb2170.com/#def-start-byte). A [first byte](https://utf-8000.jb2170.com/#def-first-byte) is necessarily a [start byte](https://utf-8000.jb2170.com/#def-start-byte), but not the other way around. It is for this reason that [first byte](https://utf-8000.jb2170.com/#def-first-byte) is sometimes also known as [first start byte](https://utf-8000.jb2170.com/#def-first-byte).  Fun observation: because of the [self-synchronization prefix](https://utf-8000.jb2170.com/#def-self-synchronization-prefix) `0` the upper hex nibble of ASCII bytes can only be one of `0`, `1`, `2`, `3`, `4`, `5`, `6`, `7`.   This term is mutually exclusive with [continuation byte](https://utf-8000.jb2170.com/#def-continuation-byte) due to [self-synchronization](https://utf-8000.jb2170.com/#def-self-synchronization-prefix).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| Continuation Byte           | A byte beyond the [first byte](https://utf-8000.jb2170.com/#def-first-byte) of a multi-byte UTF-8000 [code unit](https://utf-8000.jb2170.com/#def-code-unit).  The [self-synchronization prefix](https://utf-8000.jb2170.com/#def-self-synchronization-prefix) of a [continuation byte](https://utf-8000.jb2170.com/#def-continuation-byte) is `10`, which is also known as the [continuation prefix bits](https://utf-8000.jb2170.com/#def-continuation-byte).   Fun observation: because of the [self-synchronization prefix](https://utf-8000.jb2170.com/#def-self-synchronization-prefix) `10` the upper hex nibble of [continuation bytes](https://utf-8000.jb2170.com/#def-continuation-byte) can only be one of `8`, `9`, `A`, `B`.   This term is mutually exclusive with [first byte](https://utf-8000.jb2170.com/#def-first-byte) due to [self-synchronization](https://utf-8000.jb2170.com/#def-self-synchronization-prefix).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| Self-Synchronization Prefix | The highest bits of every UTF-8000 byte that indicate whether it is a [first byte](https://utf-8000.jb2170.com/#def-first-byte) or a [continuation byte](https://utf-8000.jb2170.com/#def-continuation-byte).  The possible [self-synchronization prefixes](https://utf-8000.jb2170.com/#def-self-synchronization-prefix) form a prefix-free tree:  ```    .----0              First byte for ASCII   `----1---0 Continuation byte for multi-byte UTF-8000         `----1        First byte for multi-byte UTF-8000                        ```  This piece of the clever architecture of UTF-8, which UTF-8000 inherits, provides the property of [self-synchronization](https://utf-8000.jb2170.com/#def-self-synchronization-prefix) at a byte level: we can instantaneously tell what kind of byte we are looking at, and where it should belong in a code unit, just by looking at these highest bits.   This is most useful when decoding part of a file encoded in UTF-8000. If we randomly seek through the file to an arbitrary byte, we can unambiguously tell whether we are at a [first byte](https://utf-8000.jb2170.com/#def-first-byte) whence we can begin decoding a new [code unit](https://utf-8000.jb2170.com/#def-code-unit) immediately, or that we are at a [continuation byte](https://utf-8000.jb2170.com/#def-continuation-byte) whence we need to seek a little further on in order to find the next [first byte](https://utf-8000.jb2170.com/#def-first-byte) in order to begin decoding. Nor do we have to process any bytes prior to our seek position in order to discover some global state or the context of the byte we have seek-ed to; a [first byte](https://utf-8000.jb2170.com/#def-first-byte) is always unambiguously a [first byte](https://utf-8000.jb2170.com/#def-first-byte) wherever it appears, which we can deduce by its [self-synchronization prefix](https://utf-8000.jb2170.com/#def-self-synchronization-prefix) being either `0` or `11`.   This is useful not only for random access, but also for error recovery. Suppose that we are decoding an error-prone stream of UTF-8000 bytes and that whenever when we encounter an error (e.g. a rogue 0xC0 byte) we wish to keep calm and carry on instead of immediately exiting. We can yield Unicode replacement characters `U+FFFD` � and then await the next [first byte](https://utf-8000.jb2170.com/#def-first-byte), discarding anything in the interim.  See the Wikipedia article for [self-synchronizing code](https://en.wikipedia.org/wiki/Self-synchronizing_code) for more general info.   These bits are highlighted in bright cyan.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| Start Byte                  | A byte containing one or more [start bits](https://utf-8000.jb2170.com/#def-start-bits). The [start bytes](https://utf-8000.jb2170.com/#def-start-byte) exist contiguously at the beginning of a UTF-8000 [code unit](https://utf-8000.jb2170.com/#def-code-unit). The power of UTF-8000 is that we can have multiple [start bytes](https://utf-8000.jb2170.com/#def-start-byte), to achieve arbitrary [code unit](https://utf-8000.jb2170.com/#def-code-unit) lengths, to encode arbitrarily large [codepoints](https://utf-8000.jb2170.com/#def-codepoint).  Sometimes it is sensible to colloquially also include ASCII as a [start byte](https://utf-8000.jb2170.com/#def-start-byte) when we are talking about the bytes towards the start of a code unit, even though ASCII bytes have no [start bits](https://utf-8000.jb2170.com/#def-start-bits).  Every non-ASCII [code unit](https://utf-8000.jb2170.com/#def-code-unit) has at least one [start byte](https://utf-8000.jb2170.com/#def-start-byte). The first [start byte](https://utf-8000.jb2170.com/#def-start-byte) is the [first byte](https://utf-8000.jb2170.com/#def-first-byte), and it is followed by zero or more [continuation bytes](https://utf-8000.jb2170.com/#def-continuation-byte) that are also [start bytes](https://utf-8000.jb2170.com/#def-start-byte). Therefore because a UTF-8000 [code unit](https://utf-8000.jb2170.com/#def-code-unit) can have multiple [start bytes](https://utf-8000.jb2170.com/#def-start-byte), this term is **not** synonymous with [first byte](https://utf-8000.jb2170.com/#def-first-byte).  In restricting to only UTF-8 without UTF-8000, this term *is* synonymous with [first byte](https://utf-8000.jb2170.com/#def-first-byte). This is because UTF-8-length [code units](https://utf-8000.jb2170.com/#def-code-unit) only require one [start byte](https://utf-8000.jb2170.com/#def-start-byte), whether using up to 4 bytes in the current UTF-8 standard ([RFC 3629](https://datatracker.ietf.org/doc/html/rfc3629) (2003)), or using up to 6 bytes in former standards ([RFC 2044](https://datatracker.ietf.org/doc/html/rfc2044) (1996) and [RFC 2279](https://datatracker.ietf.org/doc/html/rfc2279) (1998)).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| Start Bits                  | The unary-code sequence of bits contained in the [start bytes](https://utf-8000.jb2170.com/#def-start-byte) of a multi-byte UTF-8000 [code unit](https://utf-8000.jb2170.com/#def-code-unit) that tells us the length of the [code unit](https://utf-8000.jb2170.com/#def-code-unit) in bytes.  For a [code unit](https://utf-8000.jb2170.com/#def-code-unit) made of `n` bytes the [start bits](https://utf-8000.jb2170.com/#def-start-bits) are `n-2` `1` bits followed by a terminating `0` bit. To be clear, the [start bits](https://utf-8000.jb2170.com/#def-start-bits) include this terminating zero bit. Thus the [start bits](https://utf-8000.jb2170.com/#def-start-bits) sequence is of length `n-1` and looks like `111...10`.   The possible [start bits](https://utf-8000.jb2170.com/#def-start-bits) sequences form a prefix-free tree:  ```    .----0                    Two byte UTF-8   `----1---0            Three byte UTF-8         `----1---0       Four byte UTF-8               `----1---0 Five byte UTF-8000                     `----...    n byte UTF-8000                        ```  For an `n` byte [code unit](https://utf-8000.jb2170.com/#def-code-unit) where `n < 8` the [start bits](https://utf-8000.jb2170.com/#def-start-bits) all fit together snugly in the [first byte](https://utf-8000.jb2170.com/#def-first-byte). Otherwise they are striped across as many of the first few bytes as they need, filling the free bits that are not occupied by [continuation prefix bits](https://utf-8000.jb2170.com/#def-continuation-byte).  This is another piece of the clever architecture of UTF-8, which UTF-8000 inherits, that provides the property of [self-punctuation](https://utf-8000.jb2170.com/#def-start-bits) also known as a prefix code or a prefix-free code: when decoding a multi-byte [code unit](https://utf-8000.jb2170.com/#def-code-unit), once we have read to the end of the [start bytes](https://utf-8000.jb2170.com/#def-start-byte), that is we have encountered the terminating `0` bit, we know exactly how many bytes we expect in that [code unit](https://utf-8000.jb2170.com/#def-code-unit). Notwithstanding errors we can therefore succeed in decoding the [code unit](https://utf-8000.jb2170.com/#def-code-unit) by reading exactly that many bytes, and no more.   This avoids a problem of dumber variable-length encodings whose [code units](https://utf-8000.jb2170.com/#def-code-unit) do not intrinsically indicate their length: one has to read beyond the last byte of a [code unit](https://utf-8000.jb2170.com/#def-code-unit), that is one reads the [first byte](https://utf-8000.jb2170.com/#def-first-byte) of the next [code unit](https://utf-8000.jb2170.com/#def-code-unit), in order to know that the current [code unit](https://utf-8000.jb2170.com/#def-code-unit) has finished. For very dumb encodings which have neither [self-synchronization](https://utf-8000.jb2170.com/#def-self-synchronization-prefix) nor [self-punctuation](https://utf-8000.jb2170.com/#def-start-bits), to make random access possible one would have to put dedicated auxiliary bytes, punctuation like a comma byte, between [code units](https://utf-8000.jb2170.com/#def-code-unit) to be able to tell where one ends and another begins.  See the Wikipedia articles for [prefix code](https://en.wikipedia.org/wiki/Prefix_code) and [unary coding](https://en.wikipedia.org/wiki/Unary_coding) for more general info.   This term is mutually exclusive with [content bits](https://utf-8000.jb2170.com/#def-content-bits).  These bits are highlighted in bright magenta. |
| Content Byte                | A byte containing one or more [content bits](https://utf-8000.jb2170.com/#def-content-bits).  A byte being a [content byte](https://utf-8000.jb2170.com/#def-content-byte) does not imply that it is a [continuation byte](https://utf-8000.jb2170.com/#def-continuation-byte). For example a 3-byte [code unit](https://utf-8000.jb2170.com/#def-code-unit) begins with `11 10 xxxx`, which contains 4 [content bits](https://utf-8000.jb2170.com/#def-content-bits) and is not a [continuation byte](https://utf-8000.jb2170.com/#def-continuation-byte).   A byte being a [continuation byte](https://utf-8000.jb2170.com/#def-continuation-byte) does not imply that it is a [content byte](https://utf-8000.jb2170.com/#def-content-byte). For example a 22-byte [code unit](https://utf-8000.jb2170.com/#def-code-unit) contains `10 111111` as its second byte, which is a [continuation byte](https://utf-8000.jb2170.com/#def-continuation-byte) and has no [content bits](https://utf-8000.jb2170.com/#def-content-bits).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| Content Bits                | The sequence of bits in a [code unit](https://utf-8000.jb2170.com/#def-code-unit) beyond the [start bits](https://utf-8000.jb2170.com/#def-start-bits) and to the end of the [code unit](https://utf-8000.jb2170.com/#def-code-unit), in which the [codepoint](https://utf-8000.jb2170.com/#def-codepoint)'s binary bits are stored. For example a 3-byte [code unit](https://utf-8000.jb2170.com/#def-code-unit), which has the form `11 10 xxxx` `10 x xxxxx` `10 xxxxxx`, has 16 [content bits](https://utf-8000.jb2170.com/#def-content-bits).   For ASCII there are 7 [content bits](https://utf-8000.jb2170.com/#def-content-bits). These seven bits `xxxxxxx` combined with a byte's highest bit being set to the [self-synchronization prefix](https://utf-8000.jb2170.com/#def-self-synchronization-prefix) `0` means that ASCII is perfectly included into UTF-8 without being altered. Thus ASCII [code units](https://utf-8000.jb2170.com/#def-code-unit) take the form `0 xxxxxxx`.   Otherwise for an `n` byte [code unit](https://utf-8000.jb2170.com/#def-code-unit), where `n > 1`, there are `5n+1` [content bits](https://utf-8000.jb2170.com/#def-content-bits). This is how we arrive at that formula: We start with `n` blank bytes, each of which has `8` bits. For each byte `2` bits are taken by the [self-synchronization prefix](https://utf-8000.jb2170.com/#def-self-synchronization-prefix). Then an additional `n-1` bits are taken by the [start bits](https://utf-8000.jb2170.com/#def-start-bits). Thus there are `8n - 2n - (n-1) = 5n+1` bits left for [content bits](https://utf-8000.jb2170.com/#def-content-bits). Another way to think about the `5` in this formula is by extending from `n-1` bytes to `n` bytes by appending another [continuation byte](https://utf-8000.jb2170.com/#def-continuation-byte). By doing this we gain `6` free bits in the [continuation byte](https://utf-8000.jb2170.com/#def-continuation-byte), but we lose `1` bit to the longer [start bits](https://utf-8000.jb2170.com/#def-start-bits) sequence, thus overall we gain `6-1 = 5` bits for [content bits](https://utf-8000.jb2170.com/#def-content-bits).  This term is mutually exclusive with [start bits](https://utf-8000.jb2170.com/#def-start-bits).  These bits are highlighted in lime.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| Mandatory Content Byte      | A byte containing one or more [mandatory content bits](https://utf-8000.jb2170.com/#def-mandatory-content-bits).  These are the bytes we check for [overlong encoding](https://utf-8000.jb2170.com/#def-overlong-encoding) when decoding a [code unit](https://utf-8000.jb2170.com/#def-code-unit).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| Mandatory Content Bits      | The first 0, 4, or 5 [content bits](https://utf-8000.jb2170.com/#def-content-bits) of a [code unit](https://utf-8000.jb2170.com/#def-code-unit) in which there must be at least one `1` bit, lest the bytes form an [overlong encoding](https://utf-8000.jb2170.com/#def-overlong-encoding), which is forbidden.   For ASCII there are 0 [mandatory content bits](https://utf-8000.jb2170.com/#def-mandatory-content-bits), and thus no anti-[overlong](https://utf-8000.jb2170.com/#def-overlong-encoding) checking is required. This is because ASCII is the smallest possible code unit.  For 2-byte UTF-8000 there are 4 [mandatory content bits](https://utf-8000.jb2170.com/#def-mandatory-content-bits). This is because in the jump from 1-byte ASCII to 2-byte UTF-8 we jump from 7 [content bits](https://utf-8000.jb2170.com/#def-content-bits) to 11 [content bits](https://utf-8000.jb2170.com/#def-content-bits). Thus the number of [content bits](https://utf-8000.jb2170.com/#def-content-bits) we gain is 11 minus 7 which is 4.  Otherwise for `n` byte UTF-8000, where `n > 2`, there are 5 [mandatory content bits](https://utf-8000.jb2170.com/#def-mandatory-content-bits). This is because in the jump from `n-1` byte UTF-8000 to `n` byte UTF-8000 we add on an extra continuation byte, which has 6 free bits, but we lose 1 bit to the longer [start bits](https://utf-8000.jb2170.com/#def-start-bits) sequence. Thus overall the number of [content bits](https://utf-8000.jb2170.com/#def-content-bits) we gain is 6 minus 1 which is 5.  Read about [overlong encoding](https://utf-8000.jb2170.com/#def-overlong-encoding) for why [mandatory content bits](https://utf-8000.jb2170.com/#def-mandatory-content-bits) are of interest.  These bits are highlighted in bright lime.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| Overlong Encoding           | Forbidden encodings of [codepoints](https://utf-8000.jb2170.com/#def-codepoint) that could be encoded correctly in UTF-8000 using a shorter [code unit](https://utf-8000.jb2170.com/#def-code-unit).  For example one could incorrectly try to encode the [codepoint](https://utf-8000.jb2170.com/#def-codepoint) 0x41, 65, ASCII capital A, using 2-byte UTF-8 as `11 0 0000 1` `10 000001`. Observe that all the [mandatory content bits](https://utf-8000.jb2170.com/#def-mandatory-content-bits) are `0` which is the definition an [overlong encoding](https://utf-8000.jb2170.com/#def-overlong-encoding). This indicates that we could have encoded 0x41 in a shorter [code unit](https://utf-8000.jb2170.com/#def-code-unit), in this case as ASCII `0 1000001`.   Security is one main reason why we forbid [overlong encoding](https://utf-8000.jb2170.com/#def-overlong-encoding). For example we ensure that `11 10 0000` `10 0 00000` `10 000000` cannot be decoded as [codepoint](https://utf-8000.jb2170.com/#def-codepoint) 0, the null byte, lest one speciously pass such an overlong byte ([code unit](https://utf-8000.jb2170.com/#def-code-unit)) to C functions like `strcpy(3)` and friends. `strcpy` would not interpret this [code unit](https://utf-8000.jb2170.com/#def-code-unit) as a null byte, leading to a segfault at best, and serious vulnerabilities at least-worst.   Uniqueness of encoding is another reason why we forbid [overlong encoding](https://utf-8000.jb2170.com/#def-overlong-encoding). Every [codepoint](https://utf-8000.jb2170.com/#def-codepoint) has one unique valid representation as a UTF-8000 [code unit](https://utf-8000.jb2170.com/#def-code-unit), which is easy to encode and decode using bitshifting.  Fun observation: because all 4 of 2-byte UTF-8's [mandatory content bits](https://utf-8000.jb2170.com/#def-mandatory-content-bits) lie in the first-and-final [start byte](https://utf-8000.jb2170.com/#def-start-byte), we can explicitly rule out `11 0 0000 0` (0xC0) and `11 0 0000 1` (0xC1) as permanently invalid bytes. They will never ever appear anywhere in a valid UTF-8000 [code unit](https://utf-8000.jb2170.com/#def-code-unit)!                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |

Properties

Many of these properties of UTF-8000 are explained in detail in an appropriate section of the [glossary](https://utf-8000.jb2170.com/#sec-glossary) and hyperlinks to the glossary are provided.

### Bit Counts

The number of content bits and mandatory content bits are very predictable as a function of `n`, the length of a code unit.

| code unit length | number of content bits | number of mandatory content bits |
| ---------------- | ---------------------- | -------------------------------- |
| `n = 1`          | `7`                    | `0`                              |
| `n = 2`          | `5n+1 ( = 11)`         | `4`                              |
| `n > 2`          | `5n+1`                 | `5`                              |

### Why the Special Cases?

As stated in the [tldr](https://utf-8000.jb2170.com/#sec-tldr), there are only two special cases, both of which are inherited from UTF-8:

**1-byte UTF-8 (ASCII)** which has two points of interest:

- It has 7 content bits which does not fit the pattern of `5n+1`. See the glossary section for [content bits](https://utf-8000.jb2170.com/#def-content-bits) for an explanation, and see the rejected alternative [ASCVI](https://utf-8000.jb2170.com/#sec-rejected-ascvi) code for a version of UTF-8 if ASCII were 6 bit instead of 7 bit which eliminates this special case.
- ASCII has 0 mandatory content bits because it cannot possibly be overlong since it is the smallest possible code unit. This is fine.

**2-byte UTF-8** which has one point of interest:

- It has 4 mandatory content bits, as opposed to 5 for all longer code units. See the glossary section for [mandatory content bits](https://utf-8000.jb2170.com/#def-mandatory-content-bits) for an explanation.

The remarkable fact that UTF-8000 does not introduce any new special cases in extending UTF-8 is confirmation to me that this is the canonical, correct way to extend UTF-8. In other words UTF-8 in its current restricted 4 byte form *is* UTF-8000, but only a small part of it.

The fact that we are even able to extend in the first place is also testament to the clever planning and care that Ken Thompson and Rob Pike put into the architecture of UTF-8, which we ensure to maintain as we extend to UTF-8000. Unary code codewords for the start bits sequences, which form a self-similar tree, were a great choice being simple and extensible. In the [earliest draft](https://www.cl.cam.ac.uk/~mgk25/ucs/utf-8-history.txt#:~:text=111111xx) of UTF-8, the six-byte start-byte looked like `11 1111 xx`. This was changed a [few days later](https://www.cl.cam.ac.uk/~mgk25/ucs/utf-8-history.txt#:~:text=1111110v) to `11 11110 x`. That way the number of content bits is not a special case, and the start bits don't saturate the unary code binary tree, leaving the door open for our future expansion.

This is why I think of UTF-8 as the capstone of the Unix Philosophy.

### Information Rate

What proportion of a code unit is content bits?

For ASCII this is `7/8 = 87.5%`.

Otherwise for an `n` byte code unit this is `(5n+1) / 8n`, that is `5n+1` content bits out of a total of `8n` bits from `n` bytes. We can rewrite this as `(5/8) + 1/(8n)` which moderately quickly approaches `5/8 = 62.5%`. It is nice that this limit is nonzero and does not depend on `n`.

### Self-Synchronization

Inherited from UTF-8 and maintained in UTF-8000.

See the glossary section for [self-synchronization prefix](https://utf-8000.jb2170.com/#def-self-synchronization-prefix) for an explanation of self-synchronization.

Here's a bit of history: Self-synchronization is one of the reasons why Ken Thompson and Rob Pike decided to design UTF-8, to supersede the earlier [FSS-UTF draft](<https://www.cl.cam.ac.uk/~mgk25/ucs/utf-8-history.txt#:~:text=110zzzzz 1yyyyyyy 1xxxxxxx>) by Dave Prosser et al. FSS-UTF proposed a design like eg `110 xxxxx` `1 xxxxxxx` `1 xxxxxxx` for three-byte code units. The problem with it is that one cannot distinguish between first bytes (`110 xxxxx`) and continuation bytes (`1 10xxxxx`) without knowing the prior history of a stream. The UTF-8 fix is to make first byte and continuation byte values disjoint from each other, as one can witness in the [byte map](https://utf-8000.jb2170.com/#sec-properties-byte-map) below. I have not put Prosser's draft into the rejected ideas section as it has already been formally addressed and superseded by UTF-8.

### Self-Punctuation

Inherited from UTF-8 and maintained in UTF-8000.

See the glossary section for [start bits](https://utf-8000.jb2170.com/#def-start-bits) for an explanation of self-punctuation.

### Byte Map

Extended from UTF-8, making use of the higher value bytes. Based off Wikipedia's [UTF-8 Byte Map](https://en.wikipedia.org/wiki/UTF-8#Byte_map).

|    | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9  | A  | B  | C  | D  | E  | F |
| -- | - | - | - | - | - | - | - | - | - | -- | -- | -- | -- | -- | -- | - |
| ␀  | ␁ | ␂ | ␃ | ␄ | ␅ | ␆ | ␇ | ␈ | ␉ | ␊  | ␋  | ␌  | ␍  | ␎  | ␏  |   |
| ␐  | ␑ | ␒ | ␓ | ␔ | ␕ | ␖ | ␗ | ␘ | ␙ | ␚  | ␛  | ␜  | ␝  | ␞  | ␟  |   |
| ␠  | ! | " | # | $ | % | & | ' | ( | ) | \* | +  | ,  | -  | .  | /  |   |
| 0  | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | :  | ;  | <  | \= | \> | ?  |   |
| @  | A | B | C | D | E | F | G | H | I | J  | K  | L  | M  | N  | O  |   |
| P  | Q | R | S | T | U | V | W | X | Y | Z  | \[ | \\ | \] | ^  | \_ |   |
| \` | a | b | c | d | e | f | g | h | i | j  | k  | l  | m  | n  | o  |   |
| p  | q | r | s | t | u | v | w | x | y | z  | {  | &#124; | }  | \~ | ␡  |   |
|    |   |   |   |   |   |   |   |   |   |    |    |    |    |    |    |   |
|    |   |   |   |   |   |   |   |   |   |    |    |    |    |    |    |   |
|    |   |   |   |   |   |   |   |   |   |    |    |    |    |    |    |   |
|    |   |   |   |   |   |   |   |   |   |    |    |    |    |    |    |   |
|    |   | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2  | 2  | 2  | 2  | 2  | 2  |   |
| 2  | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2  | 2  | 2  | 2  | 2  | 2  |   |
| 3  | 3 | 3 | 3 | 3 | 3 | 3 | 3 | 3 | 3 | 3  | 3  | 3  | 3  | 3  | 3  |   |
| 4  | 4 | 4 | 4 | 4 | 4 | 4 | 4 | 5 | 5 | 5  | 5  | 6  | 6  | 7  | 8+ |   |

All bytes except 0xC0 and 0xC1, colored in tomato red, can appear in a valid UTF-8000 stream. See the glossary section for [overlong encoding](https://utf-8000.jb2170.com/#def-overlong-encoding) for an explanation of why those two bytes never appear.

ASCII, colored in gold yellow, occupies the first half of the table, being 7-bit. Continuation bytes occupy the region colored in sandybrown orange. All other bytes are first bytes for multi-byte code units, whose lengths are indicated in the table.

### `strcmp(3)` Ordering

Inherited from UTF-8 and maintained in UTF-8000.

The [self-synchronization prefixes](https://utf-8000.jb2170.com/#def-self-synchronization-prefix) of first bytes are monotonically-increasing-ly ordered with respect to code unit length. In other words ASCII is of length 1 and multi-byte is of length greater than 1, and `0` < `11` occupying the highest bits of UTF-8000 bytes.

The [start bit](https://utf-8000.jb2170.com/#def-start-bits) sequences are also monotonically-increasing-ly ordered with respect to code unit length. In other words if `n < m` then `111...[n]...10` `<` `111...[m]...10` as an integer value, occupying the heads of the code unit bytes beyond the self-synchronization prefixes. This would not have been the case had UTF-8 been designed to use the alternative form of unary codewords given by `000...01`.

The [content bits](https://utf-8000.jb2170.com/#def-content-bits) of code units are also monotonically-increasing-ly ordered with respect to codepoint value.

Combining these three things together means that `strcmp(3)`, the C stdlib string comparing function, works the same way on UTF-8000 bytes as it does on UTF-8, as it does on ASCII, effectively comparing the encoded codepoint values against each other without having to actually decode the code units. Nice!

### No Endianness

The quantum of ASCII, UTF-8, and UTF-8000 is a single byte. This makes life a breeze! There is no need for a concept of endianness for UTF-8000.

UTF-16 however has a quantum of two bytes, 16-bit units. When writing the codewords in bytes, 8-bit units, should the byte containing the most significant digits or least significant digits be written first? Big-endian, or little-endian? This choice gives UTF-16 two variants, UTF-16-BE and UTF-16-LE. If one cannot predetermine the endianness of a stream, one may wish to use a BOM which is discussed below.

### BOM Support

A [Byte Order Mark](https://en.wikipedia.org/wiki/Byte_order_mark) (BOM) is used at the start of an encoded text stream to indicate what encoding is used. I have never actively used BOMs myself so I've only put a bit of thought into this section.

As far as I'm aware we don't break BOM support for UTF-8, though we may wish to have a different BOM to strictly distinguish UTF-8 from UTF-8000. Maybe UTF-8000 could have multiple BOMs, one for each integer N greater than or equal to four, to indicate to a decoder the maximum expected code unit length.

One of the reasons why `U+FFFE` is not a valid Unicode Scalar Value is because `0xFE 0xFF` is the BOM for UTF-16. Since UTF-16 code units are two bytes wide, one may read either `0xFE 0xFF` or `0xFF 0xFE` depending on endianness. To make it clear that `0xFF 0xFE` implies correct for endianness and cannot be mistaken for a legitimate character, `U+FFFE` is designated as `<noncharacter-FFFE>`. We are relieved in that neither `11 111111` `11 111110` nor `11 111110` `11 111111` are valid UTF-8000 sequence extracts, ie UTF-8000 does not introduce incompatibilities with UTF-16.

### Arbitrary Lengths, Sensible Limits

I think we've made it clear by now that UTF-8000 code units can be arbitrarily large. In practice however one *may* wish to set a sensible limit on code unit lengths when decoding. Here we'll discuss a method of finding some nice code unit lengths whose code units store `5n+1 = 2^N` bits, as we are often interested in powers of 2 in computer science.

It is a common observation that 3-byte UTF-8 stores `5 * 3 + 1 = 16` bits, meaning the [Basic Multilingual Plane](https://en.wikipedia.org/wiki/Plane_\(Unicode\)#Basic_Multilingual_Plane) of Unicode can be encoded in one two and three byte UTF-8. We see that `2^4 mod5 = 16 mod5 = 1 mod5`; if `5n+1` is to be `2^N` for some `n` then certainly `2^N = 1 mod5`. If we enumerate powers of two modulo five then there is a very predictable repeating pattern of 1, 2, 4, 3. Formally you might say that 2 is a generator of 𝔽5\* if you want impress a mathematician! The takeaway is that when `N=4K` for `K≥1` we can find a corresponding `n` such that `5n+1 = 2^N`. We can rewrite `2^N` as `2^(4K) = 16^K`.

In other words any power of 16 has a UTF-8000 code unit length containing that many bits. Here are a few of these for reference.

| `K`   | `N=4K` | number of content bits `= 2^N` | code unit length `= (2^N - 1) / 5` |
| ----- | ------ | ------------------------------ | ---------------------------------- |
| `1`   | `4`    | `16`                           | `3`                                |
| `2`   | `8`    | `256`                          | `51`                               |
| `3`   | `12`   | `4096`                         | `819`                              |
| `4`   | `16`   | `65536`                        | `13107`                            |
| `...` | `...`  |                                |                                    |

Do remember that strictly speaking one shouldn't allow overlong encodings, if one were for example thinking of storing a small `uint256_t` key in a 51 byte code unit! UTF-8000's variable width nature helps out leading to smaller code units for smaller integers.

Intuitive Derivation

There are a few ways that one could arrive at the design for UTF-8000 and the bit counts above.

One may think to start with UTF-8, notice that the start byte of an `n` byte code unit is prefixed with the unary codeword of length `n+1`, that is `n` `1` bits followed by a `0`, and then figure out how to extend those bits and roll them over into the continuation bytes without losing any important properties. This is what I *originally* did.

Writing this document over a couple of weeks made me introspect the code unit anatomy further, whence I figured out that separating the leading bits into a self-synchronization part and self-punctuation part further illuminates and simplifies the thought process. We shall thus proceed with this perspective.

#### Blank Slate

We set out to derive the design of an `n` byte code unit, starting out with `n` blank bytes, all of whose bits could possibly be content bits.

`00000000` `00000000` `00000000` `...` `00000000`

To achieve self-synchronization we need to distinguish the first byte of the code unit from the continuation bytes that follow. We could do that by setting the highest bit of first bytes to a `0` and to `1` for continuation bytes. Doing it this way round maintains compatibility with ASCII's highest bit being `0`.

`0 0000000` `1 0000000` `1 0000000` `...` `1 0000000`

With the design so far, all code units begin with an ASCII byte. When decoding a code unit, we have no idea whether this first byte actually is ASCII, or it is the first byte of a multi-byte code unit. We want self-punctuation, where a code unit intrinsically tells us how long it is.

To achieve self-punctuation we create a prefix-free code binary tree, whose leaf node codewords correspond to code unit lengths. These are the start bits sequences. The codeword for `n` shall be embedded inside the code unit towards the start. It must therefore be short enough to fit into the `n` bytes, and reasonably computationally predictable. We try:

```

  .----0                          One byte UTF-8 (ASCII)
  `----1---0                    Two byte UTF-8
        `----1---0            Three byte UTF-8
              `----1---0       Four byte UTF-8
                    `----1---0 Five byte UTF-8000
                          `----...    n byte UTF-8000
              
```

This seems reasonably simple so far. We stripe the start bits into the available bits not taken by the self-synchronization prefix. All other bits shall be content bits.

|     |              |              |              |              |     |             |
| --- | ------------ | ------------ | ------------ | ------------ | --- | ----------- |
| 1   | `0 0 xxxxxx` |              |              |              |     |             |
| 2   | `0 10 xxxxx` | `1 x xxxxxx` |              |              |     |             |
| 3   | `0 110 xxxx` | `1 xx xxxxx` | `1 xxxxxxx`  |              |     |             |
| ... |              |              |              |              |     |             |
| 17  | `0 1111111`  | `1 1111111`  | `1 110 xxxx` | `1 xx xxxxx` | ... | `1 xxxxxxx` |
| ... |              |              |              |              |     |             |

But wait we've broken the distinction of ASCII! We cannot tell the difference between eg `0 110 xxxx` and `0 110xxxx`, or `0 1111111` and `0 1111111`. This code would only work if ASCII were six-bit instead of seven-bit. Out of curiosity we investigate this code in the rejected alternatives section [ASCVI](https://utf-8000.jb2170.com/#sec-rejected-ascvi).

To maintain compatibility with ASCII we must treat it as a special case, whereby the self-synchronization prefix `0` is alone sufficient to characterize ASCII. This highlights that the architecting of UTF-8 was not purely a mathematics problem, but was also an engineering problem, working around what already exists.

Seeing the ASCII-characterizing prefix `0` and the erstwhile continuation prefix `1` as forming a prefix-free tree, albeit only of size two, we must repurpose the the latter codeword as the beginning of the self-synchronization prefixes for first bytes and continuation bytes of multi-byte code units. We choose our new self-synchronization prefixes as `11` for start bytes and `10` for continuation bytes. This produces the following tree:

```

  .----0              First byte for ASCII
  `----1---0 Continuation byte for multi-byte UTF-8000
        `----1        First byte for multi-byte UTF-8000
              
```

Accordingly adjusting the self-punctuation codewords to apply only to multi-byte code units produces the following tree:

```

  .----0                    Two byte UTF-8
  `----1---0            Three byte UTF-8
        `----1---0       Four byte UTF-8
              `----1---0 Five byte UTF-8000
                    `----...    n byte UTF-8000
              
```

Putting these mechanisms together yields UTF-8000 and we're done!

|          |               |              |              |              |              |             |             |             |             |             |             |
| -------- | ------------- | ------------ | ------------ | ------------ | ------------ | ----------- | ----------- | ----------- | ----------- | ----------- | ----------- |
| ASCII    |               |              |              |              |              |             |             |             |             |             |             |
| 1        | `0 xxxxxxx`   |              |              |              |              |             |             |             |             |             |             |
| UTF-8    |               |              |              |              |              |             |             |             |             |             |             |
| 2        | `11 0 xxxx x` | `10 xxxxxx`  |              |              |              |             |             |             |             |             |             |
| 3        | `11 10 xxxx`  | `10 x xxxxx` | `10 xxxxxx`  |              |              |             |             |             |             |             |             |
| 4        | `11 110 xxx`  | `10 xx xxxx` | `10 xxxxxx`  | `10 xxxxxx`  |              |             |             |             |             |             |             |
| UTF-8000 |               |              |              |              |              |             |             |             |             |             |             |
| 5        | `11 1110 xx`  | `10 xxx xxx` | `10 xxxxxx`  | `10 xxxxxx`  | `10 xxxxxx`  |             |             |             |             |             |             |
| 6        | `11 11110 x`  | `10 xxxx xx` | `10 xxxxxx`  | `10 xxxxxx`  | `10 xxxxxx`  | `10 xxxxxx` |             |             |             |             |             |
| 7        | `11 111110`   | `10 xxxxx x` | `10 xxxxxx`  | `10 xxxxxx`  | `10 xxxxxx`  | `10 xxxxxx` | `10 xxxxxx` |             |             |             |             |
| 8        | `11 111111`   | `10 0 xxxxx` | `10 xxxxxx`  | `10 xxxxxx`  | `10 xxxxxx`  | `10 xxxxxx` | `10 xxxxxx` | `10 xxxxxx` |             |             |             |
| 9        | `11 111111`   | `10 10 xxxx` | `10 x xxxxx` | `10 xxxxxx`  | `10 xxxxxx`  | `10 xxxxxx` | `10 xxxxxx` | `10 xxxxxx` | `10 xxxxxx` |             |             |
| 10       | `11 111111`   | `10 110 xxx` | `10 xx xxxx` | `10 xxxxxx`  | `10 xxxxxx`  | `10 xxxxxx` | `10 xxxxxx` | `10 xxxxxx` | `10 xxxxxx` | `10 xxxxxx` |             |
| ...      |               |              |              |              |              |             |             |             |             |             |             |
| 22       | `11 111111`   | `10 111111`  | `10 111111`  | `10 110 xxx` | `10 xx xxxx` | `10 xxxxxx` | `10 xxxxxx` | `10 xxxxxx` | `10 xxxxxx` | ...         | `10 xxxxxx` |
| ...      |               |              |              |              |              |             |             |             |             |             |             |

It is a [trivial\*](https://utf-8000.jb2170.com/#codes-and-cryptography-example-sheet:~:text=It%20is%20question%202%20of%20example%20sheet%201) result of coding theory that the product of two prefix-free codes is also a prefix-free code. The product of our trees looks like:

```

  .----0                                                  ASCII byte
  `----1---0                               UTF-8 continuation byte
        `----1---0                    Two byte UTF-8    start byte
              `----1---0            Three byte UTF-8    start byte
                    `----1---0       Four byte UTF-8    start byte
                          `----1---0 Five byte UTF-8000 start byte
                                `----...    n byte UTF-8000 start byte
              
```

Without the color highlighting this is how most people think about UTF-8: a start byte whose prefix of `n` `1` bits and a terminating `0` bit provides both self-synchronization and self-punctuation, and continuation bytes using a short prefix of `10` for space-efficient encoding. This makes sense for a small number of bytes, but the trick to unlock a perspective of infinite extensibility is to split this tree into the self-synchronization part and self-punctuation part; ie we un-product those prefix-free codes. Failing to do this leads to the rejected alternative [UTF-Infinity](https://utf-8000.jb2170.com/#sec-rejected-utf-infinity).

Encoding

This section is based off the [reference implementation](https://utf-8000.jb2170.com/#sec-reference-implementation) which is written in Python. It is well documented, and is more specific on how to use bitwise operations. This is an abridged HTML version.

Suppose that we have an unsigned integer `n` that we want to encode in UTF-8000. Initialize an empty dynamic array of bytes `ret_ints` that will store the UTF-8000 code unit.

If `n < 0x80`, eg `n = 0x41`, then insert `n` at the head of `ret_ints` and we are done. This is the ASCII byte for `n`, which in our example of `n = 0x41` is a capital letter a, `'A'`.

Otherwise for `n ≥ 0x80`, eg `n = 0x0321C0FFEE8086`, we use UTF-8000. Initialize an integer counter `n_bits_content_occupied` to zero.

Our example `n`'s content bits look like `11 001000 011100 000011 111111 111011 101000 000010 000110` as a big raw number, with spaces added for visual ease.

While `n` has more than 6 content bits, aka `n > 63 =``00 111111`, extract the least-significant 6 bits of `n` and insert them at the head of `ret_ints`, incrementing `n_bits_content_occupied` by 6 and downwards bitshifting `n` by 6.

`n_bits_content_occupied = 48`, `n = 0b``11`,

 `ret_ints`: `00 001000` `00 011100` `00 000011` `00 111111` `00 111011` `00 101000` `00 000010` `00 000110`

Now insert the rest of `n` at the head of `ret_ints`. Count the number of bits left in `n` by downwards bitshifting `n` one bit at a time while it is non-zero. This is between 1 and 6 (inclusive), which we also add to `n_bits_content_occupied`.

`n_bits_content_occupied = 50`, `n = 0`,

 `ret_ints`: `000000 11` `00 001000` `00 011100` `00 000011` `00 111111` `00 111011` `00 101000` `00 000010` `00 000110`

Now we calculate how many bytes our UTF-8000 code unit requires, `n_utf_8000_bytes_needed`. We know that a `k`-byte code unit has capacity for `5k+1` content bits. Therefore `⌈(n_bits_content_occupied-1) / 5⌉` is the sufficient and minimal answer. Any larger code unit size would lead to an overlong encoding! For our example `n_utf_8000_bytes_needed = ⌈(50-1) / 5⌉ = 10`.

Leftwards pad `ret_ints` with empty bytes to the length `n_utf_8000_bytes_needed`.

 `ret_ints`: `00000000` `000000 11` `00 001000` `00 011100` `00 000011` `00 111111` `00 111011` `00 101000` `00 000010` `00 000110`

Now we add the start bits. The number of `1` start bits is equal to two less than the number of bytes in the code unit, which we just calculated. We therefore calculate `q, r = divmod(n_utf_8000_bytes_needed-2, 6)`, which tells us we need `q` hextets full of `1` start bits, and a final hextet of zero to five `1` bits, which also has space to contain the terminating `0` bit. In our example `(q = 1, r = 2) = divmod(10-2, 6)`.

Apply the start bits across `ret_ints` using bitwise-or. The final start bits hextet can be given by `((1 << r) - 1) << (6 - r)`.

 `ret_ints`: `00 111111` `00 110 0 11` `00 001000` `00 011100` `00 000011` `00 111111` `00 111011` `00 101000` `00 000010` `00 000110`

Any of the lowest six bits of each byte that are not set by this point, unoccupied by content bits and untouched by start bits, are really content bits that the `k`-byte capacity provides but that we didn't need. Our example's `n_bits_content_occupied = 50` is one less than `5k+1 = 5*10+1 = 51`. We can color highlight it green as a content bit for completion's sake.

 `ret_ints`: `00 111111` `00 110 011` `00 001000` `00 011100` `00 000011` `00 111111` `00 111011` `00 101000` `00 000010` `00 000110`

Now we crown the bytes with their self-synchronization prefixes, which delivers us from hextets to UTF-8000 octets. The first byte's self-synchronization prefix is `11`, and continuation bytes have `10`.

 `ret_ints`: `11 111111` `10 110 011` `10 001000` `10 011100` `10 000011` `10 111111` `10 111011` `10 101000` `10 000010` `10 000110`

And we're done!

Decoding

As with the encoding section, this section is based off the [reference implementation](https://utf-8000.jb2170.com/#sec-reference-implementation) which is written in Python and well documented.

Suppose that we are receiving a stream of UTF-8000 bytes (possibly with errors!), and we wish to extract and taxonomically annotate the incoming code units. There are a few ways that we could approach this, such as using the [byte map](https://utf-8000.jb2170.com/#sec-properties-byte-map) as a state machine, which I want to try in the future, or the classic way of using bitwise masks. We are going to use the latter approach in this section, as we describe how to decode a single code unit. But first, a look at error handling.

### Error Recovery

The errors that can occur when decoding a UTF-8000 stream are:

1. Reading a continuation byte (`10`) when we are expecting the first byte of a code unit (`0` or `11`).
2. Reading a first byte (`0` or `11`) when we are expecting a continuation byte (`10`).
3. Early EOF midway through a code unit.
4. Encountering an overlong encoding
   1. For 2-byte code units this is bytes 0xC0 (`11 0 0000 0`) and 0xC1 (`11 0 0000 1`).
   2. For `n`-byte code units in general, with `n > 2`, eg `11 10 0000` `10 0 10111` `10 010000`.
5. Encountering an encoded surrogate codepoint in the range `U+D800` to `U+DFFF`, which is forbidden for compatibility with UTF-16.

For standard UTF-8 one would also have to be concerned with codepoints beyond the range `U+10FFFF` whence bytes 0xF5 to 0xFF go unused.

For any of these errors a parser could raise an exception and refuse to continue. Alternatively it could take advantage of UTF-8000's self-synchronization property, and keep calm and carry on, yielding Unicode replacement characters `U+FFFD` � until we reach the first byte of the next code unit. Let us investigate the latter course.

To handle error 1. the parser should return one � and get ready to parse the next code unit. When handling error 2. the parser should make sure to unpop the byte encountered, as it is the first byte of the next code unit. When handling errors 2. through to 5. there are a couple of mainstream approaches for yielding � characters:

#### Maximal Subpart

The Unicode Consortium recommends, but [does not enforce](<https://www.unicode.org/versions/Unicode16.0.0/core-spec/chapter-3/#:~:text=Although the Unicode Standard does not require,gives detailed examples>), a maximal subpart approach, in which the longest well-formed part of a code unit should return a single � character, rather than one for each byte involved. For example the three bytes in error 4.2. above should return one � as it is well-formed with respect to self-synchronization and self-punctuation, and only invalid at an overlong level, being an overlong encoding of `11 0 1011 1` `10 010000` `U+05D0`, a Hebrew letter Aleph 'א'.

I dislike this approach. Waiting for maximal subparts has the problem that the rest of an invalid code unit may never arrive. If we receive the bytes `11 10 0000` `10 0 10111` from a socket, then the remote end may be waiting for us to chastise their overlong opening bytes with a response, because we can already tell that these bytes form part of an invalid code unit. Using the maximal subpart approach we *also* would be waiting, for the remote end to send a continuation byte eg `10 010000` to form an overlong but otherwise complete 3-byte code unit. This is uncooperative, and not what I want.

#### One � For Each Byte Read

We are going to do what Python, my terminal KDE Konsole, and others do, and simply return a � character for each invalid byte. In Python `b'\xE0\x97\x90'.decode(errors='replace')` returns `'���'`.

This approach is easier and more versatile. The end user can see how many invalid bytes occurred by counting the number of � characters. There are no deadlock waiting events that can occur with the maximal subpart approach.

### The Main Decode Loop

Initialize an empty dynamic array of bytes `parsed_bytes` that will store the bytes as we parse them.

Read a byte, store it as `start_byte`. Use bitwise masks to find the index, `idx_0`, of the most-significant zero bit in the byte. If there are no zeros in this byte, `idx_0` should be set to `-1`.

If `idx_0 == 7` (`0 xxxxxxx`) then `start_byte` is an ASCII byte, which has seven content bits. Append `start_byte` to `parsed_bytes` and we are done.

If `idx_0 == 6` (`10 xxxxxx`) then `start_byte` is a continuation byte, which is an invalid start byte. Go to [error 1](https://utf-8000.jb2170.com/#sec-decoding-error-1).

If `idx_0 == 5` (`11 0 xxxx x`) then this is the first byte of a 2-byte code unit. We treat this as a special case because there are only 4 mandatory content bits, not 5. As they are all contained in `start_byte` we can check them immediately for overlong encoding, to see if we need to handle [error 4.1](https://utf-8000.jb2170.com/#sec-decoding-error-4-1). If `start_byte` passes this check then append it to `parsed_bytes` and await a continuation byte. Handle [error 2](https://utf-8000.jb2170.com/#sec-decoding-error-2) if necessary, else append the continuation byte to `parsed_bytes` and we're done.

We could (should really) make `idx_0 == 4` a special case too, to check for and forbid the surrogate ranges. I have omitted this for the time being and we drop through to the generic case below.

Otherwise we enter the generic case (`11 1[1...]`). Initialize an integer counter `n_bytes_expected` to 2. Increment `n_bytes_expected` by `5 - idx_0`, as `idx_0` now serves the purpose being the index of the terminating zero of the start bits, `0`.

If `idx_0 == -1` then our code unit has multiple start bytes, exciting! Append `start_byte` to `parsed_bytes`, and `while(1)`:

Read a byte, and make sure it is a continuation byte lest we go to [error 2](https://utf-8000.jb2170.com/#sec-decoding-error-2). Use bitwise masks to find `idx_0`, the index of the most-significant zero bit in the lowest *six* bits of the byte, setting `idx_0` to `-1` if there is none. This is to continue trying to find the `0` start bit. Increment `n_bytes_expected` by `5 - idx_0`. If `idx_0 == -1` then append `start_byte` to `parsed_bytes` and continue again through this loop, until we find the `0` bit, at which point we break this loop.

At this stage, whether our code unit has multiple start bytes or just one, `start_byte` is the *final* start byte of the code unit, `idx_0` is between 0 and 5 (inclusive), and we move towards checking for overlong encoding. Just as [ordinals](https://en.wikipedia.org/wiki/Ordinal_number#Von_Neumann_definition_of_ordinals) count the number of things less than themselves, `idx_0` counts the number of content bits contained `start_byte`, occupying the least significant bits.

There are six cases for anti-overlong checking, which correspond to `idx_0`'s value. That may sound like a lot, but the looping gif below that I made should relax you. It demonstrates periodic behavior. Even though it shows deep code unit sections with multiple start bytes, this animation still applies for *all* code units of length `n > 2`. The colored bars are based off the [anatomy section](https://utf-8000.jb2170.com/#sec-anatomy) image.

![](https://utf-8000.jb2170.com/overlong-window.gif)

If `idx_0 == 5` then all the mandatory content bits are contained together in the final start byte. Thus we should immediately check `start_byte` using the mask `000 11111`. We then read the first non-start byte, a continuation byte which does not need overlong checking (`10 xxxxxx`).

Otherwise we read another continuation byte, the first non-start byte. If `idx_0 == 0` then all the mandatory content bits are contained together in this first non-start byte (`10 xxxxx x`), and we use the mask `00 11111 0` to check for overlong encoding. Else `idx_0` is between 1 and 4 (inclusive) and the mandatory content bits are straddled across the final start byte and first non-start byte. In these cases we use two masks to check for overlong encoding, which one can see in the gif above.

Perhaps the case of `idx_0 == 0` could be grouped in with `idx_0` being between 1 and 4, by using an empty mask to check the final start byte, in order to make the algorithm less branch-y, but this walkthrough isolates which bytes are responsible for potential overlong encoding.

Given that the final start byte and first non-start byte have passed the overlong check, append them to `parsed_bytes`. Finally while the length of `parsed_bytes` is less than `n_bytes_expected`, read plain-old continuation bytes (`10 xxxxxx`) and append them to `parsed_bytes`.

And we're done!

Further Ideas

Signed Variant: ZigZag Encoding

So far we have used UTF-8000 to encode codepoints, aka non-negative integers, aka unsigned integers. I have come up with a couple of modified interpretations of the content bits which allow us to encode the *entire* integers, aka the signed integers.

We make use of a marvelous bijective mapping between the signed integers and unsigned integers called the zigzag function that remains a bijection when restricting to the respective `n`-bit ranges. We use this as a final layer at the beginning/end of the standard UTF-8000 encoding/decoding procedure.

### Source

ZigZag encoding from Protobuf by Google: *[Protocol Buffers Documentation / Encoding](https://protobuf.dev/programming-guides/encoding/#:~:text=ZigZag)*

Myself: This seems like the perfect extensible solution for how to encode signed integers on top of UTF-8000.

### TLDR / Examples

The code unit structure is identical to UTF-8000. The content bits correspond to the image of the zigzag function.

| `zigzag(z)` | `z`     | `UTF-8000`    |             |
| ----------- | ------- | ------------- | ----------- |
| ...         | ...     | ...           |             |
| `124`       | ` 62  ` | `0 1111100`   |             |
| `125`       | `-63`   | `0 1111101`   |             |
| `126`       | ` 63`   | `0 1111110`   |             |
| `127`       | `-64`   | `0 1111111`   |             |
| `128`       | ` 64`   | `11 0 0001 0` | `10 000000` |
| `129`       | `-65`   | `11 0 0001 0` | `10 000001` |
| `130`       | ` 65`   | `11 0 0001 0` | `10 000010` |
| `131`       | `-66`   | `11 0 0001 0` | `10 000011` |
| ...         |         |               |             |

### The ZigZag Function

The `zigzag` function maps from the signed integers to the unsigned integers.

If `z ≥ 0` then `zigzag(z) = 2 * z = (z << 1)`

If `z < 0` then `zigzag(z) = -2 * z - 1 = -(z << 1) - 1 = ~(z << 1)`

| `z`  | `zigzag(z)` |
| ---- | ----------- |
| ...  | ...         |
| `-4` | `7`         |
| `-3` | `5`         |
| `-2` | `3`         |
| `-1` | `1`         |
| ` 0` | `0`         |
| ` 1` | `2`         |
| ` 2` | `4`         |
| ` 3` | `6`         |
| ...  | ...         |

| `zigzag(z)` | `z`  |
| ----------- | ---- |
| ...         | ...  |
| `0`         | ` 0` |
| `1`         | `-1` |
| `2`         | ` 1` |
| `3`         | `-2` |
| `4`         | ` 2` |
| `5`         | `-3` |
| `6`         | ` 3` |
| `7`         | `-4` |
| ...         | ...  |

```
       ______________
      /  __________  \
     /  /  ______  \  \
    /  /  /  __  \  \  \
   /  /  /  /  \  \  \  \
  -4 -3 -2 -1  0  1  2  3
   \  \  \  \_____/  /  /
    .  \  \_________/  /
     .  \_____________/
      .
                    
```

The ASCII art above illustrates the enumeration of the preimage of `zigzag`, showing it zigzagging between positives and negatives. This should make it clear how after `2^N` steps we have covered exactly the range `[-2^(N-1), 2^(N-1))`.

For example the preimage of the 7-bit unsigned range `[0, 128)` is the 7-bit signed range `[-64, 64)`.

#### Branchless ZigZag

If one is dealing with fixed-width integers, for example mapping from `int32_t` to `uint32_t`, one can create a branchless version of `zigzag`, wow! CPUs like branchless code.

With this example `zigzag(z) = (z << 1) ^ (z >> 31)`, where `^` here is the C bitwise-xor operator.

If `2^31 > z ≥ 0` then `(z >> 31) = 0`, because we have filled the register with the highest bit of a non-negative signed number, 0. Thus `zigzag(z) = (z << 1) ^ (z >> 31)`. Okay, nothing special?

But if `-2^31 ≤ z < 0` then `(z >> 31) = -1`, because we have filled the register with the highest bit of a negative signed number, 1. Aha, so to achieve the bitwise complement, `~(z << 1)`, we can bitwise-xor with this `-1`. Thus `zigzag(z) = (z << 1) ^ (z >> 31)`.

### Properties

Self-synchronization, self-punctuation, and arbitrary code unit length have the same conclusion as base UTF-8000. Properties that differ are discussed below.

#### Encoded Range

The content bit counts work [the same as they do for UTF-8000](https://utf-8000.jb2170.com/#sec-properties-bit-counts). Below is a summary of the ranges of integers that the content bits encode.

| code unit length | number of content bits | minimum integer        | maximum integer            |
| ---------------- | ---------------------- | ---------------------- | -------------------------- |
| `n = 1`          | `7`                    | `-2 ^ (7n-1) ( = -64)` | `+2 ^ (7n-1) - 1 ( = +63)` |
| `n ≥ 2`          | `5n+1`                 | `-2 ^ (5n)`            | `+2 ^ (5n) - 1`            |

#### Small Integers, Small Code Units

UTF-8000 is really just a variable-width bit container with some nice properties. Provided that we obey the forbidding of overlong encoding, we can use the content bits as we please, encoding from an arbitrary alphabet to unsigned integer codewords that form the content bits.

The alphabet in question for us is the signed integers, ℤ. We heuristically think of magnitude as a measure of commonness. The closer an integer is to zero, the more common it is, and thus the smaller the unsigned integer that it should be encoded as, whence the shorter the UTF-8000 code unit it occupies. This is almost common sense.

We have observed that `zigzag` achieves this. Two's complements in a fixed-width register however does *not* do this, as for example in a 64-bit CPU register the number -1 is encoded as `111...[64]...11`. This is not a problem for hardware like CPUs, but we are interested in efficient encoding for transmission and storage.

#### Modified `strcmp(3)` Ordering

Since the negative integers are interwoven (zigzagged) between the non-negative integers via `zigzag`, we lose [`strcmp` ordering](https://utf-8000.jb2170.com/#sec-properties-strcmp-ordering) from UTF-8000. For example -1 < 0 but `0 0000001` > `0 0000000`. However, being undeterred we can supersede this fact.

The purpose of `strcmp(s1, s2)` with respect to UTF-8000 is to quickly compare code units `s1` and `s2` as a proxy for comparing their contained codepoints, without having to actually decode the code units. For this modified version of UTF-8000 we wish to create a quick proxy for comparing the contained signed integers.

The only variants of UTF-8000 that can make exact use of `strcmp` are those whose content bits encode letters from a totally-ordered alphabet, for which there exists an order-preserving bijection between that alphabet and the unsigned integers. Since the unsigned integers has a minimum element, 0, and the signed integers (our alphabet) does not have a minimum element, no such bijection exists.

We know that `zigzag(z)` breaks nicely into two cases, non-negative signed integers and negative signed integers. We also know that order-preserving bijections *do* exist between 0) non-negative signed integers and the even unsigned integers, and 1) negative signed integers and the odd unsigned integers. We initially break our new `strcmpsigned(s1, s2)` function into four cases depending on the final bit of each code unit, which we know is a content bit and indicates whether the stored unsigned integer is even or odd. We can obtain these bits via `b1 = c1 & 1` and `b2 = c2 & 1` where `c1` and `c2` are the final bytes of the respective code units:

| `b1` | `b2` | `comment` | `b2 - b1` | `1 - b1 - b2` |
| ---- | ---- | --------- | --------- | ------------- |
| `0`  | `0`  | `s1 ? s2` | ` 0`      | ` 1`          |
| `0`  | `1`  | `s1 > s2` | ` 1`      | ` 0`          |
| `1`  | `0`  | `s1 < s2` | `-1`      | ` 0`          |
| `1`  | `1`  | `s1 ? s2` | ` 0`      | `-1`          |

If `b2 - b1` is non-zero then `strcmpsigned` can return that, as we are effectively comparing two signed integers of a different sign. Otherwise `(1 - b1 - b2) * strcmp(s1, s2)` effectively compares two integers of the same sign. We could therefore write this as:

`strcmpsigned(s1, s2) = (b2 - b1) ? (b2 - b1) : (1 - b1 - b2) * strcmp(s1, s2)`

That's pretty succinct! I have also assumed that `strcmp` is just the simple `{-1, 0, +1}` version.

### Verdict

I like it! I'll add it to the [reference implementation](https://utf-8000.jb2170.com/#sec-reference-implementation) when I get chance. It will most likely be a flag `-z` for zigzag used like `$ utf-8000 info -z -- -67` showing `11 0 0001 0` `10 000101`.

This zigzag variant has a big advantage over the rejected [two's complement signed variant](https://utf-8000.jb2170.com/#sec-rejected-signed-variant-twos-complement) in that we don't need to change how we do overlong checking from the standard UTF-8000 method. This means that we can effectively separate out into layers: 1) the overlong checking of code units and the extracting of their content bits, from 2) the further decoding of the content bits eg to a signed integer.

The only external metadata needed when decoding a stream of UTF-8000 bytes is is this unsigned or signed?. This is no different to decoding a stream of raw bytes, or inspecting fixed-width integers stored in two's complement form in a CPU register: it's up to the programmer's use-case to know whether signed or unsigned is expected.

UTF-16K

Could we apply some techniques from this document to also extend UTF-16? Yes, but at the cost of forbidding more codepoints from being encoded similar to the forbidden surrogate range `U+D800` to `U+DFFF`.

UTF-16 is a bit messy in its existing two-byte and four-byte form, but we can clean this up in a mostly forwards-compatible manner by using [ASCVI](https://utf-8000.jb2170.com/#sec-rejected-ascvi)-on-UTF-16. We assume the use of big-endian UTF-16 in this section.

The high (`110110`) and low (`110111`) surrogate prefixes are highlighted in bright pink.

For normal 20-content-bit surrogate pair UTF-16, the upper four content bits of high surrogates encode which Unicode Plane (collection of `2^16 = 64k` codepoints) that the code unit's content bits belong to. These bits are highlighted in bright crimson.

For multi-surrogate-pair UTF-16K we highlight only the upper three of these plane bits, the erstwhile fourth being a self-synchronization bit.

### Source

Myself: Realizing that I can challenge the UTF-16 extensions proposed by [UCS-X](https://utf-8000.jb2170.com/#sec-rejected-ucs-x).

### TLDR / Examples

|     |                         |                       |                         |                      |                         |                      |     |
| --- | ----------------------- | --------------------- | ----------------------- | -------------------- | ----------------------- | -------------------- | --- |
| 2   | `xxxxxxxx xxxxxxxx`     |                       |                         |                      |                         |                      |     |
| 4   | `110110 xx xx xxxxxx`   | `110111 xx xxxxxxxx`  |                         |                      |                         |                      |     |
| 8   | `110110 10 0 0 0 xxxxx` | `110111 xx xxx xxxxx` | `110110 10 0 1 xxxxxx`  | `110111 xx xxxxxxxx` |                         |                      |     |
| 12  | `110110 10 0 0 10 xxxx` | `110111 xx xxxxxxxx`  | `110110 10 0 1 x xxxxx` | `110111 xx xxxxxxxx` | `110110 10 0 1 xxxxxx`  | `110111 xx xxxxxxxx` |     |
| 16  | `110110 10 0 0 110 xxx` | `110111 xx xxxxxxxx`  | `110110 10 0 1 xx xxxx` | `110111 xx xxxxxxxx` | `110110 10 0 1 xxxxxx`  | `110111 xx xxxxxxxx` | ... |
| 20  | `110110 10 0 0 1110 xx` | `110111 xx xxxxxxxx`  | `110110 10 0 1 xxx xxx` | `110111 xx xxxxxxxx` | `110110 10 0 1 xxxxxx`  | `110111 xx xxxxxxxx` | ... |
| ... |                         |                       |                         |                      |                         |                      |     |
| 76  | `110110 10 0 0 111111`  | `110111 11 11111111`  | `110110 10 0 1 10 xxxx` | `110111 xx xxxxxxxx` | `110110 10 0 1 x xxxxx` | `110111 xx xxxxxxxx` | ... |
| ... |                         |                       |                         |                      |                         |                      |     |
| 144 | `110110 10 0 0 111111`  | `110111 11 11111111`  | `110110 10 0 1 111111`  | `110111 11 11111111` | `110110 10 0 1 110 xxx` | `110111 xx xxxxxxxx` | ... |
| ... |                         |                       |                         |                      |                         |                      |     |

### Properties

Two-byte UTF-16 is just the raw binary form of any 16-bit codepoint, except for the surrogate range `U+D800` to `U+DFFF` of size 2048 which any Unicode encoding (UTF-8, UTF-16, UTF-32) is forbidden to encode. The reason for this exclusion is because otherwise we could not distinguish `110110xx xxxxxxxx` from `110110 xx xx xxxxxx` and `110111xx xxxxxxxx` from `110111 xx xxxxxxxx` which you'll read about below.

Four-byte UTF-16 is two surrogate codepoints stuck next to each other, one high in the range `U+D800` to `U+DBFF` and then one low in the range `U+DC00` to `U+DFFF`. The real codepoint that they encode is 0x10000 added to the 20 binary digit number contained in the content bits within. For example `110110 00 00 001000` `110111 00 00101101` contains 0x0202D, which then has 0x10000 added to it, to give 0x1202D. `U+1202D` is 𒀭, a Mesopotamian [Dingir](https://en.wikipedia.org/wiki/Dingir).

To expand UTF-16 indefinitely instead of being stuck with 0x110000 (1,114,112) codepoints, we employ ASCVI inside of UTF-16 surrogate pairs. UTF-8 was able to expand from 7-bit ASCII without any trouble because bytes with the highest bit set were undefined. In UTF-16 however every possible value that the content bits can take defines a codepoint. A smart choice, so that we do not interfere with already assigned codepoints, and so that we do not malapportion too many pre-existing unassigned codepoints for UTF-16K, and for there to be content bit count parity with UTF-8000, is to constrain ourselves to two unassigned planes, e.g. Planes 9 and 10. These planes are nice as the surrogate pairs take the form `110110 10 0 xxxxxx` `110111 xx xxxxxxxx`. The codepoints `U+90000` to `U+AFFFF` are to be forbidden from being encoded, just as the 2048 surrogates of the [Basic Multilingual Plane](https://en.wikipedia.org/wiki/Plane_\(Unicode\)#Basic_Multilingual_Plane) are.

We use these four-byte surrogate pair containers as the quantum for UTF-16K, which uses two or more of these quanta to extend from UTF-16. The content bits encode the codepoint's binary representation, *without* adding on 0x10000 for the sake of simplicity, similar to UTF-8000.

#### Bit Counts

| number of bytes  | number of content bits | number of mandatory content bits |
| ---------------- | ---------------------- | -------------------------------- |
| `n = 2`          | `16`                   | `0`                              |
| `n = 4`          | `20`                   | `0`                              |
| `n = 8  ; k = 2` | `15k + 1 ( = 31)`      | `10, or codepoint ≥ 0x110000`    |
| `n = 4k ; k ≥ 3` | `15k + 1`              | `15`                             |

Overlong checking is complicated for the jump from 4 bytes to 8 bytes, because of the 0x10000 that is added to the content bits. Either one of the highest 10 bits has a non-zero bit, or the `2^20` bit is active and one of the `2^m` bits is active with `16 ≤ m ≤ 19`.

One may notice when enumerating `15k + 1`, the number of content bits for `k`-surrogate-pair UTF-16K, that these values overlap predictably with UTF-8000's number of content bits given by `5n + 1`. This is the result of a deliberate choice to use two planes for UTF-16K instead of e.g. one plane, or half a plane etc.

| number of UTF-16K surrogate pairs | number of UTF-8K bytes | number of content bits |
| --------------------------------- | ---------------------- | ---------------------- |
| `2`                               | `6`                    | `31`                   |
| `3`                               | `9`                    | `46`                   |
| `4`                               | `12`                   | `61`                   |
| `5`                               | `15`                   | `76`                   |
| ...                               | ...                    | ...                    |
| `17`                              | `51`                   | `256`                  |
| ...                               | ...                    | ...                    |

This means that UTF-8K and UTF-16K can be expanded in parallel in a predictable way, such that all possible codepoints from an expansion are permitted. This is in contrast to how 4-byte UTF-8 does not allow use of all `2^21` codepoints, but rather artificially restricts to `2^16 + 2^20` for parity with UTF-16K.

The number of bytes in such UTF-16K code units is always `4/3` that of an equivalent UTF-8K code unit. The reciprocal of this, `3/4`, ends up as the scaling factor of the information rate limit from UTF-8K to UTF-16K. It is nice that this ratio is independent of code unit length.

#### Information Rate

For 2-byte UTF-16 this is technically `16 / 16 = 100%`, ignoring the forbidden surrogate range.

For 4-byte UTF-16 this is `20 / 32 = 62.5%`, again ignoring the forbidden UTF-16K planes 9 and 10. This looks slightly worse than UTF-8's `21 / 32`, but do bear in mind that UTF-8 is also restrained to UTF-16's upper limit.

For beyond four bytes this is `(15k+1) / (4k*8) = 15/32 + 1/(32k)` which approaches `15/32 = 46.875%`, which is okay. As predicted above, this is `3/4` times the information rate limit of UTF-8000. `3/4 * 5/8 = 15/32`.

Below is a comparison of the efficiencies of UTF-8 and UTF-16.

| start     | end        | range size             | number of UTF-8 bytes | number of UTF-16 bytes |
| --------- | ---------- | ---------------------- | --------------------- | ---------------------- |
| `U+0000`  | `U+007F`   | `0x80 = 128`           | `1 (ASCII)`           | `2`                    |
| `U+0080`  | `U+07FF`   | `0x780 = 1,920`        | `2`                   | `2`                    |
| `U+0800`  | `U+FFFF`   | `0xF800 = 63,488`      | `3`                   | `2`                    |
| `U+10000` | `U+10FFFF` | `0x100000 = 1,048,576` | `4`                   | `4`                    |

UTF-16 is more efficient than UTF-8 only at encoding `U+0800` to `U+FFFF`, aka the three-byte UTF-8 range that UTF-16 encodes using two bytes.

For ASCII, and beyond `U+10FFFF`, UTF-8000 is far more efficient (and less ugly) than UTF-16K.

#### Self-Synchronization

UTF-16K does not have self-synchronization at the byte level because UTF-16 does not. If one experiences a single missing byte then potentially the whole stream becomes corrupted.

At the two-byte level UTF-16 has self-synchronization which UTF-16K inherits. Non-surrogate codepoints are quantum; they are to UTF-16 as ASCII is to UTF-8. Surrogate pairs provide self-synchronization with their `110110` and `110111` high and low surrogate prefixes.

UTF-16K goes even deeper, using multiple surrogate pairs that shadow Planes 9 and 10. Within the surrogate pair self-synchronization level, within the high surrogates used to encode UTF-16K, `110110 10 0 xxxxxx`, ASCVI is employed, whose leading bit provides self-synchronization, with `110110 10 0 0` and `110110 10 0 1`.

Therefore overall UTF-16K exhibits self-synchronization at the two-byte level, like UTF-16.

#### Self-Punctuation

Inherited from ASCVI.

#### Compatibility

UTF-16K forbids Planes 9 and 10 of Unicode, because it has no way to encode those codepoints, instead repurposing the surrogate pairs erstwhile required to encode Planes 9 and 10 for the purpose of encoding codepoints beyond 0x110000. An important question to ask regarding forwards compatibility is what does an existing UTF-16 parser do if it meets a UTF-16K code unit?.

In short it's Plane-9-or-10-garbage-in Plane-9-or-10-garbage-out. Each surrogate pair used in encoding a UTF-16K codepoint beyond 0x110000 would be parsed separately as though it belongs to Plane 9 or 10, but with no *syntactic* issues. *Semantically* however this would cause issues with logical character (codepoint) counts that would count each surrogate pair as a separate character, rather than contributing towards a single character.

I think that this is a better solution than UCS-X's [UTF-G-16](https://ucsx.org/g16) which breaks syntactic compatibility with UTF-16 by repurposing low surrogates as leading units for UTF-G-16 6-byte code units. One could argue that UTF-G-16 is better because those bytes could be replaced with a single replacement character � though I'm not convinced, as for example the default behavior of Python's `bytes.decode` function is `'strict'`, which raises an exception, not `'replace'` which produces replacement characters. UTF-G-16 also has flawed error handling behavior as discussed in the [feedback emails](https://utf-8000.jb2170.com/#sec-feedback-tom-bishop:~:text=For%20UTF-G-16%2C%20for,if%20it%0Aencounters%20an%20error.), arising from UTF-G-16's self-synchronization requiring a context-dependent interpretation of low surrogates to determine if they are leading or trailing, whereas UTF-16K's self-synchronization is context-independent by using a disjoint union of planes 9 and 10.

The requirement to extend the list of codepoints that all of UTF-8, UTF-16, and UTF-32 are forbidden from encoding, to include Planes 9 and 10 or elsewhere, would not be an easily negotiated feat. We would be banning an extra `2/17 = 11.8%` of pre-existing codepoints. One may notice that this situation of forbidding pre-existing codepoints is a similar situation to back when 2-byte UTF-16 extended to 4-byte UTF-16. Would we ever have to ban codepoints in pre-existing ranges again after this UTF-16K extension? No, as UTF-8K and UTF-16K are infinitely extensible.

### Verdict

The immature part of me says let UTF-16 decay and die as the short-sighted, legacy, Windows, `wchar_t`, non-self-synchronizing-at-the-byte-level, +0x10000, garbage that it is. But it will be around for a while, with [several uses](https://en.wikipedia.org/wiki/UTF-16#Usage), like the [Joliet Filesystem](https://en.wikipedia.org/wiki/ISO_9660#Joliet) for my beloved Arch Linux ISOs grrr.

The main reason I wrote this section was to provide an alternative to [UCS-X](https://utf-8000.jb2170.com/#sec-rejected-ucs-x), so that we can use the same(ish) style as UTF-8000, and lest UCS-X or an even uglier idea come along.

I predict that the Unicode Consortium would heavily push back on the idea of having to ban more codepoints, `U+90000` to `U+AFFFF`. No matter how one plans to extend UTF-16, it requires either forbidding some codepoints, or changing the syntax, either of which is a breaking change.

For our modern times UTF-8 is undoubtedly the way to go, and by the time that we need to extend to UTF-8000, I would hope that UTF-16 and all other encodings belong in a museum, and we can therefore extend UTF-8 without worrying about compatibility with the others.

### Reference Implementation

Available! See [below](https://utf-8000.jb2170.com/#sec-reference-implementation).

UTF-32K

In the same manner that ASCII is extended by UTF-8 and UTF-8000, UTF-32 could also be extended to be a multi-byte (32-bit chunk) encoding scheme.

Do we really want this though? Is UTF-32 meant to be variable-width, or is it meant to represent the raw codepoint, decoded and stored in memory as a fixed-width integer? I've written this section to demonstrate that ASCVI can be applied to a quantum as small as 3 bits (seriously lol), or large like 32 bits.

### Source

Myself: It seemed obvious how this follows from UTF-8000.

### TLDR / Examples

We could extend UTF-32 either in the style of UTF-8000, treating the one-byte code units as a special case occupying the lower 31 bits...

|     |                                          |                                        |
| --- | ---------------------------------------- | -------------------------------------- |
| 1   | `0 xxxxxxx_xxxxxxxx_xxxxxxxx_xxxxxxxx`   |                                        |
| 2   | `11 0 xxxxx_xxxxxxxx_xxxxxxxx_xxxxxxx x` | `10 xxxxxx_xxxxxxxx_xxxxxxxx_xxxxxxxx` |
| ... |                                          |                                        |

...or in the style of [ASCVI](https://utf-8000.jb2170.com/#sec-rejected-ascvi), with no special cases and the one-byte code units occupying the lower 30 bits.

|     |                                         |                                         |
| --- | --------------------------------------- | --------------------------------------- |
| 1   | `0 0 xxxxxx_xxxxxxxx_xxxxxxxx_xxxxxxxx` |                                         |
| 2   | `0 10 xxxxx_xxxxxxxx_xxxxxxxx_xxxxxxxx` | `1 x xxxxxx_xxxxxxxx_xxxxxxxx_xxxxxxxx` |
| ... |                                         |                                         |

### Properties

Mutatis mutandis, the properties of UTF-8000 and ASCVI apply. We only make further remarks on a couple of properties.

#### Bit Counts

Predictable like ASCVI.

| number of bytes  | number of content bits | number of mandatory content bits |
| ---------------- | ---------------------- | -------------------------------- |
| `n = 4`          | `30`                   | `0`                              |
| `n = 4k ; k ≥ 2` | `30k`                  | `30`                             |

#### Information Rate

Whilst the ASCVI-style UTF-32K has an information rate of `30 / 32 = 93.75%`, it is very inefficient for low-value codepoints, with the highest bytes most often being zeros. UTF-8000 has a much finer telescopic expansion mechanism at the byte level, compared to UTF-32 at a four-byte level.

#### Self-Synchronization

UTF-32 is not self-synchronizing at the byte level, and UTF-32K inherits this weakness. UTF-32 is only self-synchronizing at the four-byte level, similar to how UTF-16 is only self-synchronizing at the two-byte level. UTF-32K maintains self-synchronization at the four-byte level.

#### Endianness

Like UTF-16, and unlike UTF-8 and UTF-8000, UTF-32 has endianness, its quantum being a whopping four bytes.

### Verdict

Not our greatest priority.

UTF-32 is hardly ever used for transmission or storage due to its inefficiency and endianness.

As I wrote in the intro, UTF-32's main use is as a **non**-variable-width container, for when one decodes UTF-8 or UTF-16 to `int32_t` integers (UTF-32) for use inside a program. UTF-32K would be an anti-pattern / counterproductive.

Rejected Alternatives

Although UTF-8000 extends naturally from UTF-8, is it still the best approach? Are there any better alternatives that engineer an extension from UTF-8, just as UTF-8 [engineers](https://utf-8000.jb2170.com/#sec-intuitive-derivation-engineer:~:text=the%20architecting%20of%20UTF-8%20was%20not%20purely%20a%20mathematics%20problem,%20but%20was%20also%20an%20engineering%20problem) an extension from ASCII?

We rule out a few alternatives in this section. It's good to document the suboptimal solutions (and outright failures) so that we can work towards success. I've done that plenty of times with my own ideas don't worry! Feel satisfied in having at least made an attempt.

ASCVI

What if ASCII were only six-bit instead of seven-bit? Would this make extending to multi-byte code units more pleasant?

### Source

Myself: The [intuitive derivation](https://utf-8000.jb2170.com/#sec-intuitive-derivation) section of UTF-8000.

### TLDR / Examples

|     |              |              |              |              |     |             |
| --- | ------------ | ------------ | ------------ | ------------ | --- | ----------- |
| 1   | `0 0 xxxxxx` |              |              |              |     |             |
| 2   | `0 10 xxxxx` | `1 x xxxxxx` |              |              |     |             |
| 3   | `0 110 xxxx` | `1 xx xxxxx` | `1 xxxxxxx`  |              |     |             |
| ... |              |              |              |              |     |             |
| 17  | `0 1111111`  | `1 1111111`  | `1 110 xxxx` | `1 xx xxxxx` | ... | `1 xxxxxxx` |
| ... |              |              |              |              |     |             |

### Properties

Self-synchronization, self-punctuation, `strcmp` ordering, BOM support, and arbitrary code unit length have the same conclusion as UTF-8000. Properties that differ are discussed below.

#### Bit Counts

The number of content bits and mandatory content bits are even more predictable than those of UTF-8.

| code unit length | number of content bits | number of mandatory content bits |
| ---------------- | ---------------------- | -------------------------------- |
| `n = 1`          | `6n ( = 6)`            | `0`                              |
| `n > 1`          | `6n`                   | `6`                              |

This is because one-byte code units are not special. They use the same `0` self-synchronization prefix as any first byte. The number 6 arises from every subsequent continuation byte adding on 7 more content bits, minus 1 for the longer start bits sequence.

Consequently the number of content bits stored in an `n` byte code unit is never a power of two, unlike with UTF-8000. This is because 6, containing 3 in its prime factorization, cannot divide into a power of two. This is not a terrible defect, but we do like powers of two.

#### Information Rate

ASCVI's information rate is `6n / 8n = 6 / 8 = 75%`. This a constant independent of the length of the code unit.

For one-byte code units UTF-8000 (ASCII) is more efficient and versatile, storing double the number of codepoints and having an information rate of `87.5%`.

For multi-byte code units ASCVI is more efficient, with UTF-8000's information rate tending downwards towards `62.5%`.

Even if the US English alphabet had its 52 letters cut down to eg 27 Hebrew glyphs, or no letters at all, one would struggle to create a practical set of 64 glyphs for single-byte ASCVI. The tradeoff of ASCII being seven-bit, at the slight detriment of the information rate of multi-byte code units, seems worth it.

#### Byte Map

|   | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | A | B | C | D | E  | F |
| - | - | - | - | - | - | - | - | - | - | - | - | - | - | - | -- | - |
| 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1  |   |
| 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1  |   |
| 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1  |   |
| 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1  |   |
| 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2  |   |
| 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2  |   |
| 3 | 3 | 3 | 3 | 3 | 3 | 3 | 3 | 3 | 3 | 3 | 3 | 3 | 3 | 3 | 3  |   |
| 4 | 4 | 4 | 4 | 4 | 4 | 4 | 4 | 5 | 5 | 5 | 5 | 6 | 6 | 7 | 8+ |   |
|   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |    |   |
|   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |    |   |
|   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |    |   |
|   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |    |   |
|   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |    |   |
|   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |    |   |
|   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |    |   |
|   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |    |   |

Look at that beautiful geometric series layout. Four rows for `1`, two rows for `2`, one row for `3`, half a row for `4` etc...

Unlike UTF-8000 which [can never use the bytes 0xC0 and 0xC1](https://utf-8000.jb2170.com/#sec-properties-byte-map:~:text=All%20bytes%20except%200xC0%20and%200xC1,can%20appear%20in%20a%20valid%20UTF-8000%20stream), ASCVI uses all 256 possible bytes. Which of these is the advantageous behavior depends on whether one wants to do something extraordinary with those two bytes, or one wants to dissuade their use in chicanery.

### Intuitive Derivation

See the [intuitive derivation of UTF-8000](https://utf-8000.jb2170.com/#sec-intuitive-derivation) for how one might come up with this.

### Naming

The II in ASCII reminds me of the Roman Numerals VII for seven, and ASCII is a seven-bit code. Therefore for this six-bit code we choose to use the Roman Numerals for six, VI, and name it ASCVI.

### Verdict

7 bit ASCII, and UTF-8 that extends it, are very well established. One-byte ASCVI is inferior to the flexibility of ASCII, albeit this contributes to UTF-8 having a slightly lower information rate for multi-byte code units. I do not yearn for an alternate universe, or a fresh start of text encoding standards, where ASCII is six-bit instead of seven.

That being said, ASCVI is by no means *inherently* flawed, and we can make use of it in [UTF-16K](https://utf-8000.jb2170.com/#sec-utf-16k) and [UTF-32K](https://utf-8000.jb2170.com/#sec-utf-32k). In a sense ASCVI is the Platonic Form of UTF-8.

Signed Variant: Two's Complement

One might be shocked to find our beloved two's complement representation of the signed integers present in the rejected ideas section. This is not about one's personal taste in representing signed integers, but technological extensibility, with which the [zigzag signed variant](https://utf-8000.jb2170.com/#sec-signed-variant-zigzag-encoding) far outshines this two's complement variant.

### Source

Myself: It seemed like a good(ish) idea until I realized that the zigzag variant is better.

### TLDR / Examples

We treat the `n` content bits of a code unit as a [two's complement](https://en.wikipedia.org/wiki/Two%27s_complement) signed form, where the highest bit no longer has value `2 ^ (n-1)` but rather `-2 ^ (n-1)`.

|          |               |              |             |             |             |
| -------- | ------------- | ------------ | ----------- | ----------- | ----------- |
| ASCII    |               |              |             |             |             |
| 1        | `0 xxxxxxx`   |              |             |             |             |
| UTF-8    |               |              |             |             |             |
| 2        | `11 0 x xxxx` | `10 xxxxxx`  |             |             |             |
| 3        | `11 10 x xxx` | `10 xx xxxx` | `10 xxxxxx` |             |             |
| 4        | `11 110 x xx` | `10 xxx xxx` | `10 xxxxxx` | `10 xxxxxx` |             |
| UTF-8000 |               |              |             |             |             |
| 5        | `11 1110 x x` | `10 xxxx xx` | `10 xxxxxx` | `10 xxxxxx` | `10 xxxxxx` |
| ...      |               |              |             |             |             |

The difference in code unit layout from UTF-8000 is that the mandatory content bits are downshifted by one bit.

### Properties

#### Overlong Encoding Checking

Preventing overlong encoding requires checking that the content bits of an `n`-byte code unit encode an integer `z` from the range `[ -2^(5n), +2^(5n) ) \ [ -2^(5(n-1)), +2^(5(n-1)) )`. This may look complicated, but we can break it down into two cases:

For `z ≥ 0`, the highest content bit of both `n`-byte and `n-1`-byte code units is `0`. There must be at least one `1` bit in the following bits, which are the mandatory content bits.

For `z < 0`, the highest content bit of both `n`-byte and `n-1`-byte code units is `1`. This is to make `z` negative, using the `-2^(5n)` bit. There must be at least one `0` bit in the following bits, which are the mandatory content bits. Otherwise if all of these bits were ones, then `z` would be at least `-2^(5(n-1))`. For example the overlong encoding `11 0 1 1111` `10 000000` encodes `z = -64` which fits into a 1-byte code unit. Encoding integers below `-64` requires subtracting from these content bits, which sets at least one of the mandatory content bits to zero.

UTF-8000 never uses the bytes 0xC0 and 0xC1, which is explained in the glossary section for [overlong encoding](https://utf-8000.jb2170.com/#def-overlong-encoding). Slightly differently, this two's complement signed variant never uses the bytes 0xC0 (`11 0 0 0000`) or 0xDF (`11 0 1 1111`).

#### No `strcmp(3)` Ordering

Since negative integers set the highest content bit to `1`, we lose [`strcmp`](https://utf-8000.jb2170.com/#sec-properties-strcmp-ordering) ordering from UTF-8000. For example -1 < 0 but `0 1111111` > `0 0000000`.

### Verdict

A big downside of this two's complement signed variant is that its anti-overlong checking mechanism differs from that of UTF-8000 because of the 1-bit-downshifted position of the mandatory content bits. Consequently it is not possible to agnostically decode a stream of these bytes as though they were UTF-8000 bytes. For example, a 0xC1 byte is valid in this variant as `11 0 0 0001`, but is invalid in UTF-8000 as `11 0 0000 1`.

What if we tried to redeem this variant by proposing to move the negative bit, the `-2^(5n)` bit, to the stable tail end of the code unit, rather than it being at the ever-expanding head of the code unit? This could hopefully mean that we would not have to change the anti-overlong checking mechanism from UTF-8000. The long and short is that we would perchance intuitively reinvent the [zigzag signed variant](https://utf-8000.jb2170.com/#sec-signed-variant-zigzag-encoding) from first principles, which indeed leftwards bitshifts by one the signed integer that it encodes, using the lowest bit as the negative bit. Our redemption is found there.

UTF-Infinity

What if we naively roll the start bits over into further bytes?

### Source

Mashpoe on YouTube: *[Expanding the UTF-8 Character Set to Infinity](https://www.youtube.com/watch?v=tAMDtH9uq1Y)*

### TLDR / Examples

|     |            |              |              |             |             |             |
| --- | ---------- | ------------ | ------------ | ----------- | ----------- | ----------- |
| ... |            |              |              |             |             |             |
| 7   | `11111110` | `10 xxxxx x` | `10 xxxxxx`  | ...         | `10 xxxxxx` |             |
| 8   | `11111111` | `0 xxxxxxx`  | `10 xxxxxx`  | ...         | `10 xxxxxx` |             |
| 9   | `11111111` | `10 xxxxx x` | `10 xxxxxx`  | ...         | `10 xxxxxx` |             |
| 10  | `11111111` | `110 xxxxx`  | `10 xxxxxx`  | ...         | `10 xxxxxx` |             |
| ... |            |              |              |             |             |             |
| 15  | `11111111` | `11111110`   | `10 xxxxx x` | `10 xxxxxx` | ...         | `10 xxxxxx` |
| 16  | `11111111` | `11111111`   | `0 xxxxxxx`  | `10 xxxxxx` | ...         | `10 xxxxxx` |
| 17  | `11111111` | `11111111`   | `10 xxxxx x` | `10 xxxxxx` | ...         | `10 xxxxxx` |
| 18  | `11111111` | `11111111`   | `110 xxxxx`  | `10 xxxxxx` | ...         | `10 xxxxxx` |
| ... |            |              |              |             |             |             |

### Properties

#### Bit Counts

Effectively, every jump from `8k-1`-byte code units to `8k`-byte code units the encoding inserts another blank byte after the start bytes, by which 8 minus 1 equals 7 bits of content are gained in an ASCII-looking byte, instead of appending a continuation byte by which 6 minus 1 equals 5 bits of content are gained.

| code unit length | number of content bits | number of mandatory content bits |
| ---------------- | ---------------------- | -------------------------------- |
| `n = 1`          | `7`                    | `0`                              |
| `n ≠ 8k`         | `5n+1 + 2⌊n/8⌋`        | `5`                              |
| `n = 8k`         | `5n+1 + 2⌊n/8⌋`        | `7`                              |

#### Information Rate

For an `n`-byte code unit the information rate is UTF-8000's information rate plus `2⌊n/8⌋ / (8n)`.

I'm not working it out fully, but I can tell that this leads to a sawtooth-y profile as a graph of information rate against `n`. Therefore, counterintuitively, longer code units can have better efficiency than shorter ones.

#### No Self-Synchronization

In the 8-byte code unit example, there is no way to distinguish the second byte `0 xxxxxxx` from an ASCII byte `0 xxxxxxx`. This generalizes to `8n`-byte code units.

In the 15-byte code unit example, there is no way to distinguish the second byte, `11111110` from the first byte of a 7-byte code unit. This generalizes to `8n-1`-byte code units.

In the 16-byte code unit example, there is no way to distinguish the second byte, `11111111` from the first byte of an 8-byte code unit. This generalizes such that if one seeks to any `11111111` byte, one has no idea if this is the first byte of a code unit or not.

This list is non-exhaustive.

#### Self-Punctuation

This is the property that Mashpoe clearly prioritized preserving, however the approach was too myopic and did not lead to preserving other properties of interest.

#### Patented

Mashpoe jokes (?) in the video that he owns the patent to this encoding scheme.

Regardless of whether he is joking or not, I nonetheless find it reprehensible that someone *could* (at least try to) copyright / patent the correct way to extend UTF-8. It would be like trying to copyright the right solution to a mathematics equation, or a prime number! Therefore I am being quite loud in the copylefting of UTF-8000 in the [licensing section](https://utf-8000.jb2170.com/#sec-licensing). Everyone benefits from shared, free-as-in-freedom, open ideas.

### Verdict

The loss of self-synchronization is a fatal detriment.

The formula for the number of content bits has predictable but irritable jumps, which lead to counterintuitive information rates.

Perl utf8

Use *up to* 7 bytes to encode up to 36 bits of information in the sane way, in order to encode 32-bit integers (and a little beyond). To encode 64-bit integers, use a special-case *fixed* 13-byte code unit starting with `11111111`.

Since the `n`-byte code units with `n < 8` are the same as UTF-8000 we shall mostly only discuss the 13-byte code units.

### Source

Larry Wall for Perl5 on GitHub: *[utf8.h](https://github.com/Perl/perl5/blob/8848c13d93120f501f8dc6f9ba087f53dc743993/utf8.h#L370)*

A comment reads: A note on nomenclature: The term UTF-8 is used loosely and inconsistently in Perl documentation ... perl uses an extension of UTF-8 to represent code points that Unicode considers illegal..

### TLDR / Examples

|     |             |              |              |             |             |             |             |             |     |             |
| --- | ----------- | ------------ | ------------ | ----------- | ----------- | ----------- | ----------- | ----------- | --- | ----------- |
| ... |             |              |              |             |             |             |             |             |     |             |
| 5   | `111110 xx` | `10 xxx xxx` | `10 xxxxxx`  | `10 xxxxxx` | `10 xxxxxx` |             |             |             |     |             |
| 6   | `1111110 x` | `10 xxxx xx` | `10 xxxxxx`  | `10 xxxxxx` | `10 xxxxxx` | `10 xxxxxx` |             |             |     |             |
| 7   | `11111110`  | `10 xxxxx x` | `10 xxxxxx`  | `10 xxxxxx` | `10 xxxxxx` | `10 xxxxxx` | `10 xxxxxx` |             |     |             |
| 13  | `11111111`  | `10 000000`  | `10 00 0xxx` | `10 xxxxxx` | `10 xxxxxx` | `10 xxxxxx` | `10 xxxxxx` | `10 xxxxxx` | ... | `10 xxxxxx` |

### Properties

#### Bit Counts

There are no code units of length 8, 9, 10, 11, or 12. Nor are there any of length 14 or beyond.

| code unit length | number of content bits  |
| ---------------- | ----------------------- |
| `n = 1`          | `7`                     |
| `1 < n < 8`      | `5n+1`                  |
| `n = 13`         | `63/64 used, up to 72?` |

#### Why 13 Bytes Instead Of 12?

Encoding the maximum possible signed 64-bit integer, 0x7FFF\_FFFF\_FFFF\_FFFF, in Perl utf8 returns `11111111` `10 000000` `10 00 0111` ... `10 111111`. Even if the maximum possible unsigned 64-bit integer, 0xFFFF\_FFFF\_FFFF\_FFFF, were encodable it could fit into the lower 11 bytes. So why use 13 bytes instead of 12, with the second byte always `10 000000`?

My guess is that the designers were going to use a [UTF-Infinity](https://utf-8000.jb2170.com/#sec-rejected-utf-infinity) style `11111111` `111110 00` opening, but then they recognized that they would lose self-synchronization because the second byte looks like the start of a 5-byte utf8 sequence. Therefore they swapped the second byte to a `10 000000`. They could have removed it and used 12 bytes, but that would preclude any future reunification with e.g. UTF-8000, which with 12 bytes has only `5 * 12 + 1 = 61` content bits which is less than 64, but with 13 bytes has `5 * 13 + 1 = 66` which is sufficient.

#### Information Rate

On the surface 13-byte, 72-bit code units have an information rate of `72 / (13*8) = 72 / 104 = 69%`, which is better than that of UTF-8000 (`62.5%`).

However as only 64 of those 72 bits are used in encoding 64 bit numbers, with the whole of the first continuation byte never being used, the information rate is closer to `64 / (13*8) = 64 / 104 = 61.5%`, which is worse than UTF-8000.

#### Self-Synchronization

This is effectively the same as UTF-8000. All continuation bytes have a `10` self-synchronization prefix, and the 13-byte start byte `11111111` has a `11` self-synchronization prefix.

#### Self-Punctuation

The first byte of a 13-byte code unit being `11111111` characterizes it as a special case, providing self-punctuation. This is similar to ASCII being a special case with its characterizing prefix of `0` in the highest bit.

#### Not Infinitely Extensible

Because Perl only supports up to 64-bit numbers without a specialized `bigint` module, it was sensible of them to cap their extension of UTF-8 to a finite number of bytes. It's not the prettiest however, and I'm not sure why they chose 13 bytes when 12 would suffice. CPU alignment if they don't store the predictable start byte of all 1s?

### Verdict

Inextensible, providing only one special case beyond 7-byte UTF-8 to encode 64-bit numbers, and is thus not widely known or supported.

UCS-X

UCS-X proposes three extensions for each of UTF-8, UTF-16, UTF-32, for a total of nine specifications, twelve including the existing base specifications!

I have so far only investigated the UTF-8 extensions, as they are all dense reads, and that is what we summarize in this section, with our main contribution being bitwise color highlighting.

At a glance the UTF-16 extensions look like they break syntax with base UTF-16, whereas our [UTF-16K](https://utf-8000.jb2170.com/#sec-utf-16k) proposal does not. Ours only semantically reinterprets the high surrogates `U+DB00` to `U+DB3F`. I will have a look at UCS-X's UTF-16 and UTF-32 extensions when I get time, to see if they contain anything interesting, or if I'm wrong.

### Source

Tom Bishop and Richard Cook on ucsx.org: *[The UCS-X Family of UCS Extensions (Draft Proposal)](https://ucsx.org/)*

| UTF-8                           | UTF-16                            | UTF-32                            |
| ------------------------------- | --------------------------------- | --------------------------------- |
| [UTF-G-8](https://ucsx.org/g8) | [UTF-G-16](https://ucsx.org/g16) | [UTF-G-32](https://ucsx.org/g32) |
| [UTF-E-8](https://ucsx.org/e8) | [UTF-E-16](https://ucsx.org/e16) | [UTF-E-32](https://ucsx.org/e32) |
| [UTF-∞-8](https://ucsx.org/∞8) | [UTF-∞-16](https://ucsx.org/∞16) | [UTF-∞-32](https://ucsx.org/∞32) |

### TLDR / Examples

#### UTF-G-8

The same as original 6-byte UTF-8 ([RFC 2279](https://datatracker.ietf.org/doc/html/rfc2279)) by Ken Thompson and Rob Pike, the same as [UTF-8000](https://utf-8000.jb2170.com/#sec-tldr).

|     |             |              |             |             |             |             |
| --- | ----------- | ------------ | ----------- | ----------- | ----------- | ----------- |
| ... |             |              |             |             |             |             |
| 5   | `111110 xx` | `10 xxx xxx` | `10 xxxxxx` | `10 xxxxxx` | `10 xxxxxx` |             |
| 6   | `1111110 x` | `10 xxxx xx` | `10 xxxxxx` | `10 xxxxxx` | `10 xxxxxx` | `10 xxxxxx` |

#### UTF-E-8

The same as [Perl utf8](https://utf-8000.jb2170.com/#sec-rejected-perl-utf8).

|     |            |              |              |             |             |             |             |             |     |             |
| --- | ---------- | ------------ | ------------ | ----------- | ----------- | ----------- | ----------- | ----------- | --- | ----------- |
| ... |            |              |              |             |             |             |             |             |     |             |
| 7   | `11111110` | `10 xxxxx x` | `10 xxxxxx`  | `10 xxxxxx` | `10 xxxxxx` | `10 xxxxxx` | `10 xxxxxx` |             |     |             |
| 13  | `11111111` | `10 000000`  | `10 00 0xxx` | `10 xxxxxx` | `10 xxxxxx` | `10 xxxxxx` | `10 xxxxxx` | `10 xxxxxx` | ... | `10 xxxxxx` |

#### UTF-∞-8

This extension's code units are best characterized by the number of hex digits that the `U+...XXXX` codepoint representation consists of. The idea is that by adding on two more continuation bytes, which contain 12 content bits, one can add three more hex digits to the `U+...XXXX` codepoint representation.

The 18 hex digit, 71 and 72 content bit cases are handled specially, in the transition region of extending from UTF-E-8.

Otherwise, to encode an integer N: Subtract 18 from the number of hex digits in the integer's `U+...XXX` codepoint representation. Store this number in one or more low length-storage bytes of the form `10 10 xxxx`. Precede these low length-storage bytes with the constant high length-storage bytes `10 110100` (0xB4), where the number of high length-storage bytes is one less than the number of low length-storage bytes. Now precede this with the constant full start byte `11111111`. Now succeed all of this with continuation bytes that store the content bits, of the form `10 xxxxxx`. These content bytes come in pairs, and the number of pairs should be one third of the number of hex digits, rounded up to the next integer if necessary.

| hex digits | content bits | bytes |            |              |              |              |              |              |              |              |             |     |             |
| ---------- | ------------ | ----- | ---------- | ------------ | ------------ | ------------ | ------------ | ------------ | ------------ | ------------ | ----------- | --- | ----------- |
| ...        |              |       |            |              |              |              |              |              |              |              |             |     |             |
| 18         | 71           | 13    | `11111111` | `10 0 xxxxx` | `10 xxx xxx` | `10 xxxxxx`  | ...          | `10 xxxxxx`  |              |              |             |     |             |
| 18         | 72           | 14    | `11111111` | `10 10 0000` | `10 1 xxxxx` | `10 xxxxxx`  | ...          | `10 xxxxxx`  |              |              |             |     |             |
|            |              |       |            |              |              |              |              |              |              |              |             |     |             |
| 19         | 76           | 16    | `11111111` | `10 10 0001` | `10 000000`  | `10 00xxxx`  | `10 xxxxxx`  | ...          | `10 xxxxxx`  |              |             |     |             |
| 20         | 80           | 16    | `11111111` | `10 10 0010` | `10 0000xx`  | `10 xx xxxx` | `10 xxxxxx`  | ...          | `10 xxxxxx`  |              |             |     |             |
| 21         | 84           | 16    | `11111111` | `10 10 0011` | `10 xxxx xx` | `10 xxxxxx`  | `10 xxxxxx`  | ...          | `10 xxxxxx`  |              |             |     |             |
| ...        |              |       |            |              |              |              |              |              |              |              |             |     |             |
| 31         | 124          | 24    | `11111111` | `10 10 1101` | `10 000000`  | `10 00xxxx`  | `10 xxxxxx`  | ...          | `10 xxxxxx`  |              |             |     |             |
| 32         | 128          | 24    | `11111111` | `10 10 1110` | `10 0000xx`  | `10 xx xxxx` | `10 xxxxxx`  | ...          | `10 xxxxxx`  |              |             |     |             |
| 33         | 132          | 24    | `11111111` | `10 10 1111` | `10 xxxx xx` | `10 xxxxxx`  | `10 xxxxxx`  | ...          | `10 xxxxxx`  |              |             |     |             |
|            |              |       |            |              |              |              |              |              |              |              |             |     |             |
| 34         | 136          | 28    | `11111111` | `10 110100`  | `10 10 0001` | `10 10 0000` | `10 000000`  | `10 00xxxx`  | `10 xxxxxx`  | ...          | `10 xxxxxx` |     |             |
| 35         | 140          | 28    | `11111111` | `10 110100`  | `10 10 0001` | `10 10 0001` | `10 0000xx`  | `10 xx xxxx` | `10 xxxxxx`  | ...          | `10 xxxxxx` |     |             |
| 36         | 144          | 28    | `11111111` | `10 110100`  | `10 10 0001` | `10 10 0010` | `10 xxxx xx` | `10 xxxxxx`  | `10 xxxxxx`  | ...          | `10 xxxxxx` |     |             |
| ...        |              |       |            |              |              |              |              |              |              |              |             |     |             |
| 271        | 1084         | 186   | `11111111` | `10 110100`  | `10 10 1111` | `10 10 1101` | `10 000000`  | `10 00xxxx`  | `10 xxxxxx`  | ...          | `10 xxxxxx` |     |             |
| 272        | 1088         | 186   | `11111111` | `10 110100`  | `10 10 1111` | `10 10 1110` | `10 0000xx`  | `10 xx xxxx` | `10 xxxxxx`  | ...          | `10 xxxxxx` |     |             |
| 273        | 1092         | 186   | `11111111` | `10 110100`  | `10 10 1111` | `10 10 1111` | `10 xxxx xx` | `10 xxxxxx`  | `10 xxxxxx`  | ...          | `10 xxxxxx` |     |             |
|            |              |       |            |              |              |              |              |              |              |              |             |     |             |
|            |              |       | `11111111` | `10 110100`  | `10 110100`  | `10 10 xxxx` | `10 10 xxxx` | `10 10 xxxx` | `10 000000`  | `10 00xxxx`  | `10 xxxxxx` | ... | `10 xxxxxx` |
|            |              |       | `11111111` | `10 110100`  | `10 110100`  | `10 10 xxxx` | `10 10 xxxx` | `10 10 xxxx` | `10 0000xx`  | `10 xx xxxx` | `10 xxxxxx` | ... | `10 xxxxxx` |
|            |              |       | `11111111` | `10 110100`  | `10 110100`  | `10 10 xxxx` | `10 10 xxxx` | `10 10 xxxx` | `10 xxxx xx` | `10 xxxxxx`  | `10 xxxxxx` | ... | `10 xxxxxx` |
| ...        |              |       |            |              |              |              |              |              |              |              |             |     |             |

The maximum integer that `n` low length-storage bytes can store is `16^n - 1`, which is always congruent to `0 mod3`. Thus the number of code units in each family of `n` low length-storage bytes is `(16^n - 1) - (16^(n-1) - 1)` which is also always congruent to `0 mod3`. In the base case of `n = 1`, the maximum low length-storage byte `10 10 1111` (33 hex digits) is succeeded by the bytes `10 xxxx xx` `10 xxxxxx`, case 3/3 of the repeating pattern of mandatory content bits placement in the highest two content bytes. Thus we conclude inductively that each family of code units with `n` low length-storage bytes ends the same way, with `10 10 1111` \[ `10 10 1111` ... \] `10 xxxx xx` `10 xxxxxx`. This ensures clean transitions from `n` to `n+1` low length-storage byte families.

### Properties

#### Bit Counts

| variant | code unit length | number of content bits | same as                                                           |
| ------- | ---------------- | ---------------------- | ----------------------------------------------------------------- |
| ASCII   | `n = 1`          | `7`                    | [UTF-8000](https://utf-8000.jb2170.com/#sec-tldr)                |
| UTF-8   | `2 ≤ n ≤ 4`      | `5n+1`                 | [UTF-8000](https://utf-8000.jb2170.com/#sec-tldr)                |
| UTF-G-8 | `5 ≤ n ≤ 6`      | `5n+1`                 | [UTF-8000](https://utf-8000.jb2170.com/#sec-tldr)                |
| UTF-E-8 | `n = 7`          | `5n+1 ( = 36)`         | [UTF-8000](https://utf-8000.jb2170.com/#sec-tldr)                |
| UTF-E-8 | `n = 13`         | `63`                   | [Perl utf8](https://utf-8000.jb2170.com/#sec-rejected-perl-utf8) |
| UTF-∞-8 | `n = 13`         | `71`                   |                                                                   |
| UTF-∞-8 | `n = 14`         | `72`                   |                                                                   |

and then further for UTF-∞-8:

| number of hex digits | number of content bits | code unit length                |
| -------------------- | ---------------------- | ------------------------------- |
| `n ≥ 19`             | `4n`                   | `2(⌊log16(n-18)⌋ + 1) + 2⌈n/3⌉` |

UTF-G-8 stores 31 bits, enough to encode positive signed 32-bit integers.

UTF-E-8 stores 63 bits, enough to encode positive signed 64-bit integers.

For UTF-∞-8 this is unlimited.

#### Information Rate

Asymptotically `4n / (8 * 2(⌊log16(n-18)⌋ + 1) + 2⌈n/3⌉)` tends towards `3/4 = 75%`, which is better than that of UTF-8000 (`62.5%`). That's the power of the low length-storage bytes `10 10 xxxx` using all possible combinations of bits, whereas UTF-8000's start bits use only unary codewords. The `3/4 = 6/8` is representative of the content bytes.

#### Self-Synchronization

Every byte beyond the first begins with the continuation prefix `10`, ensuring self-synchronization.

#### Self-Punctuation

The high length-storage bytes `10 110100` provide self-punctuation. They tell us to keep reading a stream for them until we reach a low length-storage byte.

#### Byte Map

I was going to create one of these, but then I realized that unlike UTF-8000, UTF-∞-8 reuses bytes depending on context. For example 0xB4 can be `10 110100` or `10 110100`, and 0xAX can be `10 10xxxx` or `10 10 xxxx`.

#### `strcmp(3)` Ordering

The choice of 13 byte code units being limited to 71 bits leads to a second byte of the form `10 0 xxxxx`. This, and the choice of low length-storage bytes being of the form `10 10 xxxx`, high length-storage bytes being `10 110100`, and the use of a unary-code-like sequence of high length-storage bytes for self-punctuation, means that UTF-∞-8 preserves `strcmp` ordering.

It seems that any `10 11xxxx` 0xBX byte could have been used for the high length-storage bytes, and 0xB4 before is just a [fun choice](<https://ucsx.org/∞8#:~:text=zero or more bytes whose value is B4 \(%22before%22\)>).

#### BOM Support

For the same reasons as [UTF-8000](https://utf-8000.jb2170.com/#sec-properties-bom-support), BOM support is maintained.

### Verdict

It works, but it's quite complicated. It took me an entire day to figure out how it works, and to calculate its stats. I much prefer the simplicity of UTF-8000.

The high length-storage bytes `10 110100` provide self-punctuation and `strcmp` support, but somehow feel wasteful, taking up eight bits each, and are exceptional, with none of the other 0xBX bytes being used in a similar way. That being said, asymptotically UTF-∞-8 has a better information rate than UTF-8000.

The mechanism of subtracting 18 from the number of hex digits and stuffing them into the low length-storage bytes reminds me a little of UTF-16, subtracting 0x10000 from the codepoint value and stuffing that into the surrogate bytes.

Owl's Corrected UTF-8

Owl suggests a corrected version of UTF-8 with many radical changes. Many of these are opinionated, such as removing most control codes from C0. Many are technical, such as precluding the concept of overlong encodings similarly to UTF-16, by an `n`-byte code unit decoding to the binary number stored in the content bits added to the upper bound of codepoint values from `n-1`-byte code units.

Even notwithstanding the established dominance of UTF-8, I still disagree with almost everything in the document. But, he does come the closest to discovering the structure of UTF-8000's code units.

### Source

Zachary Weinberg on Owl's Portfolio: *[Corrected UTF-8](https://www.owlfolio.org/development/corrected-utf-8/)*

### TLDR / Examples

|      |             |              |              |     |             |
| ---- | ----------- | ------------ | ------------ | --- | ----------- |
| ...  |             |              |              |     |             |
| 6    | `1111110 x` | `10 xxxx xx` | `10 xxxxxx`  | ... | `10 xxxxxx` |
| 7    | `11111110`  | `10 xxxxx x` | `10 xxxxxx`  | ... | `10 xxxxxx` |
| 8    | `11111111`  | `110 xxxxx`  | `10 xxxxxx`  | ... | `10 xxxxxx` |
| 9    | `11111111`  | `1110 xxxx`  | `10 x xxxxx` | ... | `10 xxxxxx` |
| ...? |             |              |              |     |             |

If Owl had specified the second-highest bit of his continuation start bytes to be a `0` instead of a `1` then he would have beat me to UTF-8000! So close, but so far.

### Properties

#### No Self-Synchronization

In the 8-byte code unit example, there is no way to distinguish the second byte `110 xxxxx` from the first byte of a 2-byte code unit `110 xxxx x`. This generalizes beyond just 8-byte code units. This specific example could also encode `110 00001` (0xC1), which UTF-8 cannot, which may trip UTF-8 compatibility stress-tests.

#### BOM Collision

Owl acknowledges that his extension may lead to issues with the UTF-16 BOM, as (presumably?) his extension permits `11111111` `11111110` (0xFF 0xFE), the little-endian UTF-16 BOM.

### Verdict

Owl acknowledges leaving that extension for the future with respect to going beyond the 6-byte old [RFC 2044](https://datatracker.ietf.org/doc/html/rfc2044) version of UTF-8, showing humility and acknowledging his design's flaws.

I don't wish to dunk on his document too hard, but I'm greatly relieved that he failed to derive the infinite extension mechanism. It's not just for my ego's sake, but because I do not wish for UTF-8(000) to be associated with all the other junk in his specification.

Do Nothing

Why bother publishing this now and making so much noise? As of [Unicode Version 17.0](https://www.unicode.org/versions/stats/charcountv17_0.html), September 9th 2025, only 299,448 of 1,114,112 (27%) codepoints have been designated.

> We choose to go to the Moon in this decade and do the other things, not because they are easy, but because they are hard, because that goal will serve to organize and measure the best of our energies and skills, because that challenge is one that we are willing to accept, one we are unwilling to postpone, and one we intend to win...
>
> \- [JFK](https://en.wikipedia.org/wiki/John_F._Kennedy), 35th President of the USA, [1962](https://en.wikipedia.org/wiki/We_choose_to_go_to_the_Moon).

- It is a great exercise in coding theory.
- Nobody else seems to have figured it out, as only worse rejected alternatives have been previously proposed.
- If we wait until we run out of codepoints, one of those rejected alternatives may be hastily implemented just because it already exists. Granted, compatibility with UTF-16 would be broken, and first codepoints with five and six byte UTF-8 representations as per [RFC 2044](https://datatracker.ietf.org/doc/html/rfc2044) could be satisfactory without needing UTF-8000.
- The current upper bound of `U+10FFFF` on codepoints is entirely due to UTF-16's maximum capacity. UTF-16 and `wchar_t` is legacy Windows tech. If the future demands a larger set of codepoints then we should allow ourselves to not be held back.
- Who doesn't like freedom, the ability to encode any integer (unsigned or signed) that we want to?
- I feel in charge of the intellectual property to an extent, and as such [I have copylefted it](https://utf-8000.jb2170.com/#sec-licensing:~:text=This%20belongs%20to%20everyone.%20Live%20Free%20or%20Die.), rather than allowing a tyrant to (re-)discover it and publish it on their restrictive terms.
- Fortune and glory. I figured this out *myself* in the era of the rise of AI. I'll settle for the credit lol.
- I will be submitting this work to 3b1b's [Summer of Mathematics Exposition 2026](https://some.3b1b.co/).

### Verdict

Publish.

Feedback

Feedback is welcome, by [email](mailto:email@jb2170.com) or on [GitHub](https://github.com/UTF-8000), if you have any improvements or questions. I plan on reaching out to people in phases to get the most UTF-proximal feedback first. Selected feedback may go in this section.

### Ken Thompson

Ken Thompson replied to my email. That's really cool! Here is the correspondence:

emails

```
Date: Jul 4, 2026, 1:13 AM
From: Jay Berry <>
To: Ken Thompson <>
Subject: I have extended UTF-8 infinitely!

Hi Ken,

I thought you might be interested to see how (infinitely) far one can
push UTF-8, without introducing any new special cases, and while
maintaining all properties like self-synchronization, `strcmp(3)`
ordering, n-byte multibyte code units having 5n+1 content bits, etc.

I have put a one-page document on my
[website](https://utf-8000.jb2170.com/) explaining the spec. The TLDR
section should be sufficient to see what's going on, splitting the
self-synchronization bits from the self-punctuation bits, and allowing
the self-punctuation bits to roll over into continuation bytes.

I have searched high and low on the internet to try to make sure that
I have not *re*discovered this, that I am not unduly taking credit for
it. It seems to be an original thought. I have also fairly analyzed a
few rejected alternatives but they all lose key properties.

Can I ask: Did you or Rob Pike or anyone else working on FSS-UTF /
UTF-8 intend for it to be *this* extensible / future-proof? You did a
really good job! At this rate it will still be around in many
centuries' time.

Happy Fourth of July! Consider this a 250th birthday gift from Great
Britain (if you'd not already thought of it while designing UTF-8 back
in the 90s lol).

Thanks,
Jay Berry

---

Date: Jul 16, 2026, 11:29 PM
From: Ken Thompson <>
To: Jay Berry <>
Subject: Re: I have extended UTF-8 infinitely!

your first 2 extensions (5 and 6 bytes) were clearly envisioned.
the standard (up to 4 bytes) was created to cover the size of
unicode. i thought any more description would be a waste of
paper. i think your extension from 7 to 8 bytes is a little hoaky.
i requires reading the whole string rather than "knowing" the
number of follow on bytes. so, i think the only thing new is the
7 byte version.

i appreciate the mail, but i really dont think it is useful. it is
like replacing ipv6 with ipv50.

---

Date: Jul 17, 2026, 11:13 PM
From: Jay Berry <>
To: Ken Thompson <>
Subject: Re: I have extended UTF-8 infinitely!

Hi Ken,

Thanks for the reply!

I agree with the 'ipv50' remark haha. Even if we exhaust the existing
1,112,064 possible Unicode codepoints, going back to your original
6-byte UTF-8 proposal yields over 2 billion codepoints (31 bits),
which would be sufficient for a long while, without needing
continuation-start bytes.

I'm submitting UTF-8000 to the 2026 [Summer of Math
Exposition](https://some.3b1b.co/). I think it's still worth sharing
if it inspires those interested in maths / computer science, even
though it may never be used in our lifetimes.

Do you mind if I include this email chain in the feedback section? I
decided to first ask the creator of UTF-8 (yourself), then the authors
of the alternatives that I've critiqued, then the general public.

Thanks,
Jay

---

Date: Jul 18, 2026, 5:41 AM
From: Ken Thompson <>
To: Jay Berry <>
Subject: Re: I have extended UTF-8 infinitely!

you can use the reply.
                
```

Only the [zigzag signed variant](https://utf-8000.jb2170.com/#sec-signed-variant-zigzag-encoding) requires reading the entire code unit (really the last bit of the last byte) to perform `strcmp` checking, not normal UTF-8000, but yes that's a good point that he's observed.

I have emailed the authors of the [rejected alternatives](https://utf-8000.jb2170.com/#sec-rejected-alternatives) that I've reviewed, to see what are their critiques of mine.

first email

```
Date: 2 Aug 2026, 16:14
From: Jay Berry <>
To: Mashpoe          (UTF-Infinity) <>,
    Larry Wall       (Perl utf8)    <>,
    Tom Bishop       (UCS-X)        <>,
    Richard Cook     (UCS-X)        <>,
    Zachary Weinberg (Owl)          <>
Subject: Unlimited UTF-8 | UTF-8000

Hi everyone!

I believe that I have discovered the "correct" way to extend UTF-8 infinitely,
without introducing any new special cases, and while maintaining all properties
like self-synchronization, self-punctuation, `strcmp(3)` ordering, n-byte multibyte
units having 5n+1 content bits, etc. I've codenamed it "UTF-8000" or "UTF-8K".

I have put a one-page document on my [website](https://utf-8000.jb2170.com/)
explaining the spec. The TLDR section should be sufficient to see what's going on,
splitting the self-synchronization bits from the self-punctuation bits, and
allowing the self-punctuation bits to roll over into continuation bytes.
Reference implementation in Python is available on
[GitHub](https://github.com/UTF-8000/UTF-8000-Python) which can be installed
with `$ pipx install UTF-8000`.

I noticed that each of you have attempted to extend UTF-8 in different ways,
and I have constructively reviewed each of them in the
[rejected alternatives](https://utf-8000.jb2170.com/#sec-rejected-alternatives)
section of my spec. I thought you might be interested / maybe you have some
feedback for mine.

- [UTF-Infinity](https://utf-8000.jb2170.com/#sec-rejected-utf-infinity)   by Mashpoe
- [Perl utf8](https://utf-8000.jb2170.com/#sec-rejected-perl-utf8)         by Larry Wall
- [UCS-X](https://utf-8000.jb2170.com/#sec-rejected-ucs-x)                 by Tom Bishop and Richard Cook
- [Owl's "Corrected" UTF-8](https://utf-8000.jb2170.com/#sec-rejected-owl) by Zachary Weinberg

I emailed Ken Thompson, creator of UTF-8 (and Unix!) to see what he thinks,
and I got a reply! The exchange is on the
[website](https://utf-8000.jb2170.com/#sec-feedback-ken-thompson). His remark
"it is like replacing ipv6 with ipv50" is funny to me and should set a
not-too-serious atmosphere for this whole discussion.

Nonetheless I still think that UTF-8000 is fun and educational anyways, an exercise
in coding theory, and I'll be submitting it to 3b1b's 2026
[Summer of Math Exposition](https://some.3b1b.co/). But first I thought that it
would be proper to email the people whose work I've reviewed.

Thanks,
Jay Berry
                
```

#### Zachary Weinberg (Owl)

emails

````
Date: 2 Aug 2026, 19:18
From: Zachary Weinberg <>
To: Jay Berry <>
Subject: Re: Unlimited UTF-8 | UTF-8000

On Sun, Aug 2, 2026, at 11:14 AM, Jay Berry wrote:
> I believe that I have discovered the "correct" way to extend UTF-8
> infinitely, without introducing any new special cases, and while
> maintaining all properties like self-synchronization, self-
> punctuation, `strcmp(3)` ordering, n-byte multibyte units having 5n+1
> content bits, etc. I've codenamed it "UTF-8000" or "UTF-8K".

Hey, thanks for reaching out.  I'm delighted to see that I am not the
only one fed up with the artificial limitation of UTF-8's encoding space
to match UTF-16. I may actually revise my proposal to adopt your trick
for preserving self-synchronization even when the start bits extend
past the end of the first byte.

I think you're not taking the value of *eliminating* overlength
encodings seriously enough, though.  Yeah, that's the most complicated
part of my proposal, and the part that means Corrected UTF-8 doesn't
correspond to IETF UTF-8 for anything but the ASCII page, but it's also
the part that means Corrected UTF-8 decoders *cannot* be a vehicle for
path-smuggling attacks on network services, and therefore I consider it
second in importance only to lifting the artificial plane limit.
Making it impossible to encode surrogates is also important for
security reasons; the only way I could be persuaded to not do that is
if there was any chance that the surrogates might get *reassigned* as
ordinary characters in a couple decades, once UTF-16 is truly dead and
buried ... and I think the odds of that ever happening are far lower
than the odds of the Unicode Consortium backing down on their "never will
there be more than 17 planes" policy.

I've mostly come around to agree with you on the C1 controls, though.
Omitting them doesn't really help anything.  At the time I wrote the
original document (some years before I posted it on my website) I was
still working at a browser company and mislabeled or mistranscoded
Windows-1252 was a regular headache; but I have the impression that
has become much less common over the past decade and a half, and
"this can represent any Unicode codepoint with an official non-surrogate
assignment" *is* a desirable property for anything calling itself an UTF.

zw

---

Date: 3 Aug 2026, 23:34
From: Jay Berry <>
To: Zachary Weinberg <>
Subject: Re: Unlimited UTF-8 | UTF-8000

Hi Zack,

Thanks for the reply!

> I may actually revise my proposal to adopt your trick
> for preserving self-synchronization even when the start bits extend
> past the end of the first byte.

Self-synchronization was indeed a main feature that Ken Thompson figured out
in fixing FSS-UTF:

```
0vvvvvvv
10vvvvvv 1vvvvvvv
110vvvvv 1vvvvvvv 1vvvvvvv
...
```

(in which one couldn't tell the difference between eg a 2-byte start byte
`10|vvvvvv` and a continuation byte `1|0vvvvvv`) to UTF-8:

```
0vvvvvvv
110vvvvv 10vvvvvv
1110vvvv 10vvvvvv 10vvvvvv
...
```

in which the self-synchronization prefixes `0`, `10`, and `11` are distinct.
[History of FSS-UTF -> UTF-8](https://www.cl.cam.ac.uk/~mgk25/ucs/utf-8-history.txt
#:~:text=10zzzzzz%201yyyyyyy). It is definitely well worth keeping :)

> I think you're not taking the value of *eliminating* overlength
> encodings seriously enough, though.

Having n-byte (modified) UTF-8 decode to (value of content bits) plus
(1 more than maximum that (n-1)-byte UTF-8 can encode) i.e. the offsets in
your specification, sounds good at first, but if n is big then the offset accrues:
2^7 + 2^11 + 2^16 + ... + 2^(5(n-1)+1). We can write this as
2^7 + 2^11 * ((2^5)^0 + (2^5)^1 + ... + (2^5)^(n-3)) as a geometric series and
explicitly compute it as 2^7 + 2^11 * (32^(n-2) - 1) / 31 for n >= 3,
but that division is a bit 'icky' compared to addition subtraction multiplication
and bitshifting.

```py
def offset(n: int) -> int:
    if n == 1:
        return 0
    elif n == 2:
        return (1 << 7)
    else:
        return (1 << 7) + (1 << 11) * ((1 << (5 * (n - 2))) // 31)
```

It seems a lot easier to say "n-byte multibyte UTF-8 can store up to
(5n+1)-bit codepoints", a nice instance being 3-byte UTF-8 storing 16 bits,
1 2 and 3 byte UTF-8 exactly covering
[Plane 0](https://en.wikipedia.org/wiki/Plane_(Unicode)) of Unicode.

[UTF-1](https://en.wikipedia.org/wiki/UTF-1) was an earlier encoding that
Ken Thompson and Rob Pike tried out
([interview](https://www.youtube.com/watch?v=OmVHkL0IWk4&t=14275s)). It used
`mod 190` and divisions which they disliked, and eventually FSS-UTF and UTF-8
came around which use simple bitwise operations to check against overlong encodings,
and to extract the content bits. I think the anti-overlong checking is not too
complicated, 2-byte UTF-8 being the only odd one out.

Unicode offered a way forward from the ISO 8859-{1..16} diaspora of 8-bit codepages.
UTF-8 offered ASCII forwards compatibility with better efficiency than UTF-16 using
byte-precision rather than word-precision. I don't think that your offset-based
UTF-8 offers a significant upgrade. It's really just to protect noob software
developers who might write broken decoders for UTF-8 that don't do anti-overlong
checking and surrogate range checking.

> any chance that the surrogates might get *reassigned* as
> ordinary characters in a couple decades

My guess is that regardless of whether UTF-16 continues to live on, the
surrogate range will remain unencodable, due to pre-established UTF-8 parsers
rejecting them. Though I could be wrong, as iirc IP addresses that ended in `.0`
were originally not allowed, but now are. It does make one wonder what those
2048 codepoints could be assigned to...

> I've mostly come around to agree with you on the C1 controls, though.

That's great. I don't think I've ever actively used them, but yeah C1 should be
in / remain in Unicode as a way to refer to it using codepoints. It's a bit of a
shame that they don't (yet) have a 'control pictures' block like
[C0 Does](https://www.compart.com/en/unicode/block/U+2400).

> a regular headache

On a similar note there's still some software that is fiddly with them. I've got
a [bug to fix](https://github.com/jb2170/better-adb-sync/issues/42) that I've
figured out the solution to whilst messing with UTF-8(000) and encodings.
Android's Toybox's `ls` outputs U+0080 to U+009F 'C1 control codes' and
U+00A0 'non-breaking space' differently depending on whether `ls` is running over
`adb` interactively or not: interactively U+00A0 prints as `\240` octal-escape style,
but non-interactively it prints as the byte `a0`, which is either a coincidentally
decapitated UTF-8 unit `c2 a0`, or the raw ISO-8859-1 8-bit byte. The fix is to
use the `-b` flag on `ls` to force an escaped style. So tldr I understand the
fiddly-ness lol.

Thanks,
Jay
                
````

#### Tom Bishop (UCS-X)

emails

```
Date: 2 Aug 2026, 19:41
From: Thomas Eugene Bishop <>
To: All <>
Subject: Re: Unlimited UTF-8 | UTF-8000

Hi Jay,

Thanks for letting me know about your work, and the others you reference. It's
good to know that others are interested in extending the range of encoding.

I'll study the proposals more when I have time. Based on first impressions, I
have these comments.

About "the 'correct' way": maybe you mean that ironically and recognize there's
more than one way to do it, with trade-offs. On the other hand, you wrote,
"Nobody else seems to have figured it out, as only worse rejected alternatives
have been previously proposed." That sounds like an unwarranted claim that
you've solved a problem nobody else was able to solve. I wish you wouldn't use
the word "rejected" to describe alternatives, since it might be misconstrued
(maybe through an AI search) as implying a decision by an organization with some
capacity to accept or reject proposals. I think what you mean is that you
personally prefer your own proposal.

Now that multiple solutions exist, there's room to compare them by various
criteria such as efficiency, simplicity, and robustness.

You described Larry Wall's utf8 as "inextensible"; that's wrong, as proved by
its extension to UTF-∞-8. Or else, "inextensible" doesn't mean what I think it
means. Also, my understanding is that the contrast between "utf8" and "UTF-8"
was intentional.

You wrote, "UCS-X proposes three extensions for each of UTF-8, UTF-16, UTF-32,
for a total of nine specifications, twelve including the existing base
specifications!" and "it's quite complicated". I think this reference to 12
specs is an unfair criticism. The existence of multiple specs doesn't imply
complexity of the encodings themselves. The complication of the existing 3 base
specs is obviously beyond anybody's control at this point. Of the remaining 9,
you can ignore 6 if you want, since they are merely simplifications of the last
3; that is, the specs with max U+7FFFFFFF and U+7FFFFFFFFFFFFFFF are just
subsets of the specs with max infinity. We separated them out to support
implementers who might have good reasons not to go straight to infinity.

You wrote, "At a glance the UTF-16 extensions look like they break syntax with
base UTF-16, ...". I don't know what you mean by "break syntax", but UTF-∞-16 is
a compatible extension of UTF-16 in the sense that our spec defines "compatible
extension". It would be more responsible to postpone publishing a "break syntax"
assertion until you're certain and ready to explain what you mean by it.

You wrote, "... UTF-8000 code units can be arbitrarily large" -- I think you
mean UTF-8000 codes can be arbitrarily large. A UTF-8000 code unit is always 8
bits, right?

To me, while the technical details of encoding are interesting, what's more
interesting is how people might eventually use extended encodings, such as to
define their own characters and use them for public communication, without
having to wait for official approval of each character.

Best wishes,

Tom

---

Date: 3 Aug 2026, 16:57
From: Thomas Eugene Bishop <>
To: All <>
Subject: Re: Unlimited UTF-8 | UTF-8000

Hi Jay,

I wrote a script to compare the lengths of UTF-8000 and UTF-∞-8 codes, and also
their "start" bytes. That script isn't thoroughly tested and it might be only
approximate especially in some edge cases. With that disclaimer, it seems that
if a USV has 47 or more digits, then UTF-8000 is longer than UTF-∞-8. If a USV
has 18 or more digits, the number of "start bytes" (needed to determine the
length of an entire code) is longer for UTF-8000 than for UTF-∞-8. For a USV
with 128 digits, UTF-8000 has 102 total bytes and 17 start bytes, while UTF-∞-8
has 90 total bytes and 4 start bytes. UTF-8000 does have shorter codes in some
ranges, such as for USV with 10-15 digits.

Neither solution is optimal in terms of storage size. There are trade-offs such
as speed of execution, simplicity, robustness, etc.

The number of start bytes might be important in situations where text is read
into a fixed-size buffer and a buffer might contain a partial code. Software
should be able to determine the length of an entire code by scanning a
relatively small number of start bytes, both for efficiency and to avoid bugs in
cases where one code might span many buffers. This is an example of
"robustness". Another example is that protocols should enable processes to
indicate max supported USV.

The term "code unit" has a standard definition
(https://unicode.org/glossary/#code_unit) that differs from yours
(https://utf-8000.jb2170.com/#def-code-unit). I recommend following the standard
to avoid confusion.

It's wonderful that you might bring up this topic at the Summer of Math Exposition!

Cheers,

Tom

---

Date: 8 Aug 2026, 21:26
From: Jay Berry <>
To: Thomas Eugene Bishop <>
Subject: Re: Unlimited UTF-8 | UTF-8000

Hi Tom,

Thanks for the feedback!

> About "the 'correct' way": maybe you mean that ironically and recognize
> there's more than one way to do it, with trade-offs.

There are indeed other solutions such as UTF-∞-8 which preserve all properties
like self-synchronization, self-punctuation, strcmp order etc. However the
reason I've referred to it as the 'correct' way is because in my opinion it
looks like the 'natural' way to extend UTF-8, as I put in the [properties]
section addressing the fact that the anti-overlong mechanism works the same as
UTF-8, with no new special cases. I find it very simple to explain (in
retrospect) to begin with bytes endowed with self-synchronization prefixes '11'
and '10', and to stripe the self-punctuation bits across them.

> you wrote, "Nobody else seems to have figured it out, as only worse rejected
> alternatives have been previously proposed."

By that I mean that nobody else online has suggested the exact layout that
UTF-8000 proposes, which I feel is the 'natural' / 'correct' one, formally
identifying the self-synchronization and self-punctuation bits and how to use
them. The verdicts on the other proposals summarize their flaws, all but UTF-∞-8
losing key properties of interest.

> I wish you wouldn't use the word "rejected" ... implying a decision by an
> organization with some capacity to accept or reject proposals

I styled my document a bit like a [Python PEP], in which often the alternatives
have to be firmly disproven. That being said, yes I don't think I've made it
clear that this is a *proposal*, not an existing standard. In the Python
reference implementation [readme] I added the line "UTF-8000 is in no way
endorsed by or representative of the Unicode Consortium. This is a standalone
project.". I think I'll copy that to the header of the website, thanks!

> You described Larry Wall's utf8 as "inextensible"

I know it looks like I'm contradicting myself 10 seconds later by pointing out
that UCS-X extends from utf8, but what I meant is that Perl utf8 *on its own* is
designed only to go up to 2^63-1. It uses the `FF` byte to start its 13-byte
units and doesn't specify how one could continue onwards. I also don't feel the
need for UTF-8000 to extend utf8 like UCS-X does, as utf8 is not used outside
Perl, and we have the opportunity to make UTF-8000 more flexible allowing
8,9,10,11,12 byte units with 'correct' self-punctuation syntax (whereas utf8's
second byte is just a plain 0x80).

> Also, my understanding is that the contrast between "utf8" and "UTF-8" was intentional.

Yeah I'll remove that line about "utf8" vs "UTF-8", thanks. I know that the
Unicode Consortium is pedantic with referring to 'UTF-8' using a hyphen, and I
originally thought that Perl was just being a bit loose with the naming. It is
more likely that 'utf8' was chosen to show that it's not *exactly* 'UTF-8', like
I'm using 'UTF-8000' as a codename for my proposal.

> I think this reference to 12 specs is an unfair criticism.  The existence of
> multiple specs doesn't imply complexity of the encodings themselves.  We
> separated them out to support implementers who might have good reasons not to
> go straight to infinity.

We can group UTF-8 and UTF-G-8 together since they both follow the same style,
and 5/6-byte UTF-8 was envisioned by Ken Thompson. As for the UTF-E-8 and
UTF-∞-8 specifications, they are very different.

I think that UTF-8000, which is just one specification, within which there are
'natural ranges' (ie limiting to n-byte units) is a better approach. My original
specification for UTF-16K was going to use just one Unicode Plane, to provide
decent efficiency but without being too greedy in needing to claim existing
Unicode codepoints. But then I realised that this would provide 14k+1 content
bits, whereas if we used two planes instead of one, this would be 15k+1 content
bits, which overlaps nicely with 5n+1 provided by UTF-8000. So I have taken some
thought and care as to create 'ranges' like your 'Giga', 'Exa', 'Inf' ideas,
with which UTF-8 and UTF-16 can be expanded in parallel. I put this in the
[UTF-16K spec]. I think it's a lot easier to say "this is what n-byte UTF-8 and
k-surrogate-pair UTF-16 looks like. restrict to n=3k and you have ranges that
encode the same codepoints" than to have a patchwork of different standards
based on what range a codepoint is in, like UCS-X eg includes Perl utf8 as
UTF-E-8.

> I don't know what you mean by "break syntax", but UTF-∞-16 is a compatible extension

By 'compatible' I'm thinking along the lines of backwards compatibility "will
this throw an error in a UTF-8 / UTF-16 parser?" and "are we maintaining the
pre-established syntax?".

For UTF-8, technically one could argue that UTF-8000 and UTF-∞-8 "break syntax"
by eg using the byte 'FF', which when fed into a UTF-8 parser will cause an
exception. However on the other hand the byte 'FF' causing an exception is only
due to the restriction to U+10FFFF on the range of codepoints for UTF-8,
provided one's UTF-8 extension uses the byte 'FF'. So yes I'm being a bit
hypocritical, but I feel fine with that because bytes F{5..F} are currently
unused by UTF-8, and the proposed syntax of UTF-8000 is predictably the same as
UTF-8, eg wrt self-synchronization prefixes for non-ASCII first bytes being
'11', and for continuation bytes being '10'.

For UTF-16, every 16-bit word has already been used. Instead of changing the
syntax to use eg 1 high surrogate and (n-1) low surrogates, or like UTF-G-16 use
n low surrogates, I decided to use a "semantic reinterpretation" layer on top of
UTF-16, ASCVI-on-UTF-16. Ie, just as UTF-16 is a semantic reinterpretation of
UCS-2, interpreting codepoints in the ranges U+D800 to U+DBFF and U+DC00 to
U+DFFF no longer as those individual codepoint values, but rather as parts of
surrogate pairs, so too I decided to implement UTF-16K as a semantic
reinterpretation of plane 9 and 10 surrogate pairs. The nice thing about this is
that a decoder which only understands UTF-16 can open a UTF-16K encoded file,
just as a UCS-2 decoder can open UTF-16 files. Plane 9 and 10 surrogate pairs
would be displayed as UTF-16 codepoints rather than as one UTF-16K codepoint,
just as a UCS-2 parser would show two surrogate codepoints instead of one UTF-16
codepoint; semantic errors rather than syntax errors. Contrast that with
UTF-G-16, U+110000 encoded as 'DC04 DE80 DE00', with which the opening word may
immediately raise an exception in a UTF-16 parser.

For UTF-G-16, for ill-formed units, I am able to generate context-dependent
error handling behavior which leads to errors being decoded as though they are
correct. I am able to cause a contradiction in your UTF-G-16 [decoding rules]:
make 'DC04' both preceded by D800 (to make it trailing) and succeeded by DE80
(to make it leading). If we were to seek to the point 'X' in a stream 'X D800 Y
DC04 DE80 DE00' we would decode this as 'U+10004 (D800 DC04) U+FFFD (replace
DE80) U+FFFD (replace DE00)'. If we were to seek to the point 'Y' we would
decode this as 'U+110000 (DC04 DE80 DE00)', using the low surrogate 'DC04' and
leaving the high surrogate 'D800' before the seek point Y. This looks like bad
behavior. In UTF-8 and UTF-8000 because the first-byte and continuation-byte
self-synchronization prefixes make their byte ranges disjoint, I don't think a
situation like this can happen there. Ie never will a 'well formed unit X
followed by errors' be incorrectly decoded as a 'well formed unit Y with perhaps
some junk before it' if one seeks to the middle of the well formed unit 'X'. So
too UTF-16K keeps the {high surrogate | low surrogate} and {first surrogate pair
(plane 9) | continuation surrogate pair (plane 10)} ranges disjoint which avoids
this issue and maintains self-synchronization at the word-level. UTF-G-16
muddies the water by 'DC04' being trailing (UTF-16 surrogate pair) or leading
(UTF-G-16 leading) dependent on previous words. This is also why a UTF-8 /
UTF-8000 parser only ever needs to seek *forwards* to the next first byte if it
encounters an error.

~~For UTF-G-16, for well formed units, something still doesn't feel right that
one might need to look backwards to determine whether eg 'DC04' is trailing or
leading. We do not always have backwards seeking, like on a pipe or socket, or
at least we don't want to do backtracking like complicated regexes sometimes
do.~~ In well formed units we know exactly one of those conditions will be true
and we can look forwards rather than back, right? This seems like minutiae
compared to the behaviour in the previous paragraph.

Back to UTF-8, this conversation has made me realize that one could implement
*private-use extensions* on top of Unicode / UTF-8 using ASCVI-on-UTF-8, in a
similar way to UTF-16K using ASCVI-on-UTF-16. We can achieve an ASCVI-like code
in as little as 3 bits, 8 codepoints:

0: 000, 1: 001, 2: 010 110, 3: 010 111, 4: 011 101 100, 5: 011 101 101,
6: 011 101 110, 7: 011 101 111, 8: 011 110 110 100, 9: 011 110 110 101, ...

though using more bits will of course lead to more efficient codes. The
advantage of this style is that it's just a semantic reinterpretation layer on
top of UTF-8, and will pass right through a UTF-8 parser okay. A good range of
codepoints to use may be some of the U+E000 to U+F8FF Plane 0 private-use
codepoints. This seems like a great way in which one could create their own
autonomous set of 'MyUnicode' codepoints M+...XXXX starting at M+0000,
MyUnicode-on-Unicode style (as opposed to UTF-16K which uses the *public*
Unicode range and postulates starting at U+110000). This would answer your
point:

> what's more interesting is how people might eventually use extended encodings,
> such as to define their own characters and use them for public communication,
> without having to wait for official approval of each character

It does somewhat go against the spirit of "Uni"code, which is the one-and-only
'flat' layer of codepoints, to use an ASCVI layer on top of Unicode / UTF-8. One
can also imagine ASCVI-on-(ASCVI-on-UTF-8) if the M+...XXXX codepoints had their
*own* private-use area which allowed further sub-encoding. It's a fun thought to
think of trees of Unicode embedded recursively as layers on top of each other,
but it would surely be a bit anarchic and low-efficiency. Therefore my main
focus with UTF-8000 and UTF-16K has been on how *Unicode* could expand in the
long run. The private-use extensions do sound fun, but may be a bit clunky when
decoded in a programming language, being interspersed in 'normal' Unicode
strings.

> I wrote a script to compare the lengths of UTF-8000 and UTF-∞-8 codes, and
> also their "start" bytes.

Yes UTF-∞-8 has shorter units in the long run, an efficiency tending towards 6/8
whereas UTF-8000's efficiency tends towards 5/8. It's probably easiest to point
to UTF-8000 using a linear number of self-punctuation bits (n-1) -> O(n),
whereas UTF-∞-8 is roughly logarithmic O(log_2(n)).

> Software should be able to determine the length of an entire code by scanning
> a relatively small number of start bytes, both for efficiency and to avoid
> bugs in cases where one code might span many buffers.

This is a good point, and UTF-∞-8 is more succinct with respect to
self-punctuation. For 33 hex-digit codepoints, UTF-∞-8 uses 2 bytes, whereas
UTF-8000 uses 5, for 273 hex-digits UTF-∞-8 uses 4 bytes, whereas UTF-8000 uses
37! Mogs me.

and finally

> The term "code unit" has a standard definition that differs from yours. I
> recommend following the standard to avoid confusion.

I realised this half way through writing the UTF-8000 spec and I'm struggling to
think of an alternative name. I opened a GitHub [issue] to remind me to rename
it. 😅

So to conclude so far:

- I still think that UTF-8000 is simpler to explain and more predictable than UTF-∞-8
- UTF-∞-8 is asymptotically more efficient than UTF-8000 and requires less start
  bytes (self-punctuation bytes)
- UTF-G-16 (and beyond?) looks broken to me, though I haven't properly anatomized
  the UTF-X-16 family of UCS-X proposals like I have for the UTF-X-8 family.
- ASCVI-on-private-use-UTF-8 sounds like an okay idea for private-use extensions
  if they require a large amount of 'codepoints' (sub-encoded virtual
  my-codepoints M+...XXXX)
- I have a few remarks to change on my proposal

This has been a fun project! Thanks for the emails,
Jay
                
```

I have updated the UTF-16K specification [to mention](https://utf-8000.jb2170.com/#sec-utf-16k:~:text=UTF%2DG%2D16%20also%20has,10.) UCS-X's UTF-G-16's flawed error handling behavior.

### SoME 2026

I am submitting this work to 3b1b's [Summer of Mathematics Exposition 2026](https://some.3b1b.co/). I hope that it is useful to some people who view it, that it is educational about coding theory, and that maybe there'll be some feedback.

### General

I'll put a link to this page on [r/Unicode](https://www.reddit.com/r/Unicode/). There's lots of show-and-tell on there.

### Unicode Consortium

I *might* send this to the Unicode Consortium if there is good consensus from the feedback above.

But as Ken put it we don't really need IPv50 or unlimited UTF-8 right now, so I don't want to pester the Unicode Consortium when they're busy doing actually important jobs like documenting scripts, assigning codepoints, helping internationalization, etc.

Maybe this specification can sit in a 250-year time capsule in the Unicode Consortium Archives for when the time is right to expand...

Reference Implementation and Tools

### UTF-8000

Working reference implementation in Python with comprehensive code documentation is available on GitHub as [UTF-8000/UTF-8000-Python](https://github.com/UTF-8000/UTF-8000-Python). It can be installed as a PyPI package using `$ pipx install UTF-8000` which provides the command line utility `utf-8000(1)`.

The `$ utf-8000 info` subcommand displays info about a codepoint encoded in UTF-8000, with useful bit highlighting.

![](https://utf-8000.jb2170.com/utf-8000-info-example.png)

The `$ utf-8000 encode` subcommand reads codepoints from stdin and writes the raw UTF-8000 bytes to stdout.

The `$ utf-8000 decode` subcommand reads UTF-8000 bytes from stdin and feeds them to an incremental decoder, writing the decoded codepoints to stdout.

### UTF-16K

Working reference implementation for [UTF-16K](https://utf-8000.jb2170.com/#sec-utf-16k) is also available on GitHub as [UTF-8000/UTF-16K-Python](https://github.com/UTF-8000/UTF-16K-Python). It can be installed using `$ pipx install UTF-16K` which provides `utf-16k(1)` with the same subcommands as `utf-8000(1)`.

Naming

In the development phase of this project I have been using the codename UTF-8000, but I find myself increasingly drawn to UTF-8K.

Below is a comparison of different potential names, and I am open to suggestions.

### UTF-8000

Inspired by Python 3's development codenames in [PEP 3000](https://peps.python.org/pep-3000/#naming).

#### Pros

- The thousand in UTF eight thousand sounds big and futuristic. This encoding scheme should also last forever!

#### Cons

- The zeros are repetitive.
- Having to remember exactly three zeros to make 8000. Some people may read it as eight hundred, or eighty thousand etc.
- The inspiration logic doesn't exactly match up with Python because Python was moving from Python 2 to Python 3, not Python 3 to Python 3000, whereas we're going from UTF-8 to UTF-8000.
- UTF-8 is a prefix of UTF-8000. Existing software which parses a string representing the encoding's name to determine the encoding may do something like `if encoding_name[:5] == "UTF-8"` and incorrectly short-circuit. It is for this reason that Microsoft skipped from Windows 8 to Windows 10, not creating Windows 9, because existing software may check for Windows 9 to test for Windows 95 or Windows 98.

### UTF-8K

Inspired by another of Python 3's development codenames Py3K / Py3k, we could use UTF-8K, the K capitalized like the UTF. It's also shorter than 8000.

#### Pros

- Short; just one more letter than UTF-8.

#### Cons

- UTF-8 is a prefix of UTF-8K. See [here](https://utf-8000.jb2170.com/#prefix-con).

### UTF-8 & Knuckles

The K in UTF-8K reminds me of Sonic 3 & Knuckles, sometimes abbreviated to S3K.

The Sonic & Knuckles cartridge uses [lock-on technology](https://sonic.fandom.com/wiki/Lock-on_technology) to extend Sonic 3 and Sonic & Knuckles into Sonic 3 & Knuckles. In a similar way UTF-8 extends from (locks on to) ASCII, and UTF-8000 continues this extension.

#### Pros

- [Knuckles](https://en.wikipedia.org/wiki/Knuckles_the_Echidna) is cool.

#### Cons

- SEGA might not be happy, though they do seem nicer than Nintendo with respect to fanart.
- The hardest work of extending from ASCII to UTF-8 has already been achieved by Ken Thompson and Rob Pike. Is UTF-8000 lock-on if it doesn't introduce any new special cases? Not really.
- UTF-8 is a prefix of UTF-8 & Knuckles. See [here](https://utf-8000.jb2170.com/#prefix-con).
- This is just a bit of a joke-y name.

### VTF-8

Short for Variable Transformation Format 8.

#### Pros

- Removes the association with Unicode as VTF-8 is just a method of storing unsigned integers.
- UTF-8 is not a prefix of VTF-8; in fact they don't even begin with the same letter.
- The V looks Romanesque and fits with the [logo](https://utf-8000.jb2170.com/#sec-logo).

#### Cons

- The V in VTF-8 looks very similar to the U in UTF-8. At a glance, and depending on font rendering, one may not notice the difference.

### STF-8 for the Signed Variant

The U in UTF-8 could also be read as Unsigned, ie Unsigned Transformation Format 8. Thus we could take inspiration and write STF-8 for Signed Transformation Format 8.

Logo

UTF-8 does not have a logo. I had a bit of fun designing a logo for UTF-8000. I made it Romanesque but simple.

It's an eagle with UTF written across its wings and chest. The Roman numerals for 8, VIII, flank its head.

Below its claws it holds a fasces, wrapped with a continuation byte of the form `10 xxxxxx`. This represents the united strength of a bundle of continuation bytes, which makes us unstoppable in conquering the entire integers, in the name of including them into Unicode codepoints, encoded in UTF-8000.

The central fascis which sticks out represents the first byte of a code unit. This byte looks different from its continuation bytes, and has the power to start a code unit, which is represented by the wielding of the axe, sticking out on the left. On the right end the central fascis sticks out perhaps representing the terminating 0 of the start bit sequence.

![](https://utf-8000.jb2170.com/logo.blanchedalmond.png)

The favicon for this website is just the Roman numerals VIII.

![](https://utf-8000.jb2170.com/favicon.ico)

The Git repo containing these images is available on GitHub as [UTF-8000/UTF-8000-Images](https://github.com/UTF-8000/UTF-8000-Images).

Licensing / Copyright (Copyleft)

As creator of UTF-8000 I want the liberty with which UTF-8000 (the algorithm) can be used to be no less permissible than UTF-8 and ASCII before it. This belongs to everyone. Live Free or Die.

This website is licensed under [CC-BY-NC-SA-4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/), available on GitHub as [UTF-8000/UTF-8000-Website](https://github.com/UTF-8000/UTF-8000-Website).

The logo / images are licensed under [CC-BY-NC-SA-4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/), available on GitHub as [UTF-8000/UTF-8000-Images](https://github.com/UTF-8000/UTF-8000-Images).

The Python reference implementation is licensed under [GPL-3.0-only](https://www.gnu.org/licenses/gpl-3.0.txt), available on GitHub as [UTF-8000/UTF-8000-Python](https://github.com/UTF-8000/UTF-8000-Python). Any implementation of decoding and encoding UTF-8000 is going to look somewhat similar to this codebase of course; don't worry if you want to use MIT or BSD or something else in a clean-room rewrite.

Thanks

Bell Labs:

- [Claude Shannon](https://en.wikipedia.org/wiki/Claude_Shannon), founding father of the Digital Age, Information Theory, and Artificial Intelligence. His 1948 paper [*A Mathematical Theory of Communication*](https://en.wikipedia.org/wiki/A_Mathematical_Theory_of_Communication) is the most important mathematics paper of the mid 20th Century, which forms a good chunk of the University of Cambridge Mathematics Tripos course *Coding and Cryptography*. I made a Manim [YouTube video](https://www.youtube.com/watch?v=xhae09q8uVk) in 2024 covering Entropy from Information Theory and its various appearances in mathematics.
- [Ken Thompson](https://en.wikipedia.org/wiki/Ken_Thompson), creator of UTF-8, also best known for Unix and other big projects like chess computers.
- [Rob Pike](https://en.wikipedia.org/wiki/Rob_Pike), co-creator of UTF-8, also best known for Plan 9 and other big projects.

The University of Cambridge mathematics department:

- [Professor Stuart Martin](https://www.maths.cam.ac.uk/person/sm137), who lectured *Coding and Cryptography* 2019-2020.
- [Dr Ross Lawther](https://www.maths.cam.ac.uk/person/ril10), director of studies for [mathematics at Girton College](https://www.girton.cam.ac.uk/subjects-courses/mathematics) who supervised me for *Coding and Cryptography* (and other mathematics topics!) 2017-2020. It is question 2 of [example sheet 1](https://www.dpmms.cam.ac.uk/study/II/Coding/2019-2020/CC1-20.pdf) that concerns the product of two prefix-free codes, which I was reminded of by the product of the self-synchronization and self-punctuation mechanisms of UTF-8000.
- [Dr Keith Carne](https://www.maths.cam.ac.uk/person/tkc10), whose timeless lecture notes for *Codes and Cryptography* I use often. They are mirrored on my website [here](https://math.jb2170.com/videos/entropy/Codes-And-Cryptography-TKCarne.pdf). Start at chapter 3 if you're interested!

Epilogue

> To think of these stars that you see overhead at night, these vast worlds which we can never reach. I would annex the planets if I could.
>
> \- [Cecil John Rhodes](https://en.wikipedia.org/wiki/Cecil_Rhodes), founder of Rhodesia.

And annex the entire integers we have done! I find it fitting that ASCII (🇺🇸) and UTF-8 (🇺🇸) are completed by UTF-8000 (🇬🇧), another Anglosphere classic. But I do have another idea:

I am publishing this document formally on July 4th 2026, perhaps as a 250th birthday gift from Great Britain to the United States of America. Cheers!

> One man alone in a room with a computer, a typewriter as it was, can change the world.
>
> \- [Jonathan Bowden](https://en.wikipedia.org/wiki/Jonathan_Bowden), English cultural orator.
>
> (As a proud alumnus of [Girton College](https://en.wikipedia.org/wiki/Girton_College,_Cambridge) I do feel compelled to comment that man should be read in the Lockean sense of mankind!)

The world has been changed at least twice by Ken Thompson, with Unix and UTF-8. Unix was first written in solitude, in three weeks of the summer of 1969 (the same time as the [Moon landing](https://utf-8000.jb2170.com/#jfk-quote)), at Bell Labs on a teletypewriter attached to a spare PDP computer. UTF-8 was invented in one night of autumn 1992, at a New Jersey diner on a placemat, with [a slight tweak](https://utf-8000.jb2170.com/#sec-properties-special-cases:~:text=In%20the%20earliest%20draft,days%20later%20to%201111110x.) a few days later.

I, Jay Berry, have so far written this document alone in the summer of 2026, realizing that my reference implementation from autumn 2024 was a nontrivial discovery.
