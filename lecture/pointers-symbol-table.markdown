---
title: Pointers and the symbol table
layout: default
---

In most C++ programming books and tutorials, "pointers" are introduced
much later than I am doing in these lecture notes. From experience, I
have found that it is better to experience pointers sooner rather than
later, because while the concept is simple and understandable early
on, their use can become quite complex.

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
"scope" columns means "where is this variable visible?" We learned
about scope in the
[variables and types](/lecture/variables-and-types.html) notes. The
"memory location" column holds a number (written in hexadecimal
notation: each digit has 16 possible values, using symbols 0-9 and
a-f) which indicates where, in the computer's memory, the value of the
variable is kept.

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

> A memory reference instruction which is to use an indirect address
> will have a `ONE` in Bit 5 of the instruction word. [...] Thus, `Y`
> is not the location of the operand but the location of the location
> of the operand.... -- *Programmed Data Processor-1 Manual*, 1960

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
pn = &n;  // ask for n's memory location, save it in the variable pn
{% endhighlight %}

The `&` ("address of" or "reference") operator gives us a variable's
memory location.

<a href="http://xkcd.com/138/">
![xkcd comic](/images/xkcd-pointers.png "xkcd comic")
</a>

Now, `pn` "points to" `n`'s value. We can change `n`'s value by
"dereferencing" the pointer:

{% highlight cpp %}
*pn = 5;

// is the same as:
n = 5;
{% endhighlight %}

The `*` ("dereference") operator, when it's not attached to a type
(like `int*`), means in so many words, "look at the memory location
stored in this variable (`pn`), *go to that memory location* and
change the data there to this other data (`5`)."

In this example, it's the same operation as `n = 5`.

If we are using classes, such as:

{% highlight cpp %}
class Person
{
public:
    string name;
    int age;       // in years
    double height; // in cm
    double weight; // in kg
};
{% endhighlight %}

then we can create pointers in the same way:

{% highlight cpp %}
int main()
{
    Person vignesh;
    vignesh.name = "Vignesh S.";
    vignesh.age = 25;
    vignesh.height = 177;
    vignesh.weight = 68;
    
    Person* p = &vignesh;
    
    cout << p->name << " weighs " << p->weight << " kg." << endl;
    
    return 0;
}
{% endhighlight %}

Notice that when we use classes and pointers together, we use the `->`
symbols to refer to data inside the class, rather than the `.` The
`.` is used if we are not using pointers.

> **dereference** *v.* To trace, with increasing horror, the ultimate object
> (also called the pointee) being pointed at by a chain of linked pointers.
>
> In C++, the simple rule is: reference by adding an `&` and
> dereference by removing an `*`. -- *The computer contradictionary*

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


## Reserving memory locations

