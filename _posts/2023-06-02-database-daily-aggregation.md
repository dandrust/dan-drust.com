---
layout: post
title: "Database Daily: Aggregation"
date: 2023-06-02
category: toy-database
---

It's been a while!  I dropped the ball on some posts after the last 5/18 log. During those sessions I finished up hashing, started working on some single-table queries, and worked on some organization.

I'd also started thinking about how to determine schemas between nodes and how to deal with schemas changing throughout the pipeline.  That led me to the `Aggregate` node, which I'd hard-coded for sort and hadn't come back to. 

Tonight I revisted the `Aggregate` executor and generalized it to work for a sampling of aggregation functions (min, max, sum, average, array aggregation, boolean and, boolean or). Average still needs some work, so I'll start there next time.

Beyond that, I wrote a small iteration roadmap for the `Aggregate` class:
* Support something like `count(*) ... group by foo` where there are multiple groups within the dataset
* Support the ability for the schema to change (right now the aggregate output is a single aggregate value per group, period)
* Figure out how `count(distinct foo)` would work