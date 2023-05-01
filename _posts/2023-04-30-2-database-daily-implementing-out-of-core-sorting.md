---
layout: post
title: "Database Daily: Implementing Out-of-Core Sorting"
date: 2023-04-30
category: toy-database
---
**Goal:** Implement pass 1..n, see it working

It's done! I finished the algorim. It demultiplexes 50 files at once.  Pass 1 produced 6 files.  Once the number of files on disk to demultiplex is less than 50, a `DemuxSort` instance was returned.  The work finished in about 15 mintues (which is slow!).  Initially I was overzealous with debug logging which just plain slowed down the process. I iterated over the returned instance and wrote the timestamps to a file in plain text and....it worked!

While I was waiting I condsidered where to go from here. To dwell on performance might be interesting, but the Ruby VM might also get in the way. Of course, I could learn about it along the way but that isn't really the goal of this project. Trying a concurrency approach might be a nice touch, though. Alternatively, I can just use this as a fun exercise and keep moving on other database topics - choosing to use more modestly sized files. I think I'll move on to hashing and just limit the size of the data sources.

I may go on a side quest to write a buffer management layer so that I can have some constraints to work with (eg, only allow 64 4kb buffers at a time). That would also allow me to think more in buffers and pages and to design iterators around those abstractions (whereas now, I'm just using Ruby's buffering and, as far as my Ruby code is concerned, constantly doing small reads from the underlying file). As a bonus, I could incorporate some multithreading where I have a "warm" buffer that's being filled as a main buffer is being drained.

