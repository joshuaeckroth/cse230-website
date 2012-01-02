---
layout: default
title: Drawbacks of object-orientation
---

## The circle-ellipse problem



## The expression problem



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

## Hierarchical brittleness

