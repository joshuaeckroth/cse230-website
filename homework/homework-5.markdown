---
title: Homework 5
layout: default
---

From the book, p. 606, q. 10.

Skills needed to complete this assignment:

  - Creating classes and using object-oriented program design
    ([lecture notes](/lecture/classes-and-object-orientation.html))

  - Splitting code into several files
    ([lecture notes](/lecture/splitting-code.html))

(See [rectangle.cpp](/code/rectangle-cpp.html) for an example from
class that's similar to this assignment.)

Write a rational number class. This problem is revisited in Chapter 11
(in the book), where operator overloading will make the problem much
easier. For now we will use member functions `add`, `sub`, `mul`,
`div`, and `less` that each carry out the operations `+`, `-`, `*`,
`/`, and `<`. For example, `a+b` will be written `a.add(b)`, and `a<b`
will be written `a.less(b)`.

Define a class for rational numbers. A rational number is a
"ratio-nal" number, composed of two integers with division
indicated. The division is not carried out, it is only indicated, as
in 1/2, 2/3, 15/32, 65/4, 16/5. You should represent rational numbers
by two `int` values, `numerator` and `denominator`.

We want to store rational numbers in *canonical* form, meaning the
rational number 9/12 would be stored as 3/4 and 25/5 would be stored
as 5/1. This is discussed more below.

A principle of abstract data type construction is that constructors
must be present to create objects with any legal values. You should
provide constructors to make objects out of pairs of `int` values
(i.e. a constructor with two `int` parameters). Since every `int` is
also a rational number, as in 2/1 or 17/1, you should provide a
constructor with a single `int` parameter, which just sets the
numerator to the value of the parameter and the denominator to 1.

Provide *member* functions `add`, `sub`, `mul`, and `div` that return
a rational value (each of these functions has a single rational
parameter). Provide a function `less` that has a single rational
parameter and returns a `bool` value. These functions should do the
operation suggested by the name (e.g. `num1.less(num2)` returns true
if the rational number `num1` is less than the rational number
`num2`). Provide a member function `neg` that has no parameters and
returns a rational number that is the negative of the calling object.

Also provide a member function `print` that simply prints the rational
number in a format like `-9/13`.

Each of these member functions should return a different instance of
the class. So once you have an instance of the class, you cannot
change its values (rationals will be "immutable"). For example, here
is what the `add` function header looks like:

{% highlight cpp %}
Rational add(Rational &other)
{
    int n, d;
    n = ...
    // ...
    return Rational(n, d);  // return a different rational number
}
{% endhighlight %}

The function above can be understood in the following way. The `add`
function is a member function of class `Rational` (because it will be
somewhere inside the class definition, which is not shown here), and
it receives as its single parameter a reference to an instance of the
same class (which we call `other` in the function). We make it a
reference parameter ("call-by-reference") to make the function call
more efficient (the instance `other` need not be copied when the
function is called).

To repeat, inside the `add` function, you'll make an instance of the
Rational class and set its values. Then you return that instance.

Always store and print the *reduced form* of the rational number; that
is, always divide out the greatest common divisor (GCD) of the
numerator and denominator. C++ code for finding the GCD is abundant on
the internet;
[here](http://www.aivosto.com/visustin/sample/gcd-c.html) is one
example. I recommend you make a `gcd` function that's not a member
function, that only has two integers as its parameters, and that
returns an integer.

**Some advice:** Use the GCD function in the constructor that has two
inputs (numerator and denominator); only use the GCD function
here. Then, like the code example above, every other function
(e.g. `add`, `div`, etc.) should call the constructor. That way, every
function returns a reduced form of the answer.

Provide a `main` function that thoroughly tests your class
implementation (write a little test for each of the functions).

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

Keep any negative sign in the numerator; keep the denominator
positive. (So 5/-2 should be saved as -5/2.)

Finally, you must split your code into three files (exactly these
files):

  - `rational.h` which contains the class declaration

  - `rational.cpp` which contains the class functions

  - `main.cpp` which contains testing code (i.e. the `main()`
    function)

Submit to me all three files (either in a ZIP or three email
attachments).

