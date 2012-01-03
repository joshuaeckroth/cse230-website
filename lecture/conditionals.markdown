---
layout: default
title: Conditionals
---

It is a very rare program that solves a problem with the exact same sequence of
operations every time it is executed. What is more common is a program that
does some operations if the data or user inputs look a certain way, and a
different set of operations if the data look a different way.

The way we can do this in C++ is with "conditionals." The word "conditional"
means that some operations will only occur if a certain "condition" is true.

For example, consider the program that gives the user three attempts to guess a
number between 1 and 10. Here is what the program should do (in pseudo-code):

- Ask the user for the first guess. Save this guess into a variable called
  `guess1`.
- If `guess1` is the correct guess, then tell the user "Success!" and quit.
- If `guess1` is not the correct guess, then tell the user "Nope.  Try another
  guess." and save the second guess into a variable called `guess2`.
- If `guess2` is the correct guess, then tell the user "Success!" and quit.
- If `guess2` is not the correct guess, then tell the user "Nope.  Try another
  guess." and save the third guess into a variable called `guess3`.
- If `guess3` is the correct guess, then tell the user "Success!" and quit.
- If `guess3` is not the correct guess, then tell the user "Nope.  You are not
  allowed any more guesses, sorry! The number you failed to guess was [the
  number]." (where "the number" is the real number).

## How to do it in code

In C++, we have a command called `if`. This is what it looks like:

{% highlight cpp %}
if(...something...)
{
    // do stuff...
}
{% endhighlight %}

Of course, we need to change the phrase `...something...` to a "conditional."
The if command expects something that results in a `bool` value. So we can use
`bool` values like `true` and `false`:

{% highlight cpp %}
if(true)
{
    // do stuff...
}
if(false)
{
    // do stuff...
}
{% endhighlight %}

But these conditionals (`true` and `false`) are not very interesting because in
the first case, the stuff inside the `if` block will always be executed (a
"block" is the stuff between `{` and `}`). In the second case, the stuff in the
block will never be executed.

More interesting conditionals may involve the following boolean operators
(assuming `p` and `q` are `bool` variables and `x` is an `int` variable):

- `!p` -- "not p" or "opposite of the value that p has"

- `p || q` -- "p or q"

- `p && q` -- "p and q"

- `x == 5` -- "the value in the variable x is equal to 5"

- `x != 5` -- "the value in the variable x is not equal to 5"

- `x < 5`

- `x > 5`

- `x <= 5` -- "the value in the variable x is less than or equal to 5"

- `x >= 5`

- `(x >= 0) && (x <= 5)` -- "x is between 0 and 5" (x can be one of
  [0,1,2,3,4,5]); note that you *cannot* say
  `(0 <= x <= 5)` (wrong!)

So we can write our guessing program like this:

{% highlight cpp %}
#include <iostream>
using namespace std;
int main()
{
    int answer = 8;
    int guess;
    
    cout << "Enter first guess: ";
    cin >> guess;

    if(answer == guess)
    {
        cout << "Success!";
        return 0;
    }
  
    cout << "Nope. Try another guess." << endl;
    cout << "Enter second guess: ";
    cin >> guess;
    
    if(answer == guess)
    {
        cout << "Success!";
        return 0;
    }
    
    cout << "Nope. Try another guess." << endl;
    cout << "Enter third guess: ";
    cin >> guess;

    if(answer == guess)
    {
        cout << "Success!";
        return 0;
    }
    
    cout << "Nope. You are not allowed any more guesses, sorry!"
         << endl;
    cout << "The number you failed to guess was " << answer
         << endl;
    
    return -1;
}
{% endhighlight %}


## if/else

Besides `if` we can also use `else` to specify some operations that should be
executed if the conditional is *not* satisfied. Example:

{% highlight cpp %}
if(x == 5)
{
    cout << "x equals 5!" << endl;
}
else
{
    cout << "x does not equal 5!" << endl;
}
{% endhighlight %}

## Nested if's

`if` commands can be inside other `if` (or `else`) blocks:

{% highlight cpp %}
if(x < 5)
{
    if(x < 0)
    {
        cout << "x is less than 0!" << endl;
    }
    else
    {
        cout << "x is less than 5 but not less than 0!" << endl;
    }
}
else
{
    cout << "x is not less than 5!" << endl;
}
{% endhighlight %}

## Series of if/else's

It is common practice to check a series of conditions, where you only expect
one of them to be true:

