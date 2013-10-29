---
title: Homework 8
layout: default
---

Skills needed to complete this assignment:

  - Creating classes and using object-oriented program design
    ([lecture notes (part 1)](/lecture/classes-and-object-orientation.html)
    and
    [lecture notes (part 2)](/lecture/classes-and-object-orientation-2.html))

  - Using polymorphism ([lecture notes](/lecture/polymorphism.html))

  - Use of maps and sets to store pointers
    ([lecture notes](/lecture/maps-sets-etc.html))

  - Splitting code into several files
    ([lecture notes](/lecture/splitting-code.html))
    
This assignment extends Homework 7. Ideally, you will start with your
HW 7 code and extend it, so that you have the experience of adapting
existing code (it's more challenging than starting over). I won't be
checking if you did start with your prior code or did not, however.

Add an `Agent` class that, at least, has a pure virtual function `bool
act()` that must be implemented in subclasses. Make at least one
subclass for monsters (e.g. a `Grue` class), and another subclass for
the player (a subclass named `Player`). Update your `main()` function
or make a `Game` class so that, during each "turn," the monsters move
and the player(s) move (in whatever order).  Moves are accomplished by
using the monsters' or player(s)' `act()` functions.  The monsters'
`act()` functions will choose a random room (or whatever you want to
do) and will move into that room. The player(s)' `act()` function will
ask the person at the keyboard to choose a room. The `act()` function
will always return `true` for monsters but may return `false` for
players if the player types "quit" or similar. Wherever you are using
these `act()` functions, you'll need to check for true/false and quit
the game if `act()` returns false. For example:

{% highlight cpp %}
Monster* monster1 = new Monster(...);
Player* player1 = new Player(...);
Player* player2 = new Player(...);
vector<Agent*> agents;
agents.push_back(monster1);
agents.push_back(player1);
agents.push_back(player2);

while(true)
{
    for(int i = 0; i < agents.size(); i++)
    {
        bool ok = agents[i]->act();
        if(!ok)
        {
            cout << "Game quits." << endl;
            return 0;
        }
    }
}
{% endhighlight %}

Here is what your `Agent` class header (`agent.h`) might contain (at
least):

{% highlight cpp %}
#ifndef AGENT_H
#define AGENT_H

#include "room.h"

class Agent
{
protected:
    Room *cur_room;
    string name;

public:
    virtual bool act() = 0;
    string getName() { return name; }
};

#endif
{% endhighlight %}

Here is what your monster (`Grue`) class header (`grue.h`) might contain
(at least):

{% highlight cpp %}
#ifndef GRUE_H
#define GRUE_H

#include <string>
#include "agent.h"
#include "room.h"
using namespace std;

class Grue : public Agent
{
public:
    Grue(string _name, Room *starting_room);
    bool act();
};

#endif
{% endhighlight %}

Here is a quick glance at how you might program a "random" monster
that chooses a random direction to move on each turn:

{% highlight cpp %}
int n = rand() % 4;
string direction;
switch(n)
{
    case 0: direction = "north"; break;
    case 1: direction = "south"; break;
    case 2: direction = "west"; break;
    case 3: direction = "east"; break;
}
if(cur_room->getLinked(direction) != NULL)
{
    cur_room = cur_room->getLinked(direction);
}
{% endhighlight %}

If you use `rand()` anywhere in your code, be sure to include this at
the beginning of `main()`:

{% highlight cpp %}
// ensure different random numbers each time the program is run
srand(time(NULL));
{% endhighlight %}

You are required to split your code into several files. For each
class, make a `.cpp` and a `.h` file. Also write a `main.cpp` file
that has the `main()` function in it. Check out the lecture notes for
examples.

When you submit your code (via email), send me all of the `.cpp` and
`.h` files.

Your `Room` class must be updated as well to tell the player who is in
the room. That is, the `Room` class should have a set or some other
collection that holds `Agent` pointers; when an agent enters the room,
the agent's pointer is added to the collection; when an agent leaves
the room, the agent's pointer is removed from the collection. The idea
is, if the player does not know who is in the same room as the player,
then how will you, as the programmer, know if your monsters are
appropriately moving to different rooms?

You might accomplish this in the following way: add a set to the
`Room` class (i.e. add `#include <set>` in `room.h` and add a private
variable `set<Agent*> occupants` to the `Room` class). This set
contains `Agent` pointers. This way, you can store monsters and
players in the set since both can be treated as `Agent` pointers. Then
provide methods in the `Room` class called `enter`, `leave`, and
`printOccupants` that, respectively, add an `Agent` to the room,
remove an `Agent` from the room, and print all the agents in the room.

Here is what those functions might look like:

{% highlight cpp %}
void Room::enter(Agent *a)
{
    occupants.insert(a);
}

void Room::leave(Agent *a)
{
    occupants.erase(a);
}

void Room::printOccupants()
{
    cout << "Occupants in this room:" << endl;

    set<Agent*>::iterator it;
    for(it = occupants.begin(); it != occupants.end(); ++it)
    {
        // use the Agent's getName() function
        cout << (*it)->getName() << endl;
    }
}
{% endhighlight %}

To use these `enter` and `leave` functions, when an agent (monster or
player) is changing rooms, before changing rooms, "leave" the current
room (i.e.  `cur_room->leave(this)` -- recall that `this` is a pointer
variable that always exists in class methods, and points to the
monster or player who is changing rooms), then change rooms, then
enter the new room (i.e.  `cur_room->enter(this)` after `cur_room` has
been changed).

Note that there will be a small problem with the modified `Room`
class. Your new `Room` class will need a set of agents, like so:

{% highlight cpp %}
class Room
{
private:
    set<Agent*> occupants;

    // ...
};
{% endhighlight %}

You might expect to add `#include "agent.h"` to the top of
`room.h`. However, this is not going to work, because `agent.h`
already has `#include "room.h"`. The two `.h` files can't both
"include" each other -- cyclical includes are not allowed.

So the solution to this is to keep `#include "room.h"` inside
`agent.h` but don't put `#include "agent.h"` inside `room.h`. Of
course, without that "include," the `Room` class won't know what an
`Agent` is. We solve this by adding a *forward declaration* of the
`Agent` class, like so:

{% highlight cpp %}
class Agent;  // forward declaration

class Room
{
private:
    set<Agent*> occupants;

    // ...
};
{% endhighlight %}

What this says is "I promise there will be a class called `Agent`,
eventually, but not yet." As long as we stick with pointers to
`Agent`, this will work.

In the `room.cpp` and `agent.cpp` files, you can "include" whatever
you want (so `#include "agent.h"` and `#include "room.h"` in the
respective `.cpp` files); there is no cyclical dependency in this case
because `.cpp` files don't "include" each other, and are compiled
separately.

Note that you could have alternatively used a forward declaration of
`Room` instead of `Agent`; it would work either way.

## Example interaction

In this example, we have two players (Josh and Tracy). Notice that the
player is notified when somebody else is in the room (either a player
or a monster), and players switch turns.

<pre>
Welcome!

Tracy, it is your turn. You are in the Garden. The trees and shrubs
appear to be resilient to decades of neglect but the flowerbeds all
withered away long ago.

You see no other creatures here.

There is an exit to North (Kitchen) and East (Library).

Tracy, which exit? (or 'quit'): south

...No such exit or that room is full!


Josh, it is your turn. You are in the Lobby. This is a dark and dusty
room, and there are cobwebs in every corner and spanning the
furniture.

You see a monster named Kafka (a thin, nervous looking creature).

There is an exit to the South (Hallway).

Josh, which exit? (or 'quit'): south

You move to the South...


Tracy, it is your turn. You are in the Garden. The trees and shrubs
appear to be resilient to decades of neglect but the flowerbeds all
withered away long ago.

You see a monster named Napolean (once Emperor of the French).

There is an exit to North (Kitchen) and East (Library).

Tracy, which exit? (or 'quit'): north

You move to the North...


Josh, it is your turn. You are in the Hallway. Many dark portraits of
all sorts of frightening creatures blanket the walls.

You see no other creatures here.

There is an exit to the South (Kitchen).

Josh, which exit? (or 'quit'): south

You move to the South...


Tracy, it is your turn. You are in the Kitchen. Knives, pots, pans,
and other kitchenware dangle over the island like some kind of
metallic chandelier.

You see a monster named Napolean (once Emperor of the French), a
monster named Kafka (a thin, nervous looking creature), and Josh (your
husband).

There is an exit to North (Hallway), East (Library), and South
(Garden).

Tracy, which exit? (or 'quit'): east

You move to the East...
</pre>
