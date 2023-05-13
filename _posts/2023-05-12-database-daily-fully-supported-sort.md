---
layout: post
title: "Database Daily: Fully Supported Sort!"
date: 2023-05-12
category: toy-database
---
Sadly, I've done three sessions without posting notes. So some detail of the journey will be lost. But the good news is that tonight I finished out of core sorting, bringing my `Sort` class to the point where it can sort arbitrarily large streams of incoming data 🎉

As a sort of retrospective, this has been fun. The sort problem that was discussed in the CS186 [lecture on sorting and hashing](https://archive.org/details/ucberkeley_webcast_FGvKL2cmZEo) was the main motivation for tackling working through this project so it feels good to have finally reached a repeatable, self contained sort routine. If I were in undergrad working through this there's no way I could've gone so deep, so I'm thankful that I can watch the lectures and work through the content at my own pace.

Daily consistency has been key. I've missed a few days here and there but I've made up for it by doubling (or tripling) my daily hour allotment other days. Most days I try to go over an hour, though an hour has seemed to be a good limit to keep motivation high, not burn out, and not get too frustrated at a problem before walking away. I'll try to maintain a daily cadence with at least an hour of work per session.

Lately, test driven development has been quite helpful. Manual test setup was getting a bit out of control as I try to break the code into classes and modules that make sense so having repeatable tests has saved a lot of time. Writing tests has also helped me build incrementally and start with very small, trivial problems and iterate towards larger and more complex ones. And sometimes the "larger and more complex" issues ended up being only a few lines of code -- a much different magnitude than the problem represented in my head. In the end, it's been a good learning experience in breaking large problems down into smaller ones.

Documenting the journey has helped me reload and keep context in my brain. I often will reference the previous day's blog entry and review the code, talking through it as I go. I try to record myself on Loom most days (even if I don't post the videos) as if I were leading a code-along activity.  On more than one occasion I've found errors and issues because I was explaining an algorithm to my "audience" and realized what was wrong. It feels silly, but it's worked as a rubber ducky placebo.

Finally, I can't understate the value of side quests in a project like this. It's really about following the motivation! Working at sort hasn't burned me out because along the way I've worked on binary encodings, writing a file format, implementing a buffer cache, etc, etc, etc. I've tried to keep myself focused on what will drive me towards an overall goal, but I've tried to balance that with not shying away from a small challenge here or there.

Next time I may revist my goal of stringing together enough nodes to emulate a simple single-table query plan. Either that or start tacklin a `Hash` executor node.

