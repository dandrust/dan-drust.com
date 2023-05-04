---
layout: post
title: "Database Daily: Baby's First Aggregation Executor"
date: 2023-05-03
category: toy-database
---
**Goal**: Create a executor pipeline for a simple query

Good progress, and lots of time tonight. I finsihed the [sorting/hashing lecture](https://archive.org/details/ucberkeley_webcast_FGvKL2cmZEo) and also watched the [single-table queries lecture](https://archive.org/details/ucberkeley_webcast_0iSHVyIlnH0). The coverage of hashing was quick and made me want to review sorting 🙈 The single-table queries lecture was very enlightening and served as a good call-to-action for my time tonight.

The dataset I've been working with has been somewhat limiting to work with in terms of what available to sort, group, etc. - as well as the sheer size. Plus, I'm bored with it.  I used the [Faker gem](https://github.com/faker-ruby/faker) to create a bogus `users` table with some fun data to play with.

Then I wrote an `Aggregator` enumerator class that reads from a source and...counts. So, a very *specific* aggregator! I followed the pattern that I've used so far - a simple class with and `initialize` method and nothing more. 

I refactored the class to a design that I like a bit better. I'm using instace variables and private methods to make the procedure a bit more expressive (and less cluttered with `StopIteration` exception handling 🙌🏻)

So a bit of a divergence from the goal, but still progress! If I can get a simple sort or hash executor written next time I could pull off `SELECT state, count(state) FROM users`. I think I'll start there.