{% highlight cpp %}
if(x == 0)
{
    // do stuff...
}
else if(x == 1)
{
    // do stuff...
}
else if(x == 2)
{
    // do stuff...
}
else
{
    // fallback...
}
{% endhighlight %}

## An example of nested if's and an if/else-if chain

Task: Write a program that asks for three decimal numbers (*a*, *b*, *c*) and
prints a message indicating whether the quadratic equation *ax^2 + bx + c = 0* has no
solutions, one solution, two solutions, or no real solutions (i.e., only
imaginary solutions). Your program will need to make use of several `if`
statements. Nest the `if` statements appropriately so that you do not test for a
condition twice (e.g., you must check if *a* equals 0 only once, not five
times). Here is the algorithm:

- If *a* equals 0 and *b* equals 0, then there is no solution (output "No solution.").
- If *a* equals 0 and *b* does not equal 0, then there is one solution: *x = -c/b*.
- If *a* does not equal 0 and *b^2 - 4ac* equals 0, then there is one solution: *x = -b/(2a)*.
- If *a* does not equal 0 and *b^2 - 4ac* is greater than 0, then there are two solutions: *x = (-b - &radic;(d))/(2a)* and *x = (-b + &radic;(d))/(2a)*.
- If *a* does not equal 0 and *b^2 - 4ac* is less than 0, then there are no real solutions, i.e. two imaginary solutions.

Examples:

<pre>
Enter value 'a': 0
Enter value 'b': 0
Enter value 'c': 7
No solution.

Enter value 'a': 0
Enter value 'b': 0.5
Enter value 'c': 2
x = -4

Enter value 'a': 1
Enter value 'b': -6
Enter value 'c': 9
x = 3

Enter value 'a': 1
Enter value 'b': -3
Enter value 'c': 2
x = 1 OR x = 2

Enter value 'a': 1
Enter value 'b': 0
Enter value 'c': 1
No real solution.
</pre>

The code:

{% highlight cpp %}
#include <iostream>
#include <cmath>
using namespace std;

int main()
{
    double a, b, c, d;
    cout << "Enter value 'a': ";
    cin >> a;
    cout << "Enter value 'b': ";
    cin >> b;
    cout << "Enter value 'c': ";
    cin >> c;
    
    if(fabs(a) < 0.00001)
    {
        if(fabs(b) < 0.00001)
        {
            cout << "No solution." << endl;
        }
        else
        {
            cout << "x = " << (-c/b) << endl;
        }
    }
    else
    {
        d = pow(b, 2.0) - 4 * a * c;
        if(fabs(d) < 0.00001)
        {
            cout << "x = " << (-b/(2*a)) << endl;
        }
        else if(d >= 0.00001)
        {
            cout << "x = " << ((-b - sqrt(d))/(2*a)) << " OR "
                 << "x = " << ((-b + sqrt(d))/(2*a)) << endl;
        }
        else // i.e., d < 0.00001
        {
            cout << "No real solution." << endl;
        }
    }

    return 0;
}
{% endhighlight %}

## "switch" statements

An alternative series of if/else's is the `switch` statement. The
`switch` statement is the same as a series of if/else's because only
one of the conditions should turn out to be true. The `switch`
statement can only handle simple conditions, however; actually, it can
only handle conditions of the form "is some integer equal to some
value?" This means that a `switch` statement can only be used for
integers.

Here is an example.

{% highlight cpp %}
#include <iostream>
using namespace std;
int main()
{
    int number;
    cout << "Enter a number 1-4: ";
    cin >> number;
    
    switch(number)
    {
    case 1:
        cout << "You entered the number 1." << endl;
        break;
    case 2:
        cout << "You entered the number 2." << endl;
        break;
    case 3:
        cout << "You entered the number 3." << endl;
        break;
    case 4:
        cout << "You entered the number 4." << endl;
        break;
    default:
        cout << "You must have entered some other number."
             << endl;
    }
    
    return 0;
}
{% endhighlight %}

The `switch` statement above can be rewritten with a series of if/else's:

{% highlight cpp %}
// the following is the same as the "switch" statement above
if(number == 1)
{
    cout << "You entered the number 1." << endl;
}
else if(number == 2)
{
    cout << "You entered the number 2." << endl;
}
else if(number == 3)
{
    cout << "You entered the number 3." << endl;
}
else if(number == 4)
{
    cout << "You entered the number 4." << endl;
}
else
{
    cout << "You must have entered some other number."
         << endl;
}
{% endhighlight %}

