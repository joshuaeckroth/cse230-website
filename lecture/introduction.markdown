---
layout: default
title: Introduction
---

It is standard practice that the first program you write in a new programming
language should simply print the message "Hello, world!"

## Hello, world!

{% highlight cpp %}
#include <iostream>
using namespace std;

int main()
{
    cout << "Hello, world!" << endl;
    return 0;
}
{% endhighlight %}

<a href="http://www.kyon.pl/img/15506,next.html">
![Hello, world!](/images/hello-world.jpg "Hello, world!")
</a>

## A second C++ program

Now with that out of the way, here is a slightly more useful program that
computes the area of a rectangle.

{% highlight cpp %}
// rectangle_area.cpp
// Compute the area of a rectangle.
#include <iostream>
using namespace std;
int main() {
    int length, width, area;
    length = 12; // length in inches
    width = 6; // width in inches
    area = length * width;
    cout << "Area of a rectangle with ";
    cout << "length = " << length << " and width = " << width;
    cout << " is " << area;
    return 0;
}
{% endhighlight %}

When this program is run, the computer screen will display

<pre>
Area of a rectangle with length = 12 and width = 6 is 72
</pre>

We'll discuss most of that code in the following sections of these notes.

## Comments

Text that follows two double slashes (`//`) is known as a comment.

Comments do not affect how a program runs. They are used to explain various
aspects of a program to any person reading the program.

The previous program has 4 comments; the first gives the name of the file
containing the program, and the second briefly describes what the program does.
The other two comments say that the measurements are in inches.

## "include" statements

An `#include` statement is a "compiler directive" that tells the compiler to
make a specified file available to a program. For example, in both of these
programs, `#include <iostream>` tells the compiler to include the iostream
file, which is needed for programs that either input data from the keyboard or
write output on the screen.

`#include <xyz>` literally causes the file `xyz` (whichever file that
happens to be) to be copied-and-pasted (included) in place of the directive
`#include <xyz>`. We put the include directives at the top of our code
files so that those "included" files are pasted at the top of our code. The
files we are including typically have definitions of functions and so on, which
need to appear at the top of our code.

## Namespaces

`using namespace std;` -- this line informs the compiler that the files to be
used (from the "Standard" library, brought in by the "include") are in a
special region known as the namespace `std`. A namespace is just a name
attached to a large amount of code, to keep things organized especially as
programs get very large and spread across several teams and organizations.

## The "main" function

Our program consists of an `int` function called `main`. We can think of this
function as the main control center for program execution.

Every program must have a function called main. When *any* program written in
C++ is started, the `main` function is activated. No other functions are
activated unless they are activated from `main` or some other function that
`main` has activated, etc. It all begins in `main`.

The body of main is enclosed in curly braces and has a final statement: `return
0;` which signifies the completion of the function and returns control back to
the operating system (i.e., the program quits when the "return" in `main` is
reached).

## What happened in the second program?

The statement `int length, width, area;` creates three named `int` (integer)
memory cells ("variables") with no specific initial values.

The statement `length = 12;` assigns the value 12 to the memory cell named
`length`.  Likewise, the statement `width = 6;` assigns 6 to the cell named
`width`.

The statement `area = length * width;` gets the contents of the memory cells
named `length` and `width`, then multiplies these two values and stores the
result in the memory cell named `area`.

The `cout` (pronounced "see-out") statements produce the output that appears on
the computer screen.

The `return 0;` statement returns control of the program back to the computer's
operating system.

