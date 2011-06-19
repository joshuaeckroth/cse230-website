---
title: Homework 7
layout: default
---

Make a class called `Room` and a `main()` function that allow a user to "walk
through a maze." Here is an example interaction:

<pre>
You are in Entrance
A wide open entrance...
South: Hallway

Enter n/s/e/w or q to quit: s
You are in Hallway
A long hallway...
North: Entrance
East: Ballroom

Enter n/s/e/w or q to quit: e
You are in Ballroom
A huge ballroom...
West: Hallway

Enter n/s/e/w or q to quit: e

No room that direction.

You are in Ballroom
A huge ballroom...
West: Hallway

Enter n/s/e/w or q to quit: q
</pre>

The `Room` class should have at least these methods:

* a constructor that sets the `name` and `description`
* `string getName()`
* `string getDescription()`
* `void link(Room *r, char direction)`
* `Room *getLinked(char direction)`
* `void printLinked()`

You should support room links in North, South, East, and West directions. There
is no particular need for polymorphism in this program's code, but you should
use `private` and `public` data/methods in your class (make a meaningful
separation).

**If you choose to work in a group of 2 or 3 people:** First, you must tell me
who worked on what part of the code; you will all get the same grade, but
please be honest about who did what. Second, you must write a more complicated,
more interesting program than the one described above. What you add is sort of
up to you, but here are some ideas: support more than N/S/E/W links; produce a
random maze (somewhat random room names and descriptions, too); allow two or
more players to walk through the maze simultaneously; allow the player to learn
what else is in the room (so the rooms will have "occupants"); etc. I expect
the extra work a group will produce to be something like adding two or three of
those features just described.

This may be interesting if you have some free time:
[http://www.kongregate.com/games/2DArray/you-find-yourself-in-a-room](http://www.kongregate.com/games/2DArray/you-find-yourself-in-a-room)

