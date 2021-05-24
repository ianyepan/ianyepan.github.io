---
title: My Subjective Take on the Editor War
author: Ian Y.E. Pan
date: 2021-05-24 14:59:00 +0800
categories: [Emacs]
tags: [emacs, vim, linux]
---

> I use both Emacs and Vim. However, I spend 99% of my time in Emacs,
> and only 1% in Vim &mdash; read on to find out why.

I first heard of the names "Emacs" and "Vim" in the TV Show Silicon
Valley [when protagonist Richard was arguing with his new software
developer girlfriend about Tabs
vs. Spaces](https://www.youtube.com/watch?v=SsoOG6ZeyUI) (probably not
the best topic when you're trying to get laid).

If you don't know much about these two editors, this post might not
make much sense to you. If you're interested regardless, I suggest you
take a few minutes and read [this Wikipedia
article](https://en.wikipedia.org/wiki/Editor_war) first. I will not
dive into the differences between these two editors, or compare their
advantages and disadvantages, as there are already hundreds of similar
blog posts and videos online. Instead, I will focus on my hands-on
experiences and subjective opinions about these tools.

## FAQ

Before getting into my take on the editor war, let's start with a few
frequently asked questions.

#### FAQ1: Should I give up Visual Studio Code and use Emacs or Vim?

No, no one should ever tell you what you should use.

You don't have to give up Visual Studio Code, because there is nothing
wrong with it. Microsoft has done a brilliant job pushing this editor
forward, introducing a beautiful user interface now serving as
community standard, maintaining a plug-and-play Marketplace,
perfecting the integration with Windows Subsystem for Linux, and
inventing LSP (Language Server Protocol), which is now widely adopted
by almost every editor through plugins and packages, including Emacs
and Vim.

#### FAQ2: What's with you Emacs/Vim users, what *"eXactLY iS sO goOd"* about them?

It's impossible to explain it in a few words, and I won't even bother
trying. What I will say is, once you grok it, it's like Christopher
Columbus discovering the Americas. You will chuckle at yourself for
not learning it sooner.

#### FAQ3: Is Emacs/Vim for everyone?

I wish it were, but it isn't. It takes a massive amount of time and
self-hypnotizing perseverance to get through the first few weeks of
psychological pain and counter-productivity. The few people I know
who've tried could not get over this phase.

## Which one should I use, Emacs or Vim?

Vim has condescendingly superior keybindings and hence better
text-editing efficiency, while Emacs has the upper hand in pretty much
anything else, including better GUI support, greater extensibility,
better configuration language, superior package designs (such as Magit
and Org). Most important of all, Emacs allows you to completely
emulate Vim's keybindings and its superior text-editing efficiency,
and basically anything that you thought Vim is good at.

You might wonder, if Emacs exists, why would anyone still choose Vim
over it? The answer is simple: because they never gave Emacs the try
it deserves. Go on the internet now and you'll see blog posts of
people sharing the experience moving from Vim to Emacs. Why is there
barely anyone talking about moving from Emacs to Vim? Well, because
once you exposed yourself to this brilliantly magical piece of
software that is Emacs, there's no turning back.

A common argument from the Vim community is that Emacs is super
heavy. But that's false: both editors are lightweight by default (at
least to today's standards) and start up in an instance. Whenever you
hear people complaining about Emacs being "a kitchen sink" or "bloat",
it's because its ease of extension oftentimes makes you lose yourself
in the sea of packages. And as a new user, it's easy to think that
"more is better".

So, is Vim completely worthless?

No, absolutely not! Vim (or its predecessor, Vi) is ubiquitously
installed on nearly any machine I could imagine. When you're dropped
into a foreign command line and need to edit some configuration files
buried somewhere deep in the file system, Vim may be your only friend.

## My suggestion for newbie programmers/developers/CS students
