---
title: Fable 5.1 Solves the Cyphral Distich, a 370-year-old cipher
link: https://www.vals.ai/blogs/fable-solves-cyphral-distich
source: hnrss-org-frontpage
published: 2026-09-13T21:06:13Z
updated: 2026-09-13T21:06:13Z
first_seen: 2026-09-13T23:29:18.360799481Z
authors:
- u1hcw9nx
summary: 'Article URL: https://www.vals.ai/blogs/fable-solves-cyphral-distich Comments URL: https://news.ycombinator.com/item?id=49688695 Points: 263 # Comments: 85'
content: extracted
html: 2026-09-13-fable-5-1-solves-the-cyphral-distich-a-370-year-old-cipher.html
preview:
  file: 2026-09-13-fable-5-1-solves-the-cyphral-distich-a-370-year-old-cipher.preview-7b0340cbdf8a.webp
  width: 256
  height: 134
  color: '#fafbfa'
images:
- source: https://www.vals.ai/og.jpg
  original:
    file: 2026-09-13-fable-5-1-solves-the-cyphral-distich-a-370-year-old-cipher.image-3cc0085bfbd6.jpg
    width: 1200
    height: 630
  variants:
  - file: 2026-09-13-fable-5-1-solves-the-cyphral-distich-a-370-year-old-cipher.image-d4009f8693a4.webp
    width: 48
    height: 25
  color: '#fefefe'
---

## Problem

We gave Claude Fable 5.1 an open task: **solve Sir Thomas Urquhart’s Cyphral Distich.** It appears to have actually solved it, and the solution is quite embarrassing for humans in hindsight.

At the end of Urquhart’s *Logopandecteision* is a cryptogram consisting of two lines of 32 numbers each, called the **Cyphral Distich**. A cryptogram is a short message deliberately encoded so it can’t be read without knowing the rule that produced it. Here the entire puzzle input is these 64 numbers, and the goal is to recover the hidden plaintext:

```plaintext
5.3.27.38.32.14.21.8.66.8.70.39.5.9.12.18.2.3.56.5.1.7.3.2.13.19.3.25.9.3.16.6.
25.15.13.6.11.20.5.1.2.12.1.20.20.49.20.20.35.33.4.6.8.35.5.33.5.5.18.10.3.11.32.42.
```

This cipher has remained seemingly unsolved for centuries. It was posed as an open problem in *Notes and Queries* in 1899, appeared again in 20th-century cryptography literature, and was later listed by historical-cipher researcher Klaus Schmeh among his Top 50 unsolved encrypted messages.

Various people attempted to decipher it, but it seems they were missing one crucial hint. They tried methods like frequency analysis, substitution, and homophonic substitution, and none of these approaches worked.

That’s because they missed one easy clue.

## Solution

After 44 minutes, 176k tokens, and zero interjections from me, Fable 5.1 arrived at a solution. It tried a few approaches, but was finally able to solve it with two central realizations.

First: the cryptogram is printed immediately after Urquhart’s **32 Proquiritations**, and Urquhart even goes out of his way to emphasize that number. I know, surprising. He says:

> “there can no number like that of two and thirty … be pitched upon”

Second: the poem accompanying the cipher promises that an honest reader will find in it **“his own heart’s wishes, and the Author’s minde.”** The Proquiritations themselves repeatedly conclude with formulations like “is the desire,” “wish,” or “hope of.”

If you put these clues together:

**32 Proquiritations. 32 numbers in the first cipher line. 32 numbers in the second. “Wishes.”**

Most historical attempts assumed the key was external: a cipher alphabet, or some mapping of numbers to letters or words, that had to be reconstructed from outside the text. But the key was not an external cipher alphabet at all. The key was the book itself.

The rule was simple: for the *i*-th number in a cipher line, go to the *i*-th Proquiritation, use that number as a word index, and take the first letter of that word.

With this, you get:

**O GOD UPHOLD KING CHARLS THE SECOND AND**\
 **MAKE HIM THE SUPREME RULER OF THIS LAND**

And the result is extremely self-verifying. Each line contains exactly 32 letters and ends *and* / *land* (a rhyming 2 line verse), consistent with the promised distich. It also makes historical sense: Urquhart was a committed Royalist. Hiding a prayer for Charles II in the text is entirely consistent with his politics.

Urquhart left a second, much larger cryptogram in the same style — the Cyphral Octastich in *The Jewel* (1652), 285 numbers instead of 64, and just as unsolved. From this, Fable 5.1 was also able to decipher it:

