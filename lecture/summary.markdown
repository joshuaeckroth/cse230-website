---
title: Summary of the whole class
layout: default
---

What follows is a quick review of all the material covered in the class, with a
little explanation of why it was covered.

## Variables and types

Essential to any program is the ability to retain values, to store calculations
in memory and retrieve from memory what has been stored. Values are identified
by names (unless we're using an array, in which case values are identified by a
name plus a position). These named values are called **variables**, and at
least in C++, each variable has a **type**. Types are useful so that we can
treat the *string* "11" differently than we treat the *integer* 11. If we have
two strings, "11" and "22", and they are "added" together, we get another
string: "1122". On the other hand, adding the integers 11 and 22 obviously
gives us 33. Thus, the type of a value determines how it behaves in various
operations. Also, the type determines how much space in memory is needed to
store the value: integers typically require 4 bytes, while a double value
(decimal number) typically requires 8 bytes.

## Arithmetic

Once we learned how to use variables, and we learned about some basic
**arithemetic functions** and other mathematical functions, we could perform
mathematical operations. However, the inputs to these functions should come
from the user of the program, and not be hard-coded into the source code. Thus,
some type of **input** mechanism was needed. Likewise, the results of the
operations should be shown back to the user, so some type of **output**
mechanism was needed. We used `cin` and `cout` for input and output
(respectively). Obviously, there are other ways to get input: from the mouse,
from a touchscreen, etc. And there are other ways to give output: 2D graphics,
3D graphics, sound, etc. `cin` and `cout` are very simple: input via
the keyboard, output via the screen in text format.

## Conditionals

Next, we saw that only a limited range of calculations can be performed with
variables and mathematical functions. Most interesting programs exhibit
different behavior depending on context or properties of the world (obtained
via input from the user). That is, most programs have **conditionals** littered
throughout. A conditional is a statement of the form "if XYZ is true, do this;
otherwise, do that." With conditionals, our programs can pursue one or more of
several possible paths of execution, thus making the program more flexible and
responsive to user input.

## Loops

The final piece of the computational puzzle is **loops**. Only by adding loops
to our repertoire can we perform computations repeatedly until some condition
is met (such as successive approximation problems). Also, a program with a
user-input loop is a fully interactive program: the user can have a
"conversation" with it that lasts as long as the user wishes it to last. Most
programs that we use every day have this looped conversational mode.  Your text
editor, your web browser, etc. all sit in some loop, waiting for your input.
And, as it happens, only by adding loops do we get the full power of
computation, the ability to solve any problem that can be solved by a computer.

## Functions

**Functions** were introduced so that we can keep our sanity. Simply put, a
function is a chunk of code with a name. That's convenient because we can refer
to that chunk of code by its name, and avoid resorting to copying-pasting code.
For example, the `sqrt()` function (square root) is presumably somewhat
complicated, and not something we want to copy-paste every time we need to
calculate a square root. Because it's a function, we can just refer to it by
name, essentially writing in our code, "Hey `sqrt()` function, go calculate the
square root of this number and give me back the result." Another way of looking
at functions is delegation: your main code delegates the minor tasks to other
functions so that your main code is not required to do all the work. A final
way of looking at functions is modularization: each function should solve a
small problem, a problem isolated from the greater program. Each function only
knows how to do one thing (ideally), and is not concerned with facts like when
it is used or why it is used (in what context). The `sqrt()` function performs
the same calculations regardless of when or why it is used. So, the code that
makes up the `sqrt()` function will not make any references to any greater
context, will not make reference to other variables or user input or output. By
breaking our code into isolated functions, we have introduced modularity
(separation of concerns, ability to easily replace modules, etc.).

One seemingly innocuous feature of C++ is the distinction between
**call-by-value** and **call-by-reference**. This distinction relates to how
functions receive their inputs and is only seemingly innocuous because the
distinction necessitates the use of pointers, which is a topic that we'll
broach later. Call-by-value is the "default" case. A function written in a
normal fashion receives a copy of its inputs' values:

{% highlight cpp %}
void printSum(int a, int b)
{
    a = a + b;
    cout << a << endl;
}

int main()
{
    int x = 4;
    int y = 10;
    printSum();
    cout << x << " " << y << endl;
    return 0;
}
{% endhighlight %}

In this example, `a` and `b` inside the function `printSum()` have the values 4
and 10, but `a` and `b` are distinct variables from `x` and `y`. Variables are
distinct if they do not share the same memory locations. So, `a` sits in a
different place in memory than does `x`. When the function is called, `x`'s
value is copied into `a`. Likewise for `y` and `b`. This is call-by-value, the
default situation. One effect of call-by-value in this code is that the value
of `x` does not change even though the value of `a` did change.

Now consider the following modified example:

{% highlight cpp %}
void printSum(int &a, int &b)
{
    a = a + b;
    cout << a << endl;
}

int main()
{
    int x = 4;
    int y = 10;
    printSum();
    cout << x << " " << y << endl;
    return 0;
}
{% endhighlight %}

