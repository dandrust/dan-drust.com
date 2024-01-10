---
layout: post
title: "Building a Shell"
date: 2024-01-03
category: posts
---

I've been part of an early release of [CS Primer](https://csprimer.com) and the [Operating System module](https://csprimer.com/courses/operating-systems/#processes) includes a series of challenges to build a shell. It's a project that I'd been wanting to tackle for a while, so I started it back in October.  And it's finally done!
 
I wrote it in C to get some practice with the language.  The project was broken into three challenges: first a basic shell that uses `fork`/`exec` to run a program, then implementing support for pipes, and finally implementing job control. Each challenge included a code-along video that I only let myself watch after completing each stage (though I did take some inspiration from the videos as I implemented subsequent stages).  You can see the final result [on GitHub](https://github.com/dandrust/shell).

This was the first side project in a while to capture my attention through completion.  I was able to get into a flow state and really find a lot of joy in the process. Below are some reflections on what helped make this a good experience for me:

**Side quests kept things fun**

I went on small tangential side quests when my mind wandered to a related concept or question. For example, I wondered how I might implement an OOP object with a C struct so I spent an afternoon trying it out. It was cumbersome, but now I understand why Python instance methods take an initial `self` param!  I also wanted to be able to print debugging information on structs I was working with, which led me to think about how Rust traits solve that.  A particularly frustrating memory-clobbering bug led to learning enough `gdb` to be dangerous.  I tried to not let productivity be the primary outcome; instead, I tried my best to follow and honor my interests as they appeared and faded.

**Motivation came in waves**

In 2023 I experimented with working on a larger project for at least 30 minutes each day ([Database Daily](https://dan.drust.dev/projects/toy-database)). It was sustainable for a while but it was easy to miss a day or two and get discouraged.  And then a vacation killed it!  With the shell project I let the motivation come to me.  Some weekends I spent 12+ hours on the project, some weekends I didn't even think about it.  The itch to work on the shell usually came after some time away from it. It would be nice to find a sustainable, predictable cadence to work on these sorts of projects.  For now, letting intrinsic motivation steer the ship seems to work.

**The manpages only got me so far**

Initially I tried very hard to avoid any spoilers and rely only on manpages and my own previous understanding.  This approach worked for the basic shell and for implementing pipes.  It *really* fell apart when I started working on job control, however.  I struggled to understand the big picture with process groups and signals, and with terminal control and signals.  I ended up reading [CS:APP](https://csapp.cs.cmu.edu)'s chapter on exceptional control flow which helped quite a bit, and then I reviewed multiple chapters on signaling, child processes, and process groups in [TLPI](https://man7.org/tlpi/).  The TLPI chapters were particularly helpful -- it was there that I was able to build up a mental model for what job control was all about and how processes are organized.  

Even though I ended up consulting some resources that held spoilers, I didn't allow myself to copy code from them.  I could only take inspiration and I had to implement ideas in code on my own, using the manpages when needed.  That ended up feeling like a good balance of tackling the problem myself but giving myself enough room to learn from others.

**Small changes and small challenges**

I found myself in a number of situations where I'd make a change to my code, expect something to happen, but see no change.  This often defied my understanding of what a particular system call was supposed to do, how I expected a process to behave in response to a signal, etc.  In those cases I ended up writing a dead-simple, distilled program to convince myself that I understood a concept.

This was particularly frustrating when I was 1.) in the middle of a change and 2.) confused about, eg, why a call wasn't working as expected.  In the later stages of the project I developed more discipline for doing just one thing at a time -- be it a refactor, introducing a new concept, using a new system call, etc. That helped me keep better focus and not get overwhelmed when multiple things weren't working at once.
