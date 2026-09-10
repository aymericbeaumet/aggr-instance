---
title: 'Bespoke: A programming language for people who say please'
link: https://blog.hofstede.it/bespoke-a-programming-language-for-people-who-say-please/
source: hnrss-org-frontpage
published: 2026-09-06T08:13:53Z
updated: 2026-09-06T08:13:53Z
first_seen: 2026-09-10T04:31:32.185563254Z
authors:
- birdculture
summary: 'Article URL: https://blog.hofstede.it/bespoke-a-programming-language-for-people-who-say-please/ Comments URL: https://news.ycombinator.com/item?id=49584361 Points: 145 # Comments: 34'
content: extracted
html: 2026-09-06-bespoke-a-programming-language-for-people-who-say-please.html
preview:
  file: 2026-09-06-bespoke-a-programming-language-for-people-who-say-please.preview-b78885ab6e1c.webp
  width: 256
  height: 139
  alt: A Victorian programmer politely presenting a handwritten program to a deeply unimpressed brass computer
  color: '#433423'
images:
- source: https://blog.hofstede.it/images/2026-09-04-bespoke-programming-language.webp
  original:
    file: 2026-09-06-bespoke-a-programming-language-for-people-who-say-please.image-5dec5541738b.webp
    width: 1702
    height: 924
  variants:
  - file: 2026-09-06-bespoke-a-programming-language-for-people-who-say-please.image-81b1a56d9142.webp
    width: 48
    height: 26
  - file: 2026-09-06-bespoke-a-programming-language-for-people-who-say-please.image-964066206a20.webp
    width: 320
    height: 174
  color: '#1b130a'
---

* * *

