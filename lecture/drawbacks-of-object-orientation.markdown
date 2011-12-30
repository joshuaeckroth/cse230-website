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
polymorphism is enabled (in the superclass), the redefined methods
won't be activated when all we have is a pointer to the parent class
type.

> Q: What's big and gray, has a trunk, and lives in the trees?

> A: An elephant -- I lied about the trees.

What we'd like to be able to do is create, say, an Animal class, a
Mammal class, and an Elephant class (with the obvious inheritance
relations). Animals will have generic properites, like `int age`.

Mammals will have interesting methods like `birth()` and
`grow_more_hair()`. Elephants will have methods like `walk()` that
uses the elephants four legs. And so on.

Now, say we want to create a subclass of Elephant, such as
GlassElephantFigurine that


## Hierarchical brittleness

