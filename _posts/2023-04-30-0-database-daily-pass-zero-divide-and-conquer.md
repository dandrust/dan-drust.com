---
layout: post
title: "Database Daily: Pass 0 - Divide and Conquer"
date: 2023-04-30
category: toy-database
---
**Goal**: Sort the ratings.db file by creation timestamp without reading the entire file into memory.

Today was the first step in implementing out-of-core sorting for large data sets. Following the algorithm described in Joe Hellerstien's CS186 [lecture on sorting and hashing](https://archive.org/details/ucberkeley_webcast_FGvKL2cmZEo), I was able to divide the 300MB `ratings.db` file into 280 sorted temporary files. 

These temporary files differ from the "normal" database files I've been working with in that they don't have headers. To make reading them back a bit easier, I implemented a `Relation.from_headless_db_file` method.

Finally, I began writing the script to merge 2+ sorted temporary files.  My approach will be to use a queue of queues and a demultiplexing sort enumerator that puts an iterator interface in front of 2+ scan iterators.

Tomorrow, I'll continue with the pass 1...n code and hopefully be able to write a sorted 300MB database file to disk!

<iframe width="560" height="315" src="https://www.youtube.com/embed/a4BKGfnzorU" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>