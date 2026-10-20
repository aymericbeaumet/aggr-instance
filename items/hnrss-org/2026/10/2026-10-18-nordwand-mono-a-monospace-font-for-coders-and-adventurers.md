---
title: Nordwand Mono – A monospace font for coders and adventurers
link: https://github.com/tywr/Nordwand-Mono
source: hnrss-org
published: 2026-10-18T14:48:01Z
updated: 2026-10-18T14:48:01Z
first_seen: 2026-10-20T12:56:01.525437407Z
authors:
- noriax86
content: extracted
html: 2026-10-18-nordwand-mono-a-monospace-font-for-coders-and-adventurers.html
remote_preview:
  url: https://opengraph.githubassets.com/d2a529b545c99b53bdec04f89856bdfd6ea06c9ab0b448cdb57f8ce0014468ed/tywr/Nordwand-Mono
  alt: A compact monospace neo-grotesque font designed for developers (and adventurers). - tywr/Nordwand-Mono
---

[![Nordwand Mono](https://github.com/tywr/Nordwand-Mono/raw/main/assets/banner.png)](https://github.com/tywr/Nordwand-Mono/blob/main/assets/banner.png)

A compact neo-grotesk font designed for coding.

[![Specimen](https://github.com/tywr/Nordwand-Mono/raw/main/specimens/specimen.pdf)](https://github.com/tywr/Nordwand-Mono/blob/main/specimens/specimen.pdf)

## Philosophy

I'm very passionate about the font I am using for my terminal and overall neovim experience, and I couldn't find the "perfect" font. So I decided to design one from scratch using python. This font aims to bring some elements from the all time classic "Univers" font into the proportions of the "JetBrains Mono" font. The name "Nordwand" is a reference to switzerland and its iconic typography – bringing it into the stiff and cold atmosphere of the swiss north faces.

Amongst the inspirations for the design, you might identify:

- Univers from Adrian Frutiger
- SF Pro & SF Mono from Apple
- Alpes Mono from Sharp Type
- Ubuntu Mono from Dalton Maag

## Alternative characters

Some alternative character designs are available to tweak the vibe of the font to your liking. See the [alternative characters document](https://github.com/tywr/Nordwand-Mono/blob/main/doc/alternative_characters.md) for a visual comparison and the corresponding OpenType feature tags.

## Ligatures

Only two minimal ligatures are exposed with "->" and "=>". I might add a full set of ligatures later on, but I personally won't be using them.

## Compile your own custom version

This font is generated with python. Each glyph is drawn from a python glyph class, in which you can inject a drawing context object. You can for example modify some metrics like the x-height, the capital height etc.

To do so, override the config.example.yml with the values of your liking. Be sure to visualize the individual glyphs with `python -m visualize --config config.yml <glyph_id>`. Then you can geneate the full font with `python -m generate_font --config config.yml`. The font files will be generated inside `./custom-fonts`.

Warning

There are no guarantees that the font might look good with different metrics, and some glyphs might require some tweaking if the metrics are altered !

## Characters

[![characters](https://github.com/tywr/Nordwand-Mono/raw/main/assets/samples/sample_char.png)](https://github.com/tywr/Nordwand-Mono/blob/main/assets/samples/sample_char.png)

## Weights

[![weights](https://github.com/tywr/Nordwand-Mono/raw/main/assets/samples/sample_weights.png)](https://github.com/tywr/Nordwand-Mono/blob/main/assets/samples/sample_weights.png)

## Samples

[![python](https://github.com/tywr/Nordwand-Mono/raw/main/assets/samples/les_drus.png)](https://github.com/tywr/Nordwand-Mono/blob/main/assets/samples/les_drus.png)

### Python

[![python](https://github.com/tywr/Nordwand-Mono/raw/main/assets/samples/sample_1.png)](https://github.com/tywr/Nordwand-Mono/blob/main/assets/samples/sample_1.png)

### C++

[![c++](https://github.com/tywr/Nordwand-Mono/raw/main/assets/samples/sample_2.png)](https://github.com/tywr/Nordwand-Mono/blob/main/assets/samples/sample_2.png)

### Haskell

[![haskell](https://github.com/tywr/Nordwand-Mono/raw/main/assets/samples/sample_3.png)](https://github.com/tywr/Nordwand-Mono/blob/main/assets/samples/sample_3.png)

## Alternative Alphabets

### Cyrillic

[![Cyrillic](https://github.com/tywr/Nordwand-Mono/raw/main/assets/samples/sample_cyrillic.jpg)](https://github.com/tywr/Nordwand-Mono/blob/main/assets/samples/sample_cyrillic.jpg)
