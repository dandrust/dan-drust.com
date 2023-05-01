---
layout: post
title: "Database Daily: Demultiplexing Sorted Files"
date: 2023-04-30
category: toy-database
---
**Goal**: Finish implementing a DemuxSort class and see it working

Completing the implementation of `DemuxSort` took longer than anticipated because I forgot to put a looping construct in the enumerator and spun my wheels debugging.  In any case, it's done!

For a manual test I initialized three scanners and passed them to the sorting class, ordering by timestamp.  It worked!  However, I was puzzled when I tried sorting by a different column and direction.  The results were chaotic.  BUT! That's because the scanner sources need to be ordered in the same way that the demux sorter is ordering.  Otherwise the algorithm doesn't make sense.  You can't divide and conquer if your strategy changes between the diving and the conquering!  My puzzled looks near the end of the video can be resolved.

My puzzlement led me to want to put `DemuxSort` under test next time, but I don't think I will. It would be nice to have things under test, but I think that might deflate my motivation. Instead, I'd like to get back to implementing the pass 1..n code.

Of note, the divide and conquer strategy seems useful for a general case -- even if the dataset to be sorted isn't so large to require out-of-core sorting.  That is, it may be useful to still operate on chunks of data (eg, 4kb) and sort those, then put the sorted pages behind a demux sort interface.  In that case, I'd think that the `DemuxSort` implementation I have is nice because it takes a scanner instance and the scanner can worry about the details of what and how it's scanning.  Though, I should be sure that scanners reading from disk are reading 4kb at a time so that we get enough data for each IO.

<iframe width="560" height="315" src="https://www.youtube.com/embed/GR60LrwZ3Ug" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>