---
title: The lost malloc()
link: https://napcakes.nekoweb.org/posts/malloc/
source: hnrss-org
published: 2026-11-04T07:37:30Z
updated: 2026-11-04T07:37:30Z
first_seen: 2026-11-04T15:43:16.719955117Z
authors:
- matt_d
content: extracted
html: 2026-11-04-the-lost-malloc.html
remote_preview:
  url: https://napcakes.nekoweb.org/posts/malloc/unix-v4-tape.jpg
  alt: Magnetic tape of UNIX v4 from Bell Labs, circa 1973
---

2026-10-29

(Sorry, I couldn't resist not putting a catchy title :)

Prompted by another project idea that popped into my mind, I did some Unix archaeology and looked at the original [sources](https://www.tuhs.org/cgi-bin/utree.pl "The Unix Tree"). This is the most fun I've ever had reading someone else's code - probably due to the historical significance. But also because of how delightfully approachable it was, compared to modern monstrocities.

This is a short(-ish) post regarding how they handled the mundane and boring task of allocating and managing memory (in C). In the future I'd like to read more about Unix the OS, its design, architecture & implementation.

## A long time ago...

In a PDP far, far away, our journey begins with the fourth edition of Unix, released November '73, *more than half a century ago*. C actually first appeared in the second edition, released the year before, but wasn't extensively used. V4 is special because even the majority of the kernel was written in (let's call it "medieval") C by that point. The system had grown, however, and it was quite capable. You'll find that many Unix things we have today in POSIX-land were already present in V4.

**Super fun fact:** A tape containing V4 was only found [last year](https://discuss.systems/@ricci/115504720054699983)! Previously, it was thought to be completely lost to time.

![Magnetic tape of UNIX v4 from Bell Labs, circa 1973](https://napcakes.nekoweb.org/posts/malloc/unix-v4-tape.jpg "Magnetic tape of UNIX v4 from Bell Labs, circa 1973")

*While cleaning a storage room, our staff found this tape containing UNIX v4 from Bell Labs, circa 1973*

*\~ Rob Ricci*

### Linear allocation

The first interesting bit is found among the C "compiler" sources. In quotes, because it wasn't a monolithic compiler as we know it today. Rather, it was a set of distinct tools which you chained in order to get an object file from C code.

In [`/usr/c/c20.c:814`](https://www.tuhs.org/cgi-bin/utree.pl?file=V4/usr/c/c20.c), from what I can gather a very primitive optimiser, if you will (referred to as "C object code improver" in V5; there aren't many comments in V4) you see:

```c
alloc(an)
{
    register int n;
    register char *p;

    n = an;
    n++;
    n =& ~01;
    if (lasta+n >= lastr) {
        if (sbrk(2000) == -1) {
            write(2, "Out of space\n", 14);
            exit(1);
        }
        lastr =+ 2000;
    }
    p = lasta;
    lasta =+ n;
    return(p);
}
```

Isn't that fascinating? This routine is used throughout the program to allocate memory. And if you squint, this is just a linear allocator. It aligns the size, checks if the region is big enough, otherwise tries to extend it. On failure, it just cold-exits. Which means callers don't have to check for `NULL` every single time, and indeed they do not.

The "region" isn't named, unlike how you'd do it today with arenas. However, the above does have a nice property, in that by using `sbrk()`, when we extend the heap, no pointer is invalidated.

And they don't bother freeing memory, because why would they? There's no need. Everything exists for the lifetime of the program.

### `dmr/malloc`

This is the most interesting bit in my opinion, and the reason why I chose that title.

In Dennis' directory, you find a file named [`malloc.c`](https://www.tuhs.org/cgi-bin/utree.pl?file=V4/usr/sys/dmr/malloc.c). And you see two functions there:

```c
struct map {
    char *m_size;
    char *m_addr;
};

malloc(mp, size)
struct map *mp;

mfree(mp, size, aa)
struct map *mp;
```

Seems familiar? Yep, it's `mfree()` with an `m`. These functions obviously deal with memory (de)allocation, and are the precursor to the ones we're familiar with today. *But*, the interfaces are different.

`malloc()` receives an additional first argument called `mp` (for "map"), and so does `mfree()`. What is that? Well, let's see how they're used:

```c
p = malloc(coremap, sizeof foo);
...
mfree(coremap, sizeof foo, p);

q = malloc(swapmap, sizeof bar);
...
mfree(swapmap, sizeof bar, q);
```

So, they pass in some objects `coremap` and `swapmap`. What are those?\
 From [`/usr/sys/systm.h:2`](https://www.tuhs.org/cgi-bin/utree.pl?file=V4/usr/sys/systm.h):

```c
int	coremap[CMAPSIZ];
int	swapmap[SMAPSIZ];
```

They're just flat memory regions! So these `malloc()` and `mfree()` work on *explicit memory regions*! Not only that, `mfree()` receives the size of the object it is deallocating, *as it should*. Those are just infinitely-times better interfaces than traditional `malloc()`/`free()`.

Imagine using a library that has such an interface. You could deallocate all its memory resources by just discarding the entire region you delegated to it. Heap corruption bugs would be less severe because you could partition memory. You could trivially limit the amount of memory third party code allocates. The list goes on and on...

(Side note: They slightly messed up here and the regions should've been sized `CMAPSIZ/2` and `SMAPSIZ/2` respectively. They did fix that later.)

However, I don't think all these benefits were in the minds of Dennis and Ken. So why did they go with that interface and why did they partition memory to "core" and "swap"? The reason is, I believe, just hardware constraints. These interfaces stay like that up to V6 (maybe V7 too?), released the spring of '75, and thankfully there's an infamously good resource about that edition.

![Brian Kernighan holding a copy of Lions's Commentary](https://napcakes.nekoweb.org/posts/malloc/Brian_Kernighan_in_2012_at_Bell_Labs_2.jpg "Brian Kernighan holding a copy of Lions's Commentary")

Brian Kernighan, from [Wikipedia](https://en.wikipedia.org/wiki/A_Commentary_on_the_UNIX_Operating_System#/media/File:Brian_Kernighan_in_2012_at_Bell_Labs_2.jpg).

From *A Commentary on the Sixth Edition UNIX Operating System* by John Lions:

> These are concerned with the allocation and subsequent release of two kinds of memory resources, namely:
>
> - **main memory** in units of 32 words (64 bytes);
> - **disk swap area** in units of 256 words (512 bytes).
>
> For each of these two kinds of resource, a list of available areas is maintained within a resource "map" (either "coremap" or "swapmap"). A pointer to the appropriate resource "map" is always passed to "malloc" and "mfree" so that the routines themselves do not have to know the kind of resource with which they are dealing.

And it goes on to explain how the underlying allocation algorithm works. These do not do linear allocation (hence the `mfree()`). Basically, it's a list of blocks (`struct map`), ordered by address, where you do first-fit and also coalesce neighbour blocks.

The sixth edition is also the first where the term "arena" appears, in [`/usr/source/s4/alloc.s:17`](https://www.tuhs.org/cgi-bin/utree.pl?file=V6/usr/source/s4/alloc.s):

```
/char *allocs[2] {		/*initial empty arena*/
```

### Rolling back

... to the topic of linear allocation... In V6's [`/usr/source/s1/diff1.c:88`](https://www.tuhs.org/cgi-bin/utree.pl?file=V6/usr/source/s1/diff1.c), I think the first ever version of `diff(1)`, you find:

```c
char *area;
char *top;
alloc(n)
{
    register char *p;
    p = area;
    n = (n+1) & ~1;
    area =+ n;
    while(area > top) {
        if(sbrk(1024) == -1) {
            mesg("Out of space\n");
            exit(1);
        }
        top =+ 1024;
    }
    return(p);
}
```

This is not all that much different from the previous `alloc()`, but this time it handles any allocation size. It uses a `while` to continue extending the region and stops when enough space has been reserved. The *actual* interesting part happens later, at `unsort():143`:

```c
unsort(f, l, b)
struct line *f;
int *b;
{
    int *a;
    int i;
    a = alloc((l+1)*sizeof(a[0]));
    for(i=1;i<=l;i++)
        a[f[i].serial] = f[i].value;
    for(i=1;i<=l;i++)
        b[i] = a[i];
    area = a;
}
```

I'm not exactly sure what that function does, but it doesn't matter. What matters is that they allocate some memory (`a = alloc(...);`), then roll back the region (`area = a;`)! Though they subsequently just copy it to `b`, so it seems like this was redundant.

## Computing sizes

Most of the code directly calls `malloc()`/`alloc()` and friends, and inlines the size computation logic. Like the line we saw earlier:

```c
a = alloc((l+1)*sizeof(a[0]));
```

I don't do that anymore, and neither should you! I use a couple macros akin to C++'s `new` that handle all the `sizeof` business. But I am a dumb-dumb. Surely these guys must've come across the same pain points, and, if so, what did they do? Let's jump to V7, released January '79, four years after V6. You find this macro:

```c
#define ALLOC(x) (struct x *) ckalloc(sizeof(struct x))
```

where they use it like:

```c
struct foo { ... };
p = ALLOC(foo);
```

Nice! It's interesting to note *where* you find it; In [`/usr/src/cmd/f77/defines:38`](https://www.tuhs.org/cgi-bin/utree.pl?file=V7/usr/src/cmd/f77/defines) and [`/usr/src/cmd/make/defs:38`](https://www.tuhs.org/cgi-bin/utree.pl?file=V7/usr/src/cmd/make/defs). Slightly suspicious. The former is part of the Fortran 77 compiler and latter of `make`. Stuart Feldman is the person who worked on both of those programs! It's clear that multiple people were working on the system at that time. A macro like that isn't used in any other program, sadly.

V7 is the first edition where you also find the `assert()` macro, and it was used extensively back then too. Good job! They also significantly expanded the number of man pages. Fun fact: I did `$ man V7/usr/man/man3/foo.3` -- on 47 year old pages -- and it worked first try absolutely fine.

Let's go to V8, released in February '85, four years before the first C standard. You find this wild macro in [`/usr/src/cmd/ideal/memut.c:7`](https://www.tuhs.org/cgi-bin/utree.pl?file=V8/usr/src/cmd/ideal/memut.c):

```c
char *fooalloc;
#define	tryalloc(new,kind)	\
    if (!(new =(kind *) malloc(sizeof (kind)))) {\
    emergency ();\
    if (!(new =(kind *) malloc(sizeof (kind)))) {\
        fprintf (stderr, "ideal: Out of space\n");\
        exit (1);\
        }\
    };\
    for (fooalloc = (char *) new;\
        fooalloc < ((char *) new) + sizeof (kind);\
        fooalloc ++)\
        *fooalloc = '\0';
```

If someone is too harsh because you used macros, point them at the above. Usage is bit different this time:

```c
struct foo *x;
tryalloc(x, struct foo);
```

I decided to include it because the macro zero-initialises by default whatever was allocated! You also see a more familiar-looking `malloc()` being used. It appears this is the edition that they switched to the more traditional `malloc()`/`free()` interfaces we know. I'll hypothesize why that is at the very end.

*But it gets even more interesting.* In V8 you also find these macros:

```c
#define new(type) ((type *) alloc (sizeof (type)))
#define new(t) ((t *) alloc (sizeof (t)))
#define        new(s)  (s *)alloc(sizeof (s))
```

When I saw these definitions I jumped out of my chair. It's basically how I'd define it too! You'll find them in:

- [`/usr/src/cmd/finddev.c:245`](https://www.tuhs.org/cgi-bin/utree.pl?file=V8/usr/src/cmd/finddev.c), part of [`finddev(8)`](https://man.cat-v.org/unix_8th/8/finddev).
- [`/usr/src/cmd/asd/asd.h:13`](https://www.tuhs.org/cgi-bin/utree.pl?file=V8/usr/src/cmd/asd/asd.h), part of [`asd(8)`](https://man.cat-v.org/unix_8th/8/asd).
- [`/usr/include/ic.h:55`](https://www.tuhs.org/cgi-bin/utree.pl?file=V8/usr/include/ic.h), part of \**checks notes*\* I have no idea.

respectively.

Unfortunately, I can't find the author(s) of any of these.

### Cfront

In V8, you'll also find Cfront, a.k.a. medieval C++. It's then reasonable to assume that Stroustrup was inspired by the above macros and decided to incorporate `new` as a language-level feature. However, in [*A History of C++: 1979−1991*](https://www.stroustrup.com/hopl2.pdf), he writes:

> In Simula, it is not possible to have local or global variables of class types; that is, every object of a class must be allocated on the free store using the `new` operator.

Huh. So C++'s `new` was inspired by the programming language "Simula". It's fair to say that the macros above were likely also derived from there. But Simula was quite old even by the 70s, let alone 1985. Perhaps Stroustrup persuaded people, or wrote those macros himself. He later writes:

> The `new` operator that both allocates memory from the free store and invokes a constructor to ensure initialization was borrowed from Simula. The `delete` operator was a necessary complement because I did not want C with Classes to depend on a garbage collector. The argument for the `new` operator can be summarized like this. Would you rather write:
>
> ```
> X* p = new X(2);
> ```
>
> or
>
> ```
> struct X * p = (struct X *) malloc(sizeof(struct X));
> if (p == 0) error("memory exhausted");
> p−>init(2);
> ```
>
> and which version are you most likely to make a mistake in? The arguments against -- which were voiced quite a lot at the time -- were "but we don’t really need it," and "but someone will have used `new` as an identifier." Both observations are correct, of course. Introducing operator `new` thus made the use of free store more convenient and less error−prone. *This increased its use even further so that the C free store allocation routine `malloc()` used to implement `new` became the most common performance bottleneck in real systems.* \[...\] Having real **programs spend 50% or more of their time in `malloc()`** wasn’t acceptable.

(emphasis mine :P)

He makes a compelling argument, which I agree with, but doesn't mention the `new()` macros that basically did the same thing. Although you'll find many programs in V8 that do the latter, verbose thing. Now I want to know the author(s) of those macros even more!

### Sims

This is a big parentheses because I wanted to know how Simula came up with `new`. [Wikipedia](https://en.wikipedia.org/wiki/Simula "Simula") states:

> Simula 67 introduced objects, classes, inheritance and subclasses, virtual procedures, coroutines, and discrete event simulation, and featured garbage collection.

And it appeared in **1967**. Wow! In the [1964 report](https://www.mn.uio.no/english/services/it/simula/versions/simula-i/simula-i-v5.pdf) of SIMULA I, you'll find:

> \[...\] The symbol "`new`" only serves to resolve an ambiguity of the identifier `A` inside the activity *blah blah blah* \[...\]. Elsewhere the symbol `new` is redundant, but can often be used to advantage to improve readability.

So it was only used to resolve grammar ambiguities (I think?). And Stroustrup re-purposed it. Clever!

Further reading

[*The History of Simula*](https://web.archive.org/web/20251027205559/https://users.dcc.uchile.cl/~cgutierr/cursos/LP/SimulaHistory.html "The History of Simula"), by Jan Rune Holmevik. Reads like (bad) Sci-Fi. All the politics nonsense aside, what I found most curious is that they completely specified the language before attempting to write a compiler for it. They started writing the first compiler *atleast a year after*. This would sound insane today, but I guess the cost of writing code was substantially higher back then.

Also: [*The development of the SIMULA languages*](https://dl.acm.org/doi/abs/10.1145/800025.1198392 "The development of the SIMULA languages"), by the authors. I didn't read that one, although I should...

It's honestly amazing how influential Simula was. In "10.5 Operating Instruction" they even use the term "run time system" and give it the same meaning we're accustomed to.

Simula, in turn, was partly inspired by SIMSCRIPT (don't laugh). If you skim its [1962 report](https://www.rand.org/content/dam/rand/pubs/research_memoranda/2009/RM3310.pdf "SIMSCRIPT: A SIMULATION PROGRAMMING LANGUAGE"), you won't find a mention of `new`. You'll however find `CREATE`:

> When a new Entity is created by a `CREATE` statement, space for storing its Attribute values is allocated in the amount specified on the Definition Form. This record is given an identification number, and the Variable specified in the `CREATE` statement is automatically set equal to this number.
>
> For example, the statement "`CREATE DOG CALLED FIDO`" allocates the required amount of storage for the Attributes of a `DOG`, and sets `FIDO` equal to the identification number of the Attribute record.

Fascinating. The whole SIMSCRIPT report is fascinating actually, and I only briefly skimmed through it. At the beginning, it even describes something like a primitive [ECS](https://en.wikipedia.org/wiki/Entity_component_system "Entity component system"), by my understanding. At the end though it slaps you back to reality:

![COMPILATION AND EXECUTION - SIMSCRIPT](https://napcakes.nekoweb.org/posts/malloc/simscript-deck.png "COMPILATION AND EXECUTION - SIMSCRIPT")

COMPILATION AND EXECUTION - SIMSCRIPT

and you remember what year it was.

### Later editions

In V9's [`/jtools/src/sam/sam.h:14`](https://www.tuhs.org/cgi-bin/utree.pl?file=V9/jtools/src/sam/sam.h), part of the [Sam](http://sam.cat-v.org/ "The Text Editor Sam by Rob Pike") editor by Rob Pike, you find a "generalised" version of `new()`:

```c
#define    new(t, n)       ((t *)alloc(((ulong)(n)*sizeof(t))))
```

Although, by and large, almost every program doesn't do this. Most just directly call `malloc()` and friends, and do size calculations \\w error checking at every call site. I don't know why, considering how much error-prone that is, but maybe due to inertia. It's a similar story in V10, but I didn't check it as thoroughly since it's quite big (it includes medieval GCC!).

## So what happened?

They were so close to arenas; with linear allocation and explicit memory regions, rolling back, and even having a nice `new()` macro. All this is speculation at this point, but maybe some of it is correct. First of all, most programs had trivial object lifetimes. Allocate memory, read some data, process/crunch and spit out some result. You don't need any disciplined memory allocation strategy for that. You can even use `malloc()` for everything and let the OS reclaim it once the process exits. Given how simple `malloc()` was at first, it wouldn't be much of a bottleneck either. The philosophy of programs only "doing a single thing well" also fitted this pattern.

Secondly, it's clear some decisions were motivated by the very little memory they had to play with. And there was no virtual memory either (initially, that is). Nowadays, you can "allocate" however big of a memory region you want and all will probably be okay. They couldn't really do that, so they were fighting to reclaim every empty block and gap there was and scale a program's space with how much is actually needed. There's a counterpoint to this, however, because some code is confused. Sometimes you see fixed-size buffers/arrays pop up, seemingly out of nowhere, and the program bail out if they fill. I'm not sure why, but perhaps they didn't deem it worthwhile the complexity to use `malloc()` in those cases and/or "don't fix it if it ain't broke" kind-of thing.

And clearly, *when* they could get away with simple linear allocation, they did so.

Thirdly, I suspect they had grown tired of doing *any* manual memory management at all. Dropping explicit memory regions from the `malloc()` interface is a change that, on the surface, simplifies stuff even further. You make the implementation-side smarter and you don't have to worry about passing `coremap` or `swapmap` or `anymap`. Remember: the way programs and problems were approached didn't lead to complex object lifetimes. And when you had to deal with tangled lifetimes, `malloc()`/`free()` appear deceivably simple in the beginning.

That last point seems more "vibes" than evidence, but there is some evidence actually. In V7 they added [`galloc(3)`](https://man.cat-v.org/unix_8th/3/galloc):

```pre
GALLOC(3)                                               GALLOC(3)

NAME
     galloc, gfree, garbage - storage allocation with garbage
     collection

SYNOPSIS
     char *galloc(n)
     unsigned n;

     gfree(p)
     char *p;

     garbage( )

DESCRIPTION
     These functions perform heap storage allocation with garbage
     collection.

     Galloc allocates a block of at least n bytes and returns a
     pointer to it.  Gfree frees a block previously allocated by
     galloc.

     When space gets tight, garbage blocks are freed automati-
     cally.  A block allocated by galloc is deemed to be garbage
     unless it is reachable.  A reachable block is one whose
     first byte is pointed to by a declared C variable or by a
     pointer in a reachable block.
```

and Rob Pike had this in [`/jtools/src/sam/sam.h:15`](https://www.tuhs.org/cgi-bin/utree.pl?file=V9/jtools/src/sam/sam.h):

```c
#define    gcnew(p, n)     ((void)gcalloc((ulong)(n)*sizeof((p)[0]), (long **)&(p)))
```

Ha, myth busted! The authors and people closest to C *wanted garbage collection*. Given how fast computers are -- and the pace of that -- this makes sense. It's even clearer if you look at what they did later with Limbo and Go. They wanted a higher level general purpose language to write programs in. They had C, so they tried to bolt GC on top of that.

Lastly, I should note that many of these problems were largely known, even at the time. In *The Standard C library (1992)*, P. J. Plauger writes:

> Allocating and freeing data objects on the heap in arbitrary order inevitably leaves unusable holes between some of the allocated data objects. That too lowers the usable size of the heap.
>
> Don't overreact to this knowledge. Gather related data into a structure and allocate it all at once. That minimizes heap overhead, to be sure, but it is also good programming style. Do not gather unrelated data just to save heap overhead. Similarly, allocate data objects with similar lifetimes all at once, then free them at about the same time. That minimizes heap fragmentation, but it too is good style. Do not advance or defer unrelated heap operations just to minimize fragmentation. The storage allocation functions are an important aid to programming flexibility. Use them as they are intended to be used.

Criticism by yours truly: It's much easier to say "just do the right thing™" than show and provide tools for how to actually do it.

## Conclusion

I don't know. Pay attention to good ideas, otherwise they might get lost? (literally in the case of V4, lol) Sounds cheesy.

Also, the "C object code improver" shatters the illusion that C was somehow magically fast. In fact, Ritchie later added an expression optimiser, and they kept extending both in each edition. I think C just wasn't slow, and it was made fast only with engineering effort put into the compiler (and even then, they had to add `register` because the compiler wasn't that smart either). It probably remained fast because Unix spread quickly and it carried C along with it, so hardware manufacturers adapted *to C*. There's a nice essay by [Mike Pall](https://www.freelists.org/post/luajit/Ramblings-on-languages-and-architectures-was-Re-any-benefit-to-throwing-off-lua51-constraints "Ramblings on languages and architectures (was Re: any benefit to throwing off lua51 constraints?)") discussing how modern CPUs try very hard to infer invariants from compiled C code (which the compiler tried very hard to optimise) and the costs associated with that.

At one point, I also want to investigate Inferno and Plan 9 more. Maybe the BSDs too. Until then...

Cya!

```
~/V8$ grep -Rn <redacted> .
```

```
./usr/src/cmd/c2/c21.c:660:			if (p->op==ASH && preg==(regs+RT1+1)) cp2[-1]=BYTE; /* stupid DEC */
./usr/src/cmd/c2/c22.c:11:/* cursed be the preprocessor, whose impotence and stupidity
./usr/src/cmd/adb/main.c:16:int xargc;		/* bullshit */
./usr/src/cmd/ccom/vax/gencode.c:644:	case LS:	/* stupid vax */
./usr/src/cmd/ccom/vax/gencode.c:664:		if(incrsize(p) != 4) {	/* stupid vax */
./usr/src/cmd/ccom/vax/gencode.c:705:		 * instead, generate crummy code (stupid assembler)
./usr/src/cmd/ccom/vax/genaux.c:598:	op->in.op -= (ASG 0);	/* crap for crap */
./usr/src/cmd/ex/ex.c:352:		 * Don't have to be upward compatible with stupidity
./usr/src/cmd/ex/exrecover.c:492:	 * This is old and stupid code, which
./usr/src/cmd/ex/ex_put.c:965: * This is so idiots who make esc their start/stop don't lose.
./usr/src/cmd/upas/send/send.c:288:	/* check forwarding (what anti-social crap!) */
./usr/src/cmd/uucp/Makefile:2:# this makefile is full of crap
./usr/src/cmd/rarepl/rct.h:2: * first block of rct: random crap
./usr/src/cmd/sdb/machine.h:16:struct {short hiword; short loword;}; /* stupid fp-11 */
./usr/src/libc/gen/regcomp.c:26:/* predeclared crap */
./usr/sys/dev/uda.c:951:	register u_short *sp;		/* for sdi crap */
./usr/sys/dev/uda.c:1123:		 * should do read-after-write crap
./usr/sys/dev/flp.c:206:			 * We're really stupid today - call it an
./usr/sys/dev/kmc.c:9:/* nonstandard crap */
./usr/sys/dev/vaxcpu.c:36: * Information to patch around the stupidity of configuration
./usr/sys/sys/sys1.c:488:		else	/* stupid 1k 0413 files */
./usr/sys/sys/vmproc.c:238: * when large procs stupidly fork() instead of vfork().
./usr/sys/boot/bb/bblock.s:117:	movl	r5,r11		# put boot flags in stupid bky place
./usr/sys/boot/bb/bblock.s:118:	movzbl	$DEVNUM,r10		# more berkeley bullshit - 7 == uda50
./usr/sys/boot/bb/1k.s:2:	.set	DEVNUM, 8	# stupid hack
```

(Kindly let me know if I made some mistake :)
