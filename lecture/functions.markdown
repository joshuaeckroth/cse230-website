---
layout: default
title: Functions
---

All of our programs so far have lived entirely inside the `main()`
function. In other words, all the code was in one big pile.  So far
this has not been much of a problem because our programs were small.

But we did use functions that others have written. For example, we
have used cmath's `pow()` and `sqrt()` functions. Even `cout` and
`cin` are functions, though they are quite a bit more complicated and
a little bit special (actually, the stream operators `<<` and `>>` are
functions too, as we'll see several weeks from now).
 
If we did not have, say, `sqrt()`, at our disposal as a function, we
would need to include our own code for `sqrt()` (using an
approximation method like Newton's) in our big pile of code in
`main()`. This is undesirable.

Functions allow the programmer to *modularize* and *isolate* code, so
that in any single function, only one task is being performed. This
generally makes code more understandable.

Additionally, writing a function allows one to potentially hide
code. A function can be compiled and provided (in machine-readable
form, but not human-readable form) to other programmers. This is very
common; e.g., Microsoft provides many functions specific to Windows
that others can use (to write their own Windows programs) but that
others cannot read.

You can think of a function as simply code that has a name. The name
of some chunk of code is what the code *does*. For example, the
`sqrt()` function contains *only* enough code to find the square root
of a number.

<a href="http://hackles.org/strips/cartoon37.png">
![hackles comic](/images/methods.png "hackles comic")
</a>
 
## Where to write your own functions
 
For now, we will write functions in the same file as `main()` so that
our programs still consist of just one file. Programs need not be just
one file, however, and in the future we will explore ways to split up
code into multiple files (by putting functions in different files).

Functions can be defined above `main()`, or they can have just a
*prototype* (or *signature*) above `main()` with the function
definition below `main()`.  (Function definitions cannot be placed
inside other functions, although prototypes can.) Here are examples:

{% highlight cpp %}
#include <iostream>
using namespace std;
// this is an example of a function fully defined above main()

double timesTwo(double x)
{
    return 2*x;
}

int main()
{
    cout << timesTwo(4.5) << endl;
}
{% endhighlight %}

{% highlight cpp %}
#include <iostream>
using namespace std;

// this is an example of just a prototype above main()
// the function code itself is below main()

double timesTwo(double);

int main()
{
    cout << timesTwo(4.5) << endl;
}

double timesTwo(double x)
{
    return 2*x;
}
{% endhighlight %}

The importance of where the function is defined and defined (above
`main()` or not) is that the compiler requires that either the
function prototype or the whole function itself is found *above the
code that uses the function*. If you try to use a function called
`timesTwo()` but the compiler has not seen any function by that name
yet (either as a prototype or definition), then you get a compiler
error.
 
## Function return types and parameter types
 
Every function must have a return type (e.g. `int`, `double`, etc.),
or the function could be a `void` function, which means we write
`void` instead of a return type (`void` is not technically a
type). The return type goes before the function name. If the function
is not a void function, then the code inside the function must use
`return` somewhere, returning a value of the proper type. A void
function does not need the `return` command; it can include the
`return` command (this causes the function to terminate) but cannot
use `return` to return a value.

A function may have *parameters*. But it need not. A function that has
no parameters has empty parentheses. A function that has two `int`
parameters would have `int x, int y` in the parentheses. The `x` and
`y` names are usually not written in the function prototype (though
they can be). The `x` and `y` should be included, however, when the
function is being defined.

The `x` and `y` names *exist only inside the function*. Assume a
function is called `add` and it has two `int` parameters. This is what
it looks like:

{% highlight cpp %}
int add(int a, int b)
{
    return a+b;
}
{% endhighlight %}

Then when the function is called (say, from some code in `main()`),
two integer values must be provided as *arguments*: `add(4, 12)` (the
result of that function call is `16`, naturally). Inside the function,
the `4` is assigned to the variable name `a` and the `12` is assigned
to `b`. This `a` and `b` exist only inside the function.
 
Consider this example: 

{% highlight cpp %}
#include <iostream>
using namespace std;

int add(int a, int b)
{
    return a+b;
}

int main()
{
    int a = 15;
    int b = 20;
    cout << add(a, b) << endl;
}
{% endhighlight %}

The `a` and `b` inside `main()` are different variables than those
inside `add()`. Even if the code inside the function `add()` decided
to change the values of `a` and `b`, it would only change the values
for the variables known by those names inside the `add()` function,
not those known by those names inside `main()`. The function `add()`
cannot access the variables declared inside `main()`.

Since the variables inside a function are not the same as those inside
a different function, they need not have the same names:

{% highlight cpp %}
#include <iostream>
using namespace std;

int add(int someSillyNameX, int someSillyNameY)
{
    return (someSillyNameX + someSillyNameY);
}

int main()
{
    int a = 15;
    int b = 20;
    cout << add(a, b) << endl;
}
{% endhighlight %} 

Note that function names (e.g. `add`) have the same restrictions as
variable names (i.e. they cannot start with a number or special
symbol, etc.).

## Function calling & parameter passing

The functions demonstrated above use *call-by-value*, which means only
the value of the arguments is provided to the function. Here is the
textbook's explanation of how call-by-value works:

  * It is the values of the arguments that are plugged in for the
    formal parameters. If the arguments are variables, the values of
    the variables, not the variables themselves, are plugged in.

  * The first argument is plugged in the for the first parameter, the
    second argument is plugged in for the second parameter, and so
    forth.

Functions can also use *call-by-reference*. If a function uses
call-by-reference, then it receives not just the value of an argument
but also the *memory location* of the original variable. All variables
keep track of their memory location so they know where their value is
stored in memory. When a variable is updated or assigned, the value in
the variable's memory location is changed.

When a function uses call-by-reference, it also has access to the
memory locations that the original variables (in the calling function)
used. So the function can change the values in those memory locations.

Here is an example of a function that uses call-by-reference. It's the
"increment" function, which means it takes an integer parameter and
increases that integer by one.

{% highlight cpp %}
#include <iostream>
using namespace std;

void increment(int &x)
{
    x++;
}

int main()
{
    int a = 5;
    increment(a);

    cout << a << endl;
    return 0;
}
{% endhighlight %}

You know the `increment()` function uses call-by-reference for its
single parameter because that parameter has an `&` in front of
it. When `main()` calls `increment()`, the variable `a` (inside
`main()`) is *passed by reference* to the function. The function does
not name this variable (and its memory location) "a" but instead names
it `x`. Otherwise, *they are the same variable*. Any changes to `x`
inside the function cause the same changes to `a` inside the calling
function (i.e. `main()`) because `x` and `a` *use the same memory
location to store their value*. We say this is a case of
"call-by-reference" because it's not the value of `a` that is being
provided to the function (like it would be if we left out the `&` and
therefore had a call-by-value parameter) but rather the memory
location to which `a` *refers*.

A function can have a mix of call-by-reference (or
"pass-by-reference") and call-by-value parameters. For example, this
function updates two variables `w0` and `w1` (which are passed by
reference, so that they can be updated) depending on the value of
another variable `y` (which is just passed by value):

{% highlight cpp %}
void updateXYZ(double &w0, double &w1, double y)
{
    if(y < 0.0)
    {
        w0 = w1 = 0;
    }
    else
    {
        w0 = pow(w0+w1, 2.0);
        w1 = pow(w0-w1, 2.0);
    }
}
{% endhighlight %}

## Pointers as parameters to functions

The C language (which came before C++) didn't have
"call-by-reference," so in order to make a function that could change
the values of its arguments, pointers were used:

{% highlight cpp %}
void changeValues(int *px, int *py)
{
    // add one to each variable pointed to by the parameters
    *px = *px + 1;
    *py = *py + 1;
}
{% endhighlight %}

C++ can do the same thing (although how the function is used must
change):

{% highlight cpp %}
void changeValues2(int &x, int &y)
{
    // add one to each variable
    x = x + 1;
    y = y + 1;
}
{% endhighlight %}

These are equivalent except in how they are used. Here is how the C
version (which uses pointers) is used:

{% highlight cpp %}
int x = 5, y = 8;
changeValues(&x, &y);
{% endhighlight %}

Note that you have to provide the "memory location" of the variables
`x` and `y` to use the function that has pointer parameters.

The C++ version (call-by-reference) can be used in a more
straight-forward manner:

{% highlight cpp %}
int x = 5, y = 8;
changeValues2(x, y);
{% endhighlight %}

This is why call-by-reference is useful; it makes the code a little
simpler, but has the same effect.


## An example from class: TimeAdder

{% highlight cpp %}
// This program adds a specified number of minutes
// to a specified time (given in 24-hour format),
// and displays the result, after converting
// the resulting time to 12-hour format.

#include <iostream>
using namespace std;

// convert a 24-hour time to a 12-hour time,
// and set ampm equal to P or A
void to_am_pm(int &hour, int minute, char &ampm)
{
    if(hour > 12)
    {
        ampm = 'P';
        hour -= 12;
    }
    else
    {
        ampm = 'A';
    }
}

// add a specified number of minutes to
// a time; set hour and minute to the
// resulting time; note that hour may
// become greater than 12, and in cases
// when adding lots of minutes, may become
// greater than 23
//
// refer to the to_am_pm() function to
// convert the result from 24-hour time
// to 12-hour time
void add_min(int &hour, int &minute,
             int min_to_add, char &ampm)
{
    int plus_hours = min_to_add / 60;
    min_to_add %= 60;
    hour += plus_hours;
    minute += min_to_add;
    to_am_pm(hour, minute, ampm);
}

int main()
{
    int h, m;
    cout << "Enter hour (0-23) followed by space "
         << "followed by minutes (0-59): ";
    cin >> h >> m;

    int min_to_add;
    cout << "Enter minutes to add: ";
    cin >> min_to_add;

    char ampm;
    add_min(h, m, min_to_add, ampm);

    cout << "Final time is " << h << ":" << m
         << " " << ampm << "M" << endl;

    return 0;
}
{% endhighlight %}

## Functions inside classes ("methods")

We can put a function inside a class; in this case, the function can
only be used on an existing object of that class.

Recall the `Person` class:

{% highlight cpp %}
class Person
{
public:
    string name;
    int age;    // in years
    int height; // in cm
    int weight; // in kg
};
{% endhighlight %}

Let's add a function that prints the person's weight:

{% highlight cpp %}
class Person
{
public:
    string name;
    int age;    // in years
    double height; // in cm
    double weight; // in kg
    
    void printWeight()
    {
        cout << name << " weighs " << weight << " kg." << endl;
    }
};
{% endhighlight %}

Suppose we have an object:

{% highlight cpp %}
Person vignesh;
vignesh.name = "Vignesh S.";
vignesh.age = 25;
vignesh.height = 177;
vignesh.weight = 68;
{% endhighlight %}

We can use the `printWeight()` function in the following way, using
the object as the prefix (just like when we set values like `weight`):

{% highlight cpp %}
vignesh.printWeight();
{% endhighlight %}

Or, we can add a function that returns the weight in pounds:

{% highlight cpp %}
class Person
{
public:
    // ...
    
    double getWeightPounds()
    {
        return (2.204 * weight);
    }
};
{% endhighlight %}

And we can use it like so:

{% highlight cpp %}
double pounds = vignesh.getWeightPounds();
{% endhighlight %}

## A final word

> It is surely time to recover the original sense of "argument" (via
> Latin *arguere*, to put in a clear light) as "clarification, proof."
> The depressing confusion over name/value calling, between
> real/formal arguments and/or parameters, and how/when/where they are
> initialized and/or assigned *must* be resolved here and
> now. Remember: if you pass by name, the function can corrupt your
> actual argument, but if you pass by value, the function can only
> corrupt a *copy* of your argument. Some sophisticated languages let
> you pass explicit pointers, pointers-to-pointers, references,
> references-to-pointers, pointers-to-references, and so on to any
> depth (whence the phrase "beyond fathomage"), allowing the function
> to corrupt not only your arguments *and* their copies, but also
> those of your erstwhile friends running in distant parts of the
> system. It's your call, as they say. -- *The computer
> contradictionary*

