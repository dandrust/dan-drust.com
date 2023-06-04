---
layout: post
title: "Database Daily: Group By Aggregation"
date: 2023-06-02
category: toy-database
---

After an embarassing foray into math, I implemented the ability to do something like `SELECT COUNT(id) GROUP BY age`.  This added awareness of when a grouped set starts/ends while the `Aggregate` node is reading it's input stream.

No ah-ha moments yet around things like `COUNT(DISTINCT age)`.  Though at a few points I thought about how output schema (and aggregates inside of it) might be a separate concern from the grouping/aggregating dimensions.  I'd like to pull that thread next time so that I could implement something like `SELECT array_agg(name), count(id), age FROM users group by age` -- where the output tuples follow the `array_agg(name), count(id), age` schema.

<iframe width="560" height="315" src="https://www.youtube.com/embed/jD1iFxhoh60" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>