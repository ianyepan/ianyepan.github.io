---
title: Gentle Intro To System Default Monospace Fonts (Part 2)
author: Ian Y.E. Pan
date: 2021-05-12 14:31:00 +0800
categories: [Typography]
tags: [fonts]
---

In my previous post, I talked about the practical reasons to use
monospace fonts and introduced four system default fonts: Consolas, SF
Mono, DejaVu Sans Mono, and Roboto Mono.

Here's to refresh the memory:

| Operating system | Default monospace font |
| -----------      | -----------            |
| Windows 10       | Consolas               |
| macOS            | SF Mono                |
| Linux            | DejaVu Sans Mono       |
| Android          | Roboto Mono            |


In addition to these four system fonts, I also mentioned Menlo and
Monaco as deprecated default macOS fonts. In this blog post, I will
give a subjective comparison of these 6 typefaces, as well as
their respective historical backgrounds. Buckle up and sit tight!

## Consolas

![Consolas Preview](/images/Consolas.png){: width="200"}
_Consolas_

First on the list is Microsoft's Consolas, which happens to be my
all-time favorite monospace font. For practical use, I love the
thicker characters that enhances readability at smaller sizes, and the
generally narrower body to allow more columns to be shown in my code
editor. The details in the low-hanging "r", slightly triangular angles
of "M", the slanting vertical line of the dollar symbol, and the
elongated comma are in my opinion, what makes this font elegantly
timeless. In general, I also prefer the look of a [looptail
double-storey](https://en.wikipedia.org/wiki/G#Typographic_variants)
"g", instead of an opentail single-storey one, which is found on SF
Mono, Menlo, DejaVu, Roboto Mono etc.

However, nothing is perfect, and there are two things that bug me
about Consolas. First, the italic version of lowercase "k".  Cool as
it may be, the cursive loop on top makes the character somewhat stand out
unnecessarily every time I glance through a code base.

![italic k Preview](/images/Consolas-k.png){: width="150"}
_Consolas' italic k_

The other
problem I have might be due to the
[ClearType](https://en.wikipedia.org/wiki/ClearType) technology behind
the font. Consolas changes the body weight from its regular thickness
to a lighter version (despite it not "actually" having a light
font-weight option) when the font size is smaller than a certain
threshold. On some screen resolutions, that threshold is somewhere
around 9 pt.

## SF Mono

![SF Mono Preview](/images/SF-Mono.png){: width="200"}
_SF Mono_

Introduced in 2016, SF Mono is part of Apple's San Francisco font
family. Its license prohibits usage beyond default Apple applications
like Terminal and Xcode. This means that third party code editors,
such as Sublime Text or Microsoft's Visual Studio Code, will have to
fall back to the previous default macOS font, Menlo. If you really
like SF Mono, there are still ways that you could use it anywhere you
want (the morality of proprietary font usage is controversial, but
there are a few tutorials online that teach you how do it. Spoiler
alert, it's not hard at all).

In terms of the design, I personally find SF Mono more
[Geometric](https://en.wikipedia.org/wiki/Sans-serif#Geometric) than
[Neo-grosteque](https://en.wikipedia.org/wiki/Sans-serif#Neo-grotesque),
especially with the perfectly round curves of 5, 6, and 9. The long
serifs of 'i', 'j', 'l', and 'r' contribute greatly to the overall
neat, well-ordered, and rigid feel of the typeface. However, the
trade-off of tidiness would be the less relaxing vibe that SF Mono
gives off. Which is why every time I come back to it, I
psychologically feel more uptight and as a result get tired more
easily. Regarding readability, Apple has nailed it here &mdash; I can
recognize characters with easy even at much lower font sizes.

## Menlo

> A decade ago, Menlo replaced Monaco as the default monospace font on
> macOS. After a couple of years, Menlo was replaced by SF Mono.

![Menlo Preview](/images/Menlo.png){: width="200"}
_Menlo_

To understand the design of Apple's Menlo, we must first talk about
Bitstream Vera Sans Mono, perhaps the most influential monospace font
in recent typography. The later popular DejuVa Sans Mono, for example,
is based on Vera, with modifications to certain letters, and a greater
coverage of Unicode characters.

Apple's Menlo, the default system font from Snow Leopard to El
Capitan, is in turn based on DejaVu with little modification. In fact,
in the following picture you can see that Apple's Menlo barely changed
from DejaVu Sans Mono, except for the lower position of the asterisk,
the enlargement of quotes and periods, and changing a dotted zero to a
slashed zero. The differences are highlighted in red for DejaVu and
cyan for Menlo.

![Menlo vs. DejaVu](http://luc.devroye.org/DejaVuSansMono-vs-AppleMenloSansMono--both-based-on-BitstreamVeraSansMono-2011.png){: width="300"}
_Source: http://luc.devroye.org/fonts.html_


## DejaVu Sans Mono

![DejaVu Sans Mono Preview](/images/DejaVu-Sans-Mono.png){: width="200"}
_DejaVu Sans Mono_

Having discussed Menlo, there's little left to say about DejaVu. I
have never really liked this font because I find the dashes (minus
symbol) too short, and the dots and commas too small. In addition, the
font renders rather poorly on the Windows 10 operating system with
suboptimal screen resolution. However, there is no denying that DejaVu
is a very practical font with a massive collection of Unicode
characters, and its derivatives, Menlo and Hack just to name two, are
tremendously popular among developers.
