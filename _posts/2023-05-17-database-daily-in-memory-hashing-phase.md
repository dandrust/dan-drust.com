---
layout: post
title: "Database Daily: In-memory Hashing Phase"
date: 2023-05-16
category: toy-database
---

My POC from yesterday worked because my specs were set up to have only one distinct value per hash bucket during the coarse-grained first phase.  Today I wrote a helper function to provide a set of input that included more than one distinct value per bucket. The specs failed - the hash class returned somethin like `[1, 2, 2, 1, 4, 4, 3, 3, 6, 5, 6, 5]`.  You could see where the bucket boundaries are but the values within the buckets are interleaved.  For the fine-grained hashing phase I simply used a ruby hash to put everything in it's distinct bucket.  

I'm cheating a bit here, in a couple of ways.  First, I haven't implemented a hash table from scratch.  To be fair, I didn't implement a sorting algorithm from scratch either so I'm not too upset about it.  The point is to think about storage and retrieval.  Second, I'm not quite sure how to think about memory constraints with hashing.  The in-memory hash table can grow infinitely large and Ruby will take care of the details.  The memory constraints that I imposed on myself with sorting via the `BufferPool`'s quantity of available buffers doesn't work well here.  With sorting I'd be sure to fill an array to a full page's worth of tuples before sorting them and flushing them to disk. With hashing, the in-memory structure that Ruby's helping me out with isn't a staging area for data bound for disk -- it's instead being read from disk! 

I may just let that be for now.  It's in the back of my mind and perhaps a direction will become clear as I continue working through hashing.

Overall, this is going pretty quick.  Probably thanks to having sorted out (ha!) my mental model during sorting.

Next I'd like to update the `Hasher` to be able to spill to disk if a partition bucket grows larger than a buffer.