---
title: How curl was accidentally Turing-complete for nine months
link: https://seriot.ch/computation/curl.html
source: lobste-rs
published: 2026-10-30T07:24:01Z
updated: 2026-10-30T07:24:01Z
first_seen: 2026-10-30T22:55:36.634700404Z
authors:
- seriot.ch via kytta
labels:
- compsci
summary: Comments
content: extracted
html: 2026-10-30-how-curl-was-accidentally-turing-complete-for-nine-months.html
remote_preview:
  url: https://seriot.ch/resources/curl_config.png
---

### Nicolas Seriot

#### [Computation](https://seriot.ch/computation/index.html) > How curl was accidentally Turing-complete for nine months

*29th October 2026*

From curl 8.12 through 8.16, config files could slice variable values and include other config files without an explicit depth cap. Together, these features let curl simulate any two-counter Minsky program. Under the usual idealization of unbounded strings and resources, this construction was Turing-complete.

Curl 8.17.0 added a five-level include cap that breaks this reduction.

**Update 2026-10-30**: Jonathan Tremesaygues wrote me: *I couldn't help but implement “99 Bottles of Beer on the Wall”* [99\_bottles.tar.gz](https://seriot.ch/resources/99_bottles.tar.gz)

### Minsky 2-Counter Reduction

A counter value `n` is represented by `n` copies of `x`.

Each instruction state uses one or more config files, with file names acting as labels, and `expand-config` selecting the next file.

Syntax:

- `{{b}}` expands the current value
- `[0-0]` keeps only the first byte (`x` or empty)
- `[1-]` keeps everything except the first byte
- empty slices yield empty strings ([doc](https://github.com/curl/curl/blob/curl-8_12_0/docs/cmdline-opts/variable.md))

| Minsky         | Curl config                                                             |
| -------------- | ----------------------------------------------------------------------- |
| Register `R`   | Variable `r` (`R` is the number of `x` bytes)                           |
| `INC(R)`       | `expand-variable = "r={{r}}x"`                                          |
| `DEC(R)`       | `expand-variable = "r[1-]={{r}}"`                                       |
| `JZ(R, _, _x)` | `expand-variable = "t[0-0]={{r}}"` `expand-config = "state_{{t}}.conf"` |
| `JMP(S)`       | `expand-config = "S.conf"`                                              |
| Halt           | No further `expand-config`                                              |

With that, we can compile any two-counter program into curl config files.

### PoC: Adding 2 + 3

Four files in [curl\_addition.zip](https://seriot.ch/resources/curl_addition.zip) compute 2 + 3.

![](https://seriot.ch/resources/curl_config.png)

Build a local curl 8.12.0 binary if needed:

```
mkdir -p /tmp/curl-tc-build
cd /tmp/curl-tc-build
curl -fsSLO https://curl.se/download/curl-8.12.0.tar.gz
tar -xzf curl-8.12.0.tar.gz
cd curl-8.12.0
./configure --without-ssl --disable-shared --disable-manual --disable-docs
make -j4
```

Then, from the `addition/` directory, run:

```
/tmp/curl-tc-build/curl-8.12.0/src/curl -q -s -K main.conf
```

Expected output:

```
A=xxxxx B=
```

### Runtime Bounds

The reduction assumes unbounded counters and unbounded steps. The real binary has neither:

- **Expanded content (curl's own limit).** Each expansion is capped at `MAX_EXPAND_CONTENT` = 10,000,000 bytes ([`src/var.c`](https://github.com/curl/curl/blob/curl-8_12_0/src/var.c#L42)).
- **Step count.** There is no loop: each step is a nested `--config` that returns only at halt. Every level uses a C stack frame and keeps its config file open until it finishes. In a minimal countdown loop on 8.12, an 8 MB stack allowed 922 steps before a segfault.
- **Retained variable versions.** Reassignment [adds a new entry](https://github.com/curl/curl/blob/curl-8_12_0/src/var.c#L347-L370) while retaining the old one until [cleanup](https://github.com/curl/curl/blob/curl-8_12_0/src/var.c#L58-L78). Memory use and some variable lookups grow with the number of steps.

### History and Version Support

- curl 8.3 (2023-09): Variables expansion
- curl 8.12 (2025-02): Byte-range assignment
- curl 8.17 (2025-11): [five-level cap](https://github.com/curl/curl/blob/curl-8_17_0/src/tool_parsecfg.h#L28-L30) defeats the reduction

The [pull request](https://github.com/curl/curl/pull/19168) says the cap prevents a config file from including itself indefinitely. Its [regression test](https://github.com/curl/curl/blob/curl-8_17_0/tests/data/test774) does exactly that and expects an error. The public explanation does not discuss Turing completeness or why the cap is five.

A Bash `while` loop can complement bounded curl by invoking a config once per step and carrying the counters and instruction label between invocations. The repetition then comes from Bash, so this is a composition rather than curl-alone Turing completeness.

### Security impact

None.
