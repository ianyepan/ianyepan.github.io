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
Valley [when the protagonist Richard was arguing with his new software
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

#### FAQ 1: Should I give up Visual Studio Code and use Emacs or Vim?

No, no one should ever tell you what you should use.

You don't have to give up Visual Studio Code, because there is nothing
wrong with it. Microsoft has done a brilliant job pushing this editor
forward, introducing a beautiful user interface now serving as
community standard, maintaining a plug-and-play Marketplace,
perfecting the integration with Windows Subsystem for Linux, and
inventing LSP (Language Server Protocol), which is now widely adopted
by almost every editor through plugins and packages, including Emacs
and Vim.

#### FAQ 2: What's with you Emacs/Vim users, what *"eXactLY iS sO goOd"* about them?

It's impossible to explain it in a few words, and I won't even bother
trying. What I will say is, once you grok it, it's like Christopher
Columbus discovering the Americas. You will chuckle at yourself for
not learning it sooner.

#### FAQ 3: Is Emacs/Vim for everyone?

I wish it were, but it isn't. It takes a massive amount of time and
self-hypnotizing perseverance to get through the first few weeks of
psychological pain and counter-productivity. The few people I know
who've tried could not get over this phase.

## My subjective take on the Editor War

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
people sharing the experience of moving from Vim to Emacs. Why is
there barely anyone talking about moving from Emacs to Vim? Well,
because once you exposed yourself to this brilliantly magical piece of
software that is Emacs, there's no turning back.

A common argument from the Vim community is that Emacs is super
heavy. But that's false: both editors are lightweight by default (at
least to today's standards) and start up in an instance. Whenever you
hear people complaining about Emacs being "a kitchen sink" or "bloat",
it's because its ease of extension oftentimes makes you lose yourself
in the sea of packages. And as a new user, it's easy to think that
"more is better".

So, is Vim completely worthless? No, absolutely not! Vim (or its
predecessor, Vi) is ubiquitously installed on nearly any machine I
could imagine. When you're dropped into a foreign command line and
need to edit some configuration files buried somewhere deep in the
file system, Vim may be your only friend.

At this point you might ask, what would be beneficial to a newbie
programmer/developer/CS student?

My recommendation would be the following. Learn to touch type before
anything else. Then, learn programming to a decent extent before
worrying about which editor to master. In the first couple of months,
just use Visual Studio Code or Sublime Text or Atom or whichever IDE
your professor or supervisor recommends. Once you're comfortable with
programming in a couple of languages and gradually notice that editing
speed has become the bottleneck of your coding efficiency, you can
start learning Vim.

The beauty of Vim is that [it fundamentally
changes](https://www.youtube.com/watch?v=ST7vnfKjfvY) the way you
perceive text editing. You no longer operate on mere characters with
your cursor, but rather on text objects like words, sentences, lines,
paragraphs, and contents within parentheses, quotes, and tags. You
seldom have to touch the mouse again because Vim's efficient
keybindings will help you glide through your coding buffer from one
position to another. Having mastered this ubiquitous editor, you will
find yourself editing system configuration files with ease and
eventually learning more about the computer architecture and structure
of your operating system because this terminal editor has opened the
doors for you to operate on any system file you wish.

After spending a year or two in Vim, I would recommend that you try
out Emacs, specifically with the [Evil
Mode](https://github.com/emacs-evil/evil) package, so that you can
re-use all the muscle memories you acquired in your time with Vim, now
in Emacs' superior computing environment. You'd probably have heard
some Vim users claiming the holy status of their beloved editor and
belittling Emacs as a bloated kitchen sink that is slow and
useless. Don't listen to them! They most likely spent only a day or
two in Emacs before jumping right back to their comfort zone. There's
a reason that the creator of Python, the creator of Java, the creator
of Ruby, the creator of Clojure, the creator of TeX, the creator of
Erlang, the author of MySQL, the creator of JavaScript, the founder of
WikiLeaks, and the founder of Facebook (yes, Mark Zuckerburg) all used
or still use Emacs as their main editor ([Reference
1](http://wenshanren.org/?p=418) and [Reference
2](http://ergoemacs.org/misc/famous_emacs_users.html))

And that reason is simple. Emacs is the best editor in the world.

## Concluding words

In this post, I shared my view of Emacs and Vim, the two subjects in
the Editor War. Do take this opinionated article with a (big) grain of
salt, as I'm obviously partial to Emacs. Objectively speaking, it
doesn't matter which editor you use &mdash; at the end of the day,
they're just tools. And the bottom line is you won't get paid based on
which computing environment you use to accomplish the work. You get
paid for how good your work is.
