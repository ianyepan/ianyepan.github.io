---
title: Gentle Intro to System Default Monospace Fonts (Part 1)
author: Ian Y.E. Pan
date: 2021-05-12 13:22:00 +0800
categories: [Typography]
tags: [fonts]
---

> A week ago (on May 4. 2021), Stack Exchange
> [announced](https://meta.stackexchange.com/questions/364048/we-are-switching-to-system-fonts-on-may-10-2021)
> that they are switching to system fonts as their default font
> stack. This change impacts the way body texts look, as well as the
> font family used in code blocks. In the following blog post I'll
> discuss system default monospace fonts, leaving system sans serif
> fonts for some other time.

## Why monospace fonts

If you program at all, you probably have noticed that no matter which
code editor you use, the default editor font will always be a
monospace font. This kind of fonts enhance readability for coding
because it visually aligns each character with equal spacing. However,
monospace fonts are not suitable for general non-code text reading, as
we are actually trained to read and recognize words faster by taking
advantage of the differences in character width.

## System defaults

Here are the system default monospace fonts on Windows 10, macOS,
Google Android, and most Linux distributions.


| Operating system | Default monospace font |
| -----------      | -----------            |
| Windows 10       | Consolas               |
| macOS            | SF Mono                |
| Linux            | DejaVu Sans Mono       |
| Android          | Roboto Mono            |

System default fonts can change over time, and to some degree reflect
the aesthetics of a generation's typography. For instance, Microsoft's
Consolas is designed in the 2000s as a replacement for the
(way-too-old-school-looking) Courier New. Apple on the other hand has
changed their system default monospace fonts first from Monaco to
Menlo in 2009 (Snow Leopard), and then from Menlo to SF Mono in 2018
(El Capitan).

Perhaps the Linux community has been the most consistent in this
regard, using DejaVu Sans Mono at least as a safe default fallback on
most distributions that I know of. However, individual distros may
ship their own fonts to better integrate with the rest of the
system. For instance, the default monospace font on Ubuntu is
Canonical's own Ubuntu Mono.

## The importance of system default monospace fonts

The default monospace font not only lives in code editors and
terminals, but also quite prevalently in modern browsers. Many
websites, such as StackOverflow and GitHub, use CSS rules to target
the font family to show in code blocks. A simple example would be:


```css
font-family: Menlo, Consolas, "Ubuntu Mono",
             "Roboto Mono", "DejaVu Sans Mono",
             monospace;
```


A [more specific example](https://meta.stackexchange.com/questions/364048/we-are-switching-to-system-fonts-on-may-10-2021) would be how the entire Stack Exchange family
(including StackOverflow) defines its monospace options:
```scss
@ff-mono:
    ui-monospace,
    "Cascadia Mono", "Segoe UI Mono",
    "Ubuntu Mono",
    "Roboto Mono",
    Menlo, Monaco, Consolas,
    monospace;
```

Here, `ui-monospace` targets SF Mono on macOS and iOS devices, and the
last entry `monospace` is the final fallback font on each system.

That's all for part 1, feel free to visit part 2 for a subjective comparison!
