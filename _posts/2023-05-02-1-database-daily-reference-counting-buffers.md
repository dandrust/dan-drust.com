---
layout: post
title: "Database Daily: Reference Counting Buffers"
date: 2023-05-02
category: toy-database
---
**Goal**: Implement reference counting for buffers in the buffer pool so that I can Uuse the scanner to iterate through a large database file with an out of memory error

Reference counting is in place! Clients can use `BufferPool.get_page(relation, page_no)` to get a buffer with the page's contents. When finished, clients can `.return_page(buffer)` to the pool. If two clients request the same page it will be re-used and usage counted accordingly. If all buffers are full when a new page is requested we'll evict a page that's not being used (ie, `refcount: 0`) and bring in the new page. If all buffers are full and in-use, an exception is raised.

To test, I scanned the `ratings.db` file. Before reference counting and evicting buffers I was only able to make it through ~17.5k records (that's 64 4kb pages worth!) After adding reference counting I got to ~800k records before I killed it. Inspecting the sorted text output from the other day I see that table has 17M records! 

I was looking at my entry from a few days ago when I wrapped up out-of-core sorting. At that point I was ready to swear off sorting to move onto other things, but now that I have a nice `Buffer` abstraction I think it might be fun to revisit the sorting exercise (though probably with a smaller dataset). With a fixed buffer pool page size, my `Sort` node would know if it should try sorting the dataset in-memory or out-of-core.

I may start there next time.  After that, I think I should return to the lecture videos and work through hashing.  Hopefully with having spent so much time on foundational work during sorting, hashing will go quicker.