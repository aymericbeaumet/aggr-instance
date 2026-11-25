---
title: I checked Clojure's core with Lean and found 17 unreported bugs
link: https://s.conao3.com/blog/2026/e4f733
source: lobste-rs
published: 2026-11-24T15:22:24Z
updated: 2026-11-24T15:22:24Z
first_seen: 2026-11-25T01:32:55.949141972Z
authors:
- s.conao3.com via nathell
labels:
- clojure
summary: Comments
content: extracted
html: 2026-11-24-i-checked-clojure-s-core-with-lean-and-found-17-unreported.html
remote_preview:
  url: https://s.conao3.com/og/blog/2026/e4f733.png
  alt: I checked Clojure's core with Lean and differential testing and found unreported bugs in HAMT and vector-of
---

I read Koukyosyumei's article "[著名なRust製OSSをLean 4で形式検証してみたら、めちゃめちゃバグが見つかった話](https://zenn.dev/koukyosyumei/articles/31c8fcf4da45b7)" (in Japanese; the title means "I formally verified well-known Rust OSS with Lean 4 and found a lot of bugs"). The author ported well-known Rust projects into a form they could prove in Lean 4, and where a proof failed, they found a bug in the original code.

Many people have used Clojure's persistent data structures since around 1.0, so I expected no bugs to remain. Still, I wanted to see whether Lean would find something there too.

