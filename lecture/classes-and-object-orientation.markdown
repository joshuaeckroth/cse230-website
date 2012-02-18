---
title: Classes and object orientation
layout: default
---

Imagine we're writing a game. We'll need "data structures"
(collections of variables) to represent player characters (PCs; the
characters controlled by humans), non-player characters (NPCs; the
enemies that are part of the game), and physical objects (weapons,
crates, stairs, etc.). We could do this with basic classes:

{% highlight cpp %}
class Object
{
public:
    int weight;
    int position_x;
    int position_y;
};

class Weapon
{
public:
    int damage;
    int weight;
    string success_msg; // e.g. "the arrow hit!"
    string failure_msg; // e.g. "the arrow missed!"
};

class Player
{
public:
    int position_x;
    int position_y;
    int health;
    int strength;
    string name;
    Weapon *current_weapon;
    int id; // tells us which human is controlling which player
};

class Enemy
{
public:
    int position_x;
    int position_y;
    int health;
    int strength;
    string name;
    Weapon *current_weapon;
    int strategy_id; // what kind of AI (e.g. aggressive, friendly, etc.)
};
{% endhighlight %}

Good so far. Now we need functions. We want players and enemies to
move around, to fight each other; we want to determine which
objects/players/enemies are present at some position, etc. Here are
some function headers:

{% highlight cpp %}
void walk_one_step_player(Player* p, int direction);
void walk_one_step_enemy(Enemy* e, int direction);
void attack_enemy(Player* p, Enemy* e);
void attack_player(Enemy* e, Player* p);
{% endhighlight %}

Etc.

Since we're using classes, we can actually put the methods *inside*
the respective classes and get rid of the first parameter (the
pointer):

{% highlight cpp %}
class Player
{
public:
    int position_x;
    int position_y;
    int health;
    int strength;
    string name;
    Weapon *current_weapon;
    int id; // tells us which human is controlling which player
    
    void walk_one_step(int direction);
    void attack_enemy(Enemy* e);
    void attack_player(Player* p);
};

class Enemy
{
public:
    int position_x;
    int position_y;
    int health;
    int strength;
    string name;
    Weapon *current_weapon;
    int strategy_id; // what kind of AI (e.g. aggressive, friendly,
    etc.)
    
    void walk_one_step(int direction);
    void attack_enemy(Enemy* e);
    void attack_player(Player* p);
};
{% endhighlight %}

Because enemies, players, objects, and so on are distinct classes, we
have lots of specific functions. But these classes have a lot of
commonalities. Players and enemies are nearly the same thing except
for just a few differences. What if, instead, we could say "a player
is a kind of 'agent,' and an enemy is a kind of agent, and agents are
a kind of 'object' or 'thing'..."

This is what "object orientation" in general offers.

## Classes and inheritance

We really want a player to be a "kind of" agent and an agent a "kind
of" object. This is called "inheritance." We can diagram it like so
(in "UML" format):

![Class diagram (no methods)](/images/class-diagram-no-methods.png "Class diagram (no methods)")

The arrows mean that a class *inherits* properties from the parent
class (what an arrow points to). So a Player automatically has the
properties `health`, `strength`, and so on from the Agent class, and
has `position_x` and so on from the Object class.

We could redefine the functions above in order to work on instances of
the Object class, instances of the Agent class, and so on. Typically,
we put functions inside the classes themselves. (And we call typically
them "methods" when we do that, rather than "functions.") So our
diagram now shows methods as well:

![Class diagram (with methods)](/images/class-diagram-methods.png "Class diagram (with methods)")

Classes inherit methods as well as properties. So the Player class
automatically has access to the method `walk_one_step()`, which comes
from the Agent class.

## Why inheritance?

Without inheritance, we had to write functions like
`walk_one_step_player()` and `walk_on_step_enemy()`, or alternatively,
if we put the functions inside the classes, we had to write the
function `walk_one_step()` twice (for the `Player` and `Enemy` classes
each). Either way, there is duplication.

