---
layout: post
title: "Database Daily: Set Up For In-Core Sorting"
date: 2023-05-03
category: toy-database
---
**Goal**: Implement an in-core sorter node using an abbreviated data set

Progress was slow. I tried wrting 20 pages from the `movies.db` to a smaller database file but when I read it back I got an error.  It turns out the scanner was reading the header as tuples, so I needed to offset the initial position by header length at the start.  So I fixed that.

Then I noticed when I was scanning the`movies.db` file that the last id was 190, when in the CSV version it was much higher. The id overflowed a short (16 bit) unsigned int, so I need to update the integer type to be 4 bytes wide. In the data type definition, I updated the template string to be `L` (long) but didn't update the byte size to be 4. So that led to a little debugging side quest!

In the end, I was able to rebuild the `movies.db` file to use long integers. I read back the table and saw it working nicely.  I also took the first 20 pages of that file and put it into an abbreviated movies database file, `movies_small.db` (the original goal!). 

Next, my plan is to implement a `Sorter` node that basically divides and conquers, but ONLY in-memory (so, constrained to 64 4k pages at a time, in the best case!). The `movies_small.db` will be my sorting target. After that I can extend it if I want to use an out-of-core strategy if needed. Then, finally (hopefully?) I can let this go and move on to hashing!