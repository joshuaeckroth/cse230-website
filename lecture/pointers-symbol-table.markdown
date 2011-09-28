---
title: Pointers and the symbol table
layout: default
---

Although all we have learned about C++ up to this point is how to
create and use variables, it may be highly useful to us to think about
how the computer keeps track of information about
variables. Understanding this now will help us understand everything
that follows.

Imagine we have the following (simplistic) program:

{% highlight cpp %}
#include <iostream>
#include <cmath>
using namespace std;

int main()
{
    double a, b;
    cout << "Enter triangle lengths a and b: ";
    cin >> a >> b;
    double c = sqrt(a*a + b*b);
    cout << "Side c has length: " << c << endl;

    int n;
    cout << "Enter an integer: ";
    cin >> n;
    int sum = (n * (n+1)) / 2;
    cout << "Sum of integers from 1 to "
         << n << " is: " << sum << endl;

    return 0;
}
{% endhighlight %}

This program has five variables: `a, b, c, n, sum`. The first three
have the "type" `double` while the latter two have the type
`int`. Also, these variables hold particular values at particular
times (obviously).

How does the computer keep track of this information? It uses a symbol
table.

## Symbol tables

For purposes of this class, a symbol table has a row for each
variable, and several columns describing the variable. Here is an
example (using the variables above):

<table>
<thead>
<tr>
  <th>Variable name</th>
  <th>Scope</th>
  <th>Type</th>
  <th>Memory location</th>
</tr>
</thead>
<tbody>
<tr>
  <td><code>a</code></td>
  <td>top to bottom of <code>main()</code> function</td>
  <td><code>double</code></td>
  <td>0x38235628</td>
</tr>
<tr>
  <td><code>b</code></td>
  <td>top to bottom of <code>main()</code> function</td>
  <td><code>double</code></td>
  <td>0x89f83a4b</td>
</tr>
<tr>
  <td><code>c</code></td>
  <td>middle to bottom of <code>main()</code> function</td>
  <td><code>double</code></td>
  <td>0xbf732acd</td>
</tr>
<tr>
  <td><code>n</code></td>
  <td>middle to bottom of <code>main()</code> function</td>
  <td><code>int</code></td>
  <td>0xd64ca84b</td>
</tr>
<tr>
  <td><code>sum</code></td>
  <td>bottom of <code>main()</code> function</td>
  <td><code>int</code></td>
  <td>0x8fff5321</td>
</tr>
</table>

The "variable name" column is obvious, as is the "type" column. The
"scope" columns means "where is this variable visible?" We'll talk
more about scope later. The "memory location" column holds a number
(written in hexadecimal notation: each digit has 16 possible values,
using symbols 0-9 and a-f) which indicates where, in the computer's
memory, the value of the variable is kept.

It's *very important* to see that the *value* of the variable is not
in the symbol table! Only the memory location of where the value is
kept is in the symbol table. Since different kinds of variables have
different sizes (`double` types hold more data than `char` types, for
example), the symbol table would not be a simple, compact table of
information but rather a complicated, varying-size table of
data. That's not what we want. The symbol table stays simple and
compact if we only record where, in memory, the data is kept, and
don't actually keep the data in the symbol table.

Note that the memory location is the *starting location* of the
variable's data. A memory location names a particular byte of memory;
a `double`, for example, has 8 bytes so the memory location would just
say where the first byte is stored (the other 7 bytes can be obtained
by adding 1, 2, 3, etc. to the memory location).

## Pointers

Pointers are a very easy concept but can be very tricky to use
correctly. The easy aspect of pointers will be presented now, so that
you may be better prepared later learn how to use them correctly.

Say we want to refer to a variable by a different name. Using the same
symbol table above, say we want to modify the value of `n` but don't
want to use the name `n`. How do we modify `n`'s value?

We can create a new variable that "points to" `n`'s value. This is a
pointer. Since `n` is an integer, we want an integer pointer, written
`int*`. Let's make that pointer:

{% highlight cpp %}
int* pn;
{% endhighlight %}

