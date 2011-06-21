---
title: Homework 5
layout: commentable
---

From the book, p. 606, q. 10. Due Aug 1, 11pm.

Write a rational number class. This problem will be revisited in Chapter 11,
where operator overloading will make the problem much easier. For now we will
use member functions `add`, `sub`, `mul`, `div`, and `less` that each carry out
the operations +, -, *, /, and <. For example, a+b will be written `a.add(b)`,
and a<b will be written `a.less(b)`.

Define a class for rational numbers. A rational number is a "ratio-nal" number,
composed of two integers with division indicated. The division is not carried
out, it is only indicated, as in 1/2, 2/3, 15/32, 65/4, 16/5. You should
represent rational numbers by two `int` values, `numerator` and `denominator`.

A principle of abstract data type construction is that constructors must be
present to create objects with any legal values. You should provide
constructors to make objects out of pairs of `int` values; this is a
constructor with two `int` parameters. Since every `int` is also a rational
number, as in 2/1 or 17/1, you should provide a constructor with a single `int`
parameter.

Provide a member function `output` that takes an `ostream` argument and writes
rational numbers in the form 2/3 or 37/51 to the screen or a file (to the
`ostream`).

Provide a *non-member* function `input` that takes an `istream` argument an
reads from the `istream` two integers, representing a numerator and a
denominator, and returns a rational number.

Provide member functions `add`, `sub`, `mul`, and `div` that return a rational
value. Provide a function `less` that returns a `bool` value. These functions
should do the operation suggested by the name. Provide a member function `neg`
that has no parameters and returns the negative of the calling object.

Each of these member functions should return a different instance of the class.
So once you have an instance of the class, you cannot change its values
(rationals will be "immutable"). For example, here is what the `add` function
header looks like:

{% highlight cpp %}
Rational add(const Rational &other);
{% endhighlight %}

That function header indicates: the `add` function is a member function of
class `Rational` (because it will be somewhere inside the class definition,
which is not shown here), and it receives as its single parameter a reference
to a constant instance of the same class (which we call `other` in the
function). We make it a reference parameter to make the function call more
efficient (the instance `other` need not be copied when the function is
called), and since it's a reference parameter, we make it `const` to ensure we
don't accidentally change it.

To repeat, inside that `add` function, you'll make an instance of the Rational
class after you figure out what its values will be. Then you return that
instance.

Always store and print the *reduced form* of the rational number; that is,
always divide out the greatest common divisor (GCD) of the numerator and
denominator. C++ code for finding the GCD is abundant on the internet;
[here](http://www.aivosto.com/visustin/sample/gcd-c.html) is one example.

Provide a `main` function that thoroughly tests your class implementation.

The following formulas will be useful in defining functions.

{% highlight cpp %}
a/b + c/d = (a*d + b*c) / (b*d)
a/b - c/d = (a*d - b*c) / (b*d)
(a/b) * (c/d) = (a*c) / (b*d)
(a/b) / (c/d) = (a*d) / (c*b)
-(a/b) = (-a/b)
(a/b) < (c/d) means (a*d) < (c*b)
(a/b) == (c/d) means (a*d) == (c*b)
{% endhighlight %}

Let any sign be carried by the numerator; keep the denominator positive.

