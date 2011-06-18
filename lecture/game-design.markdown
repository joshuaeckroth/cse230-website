---
title: Object-oriented design for a game
layout: default
---

A game is a good domain to study object-oriented design. Since CSE 230 is not
an object-oriented design class (that's CSE 502 or CSE 616), I will demonstrate
a somewhat-complicated object-oriented rather than asking you to come up with
your own.

In my game, there are one or more human players (the Player class), and some
grues (the Grue class; a grue is a scary monster that eats people, but is never
present in lighted rooms, for what it's worth). The Game class tells the grues
and players to make their moves (the grues' moves are random, the players'
moves come from asking the person at the keyboard to make a move).

Rooms are connected to each other by arbitrary exits (e.g. "south," "out the
window"). Each room keeps track of the things inside it (using a `set`). Also,
there are generic things (the Thing class) such as statues and forks and
knives, lying around the maze. Since Players and Grues are Agents and Agents
are Things, each room is keeping track of the things, players, and grues inside
of it.

The solid-line arrows in the diagram indicate inheritance relationships. The
dotted-line arrows represent "dependency," which means, for example, that the
Player and Grue classes may need to be modified if the Room class changes its
definition.

Private variables/methods start with a dash (-), protected variables/methods
start with a hash (#), and public variables/methods start with a plus (+). Pure
virtual methods are bold and italicized.

![Game UML](/images/grues-uml.png "Game UML")

Here is my `main()` function:

{% highlight cpp %}
int main()
{
    Game game;

    Room *entrance = new Room("Entrance",
                              "A wide open entrance...", 100);
    Room *hallway = new Room("Hallway",
                             "A long hallway...", 50);
    Room *ballroom = new Room("Ballroom",
                              "A huge ballroom...", 200);

    entrance->link(hallway, "south");
    hallway->link(entrance, "north");
    hallway->link(ballroom, "east");
    ballroom->link(hallway, "west");

    Player *josh = new Player("Josh", "A prince", 50);
    Player *tracy = new Player("Tracy", "A princess", 40);
    josh->moveTo(entrance);
    tracy->moveTo(entrance);
    game.addAgent(josh);
    game.addAgent(tracy);

    Grue *napolean = new Grue("Napolean");
    Grue *kafka = new Grue("Kafka");
    napolean->moveTo(ballroom);
    kafka->moveTo(ballroom);
    game.addAgent(napolean);
    game.addAgent(kafka);

    Thing *liberty = new Thing("Statue of Liberty",
                               "A miniature Statue of Liberty", 5);
    Thing *hoop = new Thing("Hoop", "A basketball hoop", 30);
    liberty->moveTo(entrance);
    hoop->moveTo(ballroom);

    cout << "Welcome!" << endl;

    // the step() function in the Game class will eventually
    // return false, when a player chooses to quit;
    // this tiny "while" loop keeps asking the game.step()
    // function if it is false or true; the effect is
    // that the step() function is called repeatedly
    // until it returns false
    while(game.step());

    return 0;
}
{% endhighlight %}

Here is `thing.h`:

{% highlight cpp %}
#ifndef THING_H
#define THING_H

#include <string>
#include "room.h"
using namespace std;

class Thing
{
    private:
    string name, description;
    int size;

    protected:
    Room *cur_room;

    public:
    Thing(string _name, string _description, int _size);

    bool moveTo(Room *r);

    string getName();
    string getDescription();
    int getSize();
};

#endif
{% endhighlight %}

... and `thing.cpp`:

{% highlight cpp %}
#include "thing.h"

// this constructor just sets all the Thing class's properties;
// it uses the ":" syntax for convenience
Thing::Thing(string _name, string _description, int _size)
 : name(_name), description(_description), size(_size), cur_room(NULL)
{}

bool Thing::moveTo(Room *r)
{
    // moving to a room will fail if the room is full;
    // the add() function (part of the Room class) returns
    // true or false depending on whether the thing can
    // fit into the room (rooms have capacities and things;
    // things have sizes)
    if(r->add(this))
    {
        if(cur_room != NULL)
            cur_room->remove(this);

        cur_room = r;
        return true;
    }
    return false;
}

string Thing::getName()
{
    return name;
}

string Thing::getDescription()
{
    return description;
}

int Thing::getSize()
{
    return size;
}
{% endhighlight %}

Here is `game.cpp`:

{% highlight cpp %}
#include <cstdlib>
#include "game.h"

Game::Game()
{
    // initialize the random number generator (for Grue movements)
    // when a new game is started
    srand(time(NULL));
}

void Game::addAgent(Agent *agent)
{
    agents.push_back(agent);
}

bool Game::step()
{
    // give each agent a chance to move;
    // if any return false (e.g. a player quits), also return false
    for(unsigned int i = 0; i < agents.size(); i++)
    {
        if(!agents[i]->act())
            return false;
    }
    return true;
}
{% endhighlight %}

