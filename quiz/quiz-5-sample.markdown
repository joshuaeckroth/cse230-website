---
title: Quiz 5 sample
layout: default
---

Study the
[object orientation lecture notes](/lecture/classes-and-object-orientation.html)
to prepare for this quiz.

Be able to answer all of the following questions (in English, not
code):

* What are some benefits of object-oriented programming?

* How would you describe what a "class" is to somebody who knows how
  to program computers but does not know anything about
  object-oriented programming?

* Why do we use inheritance?

* Why is object-oriented programming a good way to write software that
  simulates some aspect of the world?

## My answers

*What are some benefits of object-oriented programming?*

All the data and code that makes sense for some type of thing can live
in one place and basically act as a black box. This has huge
advantages for mental models (having an idea about what the program is
doing) and imposes a good organization of the actual code. For
example, a Person class can encapsulate all the data and methods that
make sense for Person things, so that in some part of code one can
write `john.changeClothes()` and not worry about how it's actually
done or what data is read or altered. Object-oriented programming
allows a programmer to work with "objects" on which "actions" can be
performed; and the results of actions are typically invisible to the
programmer: instead of passing data into a function, internal object
data is utilized. Applying actions to objects is often exactly how a
programmer wants to think about a problem.

*How would you describe what a "class" is to somebody who knows how to
program computers but does not know anything about object-oriented
programming?*

A class is a kind of definition that says what data and actions are
available to members of that class. So a Person is a class, Siddharth
is a member (or instance or object) of that class, and Siddharth, like
all other members of the same class, has certain data and potential
actions: Siddharth has a name, an age, a home address, etc. and
Siddarth can sit down, stand up, change clothes, buy a house,
etc. More specifically, a class defines the data and actions that
operate on that data; once you have a class you can create instances
(variables) of that class, and set the data and perform the actions.

*Why do we use inheritance?*

Sometimes distinct classes share common data and/or common
actions. For example, rectangles, triangles, and ellipses are all
distinct classes, but have common "shape" properties and actions. Each
of those three classes have an x,y position in a 2D space and should
be able to compute an area, or rotate, or display themselves
graphically. Those common data and actions can be pulled out of the
three classes and put into a new Shape class, from which Rectangle,
Triangle, and Ellipse inherit. When one class inherits from another,
we say the inheritting class is a subclass, and that the subclass "is
a" member of the parent class. So Rectangle "is a" Shape because
Rectangle inherits from Shape. Having a superclass or inherited class
allows us to extract common features, which not only reduces our code
but also documents a hierarchical relationship among classes.

*Why is object-oriented programming a good way to write software that
simulates some aspect of the world?*

Often, we carve the world into hierarchical categories. For example,
an elephant is an African mammal which is further more a Mammal. If we
know something about Mammals (such as that they have hair) then we can
*infer* that Elephants do, too. Of course, this is true, as far as
mammals and elephants go, because Mammal is a category that was
created for exactly this purpose (inferring the properties of the
members of the class). In another example, we might simulate planetary
motions by creating a "Moving Body" class which has mass (and
therefore gravitational force), velocity, etc. Asteroids, planets, and
other such things would be members (or maybe subclasses) of the
"Moving Body" class. If we can take a real-world phenomenon and
categorize like we have done with the animal kingdom or planetary
motions, then we can easily use object-oriented programming to
simulate or model the phenomenon.
