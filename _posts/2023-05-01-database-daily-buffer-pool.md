---
layout: post
title: "Database Daily: Buffer Pool"
date: 2023-05-01
category: toy-database
---
**Goal:** Sketch out a buffer pool concept, thinking about file management

Tonight I turned my attention to putting some limits around what how much memory the database application will be able to handle at a time. These are sort of fake constraints to give me a tighter leash - prompting me, I hope, to consider things like memory management and performance more carefully.

I wrote a `BufferPool` singleton that holds up to 64 4kb buffers.  A `Buffer` is simply a wrapper around Ruby's `StringIO`.  Any class that needs to request data from persistent storage will do so via the `BufferPool` interface, which will return a `Buffer` instance.  Eventually, clients will be able to fill a buffer, modify it, and ask the `BufferPool` to save it back to persistent storage.  But for now, it can read a single 4kb page from a given file!

In keeping with being a good steward of resources, the `BufferPool` will track which pages are under it's managent at a given time.  So two consecutive calls to `BufferPool.get_page(relation, 1)` will return the same buffer (representing the 1-index page from the relation's underlying storage file).  

This increment means that `Relation` loses some responsibility, conceptually. Once it's all said and done `Relation` should be a light class at this point - storing a file path and information about the relations schema.

Next time, I'd like to implement a `Scan` node that is buffer-aware. That is, it should request a page from the `BufferPool`, iterate over it, return it, request the next page, keep iterating, etc., until the entire relation is scanned.  This will serve to 1.) stand up a reference implementation for a buffer-aware iterator and 2.) think about the `Buffer` lifecycle.

<iframe width="560" height="315" src="https://www.youtube.com/embed/9jOS15BY7vA" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>