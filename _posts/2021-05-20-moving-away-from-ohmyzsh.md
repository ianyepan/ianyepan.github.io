---
title: Moving Away From Oh-My-Zsh
author: Ian Y.E. Pan
date: 2021-05-20 20:18:00 +0800
categories: [Linux]
tags: [workflow, linux]
---
> If you have 12 minutes of free time and would like to learn more
> about this topic, I highly recommend watching Brodie
> Robertson's [YouTube video on why you should uninstall oh-my-zsh](https://www.youtube.com/watch?v=21_WkzBErQk).

I started using zsh around 4 years ago, when macOS was still using
bash as the default shell just like most Linux distributions. The
reason I made the switch from bash to zsh was because Linux YouTubers
like [Luke
Smith](https://www.youtube.com/channel/UC2eYFnH61tmytImy1mTYvhA) and
[DistroTube](https://www.youtube.com/channel/UCVls1GmFKf6WlTraIb_IaJg)
were popularizing and advocating it. Another shell in question was
fish. But after trying out both zsh and fish, I felt like the former
had a greater following and community support. For that reason, I
decided to use zsh as my default shell.

Like most new zsh users, the first thing I did after installing zsh
was installing [oh-my-zsh](https://github.com/ohmyzsh/ohmyzsh) and
followed the README to choose a cool theme for my prompt (there are
more than 140 of them!). For those who have never heard of it,
oh-my-zsh is a community-driven framework and plugin manager that can
allow zsh users to extend their shell functionality and appearances
with significant ease.

Technically, using the term
"[plugins](https://github.com/ohmyzsh/ohmyzsh/wiki/Plugins)" is a bit
of a stretch. Every zsh "plugin", for instance "autojump", or
"colored-man-pages", is just a small shell script, that can be
"plugged into" your `.zshrc` configuration when loading up your zsh.

## So, why move away?

The short answer is "bloat". I find myself using oh-my-zsh only for
prompt themes, colored man pages, fzf, and git integration. Yet there
are 286 plugins in total, waiting for me to enable them. Do I really
need them installed on my machine? It's also not uncommon to think
that "I might as well enable these plugins now that they're installed
anyways" and bit by bit the load time of launching a shell grows.

## What are my essentials?

So last week, I sat down before my `.zshrc` config file and inspected
it. I had made a list of the essentials I need, and if by the end of
my small research, I can do without oh-my-zsh, then I'd uninstall it.

The essential list was:

- A nice prompt.
- No duplicate history when reverse-searching my command history.
- Case insensitive completion.
- Emacs-style keybindings in zsh's line editor (the place where you
  type out the commands).
- zsh syntax highlighting.

In the process of listing my essentials, I eliminated "git
integration" from my command line, since 99% of the time I deal with
git stuff in Emacs using the "Magit" package anyways. I have developed
a rather sophisticated workflow in
[Magit](https://emacsair.me/2017/09/01/magit-walk-through/) that
allows me to stage, commit, push, pull, stash, rebase, reword, and
squash etc. Showing the git status in my shell prompt is cool, but not
essential.

## Uninstalling oh-my-zsh and plugging in my essentials

With a clear goal in mind, I [uninstalled
oh-my-zsh](https://github.com/ohmyzsh/ohmyzsh#uninstalling-oh-my-zsh). The
next step is to put back the five essentials listed above into my
`.zshrc` file.

### Goal 1: A nice prompt

This one is easy &mdash; I just need my prompt to be shortened and
easy to notice. With the script `PROMPT='ianpan@arch:%1~/ %# '` placed
somewhere in `.zshrc`, I can make my shell prompt look like this:

![Zsh prompt 1](/images/zsh-prompt1.png){: width="80%"}
_PROMPT='ianpan@arch:%1~/ %# '_
