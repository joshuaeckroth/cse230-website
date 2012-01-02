---
title: Polymorphism
layout: default
---

Consider the `Shape` class, and its descendents, defined in previous
lectures.  A `Shape` is a kind of "abstract" category that cannot be
used to perform any real task. For example, you cannot draw a shape,
or find its area. At least, that's how we are thinking about
shapes. When we defined the `Shape` class, we did not provide any of
these methods (e.g. `draw()`, `area()`, etc.). However, subclasses
like `Rectangle` and `Ellipse` did have some useful methods (e.g.
`area()`). That's because, obviously, one can actually draw a
rectangle or calculate its area.

We already know that using object-orientation and inheritance in
particular is a good idea for shapes; that's because rectangles,
ellipses, and so on share many common properties and methods, such as
the properties `x` and `y` (location in a 2D space), and perhaps some
methods like `translate()`, `rotate()`, and so on. However, we were
unable to put the `area()` method in the `Shape` class, even though all
subclasses will have their own `area()` methods, because there is no
concept of the "area" of a "generic shape." So instead we just defined
the `area()` method for each subclass.

Now, there is a significant problem with this state of affairs we have
constructed. We're not getting the full power of object-orientation
and inheritance. We all know that every shape has an area, so any
object that is a `Shape`, by way of inheritance, should have an
`area()` method. Recall that if we create a `Rectangle` object, say
`Rectangle r(3, 5);` then we are simultaneously creating a `Shape`
object as well; because `Rectangle` is a subclass of `Shape`, any
`Rectangle` object is also a `Shape` object. That means you can change
a `Rectangle` object into a `Shape` object:

{% highlight cpp %}
// this is valid since Rectangle inherits from Shape
Shape s = Rectangle(3, 5);

// or using pointers instead:
Shape *s = new Rectangle(3, 5);
{% endhighlight %}

However, even though `s` *really is a `Rectangle`*, now that we are
treating it as a `Shape` we cannot ask for its area, because the `Shape`
class does not have an `area()` method.

However, we often find it convenient, or even necessary, to gather up
several objects of several distinct types (different classes) which
all happen to inherit from the same parent class. In our shapes
example, we might want to gather up several objects of several kinds
of shapes, and put them into a vector:

{% highlight cpp %}
// make a few shapes
Triangle t(3, 4, 3.141/2.0);
Rectangle r(8, 12);
Ellipse e(3.4, 3.4);

vector<Shape> myshapes;
myshapes.push_back(t);
myshapes.push_back(r);
myshapes.push_back(e);

// or using pointers
vector<Shape*> myshapes2;
myshapes2.push_back(&t);
myshapes2.push_back(&r);
myshapes2.push_back(&e);
{% endhighlight %}

However, now that they are all considered *just* shapes and not
rectangles, ellipses, and so on, the objects in the vector can only be
treated as objects of type `Shape`. So, in particular, we can't ask any
object in the vector to calculate its area.

This is the problem that polymorphism solves. Polymorphism means that
an object of some class, say `r` of the class `Rectangle`, can "look
like" a `Shape` object but *act* like a `Rectangle` object when its
asked to do things that may be done differently by a `Rectangle`.

Consider a different example: human infants are humans, and humans
walk on two feet. Perhaps you have defined a class named `Human` and a
class named `HumanInfant` (which is a subclass of `Human`). The
`Human` class may have a method `move()` that visually animates a
human walking on two feet. The `HumanInfant` will also have a method
called `move()` but what you'll see is a baby crawling. So of course
the code for the two `move()` methods will be different. Now consider
that you have a collection of `Human` objects, say a vector called
`characters_in_the_game`, and you want to tell each character to move
in some random direction. You want the correct `move()` method to work
on each character. In this example, you will have `Human` objects and
`HumanInfant` objects, but they will all "look like" `Human` objects
when the are put into the vector (because vectors can only hold one
type of thing, and you don't want to change `Human` objects into
`HumanInfant` objects; rather, the other way around is more
appropriate, because the class hierarchy says `HumanInfants` are
`Humans`, not vice versa). Even though the baby characters will appear
to be regular adult characters (because every object in the vector
will be of the type `Human`-pointer, i.e. `Human*`), when the `move()`
method is called for each object in the array, the `HumanInfant`'s
`move()` method is called if the object is *actually* of that type,
and not of the type `Human`. This is polymorphism (an object looking
like another, as far as types are concerned, but behaving in its
specific way).

