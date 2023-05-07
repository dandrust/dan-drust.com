---
layout: post
title: "Database Daily: Inching Toward `Sort`"
date: 2023-05-04
category: toy-database
---
**Goal**: Get a little closer...

Slow progress, but progress. I had been putting 2-3 hours/day into this last week so I took Friday off.  Today I'm getting to work late so I'm not going too far into it.

I was struggling to wrap my head around where the loops would be for the sorter's initialization.  I started from the simplest case (tuple by tuple) and went from there (we fill many buffers, then perhaps we fill many temp files). There will be an interaction with detecting how much memory is avaiable that I don't quite have a grasp on yet. I've been thinking of that part in the context of a concurrent application, but I think I need to just consider a single thread for the time being. Concurrency is a blind spot for me. Eventually it'll be nice to consider it more deeply - hopefully in this project.  Progress, not perfection.

In any case, I put together a spec file for this class because I don't want to dump lots of time manually testing, as I'm sure it'll take me a bit to nail the implementation. Thinking through what to test has also helped me consider how to iterate from a simple case to more complex ones.

Next time I'd like to get to the point where the sorter node can sort a source whose contents fit within a single 4k buffer.