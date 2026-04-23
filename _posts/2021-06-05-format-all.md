---
title: Formatting Code in Emacs with Format-All
author: Ian Y.E. Pan
date: 2021-06-05 23:59:00 +0800
categories: [Emacs]
tags: [emacs, programming]
---

[Format-all](https://github.com/lassik/emacs-format-all-the-code) is a
simple yet awesome Emacs package that allows you to "format source
code in different programming languages using the same command,
instead of learning a different Emacs package and formatting command
for each language"
([Reference](https://github.com/lassik/emacs-format-all-the-code#what-does-it-do)). The
author chose great default formatters for each programming language,
so the package is almost plug-and-play. For instance, format-all uses
[clang-format](https://clang.llvm.org/docs/ClangFormat.html) for
C/C++, [prettier](https://prettier.io/) for JavaScript/Typescript, and
[black](https://github.com/psf/black) or
[yapf](https://github.com/google/yapf) for Python. You can find the
list of supported languages
[here](https://github.com/lassik/emacs-format-all-the-code#supported-languages). On
top of that, format-all provides an Emacs minor mode,
`format-all-mode`, that enables format on save. Like most third-party
Emacs packages, it's recommended to install format-all [from
MELPA](https://melpa.org/#/format-all).

After installation, there's little configuration to be done. You have
to make sure that you have the external formatter programs
(e.g. clang-format, prettier, etc.) installed and put on your
`PATH`. I define a custom function to extend the `format-all-buffer`
command to operate on Prolog buffers as well, since format-all
currently does not support the Prolog language. With inspiration taken
from Visual Studio Code, I bind `Alt-Shift-f` to invoke my custom
function and auto-format the current buffer. Lastly, I make sure the
default formatter is used whenever I enter a coding buffer of a
recognized programming language.

Putting everything together, this is all the configuration I have for format-all:

```emacs-lisp
(use-package format-all
  :preface
  (defun ian/format-code ()
    "Auto-format whole buffer."
    (interactive)
    (if (derived-mode-p 'prolog-mode)
        (prolog-indent-buffer)
      (format-all-buffer)))
  :config
  (global-set-key (kbd "M-F") #'ian/format-code)
  (add-hook 'prog-mode-hook #'format-all-ensure-formatter))
```

Here, `M-F` is synonymous with `Alt-Shift-f` because Emacs understands
`Alt` as the `Meta` key. I didn't enable "format-on-save" because I'm
more used to manually invoking the formatting function.

If you're interested in more customization options for format-all,
such as setting specific formatters for a project (via
`.dir-locals.el`), you can read [this
section](https://github.com/lassik/emacs-format-all-the-code#how-to-customize)
of the README. If you are unfamiliar with use-package, you can read my
quick-start guide [here](../setting-up-use-package).

That's all for today, have a good one.

## Update (Apr 2026)

When I wrote this post back in 2021, I was in university writing
predominantly personal projects where I controlled the style of the
entire codebase. A few years into working professionally as a software
engineer, I've found myself reaching for `format-all-region` far more
often than `format-all-buffer`. Most days I'm working on shared
codebases and only want to format the new code I'm adding - formatting
an entire file touched by other authors generates whitespace noise in
my commits and is frowned upon in most engineering teams.

The format-all package has since implemented the very handy
`format-all-region-or-buffer` command that will intelligently call
either `format-all-region` or `format-all-buffer` depending on whether
a region is active. In addition, format-all also supports customizing
specific formatters. So for instance, if I want to use `yapf` to
format Python code instead of the default `black`, I can set it
locally whenever I visit a Python buffer. My updated configuration is
as follows:

```elisp
(use-package format-all
  :preface
  (defun ian/format-code ()
    "Auto-format region if active, otherwise format whole buffer."
    (interactive)
    (if (derived-mode-p 'prolog-mode)
        (prolog-indent-buffer)
      (format-all-region-or-buffer)))
  :config
  (global-set-key (kbd "M-F") #'ian/format-code)
  (add-hook 'prog-mode-hook #'format-all-ensure-formatter)
  (add-hook 'python-mode-hook #'(lambda ()
                                  (setq-local format-all-formatters '(("Python" yapf))))))
```