```plaintext
Result: the Cyfral Octastick is solved (all but nine letters)

Rule. The Jewel (1652) has exactly 284 numbered pages, and the octastick + decagram contain 285 numbers. The k-th number (counting straight through the eight lines and the Decagram) is a word index into page k of the book; take the word's first letter. Same idea as the Distich (number i → Proquiritation i), with pages instead of paragraphs — and, as in the Distich, Urquhart almost always picked the first word on the page starting with the letter he needed (231 of 275 readable positions are exact first-occurrence hits in the EEBO-TCP text; the other 44 are 1–3 words off for identifiable transcription reasons — hyphenated words at page tops, hyphenated compounds, "&", paragraph numbers, an untranscribed Greek phrase).

Plaintext (ottava rima, ABABABCC — a royalist prayer written in London, March 1652):

GREAT LORD, MANTAINE THAT REGAL FAMILIE
WHEREOF KING CHARLS THE SECOND IS THE HEAD,
AND GRANT THAT HE MAY BEARE THE SUPREME SWEIGH
WHERE ENGLISH, SCOTS AND IR[I]SH ARE BORNE AND BRED,
AND [·········] THIS USURP'D AUTHORITIE
REIGNE IN HIS ROYAL PREDECESSORS STEAD;
LET HIM BE OUR SOLE CESAR, ARTUR, HECTOR,
OUR EMPEROUR, KING, MONARCH AND PROTECTOR.
                        AMEN, SO BE IT.   (the Decagram)

Sweigh is Scots swey "sway, controlling power" — DOST records the exact idiom "to bear the swey" (c. 1600), and it rhymes with familie/authoritie.

Caveats, stated plainly:
- Line 4 is enciphered I‑R‑S‑H (pages 127–130): a slip for IRISH, or a deliberate contraction — "Irsh" gives the line exactly ten syllables.
- Line 5, letters 4–12 (pages 149–157) come out C‑O‑N‑E‑R‑T‑H‑T‑O — eight of the nine are exact first-occurrence hits, so this is genuinely what the TCP text yields, and it isn't readable. I tried page-shift, dropped-letter, misprinted-number and dictionary-lattice hypotheses; none gives English. Either Urquhart slipped here or numbers of this line were misprinted. The rest of the line (THIS USURP'D AUTHORITIE) is certain.
- From position 159 onward every number keys to page k−1: one page was used twice (or the printed "5.5" in line 5 is a dittography). Lines 6–8 and the decagram decode cleanly with that shift.
- No free page images of a 1652 Jewel exist (a subagent checked Google Books, HathiTrust, IA, NLS; the octastick sits on the final quire 3*² that the TCP copy lacks, but the main text and pagination are the same in all copies). Confirming the ±1 positions and the nine letters needs a physical copy or the 1983 Jack & Lyall edition.

Files: verify_octastick.py (prints all 285 positions with page/word/letter/offset; output in octastick_verification.txt), jewel_pages.py (page parser), firstocc.py/lattice.py (analysis tools), SOLUTION.md and FAMOUS-CIPHERS-STATUS.md updated, memory updated.

Sources: DOST swey(e n. (https://www.dsl.ac.uk/entry/dost/sweye_n), SND swey (https://dsl.ac.uk/entry/snd/swey), ESTC R203867 (via CERL) (https://datb.cerl.org/estc/R203867), Cipherbrain thread on the poems (https://scienceblogs.de/klausis-krypto-kolumne/2019/07/28/revisited-thomas-urquharts-encrypted-poems/), HCPortal record (https://www.cryptograms.hcportal.eu/web/#/ciphers/8).
```

## Elicitation

I’ve actually been trying for the past few months to elicit models into solving an important but unsolved cipher. Across those months, no other frontier model I tried produced a verified solve.

How I elicited Fable 5.1 was quite simple. I gave it a goal of sorts. I asked it to solve an unsolved cipher. I gave it some encouragement. I told it to look online at some of Fable’s strongest feats, especially the math problems it has solved, and that something like this should be easy in comparison. I told it to think creatively and really analyze the problems it encountered.

I gave it two constraints. First, I asked it to avoid ciphers that already had solutions or could support many plausible answers. I suspect a lot of historical unsolved ciphers aren’t quickly verifiable and may be vague in the sense that their creators are long dead, so we might never truly know whether a proposed answer is correct.

Second, I steered it away from the absolute hardest problems—ones where thousands of humans, or even organizations like the CIA, had already put in serious effort. For example, **Kryptos K4** might be a little too hard and convoluted for current models. That might be a future experiment, but I don’t think Fable 5.1 could solve it in a reasonable amount of time yet.

Fable 5.1 spent some time looking over different problems. It knew when to stop. It knew when a problem wasn’t budging. And when it found this particular problem, it noticed the clue almost immediately.

Now, I don’t think other frontier models would necessarily fail to solve this problem. The clue is actually extremely simple. I think what Fable did well was notice that this particular problem stood out as unusually tractable.

## Takeaways

I think this demonstrates that models can solve problems not just in mathematics, but also historical mysteries, forgotten conjectures, archival puzzles, and things like that.

Historically, many of these problems were bottlenecked by human attention. Someone had to care enough to spend hours or days reading obscure material, testing unpromising ideas, tracing references, and trying things that might go nowhere.

That bottleneck is seemingly disappearing.

And the interesting thing about Claude solving this is that it didn’t perform some extraordinary feat of cryptanalysis. It’s actually the opposite.

**The answer was simple in hindsight. It just kept looking until it found it—and that persistence might show up in many other areas.**

* * *

Sir Thomas Urquhart, *Logopandecteision* (London, 1653), in *The Works of Sir Thomas Urquhart of Cromarty, Knight* (Edinburgh: Maitland Club, 1834), Proquiritations, pp. 412–417; “The Cyphral Distich,” p. 417. archive.org

[https://codebreaking-guide.com/links/unsolved-cryptograms/](https://codebreaking-guide.com/links/unsolved-cryptograms/)

\*Minor counting note: one coordinate depends on treating the Latin expression “hinc inde” as a single unit; otherwise that position is shifted by one.