![A Victorian programmer politely presenting a handwritten program to a deeply unimpressed brass computer](https://blog.hofstede.it/images/2026-09-04-bespoke-programming-language.webp "Submitting a Bespoke program for the compiler's esteemed consideration")

Programming has developed an unfortunate tone.

We `kill` processes, `abort` transactions, `throw` exceptions, `break` out of loops, and `execute` instructions. We acquire locks without asking, mutate values without apologising, and order the machine to `return` as though it were a Labrador. Even the gentler languages expect us to address the runtime in a sequence of terse imperatives:

```
let carriageCount = 0;
carriageCount = 42;
console.log(carriageCount);
```

No salutation. No explanation. Not so much as a *much obliged*.

This will not do.

I therefore propose **Bespoke**, a statically typed and uncompromisingly civilised programming language for developers who believe that machine execution should never come at the expense of good manners. Informally it is known as *The Queen’s Code*: Victoria’s, naturally; the etiquette committee has yet to approve the twentieth century. Source files use the `.charming` extension, and the compiler reserves the right to be disappointed in you.

The implementation is, at present, somewhat less advanced than the etiquette manual. This is entirely appropriate: one does not rush a formal introduction.

(No relation to [Josiah Winslow’s Bespoke](https://pypi.org/project/bespokelang/), the 2025 esolang based on Poetic that encodes instructions through word lengths. We regret the social inconvenience.)

## Table of Contents

- [Table of Contents](https://blog.hofstede.it/bespoke-a-programming-language-for-people-who-say-please/#table-of-contents)
- [The Compiler Is Not Your Servant](https://blog.hofstede.it/bespoke-a-programming-language-for-people-who-say-please/#the-compiler-is-not-your-servant)
- [A Type System of Proper Breeding](https://blog.hofstede.it/bespoke-a-programming-language-for-people-who-say-please/#a-type-system-of-proper-breeding)
- [Mutation Requires an Apology](https://blog.hofstede.it/bespoke-a-programming-language-for-people-who-say-please/#mutation-requires-an-apology)
- [Control Flow by Deliberation](https://blog.hofstede.it/bespoke-a-programming-language-for-people-who-say-please/#control-flow-by-deliberation)
- [Functions as Diplomatic Proposals](https://blog.hofstede.it/bespoke-a-programming-language-for-people-who-say-please/#functions-as-diplomatic-proposals)
- [Regrettable Circumstances](https://blog.hofstede.it/bespoke-a-programming-language-for-people-who-say-please/#regrettable-circumstances)
- [Concurrency Without the Shoving](https://blog.hofstede.it/bespoke-a-programming-language-for-people-who-say-please/#concurrency-without-the-shoving)
- [Diagnostics of Wounded Dignity](https://blog.hofstede.it/bespoke-a-programming-language-for-people-who-say-please/#diagnostics-of-wounded-dignity)
- [A Complete Programme](https://blog.hofstede.it/bespoke-a-programming-language-for-people-who-say-please/#a-complete-programme)
- [The Future of Polite Computing](https://blog.hofstede.it/bespoke-a-programming-language-for-people-who-say-please/#the-future-of-polite-computing)
- [When All Else Fails](https://blog.hofstede.it/bespoke-a-programming-language-for-people-who-say-please/#when-all-else-fails)

## The Compiler Is Not Your Servant

Bespoke begins with one simple observation: the compiler is an esteemed collaborator, not a menial functionary. It will not accept an unadorned command. Every request must open with an appropriate form of address, state its purpose courteously, and conclude with an approved acknowledgement. Control-flow blocks supply their own ceremonial openings and closings.

Allocations and updates are addressed to the Compiler; terminal output, including standard error, to the Output Console; filesystem operations and runtime services to the Host Environment. Shared objects and worker threads receive their correspondence directly. One does not ask the groundskeeper to announce the guests.

Declaring an integer consequently looks like this:

```
Dear Compiler,
Would you be so exceptionally kind as to allocate a slot for An Integer Number
known henceforth as carriageCount, initialised with the value 0?
Thank you ever so much.
```

This is more verbose than `int carriageCount = 0`, but considerably less likely to create a hostile work environment.

Punctuation is grammatical rather than ornamental. Statements end with full stops, inquiries with question marks, and a block with a suitable sign-off. Semicolons are rejected as the conversational equivalent of slamming a door.

## A Type System of Proper Breeding

Bespoke is statically typed because allowing a value to pretend to be something it is not would be dishonest. It is also nominally typed because introductions matter. Types are written in full; clipped little vulgarisms such as `int`, `char`, and `bool` have no place in polite source code.

| Bespoke type                 | Less refined equivalent                     | Example                     |
| ---------------------------- | ------------------------------------------- | --------------------------- |
| `An Integer Number`          | 64-bit signed integer                       | `42`                        |
| `A Rational Fraction`        | Exact ratio of arbitrary-precision integers | `355/113`                   |
| `A Textual Passage`          | UTF-8 string                                | `"Good day to you."`        |
| `An Individual Character`    | Unicode scalar value                        | `'£'`                       |
| `A Truth Value`              | Boolean                                     | `Quite True`                |
| `A Distinguished Colour`     | First-class colour                          | `Colour.BritishRacingGreen` |
| `A Regrettable Circumstance` | Exception                                   | `LedgerMissingMisfortune`   |

British orthography is enforced during lexical analysis. `Colour` is the namespace for values of `A Distinguished Colour`. `Color` is evidence that the source has arrived without a proper education. Initialisation has an `s`, centre is spelled correctly, and an optimiser may remove redundant work but will never *optimize* it.

There are precisely two truth literals: `Quite True` and `Patently False`. A third, implementation-defined state named `Perhaps, Though I Should Not Like to Presume` was considered for database work and rejected as insufficiently deterministic.

## Mutation Requires an Apology

Functional programmers are correct that mutable state is troublesome, but prohibition feels rather severe. Bespoke permits mutation provided that the programmer acknowledges the inconvenience:

```
Dear Compiler,
If it causes you no undue hardship, might we trouble you to update the value
of carriageCount to 42?
Much obliged.
```

The compiler is under no obligation to accept a mutation phrased as an order. `carriageCount = 42` produces diagnostic B-101, **Blunt Imperative**, and compilation is suspended until the developer has reflected upon their conduct.

Memory management follows the same principle. Automatic garbage collection is not assumed; one formally engages the groundskeeper:

```
Dear Host Environment,
Pardon the intrusion, but might the groundskeeper occasionally tidy away
any allocations no longer reachable by the programme, at your leisure?
Yours gratefully.
```

Manual deallocation is available for performance-sensitive correspondence, although discarding an allocation without a sincere apology is undefined behaviour and, worse, common.

## Control Flow by Deliberation

An `if` statement is an accusation: *this condition is true, now do as I say*. Bespoke instead places a hypothetical question before the court.

```
Should it please the court to consider whether carriageCount is strictly greater than 10,
    Dear Output Console,
    Kindly convey to the standard stream the Textual Passage "Capital progress, indeed!".
    Thank you kindly.
Or, should the contrary prove true,
    Dear Output Console,
    Kindly convey to the standard stream the Textual Passage "A modest showing, regrettably.".
    Thank you kindly.
Thus concludes the matter.
```

For a given truth value, the choice of branch is deterministic. The court is ceremonial and cannot be lobbied by either side.

Closings follow a register: ordinary requests may use any approved expression of thanks, including `Much obliged` and `Ever grateful`; `Cheers` is accepted only in local scopes, while public library APIs require `Yours faithfully`. Compiler flags can tighten this under `--deference=white-tie`. Reports of misfortune and invitations to retire have their own approved valedictions.

Loops are permitted, though repeatedly asking the computer to do the same thing can try its patience. Returning `carriageCount` to 0 requires another apology; the loop then explicitly recognises the burden of repetition:

```
Dear Compiler,
If it causes you no undue hardship, might we trouble you to update the value
of carriageCount to 0?
Much obliged.

Dear Compiler,
Whilst it remains Quite True that carriageCount is strictly less than 3,
might we impose upon your patience to execute the following:
    Dear Output Console,
    Pray convey to the standard stream the value of carriageCount.
    Thank you.

    Dear Compiler,
    Would you mind terribly advancing carriageCount by the value 1?
    Cheers.
We are deeply indebted for your forbearance.
```

## Functions as Diplomatic Proposals

A function does not seize control and return a value. It proposes a small, bounded collaboration:

```
To Whom It May Concern,
I hereby propose a diplomatic procedure designated calculateFare,
accepting as input:
    An Integer Number known as distanceInMiles,
    A Truth Value known as isFirstClass,
and ultimately returning An Integer Number.

May we respectfully undertake the following:
    Should it please the court to consider whether isFirstClass is Quite True,
        I humbly submit that the appropriate return value is distanceInMiles multiplied by 4.
    Or, should the contrary prove true,
        I humbly submit that the appropriate return value is distanceInMiles multiplied by 2.
    Thus concludes the matter.
Thus concluded.
Yours faithfully.
```

The verbosity has a practical advantage. Nobody has ever opened a Bespoke code review and complained that the function’s contract was unclear. They have complained that it was twelve pages long, but that is a separate metric.

## Regrettable Circumstances

The phrase *throw an exception* is needlessly aggressive. In Bespoke, a component reports an awkward predicament and humbly raises it for gracious consideration:

```
Dear Host Environment,
I am dreadfully sorry to report an awkward predicament:
A Regrettable Circumstance designated LedgerMissingMisfortune has arisen,
specifically: "The parchment could not be located in the archives."
Might I humbly raise this for your gracious consideration?
With profound regret.
```

Nor does another component *catch* the poor thing. Contingency handling is a discreet attempt followed by a compassionate reception:

```
May we venture to attempt the following, with the greatest discretion:
    Dear Host Environment,
    Kindly inspect the ledger at "parchments/annual_audit.csv".
    Thank you kindly.
Should our modest enterprise regrettably miscarry,
and A Regrettable Circumstance designated LedgerMissingMisfortune be raised:
    Dear Output Console,
    Pray whisper to the standard error:
        "A dreadful pity; we shall brew fresh tea instead.".
    Much obliged.
In any eventual outcome, whether triumph or tragedy:
    Dear Host Environment,
    Would you be so exceptionally good as to restore the desk to an orderly state?
    Yours faithfully.
Thus concludes our contingency.
```

This is structurally equivalent to `try`, `catch`, and `finally`, but it gives the filesystem room to retain its dignity.

## Concurrency Without the Shoving

Concurrent programming reveals how violent our vocabulary has become. Threads compete. They race. They seize locks. The loser starves. Eventually somebody kills the process.

Bespoke threads behave better. A mutex is a **Request for Exclusive Audience**:

```
Dear SharedLedger,
If it would not cause you the slightest inconvenience,
might I venture to crave exclusive audience for a brief contemplation?
I pledge to retire the moment our business concludes.
Yours most considerately:
    [... protected operations ...]
I release you from my tedious presence with boundless gratitude.
```

Provided every access to the shared ledger observes this protocol, its data is protected from races. The order in which callers obtain an audience may still vary: manners do not determine thread scheduling. Nor do they eliminate deadlocks.

When Thread Alpha and Thread Beta each require a resource held by the other, both insist that the other proceed first:

> **Thread Alpha:** After you, my good sir.
>
> **Thread Beta:** Under no circumstances! I insist: after you.

Neither would dream of pushing ahead rudely, so the program remains bowed at the doorway until the heat death of the universe. The runtime records this not as a deadlock, but as an **Exemplary Stalemate of Mutual Deference**, notes its admiration for the threads’ upbringing, and tolerates the pause indefinitely.

For the same reason, threads are never killed or terminated. They receive a formal invitation to retire:

```
Dear Companion Worker Thread,
While your service has been an unmitigated delight,
might we gently suggest that the hour grows late and tea is served?
Perhaps you might see fit to conclude your earthly endeavours
at your earliest convenience?
With highest regards.
```

This is cooperative cancellation with a dress code.

## Diagnostics of Wounded Dignity

Most compilers respond to a mistake by dumping a stack of punctuation on the terminal. The Bespoke compiler expresses precise but restrained disappointment.

```
Diagnostic B-101 (Blunt Imperative)
Pardon me, but on line 14 you wrote 'x = 5;'. We are not
cattle-herders. Please frame your request as a civil inquiry.

Diagnostic B-204 (Vulgar Orthography)
Uncouth orthography detected on line 22: 'set_color'. The letter
'u' is not optional in polite society. Did you mean 'set_colour'?

Diagnostic B-310 (Insufficient Gratitude)
Your petition on line 88 was impeccably phrased, but lacked a
concluding 'Thank you'. The request has been quietly declined.
```

Warnings are delivered on cream paper where a suitable printer is available. `--quiet` suppresses compliments, never criticism.

## A Complete Programme

With the preliminaries settled, here is Hello World in its entirety:

```
To the Most Honourable Compiler of the Realm,

I have the distinct honour of presenting the Main Entry Point
for your esteemed consideration.

May it please your grace:
    Dear Compiler,
    Would you be so kind as to allocate a slot for A Textual Passage
    known as salutation, initialised with "Good day, planet Earth."?
    Thank you kindly.

    Dear Output Console,
    Pray convey to the standard stream the contents of salutation.
    Ever grateful.

    I respectfully conclude this routine, offering An Integer Number
    representing the exit status of 0.
Thus concludes our business.

I remain, sir, your most humble and obedient servant,
Arthur Pendleton, Esq.
```

The programme contains one variable, one output operation, and no ambiguity about who is speaking to whom. At 676 characters, counting spaces and LF line breaks including the final newline, it also ensures that storage manufacturers retain a reason to innovate.

## The Future of Polite Computing

Bespoke will not make software faster. Its proposed binaries are conventional, but its source has an information density comparable to a nineteenth-century treaty. Nor will it prevent concurrency bugs, as the exemplary stalemate demonstrates.

It does, however, ask an important question: if we are going to spend our days demanding impossible things from machines, could we at least be pleasant about it?

The reference compiler will be released as soon as it has finished considering my letter of intent. I sent it first class and enclosed a stamped, self-addressed envelope.

## When All Else Fails

A programme may still attempt division by zero, exhaust its memory, or suffer a production deployment at 16:55 on Friday. Bespoke can at least fail with grace. It does not panic or crash. The execution engine tenders its resignation in a sealed envelope:

```
======================================================================
                       A FORMAL APOLOGY
======================================================================
To: The Respected User
From: The Runtime Subsystem

Sir/Madam,

It is with the deepest personal humiliation that I must confess my
inability to proceed with line 84. A division by zero was solicited.
Whilst I attempted to interpret the request with all due charity,
mathematical propriety and the laws of the realm forbid it.

Rather than cause a scene, I have taken the liberty of stepping down
from my post.

I remain,
Your broken-hearted and disgraced servant,
The Bespoke Execution Engine.
======================================================================
```

The exit status is still non-zero. We are civilised, not delusional.
