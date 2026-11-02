---
title: Novellas with Groff
link: https://movq.de/blog/postings/2026-10-31/0/POSTING-en.html
source: hnrss-org
published: 2026-11-01T13:51:41Z
updated: 2026-11-01T13:51:41Z
first_seen: 2026-11-02T00:33:20.199752031Z
authors:
- dverlaeckt80
content: extracted
html: 2026-11-01-novellas-with-groff.html
remote_preview:
  url: https://movq.de/blog/postings/2026-10-31/0/t/pdfinfo-wc.png.jpg
  alt: 'pdfinfo-wc.png: Screenshot of the output of ''pdfinfo'' and ''wc'': About 50 pages / 15k words for the first document, 120 pages / 40k words for the second one.'
---

[blog](https://movq.de/blog/) - [git](https://movq.de/git/) - [desktop](https://movq.de/desktop/) - [contact](https://movq.de/contact.html)

* * *

2026-10-31

I recently finished my second "novella".

[![pdfinfo-wc.png: Screenshot of the output of 'pdfinfo' and 'wc': About 50 pages / 15k words for the first document, 120 pages / 40k words for the second one.](https://movq.de/blog/postings/2026-10-31/0/t/pdfinfo-wc.png.jpg "Screenshot of the output of 'pdfinfo' and 'wc': About 50 pages / 15k words for the first document, 120 pages / 40k words for the second one.")](https://movq.de/blog/postings/2026-10-31/0/pdfinfo-wc.png)

Don't even think about it: I won't publish this. Nor send it to you privately, which is mostly the same as publishing it.

The first story is roughly 50 [A5 pages](https://en.wikipedia.org/w/index.php?title=DIN_A5) long (~15k words), the second one is about 120 pages (~40k words). (Both barely fit the [definition of a "novella"](https://en.wikipedia.org/wiki/Novella), apparently, with the first one being a bit too short and the second one a tiny bit too long.)

It feels pretty absurd and surreal that I have done this at all. In school, *anything* related to "writing" was pure horror for me -- unless it was related to maths, physics, or other "technical" stuff. But here we are.

How long did it take? Well, the first one took over seven months. But the second one, the longer one? Ten days.

Some time last year, probably around July, I started reading some of the books that were lying around in my appartment for decades. And this was much more enjoyable than I thought. No idea why. It's not like I never read a book before, but it was always a somewhat boring thing to do and it took me ages -- unless it was something technical. I'm only referring to fiction and novels here. Like, one book per year, tops. Yet, I somehow got hooked last year and now I've read about 30 novels since July 2025. Some faster (a couple of days), some slower (three months).

It wasn't that much of a stretch to try to do this myself.

This blog is still technical in nature, so I'll walk through the software that I used.

* * *

Table of contents:

- [groff](https://movq.de/blog/postings/2026-10-31/0/POSTING-en.html#groff)
  - [Basic formatting](https://movq.de/blog/postings/2026-10-31/0/POSTING-en.html#basic-formatting)
  - [Footers and page numbers](https://movq.de/blog/postings/2026-10-31/0/POSTING-en.html#footers-and-page-numbers)
  - [Header](https://movq.de/blog/postings/2026-10-31/0/POSTING-en.html#header)
  - [A closer look at traps](https://movq.de/blog/postings/2026-10-31/0/POSTING-en.html#a-closer-look-at-traps)
  - [Inter-sentence spacing](https://movq.de/blog/postings/2026-10-31/0/POSTING-en.html#inter-sentence-spacing)
  - [German hyphenation](https://movq.de/blog/postings/2026-10-31/0/POSTING-en.html#german-hyphenation)
- [Markdown and other pre-processing](https://movq.de/blog/postings/2026-10-31/0/POSTING-en.html#markdown-and-other-pre-processing)
- [Vim](https://movq.de/blog/postings/2026-10-31/0/POSTING-en.html#vim)
  - [Goyo](https://movq.de/blog/postings/2026-10-31/0/POSTING-en.html#goyo)
  - [Reflowing text](https://movq.de/blog/postings/2026-10-31/0/POSTING-en.html#reflowing-text)
- [Git](https://movq.de/blog/postings/2026-10-31/0/POSTING-en.html#git)
- [demo-book.tar.gz: Putting it all together](https://movq.de/blog/postings/2026-10-31/0/POSTING-en.html#demo-booktargz-putting-it-all-together)
- [The writing process itself](https://movq.de/blog/postings/2026-10-31/0/POSTING-en.html#the-writing-process-itself)
- [Where to go from here?](https://movq.de/blog/postings/2026-10-31/0/POSTING-en.html#where-to-go-from-here)

* * *

## groff

Both of my books (they're not really "books", but I'll keep referring to them as such for the sake of simplicity) are typeset in [groff](https://www.gnu.org/software/groff/). You know, the thing that usually renders manual pages in your terminal on Linux. groff/troff is a full typesetting system, though, and it can be used to [produce real books](https://www.troff.org/pubs.html), which I've already mentioned a couple of times in [previous blog posts](https://movq.de/blog/postings/2025-08-30/0/POSTING-en.html).

[![troff-books-1.webp: Photo of my copies of 'The C Programming Language', 'The UNIX Programming Environment', and 'UNIX Network Programming'.](https://movq.de/blog/postings/2026-10-31/0/t/troff-books-1.webp.jpg "Photo of my copies of 'The C Programming Language', 'The UNIX Programming Environment', and 'UNIX Network Programming'.")](https://movq.de/blog/postings/2026-10-31/0/troff-books-1.webp) [![troff-books-2.webp: A page of 'The C Programming Language'.](https://movq.de/blog/postings/2026-10-31/0/t/troff-books-2.webp.jpg "A page of 'The C Programming Language'.")](https://movq.de/blog/postings/2026-10-31/0/troff-books-2.webp) [![troff-books-3.webp: A page of 'UNIX Network Programming', showing some diagrams.](https://movq.de/blog/postings/2026-10-31/0/t/troff-books-3.webp.jpg "A page of 'UNIX Network Programming', showing some diagrams.")](https://movq.de/blog/postings/2026-10-31/0/troff-books-3.webp)

In my case, the appeal of groff is that it's relatively small and *fast*, but still able to produce high-quality output. Making that 50-page document takes about *700 ms* on my 13.5 year old PC, and the 120-page one takes *1.6 seconds*. This includes some pre-processing which we'll cover later. Given that most modern software these days feels like walking in mud, this is refreshingly quick.

Because it's more fun that way, I made my own little macro package. There really isn't a lot to it, though. It's tiny. Not a lot is needed for a simple novel / novella -- no table of contents, no cross-references, no diagrams, no code blocks, no lists, ... Just nicely flowing text and *sometimes* a word gets emphasized.

Disclaimer: You'll note some TODO items in the text below. groff is a full-blown typesetting system and these things are complicated. I'm a long way from having mastered every aspect of it.

### Basic formatting

There are only three macros:

```
.\" Section header / chapter title
.de SH
.   sp 1v
.   ps +5p
.   ce
\f[HR]\\$1\fP
.   ps
.   sp 4v
..
.\" Normal paragraph with hanging indent.
.de PP
.   br
.   ti +2m
..
.\" "Long" paragraph, longer empty space.
.de LP
.   sp 2v
..
```

(The name `SH` is a spill-over from manual pages. I've written quite a few of those and this name somehow stuck. `CH` for "chapter heading" or `CT` for "chapter title" might have been better choices. Or, you know, `.CHAPTER`.)

You start defining a macro with `.de` -- anything until `..` is part of that macro.

- `.SH`: Go down one [vee](https://www.gnu.org/software/groff/manual/groff.html.node/Measurements.html), increase type size, request centering, print the macro's first argument with the `HR` font (Helvetica), return to previous type size, add another four vees of space.
- `.PP`: Just break the current line, then request a temporary indent for the next output line. This creates a hanging indent, which is customary in German books.
- `.LP`: Just some empty space between paragraphs. Used for things like intra-chapter time jumps.

You can look up the individual commands in `man 7 groff`.

And that would already be enough to produce a document that somewhat resembles a book.

Put the snippet above in a file called `macros.groff`. I made one file per chapter. For example: [`chapter01.groff`](https://movq.de/blog/postings/2026-10-31/0/chapter01.groff). You can then make a PDF using a command like this:

```
groff -Tpdf -dpaper=a5 -P-pa5 macros.groff chapter01.groff >book.pdf
```

A5 is roughly the page size of a "typical book".

### Footers and page numbers

You'll note that there are no page numbers by default. You could actually just decide to not have them if all you want is a digital PDF. They're useful for printing, but you don't really need them in a file, if we're being honest.

Except: The thing gets confused about page breaks. Create the PDF using the example above and you'll see:

[![wrong-page-break.webp: PDF viewer screenshot: Last line of text outside of the page.](https://movq.de/blog/postings/2026-10-31/0/t/wrong-page-break.webp.jpg "PDF viewer screenshot: Last line of text outside of the page.")](https://movq.de/blog/postings/2026-10-31/0/wrong-page-break.webp)

Looks like the baseline of the last line of output is just outside the page. Not sure what's going on here -- it's related to setting the page size to A5. In other words, I probably missed *another* spot where this has to be set. It'll be fixed implicitly by the following.

The whole page number thing consists of three parts: First, how large is the footer (the area where the page numbers live)? I defined this in a [register](https://www.gnu.org/software/groff/manual/groff.html.node/Setting-Registers.html) (which is not really necessary but I find the "code" easier to read this way) -- the name is arbitary:

```
.nr _FooterSize 5v
```

We than plant a [trap](https://www.gnu.org/software/groff/manual/groff.html.node/Page-Location-Traps.html) at the bottom of the page:

```
.wh -\n[_FooterSize]u _FooterTrap
```

`\n[_FooterSize]` "gets" the value of the register called `_FooterSize`. There's a minus sign in front of it, it's negative and relative to the bottom of the page. [The documentation says](https://www.gnu.org/software/groff/manual/groff.html.node/Default-Units.html):

> A general-purpose register (one created or updated with the nr request) is implicitly dimensionless, or reckoned in basic units if interpreted in a measurement context.

I'm not 100% sure why I need to add the `u`. I haven't read every page of the manual, yet. Needless to say, it doesn't work without it.

Okay, so the trap gets installed at 5v before the end of the page and it'll call the `_FooterTrap` macro. It looks like this:

```
.de _FooterTrap
.\" Push current environment on stack and switch to `_FooterEnv`.
.\" Parent env will keep partially collected lines.
.   ev _FooterEnv
.\" Copy settings from default environment.
.   evc 0
.\" Don't do this for pages 1 and 2, they're title pages in my actual
.\" "book".
.   if \\n[%]>2 \{\
.\" Set vertical position, explained in the text below.
.       sp |\\n[.p]u-2v
.\" Print % register centered.
.       ce
\fR\\n[%]\fP
\}
.\" Go back to previous environment.
.   ev
.\" Request page break.
'   bp
..
```

It's much easier to read without the inline comments:

```
.de _FooterTrap
.   ev _FooterEnv
.   evc 0
.   if \\n[%]>2 \{\
.       sp |\\n[.p]u-2v
.       ce
\fR\\n[%]\fP
\}
.   ev
'   bp
..
```

Why is it `'bp` instead of `.bp`? We'll get to that.

What I'm unsure about: I set the vertical position to `|\\n[.p]u-2v`. That's relative to the *top* of the page, it's the page length minus two vee. I would have expected `|-2v-1v` (including the "minus one vee" mentioned in the docs) to also work, but that leads to an infinite loop (probably triggers the same trap again and again). Why?

For symmetry, I also want some empty space at the top of the page:

```
.nr _HeaderSize 5v
.de _HeaderTrap
'   sp |\\n[_HeaderSize]u
..
.wh 0v _HeaderTrap
```

And another TODO for me: This isn't 100% symmetrical. The length of the header is a bit larger than the footer. An issue with the page size again?

### A closer look at traps

Let's have a closer look at traps. I'm not *100%* sure on this -- this is to the best of my understanding.

Actually, let's take a step back from the example above. Let's start with this:

```
This is a little excerpt from Mary Shelly's Frankenstein.

These visions faded when I perused, for the first time, those poets
whose effusions entranced my soul and lifted it to heaven. I also became
a poet and for one year lived in a paradise of my own creation; I
imagined that I also might obtain a niche in the temple where the names
of Homer and Shakespeare are consecrated. You are well acquainted with
my failure and how heavily I bore the disappointment. But just at that
time I inherited the fortune of my cousin, and my thoughts were turned
into the channel of their earlier bent.

Six years have passed since I resolved on my present undertaking. I can,
even now, remember the hour from which I dedicated myself to this great
enterprise. I commenced by inuring my body to hardship. I accompanied
the whale-fishers on several expeditions to the North Sea; I voluntarily
endured cold, famine, thirst, and want of sleep; I often worked harder
than the common sailors during the day and devoted my nights to the
study of mathematics, the theory of medicine, and those branches of
physical science from which a naval adventurer might derive the greatest
practical advantage. Twice I actually hired myself as an under-mate in a
Greenland whaler, and acquitted myself to admiration. I must own I felt
a little proud when my captain offered me the second dignity in the
vessel and entreated me to remain with the greatest earnestness, so
valuable did he consider my services.
```

Save it as `test.groff` and make a PDF:

```
groff -Tpdf test.groff >book.pdf
```

Looks like this:

[![traps-1.webp](https://movq.de/blog/postings/2026-10-31/0/t/traps-1.webp.jpg "traps-1.webp")](https://movq.de/blog/postings/2026-10-31/0/traps-1.webp)

Install a trap at 10v. It's empty so far and does nothing -- insert this at the very top of the file:

```
.de MyTrap
..
.wh 10v MyTrap
.\"
```

The PDF looks the same.

Now what happens when we change the trap macro to this?

```
.de MyTrap
FOO
..
```

This is the result:

[![traps-2.webp](https://movq.de/blog/postings/2026-10-31/0/t/traps-2.webp.jpg "traps-2.webp")](https://movq.de/blog/postings/2026-10-31/0/traps-2.webp)

When the output reached the word "fishers", it sprang the trap. "FOO" comes after "fishers" in the PDF: "fishers" triggered the trap, then "FOO" got inserted. I think it's as simple as that, isn't it?

Suppose we want to put "FOO" at the beginning of the line and we want nothing but that word on that line. "fishers" shall be put on the next line.

If I understand correctly, switching environments is key: The environment holds the currently collected line (among many other things). Switch to a new one and we get a new buffer/line -- one that's empty so far. We can then flush/break this line to have it included in the PDF, and then switch back to the previous environment:

```
.de MyTrap
.   ev MyTrapEnv
FOO
.   br
.   ev
..
```

Looks like this:

[![traps-3.webp](https://movq.de/blog/postings/2026-10-31/0/t/traps-3.webp.jpg "traps-3.webp")](https://movq.de/blog/postings/2026-10-31/0/traps-3.webp)

So far, so good. Now I want there to be an empty line before and after "FOO":

```
.de MyTrap
.   ev MyTrapEnv
.   sp
FOO
.   sp
.   ev
..
```

In the new environment, we issue `.sp`: It breaks and inserts space. Nothing was collected so far, so the break doesn't print anything at this point. Then we have `FOO` -- this isn't printed directly, it's just collected. The next `.sp` breaks again (thus printing "FOO") and then inserts more space.

[![traps-4.webp](https://movq.de/blog/postings/2026-10-31/0/t/traps-4.webp.jpg "traps-4.webp")](https://movq.de/blog/postings/2026-10-31/0/traps-4.webp)

Okay. Now. Suppose that printing "FOO" with those two lines around it is all we want to do inside of the trap. We then want the normal text to continue at some absolute position on the page, say at 17v.

So, how about the following? We go back to the previous environment and then issue an absolute location request:

```
.de MyTrap
.   ev MyTrapEnv
.   sp
FOO
.   sp
.   ev
.   sp |17v
..
```

Oops:

[![traps-5.webp](https://movq.de/blog/postings/2026-10-31/0/t/traps-5.webp.jpg "traps-5.webp")](https://movq.de/blog/postings/2026-10-31/0/traps-5.webp)

`.sp |17v` broke the already collected line (the one with just "fishers" in it) and *then* inserted the space. We don't want that break. (Also note that "fishers" is at the exact same position where we left off inside the trap.)

This can be avoided by using `'` instead of `.`:

```
'   The neutral apostrophe is the no-break control character, rec‐
    ognized where the control character  is.   It  suppresses  the
    (first) break implied by the bp, ce, cf, fi, fl, "in", nf, rj,
    sp,  ti,  and trf requests.  The requested operation takes ef‐
    fect at the next break.
```

So let's do this:

```
.de MyTrap
.   ev MyTrapEnv
.   sp
FOO
.   sp
.   ev
'   sp |17v
..
```

Result:

[![traps-6.webp](https://movq.de/blog/postings/2026-10-31/0/t/traps-6.webp.jpg "traps-6.webp")](https://movq.de/blog/postings/2026-10-31/0/traps-6.webp)

The example in [the documentation](https://www.gnu.org/software/groff/manual/groff.html.node/Page-Location-Traps.html) doesn't change environments. But I find it much easier to understand what's going when changing them. (And I hope I got it right.)

One final remark: Environment carry settings. `.ev foo` starts a completely new environment (unless an environment with that name already exists), it doesn't clone the previous one. That's why my macro package also does `.evc 0`.

As a little exercise, see what happens when you use `'sp |12v`, `'sp
|11.5v`, `'sp |11v`, all the way down to `'sp |9v`.

### Inter-sentence spacing

Only when writing this blog post, I found the following sentence in [the docs (Basics)](https://www.gnu.org/software/groff/manual/groff.html.node/Basics.html):

> If you don't start a sentence on a new line, put two spaces after the previous sentence. GNU troff then recognizes punctuation that ends a sentence, and inserts inter-sentence space accordingly.

I didn't do that, because this isn't customary in German. But now I see it. Sometimes, it looks like this in the PDF:

```
Foo. Bar.
```

And sometimes there's a little bit of extra space:

```
Foo.  Bar.
```

As far as I know, this is usually not done in German books. I disabled it with this request, which keeps "minimal inter-word spacing" at the default value of 12 but sets "supplemental inter-sentence spacing" to 0:

```
.ss 12 0
```

This isn't part of my macro package, but it comes first in the "index" file (`book.groff`, see demo package below).

### German hyphenation

Apparently, it's as simple as this:

```
.hla den
.hpf hyphen.den
```

`den` is "de(utsch) new": This includes the "new" German rules from 2006.

## Markdown and other pre-processing

I actually wrote both of my stories in plain groff, using the macros I explained above.

[Markdown](https://en.wikipedia.org/wiki/Markdown) has the big advantage of being "visually pleasing": It mostly looks like text that is intended to be read by a human being. groff isn't, really. I mean, it wasn't *horrible*, I wouldn't have written over 170 pages in groff if this was *pain*. It's fine. I use groff in several other places as well. But Markdown is a tiny little bit better, and I assume that it allows me to focus more on the creative process.

So, afterwards, I converted the second story to "Markdown". Didn't bother to do that for the first one. I put that in quotation marks because there are only very few elements, it's not full Markdown:

- `# Chapter title` at the top of the file.
- Empty lines between paragraphs, `---` for "long empty paragraphs".
- `*foo*` for emphasized text.

And that's it. Makes the conversion really easy.

I could have just use plain Markdown for this whole thing and created an HTML file. I already have the tooling for that -- that's what drives my blog. Maybe this could even produce a PDF file. But what would be missing there, among other things, would be an engine to *properly justify text*. It hurts my eyes to see justified text in HTML, it just looks like a complete disaster. The results that groff or TeX create are much, much better.

While we're at it, we can also do some basic character replacements: I have the German typographic quotation marks U+201E and U+201C directly on my keyboard, but [Guillemets](https://en.wikipedia.org/wiki/Guillemet) are preferred in books.

I first wanted to do all this in Python but then switched to a little Perl snippet, because it makes it easy to read the entire file and then do multiline-regex stuff on it. It goes something like this:

```
perl -0pe '
# Guillemets are preferred in books.
s/... German typographic quotes, see demo package .../... Guillemets .../g;

# Markdown "h1" into ".SH".
s/^# ([^\n]+)\n/.SH "\1"\n/smg;

# Markdown "hr" into ".LP".
s/\n---\n/.LP/smg;

# Markdown paragraphs into ".PP".
s/\n\n/\n.PP\n/smg;

# Markdown emphasize into groff font selection:
s/\*([^*]+)\*/\\fI\1\\fP/smg;
'
```

The output is still groff for my little macro package. But I can now write it like this:

```
# Kapitel 42: Die Antwort

Da war sie also. Die Antwort auf *alle* Fragen.

Und dann ging es weiter:

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Vestibulum
hendrerit blandit suscipit. Maecenas sed nisi nec ante mattis
vulputate. Pellentesque maximus leo id purus vestibulum dictum.
Donec tincidunt mollis viverra. Curabitur at ligula in turpis
pellentesque vehicula.

Duis hendrerit commodo mauris, non posuere leo vulputate ut. Nulla
finibus urna eget vehicula auctor. Mauris in posuere nibh. Vivamus
ut metus vel nisi gravida molestie vitae et tellus. Integer mi odio,
vestibulum eget laoreet vel, auctor nec ligula. Quisque eu placerat
quam, eu tincidunt mauris. Nam vestibulum lectus non leo dignissim,
non sagittis dolor venenatis.
```

This reads much nicer and there is syntax highlighting in Vim for it. Well, the `*emphasized*` parts, that is -- the rest doesn't matter:

[![vim-md.png: Screenshot of Vim with standard Markdown highlighting: Emphasized text uses the terminal's 'standout' attribute.](https://movq.de/blog/postings/2026-10-31/0/t/vim-md.png.jpg "Screenshot of Vim with standard Markdown highlighting: Emphasized text uses the terminal's 'standout' attribute.")](https://movq.de/blog/postings/2026-10-31/0/vim-md.png)

Speaking of Vim ...

## Vim

### Goyo

There is one super important [Vim](https://vim-classic.org/) plugin for this:

[https://github.com/junegunn/goyo.vim](https://github.com/junegunn/goyo.vim)

It hides almost all UI elements.

On top of that, I put the terminal window in fullscreen mode to hide my desktop.

And there you go. "Distraction-free writing" indeed. Maybe put on some music, but there's nothing else. No browser, no clock, no nothing. Just a typewriter with an easy undo function.

[![vim-photo.webp: Photo of Vim with Markdown highlighting and active Goyo, terminal in fullscreen mode.](https://movq.de/blog/postings/2026-10-31/0/t/vim-photo.webp.jpg "Photo of Vim with Markdown highlighting and active Goyo, terminal in fullscreen mode.")](https://movq.de/blog/postings/2026-10-31/0/vim-photo.webp)

The font is the 11x22 variant of [UW ttyp0](https://people.mpi-inf.mpg.de/~uwe/misc/uw-ttyp0/).

### Reflowing text

There's a popular way to format text in plain-text files:

```
This is one sentence.
This is another one.
And so on and so on.
One sentence per line, that's it.
Sentences can be short.
Or they can be pretty long, sometimes even exceeding the available width on your screen, thus requiring you to either scroll or have your editor soft-wrap it for you when displaying it.
```

As we've learned above from [the groff docs](https://www.gnu.org/software/groff/manual/groff.html.node/Basics.html), this actually carries meaning for groff (properly detect the ends of sentences). And it makes the diffs in VCS's less noisy.

Another popular way would be to put each paragraph on its own line. Like, there's only one "physical" newline character after the whole thing and it's your editor's job to properly display this:

```
This is one sentence. This is another one. And so on and so on. One sentence per line, that's it. Sentences can be short. Or they can be pretty long, sometimes even exceeding the available width on your screen, thus requiring you to either scroll or have your editor soft-wrap it for you when displaying it.
```

I used the second version for a couple of years at the end of the 1990ies and the early 2000s, but eventually switched to hard-wrapped text. Like this:

```
This is one sentence. This is another one. And so on and so on. One
sentence per line, that's it. Sentences can be short. Or they can be
pretty long, sometimes even exceeding the available width on your
screen, thus requiring you to either scroll or have your editor
soft-wrap it for you when displaying it.
```

I just find this easier to read and easier to navigate in. And it reminds me of the "good" old DOS days, where I used [the `T` editor](https://ecsoft2.org/t-tiny-editor) a lot and all my files looked like that (manually formatted, of course). It does have drawbacks, yes. It's not great for collaboration with other people, but that doesn't matter in my case.

Another drawback is that I have to reformat these paragraphs while editing. Luckily, Vim makes that easy:

```
" In ~/.vimrc:
set nojs
map Q gq}
```

I can then just put my cursor at the beginning of a paragraph:

```
[T]his is one sentence.
This is another one.
And so on and so on.
One sentence per line, that's it.
Sentences can be short.
```

And hit `Q` to turn it into this:

```
This is one sentence. This is another one. And so on and so on. One
sentence per line, that's it. Sentences can be short.
[_]
```

The cursor will be positioned after that paragraph. To reflow the entire document, I can just keep `Q` pressed down until it hits the end.

*-- edit: Or do `:set formatoptions+=a` to have paragraphs reformatted automatically while editing it. I usually don't want that, I usually want it to be an explicit operation. But for writing prose, this might be pretty useful.*

The `nojs` option (funny name, huh?) was new to me, only found out about it just now. The full name is `nojoinspaces` and it prevents the very thing that I *should* be doing (but which is not customary in German): It does *not* insert two spaces after a period. Without it, i.e., with `set js`, the result would look like this:

```
This is one sentence.  This is another one.  And so on and so on.
One sentence per line, that's it.  Sentences can be short.
```

Some people like that, I don't.

## Git

As with virtually everything I do, this is in Git.

But that's the end. I regularly commit the current state, but I hardly care about anything else. No proper commit messages, nothing. It's just a tool for making "backups" easier and to sync the data between my laptop and desktop PC.

## `demo-book.tar.gz`: Putting it all together

As I said, I won't publish the actual "books". But here's a demo of the "scaffold":

[demo-book.tar.gz](https://movq.de/blog/postings/2026-10-31/0/demo-book.tar.gz)

(I won't update this. This is just a snapshot of the current status.)

## The writing process itself

I won't say much about this. I'm a total noob.

I changed course a couple of times. For both stories, I tried to draft the whole thing before I began writing the actual text, but that didn't help much. The first story isn't that great anyway. It has an interesting premise, but I wasn't really "in the zone". That's also why it took so long. The second story has a much more mundane basic idea to it and then I eventually realized how interesting it actually was. There are quite a few twists and turns in it, and I love that.

It was *way* harder than I thought to keep the facts straight, especially with the course changes. Which character knows what, when, why? What are the motives? Is everything consistent and plausible? Constantly question everything -- does she *really* need to do that now or is there some other, much more likely way out? Does every sentence she said 100 pages ago still make sense after the big reveal at the end? Stuff like that. I'll have to get better at that. Probably use a little "character inventory" next time, like, a profile for each one.

Building a world with its own set of rules and history and what not turns out to be very fascinating. As is building the characters.

The length of the stories make proof-reading ridiculously hard. Like, I'm not going to read 120 pages in a couple of minutes. I re-read the second story one last time yesterday and it took ... \*checks notes\* ... three hours. Ouch.

## Where to go from here?

Now that I've re-read that 120-page thing again yesterday ... I actually like it. I'm happy with the result. There's obviously a lot to learn: This entire field is completely new to me. I have identified quite a few places where the storytelling isn't great -- the pace isn't right, things progress too fast. (At the moment, I only notice this when reading the whole thing in one go. Oof.) Sometimes the wording isn't great. Not enough detail. And so on. But the story itself is something that I'd enjoy reading myself.

Digging deeper into groff is on my TODO list, also for obvious reasons.

What I really loved about this whole thing is that I ended up completely in the tunnel. On one of those ten days that it took me to write the 120-page thing, I wasn't even actually typing on a keyboard: We were travelling across the country, lots of idle time, I didn't have a laptop, but I basically "wrote" at least one chapter and then memorized (most of) it. Went back and forth, new ideas. Completely absorbed.

I want more of that. :-)

Maybe stuff will eventually get published (as in: I might put it on this website), but not now. I do this for myself. I'm not even remotely interested in my work getting torn apart by rude critics. Leave me alone. :-)

 And if you think about suggesting the use of LLMs: Congratulations, you have *completely* missed the point. "Setzen, 6."
