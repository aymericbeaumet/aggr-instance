---
title: Parser Combinators
link: https://t3.at/blog/2026-10-17-parser-combinators/
source: hnrss-org
published: 2026-10-18T08:19:53Z
updated: 2026-10-18T08:19:53Z
first_seen: 2026-10-18T15:47:38.003788439Z
authors:
- schindlabua
content: extracted
html: 2026-10-18-parser-combinators.html
extra:
  comments_url: https://news.ycombinator.com/item?id=50139067
  num_comments: 54
  points: 124
---

`"Hello, world!"` `{
  greeting: "Hello",
  who: "world"
}`

The string "Hello, world!" parses into an object with greeting "Hello", who "world".

`type Parser<T> = (input: string) => [T, string]`

`[" pInteger number10, " string 20 30 "]`

The type Parser of T is a function from an input string to a pair of a T and the rest of the string. For example, pInteger cuts "10 20 30" after"10", and returns the number 10 and the rest of the string," 20 30".

`pHello` `[" Hello ", " 100 "]`

`pIgnoreWhitespace` `["  null ", " 100 "]`

`pInteger` `[" 100 ", "  "]`

"Hello 100" parsed by pHello, then pIgnoreWhitespace, then pInteger, each taking the rest the one before left over: pHello returns "Hello" and " 100". pIgnoreWhitespace returns " " and "100". pInteger returns "100" and "".

`pHello100` `[" Hello 100 ["Hello", null, 100] ", "  "]`

"Hello 100" parsed by pHello100, each taking the rest the one before left over: pHello100 returns "Hello 100" and "".

`pSeq(
  pHello,
  pIgnoreWhitespace,
  pInteger
)` `Parser<[
  string,
  null,
  number
]>`

pSeq given pHello, pIgnoreWhitespace, pInteger is a Parser of the tuple \[string, null, number\].

* * *

`const pHello100 = pSeq(...)` `const result = pHello100("Hello 100")`

`pHello100` `[" Hello 100 ["Hello", null, 100] ", "  "]`

"Hello 100" parsed by pHello100, each taking the rest the one before left over: pHello100 returns "Hello 100" and "".

`pGreeting` `[" Hello 100 ["Hello", null, 100] {
  greeting: "Hello",
  who: 100
} pMap ", "  "]`

"Hello 100" parsed by pGreeting, each taking the rest the one before left over: pGreeting returns "Hello 100" and "".

`pSeq(pNumber, pSemicolon)` `[" 10; [10, ";"] ", " 20;30; "]`

`pSeq(pNumber, pSemicolon)` `[" 20; [20, ";"] ", " 30; "]`

`pSeq(pNumber, pSemicolon)` `[" 30; [30, ";"] ", "  "]`

"10;20;30;" parsed by pSeq(pNumber, pSemicolon), then pSeq(pNumber, pSemicolon), then pSeq(pNumber, pSemicolon), each taking the rest the one before left over: pSeq(pNumber, pSemicolon) returns "10;" and "20;30;". pSeq(pNumber, pSemicolon) returns "20;" and "30;". pSeq(pNumber, pSemicolon) returns "30;" and "".

`PING 1.1.1.1 (1.1.1.1) 56(84) bytes of data.
64 bytes from 1.1.1.1: icmp_seq=1 ttl=52 time=389 ms
64 bytes from 1.1.1.1: icmp_seq=2 ttl=52 time=68.7 ms
64 bytes from 1.1.1.1: icmp_seq=3 ttl=52 time=47.1 ms
64 bytes from 1.1.1.1: icmp_seq=4 ttl=52 time=48.6 ms
64 bytes from 1.1.1.1: icmp_seq=5 ttl=52 time=85.8 ms
64 bytes from 1.1.1.1: icmp_seq=6 ttl=52 time=84.8 ms
64 bytes from 1.1.1.1: icmp_seq=7 ttl=52 time=87.4 ms
--- 1.1.1.1 ping statistics ---
8 packets transmitted, 7 received, 12.5% packet loss, time 7008ms
rtt min/avg/max/mdev = 47.129/115.880/388.750/112.515 ms`

`{
  header: { targetIp: "1.1.1.1", resolvedIp: "1.1.1.1", icmpPayloadSize: 56, totalPacketSize: 84 },
  packets: [
    { byteCount: 64, from: "1.1.1.1", seq: 1, ttl: 52, time: 389 },
    { byteCount: 64, from: "1.1.1.1", seq: 2, ttl: 52, time: 68.7 },
    { byteCount: 64, from: "1.1.1.1", seq: 3, ttl: 52, time: 47.1 },
    { byteCount: 64, from: "1.1.1.1", seq: 4, ttl: 52, time: 48.6 },
    { byteCount: 64, from: "1.1.1.1", seq: 5, ttl: 52, time: 85.8 },
    { byteCount: 64, from: "1.1.1.1", seq: 6, ttl: 52, time: 84.8 },
    { byteCount: 64, from: "1.1.1.1", seq: 7, ttl: 52, time: 87.4 },
  ],
  statistics: {
    packetsTransmitted: 8, packetsReceived: 7, packetLoss: 12.5, time: 7008,
    rtt: { min: 47.129, avg: 115.88, max: 388.75, mdev: 112.515 },
  },
}`

Fold`const result = list.reduce((acc, n) => acc + n)` `["Hello", " ", 100] → "Hello 100"`

* * *

Unfold`const result = pSeq(...)("Hello 100")` `"Hello 100" → ["Hello", " ", 100]`
