---
layout: post
title: "Database Daily: Page Block Boundaries"
date: 2023-04-28
category: toy-database
---
I've come back to this after a week or so away. I'd gotten a little bogged down with wanting to know everything before doing anything.  After taking stock, I'd really like to use the Berkely lectures as a guide so I'll stick to that content.  

Previously I went on the side quest with making a binary-encoded format so that I could reliably grab a chunk of data without worrying that I'd grab half a record at the end of the chunk. In doing that I realized that fixed-size records worked great when there were integers, floats, and timestamps involved. But strings were a bit trickier!  So I expanded my file format but in doing so I wasn't careful about boundaries, so I was back at square one where if I grab a predetermined chunk of data I might have a tuple that spans the boudnary.

So today I revisited the way that I'm inserting tuples so that I don't write a record across a 4096 byte boundary (`0x1000`). However, that breaks code that reads back the records! 

For next time: How will I have the code reading a file know that it's reached the last tuple on a 4kb block? As long as the first long it reads is zero, will that be enough to signal that it's time to grab a new block?