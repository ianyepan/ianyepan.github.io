---
title: Profiling Performance in Emacs
author: Ian Y.E. Pan
date: 2022-03-11 23:30:00 -0500
categories: [Emacs]
tags: [linux, emacs, tutorial, tips, workflow, programming]
---

> This short blog post showcases the builtin "performance profiling"
> support that Emacs has. TLDR: all you need to know are 3 commands:
> `M-x profiler-start`, `M-x profiler-stop`, and `M-x
> profiler-report`.

## Backstory

Earlier this afternoon, I was checking out a tutorial series on OpenGL
and GLFW, since I've just finished my midterm exams at Columbia and
was looking for interesting C++ frameworks to look into. Though the
industry standard is to develop OpenGL on Visual Studio (the IDE, not
VSCode), me as a proud Emacs user obviously wanted to set it up in my
beloved editor. So I installed `glfw-x11`, `glew`, and `glut` on my
Arch machine, created the project folder structure, wrote a
CMakeLists.txt file, and generated a compilation database
(`compile_commands.json` file) with compiledb so my clangd server can
pick up the library include paths correctly.

I followed the tutorial and wrote some code. When I came
across these lines:

```cpp
std::array<float, 6> positions{-0.25f, -0.25f, 0.0f, 0.0f, 0.5f, -0.5f};
unsigned int buffer;
glGenBuffers(1, &buffer);
glBindBuffer(GL_ARRAY_BUFFER, buffer);
glBufferData(GL_ARRAY_BUFFER, 
             positions.size() * sizeof(float), 
             positions.data(), GL_STATIC_DRAW);
```

, I wanted to investigate the function signature of `glGenBuffers`. I
naturally pressed F12 for LSP's go-to-definition. And to my surprise,
Emacs hung for 10 seconds, before it finally took me to the macro
definition in `/usr/include/GL/glew.h`, which looked something like
this:

```
#define glGenBuffers GLEW_GET_FUN(__glewGenBuffers)
```

I invoked F12 again on `__glewGenBuffers` to explorer deeper. This
time, Emacs straight-up froze for 15 seconds before it finally found
the definition... Well, I thought, this hasn't happened before, surely
it's because this `glew.h` file has more than 26,000 lines of code,
right? But seriously, 15 seconds?

I needed to compare. I opened VSCode to try the same LSP actions: and
it took only 1-2 seconds. My stomach started to ache as I fired up Vim
(with LSP configured) and tried doing the same thing... only 2-3
seconds!

> My sight grew dim as I felt the world collapsing on me -- NO! Emacs
> can't lose to Visual Studio Code or (neo)vim! It's the holy grail of
> all editors!

I started disabling fancy minor modes and unimportant features to see
which package I installed was causing the trouble, but nothing
helped. I tweaked settings of lsp-mode and bumped up the threshold for
certain actions. I even uninstalled Emacs 27.2 and grabbed the latest
Emacs 29.0 with native-compilation (which by the way took me quite a
while to build), yet still! No improvement on the go-to-definition on
the C file that's 26K+ lines long.

## Builtin profiler support in Emacs

Just when I was about to give up, I came across a post on Reddit that
was talking about "profiling" in Emacs. I followed the steps and typed
`M-x profiler-start`, chose "CPU" as the mode, invoked a couple of F12
go-to-definitions of my OpenGL code for Emacs to "record", and ended
the session by typing `M-x profiler-stop`.

I then typed `M-x profiler-report` to see what was really going
on. And these are the results:

```text
- redisplay_internal (C function)                                5109  75%
 - jit-lock-function                                             5094  75%
  - jit-lock-fontify-now                                         5094  75%
   - jit-lock--run-functions                                     5094  75%
    - run-hook-wrapped                                           5094  75%
     - #<compiled 0x158e566e17f5>                                5094  75%
      - font-lock-fontify-region                                 5094  75%
       - #<compiled 0x158e5585c36d>                              5094  75%
        - apply                                                  5094  75%
         - tree-sitter-hl--highlight-region-with-fallback        5094  75%
          - tree-sitter-hl--highlight-region                     5094  75%
           + mapc                                                   5   0%
           + tree-sitter-hl--extend-regions                         5   0%
           + font-lock-fontify-keywords-region                      4   0%
 + eval                                                            15   0%
+ command-execute                                                 888  13%
+ timer-event-handler                                             733  10%
+ ...                                                               9   0%
  jit-lock--antiblink-post-command                                  1   0%
+ #<compiled 0x158e564836dd>                                        1   0%
```

You don't need to be a profiler expert to see that the culprit is
"tree-sitter-hl". For those who don't know,
[tree-sitter](https://tree-sitter.github.io/tree-sitter/) is a "parser
generator tool and an incremental parsing library, capable of building
a concrete syntax tree for a source file and efficiently updating the
syntax tree on-the-fly." I use it mainly to provide my source code
with richer and more modern syntax highlighting. It's a cool package,
but not a necessity. After finding out that tree-sitter is causing the
poor performance in Emacs, I disabled the package, restarted the
editor, and tried the LSP go-to-definition code action one more
time. This time, it took less than a second for clangd to find the
implementation of the macro.

The builtin profiler saved the day, and Emacs feels snappy once again!

## Conclusion

Whenever Emacs feels sluggish, don't panic. Just start the profiler,
perform the actions you thought were slow, and check what was causing
the trouble under the hood. This is my first time using the builtin
profiler, and I've been using Emacs for years! I suppose you learn
something new every day, and this brilliant piece of software from the
70s never ceases to amaze me.