This is unfortunate because players and enemies are very much alike;
in fact, the two classes have many of the same attributes (internal
data): `int position_x` and `int position_y` in particular. This means
that the code for the `walk_one_step_player()` function and the code
for the `walk_one_step_enemy()` function or both class-specific
`walk_on_step()` functions will be identical! These functions will
change the `position_x` and/or `position_y` values of the player or
enemy class.

Well sheesh, isn't there some way to write a single function that
works on both players and enemies, so we don't have to repeat code?

In order to write just one function, we have tell the compiler that
players and enemies are going to share code. The way we do this is we
create a parent class `Agent` that has the `walk_one_step()` method.
We'll have the new `Player` and `Enemy` classes "inherit" the
attributes `position_x` and `position_y` from the `Agent` parent
class; they will also inherit the method `walk_one_step()`.

> The primary point of object-oriented programming is to move the
> focus of program design from algorithms to data structures. In
> particular, a data structure is elevated to the point that it can
> contain its own operations.

> But such data structures--called objects--often are related to one
> another in a particular way: One is just like another except for
> some additions or slight modifications. In this situation, there
> will be too much duplication if such objects are completely
> separate, so a means of inheriting code--methods--was developed.

> Hence we see the claim of reuse in object-oriented languages: When
> writing a single program, a programmer reuses code already developed
> by inheriting that code from more general objects or classes. There
> is a beauty to this sort of reuse: It requires no particular process
> because it is part of the nature of the language.

> In short, subclassing is the means of reuse. -- Richard P. Gabriel,
> *Patterns of Software*

## How to use class methods

Ok, now that we have the right hierarchy for Players and Enemies, we
need to figure out how to use the `walk_one_step()` function for some
particular player or enemy.

Let's create a player:

{% highlight cpp %}
Player me;
{% endhighlight %}

Now set its position:

{% highlight cpp %}
me.position_x = 5;
me.position_y = 9;
{% endhighlight %}

(Notice that we set attributes just like we did when we used
structures.)

Since a player is an agent and agents have a `walk_one_step()` method,
we can use it on the player called `me`:

{% highlight cpp %}
me.walk_one_step(0); // 0 means walk North
{% endhighlight %}

We use a method on a class variable (called variously an "instance" or
an "object") in the same we that we get or set attributes. That is to
say, we put the class variable or object first (`me`) then a dot
(`me.`) then the method name like a normal function call
(`me.walk_one_step(0)`).

This is not how we use the functions at the top of these lecture notes;
those functions were defined outside of the classes and thus needed a
parameter that was a pointer to a class object:

{% highlight cpp %}
Player me;
me.position_x = 5;
me.position_y = 9;
walk_one_step_player(&me, 0);
{% endhighlight %}

But not now that we are using classes:

{% highlight cpp %}
Player me;         // same as above
me.position_x = 5; // same as above
me.position_y = 9; // same as above
me.walk_one_step(0); // different
{% endhighlight %}

Since `walk_one_step()` is now a class method, it has to be called "on an
object" (the `me` before the dot). The code inside the method can just refer to
attributes `position_x` and `position_y` without using a pointer.

Let's compare. Here is the function:

{% highlight cpp %}
// walk_one_step_player() function
void walk_one_step_player(Player* p, int direction)
{
    if(direction == 0) // North
    {
        p->position_y = p->position_y - 1;
    }
    else if(direction == 1) // South
    {
    //...
    }
}
{% endhighlight %}

Now for the method version:

{% highlight cpp %}
// walk_one_step() class method
void walk_one_step(int direction)
{
    if(direction == 0) // North
    {
        position_y = position_y - 1;
    }
    else if(direction == 1) // South
    {
    //...
    }
}
{% endhighlight %}

The variable `position_y` in the code above (the second example, the
method) doesn't seem to exist (it's not a parameter and it's not
created in the method). So how do we know what `position_y` refers to?
Recall that a Player inherits the attribute `int position_y` from the
Agent class. And recall that the `walk_one_step()` method is being
called from a Player variable (object) called `me`:

{% highlight cpp %}
me.walk_one_step(0);
{% endhighlight %}

This means that `position_y` in the method is the `position_y` of the
`me` object.

Continue to
[Classes and object orientation (part 2)](/lecture/classes-and-object-orientation-2.html)
to learn how to create complete programs.

