---
layout: post
title: "Database Daily: Spilling Into Temp Files"
date: 2023-05-10
category: toy-database
---
**Goal**: Sort class can spill sorted tuples into temporary files when out of memory

Good progress and a really frustrating bug! Writing a temp file with sorted tuples was done within ~30 minutes, no problem. Then, a gnarly bug.

The bug presented as an in-memory buffer being empty (even though it should be full). Some debugging output showed that the first buffer in the sorted array -- presumably, the one that I'd "reused" after the last temp file spill -- had double the size I'd expected. 

I spent quite a bit of time trying to understand why double the tuples were getting committed to this buffer. I suspect that it had something to do with not having properly truncated the buffer. Though, I'd been sure to clear the buffer and I also added a line in the buffer pool to clear any returned buffers when they come back. I didn't end up finding the cause of the issue.

When I changed the code that resets the state after spilling to a temp file to return ALL buffers and then request a net-new one (which, under the hood is just recycling returned buffers 🫠) the issue was resolved. I don't understand why that worked!

I think that it would be prudent for the sorter to keep one of the buffers around so that it doesn't risk being starved by other processes.  Though, that concern is far down the line. Perhpas I should let that go and just keep moving.

Next time I will work on getting the sort to be aware of spill files when it emits sorted tuples. Right now it's just reading what's in memory, which ends up being a subset of the source if there are files on disk.

***PostScript:***
I found the issue! Truncating a StringIO doesn't reset it's position to the begining of the IO:

```ruby
require 'stringio'

sio = StringIO.new("hello world")
 #=> #<StringIO:0x00007fbfd724b118> 
sio.rewind
 #=> 0 
sio.read
 #=> "hello world" 
sio.pos
 #=> 11 
sio.truncate(0)
 #=> 0 
sio.pos
 #=> 11    
sio.write("I'd expect this to be written at position 0")
 #=> 43 
sio.rewind
 #=> 0 
sio.read
 #=> "\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000I'd expect this to be written at position 0" 
```

In order to reuse a buffer I needed to 1.) truncate its data and 2.) rewind the position counter to zero. After writing data to a file and truncating the buffer, data was being writted at position 4096, with 4096 null bytes padded in front. When the scanner read that back during the next spill to disk it was understood to be a null set -- since null bytes indicate that we've reached some padding at the *end* of a page. That led to the scan enumerator thinking it was empty and raising the `StopIteration` exception unexpectedly.

I've added a `TODO` to write a `Buffer#clear` method to make the buffer API a bit more convenient than what we get from `StringIO`.

Tests now successfully write spill files but still fail since only sorted tuples in-memory when the source reaches it's end are being output.  That's where we'll start next time.

<iframe width="560" height="315" src="https://www.youtube.com/embed/t84vlM6l0FI" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>