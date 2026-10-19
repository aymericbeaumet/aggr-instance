---
title: Making a 88x31 button book using ImageMagick
link: https://k.sosnierz.me/08-11-24/
source: lobste-rs
published: 2026-10-16T11:21:12Z
updated: 2026-10-16T11:21:12Z
first_seen: 2026-10-19T19:14:52.283302801Z
authors:
- k.sosnierz.me via nathell
labels:
- art
summary: Comments
content: extracted
html: 2026-10-16-making-a-88x31-button-book-using-imagemagick.html
remote_preview:
  url: https://k.sosnierz.me/08-11-24/anybrowser.gif
  alt: Best viewed with any HTML browser
---

## (On a posix system)

## Introduction

If you've spent a considerable amount of time surfing the internet (and especially the Small Web), you might have seen an 88x31 button, or a few dozen of them. Some of them look like this:

![Best viewed with any HTML browser](https://k.sosnierz.me/08-11-24/anybrowser.gif)

However, the nice thing about computer displays is that they are able to cope with quickly changing visuals, hence some have animation:

![Edited with Vim — Improved and free — I love my editor](https://k.sosnierz.me/08-11-24/vim.gif)

I have noticed that among [very numerous examples](http://textfiles.com/underconstruction/88x31/), i haven't seen any that'd go very far in that axis. Usually the animations are there just to catch your eye, or to display several words in succession. But why not Moby Dick?

## Size limitations

I was surprised to find that there seems to be no limit to the number of frames in a GIF file, and that the maximum resolution is 2^16 by 2^16, which is 4295 megapixels. We are of course limiting ourselves to just 88 by 31 pixels (or 0.003 megapixel), which might mean that regular text fonts don't get much space to shine — Arial, for example, seems to become legible at a minimum of 10 pixels height, which allows us to display about eleven words at once.

![One two three four five six seven eight nine ten eleven.](https://k.sosnierz.me/08-11-24/arial.gif)

## Alternative fonts

As is often the case in computing, popular conventions end up upsetting someone enough that they develop an alternative way. In this case the alternative we're looking for are fonts designed to be legible at extremely low font sizes. There are some as small as 3x3, but personally I find [TomThumb](https://robey.lag.net/2010/01/23/tiny-monospace-font.html) to be the golden middle here. [You can get TomThumb in truetype here](https://k.sosnierz.me/08-11-24/tom-thumb.zip).

![All glyphs of the TomThumb font](https://k.sosnierz.me/08-11-24/tom-thumb-new.png)

## Getting Moby Dick

Firstly, you will want to obtain a plain text copy of the book's content. The methods might vary depending on what formats you have access to — [you can use OCR to grab the text from a physical book](https://oldcomputer.info/hacks/diyscan/index.htm), with an .EPUB or .PDF you can convert them (or even copy text manually), or you might get lucky and find what you're looking already shipped as a plain text .txt file, as is the case with [Moby Dick](https://www.gutenberg.org/ebooks/2701).

I downloaded the file and stripped the license at the start and the end. I also cropped off everything before the first chapter.

## Formatting the work

If we take our button, which is 88x31, and our font, which is 6x4 (5x3 + spacing), we find that we can fit 5 rows of 22 characters each. ImageMagick, the tool we're going to use, is not very good at laying out text, so it'd be wise to take care of that ourselves. There's several ways to do this, but the neatest one uses three posix tools: cat, tr and fold:

`cat input.txt | tr '\n' ' ' | tr -d '\r' | fold -s -w 22 > 22w.txt`

cat is used to send the text to tr. tr translates each newline into a space, condensing the whole text file into one line. fold then takes that one line, splits it into multiple lines that are up to 22 characters wide (-w 22), and tries to avoid splitting words in half (-s).

It's likely that at this point, your text looks fine. Mine was a bit garbled up, because fold doesn't handle multibyte characters well. I checked what non-ascii characters input.txt contained, and got rid of them using sed, like this: `sed -i 's/”/"/g'`. The -i flag allows you to modify the file in place. You can find all the non-ascii characters using grep: `grep -P -n '[^\x00-\x7F]' input.txt`.

Of course, formatting is subjective. Depending on the kind of literary work, you might want to do it differently. I used `sed -i 's/CHAPTER /\n\n\nCHAPTER /g' input.txt` to add some space between chapters, but chapters may be marked differently in the text you're using, or they might not be present at all.

## And turning it into a GIF?

For this part, you're going to need [ImageMagick](https://imagemagick.org/) installed. Although it's likely that you already have it. We use the following command to generate the first version of our GIF file:

`cat 22w.txt | convert -page 88x39+0+0 -background white -fill black -font tom.ttf -pointsize 6 text:- high.gif`

Note that I have the actual tom.ttf file placed in the current working directory. It's mostly self explanatory. Convert takes the text from standard input and, just like that, puts it onto a GIF, frame by frame. Lovely. However, it seems to force some margin on the bottom of the image, which is why we drew the text on an image eight pixiels higher than we wanted. We're going to crop it now:

`convert high.gif -set delay 600 -coalesce -repage 0x0 -crop 88x31+0+0 +repage good.gif`

We're cropping those last eight pixels off, and also setting a reasonable delay between frames (in my experience, 6 seconds seems fine — please let me know if you disagree.

Now good.gif should look somewhat like this:

![First chapter of mobydying being shown frame by frame, at about a dozen words at once](https://k.sosnierz.me/08-11-24/good.gif)

## Python bonus: centered text

I think it would be neat to have the text aligned to center, rather than having it stick so hard to the left edge. I wrote a quick python script for that, made it executable, then piped it's output into a file and once again regenerated the GIF.

```
#!/usr/bin/python3
from math import floor
with open("22w.txt") as file:
    for line in file:
        i = (22-len(line.strip()))/2
        o = " "*floor(i)+line.strip()
        print(o)
```

![First chapter of mobydick being shown frame by frame at about a dozen words at once, but it looks neater](https://k.sosnierz.me/08-11-24/centered.gif)

## Moby Dick bonus: the full book

To save on bandwidth, the above-attached GIFs are examples consisting of only the first chapter. [Click here to wintess the full, 4.2 MB, over 19-hour long beauty](https://k.sosnierz.me/08-11-24/full.gif).