[hirosassa/lean-verify](https://github.com/hirosassa/lean-verify) describes a procedure for modeling a small function in Lean 4 and checking it for all inputs, so I applied it to the part of Clojure's core written in Java.

I modeled integer and bit arithmetic in Lean and searched for counterexamples. I checked the collections with differential testing against the `java.util` collections, and for Java interop I compared the same call with and without a type hint.

I found 17 bugs that I could not find in Clojure's JIRA or on ask.clojure.org. They include `reduce` on `vector-of`, the hash collision node of the HAMT, and calls to functional interfaces without a type hint.

The HAMT itself showed no mismatch in 3 million operations of differential testing, and the vector tree showed none around its boundaries. The bugs sat in separate code outside the trees, and in places where two mechanisms meet, such as `=` and hashing.

I used Clojure 1.12.7, released on 2026-11-18, and JDK 21. The links to the source point to the [clojure-1.12.7](https://github.com/clojure/clojure/tree/clojure-1.12.7) tag.

## How I checked

The lean-verify procedure picks one function and models it as a Lean definition. You write a property that compares it with a naive specification computed at a wider width, and `bv_decide` searches for a counterexample.

If you model Java's `long` and `int` as `BitVec 64` and `BitVec 32`, the wraparound of addition and multiplication maps onto `BitVec` operations. So do the rounding of `/` toward zero and the sign of `%`.

When Lean found a counterexample, I turned the value into a Clojure input and reproduced it in the original code. I then modeled the fixed code in Lean and proved the same property. Lean can only take small functions that reduce to integer and bit arithmetic, so I checked the large persistent data structures with differential testing.

Each test applies the same random sequence of operations to `PersistentHashMap` and to `java.util.HashMap` and compares the results. For the HAMT, the keys mix partial collisions at each 5-bit level with fully identical hashes, and I ran 1,500 operations for each of 2,000 seeds.

For vectors, I compared against `ArrayList` around 32, 1056, 32800, and 1048608, where the tree gains a level. The operations were `conj`, `pop`, `assoc`, and their transient versions.

I checked whether anyone had reported each bug before. I searched Clojure's JIRA through its REST API and ask.clojure.org through web search and the site's own search, and found no matching report.

## reduce on vector-of never finishes

`vector-of` is a vector that stores its values in primitive arrays. Clojure implements it in `gvec.clj`, written in Clojure and separate from `PersistentVector`. If you `drop` from one and then `reduce` it, the call never returns once the vector has 65505 or more elements.

```
(reduce + (drop 1 (into (vector-of :long) (range 65504))))
;; => 2145354256
(reduce + (drop 1 (into (vector-of :long) (range 65505))))
;; => never returns
(reduce + (drop 1 (vec (range 65505))))
;; => 2145419760
```

The cause is [line 79 of gvec.clj](https://github.com/clojure/clojure/blob/clojure-1.12.7/src/clj/clojure/gvec.clj#L79), where the seq's `reduce` computes the start of the next 32-element chunk.

```
(recur result (bit-and 0xffe0 (+ aidx 32)))
```

The code uses `0xffe0` to clear the low 5 bits and round down to a multiple of 32, but the mask also clears bit 16 and everything above it. With 65505 elements, the last chunk starts at index 65504. After that chunk, the start of the next one should be 65536, but the mask turns it into 0, so `reduce` goes back to the start and reads the vector again and again.

[![img](https://s.conao3.com/img/e4f733-mask.svg)](https://s.conao3.com/img/e4f733-mask.svg)

The `0xffe0` mask has stayed in place since the [2010 commit](https://github.com/clojure/clojure/commit/5b281880) that added `internal-reduce`. In Lean, I wrote the property that from a position `a` that is a multiple of 32, the next chunk starts at `a + 32`.

```
import Std.Tactic.BVDecide

def stepImpl (a : BitVec 64) : BitVec 64 := 0xffe0 &&& (a + 32)
def stepFix (a : BitVec 64) : BitVec 64 := (~~~0x1f) &&& (a + 32)

theorem impl_ok (a : BitVec 64) (h32 : a &&& 0x1f = 0) (hlt : a < 0x7fffffe0) :
    stepImpl a = a + 32 := by
  unfold stepImpl; bv_decide
theorem fix_ok (a : BitVec 64) (h32 : a &&& 0x1f = 0) (hlt : a < 0x7fffffe0) :
    stepFix a = a + 32 := by
  unfold stepFix; bv_decide
```

`impl_ok` fails with the counterexample `a = 1073741792`, and `#eval stepImpl 65504` returns 0 as well. `fix_ok` uses `(bit-not 0x1f)` as the mask, and its proof goes through under the assumption that `a` is a multiple of 32 below `0x7fffffe0`.

## dissoc on a HAMT collision node hides another key

Clojure's hash-map is a HAMT: it splits the key's hash into 5-bit pieces and uses them to walk down the tree. Keys whose 32-bit hashes are identical share a `HashCollisionNode` at the bottom of the tree, and the node finds a key among them with `=`.

[HashCollisionNode.without](https://github.com/clojure/clojure/blob/clojure-1.12.7/src/jvm/clojure/lang/PersistentHashMap.java#L964-L971) builds the new node with one key removed. It should give the new node its own hash, `this.hash`, but it passes the parameter `hash` instead.

```
return new HashCollisionNode(null, hash, count - 1, removePair(array, idx/2));
```

The parameter `hash` is the hash of the key you pass to `dissoc`. If you `dissoc` with a key that is `=` to the stored key but has a different hash, the node records the wrong hash.

In Clojure, a vector is `=` to a Java `ArrayList` with the same elements, but the two hash differently. [CLJ-1372](https://clojure.atlassian.net/browse/CLJ-1372) tracks this mismatch.

In the example below, I chose `a` and `b` so that both `ArrayList` keys hash to 1003.

```
(def a (java.util.ArrayList. [0 42]))
(def b (java.util.ArrayList. [1 11]))
[(hash a) (hash b) (hash [0 42]) (= a [0 42])]
;; => [1003 1003 1867155179 true]

(def m3 (-> (hash-map a :A b :B)
            (dissoc [0 42])
            (assoc a :A2)))
[m3 (count m3) (get m3 b)]
;; => [{[1 11] :B, [0 42] :A2} 2 nil]
```

When you print `m3`, you can see the key `b`, but `(get m3 b)` returns `nil`. After the `dissoc`, the node records 1867155179, the hash of `[0 42]`. The next `assoc` then concludes that `a`, with hash 1003, does not collide with the node, and splits the two into separate branches.

A lookup of `b` follows the branch for 1003 and finds only `a` there, so it never reaches `b`. In this example the collision node sits on the first level of the tree, so the key passed to `dissoc` needs the same low 5 bits as 1003, and 1867155179 has them.

[![img](https://s.conao3.com/img/e4f733-hamt.svg)](https://s.conao3.com/img/e4f733-hamt.svg)

`disj` on a hash-set has the same bug, while the path through the transient `dissoc!` does not.

I changed that line in the 1.12.7 `PersistentHashMap.java` to `this.hash` and compiled it. With the patched class ahead of the jar on the classpath, the same example returned `:B` for `(get m3 b)`.

## removeIf gives different results with and without a type hint

Since Clojure 1.12, you can pass a Clojure function directly where a Java method takes a functional interface. [CLJ-2864](https://clojure.atlassian.net/browse/CLJ-2864) removed the logic that read the function's return value as a Clojure truthy value.

> In prior alphas, the FI adapter returns inserted Clojure truthy return logic when adapting a Clojure function to an FI with boolean return.

The compiler follows this for calls where it knows the type. A call without a type hint goes through reflection and uses [Reflector.coerceAdapterReturn](https://github.com/clojure/clojure/blob/clojure-1.12.7/src/jvm/clojure/lang/Reflector.java#L579-L591). That method still applies `RT.booleanCast` to a `boolean` return value, so the same `.removeIf` call gives a different result depending on the type hint.

```
(let [l (java.util.ArrayList. [1 2 3])]
  (.removeIf l #{1 2}) (vec l))
;; => ClassCastException
(let [l (identity (java.util.ArrayList. [1 2 3]))]
  (.removeIf l #{1 2}) (vec l))
;; => [3]
```

Wrapping the list in `identity` hides the type of `l`, so the call goes through reflection.

A set used as a function returns the element itself. In the call with a known type, the adapter cannot turn the `Long` into a `Boolean` and throws. The reflective call reads the return value as true and removes the elements.

[![img](https://s.conao3.com/img/e4f733-removeif.svg)](https://s.conao3.com/img/e4f733-removeif.svg)

## int overflow and a negative subvec index

Clojure 1.12 added `IDrop` implementations that make `drop` faster. The ones for string seqs and vector seqs add the current position and `n` as ints and then compare the sum with the length. [StringSeq.drop](https://github.com/clojure/clojure/blob/clojure-1.12.7/src/jvm/clojure/lang/StringSeq.java#L61-L68) for string seqs is one of them.

```
(drop Integer/MAX_VALUE (next (seq "abc")))
;; => StringIndexOutOfBoundsException Index -2147483648
(drop Integer/MAX_VALUE (seq "abc"))
;; => ()
```

The Lean model gave the counterexample `i = 2147483646` and `n = 2147483647`. I changed the check to compare `n` with the remaining length `s.length() - i`. Under the assumptions `0 <` i < s.length()= and `0 < n`, Lean proved that the fixed check matches the naive specification for every 32-bit input.

[![img](https://s.conao3.com/img/e4f733-drop.svg)](https://s.conao3.com/img/e4f733-drop.svg)

For `assoc` on the result of `subvec`, [SubVector.assocN](https://github.com/clojure/clojure/blob/clojure-1.12.7/src/jvm/clojure/lang/APersistentVector.java#L661-L667) does not reject a negative index.

```
(assoc [2 3] -1 :x)
;; => IndexOutOfBoundsException
(let [s (assoc (subvec [0 1 2 3 4] 2 4) -1 :x)]
  [s (.-v s)])
;; => [[2 3] [0 :x 2 3 4]]
```

The call overwrites an element of the underlying vector outside the subvec, and returns a subvec that looks unchanged. [CLJ-962](https://clojure.atlassian.net/browse/CLJ-962) fixed the same bug in `nth`, but `assoc` still has it.

[![img](https://s.conao3.com/img/e4f733-subvec.svg)](https://s.conao3.com/img/e4f733-subvec.svg)

## Other findings

The other 12 bugs have a smaller impact, and I reproduced each of them on Clojure 1.12.7.

| Behavior                                                                                                                          | Example                                                                                        |
| --------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------- |
| `reduce-kv` throws on `vector-of`                                                                                                 | `(reduce-kv f [] (vector-of :long 5 6))`                                                       |
| With a functional interface of three or more arguments, a call with a known type truncates an `int` return value without an error | `(fn [a b c] 4294967297)` returns 1, and `(fn [a b] 4294967297)` throws `ArithmeticException`  |
| `next` on a seq of a string, a primitive array, or an infinite `repeat` keeps the metadata                                        | `(meta (rest (with-meta (seq "abc") {:m 1})))` is `{:m 1}`, and for a vector's seq it is `nil` |
| `(long ##NaN)` and `(int ##NaN)` return 0                                                                                         | `(long ##NaN)` is 0, and `(byte ##NaN)` throws                                                 |
| `long` clamps a double 2^63, and `int` a float 2^31, to the maximum value                                                         | `(long (Math/pow 2 63))` is `Long/MAX_VALUE`                                                   |
| `char` wraps a BigInt at 2^64                                                                                                     | `(char 18446744073709551681N)` is `\A`                                                         |
| `float` rejects `##Inf` but lets `##NaN` through                                                                                  | `(float ##Inf)` throws, and `(float ##NaN)` is `##NaN`                                         |
| Boxed and primitive doubles use different range checks when converted to byte and similar types                                   | `(byte (identity 127.9))` is 127, and `(byte 127.9)` throws                                    |
| `get` truncates a fractional index on strings and Java arrays                                                                     | `(get "abc" 1.9)` is `\b`                                                                      |
| `byte-array` and `short-array` reject an initial value                                                                            | `(byte-array 2 7)` throws                                                                      |
| On a seq of a primitive array, `.indexOf` and `.contains` disagree                                                                | `(.indexOf (seq (byte-array [1 2 3])) 2)` is -1                                                |
| `.lastIndexOf` on a list compares with `equals` instead of `=`                                                                    | `(.lastIndexOf (list 1 2) (int 2))` is -1                                                      |

## Where I found nothing

`PersistentHashMap` and `PersistentHashSet` matched `java.util.HashMap` over 3 million operations that mixed persistent and transient updates. The tests also covered maps before an update staying unchanged, `ArrayNode` splits and merges, and collision nodes down to depth 7.

`PersistentTreeMap` matched `java.util.TreeMap` and kept the red-black tree invariants. Vectors matched `ArrayList` around every tree boundary I tested.

On the numeric side, Lean proved for 64 bits that the check `Numbers` uses to pick the fast path for addition and subtraction agrees with the overflow check.

For multiplication, `bv_decide` timed out at 16 bits, so I checked 8 and 10 bits. I proved up to 32 bits that `clojure.core/mod` matches the floored remainder.

Every property I proved in Lean holds for the model shown in this article. I checked that each model means the same thing as the Java code by comparing it with the source and with the reproduced behavior.