The only change is the introduction of `&` before `a` and before `b` in the
function header. But that change turns a call-by-value function into a
call-by-reference function. Now, `x` and `a` are the same variable (they store
their data in the same locations in memory); likewise, `y` and `b` are the same
variable. So, the effect is that `x` is modified, because `a` is modified.

We use call-by-reference when we want a function to "return" several values
rather than just one. In this case, the "reference parameters" to the function
will be used to hold results. Also, call-by-reference can be more efficient
because values will not be copied (values may be very large, such as large
strings).

How this relates to pointers will be discussed shortly.

## Recursion

Next we learned about **recursion**. Recursion is a function calling itself, so
clearly we can't talk about recursion without first talking about functions.
Recursion was introduced because some problems can be solved more "cleanly"
with recursive procedures rather than non-recursive (iterative) procedures. For
example, one of the fastest "sorting algorithms" ("quick sort") is recursive:
given a list, it breaks the list into two parts, and then sorts the two smaller
lists. So the "quick sort" function performs "quick sort" on two smaller lists.
This is the divide-and-conquer technique that is often so effective (in
militaristic situations and programming). Coding divide-and-conquer techniques
without using recursion is a big mess.

Interestingly, some programming languages avoid iteration (e.g. `while()` loops
and `for()` loops) and instead use recursion to accomplish the same effect. In
C++'s perspective, recursion is a nice feature, when you need it.

## Arrays

Now, changing subjects, consider a program that asks the user to provide some
numbers, and then calculates the mean, variance, etc. of the numbers. How many
numbers will the user provide? The program does not know this. And, as it
happens, the variance and some other statistical properties of a list of
numbers cannot be calculated "as you go," as a stream. Rather, calculating the
variance requires calculating the mean (which can be done "as you go"), then
looking back at every number again and measuring how far away it is from the
mean. So variance cannot be calculated without having "all the numbers all at
once" in some type of collection. **Arrays** are one way to collect these
numbers.

An array is a single variable name (e.g. `vals` or `xs` or `numbers`) that has
lots of values inside. To refer to each particular value, we use a position or
index (starting at 0). So the fifth element in the array `vals` is at position
4, and can be identified by the syntax `vals[4]`.

Arrays are necessary for only one reason: C++ does not allow variables to be
created "on the fly." That is, we have no way, in C++, to write code that
introduces a variable called, say, `vals4`, while the program is running; all
variables must be created when the program is written, and those variables are
written in stone, so to speak. C++ allows us to *reserve more memory* for a
single *array* variable, if needed, but we cannot introduce *new* variables.

This restriction is almost splitting hairs: who cares if new variables can be
introduced or not? We get the same power by using arrays. This much is true,
but because new variables cannot be introduced, we are forced to use and
understand *pointers.*

## Pointers

The inability to introduce new variables is an effect of the inability to write
code that reasons *about* variables. There is no way, in C++, to write code
that asks "is this variable named 'x'?" or that can "change this variable's
name to 'y'" or "introduce a new variable called 'valsX' where X is the next
available number" or "give this function the variable named 'a'." We have no
way of talking about variables; in particular, we cannot refer to their names.

But a variable name is a unique identifier for a value. If we could write code
that talks about variable names, we could write code like "give this function
the variable named 'a'," and that code would make perfect sense because there
could only be one variable named 'a' (disregarding variable "scope"). But we
cannot do this, so how does code uniquely identify a variable?

To be clear, most code does not need to uniquely identify a variable. The `sqrt()`
function does not care where its input comes from, it simply cares what is the
value of the input. So we use `sqrt()` in a call-by-value situation, just
giving the values of the inputs to the function. However, other functions that
use `call-by-reference` need to know which *variable* is being given to the
function, not its value (the value may not even be used by the function; it may
only use the variable to assign it a value). C++ provides the call-by-reference
syntax (prepending the `&` to the variable name) to allow us to link two
distinct names to the same variable, in the context of calling a function.

However, there are other circumstances in which we want to refer to a variable.
For example, in a linked list, each "node" links to another node. Say we create
two nodes, `node1` and `node2`. We have no way of saying, "`node1` should link
to the variable called `node2`." So how to do link the nodes? How does the
variable `node1` ever "get to" or refer to the variable `node2` if not by its
name?

Since every variable lives in memory somewhere, a variable's memory location is
a unique identifier. And in C++, we *can* refer to memory locations. We use
**pointers** to do that. So, `node1` can remember the memory location, i.e. the
pointer to, `node2`, and never know the name of `node2`. The issue of uniquely
referring to variables has been resolved. It only required these cringe-worthy
pointers to do so!

(Coming soon: discussion of "classes" and object-oriented programming, and
"template meta-programming")

> It turns out that an eerie type of chaos can lurk just behind a
> fa&ccedil;ade of order -- and yet, deep inside the chaos lurks an
> even eerier type of order. -- *Douglas Hofstadter, Metamagical
> Themas*

