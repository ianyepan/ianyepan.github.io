---
title: Git Gutter in Emacs
author: Ian Y.E. Pan
date: 2022-02-17 14:00:00 -0500
categories: [Emacs]
tags: [linux, emacs, tutorial, tips, workflow, programming]
---

When I first started programming, I used Visual Studio Code. I've
benefited much from the git gutter indicators that show
added/deleted/modified code blocks that haven't been committed via
git. This allows for a quick overview of the lines I've changed and
helps me keep track of my actions since the last commit.


![VSCode Git Gutter](/images/vscode-gutter.png){: width="50%"}
_VSCode showing git gutter indicators_

Over at the (neo)vim community, a similar effort has been made with the project
"[gitsigns](https://github.com/lewis6991/gitsigns.nvim)". I
particularly like the way they mimic the visuals of the indicators of
VSCode and Sublime Text: a thin colored line on the left fringe of the editor.

This got me to wonder whether we can get the same functionalities and
visuals in Emacs. There are a couple of packages created for this
purpose, including git-gutter, diff-hl, and git-gutter-fringe, but
none of them have the same modern visuals as found in VSCode.

Fortunately, Doom Emacs has modified git-gutter and
git-gutter-fringe's bitmap values to mimic the appearance of the
indicators that I like, and the author of Doom was [kind enough to
point me](https://github.com/hlissner/doom-emacs/issues/2246) to how
he's done it. In the following paragraphs, I'll show you how you can
achieve the same visuals and functionalities in your own
configuration, without having to use Doom Emacs.

*P.s. Doom is truly an awesome project. But some people, like myself,
simply find joy in tinkering and maintaining their own configuration
from scratch.*

## First, let's see the result

![VSCode Git Gutter](/images/emacs-gutter.png){: width="80%"}
_Emacs showing git gutter indicators similar to modern editors._

## Step 1: Installing necessary packages

The two packages we'll need for this to work are
[git-gutter](https://github.com/emacsorphanage/git-gutter) and
[git-gutter-fringe](https://github.com/emacsorphanage/git-gutter-fringe). We
can install them with use-package by putting this into our config:

```emacs-lisp
(use-package git-gutter)

(use-package git-gutter-fringe)
```

P.s. If you don't have `use-package-always-ensure` set to `t`, then
you'll need an extra `:ensure t` in the code block for use-package to
automatically download the packages upon next startup, like so:

```emacs-lisp
(use-package git-gutter
  :ensure t)

(use-package git-gutter-fringe
  :ensure t)
```
## Step 2: Specifying when to enable git-gutter and fine-tuning update interval

Ideally, we'd like git-gutter to show up when the current buffer is
version-controlled. However, in Emacs there doesn't seem to be a
built-in variable indicating whether a buffer is in a git repository
(**Edit:** *There is, you may use the function "vc-backend" to query
it. Thanks, u/emax-gomax on Reddit!*). Not to worry though, we can
simply enable git-gutter for every buffer that's derived from
`prog-mode`, which is a "parent mode" for many programming
languages. Here, I choose to use the use-package macro `:hook`, but
you can explicitly set it with `add-hook` as well. If you need a quick
tutorial on use-package, consider [this post I
wrote](../setting-up-use-package).

On top of that, I set the git-gutter's update interval/delay to be
0.02 seconds. I find it a good balance between showing up instantly,
but also not setting the delay too short such that it drags down
performance. Depending on your preference, you can tune this variable
to whatever you like. By default, this variable is set to "0", which,
contrary to intuition, doesn't mean "instantly", but rather "update
the indicators only upon saving the file".

```emacs-lisp
(use-package git-gutter
  :hook (prog-mode . git-gutter-mode)
  :config
  (setq git-gutter:update-interval 0.02))
```

## Final step: Defining bitmap for appearance

To make the indicators look like the ones in Visual Studio Code, Doom
Emacs has defined bitmap values for git-gutter-fringe. I have taken
this snippet straight from their source code, so full credits to the
contributors of Doom! In the configuration of git-gutter-fringe, add
these 3 lines:

```emacs-lisp
(define-fringe-bitmap 'git-gutter-fr:added [224] nil nil '(center repeated))
(define-fringe-bitmap 'git-gutter-fr:modified [224] nil nil '(center repeated))
(define-fringe-bitmap 'git-gutter-fr:deleted [128 192 224 240] nil nil 'bottom)
```

Each integer in the vector is a row of the bitmap, where the 8 bits
(MSB-first, left-to-right) represent which pixels are lit across the
fringe width.

```text
224 = 0b11100000  ->  ███░░░░░
```

With `'(center repeated)` it stacks this single row vertically to fill
the line height, giving us a solid left-aligned bar.

Similarly, for `git-gutter-fr:deleted` we have:

```text
128 = 0b10000000  ->  █░░░░░░░
192 = 0b11000000  ->  ██░░░░░░
224 = 0b11100000  ->  ███░░░░░
240 = 0b11110000  ->  ████░░░░
```

This produces a partial arrow/triangle anchored to the bottom of a
line, which is conventional for deleted indicators since there's no
line to sit on, only a position between lines.

## Wrapping it up

In conclusion, this is the full configuration you'll need to achieve
these pretty git-gutters in Emacs:

```emacs-lisp
(use-package git-gutter
  :hook (prog-mode . git-gutter-mode)
  :config
  (setq git-gutter:update-interval 0.02))

(use-package git-gutter-fringe
  :config
  (define-fringe-bitmap 'git-gutter-fr:added [224] nil nil '(center repeated))
  (define-fringe-bitmap 'git-gutter-fr:modified [224] nil nil '(center repeated))
  (define-fringe-bitmap 'git-gutter-fr:deleted [128 192 224 240] nil nil 'bottom))
```

That's all for today, thanks for stopping by.

## Update (May 2026):

I have since found a much more visual way to define the fringe bitmaps
via `fringe-helper-define`. Improvements were made to the "deleted"
arrow shape too:

```elisp
  (fringe-helper-define 'git-gutter-fr:added '(center repeated)
    "XXX....."
    "XXX....."
    "XXX....."
    "XXX.....")
  (fringe-helper-define 'git-gutter-fr:modified '(center repeated)
    "XXX....."
    "XXX....."
    "XXX....."
    "XXX.....")
  (fringe-helper-define 'git-gutter-fr:deleted 'bottom
    "X......."
    "XX......"
    "XXX....."
    "XXXX...."
    "XXXXX..."
    "XXXXXX.."
    "XXXXXXX."
    "XXXXXXXX"
    "XXXXXXX."
    "XXXXXX.."
    "XXXXX..."
    "XXXX...."
    "XXX....."
    "XX......"
    "X.......")
```

`fringe-helper` converts the ASCII grid to the bit vector internally,
so we get identical output with none of the manual bit
arithmetic. The deleted arrow in particular would have been tedious to
tune pixel-by-pixel in the original form.

