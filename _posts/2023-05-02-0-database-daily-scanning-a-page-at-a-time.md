---
layout: post
title: "Database Daily: Scanning a Page at a Time"
date: 2023-05-02
category: toy-database
---
**Goal**: Update the scanner class to be page-aware, using `BufferPool` to fetch data from disk during iteration

I made good progress this morning updating the `Scanner` class to rely on the `BufferPool` to do disk reads on page at a time. The scanner asks for a page, iterates through it, returns it, and then requests another.

This led to some light refactoring in the scanner class itself to recover from null reads more gracefully. Additionally, I mocked out a reference counting strategy that the `BufferPool` can use to evict buffers that are no longer in use.

I left myself a number of TODO's in `buffer_pool.rb` - next time I'll work through those so that we'll have a (naive) eviction strategy in place and we can see a scanner over `ratings.db` iterate through all records with the `BufferPool` running out of memory.

<iframe width="560" height="315" src="https://www.youtube.com/embed/o-6O_tJ44kg" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>