---
layout: default
title: Variables and types
---

It is very important when writing computer programs that one is able to save
data in temporary locations, move data in and out of these locations and change
the data over time. How we do this in C++ is with variables.

Consider the mathematical formula c =
(a<sup>2</sup>+b<sup>2</sup>)<sup>0.5</sup> (Pythagoras' formula). This formula
can find the length of the hypotenuse of *any* right triangle. If we were to
write a program that implemented this formula, then we would have a program
that can find the length of the hypotenuse of any right triangle. This is
clearly useful (though perhaps not as its own program).

The reason the letters *a*, *b*, and *c* are used in the formula is because
they *stand in* for whatever values some particular right triangle has. Our
program would ask for these values (specifically, the values for *a* and *b*)
from the user (or read them from a file, or whatever). Our program must also
save these values in variables.

However, unlike mathematics, C++ variables are *typed*.  This means that every
variable in C++ is a variable of a certain type. We have the following types
available to us:

- `int` for integers (..., -2, -1, 0, 1, 2, ...)

- `double` for "floating point" values (i.e. decimal values like -0.5
  or 1.23040E-23)

- `string` for "strings" (i.e. text)
 
- `bool` for true/false values

- and some others...

Variables in C++ are typed for two reasons: (1) programs can be executed more
efficiently if the type is known beforehand and need not be determined based on
what kinds of values the user provides (technically, this advantage is only
gained because C++ is *statically* typed, which means the types of variables
never change while the program is executing); (2) the C++ compiler is able to
ensure that you are using variables the right way by checking if you are using
values of the right type. For example, trying to save a string inside an
integer type of variable results in an error.

## Overflow and underflow

Computers are finite machines, so they cannot store arbitrarily large values or
arbitrarily small values (i.e. teeny-tiny fractions). Most programs do not need
extremely large or small values, so very rarely is the fact that computers are
finite an issue. However, C++ types also have their limits, and some of these
limits need to be considered when writing certain kinds of programs.

For any general category of types such as integers, decimal numbers, etc. C++
has multiple specific types that you can choose from to use in your programs.
For example, `int` is not the only C++ type that stores integer values. There
is also `char`, `short`, `long`, `long long`, and possibly more depending on
the compiler. As well, `double` is not the only type that can store decimal
values. There is also `float` and `long double`. A quick comparison of these
types will help you decide which is appropriate for each variable in your
program.

## Integer types

- `char` can store values between -127 and 128 (signed) or between 0
  and 255 (unsigned)

- `short` can store values between -32767 and 32767 (signed) or
  between 0 and 65535 (unsigned)

- `int` can store values between -2147483647 and 2147483647 (signed)
  or between 0 and 4294967295

- `long` is the same as `int` (on many computers)

- `long long` can store values between -9223372036854775807 and
  9223372036854775807 (signed) or between 0 and 18446744073709551615
  (unsigned)

## Decimal ("floating point") types

- `float` can store values up to 3.40282E +/- 38, 6
  precision digits are kept

- `double` can store values up to 1.79769E +/- 308,
  15 precision digits are kept

- `long double` can store values up to 1.7E +/- 4932, 18 precision digits are kept

*Note about precision in floating point types*--The floating point
types do not represent values precisely. It is not a good idea to do
this:

{% highlight cpp %}
double x;
cin >> x;
if(x == 2.6) // NOT GOOD
{
    // blah...
}
{% endhighlight %}

The value 2.6 cannot be represented precisely, so even if the user inputs 2.6,
it may not be stored as 2.6. Thus we cannot test if the value is "exactly" 2.6.
We must check, instead, if the value is "close" to 2.6, using the following
approach:

{% highlight cpp %}
double x;
cin >> x;
if(x < 2.60001 && x > 2.59999) // YES, GOOD
{
    // blah...
}
{% endhighlight %}

For more gory details, see [this article](http://www.cygnus-software.com/papers/comparingfloats/comparingfloats.htm) or the [Wikipedia article](http://en.wikipedia.org/wiki/Floating_point).

## String type

A "string" is literally a collection of characters (`char`). C++ allows us to
type strings without resorting to creating lots of `char` variables. So the
string `"hello"` is literally the collection of `char` thingies `'h'` `'e'`
`'l'` `'l'` `'o'` (there is actually another special `char` at the end that
tells the computer the string is finished). The double-quotes `"` tell C++ we
are using a string; single quotes `'` tell C++ we are referring to a `char`
(which is only one thing, one symbol).

Before C++, in the C language, strings, as we use them in C++, were not
available. C++-style strings are much more convenient because you can easily
make them larger or smaller, ask how long they are, etc.

To use strings, you have to `#include <string>` Here is an example using a
`string`:

{% highlight cpp %}
#include <iostream>
#include <string>  // necessary to use strings
using namespace std;

int main()
{
    string myname = "Percival";

    cout << myname << endl;

    return 0;
}
{% endhighlight %}

## Character type

There is a single "character" type, the `char`. The `char` is the smallest
type; it only holds one byte (8 bits) of information, which is the smallest
unit of storage in any modern computer. This gives the `char` type 256 possible
values. We can think of each of these values as integers (either -127 to 128 or
0 to 255), and a lot of programmers use `char` variables to keep track of
small-valued integers.

However, our use of `char` will be mostly to keep track of *symbols* like
letters and numbers. When a user types a single symbol, and your code is
supposed to capture that symbol and check if it equals something or another,
then you want to use a `char`. Here is an example:

{% highlight cpp %}
char c;
cout << "Enter a symbol: ";
cin >> c;

if(c == 'x')
{
    cout << "You typed x!" << endl;
}
if(c == '!')
{
    cout << "You typed a bang!" << endl;
}
// etc...
{% endhighlight %}

Notice that when we check if a `char` variable equals some particular symbol,
we put the symbol in single quotes, like so: `'x'` or `'!'`.

The compiler actually translates something like `'x'` to the *integer* 120. Why
is *x* integer 120? The ASCII table defines the mapping. (See:
[asciitable.com](http://www.asciitable.com/))

If you knew which ASCII symbol you wanted (e.g., if you knew *x* was 120), then
you could set a `char` variable equal to an integer, and it would be the same
as setting the variable equal to the corresponding symbol.

{% highlight cpp %}
// these are equivalent assignments
char c = 'x';
char d = 120;
{% endhighlight %}

"Strings" are literally a collection of `char` values ("strung" together).

## Commonly-used types

Although you may know your program has variables that would work just fine as
type `short` (for example), it's not always the best idea to use `short`. The
type `short` is relatively uncommon. (Think about metric units: although you
could use centiliters, it's normal to switch to milliliters at that point.) The
common types (and what you are encouraged to use) are:

- True/False values: `bool` (named after George Boole)

- Integers: `int`

- Floating point: `double`

- Strings: `string`

Even with these large ranges of values, sometimes your variables may suffer
overflow or underflow. Overflow occurs when you try to store a value that is
too large for the type.  Underflow occurs when you try to store a value that is
too small for the type. Underflow only makes sense with decimal types: if you
try to store a very small number, say 0.0 followed by 5000 0's followed by a 1,
not even a `long double` can store that value. Overflow and underflow should be
rare for the programs we are interested in, however.

> The laws we have to examine are the laws of one of the most
> important of the mental facilities. The mathematics we have to
> construct are the mathematics of the human intellect. -- George
> Boole

## Variable naming

Here are the rules for naming variables:

- 1 to 255 characters

- must begin with a letter or `_`

- after the first letter or `_`, can contain numbers

- uppercase and lowercase letters are considered different (e.g. `xyz`
  is not the same variable as `Xyz`)

- "reserved" words cannot be used (e.g. "using" "namespace" "if"
  "else" etc.)

- do not use all uppercase letters unless the variable is a
  "constant" ("constant" means its value never changes; e.g.
  `const double PI = 3.14159` is a proper use of all
  uppercase letters)

## Declaring variables

You can declare multiple variables in one line (separated by commas), if they
are all of the same type:

{% highlight cpp %}
double a, b, c, d, e, f, g;
int x, y, z;
{% endhighlight %}

You can also give them values:

{% highlight cpp %}
double a = 1.1, b = 2.2, c = 3.3;
int x = 1, y = 2, z = 3;
{% endhighlight %}

Declare and define constant values like this (using all caps for the variable
names):

{% highlight cpp %}
const double PI = 3.14159;
const int MULTIPLIER = 5;
{% endhighlight %}

Because these variables have the modifier `const`, you are not allowed to
change their values (the compiler will display an error if you try).

## Variable scope

The "scope" of a variable is the locations in your program where the
variable "exists" and is accessible. Basically, the rule is: a
variable is accessible below its creation and inside any deeper
"blocks" (blocks are defined by `{` and `}`), but not outside the
block in which it is defined. A variable can be "shadowed" or
overridden if a new variable of the same name is created in a deeper
block. A variable of the same name cannot be created inside the same
block as the earlier variable.

Here is an example:

{% highlight cpp %}
{
    cout << "hello" << endl;  // x does not exist
    int x;                    // now x exists
    x = 5;
    cout << x << endl;
    {
        x = 12;               // x still exists here
        cout << x << endl;
        {
            double x = 5.2;   // new x variable, original "shadowed"
            cout << x << endl;
        }
        cout << x << endl;    // original is back
    }
    cout << x << endl;        // still original
}
cout << "goodbye" << endl;    // no more x
{% endhighlight %}

