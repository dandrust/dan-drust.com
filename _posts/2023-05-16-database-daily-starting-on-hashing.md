---
layout: post
title: "Database Daily: Starting on Hashing"
date: 2023-05-16
category: toy-database
---

I've taken a few days off since getting sort fully implemented. Hashing has been in the back of my mind and finally today I found quick 20 minutes to review the CS186 [lecture on sorting and hashing](https://archive.org/details/ucberkeley_webcast_FGvKL2cmZEo) to think a bit more carefully about hashing.

The problem is different from sort in a few ways. With sort, I didn't necessarily have to think about how many buffers or files I'd need to use to sort a file - I simply kept filling buffers and writing to files when I ran out of memory. With hashing you need a plan up front - how many partitions will you set up? I'm taking a naive approach and just using the number of buffers available, reserving a single one for streaming the data source from disk. Though in my implementation I'm relying on an iterator to stream this data; my `Hasher` class doesn't really care where it's coming from or what memory's being used to stream it 🤔

In any case, I was able to get a really simple POC stood up tonight in about an hour.  Firstly, I read about how Ruby's `#hash` method works - particularly for integers. There's a random seed that's used so you don't get the integer itself passed through the hashing function. In order to get an input set that I know will predictibly fill the partitions in my specs, I'm looping until `n.hash % partition_count` fills zero through my partition count.

Next, I did a quick-and-dirty implementation of `Hasher` where I get `N` buffers and use a naive modulo-based hash function to partiiton the incoming data. Using the helper described above I know that only one tuple will be written to each buffer.  After the partitioning phase I just loop through the buffers and read them sequentially until they're empty.

This approach works for my simple case, but there are a number of limitations:
* Hash collisions in the coarse-grained partitioning phase never get addressed
* If a buffer get filled up I'm not dealing with any disk IO
* If a partition written to disk exceeds the amount of buffers I have available in memory...fail

I'll keep iterating with slightly more complex cases like I did with sort. Next time I'd like to tackle the first point. I'll generate a set of input that intentionally contains collisions in my coarse-grained partitioning phase, requiring more fine-grained in-memory hashing as the `Hasher` outputs tuples.