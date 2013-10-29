---
title: Homework 7
layout: default
---

  - Creating classes and using object-oriented program design
    ([lecture notes (part 1)](/lecture/classes-and-object-orientation.html)
    and
    [lecture notes (part 2)](/lecture/classes-and-object-orientation-2.html))

  - Use of maps to store pointers
    ([lecture notes](/lecture/maps-sets-etc.html))

  - Splitting code into several files
    ([lecture notes](/lecture/splitting-code.html))

Make a class called `Room` and a `main()` function that allow a user to "walk
through a maze." Here is an example interaction:

<pre>
You are in the Garden. The trees and shrubs appear to be resilient to
decades of neglect but the flowerbeds all withered away long ago.

There is an exit to North (Kitchen) and East (Library).

Which exit? (or 'quit'): north

You move to the North...

You are in the Kitchen. Knives, pots, pans, and other kitchenware
dangle over the island like some kind of metallic chandelier.

There is an exit to North (Hallway), East (Library), and South
(Garden).

Which exit? (or 'quit'): west

There is nothing over there.

Which exit? (or 'quit'): east

You move to the East...
</pre>

The `Room` class should have at least these methods:

* a constructor that sets the `name` and `description`
* `string getName()`
* `string getDescription()`
* `void link(Room *r, string direction)` -- establish a link between rooms
* `Room *getLinked(string direction)` -- get the room linked in some direction
* `void printLinked()` -- print all rooms linked to this room

You should support room links in North, South, East, and West
directions (at least). Use a map (key is the direction, a string;
value is the `Room` pointer).

You can establish the maze in the code (in the `main()` function
probably); the maze need not be designed by the user interactively
(that would be slightly harder).

There is no particular need for polymorphism in this program's code,
but you should use `private` and `public` data/methods in your class
(make a meaningful separation). Also, split your code into three
files: `room.h`, `room.cpp`, and `main.cpp`.

This may be interesting if you have some free time:
[http://www.kongregate.com/games/2DArray/you-find-yourself-in-a-room](http://www.kongregate.com/games/2DArray/you-find-yourself-in-a-room)

You can also play the original (updated to be played in a browser)
[Colossal Cave Adventure](http://www.ifiction.org/games/playz.php?cat=&game=1&mode=html)