I call it `pn` because it will point to `n`'s value. Right now,
however, it doesn't point to `n`'s value. We see from the symbol table
that `n`'s value is at memory location 0xd64ca84b. But we can't just
write that number in our code, because that number (that memory
location) will probably change every time we run our program. So let's
just ask for `n`'s memory location:

{% highlight cpp %}
int* pn = &n;  // ask for n's memory location, save it in the variable pn
{% endhighlight %}

The `&` ("address of") symbol gives us a variable's memory location.

Now, `pn` "points to" `n`'s value. We can change `n`'s value by
"dereferencing" the pointer:

{% highlight cpp %}
*pn = 5;

// is the same as:
n = 5;
{% endhighlight %}

The `*` ("dereference") symbol, when it's not attached to a type (like
`int*`), means in so many words, "look at the memory location stored
in this variable (`pn`), *go to that memory location* and change the
data there to this other data (`5`)."

In this example, it's the same operation as `n = 5`.

## Pointers in the symbol table!

A pointer (like `pn` above) is a variable. So, information about it is
kept in the symbol table:

<table>
<thead>
<tr>
  <th>Variable name</th>
  <th>Scope</th>
  <th>Type</th>
  <th>Memory location</th>
</tr>
</thead>
<tbody>
<tr>
  <td><code>a</code></td>
  <td>top to bottom of <code>main()</code> function</td>
  <td><code>double</code></td>
  <td>0x38235628</td>
</tr>
<tr>
  <td><code>b</code></td>
  <td>top to bottom of <code>main()</code> function</td>
  <td><code>double</code></td>
  <td>0x89f83a4b</td>
</tr>
<tr>
  <td><code>c</code></td>
  <td>middle to bottom of <code>main()</code> function</td>
  <td><code>double</code></td>
  <td>0xbf732acd</td>
</tr>
<tr>
  <td><code>n</code></td>
  <td>middle to bottom of <code>main()</code> function</td>
  <td><code>int</code></td>
  <td>0xd64ca84b</td>
</tr>
<tr>
  <td><code>sum</code></td>
  <td>bottom of <code>main()</code> function</td>
  <td><code>int</code></td>
  <td>0x8fff5321</td>
</tr>
<tr>
  <td><code>pn</code></td>
  <td>bottom of <code>main()</code> function</td>
  <td><code>int*</code></td>
  <td>0x99267dac</td>
</tr>
</table>

Here it gets a little tricky. Since `pn` is a variable, it has a
value. What is its value? It is 0xd64ca84b. Where is that value kept?
It's kept in memory of course! (like all values) *Where in memory?* At
this location: 0x99267dac.

(Note that all pointers have the same amount of data (32 bits on an
older computer, 64 bits on a new computer). This is because no matter
if you're "pointing to" a `double` or `int` or `char` or whatever, all
memory locations are of the same type (32 bits or 64 bits).)

Since a pointer is a variable, too, you can point to a pointer:

{% highlight cpp %}
int** ppn = &pn;
{% endhighlight %}

And so on, ad nauseum. The lesson is, pointers are not magical,
they're just variables with values that can be used in a particularly
useful way. And we use pointers by asking for the memory location of
another variable using the `&` symbol, and "going to" a memory
location and changing the data there using the `*` symbol.

Naturally, this is all *pointless* until we start solving problems
that truly require pointers. We'll see that in <a
href="/lecture/linked-lists.html">Linked lists</a>, if not sooner.

## Minimal example

This example was shown in class.

{% highlight cpp %}
#include <iostream>
using namespace std;

int main()
{
    int n;
    cout << "Enter value for n: ";
    cin >> n;

    cout << "n = " << n << endl;
    cout << "n's memory address is: " << &n << endl;

    int *pn;
    pn = &n;

    cout << "pn = " << pn << endl;
    cout << "*pn = " << *pn << endl;

    *pn = 5;

    cout << "*pn = " << *pn << endl;
    cout << "pn = " << pn << endl;
    cout << "n = " << n << endl;

    return 0;
}
{% endhighlight %}

Output:

<pre>
Enter value for n: 14
n = 14
n's memory address is: 0xbf868ea8
pn = 0xbf868ea8
*pn = 14
*pn = 5
pn = 0xbf868ea8
n = 5
</pre>
