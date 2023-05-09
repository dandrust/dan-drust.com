---
layout: post
title: "Database Daily: Working In-Core Sort!"
date: 2023-05-08
category: toy-database
---
**Goal**: Sort class can sort a source that fits into a 4kb page and iterate the sorted results

Finally! I took heavy influence from the `DemuxSort` class I'd written previously to get the sorter working. I largely used TDD, iterating through incrementally more complex scenarios. I found a number of small issues that I probably wouldn't have caught without automated specs. It came down to some silly instance variable issues.  But one of the issues had to do with loops, which are something that I'm happy to keep working on.

At this point I can successfully sort sources that don't exceed 64 * 4kb buffers. Next up is to be able to write two temporary sorted files. Then, I'll move onto the recursive divide and conquer strategy so that arbitrarily large sets can be sorted!