---
title: Emacs Appearances in Pop Culture
author: Ian Y.E. Pan
date: 2026-06-07 22:00:00 -0800
categories: [Emacs]
tags: [linux, emacs]
---

> Thank you for the support, this blog post got featured on
> the front page of Hacker News (YCombinator).

As an Emacs user, few things are as delightful as catching my favorite
text editor out in the wild. It doesn't happen often though -- Emacs
is niche, and pop culture rarely gives it a nod. This post tracks down
every one I know of (as of June 2026), and I'll keep adding to it as I
stumble across more.

Here you go, in no particular order:

## 2010 Movie, *The Social Network*

*The Social Network* is a biographical drama film portraying the
founding of Facebook.

![The Social Network](/images/social-network-dorm.jpg){: width="60%"}
_The Social Network (2010)_

In the scene where young Zuckerberg (played by Jesse Eisenberg) is
putting together Facemash by scraping pictures from all the Harvard Houses
(campus dorms), he fires up Emacs and writes a Perl script to crawl
the website of Leverett House.

![Emacs in The Social Network](/images/social-network-emacs-2.png){: width="90%"}
_Movie scene where Zuckerberg is shown scripting Perl on Emacs in his
Harvard dorm room_

As the movie scene plays, Zuckerberg narrates, "*... and there's no way
I'm gonna go through 500 pages to download pics one at a time. So it's
definitely necessary to **break out Emacs** and modify that Perl script.*"

## 2010 Movie, *Tron: Legacy*

The other movie featuring Emacs coincidentally hit theaters the
same year, 2010. *Tron: Legacy* is a well-received sci-fi film and the
second installment of the Tron series. The Daft Punk soundtrack was
awesome too, to say the least.

![Tron Legacy](/images/tron-legacy-movie.jpg){: width="60%"}
_Tron: Legacy (2010)_

In one of the opening scenes, Edward Dillinger Jr.
(played by Cillian Murphy) fires up Emacs' eshell to grep and kill the
system process that protagonist Sam Flynn initiated to attack ENCOM's new OS 12.

![Emacs eshell in Tron Legacy](/images/tron-legacy-emacs.png){: width="90%"}
_Emacs' eshell used to grep and kill Flynn's hacking program_