We can create new variables without giving them names. Obviously, `int
x = 5` makes a variable called `x` and sets it equal to 5. But what if
we wanted to create variables in a loop, or variables that aren't
deleted behind our backs (when the variable's scope ends)? We use the
`new` operator:

{% highlight cpp %}
// reserve space for an integer, with no name
new int;
{% endhighlight %}

Well that does what we wanted (reserve some space for an integer) but
we have no way of using that reserved space. Why? Because we don't
know where that space is!

It turns out that the `new` operator actually returns a pointer (a
memory address) so we can save that and then use the pointer to put
values into the space that was reserved.

{% highlight cpp %}
// reserve space for an integer, with no name;
// but save the address of that space as px
int *px = new int;

// now put a value in that reserved space
*px = 5;
{% endhighlight %}

When we use `new` we should later use `delete` to free up the space.

{% highlight cpp %}
delete px;
{% endhighlight %}

Now that space is no longer ours to use (even though `px` still points
to it).  So we should only delete space when we are done with it.

The reason we need to "delete" space after we are done with it
(assuming we used `new` to reserve the space) is because the normal
scope rules don't apply to memory that is reserved with the `new`
operator.

Recall the rules of scope: If you have a variable `x` inside some
braces, like so:

{% highlight cpp %}
{
   int x = 5;
   ...
}
// x no longer exists (outside those braces)
{% endhighlight %}

then `x` is inaccessible (the variable is forgotten) when its
enclosing block (braces) ends. For example, functions have their own
sets of braces, so variables created inside functions no longer exist
after the functions are finished.

But if we use the `new` operator to reserve memory, then that memory
will be ours to use, regardless of scope, for as long as we wish
(until we say `delete`). We just have to keep track of the pointer
(address) of the memory that was reserved.

Note, this also works on classes:

{% highlight cpp %}
Person* p = new Person;
p->name = "Mary S.";
p->age = 45;
// etc...

// later:
delete p;
{% endhighlight %}

## An analogy (oh no!)

<a href="http://www.flickr.com/photos/guardianstoragesolutions/2714239269/in/photostream/">
![Storage units](/images/storage_units.jpg "Storage units")
</a>

This analogy is a bit long, but hopefully it is useful.

Consider self-storage units. Think of each storage unit as a little
hunk of memory -- you can put stuff (data) inside.

If the storage units are memory, then the unit number or "address" is
like a memory address. Imagein each unit has an address like "row 15,
unit 8."

To expand the analogy, we must figure out how to describe a C++
variable. A variable is a name which we use to refer to data. We don't
care where that data is stored, we prefer to use the name of the
data. Of course, the compiler is responsible for generating the
appropriate code that reads the data from memory, or saves the data
into memory, when we use the variable's name. In our analogy, perhaps
the manager of the self-storage business is responsible for grabbing
our stuff from the storage unit, and putting our stuff into the unit,
when we ask (which is a lot to ask of a manager, but oh well).

The manager keeps a list of variable names and storage unit addresses;
we don't care too much what's on this list (for the moment). Also,
this list is updated over time. Specifically, this list is updated
when we ask the manager to start using a new variable name. This list
is also updated when names are removed and storage units are freed up
(for other customers). Variable names are removed from the list when
they are "out of scope" in terms of our code.

Variables go "out of scope" for the convenience of the
programmer. "Scope" allows the programmer to reuse variable names and
to free up memory when it's no longer needed. Scope only affects
"temporary" variables, that is, variables created in the middle of
your code and that aren't needed in other parts of your code. If we
want variable names to persist, we can create them at the top of our
code (outside all "blocks"); such variables are referred to as
"global" variables because they are always in the current scope. Use
of too many "global" variables is bad practice because it's difficult
for programmers to keep track of all those variables. Rather, we like
the fact that variables go "out of scope" and are destroyed because
that means fewer variables are active at any one time. Ideally,
variables are created just before they are needed for some
computation, and are destroyed (go out of scope) when they are no
longer needed.

So, the manager's list of variable names and their corresponding
storage units is updated over time. It is not our responsibility to
update that list, but of course we know how the list is updated
because we know the rules of scope.

Naturally, in this analogy, a pointer is simply the address of a
storage unit, because in reality it's a memory location. We can obtain
a pointer by asking a variable what is its address (what storage unit
is it using to store its data) or by asking the manager to reserve a
new storage unit for us.

When we ask for a storage unit to be reserved, we are telling the
manager that we want to decide when we are done with the unit; we
don't want scope rules to apply. The manager gives us the address of a
unit, but the manager does not put any variable name on her list of
names (because variable names are *always* subjected to scope
rules). The manager will not decide when we are done with the unit;
that is our responsibility.

To reserve a storage unit, we use the `new` operation:

{% highlight cpp %}
new int;
{% endhighlight %}

(that code reserves four or eight bytes, enough for an integer)

That code is incomplete; the storage unit was reserved but we don't
have its address! Let's save its address:

{% highlight cpp %}
int* px = new int;
{% endhighlight %}

Ok, `px` holds the address of the reserved unit. We can put an integer
in this unit:

{% highlight cpp %}
*px = 52;
{% endhighlight %}

When we are done with the unit, we can `delete` it:

{% highlight cpp %}
delete px;
{% endhighlight %}

After we have deleted it, we should not attempt to access it (read or
write to that memory location) because there is a good chance it is
being used by some other customer (some other program, or some other
part of your program).

And on that same note, we should not attempt to access storage units
that we have not reserved or that our not associated with some
variable that is currently in scope. Any arbitrary storage unit may be
in use by another program. The computer will typically notice our
mistake, and crash our program. (It's just not friendly!)

Notice that the pointer `px` is a regular variable, so it is subject
to normal scope rules. We may lose the address of our reserved storage
unit if `px`, the variable, is destroyed because it goes out of
scope. If this occurs, and we never deleted (freed) the storage unit
that `px` points to, then we'll never be able to delete it, and it
will sit around forever (as long as our program is still running),
completely inaccessible but also still reserved. No other customer can
use that unit. This is called a "memory leak." A memory leak occurs
when you reserve memory space but forget to delete it (probably
because you lost the address of the unit you reserved).

> **Interesting note** A 2.5 acre self-storage facility can offer
> about 511 (10ft x 10ft) storage units. If we think of each 10x10
> storage unit as enough space to hold an integer (which we'll say is
> 8 bytes), then with 4GB of computer RAM, we'd need a storage
> facility that spans about 1.1281e16 acres, or the surface area of
> about 89,602 earths or about 7.5 suns. So when you lose the address
> of your storage unit (memory location), you have *really* lost any
> hope of finding it again...

Now, why use pointers? I'll offer two use cases.

The first is the "linked list." Imagine you want to store a lot of
data but you don't know how much, and data may be added and removed
over time. In C++, at least, you will not be able to continue creating
variables for this data, simply because you don't know how many
variables you'll need to create. (If you try to create a variable in a
loop, the variable will be destroyed when the loop repeats because its
scope ended. Oops!)

How would we solve this problem with storage units? It's quite simple
actually. In each of our units, each of which has some data, just
include a little piece of paper with the address of the next unit in
the list. Then, when you want to add data, you first reserve a new
unit (and temporarily remember its address). Then, find the last unit
in the list (by following the "bread crumbs" so to speak, until you
reach a unit with no "next" pointer), write the address of the newly
reserved unit on a piece of paper, and drop it in the last unit. Now
the newly-reserved unit is the new "last" unit, and the previous
"last" unit has a pointer to the new "last" unit.

All you need to keep track of (keep in "scope") is the address of the
first unit in the list. It's easy enough to keep one variable in
scope. It's impossible to create an unknown, growing number of
variables in scope.

Now the second use case. Suppose you wanted to create a maze that a
player can "walk" through. This is quite simple. Each storage unit
acts like a "room" in the maze (perhaps it's decorated like a room,
too). Storage units don't exactly have "doors," especially not "east,"
"west," ... doors. So, instead we write the locations of the storage
units that we are pretending are accessible through the "east,"
"west," ... doors of the room. If the player wants to move "west,"
your code just finds the piece of paper in the current room that is
labeled "west," reads the location off that piece of paper, and walks
over to that storage unit. By representing the maze this way (where
each room knows its own exits), we don't have to keep track of the
rooms and their exits in some huge complicated variable (such as a
matrix). We also don't have to worry much about scope; we only need to
keep track of the location of our "current room" (keep the "current
room" pointer in scope).

## Blinky pointer fun (no, really)

Check out this video: [Blinky pointer
fun](http://cslibrary.stanford.edu/104/) from Stanford University.

We'll review this video in class; if you're reading this from home,
you may want to look at the associated [Pointer
basics](http://cslibrary.stanford.edu/106/) document that explains
more what Blink pointer fun was all about.

## The NULL pointer

Since virtually any memory address (e.g. 1900, 3720446, whatever) may
well be a valid memory address, how do we indicate that a pointer
points to nothing? (A pointer "pointing to nothing" is useful when we
want to be clear that a pointer is no longer valid.) We have
designated that the address 0 is an invalid address. There is data at
address 0, but there's no chance that our little C++ program has
legitimate access to that address (the operating system manages stuff
at the very early areas of memory).

When do we want a pointer that points to nothing? Pointers are very
common in complex data structures; for example, a "linked list" (which
we'll learn about later) is composed of values and pointers; each
pointer points to the next value in the list. So, the last pointer
should point to nothing (there is no next value). Thus, that last
pointer equals 0.

A lot of people write `px = 0` to point to address 0. Most C++
compilers also let us write `px = NULL` (NULL is the same as 0) to
make it quite clear in the code that `px` points to nothing.

If a pointer points to an invalid location (a memory location not
accessible by our program), and that pointer is dereferenced, the
program will crash with a "segmentation fault."

{% highlight cpp %}
int* px = NULL;
cout << *px << endl; // crashes the program with a "segfault"
{% endhighlight %}


<a href="http://xkcd.com/371/">
![xkcd comic](/images/xkcd-compiler-complaint.png "xkcd comic")
</a>

## Conclusion

Any discussion of pointers is a bit esoteric without showcasing
applications.  The real use for pointers will come when we discuss
interesting data structures, such as linked lists.

> **pointee** *n.* That, if anything, pointed at by a pointer.
>
> Many computer languages offer data types such as "pointer to data
> type T" where T itself can be a pointer type. Thus, pointees may
> well be pointers, yea even unto themselves. A pointer can be
> interpreted as the memory address of its pointee (the putative
> object residing at that place in memory). The devout hope, a sort of
> computer-scientific Calvinism, is that pointer and pointee values
> maintain this preordained relationship throughout the manifest
> volatilities that RAM and code are heir to. A symptom of widespread
> pointer paranoia is the fact that in C/C++, for example, zero-valued
> (or NULL) pointers are *non-grata*; they point *nowhere*, have no
> pointees, and noisily resist dereferencing. There is a growing
> backlash from the parsimonious who resent the fact that a perfectly
> respectable, physical byte at address 0 is pointlessly ghettoed. --
> *The computer contradictionary*