To achieve this effect with our `Shape` class, we indicate that the
`area()` method is `virtual`. By `virtual` we mean polymorphic (but
the C++ creator thought `virtual` was a better word, it seems). Here
is the modified class:

{% highlight cpp %}
class Shape
{
public:
    double x;
    double y;

    // this method is virtual so that subclasses can redefine it
    virtual double area()
    {
        // if this method is used with a true generic shape object,
        // just return 0.0
        return 0.0;
    }
};
{% endhighlight %}

The subclasses need not change at all.

Finally, this `Shape` class should not really have an `area()` method
at all, since we don't ever want to create true generic `Shape`
objects. Rather, we will only be creating `Rectangle`, `Ellipse`,
etc. objects. So we delete the code for the `area()` method in the
`Shape` class and just write `= 0;` instead to indicate that
subclasses will have this `area()` method but the `Shape` class will
not. By doing this, we are turning `area()` into a "pure virtual"
method, and thus changing the `Shape` class into an "abstract class."
The reason for this extra terminology is that we will no longer be
able to create `Shape` objects (e.g.  `Shape s;` which we never
attempted to do anyway) because the `Shape` class has a method with no
definition (a purely virtual method). This makes the `Shape` class a
kind of "contract" which says "any subclasses must provide code for
their own `area()` method because I am introducing such a method but
not providing any code for it."

{% highlight cpp %}
class Shape
{
public:
    double x;
    double y;

    // this method is purely virtual; subclasses *must* define it
    virtual double area() = 0;
};
{% endhighlight %}

Now that `Shape` is an abstract class, we can't turn `Rectangle` or
whatever into `Shape`; we can only use the `Shape` class as a pointer
to a true `Rectangle`, `Ellipse`, etc.

{% highlight cpp %}
// not possible; ERROR!
Shape s = Rectangle(3, 4);

// this is ok
Shape *s = new Rectangle(3, 4);
{% endhighlight %}

And we can ask for the rectangle's area even though it "looks like" a
`Shape`:

{% highlight cpp %}
// polymorphism is used here
Shape *s = new Rectangle(3, 4);
cout << s->area() << endl;
{% endhighlight %}

Here is another, completely different example.

{% highlight cpp %}
// stolen from http://en.wikipedia.org/wiki/Virtual_function

#include <iostream>
#include <vector>
using namespace std;
 
class Animal
{
public:
    virtual void eat()
    { 
        cout << "I eat like a generic Animal." << endl; 
    }
};
 
class Wolf : public Animal
{
public:
    void eat()
    { 
        cout << "I eat like a wolf!" << endl; 
    }
};
 
class Fish : public Animal
{
public:
    void eat()
    {  
        cout << "I eat like a fish!" << endl; 
    }
};
 
class GoldFish : public Fish
{
public:
    void eat()
    { 
        cout << "I eat like a goldfish!" << endl; 
    }
};
 
class OtherAnimal : public Animal
{
    // does nothing special
};
 
int main()
{
    vector<Animal*> animals;
    animals.push_back(new Animal());
    animals.push_back(new Wolf());
    animals.push_back(new Fish());
    animals.push_back(new GoldFish());
    animals.push_back(new OtherAnimal());
 
    for(int i = 0; i < animals.size(); i++)
    {
        animals[i]->eat();
    }
 
    return 0;
}
{% endhighlight %}

Output:

<pre>
I eat like a generic Animal.
I eat like a wolf!
I eat like a fish!
I eat like a goldfish!
I eat like a generic Animal.
</pre>

If we did not use `Animal` pointers, but instead put instances of each
class into the vector (rather than pointers), like so:

{% highlight cpp %}
vector<Animal> animals;
animals.push_back(Animal());
animals.push_back(Wolf());
animals.push_back(Fish());
animals.push_back(GoldFish());
animals.push_back(OtherAnimal());

for(int i = 0; i < animals.size(); i++)
{
    animals[i].eat();
}
{% endhighlight %}

...then polymorphism would not take effect. This would be the output:

<pre>
I eat like a generic Animal.
I eat like a generic Animal.
I eat like a generic Animal.
I eat like a generic Animal.
I eat like a generic Animal.
</pre>
