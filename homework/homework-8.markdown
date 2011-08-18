---
title: Homework 8
layout: commentable
---

Due Aug 22, 11pm.

This assignment extends Homework 7. Ideally, you will start with your HW 7 code
and extend it, so that you have the experience of adapting existing code (it's
more challenging than starting over). I won't be checking if you did start with
your prior code or did not, however.

Add an Agent class that, at least, has a pure virtual function `bool act()`
that must be implemented in subclasses. Make at least one subclass for monsters
(e.g. a Grue class), and another subclass for the player (a subclass named
Player). Update your `main()` function or make a Game class so that, during
each "turn," the monsters move and the player(s) move (in whatever order).
Moves are accomplished by using the monsters' or player(s)' `act()` functions.
The monsters' `act()` functions will choose a random room (or whatever you want
to do) and will move into that room. The player(s)' `act()` function will ask
the person at the keyboard to choose a room. The `act()` function will always
return `true` for monsters but may return `false` for players if the player
types "quit" or similar. Wherever you are using these `act()` functions, you'll
need to check for true/false and quit the game if `act()` returns false. For
example:

{% highlight cpp %}
Monster *monster1 = new Monster(...);
Player *player1 = new Player(...);
Player *player2 = new Player(...);
Agent *agents[3] = {monster1, player1, player2};

while(true)
{
    // make an array of agents "act"
    for(int i = 0; i < 3; i++)
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

Here is what your Agent class header (`agent.h`) might contain (at least):

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

Here is what your monster (Grue) class header (`grue.h`) might contain (at
least):

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

Here is a quick glance at how you might program a "random" monster that chooses
a random direction to move on each turn:

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

If you use `rand()` anywhere in your code, be sure to include this at the
beginning of `main()`:

{% highlight cpp %}
// ensure different random numbers each time the program is run
srand(time(NULL));
{% endhighlight %}

You are required to split your code into several files. For each class, make a
`.cpp` and a `.h` file. Also write a `main.cpp` file that has the `main()`
function in it. Check out the lecture notes for examples.

When you submit your code (via email), send me all of the `.cpp` and `.h`
files.

Your Room class must be updated as well to tell the player who is in the room.
That is, the Room class should have a vector or array or some other collection
that holds Agent pointers; when an agent enters the room, the agent's pointer
is added to the collection; when an agent leaves the room, the agent's pointer
is removed from the collection. The idea is, if the player does not know who is
in the same room as the player, then how will you, as the programmer, know if
your monsters are appropriately moving to different rooms?

If you work in a group of 2 or 3, you'll need to do more; for example, add a
combat system (as demonstrated in class), or add physical objects that don't
move, and add room capacities so that a room full of stuff may not be able to
accommodate the player.

