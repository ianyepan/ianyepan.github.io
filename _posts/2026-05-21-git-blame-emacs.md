---
title: Git Blaming in Emacs
author: Ian Y.E. Pan
date: 2026-05-21 14:00:00 -0800
categories: [Emacs]
tags: [linux, emacs, tutorial, tips, workflow, programming]
---

Many times when working on open-source codebases, I encounter a
piece of code that I'd like to learn more context about or simply want
to know who wrote it or when it was written. In most modern IDEs,
there's some sort of "git blame" functionality to show the history of
any line of code, either built-in natively or through extensions like
VSCode's GitLens. In this short post, let's explore how we can achieve
the same in Emacs.

**Magit** is a package that needs no introduction. It's the center of
most Emacs user's git workflow and makes non-trivial git operations
like interactive rebasing or selecting chunks to stage/unstage in a
changed file more intuitive and efficient. For many people, Magit
along with Org Mode were the "kill apps" that converted them to
becoming full-time Emacsers in the first place. I've been a happy
Magit user for years but only recently did I find out about the handy
`magit-blame-addition` command. This command turns your current buffer
into an interactive one, annotated with commit messages and timestamps
before each chunk of code. It does not extend the left fringe like
most git blame UI's do, but rather inserts the extra git info directly
between the lines of code you're viewing. This allows for fewer
annotations because multiple lines of code can belong to the same
annotation group if they come from the same commit, and the text of
the annotation gets to extend the full width of your Emacs
buffer. Hitting "RET" (enter key) on any line invokes
`magit-show-commit` which opens a new split window containing the full
git commit detail associated with that line. This is very useful if I
want to read the full commit message body and inspect any other code
chunks that were committed along with that piece of code I'm
reading. Hitting "Q" in the Magit blame buffer restores the original
code view and removes all blame annotations.

This is great and all, but there are also times when I just wanna quickly
see the commit author or the commit date of a piece of code without
having to enter Magit blame buffer view, open the full commit details
in a new window, and later restore both.

Introducing [blamer.el](https://github.com/Artawower/blamer.el)!
**Blamer** is a neat little package (well, little in the sense that's
it's a single-file package. It's still 1.2K lines of Emacs Lisp code.)
that provides the command `blamer-mode` to toggle the "blamer" minor
mode. When it's activated, any line your cursor lands on will
automatically have a trailing annotation that shows the author, commit
title, and relative time when the code was written. The git blame info
updates and follows your cursor whenever you jump to a new line. It's
designed to be an on-the-fly and non-intrusive version control hint
provider. Blamer also allows you to configure many options, including
date/time format, max text length, and pop-up delay in seconds. The
following is my preferred setting. I set the keyboard shortcut of the
blamer toggle command to `Ctrl-c g`.

```emacs-lisp
(use-package blamer
  :bind (("C-c g" . blamer-mode))
  :config
  (setq blamer-idle-time 0.05)
  (setq blamer-author-formatter "%s ")
  (setq blamer-datetime-formatter "[%s]")
  (setq blamer-commit-formatter ": %s")
  (setq blamer-max-commit-message-length 100)
  (setq blamer-min-offset 70))
```

P.s. If you're unfamiliar with the usage of use-package, check out my
[2021 blog post](../setting-up-use-package) detailing how to use its
syntax to organize your config. As of Emacs 29.1 (released 2023 July),
use-package is a built-in feature and you no longer need to install it
from external repos.

That's it for today! Thanks for reading and hope you picked up
something new.
