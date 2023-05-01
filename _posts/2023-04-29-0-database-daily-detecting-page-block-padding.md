---
layout: post
title: "Database Daily: Detecting Page Block Padding"
date: 2023-04-29
category: toy-database
---
**Goal**: Modify relation scan iterator code to deal with data gaps at page boundaries

A bit of a winding road this morning.  Initially I thought that putting a 2 byte integer representing tuple size would work nicely. If we read a zero size, then the reader can just keep reading until it reads on non-zero size.

The issue with that approach was that the padding at the end of a 4kb page could be and odd number of bytes - which meant that reading a 16 bit int would pick up "junk" data past the 4kb page boundary, suggesting that there was a valid tuple size being read when there actually wasn't!

My simple solution was to include a tuple header whose first byte is non-zero, and then read only a single byte to determine presence.  If a null byte is read, it's essentially a "null tuple" header.

```text
   +- tuple header
   |
|-----|
 1 2 3 4 5 6 7 8 ... n
+-+---+----------------+
| |   | tuple contents |
+-+---+----------------+
 |  |
 |  +- tuple size
 |
 +- tuple presence
```

This means that records no longer span the 4kb boundary! Next time, I can look into reading a 4kb page at a time. That gets me back on track to work on the out-of-core merge sort algorithm -- being able to read some fixed number of pages at a time, sort those, and write the sorted set back to disk.

<iframe width="560" height="315" src="https://www.youtube.com/embed/LZDMkg1cylo" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>