---
title: 'Janet on x32: 32-bit Pointers, 64-bit Speed, 25% Less RAM'
link: https://alexalejandre.com/programming/lisp/janet-for-the-x32-abi/
source: lobste-rs
published: 2026-10-06T22:26:46Z
updated: 2026-10-06T22:26:46Z
first_seen: 2026-10-07T19:30:21.779631669Z
authors:
- alexalejandre.com by veqq
labels:
- linux
- performance
summary: Comments
content: extracted
html: 2026-10-06-janet-on-x32-32-bit-pointers-64-bit-speed-25-less-ram.html
---

* * *

> It’s possible to save a dramatic amount of memory (close to half for programs with pointer heavy heaps) and also speed up programs by a modest amount (by way of more data fitting in cache) by using 32 bit pointers on 64 bit systems.
>
> The Linux x32 ABI lets you do just this, but it’s tragically underused and overlooked. It’s disabled by default on Debian (though you can enable it with a boot flag) and not even compiled in on Arch Linux. Most software compiles just fine for x32, but there isn’t much packaging for it so you do have to compile nearly everything yourself.
>
> I experimented with deploying mastodon on x32 and it cut the app’s memory usage from 650mb to 350mb. There’s so much potential here, but the work is mostly of the thankless coordination and communication type and I don’t have the time or motivation to push it forward myself. - [Hailey](https://lobste.rs/s/gns27z/what_are_your_programming_hunches_you#c_tw3vok)

### How to Compile for 32 Bits?

Janet libraries like Spork supply their own build flags, but we can hijack them with a fake `cc` which starts the real one with the necessary `-m32 -msse2 -mfpmath=sse` flags. I modified the beginning of my default [Janet build script](https://codeberg.org/veqq/install-latest-janet/src/branch/master/install-latest-janet.sh):

```bash
#!/bin/sh
set -eu

TARGET="$PWD/janet32"
JANET="$TARGET/bin/janet"

mkdir -p "$TARGET/cc32"
printf '#!/bin/sh\nexec gcc -m32 -msse2 -mfpmath=sse "$@"\n' > "$TARGET/cc32/cc"
chmod +x "$TARGET/cc32/cc"
export PATH="$TARGET/cc32:$PATH"
export JANET_TOOLCHAIN=cc

unset JANET_PATH JANET_TREE

mkdir -p "$TARGET"
cd "$TARGET"
rm -rf build/spork janet

git clone https://github.com/janet-lang/janet
cd janet
```

Easy enough! Now let’s try it!

### Failing on Arch

tl;dr: 20% memory reduction, 50% slower

See the Scripts and Results

This is on cachyos (arch, btw) with lib32-glibc lib32-gcc-libs. Adding this to a build script forces libraires like spork to build at 32 bit too!

For `mem.janet`:

```janet
(let [live @[]]
  (for i 0 2_000_000
    (array/push live [i (+ i 1)]))
  (gccollect)
  (print (length live)))
```

Note, these use Fish and I don’t want to rebuild my site with other language support:

```bash
sudo pacman -S time # different from just time
command time -v ./bin/janet mem.janet # command skips the fish version
command time -v ./janet32/bin/janet mem.janet

2000000
       Command being timed: "./bin/janet mem.janet"
       User time (seconds): 0.24
       System time (seconds): 0.05
       Percent of CPU this job got: 100%
       Elapsed (wall clock) time (h:mm:ss or m:ss): 0:00.30
       Average shared text size (kbytes): 0
       Average unshared data size (kbytes): 0
       Average stack size (kbytes): 0
       Average total size (kbytes): 0
       Maximum resident set size (kbytes): 145196
       Average resident set size (kbytes): 0
       Major (requiring I/O) page faults: 0
       Minor (reclaiming a frame) page faults: 33513
       Voluntary context switches: 1
       Involuntary context switches: 9

2000000
       Command being timed: "./janet32/bin/janet mem.janet"
       User time (seconds): 0.47
       System time (seconds): 0.03
       Percent of CPU this job got: 99%
       Elapsed (wall clock) time (h:mm:ss or m:ss): 0:00.50
       Average shared text size (kbytes): 0
       Average unshared data size (kbytes): 0
       Average stack size (kbytes): 0
       Average total size (kbytes): 0
       Maximum resident set size (kbytes): 113716
       Average resident set size (kbytes): 0
       Major (requiring I/O) page faults: 0
       Minor (reclaiming a frame) page faults: 25686
       Voluntary context switches: 1
       Involuntary context switches: 21
```

I added a 0 in the test

```bash
command time -v ./bin/janet mem.janet
                  command time -v ./janet32/bin/janet mem.janet
20000000
        Command being timed: "./bin/janet mem.janet"
        User time (seconds): 2.83
        System time (seconds): 0.46
        Percent of CPU this job got: 99%
        Elapsed (wall clock) time (h:mm:ss or m:ss): 0:03.31
        Average shared text size (kbytes): 0
        Average unshared data size (kbytes): 0
        Average stack size (kbytes): 0
        Average total size (kbytes): 0
        Maximum resident set size (kbytes): 1410668
        Average resident set size (kbytes): 0
        Major (requiring I/O) page faults: 0
        Minor (reclaiming a frame) page faults: 316820
        Voluntary context switches: 1
        Involuntary context switches: 164
        Exit status: 0
20000000
        Command being timed: "./janet32/bin/janet mem.janet"
        User time (seconds): 4.64
        System time (seconds): 0.35
        Percent of CPU this job got: 99%
        Elapsed (wall clock) time (h:mm:ss or m:ss): 0:05.00
        Average shared text size (kbytes): 0
        Average unshared data size (kbytes): 0
        Average stack size (kbytes): 0
        Average total size (kbytes): 0
        Maximum resident set size (kbytes): 1098372
        Average resident set size (kbytes): 0
        Major (requiring I/O) page faults: 0
        Minor (reclaiming a frame) page faults: 238633
        Voluntary context switches: 1
        Involuntary context switches: 193
```

For e.g. `declarative-dsls/tests.janet`

```bash
98 passed
        Command being timed: "./bin/janet ./lib/janet/bin/judge declarative-dsls/tests.janet"
        User time (seconds): 0.42
        System time (seconds): 0.02
        Percent of CPU this job got: 100%
        Elapsed (wall clock) time (h:mm:ss or m:ss): 0:00.45
        Average shared text size (kbytes): 0
        Average unshared data size (kbytes): 0
        Average stack size (kbytes): 0
        Average total size (kbytes): 0
        Maximum resident set size (kbytes): 61900
declarative-dsls/tests.janet

98 passed
        Command being timed: "./bin/janet ./lib/janet/bin/judge declarative-dsls/tests.janet"
        User time (seconds): 0.70
        System time (seconds): 0.02
        Percent of CPU this job got: 99%
        Elapsed (wall clock) time (h:mm:ss or m:ss): 0:00.72
        Average shared text size (kbytes): 0
        Average unshared data size (kbytes): 0
        Average stack size (kbytes): 0
        Average total size (kbytes): 0
        Maximum resident set size (kbytes): 54108
        Average resident set size (kbytes): 0
        Major (requiring I/O) page faults: 0
        Minor (reclaiming a frame) page faults: 12735
```

As Hailey said, Arch does not come with the [x32 ABI](https://en.wikipedia.org/wiki/X32_ABI) compiled, so I had to use `-m32` and while we do get a 20% memory savings, we execute 50% slower because half the registers aren’t used! `-m32` is the old 32-bit x86 mode using only 8 registers. To see the speed-up, we need to use `-mx32` which uses the 64-bit instruction set with shrunken 32-bit pointers. But I don’t want to recompile Linux…

Unfortunately, Ubuntu also dropped x32 support.

### Winning on Deprecated Ubuntu

Luckily, lazily, I have access to some deprecated servers (*of course* not in prod…) and OS installs, wasting hard disk space:

> Ubuntu 20.04 LTS Focal Fossa has reached its end of standard support on 31 May 2025.

Ubuntu, so we have bash again:

```bash
grep X86_X32 /boot/config-$(uname -r)  
CONFIG_X86_X32=y
```

Just what we need, although `gcc version 9.4.0` is a bit old. We already have `build-essential git gcc-multilib libc6-dev-x32`. We are ready to soar! Here is our full `32janet.sh`:

```bash
#!/bin/bash
set -eu

TARGET="$PWD/janetx32"
JANET="$TARGET/bin/janet"

mkdir -p "$TARGET/cc32"
# this forces Janet C libraries like Spork to build with 32bit also
# FFI build test fails on -mx32 build so -DJANET_NO_FFI
printf '#!/bin/sh\nexec gcc -mx32 -DJANET_NO_FFI "$@"\n' > "$TARGET/cc32/cc"
chmod +x "$TARGET/cc32/cc"
export PATH="$TARGET/cc32:$PATH"
export JANET_TOOLCHAIN=cc

unset JANET_PATH JANET_TREE
mkdir -p "$TARGET"
cd "$TARGET"

git clone https://github.com/janet-lang/janet
cd janet

export CFLAGS='-fPIC -O3 -flto -fno-semantic-interposition -march=native' # speed gains
PREFIX="$TARGET" make clean
PREFIX="$TARGET" make
PREFIX="$TARGET" make test
PREFIX="$TARGET" make install

cd ..
git clone --depth=1 https://github.com/janet-lang/spork build/spork
"$JANET" -e '(if (bundle/installed? "spork") (bundle/replace "spork" "build/spork") (bundle/install "build/spork"))'

# ### Packages
PM="$TARGET/lib/janet/bin/janet-pm"
git clone https://codeberg.org/veqq/declarative-dsls
"$JANET" "$PM" install file::declarative-dsls # also does https://codeberg.org/veqq/varray
git clone https://github.com/ianthehenry/judge # also does https://github.com/ianthehenry/cmd
"$JANET" "$PM" install file::judge

time "$JANET" "$TARGET/lib/janet/bin/judge" declarative-dsls/tests.janet # to show that it worked
```

And a \`64janet.sh to compare it with:

```bash
#!/bin/bash
set -eu

TARGET="$PWD/janet64"
JANET="$TARGET/bin/janet"
unset JANET_PATH JANET_TREE
mkdir -p "$TARGET"
cd "$TARGET"

git clone https://github.com/janet-lang/janet
cd janet

export CFLAGS='-fPIC -O3 -flto -fno-semantic-interposition -march=native' # speed gains
PREFIX="$TARGET" make clean
PREFIX="$TARGET" make
PREFIX="$TARGET" make test
PREFIX="$TARGET" make install

cd ..
git clone --depth=1 https://github.com/janet-lang/spork build/spork
"$JANET" -e '(if (bundle/installed? "spork") (bundle/replace "spork" "build/spork") (bundle/install "build/spork"))'

# ### Packages
PM="$TARGET/lib/janet/bin/janet-pm"
git clone https://codeberg.org/veqq/declarative-dsls
"$JANET" "$PM" install file::declarative-dsls # also does https://codeberg.org/veqq/varray
git clone https://github.com/ianthehenry/judge # also does https://github.com/ianthehenry/cmd
"$JANET" "$PM" install file::judge

time "$JANET" "$TARGET/lib/janet/bin/judge" declarative-dsls/tests.janet # to show that it worked
```

Now we compare them:

```bash
sudo apt install time # different from just time
/usr/bin/time -f '%e s  %M KB' janet64/bin/janet janet64/lib/janet/bin/judge janet64/declarative-dsls/tests.janet
/usr/bin/time -f '%e s  %M KB' janetx32/bin/janet janetx32/lib/janet/bin/judge janetx32/declarative-dsls/tests.janet

2.42 s  61668 KB # 64bit
2.36 s  53472 KB # 32bit
```

But [declarative-dsls](https://codeberg.org/veqq/declarative-dsls) is a worst case scenario, using [typed c arrays](https://codeberg.org/veqq/varray) for big speed ups, which don’t benefit much from this. Lets make some minimal benchmarks to really test the differences:

See the Benchmarks Scripts

`words.janet` builds a text, splits it into words, counts them and sorts on that:

```janet
(let [start (os/clock :monotonic)
      rng (math/rng 42)
      vocab (seq [i :range [0 5000]] (string "w" (math/rng-int rng 100000)))
      text @""
      counts @{}]
  (repeat 600000 (buffer/push text (in vocab (math/rng-int rng 5000)) " "))
  (each word (string/split " " text)
    (put counts word (+ 1 (get counts word 0))))
  (pp (take 3 (sort-by |(- (in $ 1)) (pairs counts))))
  (print (length counts) " distinct words")
  (print (- (os/clock :monotonic) start) " s"))
```

`records.janet` groups structs by department and summarizes those departments, with unused extra fields like real code:

```janet
(let [start (os/clock :monotonic)
      rng (math/rng 7)
      depts [:eng :ops :sales :legal :support]
      people (seq [i :range [0 300000]]
               {:id i
               # :name and :tags not used
                :name (string "person-" i)
                :dept (in depts (math/rng-int rng 5))
                :salary (+ 30000 (math/rng-int rng 90000))
                :tags [(math/rng-int rng 10) (math/rng-int rng 10)]})
      by-dept (group-by |(in $ :dept) people)]
  (each dept (sorted (keys by-dept))
    (let [staff (in by-dept dept)
          salaries (map |(in $ :salary) staff)]
      (pp [dept
           (length staff)
           (math/round (/ (sum salaries) (length staff)))
           (length (filter |(> $ 100000) salaries))])))
  (print (- (os/clock :monotonic) start) " s"))
```

`parse.janet` parses lines with a PEG and totals them:

```janet
(let [start (os/clock :monotonic)
      rng (math/rng 3)
      lines (seq [i :range [0 200000]]
              (string i "," (math/rng-int rng 1000) ",item-" (math/rng-int rng 500) "," (/ (math/rng-int rng 10000) 100)))
      row (peg/compile
            ~{:num (number (some (set "0123456789.")))
              :txt (capture (some (if-not "," 1)))
              :main (* :num "," :num "," :txt "," :num -1)})
      rows (map |(peg/match row $) lines)
      totals @{}]
  (each [id qty item price] rows
    (put totals item (+ (get totals item 0) (* qty price))))
  (print (length rows) " rows, " (length totals) " items, total " (math/round (sum (values totals))))
  (print (- (os/clock :monotonic) start) " s"))
```

`tree.janet` builds and walks binary trees, our pointer-heaviest example:

```janet
(let [start (os/clock :monotonic)
      make (fn make [depth]
             (if (= depth 0)
               [nil nil]
               [(make (- depth 1)) (make (- depth 1))]))
      check (fn check [node]
              (if (in node 0)
                (+ 1 (check (in node 0)) (check (in node 1)))
                1))
      long-lived (make 19)]
  (var total 0)
  (repeat 40 (+= total (check (make 14))))
  (print total " " (check long-lived))
  (print (- (os/clock :monotonic) start) " s"))
```

We benchmark:

```bash
for f in words records tree parse; do  
>   /usr/bin/time -f "$f 64:  %e s  %M KB" janet64/bin/janet $f.janet > /dev/null  
>   /usr/bin/time -f "$f x32: %e s  %M KB" janetx32/bin/janet $f.janet > /dev/null  
> done  
 
words 64:  0.58 s  41904 KB  
words x32: 0.67 s  31856 KB  
records 64:  6.40 s  138164 KB  
records x32: 5.80 s  126916 KB  
tree 64:  1.39 s  50504 KB  
tree x32: 1.50 s  35532 KB  
parse 64:  3.20 s  58504 KB  
parse x32: 3.19 s  44428 KB

words 64:  0.63 s  41984 KB  
words x32: 0.70 s  31976 KB  
records 64:  6.71 s  138240 KB  
records x32: 5.77 s  126844 KB  
tree 64:  1.40 s  50392 KB  
tree x32: 1.48 s  35304 KB  
parse 64:  3.37 s  58592 KB  
parse x32: 3.09 s  44496 KB

words 64:  0.59 s  41756 KB  
words x32: 0.67 s  31788 KB  
records 64:  6.50 s  138384 KB  
records x32: 5.80 s  126960 KB  
tree 64:  1.39 s  50288 KB  
tree x32: 1.43 s  35560 KB  
parse 64:  3.35 s  58520 KB  
parse x32: 3.24 s  44572 KB

words 64:  0.60 s  42012 KB  
words x32: 0.67 s  31896 KB  
records 64:  6.29 s  138448 KB  
records x32: 6.00 s  126832 KB  
tree 64:  1.38 s  50176 KB  
tree x32: 1.43 s  35392 KB  
parse 64:  3.36 s  58384 KB  
parse x32: 3.13 s  44364 KB
```

### Conclusion

`-mx32` cut RAM use by 21% (8-32%) with equal speed (-13% slower to 10% faster), while `-x32` reduced speed by half. Janet is a worst case where nanboxing already packs values into 8 bytes so the main benefit’s smaller headers. I invite others to test their software and languages similarly. With continued x32 support, we could use this as a default for scripts, utilities, daemons and the like. I thus believe Linux distros should include `-mx32` support and daemons and utilities should be compiled down for more efficient idling and day to day computing! It only costs a flag.

* * *

### P.S. Implications for Janet

If Janet works with smaller object headers, why not shrink them in the implementation for 64-bit use? By my understanding, Janet [heap objects](https://github.com/janet-lang/janet/blob/c495f11f15a29033b5f52d66f82f13a105f98123/src/include/janet.h#L974) use 16 bytes to help the garbage collector:

- 4 bytes for flags
- 4 bytes of padding
- 8 bytes linking to the next object

Reducing these would require a different garbage collection strategy e.g. our own allocator, which would hurt a core Janet use case: embedding in other projects.

Each type then adds:

- [string](https://github.com/janet-lang/janet/blob/c495f11f15a29033b5f52d66f82f13a105f98123/src/include/janet.h#L1081): 8 bytes for length and hash
- [tuple](https://github.com/janet-lang/janet/blob/c495f11f15a29033b5f52d66f82f13a105f98123/src/include/janet.h#L1061): 8 bytes for length and hash, 8 bytes for source line and column info
- [array](https://github.com/janet-lang/janet/blob/c495f11f15a29033b5f52d66f82f13a105f98123/src/include/janet.h#L1029): 8 bytes for count and capacity, 8 bytes for a pointer to the data
- [struct](https://github.com/janet-lang/janet/blob/c495f11f15a29033b5f52d66f82f13a105f98123/src/include/janet.h#L1071): 12 bytes for length, hash and capacity, 4 bytes padding and 8 bytes for its prototype’s pointer
- [table](https://github.com/janet-lang/janet/blob/c495f11f15a29033b5f52d66f82f13a105f98123/src/include/janet.h#L1045): 12 bytes for count, capacity and how many deleted, 4 bytes padding, 8 bytes for a pointer and 8 for the prototype’s pointer

For structs and tables in particular, we could regain 8 bytes through rearranging (not needing the later padding). Onlt tuples parsed from code need the source information (for errors); a flag could save normal tuples 8 bytes.

Unfortunately, glibc’s `malloc` adds 8 bytes and rounds to multiples of 16 so structs would remain the same. But tables and (some) tuples would shrunk more than the above indicates!

### Down the Rabbit Hole

Kernels must be compiled with `CONFIG_X86_X32_ABI` providing syscall entry points on Linux (with no Windows and Mac equivalent) which the compilers expose via `-mx32`.

Good reading:

- [https://lwn.net/Articles/1074897/#Comments](https://lwn.net/Articles/1074897/#Comments)
- [https://blogsystem5.substack.com/p/x86-64-programming-models](https://blogsystem5.substack.com/p/x86-64-programming-models)

Questions:

- can Zig via `zig cc -target x86_64-linux-muslx32` crosscompile for easy distribution?
- how much performance benefit remains when using a faster allocator like mimalloc? The points will still be faster, at any rate.
