---
layout: default
title: Arithmetic
---

Here are the normal math operators that work on integers and floating
point numbers:

  - `+` (add)

  - `-` (subtract; or a negative number, e.g. `int x = -5`)

  - `*` (multiply)

Here are somewhat-unique math operators that work with integers:

- `/` (quotient: `13 / 5` is 2 because 5 goes
  into 13 two times)

- `%` (remainder: `13 % 5` is 3 because the
  remainder of 5/13 is 2)

Note that `/` works as you would expect with floating point numbers
(e.g. `1.2 / 5.6` is about 0.214).

Precedence works the same as you would expect: `* / %`
happen before `+ -` so `(5 + 6 % 4 - (3 + 4) /
2)` equals 4 as you would expect. You can use parentheses to
clarify the math.


## Arithmetic shorthand

You can use the following shorthand for changing the values of
variables. The shorthand form is shown, then the equivalent form is
described in comments.

{% highlight cpp %}
double a = 0.0;
int x = 0;
x++;            // same as: x = x + 1;
x--;            // same as: x = x - 1;
x += 2;         // same as: x = x + 2;
x -= 2;         // same as: x = x - 2;
x *= 2;         // same as: x = x * 2;
x /= 2;         // same as: x = x / 2;
x %= 2;         // same as: x = x % 2;
    
a += 2.0;       // same as: a = a + 2.0;
// etc.
{% endhighlight %}

## Boolean operators

Variables of type `bool` have the following special operators:

{% highlight cpp %}
bool p = TRUE;
bool q = FALSE;
bool r;
r = !p;        // "!" means "not" or "opposite", so r == FALSE
r = p || q;    // "||" means "or", so r == TRUE
r = p && q;    // "&&" means "and", so r == FALSE
r = q || (!p)  // r == FALSE
{% endhighlight %}

Note that you cannot use "shorthand" operators with Boolean
variables. The following does *not* do what you might expect: `r != p;`

## Mathy example

This example shows use of several mathematical functions and
operators.

{% highlight cpp %}
// math example
#include <iostream>
#include <cmath>
using namespace std;

int main()
{
    cout << "The reciprocal of 10 is " << 1.0/10.0 << endl;
    cout << "The square root of 10 is " << sqrt(10.0) << endl;
    cout << "e^(10.0) = " << exp(10.0) << endl;
    cout << "The reciprocal of 15 is " << 1.0/15.0 << endl;
    cout << "The square root of 15 is " << sqrt(15.0) << endl;
    cout << "e^(15.0) = " << exp(15.0) << endl;

    return 0; // exit program
}
{% endhighlight %}

Here is the example again, but this time with a variable `x`:

{% highlight cpp %}
#include <iostream>
#include <cmath>
using namespace std;

int main()
{
    double x;
       
    x = 10.0;

    cout << "The reciprocal of 10 is " << 1.0/x << endl;
    cout << "The square root of 10 is " << sqrt(x) << endl;
    cout << "e^(" << x << ") = " << exp(x) << endl;

    x = 15.0;

    cout << "The reciprocal of 15 is " << 1.0/x << endl;
    cout << "The square root of 15 is " << sqrt(x) << endl;
    cout << "e^(" << x << ") = " << exp(x) << endl;

    return 0; // exit program
}
{% endhighlight %}

In these examples, various useful math functions were used. Here is a larger list. These all come from `cmath` (which must be "included").

* `pow(a, b)` --- raise `a` to the power `b`; `a` and `b` should have type `double`
* `exp(a)` --- raise Euler's number *e* to the power `a` (a `double`)
* `log(a)` --- find the natural logarithm of `a` (a `double`)
* `log10(a)` --- find the "base 10" logarithm of `a`
* `sqrt(a)` --- find the square root of `a` (a `double`)
* `sin(a)` --- the result of *sine(a)* (`a` is a `double`, in radians)
* `cos(a)`, `tan(a)` --- obvious
* `acos(a)`, `asin(a)` --- the arc cosine/sine of `a`; answer is in radians (`a` is a `double`, as is the answer)

## Conversion example

This code converts teaspoons to other units of measurement.

{% highlight cpp %}
#include <iostream>
using namespace std;

int main()
{
    int teaspoons;

    cout << "Please enter the number of teaspoons: ";
    cin >> teaspoons;

    int gallons = teaspoons/(3*16*2*8);
    teaspoons -= gallons*(3*16*2*8);
    int pints = teaspoons/(3*16*2);
    teaspoons -= pints*(3*16*2);
    int cups = teaspoons/(16*3);
    teaspoons -= cups*(16*3);
    int tablespoons = teaspoons/3;
    teaspoons -= tablespoons*3;

    cout << "Number of gallons: " << gallons << endl;
    cout << "Number of pints: " << pints << endl;
    cout << "Number of cups: " << cups << endl;
    cout << "Number of tablespoons: " << tablespoons << endl;
    cout << "Number of teaspoons: " << teaspoons << endl;

    return 0;
}
{% endhighlight %}

