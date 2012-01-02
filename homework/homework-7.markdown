---
title: Homework 7
layout: default
---

Due Feb 24, 11pm (Fri).

Make a class called `Room` and a `main()` function that allow a user to "walk
through a maze." Here is an example interaction:

<pre>
You are in Entrance
This is a wide open entrance...
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
* `void link(Room *r, char direction)` -- establish a link between rooms
* `Room *getLinked(char direction)` -- get the room linked in some direction
* `void printLinked()` -- print all rooms linked to this room

You should support room links in North, South, East, and West directions (at
least). You can establish the maze in the code (in the `main()` function
probably); the maze need not be designed by the user interactively (that would
be slightly harder).

There is no particular need for polymorphism in this program's code, but you
should use `private` and `public` data/methods in your class (make a meaningful
separation). Also, split your code into three files: `room.h`, `room.cpp`, and
`main.cpp`.

This may be interesting if you have some free time:
[http://www.kongregate.com/games/2DArray/you-find-yourself-in-a-room](http://www.kongregate.com/games/2DArray/you-find-yourself-in-a-room)

