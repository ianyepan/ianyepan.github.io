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
