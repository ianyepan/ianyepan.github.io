---
title: A Quick Tutorial on Use-package for Emacs
author: Ian Y.E. Pan
date: 2021-05-12 21:18:00 +0800
categories: [Emacs]
tags: [linux, emacs, tutorial, tips]
---

## Brief introduction

[Use-package](https://github.com/jwiegley/use-package) is a popular
package to organize your Emacs configuration and load your installed
packages efficiently. According to the official README, use-package is
a macro that allows you to isolate package configuration in a way
that's both performance-oriented and tidy. It also provides additional
useful macros, such as `:custom-face`, `:config`, `:hook`, `:bind`,
`:init`, among many others. If you're serious about mastering
use-package and abstract your Emacs configuration as much as possible,
I encourage you to read the official GitHub README linked above.

## Why use-package?

To install a desired package without using use-package, one may have
to manually install it from GitHub and put it under a load path where
Emacs can find it, for instance with `(add-to-list 'load-path
"/path/to/installed-package-repo")`. Alternatively, you could open up
the package list in Emacs with `M-x list-packages`, navigate your
cursor to find the package you want, and press `i` followed by `x` on
the row containing the package. Either way, you'd have to explicitly
call `(require 'package-name)` in your `init.el` before you could do
any configuration. Lastly, any configuration of the package may need
to be wrapped in a `(with-eval-after-load 'package-name ...)` block,
to avoid running into undefined variables and functions before your
package is fully loaded
([Reference](https://www.emacswiki.org/emacs/InstallingPackages#h5o-7)).

What a hassle indeed! The good news is that use-package abstracts all
of this away from you.

## Setting up use-package

The following snippet, to be put at the top of your `init.el`, takes
care of connecting to the GNU and MELPA package archives over the
internet, and download use-package for you if it's not
installed.
```emacs-lisp
(require 'package)
(add-to-list 'package-archives '("gnu"   . "https://elpa.gnu.org/packages/"))
(add-to-list 'package-archives '("melpa" . "https://melpa.org/packages/"))
(package-initialize)

(unless (package-installed-p 'use-package)
  (package-refresh-contents)
  (package-install 'use-package))
(eval-and-compile
  (setq use-package-always-ensure t
        use-package-expand-minimally t))
```

Setting `use-package-always-ensure` to `t` (meaning "true") saves
us the trouble of having to specify `:ensure t` in any future packages
we'd like to declare and install. The `:ensure` macro basically makes
sure that the packages are correctly installed at every startup, and
automatically installs the missing ones for you. This is extremely
useful when you frequently move between different machines and need to
port your entire Emacs config over to a new setup.

## Use-package in action: Example 1


Say you want to write Typescript in Emacs and need language-specific
support (i.e. better syntax highlighting etc.). We can install
[typescript-mode](https://github.com/emacs-typescript/typescript.el/tree/1043025d42602d560949955410d3afa2562130ee) for this purpose.

Let's start with a simple configuration:

```emacs-lisp
(use-package typescript-mode)
```
After restarting Emacs,
use-package will automatically install typescript-mode and load it for
you when the suitable time comes.

But what does "a suitable time" mean? Surely it means when we open a
`.ts` or `.tsx` buffer, right?

Let's specify that:

```emacs-lisp
(use-package typescript-mode
  :mode ("\\.tsx?\\'" . typescript-mode))
```

Here we see our first use-package keyword `:mode`.

All it does is that it internally expands to the normal Emacs Lisp
syntax of`(add-to-list 'auto-mode-alist '("\\.tsx?\\'"
. typescript-mode))`, which essentially states that all files ending
with `.ts` or `.tsx` will be recognized by Emacs as a suitable buffer
to be opened in typescript-mode.

Normal package configuration (to be executed after a package is
loaded) should go after the `:config` keyword, which is the
use-package keyword that you'll probably be using the most often. For
example, say I want to set the default Typescript indent level to 2
spaces (like most modern developers do), I can add on:

```emacs-lisp
(use-package typescript-mode
  :mode ("\\.tsx?\\'" . typescript-mode)
  :config
  (setq typescript-indent-level 2))
```
If you really dislike using keywords other than `:config` (and want to
be a purist), you are welcome to refactor it to:
```emacs-lisp
(use-package typescript-mode
  :config
  (add-to-list 'auto-mode-alist '("\\.tsx?\\'" . typescript-mode))
  (setq typescript-indent-level 2))
```

Which of course is more verbose, less abstracted, and more close to
the default configuration syntax. At the end of the day, it's down to
personal preference.

## Use-package in action: Example 2

Let's have a look at another example:

Say you want to write Markdown in Emacs and need additional syntax
highlighting and indentation support for it, we can install the
[markdown-mode](https://github.com/jrblevin/markdown-mode) package for
this purpose. All you have to do is put this in your `init.el`:

```emacs-lisp
(use-package markdown-mode
  :mode ("\\.md\\'" . markdown-mode))
```

Again, this is all you need. After restarting Emacs,
use-package will automatically install markdown-mode and load it for
you when the suitable time comes (that is, when you open up a `*.md` buffer).

Say I want to enable auto-fill-mode whenever I'm editing
Markdown. Auto-fill-mode is just a mode that adjusts the width of the
buffer content and makes it more readable by automatically breaking
lines on the fly while you're typing. To tell Emacs to enable
auto-fill-mode whenever we open a markdown-mode buffer, we can add a
hook as follows:

```emacs-lisp
(use-package markdown-mode
  :hook (markdown-mode . auto-fill-mode))
```

Say I also want to let markdown-mode's code-font style inherit the
style of an [Org Mode](https://orgmode.org/features.html) source
block:


```emacs-lisp
(use-package markdown-mode
  :hook (markdown-mode . auto-fill-mode)
  :custom-face (markdown-code-face ((t (:inherit org-block)))))
```

If you're familiar with Emacs Lisp, it isn't hard to guess that `:hook
(markdown-mode . auto-fill-mode)` internally expands to become
`(add-hook 'markdown-mode-hook
#'auto-fill-mode)`, and `:custom-face` expands to a `set-face-attribute`
call. So if you really don't like using the `:hook` keyword, you can
also refactor this snippet to be:

```emacs-lisp
(use-package markdown-mode
  :config
  (add-hook 'markdown-mode-hook #'auto-fill-mode)
  (set-face-attribute 'markdown-code-face nil :inherit 'org-block))
```

Some packages don't even require us to modify the auto-mode-alist for
file extension regex recognition, as they're handled by the packages
themselves. For instance:

```emacs-lisp
(use-package json-mode)

(use-package vimrc-mode)

(use-package yaml-mode)
```

By now, you are probably starting to realize both the simplicity and
flexibility of use-package. Let's have a look at one final example.

## Use-package in action: Example 3

Say I want turn on the visualization of matching parentheses. That is,
when my cursor stops at a parenthesis/bracket/brace, the matching
instance on the other end is highlighted. This requires tweaking the
"paren" package, which is a built-in package in Emacs already. To
prevent use-package from looking for the paren package in the internet
archives, we turn off the `:ensure` flag. Then, in the `:config`
section, we specify the desired show-paren-mode:

```emacs-lisp
(use-package paren
  :ensure nil
  :config
  (show-paren-mode +1))
```

You notice that there is always a slight lag before the matching
parenthesis is highlighted. This is actually not a performance issue
from Emacs, but rather because the default value of show-paren-delay
is set to 0.125. Weird choice, if I may say so myself. Let's make
it 0. Now this special variable "show-paren-delay" needs to be set
"before" the paren package is loaded, so we can't put in under the
section of `:config` as usual. Luckily, use-package provides the
`:init` keyword for exactly this purpose.

Putting it all together, we have:

```emacs-lisp
(use-package paren
  :ensure nil
  :init
  (setq show-paren-delay 0)
  :config
  (show-paren-mode +1))
```

That's all I have for you this time, hope you enjoyed my quick
tutorial on use-package! If you're an Emacs beginner feeling lost
about where to start, I might just have the perfect solution for
you. I'm maintaining the
[Yay-evil-emacs](https://github.com/ianpan870102/yay-evil-emacs)
project, which is a lightweight literate Emacs config with better
defaults. The actual goal of my project is not to give people a
plug-and-play starter config (though of course you may use it that
way), but rather to [document
neatly](https://github.com/ianpan870102/yay-evil-emacs/blob/master/config.org)
the meaning behind each configuration snippet in a structural manner,
so Emacs beginners wouldn't feel overwhelmed by the syntax of Emacs
Lisp right off the bat.