Notice that an `if` is much more powerful: you can use very
complicated conditionals. On the other hand, a `switch` is very
simple: for each "case", you provide a value that is compared to the
variable used in the switch (in this case, the variable used in the
switch is "number"). Each case basically asks, "does the value in the
variable equal this number?"

Each "case" in a `switch` needs a "break" at the end so that the next
case is not entered. It's good practice to always include a "break" at
the end of each case in a `switch`.

The "default" case is the case that is used if all the other cases
fail.

You can also use a `switch` to determine which character a user typed,
since characters are themselves just integers. Here is an example:

{% highlight cpp %}
#include <iostream>
using namespace std;
int main()
{
    char letter;
    cout << "Enter a letter: ";
    cin >> letter;
    
    switch(letter)
    {
    case 'a':
        cout << "You entered the letter 'a'." << endl;
        break;
    case 'b':
        cout << "You entered the letter 'b'." << endl;
        break;
    default:
        cout << "You must have entered some other letter."
             << endl;
    }
    
    return 0;
}
{% endhighlight %}

## Another view of the "if" construct

It turns out that, in C and C++, any integer that's not zero is considered a
"true" value, and zero is considered the singular "false" value. Thus, these
are equivalent:

{% highlight cpp %}
int x = 5;
if(x != 0)
{
    cout << "x is not equal to zero." << endl;
}
if(x)
{
    cout << "x is not equal to zero." << endl;
}
{% endhighlight %}

Actually, the details are more gruesome than this. So it's best to avoid such
trickery.

> **if** *conj.* **1** (Poetic) Kiplingian milestones on the road to
> mandelayhood and gambler's ruin. **2** (CS) One of many conditional
> control-flow obstacles faced by a process linearly anxious to complete the
> next instruction without tiresome, trivial, testing diversions.
>
> Some programmers see `if` as the soul of machine intelligence. Thus,
{% highlight cpp %}
if(CurBankBal <= 0) errmsg("Spendthrift scoundrel!");
else exit(0);
{% endhighlight %}
>
> forms the core of many a smart, ethical money manager. The home-exit loving,
> non-judgmental process, however, reluctantly forced to test transient bits of
> registers, is tempted to rule in favor of `else`. Baudelaire predicts these
> nuances in *Les Hiboux*: "Sous les ifs noirs qui les abritent..." The `if`'s
> in procedural computer languages are followed by boolean expressions that
> reduce at runtime to `TRUE` or `FALSE`. Logical implication is replaced by
> material implication: `TRUE` implies that the `then` clause (possibly empty)
> is obeyed, while `FALSE` triggers the `else` clause (also possibly empty).
> Note that we lose the asymmetry of logical implication (true implies true,
> but false implies anything), and you are free to interchange `FALSE` and
> `TRUE`: they are essentially arbitrary attributes with no semantic
> significance. You are equally free to switch your `then` and `else` clauses.
>
> C is rather different. The interesting quirk with C, in the absence of a
> dedicated Boolean type, is that zero is Boolean `FALSE`, while nonzero is
> `TRUE`. An expression such as `(1 != 1)`, which most would regard as patently
> false, does indeed evaluate to zero; `(2 == 2)` evaluates to the integer 1,
> which is abundantly nonzero and eminently true.
>
> The interesting point is that `FALSE` is single-valued (I exclude the
> metaphysics of null pointers, huge, near, and far) while `TRUE` is legion.
> There may be a Zarathustrian strand here: the diabolical equivalence of all
> dark lies constrasting with a billion points of light? However, when you want
> a function to return `TRUE` for success and `FALSE` for failure, a perfectly
> natural, dare I say, intuitive ploy, you hit a snag. Success is a
> one-of-a-kind-blessing, failure comes in whole battalions. To have the
> function return an instructive error code, it is necessary to rephrase the
> question pessimistically: Did the function fail? Yes, `TRUE`! Why did it
> fail? Test the nonzero return value:
{% highlight cpp %}
if(e = f()) { // did f() fail?
  switch(e) { // why?
    case 1: // one reason for failure
      ...
    case 2: // and another
      ...
    case n: // no end?
      ...
    break;
  }
}
// success
{% endhighlight %}
>
> Some see life itself as a *huge* case statement. --
> *The computer contradictionary*

