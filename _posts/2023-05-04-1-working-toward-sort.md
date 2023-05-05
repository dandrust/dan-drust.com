---
layout: post
title: "Database Daily: Working Toward `Sort`"
date: 2023-05-04
category: toy-database
---
**Goal**: Work on an in-memory representation of tuples that keeps the serialized bytestring (and it's length!) close by

Goal achieved...and then some!  I've spent about 3 hours tonight.  First I implemented a tuple class that wraps the bytestring, keep the length at hand, and has array semantics so that access like `tuple[0]` still works. I got into touble trying to lazily deserialize values at random from the tuple - I'm not keeping an offset or field size for variable-length fields so you really have to just iterate through te tuple to get anything.  Oh, well.

Then I returned to sort. I added `BufferPool.get_empty_page` and did some other minor update so the buffer pool class (mostly undoing some assumptions I'd made when I wrote it). That let me grab empty buffers into which I could write tuples that had been sorted in-memory.  I ran the `Sort` executor against `users.db` (which is ~30 pages long) until I got an out of memory error. I don't entirely understand how I got such an error if the file's size is less than 64 * 4kb.  My brain is spent so I'll look into it next time. Aside from that, I started to write code to "spill" the sorted pages to disk. I have a collection of pages that are sorted and I want the whole file to be sorted.  So I'll have to use the `DemuxSort` (or similar).  I need to think about how that'll work with available memory -- do I need to keep some free on the side?

Next time, with a fresh head, I'll keep working on this `Sort` executor!