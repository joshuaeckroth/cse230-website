---
layout: default
title: Drawbacks of object-orientation
---

Object-orientation generally involves creating classes or categories
of variables, using inheritance on these classes, and polymorphic
functions so that variables of one type can "pretend" to be other
types. Object-oriented design has several benefits, of which you
should be familiar by now, but there are also drawbacks to such an
approach.

## The Circle-Ellipse problem

Sometimes a widely-accepted definition of a category or class does not
fit well in an object-oriented (OO) design. A good example of this mismatch
with existing definitions and OO is the definition of a circle: a
circle is an ellipse in which the major and minor axes are equal.

You might expect to create an ellipse class that has a major and minor
axis, and a circle class that inherits from it:

![Circle-Ellipse diagram](/images/circle-ellipse-1.png "Circle-Ellipse diagram")
 
However, in such a class model, the Circle class inherits all the
properties and functions of the Ellipse class. This allows a user of
the Circle class to change the minor or major axis, and make these
axes out of sync. The circle has turned into an ellipse, but the
variable is still a Circle object.

The Circle class may add a `radius` variable and `set_radius`
function, but it cannot refuse inheriting the `set_major_axis` and
`set_minor_axis` functions. So, `set_radius` is superfluous, and does
not guarantee a circle cannot turn into an ellipse.

A different approach could be to make Circle and Ellipse completely
separate:

![Circle-Ellipse diagram](/images/circle-ellipse-2.png "Circle-Ellipse diagram")
 
But that seems like a significant failure of OO to model the real
world.

For yet another alternative, the `set_major_axis` and `set_minor_axis`
functions can be virtual (not pure virtual):

![Circle-Ellipse diagram](/images/circle-ellipse-3.png "Circle-Ellipse diagram")
 
The Circle class can reimplement these functions so that any call to
`set_major_axis` also sets the minor axis (to the same value), and
vice versa. The drawback of this approach is that the names
`set_major_axis` and `set_minor_axis` do not truly apply to circles,
and how they work in the Circle class is somewhat counter-intuitive
(and must be documented somewhere).

The C++ FAQ has a good response to this problem, full of details and
examples, and the following interesting quote (which begins the answer
to
[question 21.8](http://www.parashift.com/c++-faq-lite/proper-inheritance.html#faq-21.8)):

> *Question:* But I have a Ph.D. in Mathematics, and I'm sure a Circle
> is a kind of an Ellipse! Does this mean Marshall Cline is stupid? Or
> that C++ is stupid? Or that OO is stupid?
>
> *Answer:* Actually, it doesn't mean any of these things. But I'll tell
> you what it does mean -- you may not like what I'm about to say: it
> means your intuitive notion of "kind of" is leading you to make bad
> inheritance decisions. Your tummy is lying to you about what good
> inheritance really means -- stop believing those lies.
>
> Look, I have received and answered dozens of passionate e-mail
> messages about this subject. I have taught it hundreds of times to
> thousands of software professionals all over the place. I know it
> goes against your intuition. But trust me; your intuition is wrong,
> where "wrong" means "will cause you to make bad inheritance
> decisions in OO design/programming."

Really, that entire C++ FAQ page is a great read. Marshall Cline, the
author, makes an admirable attempt trying to explain why our OO
intuitions are some times completely inappropriate. That this issue is
so complicated (he devotes 5,000 words to it) just shows how complex
OO really can be.

## The Expression Problem

The original definition of the expression problem is as follows:

> The Expression Problem is a new name for an old problem. The goal is
> to define a datatype by cases, where one can add new cases to the
> datatype and new functions over the datatype, without recompiling
> existing code, and while retaining static type safety (e.g., no
> casts). -- Philip Wadler

In terms of C++, the problem is essentially: can we define classes,
and compile them, in such a way that in the future we can (1) create
new subclasses ("add new cases to the datatype") so that existing
functions that use the original class will still work on our new
class; and (2) create new functions in the existing classes (without
changing the original code, which has already been compiled).

C++ lets us solve issue (1): create new subclasses that look like the
original but act differently. This is possible if the original class
had virtual functions. Our subclass can redefine what those virtual
functions do. So far so good.

But C++ cannot solve issue (2): we cannot add functions to existing,
compiled classes. For example, we cannot add any function to the
string class, or the CDAccount class once they are compiled. (Even
though the CDAccount class is defined in the `cdaccount.h` file and
you can add functions to that file, you will need to recompile, so
that the rest of the code knows how to use the new class
definition. When you update the class definition, stuff might get
rearranged, and old compiled code may no longer work.)

Philip Wadler continues,

> Whether a language can solve the Expression Problem is a salient
> indicator of its capacity for expression.

I know of one language that does solve this problem, in case you are
curious: [Clojure](http://www.infoq.com/presentations/Clojure-Expression-Problem).

## Hierarchical accumulation

As we know how, an inherited class can't "refuse" to inherit data or
methods from its parent class. Subclasses (deeper and deeper) just
accumulate data and methods. Methods can be redefined but, unless
polymorphism is enabled (in a superclass), the redefined methods won't
be activated when we use a pointer to the superclass.

> Q: What's big and gray, has a trunk, and lives in the trees?

> A: An elephant -- I lied about the trees.

What we'd like to be able to do is create, say, an `Animal` class, a
`Mammal` class, and an `Elephant` class (with the obvious inheritance
relations). Animals will have generic properites, like `int age`.

Mammals will have methods like `birth()` and
`grow_more_hair()`. Elephants will have properties like `string color`
and methods like `calculateTrunkLength()`, `roam()`, `makeNoise()`,
etc.

Now, say we want to create a subclass of `Elephant`, such as
`GlassElephantFigurine` that should share many of the same properties
as the `Elephant` class but not all (a figurine can't roam or make
noise, for example), and should share some of the `Mammal` attributes
but not all, etc.

You may object and believe that a glass figurine should not inherit
*anything* from the class representing real elephants. But then the
`GlassElephantFigurine` class has to duplicate much of the
data/methods in the `Elephant` class.

If you attempt to build the inheritance as described, you wind up
having glass figurines that can roam, make noise, have hair,
etc. which is ridiculous.

The problem is that a subclass cannot refuse properties/methods from
its parent(s). It's all or nothing.

Why this restriction? It can be proven that if we allow subclasses to
refuse properties or methods, then we completely lose the ability to
make accurate assumptions about subclasses. If subclasses can't refuse
properties or methods, then we know for certain that *any* subclass or
sub-subclass of `Animal` *will* have an `int age` property, and *any*
subclass or sub-subclass of `Mammal` will have a `grow_more_hair()`
method, and so on. If we can make these accurate assumptions, then we
can save ourselves the time of checking if they're true. We know
they're true. If subclasses can refuse properties or methods, we don't
know they're true, and can't make any such assumptions; in fact, we
wouldn't know *anything* about the subclasses by just looking at the
superclass!

Read the article
["I Lied About the Trees, Or, Defaults and Definitions in Knowledge Representation"](https://www.aaai.org/ojs/index.php/aimagazine/article/viewArticle/490)
by Ronald J. Brachman for an academic take on this issue.

