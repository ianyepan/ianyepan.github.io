---
title: Native Emojis in Emacs
author: Ian Y.E. Pan
date: 2022-02-16 22:18:00 -0500
categories: [Emacs]
tags: [linux, emacs, tutorial, tips, workflow, programming]
---

This short post will help you set up emoji support in Emacs.

First, let's see the results:

![Emojis](/images/emacs-emojis.png){: width="80%"}
_Emacs showing emojis_

Step one, you must have an emoji font installed. I really adore the
new fluent design emojis by Microsoft (the new one on Windows 11),
under the name "[Segoe UI
Emoji](https://docs.microsoft.com/en-us/typography/font-list/segoe-ui-emoji)". Other
popular emoji fonts include Google's "Noto Color Emoji" and Apple's
"Apple Emoji". Since I'm using Windows 11 WSL2's Arch Linux, that
means I had to copy the .ttf font file(s) to under `~/.fonts/` and
refresh the font information cache files with `fc-cache -f`.

The next step is to tell Emacs to display characters in the "symbol"
font-set using the "Segoe UI Emoji" font (or whichever emoji font you
decided to go with). This can be achieved with
the function `set-fontset-font`, which takes quite a few
arguments. The only one you should worry about is to correctly specify
the font family name in the FONT-SPEC argument. Lastly, I wrap the
function call in a protective condition, running it only when the
"Segoe UI Emoji" font is correctly installed and found (i.e. a member
of the font-family-list).

```emacs-lisp
(when (member "Segoe UI Emoji" (font-family-list))
  (set-fontset-font
    t 'symbol (font-spec :family "Segoe UI Emoji") nil 'prepend))
```

At this point, Emacs can already correctly display emojis in your
buffer. But what if you want to search and insert emojis by their
name, or describe an emoji at the point of the cursor? The answer is
the package
[emacs-emojify](https://github.com/iqbalansari/emacs-emojify).

There are different types of emojis, including plain text ones like
"`:)`", unicode ones like "🙂", and GitHub-style ones like
"`:smile:`". I only care about the unicode ones to properly show up,
so I set the display style and emoji styles as follows. Note that
these variables are defined from the emacs-emojify package.

```emacs-lisp
(setq emojify-display-style 'unicode)
(setq emojify-emoji-styles '(unicode))
```

The package offers us a convenient way to search and insert
emojis, exposed through the function `emojify-insert-emoji`. I bound
it to Ctrl-C followed by a period, mimicking the "Windows key +
period" keybinding that is used system-wide on Windows for inserting
emojis. I'm using the `bind-key*` function with an asterisk, which
overrides any mode-specific bindings:

```emacs-lisp
(bind-key* (kbd "C-c .") #'emojify-insert-emoji) ; override binding in any mode
```

Finally, wrap all of these configuration in a use-package block and we're
done! Here's the full snippet you'll need to get emojis to work in
Emacs:

```emacs-lisp
(use-package emojify
  :config
  (when (member "Segoe UI Emoji" (font-family-list))
    (set-fontset-font
     t 'symbol (font-spec :family "Segoe UI Emoji") nil 'prepend))
  (setq emojify-display-style 'unicode)
  (setq emojify-emoji-styles '(unicode))
  (bind-key* (kbd "C-c .") #'emojify-insert-emoji)) ; override binding in any mode
```

The package "emacs-emojify" actually provides a lot more functionality
than this, including enabling emojis only in certain contexts, setting
our custom emojis with PNG files, integration with other packages such as
company, and describing emojis at the cursor point. If you're interested,
I highly recommend going through their docs to learn more about it.

That's all for this time, cheers 👋.
