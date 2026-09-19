---
title: Typst makes big strides
link: https://lwn.net/Articles/1092993/
source: lobste-rs-top-1w
published: 2026-09-18T13:14:17Z
updated: 2026-09-18T13:14:17Z
first_seen: 2026-09-19T21:30:23.395188495Z
authors:
- lwn.net via isuffix
labels:
- a11y
- programming
summary: Comments
content: extracted
html: 2026-09-18-typst-makes-big-strides.html
preview:
  file: 2026-09-18-typst-makes-big-strides.preview-9f99f50a507f.webp
  width: 256
  height: 152
  alt: '[Variable fonts]'
  color: '#dedede'
images:
- source: https://static.lwn.net/images/2026/typst-varfont.png
  original:
    file: 2026-09-18-typst-makes-big-strides.image-c871fa8b89e9.png
    width: 400
    height: 237
  color: '#fcfcfc'
- source: https://static.lwn.net/images/2026/typst-biblios.png
  original:
    file: 2026-09-18-typst-makes-big-strides.image-0b395e422316.png
    width: 400
    height: 283
  color: '#fcfcfc'
---

> **Please consider subscribing to LWN**
>
> Subscriptions are the lifeblood of LWN.net. If you appreciate this content and would like to see more of it, your subscription will help to ensure that LWN continues to thrive. Please visit [this page](https://lwn.net/Promo/nst-nag1/subscribe) to join up and keep LWN on the net.

[Typst](https://typst.app) is a system for typesetting documents into various formats: PDF, SVG, PNG, and, in progress, HTML. It is adept at handling technical material, and is often considered to be an eventual [LaTeX](https://www.latex-project.org/) replacement. We last [looked in on Typst](https://lwn.net/Articles/1037577/)\
 a year ago, when it had reached version 0.13. A new version, 0.15, was [released](https://typst.app/blog/2026/typst-0.15/) in June with lots of [new features](https://typst.app/docs/changelog/0.15.0/), including support for variable fonts, MathML, multiple bibliographies, and more. Typst is free, Apache-2.0-licensed software, programmed in Rust.

#### Variable Fonts

Typst now has support for [variable fonts](https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Fonts/Variable_fonts). Most fonts are distributed in a set of files containing their glyphs in different weights, in variations such as italic, bold, and so on. A recent development in the world of typography is the advent of variable fonts, which can contain all their variations in a single file. This both saves space and can permit greater flexibility on the part of the author or designer.

Font features and variations are chosen by setting the value for an "axis"; each axis changes some aspect of the rendered glyphs. There are typically multiple axes that can have discrete values, for turning on and off various features, or continuous values lying between two limits. The latter can be used, for example, for choosing the weight of the font along a continuum.

To test the new Typst feature, I downloaded two open-source variable fonts: [Roboto Flex](https://v-fonts.com/fonts/roboto-flex), a general-purpose font with 13 axes, and [Zycon](https://v-fonts.com/fonts/zycon), a font containing no letters but 17 small decorative pictures. Zycon's six continuous axes smoothly alter various aspects of the pictures, making the font useful in animations.

Here is a Typst document that uses both of these fonts, varying one of the axes for each of them:

```
   #set text(font:"Roboto Flex")
   #for n in (-305, -200, -98) {
       set text(variations:("YTDE": n)) 
       [A penguin jumped quietly.
   
       ]
   }
   
   #set text(font:"Zycon")
   #for n in array.range(0, 10, inclusive:true) {
     set text(variations:("M1  ": n/10))
     str.from-unicode(127773)
   } 
```

In Typst, a "#" character puts the document in "code mode", where the rest of the line or block is interpreted as code in Typst's built-in language. Within code mode, material enclosed in square brackets is interpreted as "text mode", or text to be typeset.

The code above contains two commands to set the fonts by name, using one of the options of the text() function. Next we have for loops, which operate as might be expected. Inside the loops we call the text() function to set the variations variable. The text to be used with the Roboto Flex font is entered directly, but the output using the Zycon font is a [single character](https://www.fileformat.info/info/unicode/char/1f31d/index.htm) specified with the str.from-unicode() function. It could have been entered directly, but readers may not have been able to see it, depending on the coverage in their browser's font.

The axis that we manipulate in Roboto Flex is called YTDE. As the figure below shows, this axis determines the length of the font's [descenders](https://www.designyourway.net/blog/what-are-ascenders-and-descenders/), while leaving its other characteristics unchanged. This might be useful when typesetting tables, for example, to avoid collisions between the descenders and the table-cell boundaries. One of Zycon's six axes, called "M1 " (the two trailing spaces are part of the name; all axis names contain four characters), does different things to different characters. The effect on the Moon glyph is to change the lunar phase.

Compiling this document with typst compile vfont.typ produces a PDF in vfont.pdf, which is shown in the screen shot below:

> ![[Variable fonts]](https://static.lwn.net/images/2026/typst-varfont.png)

#### MathML

HTML export is still an experimental feature of Typst, but this release shows significant progress. The main new HTML feature is the translation of mathematics into [MathML](http://www.w3.org/Math/). The previous article on Typst showed the markup for a certain definite integral and displayed its output when rendered into a PDF by Typst. The same Typst code, when rendered into HTML, produces a long string of MathML markup that almost all reasonably current web browsers know how to interpret. The result appears like this:

If you don't see an equation above, your browser does not support MathML. If you do see it, a comparison with the typeset equation from the previous article shows that the PDF and HTML results are essentially identical. The ability to use Typst to produce TeX-quality mathematics in web browsers, without requiring a JavaScript library such as [MathJax](https://www.mathjax.org/) or having to resort to images, is a boon for scientific communication.

To enable the experimental HTML output, Typst requires a special flag:

```
   typst c --features html integral.typ integral.html  
```

That is the command used to typeset the equation markup in a file called integral.typ into HTML.

#### "Bundle" output

The typical use case for software such as Typst is the creation of single files, usually papers or books in the form of PDFs. The new ["bundle" feature](https://typst.app/docs/reference/bundle/) allows the author to specify a collection of output files, in any of the formats that Typst supports, in a single source file. These files can share data and contain both intra-document and inter-document links.

The feature is ideal for the creation of web sites, which consist of an interlinked network of HTML files. It should also be of interest to academics who might want to generate a slide deck for a conference talk along with the associated preprint from a single source file.

Here is a simple example of a Typst file that creates three interlinked documents, two HTML pages and a PDF, sharing a fragment of text:

```
   #let text = ['Twas brillig, and the slithy toves
         Did gyre and gimble in the wabe:]
   
   #document("poems.html", title: [Famous Poems])[
     #link(<jabberwocky>)[Here] is a famous nonsense poem.
   ]<home>
   
   #document("jabberwocky.html", title: [Jabberwocky by Lewis Carroll])[
   This famous poem begins like this:
   
   #text
   
   The #link(<jabberwockyPDF>)[rest] of the poem.
   
   Go #link(<home>)[home].
   ]<jabberwocky>
   
   #document("jabberwocky.pdf", title: [Jabberwocky: the Complete Poem])[
   #text ....
   
   Go #link(<home>)[home].
   ]<jabberwockyPDF>   
```

The command for processing this code, if it is saved in a file called bundle.typ, is:

```
   typst c --features html,bundle --format bundle bundle.typ   
```

In my tests, the bundle feature worked as advertised, but since it, along with HTML output, is still considered a work in progress, the compiler requires the --features flag.

The command above creates a new directory called "bundle" containing the three files defined in the #document() functions. They all contain the two initial lines of the poem saved in the text variable. There are hyperlinks between the HTML pages, from one of those to the PDF, and from the latter back to the two-page website. The links are targeted using the labels, contained within angle brackets, following each document function.

#### Multiple bibliographies

Typst now permits [multiple bibliographies](https://typst.app/docs/reference/model/bibliography/) in a single document, which was an eagerly awaited feature. Its canonical application is for books that may need a separate reference section for each chapter. The feature is best introduced with a toy example:

```
   #show bibliography: set text(size: 8pt)
   
   = Chapter I
   
   According to @smith, Smith is uncommonly smart.
   
   #bibliography("works.bib",
   title: "References for Chapter I",
   group: none)
   
   = Chapter II
   
   Jones@jones has a different view. The issue was
   finally put to rest in the following year
   in @mergutroid.
   
   #bibliography("works.bib",
   title: "References for Chapter II",
   group: none)   
```

Here the first line specifies that the bibliographies should use a font size smaller than the default used in the main text. In that text, the "@" prefixes create a citation using the default number-in-brackets style. At the end of each chapter, the bibliography() function is called. Its first argument specifies which database should be used for the bibliographic information; each bibliography section can use a different database, or collection of databases, if desired (see our [recent article on Pandoc](https://lwn.net/Articles/1064692/) for a description of these text-file databases). The group argument controls how the citations are numbered. The value of none causes the numbering to begin with one for each section; numbering can alternatively be continuous for the entire work, or be grouped arbitrarily.

The figure below shows the output of the listing as it appears in PDF form:

> ![[Bibliographies]](https://static.lwn.net/images/2026/typst-biblios.png)

#### Multiple PDF standards

Avoiding the use of proprietary extensions is normally sufficient to ensure that the PDFs created with Typst, LaTeX, or any other competent software will fulfill the promise of the format: documents will be openable and appear identical in all readers, now and in the future. At a deeper level, however, a PDF is not just a PDF. There are various PDF versions and, on top of these, dozens of formal standards relating to archivability and accessibility. The standards for archivability are meant to ensure that the document really does work across a wide variety of reader software and that it will do so forever. The accessibility standards relate to the usability of a PDF for people with disabilities of various sorts.

Typst already had the ability to target various PDF versions and standards for archiving or accessibility. The new feature is the option to target more than one when compiling a document. This is useful, because one may want to generate a PDF that has both archival and accessibility attributes. The implementation of the feature helps the author to navigate the [forest of PDF standards](https://typst.app/docs/reference/pdf/#pdf-standards) by issuing warnings or errors in the cases of incompatible combinations or failure to follow best practices for the standards targeted.

As an example, here is a command that attempts to compile the "book" document from the previous section, requesting both PDF version 1.7 and the A-1b archive standard:

```
   typst c --pdf-standard 1.7,a-1b book.typ  
```

The Typst compiler responds with this error message:

```
   error: PDF 1.7 is not compatible with PDF/A-1b
   hint: PDF/A-1b requires version PDF 1.4   
```

PDF version 1.7 is compatible with A-2b, so this will work:

```
   typst c --pdf-standard 1.7,a-2b book.typ   
```

If, however, we add the UA-1 accessibility standard, as in this command:

```
   typst c --pdf-standard 1.7,a-2b,ua-1 book.typ    
```

We again get an error:

```
   error: PDF/UA-1 error: missing document title
    = hint: set the title with `set document(title: [...])`   
```

Normally the Typst compiler doesn't insist on anything beyond correct syntax, but if we specify a particular accessibility standard, the document must conform to that standard. UA-1 requires, among other things, a document title.

The foregoing is not merely arcana, although it may seem to be of little relevance to the typical author of a scientific paper or textbook. These details are important to archivists and publishers; in addition to helping disabled users, producing accessible PDFs is a legal requirement applying to state and federal governments in the US and many other countries. Typst's advanced handling of multiple PDF standards makes it a useful tool in these contexts.

#### Conclusion

Typst 0.15 has other enhancements that are not described in detail in this article. Some of these are support for [spot colors](https://typst.app/docs/reference/visualize/color/#definitions-spot), [detailed diagnostics](https://github.com/typst/typst/pull/7364) that explain any failure of [convergence](https://typst.app/blog/2026/typst-0.15#convergence-diagnostics) during the compilation process, and new [map and filter functions](https://typst.app/docs/reference/foundations/dictionary/#definitions-filter) in the built-in scripting language. The documentation, which is updated to cover the new version, is now [available in a 26MB PDF](https://github.com/typst/typst/releases/download/v0.15.0/typst-documentation.pdf).

Typst is [developed on GitHub](https://github.com/typst/typst), where it has 460 contributors. The creators of the project have written a [guide for new contributors](https://github.com/typst/typst/blob/main/CONTRIBUTING.md#contributing-to-typst) where they describe what a PR should look like and warn that any code or description generated by an LLM will be rejected. They are also forthright about the fact that Typst is a company as well as an open-source project, and that decisions about the direction of the project, as well as the suitability of individual contributions, will take the needs of the company into account. This is a factor that prospective contributors and users should keep in mind.

Progress in the development of the system is impressive. The community of users is enthusiastic; their participation has expanded the Typst ecosystem to [over 1500 packages](https://typst.app/universe).

However, network effects in the publishing world are preventing Typst from fulfilling the potential that we saw for it in our previous article. Although users have devised templates to match the style specifications of several journals, those that require source submissions (rather than just PDFs) still insist on LaTeX, Word, or some other format. Very few accept manuscripts marked up in Typst. This is not likely to change while Typst remains in a pre-1.0 status (which is probably [still a ways off](https://laurmaedje.github.io/posts/evolving-typst/)) and is in danger of requiring significant, possibly breaking changes to documents.

Despite this, Typst is an immensely useful tool today. For example, I recently had to create an SVG logo and found that writing a textual description using Typst's built-in graphics commands was quicker and easier than reaching for a drawing program. It's impossible to say whether Typst's advantages will lead to it becoming a "LaTeX replacement", as many of its admirers describe it. But, if development continues at the current pace, it is a distinct possibility, although one that may take a decade or two to come to fruition.

\

| Index entries for this article                         |                                                                     |
| ------------------------------------------------------ | ------------------------------------------------------------------- |
| [GuestArticles](https://lwn.net/Archives/GuestIndex/) | [Phillips, Lee](https://lwn.net/Archives/GuestIndex/#Phillips_Lee) |

\

* * *
