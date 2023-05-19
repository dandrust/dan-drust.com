---
layout: post
title: "Database Daily: Setting Up for Spilling"
date: 2023-05-18
category: toy-database
---

Tonight's work was relatively low key.  I set up a spec that I expected to fail where I would pass enough values to the `Hasher` that it should have to spill tuples to disk.  I expected it to fail because, well, I hadn't written any spilling code quite yet.  However, it passed!

That was due to the fact that the size of a buffer isn't enforced.  When I worked on sort I wrote it so that the caller was responsibile for not writing more that 4kb to a buffer before flushing it to disk. The buffer itself didn't limit ti's content.  So my spec passed just fine - increasing the size of the buffer past 4kb!

To get it to fail (and to give myself constraints!) I overrode the `StringIO.write` method in `Buffer` to not write past 4kb. The interface is elegant (I think) - instead of raising an error it fails somewhat silently but it will return the number of bytes written - which will be 0 if you try to write past 4kb.

That got specs failing. I then extracted the exisiting code that added content to a buffer into it's own method that can deal with fussing with full buffers.  I left some commented out code and `TODO`s that I will pick up next time -- bascially writing the spill files and accounting for them.  Readback should be for free since `Scan` 