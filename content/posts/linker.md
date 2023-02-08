---
title: "Minimum Viable Linker"
date: 2023-02-07T15:46:36-05:00
draft: true
---

I want to learn what goes into writing a compiler. And the best way to learn is by doing, so I though maybe I could just write a tiny one. Nothing too big or fancy. All I need to do is get from a sort-of-high-level-ish language to something executable.

Naturally, the next question was where to start. I mean, we'll need a compiler (obviously), that'll generate some (unoptimized) assembly, which'll be assembled by an assembler into an object file, which will be linked into an executable by a linker. I could start at any point in the chain, since each component could probably just be swapped out with GCC's version.

Anyway, why not start from the bottom up. What is the smallest possible linker? All it needs to do is string together a few self-contained object files into something executable.


