---
layout: post
title: "Database Daily: Sorting a Single Page"
date: 2023-04-29
category: toy-database
---
**Goal**: Read a page from a database file, sort it, and write the sorted result back to disk.

Round 2 for today! A bonus round! 

I re-built the ratings database file to use the updated format that I worked out this morning.  From there, I played around with `IO.pread` (*positional* read) to grab a 4kb chunk at a 4096 byte offset. Worked like a charm!

From there, I started extracting the table-scanning iterator into it's own executor class.  Since you can wrap a string with the `StringIO` class, the `Scanner` class can take a `File` or a `StringIO`.

Finally, `sort_chunk.rb` is a proof-of-concept for reading a chunk of a database file, sorting it in-memory, and then writing the result back to disk.

Next up is to write an out-of-core sorting algorithm to sort the entire ratings database file!