P.S. Inspired by this movie scene, I created an Emacs color theme
based on the color palette of *Tron: Legacy*. Check it out at
[https://github.com/ianyepan/tron-legacy-emacs-theme](https://github.com/ianyepan/tron-legacy-emacs-theme). My
repo passed 200 GitHub stars not too long ago. I suppose I made
quite a few people happy.

## 2010 Movie, *Arctic Blast*

Another 2010 film -- this time, a sci-fi disaster movie jointly
produced between Australia and Canada. At around the 20:30 timestamp,
two scientists, Jack and Zoe, attempt to recover some satellite photos
from a frozen hard drive. We see a scrolling wall of Emacs Lisp on
their computer for a brief moment as Jack disappointedly said that
most of the files are corrupted.

![Arctic Blast](/images/arctic-blast-emacs.png){: width="90%"}
_Two scientists using Emacs Lisp to recover data from a hard drive_

`;;;###autoload`, `interactive`, and `save-excursion` are all
unmistakably Emacs Lisp syntax. The Elisp program shown on screen is
in fact the [xml-parse module source
code](https://www.emacswiki.org/emacs/xml-parse.el), 
authored by John Wiegley back in 2001.

## 2014-2019 HBO, *Silicon Valley*

*Silicon Valley* is one of my favorite shows (my all-time favorite is
still Mr. Robot). It's a comedy series parodying tech-industry culture,
and it packs a surprising amount of insight into the software engineer
lifestyle, the dynamics of VC funding, and the underdog startup's fight
against the big corporations.

![Silicon Valley](/images/silicon-valley-show.png){: width="60%"}
_Silicon Valley (2014-2019)_

In a scene (Season 3, Episode 6) where protagonist Richard is coding
with his new girlfriend Winnie at her apartment (okay, yeah... that's
not how all software engineers date, whatever the outside world may
think), the two clash over the use of spaces versus tabs. Richard, a
stubborn advocate of the tab character for indentation, argues: "*I mean
I do not get why anyone would use spaces over tabs. I mean, why not just
use Vim over Emacs?*" To which Winnie replies, "*I do use Vim over
Emacs.*" Richard then breaks down, yelling, "*Oh, God help us!*"

![Tabs vs Spaces fight](/images/silicon-valley-fight.png){: width="90%"}
_Richard argues with Winnie over indentation style and choice
of editor_

Genius scene by HBO, sneaking in a brief reference to [the editor
war](https://en.wikipedia.org/wiki/Editor_war) in the middle of a
fight over indentation style. Not so genius for our poor Richard.

This scene is particularly important to me. It was, in fact, my very
first exposure to both Vim and Emacs. I remember sitting in my
university library that one evening ~10 years ago, taking a break from
studying to watch this episode, and thinking to myself, "What **are**
Vim and Emacs?" I looked them up, learned that all the 10x developers
seemed to swear by one or the other, and decided I would pick up Vim
first. After a year with Vim, I switched to Emacs with Evil-mode
full-time -- and here I am, writing this blog post in Emacs on a
Sunday night. And first thing tomorrow at work? Probably fire up Emacs
to review some pull requests : -)

## 1992-1993 DC Comics, *The Hacker Files*

*The Hacker Files* is a twelve-issue DC comics mini-series about a
freelance hacker exposing a multinational conspiracy and taking down
an evil corporation. It's a pretty good read!

In the first issue, protagonist Jack Marshall uses Emacs to edit a
source file to fight a computer virus. The comic doesn't show the
text editor's user interface, just the command `emacs cure.c`.

![The Hacker Files](/images/the-hacker-files-emacs.png){: width="90%"}
_The Hacker Files (1992-1993), Issue #1_

## 2013-2019 Manga series, *Ōsama-tachi no Viking (The King's Viking)*

*Ōsama-tachi no Viking* is a Japanese manga series about a high school
hacker teaming up with a wealthy angel investor to reshape the world
order.

In one chapter, an enemy hacker uses Emacs Lisp to exploit security
cameras (credits to: [this Reddit comment](https://www.reddit.com/r/emacs/comments/1bcxyps/comment/kvk33df/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1&utm_content=share_button)).

![The King's Viking](/images/the-kings-viking-elisp.png){: width="90%"}
_Emacs Lisp sighting in Ōsama-tachi no Viking_

The code may look like any generic Lisp variant (yes, the many
parentheses give it away), but look closely -- `pcase` and `seq-map`
are Emacs-specific constructs, from `pcase.el` and `seq.el` --
part of Emacs since 24.1 and 25.1 respectively.

Personally, I prefer `mapcar` or `cl-map` to `seq-map` in my own Emacs
Lisp code for slightly better runtime performance, but I suppose a
hacking script wouldn't care about micro-optimizations in the heat of
the moment -- as long as it does the job!

## 1994-1996 OVA, *Key the Metal Idol*

> Thank you u/PuercoPop on Reddit for suggesting this entry!

*Key the Metal Idol* is a Japanese anime series from the 90s. It follows
the story of a robotic girl Tokiko "Key" Mima and is a "somewhat dark
drama with elements of mecha and sci-fi".

In episode 9, *Return*, the mysterious character "D" is locked in a
cell with just computer terminal. In a close-up scene, we see D
hitting the return key and a scrolling wall of Emacs Lisp shows up on
his terminal screen.

![Key the Metal Idol](/images/key-the-metal-idol-emacs.png){: width="95%"}
_Emacs Lisp sighting in Key the Metal Idol (1994-1996)_

There is no mistaking for other Lisp variants, both `save-excursion`
and `set-buffer` are Emacs Lisp specific keywords.

## 2013 Movie, *The Internship*

> Thank you u/dagobah1202 on Reddit for suggesting this entry!

*The Internship* is a comedy film following the story of two 40-year-old
salesmen spending the summer competing with other much younger and
more technically skilled applicants for a job at Google. Despite its
many inaccuracies in depicting real software engineer lifestyle at
Google, it's still an enjoyable lighthearted movie.

In a scene where character Nick Campbell (played by Owen Wilson) tries
to impress a Google executive during her presentation, he raised the
question, "*Why not use Emacs rather than Vi as the default editor for
Ubuntu?*" To which the executive (played by Rose Byrne) replied,
"*That's actually a very good thought, Nick.*" Ironically, the scene is
largely unrealistic because if those were real programmers sitting
next to him, an all-out war would've started right then and there
(/s).

![The Internship Emacs vs Vi](/images/the-internship-emacs-vi.png){: width="95%"}
_Scene from The Internship (2013). A presentation slide compares the
popularity of source code editors_

## 2014-2015 Anime series, *Aldnoah.Zero*

> Thank you Reisen for suggesting this entry via email!

In Episode 5 of Japanese sci-fi anime *Aldnoah.Zero*, we catch
glimpses of both Emacs and Emacs Lisp during a fight between two
mechas. Blink and you'd miss it.

Look closely towards the bottom right, our pilot appears to be debugging
some issues with their `.emacs` initialization file. We've all declared
[.emacs
bankruptcy](https://www.emacswiki.org/emacs/DotEmacsBankruptcy) at one
point so I can certainly relate to the pain. But in the middle of a
mecha fight? Now that's a first.

![Aldnoah.Zero](/images/aldnoahzero-emacs1.png){: width="90%"}
_Backing up the .emacs init file to start over is a pain we've all gone
through at some point_

Some random snippets of Emacs Lisp are also shown on screen
shortly after. Look closely and you shall recognize keywords like
`progn`, `insert`, `beginning-of-line`, and `forward-char`.

![Aldnoah.Zero](/images/aldnoahzero-emacs2.png){: width="75%"}
_Some random Emacs Lisp shown on screen_

## 2017 Documentary, *AlphaGo*

> Thank you u/\_0-__-0\_ on Reddit for suggesting this entry!

*AlphaGo - The Movie* is an award-winning documentary about how
AlphaGo, a program developed by Google DeepMind, mastered the game of
Go through A.I. (machine learning with convolutional neural networks
trained by reinforcement learning), and played against top-ranked Go
player Lee Sedol.

During an introductory scene in which the narrator is explaining what
"neural networks" mean, the camera shows the Ubuntu desktop of a
software engineer programming Lua in Emacs.

![AlphaGo](/images/alphago-emacs1.png){: width="90%"}
_A DeepMind software engineer using TUI Emacs within Tmux, in Gnome Terminal_

The engineer appears to be using the *default* theme in the *default*
terminal app (Gnome Terminal) on Ubuntu, with the *default* Ubuntu
Mono font. He is also using the *default* dark theme in GNU Emacs,
with the background color set to `nil`, so it falls through to use the
terminal purplish background. A minimalist purist at heart indeed.

![AlphaGo](/images/alphago-emacs2.png){: width="90%"}
_A close-up shot of the programmer writing Lua in GNU Emacs. Looks
like there's a typo in the file name._


## 2019-2025 Netflix series, *How to Sell Drugs Online (Fast)*

> Thank you u/MiEdCaLe on Reddit for suggesting this entry!


![How to Sell Drugs Online (Fast)](/images/htsdo-poster.jpg){: width="40%"}
_How to Sell Drugs Online (Fast), Netflix TV series_

In Season 2, Episode 1 of this German Netflix series, Kira, an Emacs
advocate with elite hacking skills, jokes about Vi having two modes,
'beeping non-stop' and 'breaking everything'. Her date, Lenny, argues
back, "*Is that so? Emacs is surely much better - until your hands fall
off from hitting the shortcuts!*" Shortly earning himself a french fry
thrown at his face...

Jokes on you, Lenny. I use Emacs with Evil-mode -- the best of both worlds!

![How to Sell Drugs Online (Fast) - Emacs vs Vi](/images/htsdo-emacs.png){: width="90%"}
_Lenny, a Vi user, pokes fun at Emacs' nasty default shortcuts_

## 2023 Hulu miniseries, *A Murder at the End of the World*

> Thank you u/xenodium on Reddit for suggesting this entry!

*A Murder at the End of the World* is a murder mystery / psychological
thriller TV miniseries. I am definitely watching this very soon.

In one scene, the main character Darby Hart (played by Emma Corrin)
asked a lady out of the blue, "*Are you Vi or Emacs?*" to
see if she'd show a visible reaction and if not, she was probably not
a hacker.

![Are you Vi or Emacs](/images/murder-vi-or-emacs.webp){: width="90%"}
_Are you Vi or Emacs?_

The GIF above is taken from Xenodium's blog post
[https://xenodium.com/are-you-vi-or-emacs](https://xenodium.com/are-you-vi-or-emacs). The
author Álvaro Ramírez is also the creator behind the YouTube playlist
[Bending
Emacs](https://www.youtube.com/playlist?list=PLudVBwrl_ir84jCQtAzDVtBc_oSIBvdCO).

## 2002 Polish movie, *Haker (Hacker)*

> Thank you u/Esnos24 on Reddit for suggesting this entry!

*Haker* is a Polish comedy film that follows the story of two high
school friends sharing the passion of hacking and getting themselves in
trouble with some real gangsters.

In a scene where one of them is having difficulty breaking through a
firewall, the other suggested, "*Have you tried Emacs through
sendmail?*" Now maybe my hacking skills are too amateur for these two,
but I'm not quite sure what that dialogue is supposed to mean. Emacs
surely can send mail via sendmail, but that's just... sending an
email, not some firewall penetration technique. Historically speaking,
sendmail does have a reputation of being vulnerable, perhaps the
writer was referring to this. It's also possible that the English
subtitles didn't do the best job translating the original Polish
dialogue.

![Haker Emacs](/images/haker-emacs.png){: width="90%"}
_Have you tried Emacs through sendmail?_

## Honorable mentions

A few honorable mentions that may not fall into pop-culture sightings,
but are too good to leave out:

- **xkcd #378, *Real Programmers*** -- the famous strip where "real
  programmers use butterflies" to flip disk bits, capped by the line
  "'Course, there's an Emacs command to do that... good ol' `C-x M-c
  M-butterfly`." Emacs later added a real `M-x butterfly` command as an
  easter egg nodding to this very comic.

  ![XKCD Emacs](/images/xkcd-real-programmers.png){: width="60%"}
  _The origin of M-x butterfly_

- **Neal Stephenson, *In the Beginning... Was the Command Line*
  (1999)** -- the sci-fi novelist devotes a loving passage to Emacs,
  calling it "a thermonuclear word processor" and "outshines all other
  editing software in approximately the same way that the noonday sun
  does the stars".
  
- **And [here is a list of famous Emacs
  users](http://xahlee.info/emacs/misc/famous_emacs_users.html),
  curated by Xah Lee.** Notably:
  - Donald Knuth (Turing Award winner; father of analysis of algorithms)
  - Guido van Rossum (creator of Python)
  - Yukihiro Matsumoto (creator of Ruby)
  - Simon Peyton Jones (creator of Haskell)
  - Jeff Dean (Google's Chief Scientist, leading Google AI, Google DeepMind, and Google Research)
  - Jonathan Blow (game developer; creator of Jai programming language)
  - Julian Assange (founder of WikiLeaks)
  - Linus Torvalds (creator of Linux; technically uses micro-emacs, not GNU Emacs)
  - etc.


That's it for today, hope you enjoyed the post as much as I did
writing it! If you know of a reference that I left out, please don't
hesitate to send me an email at
[ian.pan@columbia.edu](mailto:ian.pan@columbia.edu).

## References
- [https://medium.com/h0llyw00d-h4x0rs/the-social-network-6591ec03443d](https://medium.com/h0llyw00d-h4x0rs/the-social-network-6591ec03443d)
- [https://www.youtube.com/watch?v=KdtPNRzuKrk](https://www.youtube.com/watch?v=KdtPNRzuKrk)
- [https://www.reddit.com/r/emacs/comments/ged5p/emacs_in_tron_legacy_three_images/](https://www.reddit.com/r/emacs/comments/ged5p/emacs_in_tron_legacy_three_images/)
- [https://www.reddit.com/r/emacs/comments/1bcxyps/spotted_emacs_in_a_comic_emacs_cultural/](https://www.reddit.com/r/emacs/comments/1bcxyps/spotted_emacs_in_a_comic_emacs_cultural/)
- [https://www.youtube.com/watch?v=Qeh3E67brBs](https://www.youtube.com/watch?v=Qeh3E67brBs)
- [https://www.handsonprogramming.io/blog/2024/02/emacsintron](https://www.handsonprogramming.io/blog/2024/02/emacsintron)
- [https://www.reddit.com/r/emacs/comments/1bcxyps/comment/kvk33df/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1&utm_content=share_button](https://www.reddit.com/r/emacs/comments/1bcxyps/comment/kvk33df/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1&utm_content=share_button)
- [https://www.imdb.com/title/tt5218484/](https://www.imdb.com/title/tt5218484/)
- [https://www.youtube.com/watch?v=V7PLxL8jIl8](https://www.youtube.com/watch?v=V7PLxL8jIl8)
- [https://web.stanford.edu/class/cs81n/command.txt](https://web.stanford.edu/class/cs81n/command.txt)
- [http://xahlee.info/emacs/misc/famous_emacs_users.html](http://xahlee.info/emacs/misc/famous_emacs_users.html)
- [https://www.reddit.com/r/emacs/comments/11kqgav/emacs_lisp_cameo_in_anime_series_key_the_metal/](https://www.reddit.com/r/emacs/comments/11kqgav/emacs_lisp_cameo_in_anime_series_key_the_metal/)
- [https://xenodium.com/are-you-vi-or-emacs](https://xenodium.com/are-you-vi-or-emacs)
- [https://www.tumblr.com/moviecode/76927836921/from-key-the-metal-idol-episode-9-the-code-shown](https://www.tumblr.com/moviecode/76927836921/from-key-the-metal-idol-episode-9-the-code-shown)
- [https://www.youtube.com/watch?v=lpdngXQOkBI](https://www.youtube.com/watch?v=lpdngXQOkBI)
- [https://www.forbes.com/sites/quora/2013/06/13/what-do-googlers-think-of-the-internship-movie/](https://www.forbes.com/sites/quora/2013/06/13/what-do-googlers-think-of-the-internship-movie/)
- [https://www.youtube.com/watch?v=wsA7N5wmre8](https://www.youtube.com/watch?v=wsA7N5wmre8)
- [https://www.reddit.com/r/emacs/comments/glvb68/meme_when_you_see_emacs_on_tv/](https://www.reddit.com/r/emacs/comments/glvb68/meme_when_you_see_emacs_on_tv/)
- [https://www.youtube.com/watch?v=WXuK6gekU1Y](https://www.youtube.com/watch?v=WXuK6gekU1Y)
- [https://www.tumblr.com/moviecode/75563286489/movie-arctic-blast-they-are-reconstructing-a](https://www.tumblr.com/moviecode/75563286489/movie-arctic-blast-they-are-reconstructing-a)
- [https://www.emacswiki.org/emacs/xml-parse.el](https://www.emacswiki.org/emacs/xml-parse.el)
