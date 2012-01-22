---
title: Homework 8
layout: default
---

Due March 5, 11pm (Mon).

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
char direction;
switch(n)
{
    case 0: direction = 'n'; break;
    case 1: direction = 's'; break;
    case 2: direction = 'w'; break;
    case 3: direction = 'e'; break;
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

In this example, we have two player (Josh and Tracy), and rooms have a
size limit (the Hallway cannot fit two people). You do not need to
implement room size limits. Also notice that the player is notified
when somebody else is in the room (either a player or a monster), and
players switch turns.

<pre>
Welcome!


--Josh--Entrance------
A wide open entrance...

You see here:
 Tracy (A princess)

Exits:
 south: Hallway

Josh, which exit? (or 'quit'): south



--Tracy--Entrance------
A wide open entrance...

You see here:

Exits:
 south: Hallway

Tracy, which exit? (or 'quit'): south

!!! No such exit or that room is full. !!!


--Josh--Hallway------
A long hallway...

You see here:

Exits:
 east: Ballroom
 north: Entrance

Josh, which exit? (or 'quit'): east



--Tracy--Entrance------
A wide open entrance...

You see here:

Exits:
 south: Hallway

Tracy, which exit? (or 'quit'): south



--Josh--Ballroom------
A huge ballroom...

You see here:
 A grue named Kafka (A nasty, horrible monster)

Exits:
 west: Hallway

Josh, which exit? (or 'quit'): west

!!! No such exit or that room is full. !!!


--Tracy--Hallway------
A long hallway...

You see here:
 A grue named Napolean (A nasty, horrible monster)

Exits:
 east: Ballroom
 north: Entrance

Tracy, which exit? (or 'quit'): east



--Josh--Ballroom------
A huge ballroom...

You see here:
 Tracy (A princess)

Exits:
 west: Hallway

Josh, which exit? (or 'quit'):  
</pre>
