---
title: Quiz 6 sample
layout: default
---

You will be asked to draw an object-oriented diagram, like the ones we
have seen in class and have appeared in the lecture notes on this
website. The diagram should represent a good object-oriented structure
that could be used to solve a particular problem. The diagram should
show several classes, how they relate (subclass--parent class,
i.e. inheritance relationships indicated by arrows with the arrow head
pointing to the parent class), what are their methods and properties,
and which methods are polymorphic. You do not need to indicate which
methods and properties are private/protected/public, nor do you need
to show constructors. Your diagram does not need to be a
formally-correct UML diagram or anything like that; it just needs to
show those aspects just mentioned.

Here is an example. Suppose you were asked to represent the following
problem domain with an object-oriented design. Software is needed for
those self-checkout kiosks at modern grocery stores. The software
needs to know how to price various products. All products have a
weight and description, but produce (vegetables and fruit) is priced
by weight and packaged goods have a fixed price. Some packaged goods
are food, and have a reduced tax rate; others are not food, and have
the full tax rate. Produce always has the reduced tax rate. Also, a
shopping cart object needs to keep track of which products have been
scanned and needs to calculate a total price (with tax). The shopping
cart class, named ShoppingCart, can be assumed to already exist. It
has a single private property: a set, or vector, or array (some kind
of collection) of Product pointers (`Product*`) that represents the
stuff already scanned at the kiosk; and it has a public method
`calculateTotal()` that finds the total cost of the shopping cart by
using the `getPrice()` and `getTaxRate()` methods of each scanned
product in the its collection.

What's a good object-oriented design for this problem?

Here is my solution:

![My solution](/images/quiz-7_0.jpg "My solution")

