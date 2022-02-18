---
title: Git Gutter in Emacs
author: Ian Y.E. Pan
date: 2022-02-17 14:00:00 -0500
categories: [Emacs]
tags: [linux, emacs, tutorial, tips, workflow, programming]
---

When I first started programming using Visual Studio Code, I've
benefited much from the git gutter indicators that show
added/deleted/modified code blocks that haven't been committed by
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
(define-fringe-bitmap 'git-gutter-fr:deleted [128 192 224 240] nil nil 'bottom))
```

You don't really need to understand how this works, just know that it
is setting the vector values of the bitmap and correctly align them at
the fringe to mimic the appearance of VSCode's git indicators.

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
