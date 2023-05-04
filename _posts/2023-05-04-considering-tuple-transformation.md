---
layout: post
title: "Database Daily: Considering Tuple Transformation"
date: 2023-05-04
category: toy-database
---
**Goal**: Start implementing a complete `Sort` executor 

The magic of this project is that it's completely unstructured and I just follow problems as they come up.  It's iterative; it's fast and loose. 

When considering the algorithm for a full-service `Sort` node I realized something kind of quirky - I want to sort a Ruby array representation of a tuple, but I want to store buffers full of as many bytestring representations of tuples as I can in 4kb.  As written, my `Scanner` class doesn't just stream tuples from disk.  It's *deserializes* tuples and streams the deserialized representation. For an executor class that doesn't need to worry about the possibility of writing a page of tuples back to disk this isn't a problem!  But for executors that must be IO aware -- `Sort` and eventually `Hash` -- there's a 2N cost of re- and de-serializing tuples.

It's not a huge deal, but it's not great either. The ergonomics are rough and as it stands today I lose some tuple metadata during transformation -- the size of the tuple being the missing piece that would make sort a bit easier even if I did accept the cost of re/deserialization.

So this morning was mostly thinking through how this might be designed better and considering some options:
* Consider how tuples are represented in the runtime
  * Arrays are easy, convenient; familiar array access API
  * Keeping tuple size at-hand would be nice
  * What if I wrapped a bytestring in a class that could deserialize a single value on demand?

* Consider how the "fields" data structure works
  * Call this "schema"
  * Maintain field order in the schema
  * Hash keys may be ordered in Ruby, but an arrays shout "I'm ordered!". Consier an array of field definition objects

Finally - should I just bit the bullet and use a systems langauge like Rust? I can move fast in Ruby, but some of the reasons I can move fast also serve as blockers. In the end, the experience has been fine - do a thing, see it working, push the boundary, find an issue, think about it, iterate. I'm not sure that I'm ready to pull the trigger on using a different language. I need to keep the tradeoff between writing a POC and having access to the bare metal in mind. Plus, this has to be fun! If it's not fun, it's game over

I may defer the language question and instead focus next on schema definition. In the end schemas will need to be dynamic - any two nodes have to be able to agree on a schema at runtime to execute a query. So thinking more carefully about schema definition can't hurt.  Though a wrapper around a tuple bystring is very tempting, too!

<iframe width="560" height="315" src="https://www.youtube.com/embed/-Zlg-XuxPW4" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>