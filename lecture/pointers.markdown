---
title: Pointers
layout: default
---

<div id="toc">
[TOC]
</div>

C++ is a somewhat "low-level" language because it allows (encourages) mucking
around with memory addresses and setting particular memory locations to
particular values. This is an extremely powerful facility, but (arguably) it
makes programming more difficult, since each pointer is a kind of
*indirection*. A pointer "points to" data rather than represents the data
itself.

Every variable (e.g. `int x = 14`) has its data somewhere in memory (some
memory location has the value 14). The location of that value is the memory
"address" where the value is kept. Memory is linear, so we only need one number
to represent this address.

Imagine the value for `x` is at memory address 1900. Since an integer is
typically 4-bytes long, the memory used to keep the value of `x` spans from
1900 to 1903. The computer knows how big an integer is, so we can simply say
the value of `x` is at memory address 1900.

## The reference `&` operator

Our introduction to pointers is with the `&` "reference" operator. Using `&` we
can get the memory address of a variable.

{% highlight cpp %}
int x = 14;
cout << &x << endl;
{% endhighlight %}

The first time I ran this code, I saw this:

<pre>
0xbfb3956c
</pre>

I ran it again, and saw this:

<pre>
0xbff6feac
</pre>

These are memory addresses (shown in hexidecimal format rather than decimal
format). It shows that the location of the variable `x` was different for two
different executions of the code. This is to be expected: the operating system
chooses which memory locations to use, so we can never be sure what the memory
locations will be. We use the `&` operator to find out a variable's location.

<a href="http://xkcd.com/138/">
![xkcd comic](/images/xkcd-pointers.png "xkcd comic")
</a>


Learning the address of a variable is useless if we don't also have *pointers*
and the *dereference operator*.

## Declaring pointers

A pointer is sort of like a new type of variable. Here is an integer pointer
variable:

{% highlight cpp %}
int *px;
{% endhighlight %}

Here `px` is a pointer. It will be used to "point to" an integer. I can give it
the value of a memory address; that address should be where some integer value
is stored.

Note that this is probably not going to work:

{% highlight cpp %}
int *px = 3700;
{% endhighlight %}

We have no reason to believe that memory location 3700 will have the value of
some particular integer. We just don't know where variables will be kept in
memory, so we can't set pointers to specific values. Instead, we have to get
the address from the reference operator:

{% highlight cpp %}
int x = 14;
int *px = &x;
{% endhighlight %}

Now `px` points to the memory location of the variable `x`, which has the value
14 at this time.

## The dereference `*` operator

The `*` used above was not the dereference operator, it was used to say that
`px` is an integer pointer variable (pointers are variables, too; their values
are stored somewhere in memory and their values can be changed; their values
can also be pointed to, which would be a pointer to a pointer to an integer).

We use the `*` dereference operator to treat a pointer variable as an actual
variable:

{% highlight cpp %}
*px = 15;
{% endhighlight %}

The code `*px` means "go to the memory location pointed to by `px`", so `*px =
15` means "go to the memory location pointed to by `px` and set the value
*there* to 15."

Since `x` is a variable whose value is at the same location that `px` points
to, the value of `x` simultaneously changed to 15.


> **dereference** *v.* To trace, with increasing horror, the ultimate object
> (also called the pointee) being pointed at by a chain of linked pointers.
>
> In C++, the simple rule is: reference by adding an `&` and
> dereference by removing an `*`. --- *The computer contradictionary*


## Pointers as parameters to functions

The C language (which came before C++) didn't have "call-by-reference," so in
order to make a function that could change the values of its arguments,
pointers were used:

{% highlight cpp %}
void changeValues(int *px, int *py)
{
    // add one to each variable pointed to by the parameters
    *px = *px + 1;
    *py = *py + 1;
}
{% endhighlight %}

C++ can do the same thing (although how the function is used must change):

{% highlight cpp %}
void changeValues2(int &x, int &y)
{
    // add one to each variable
    x = x + 1;
    y = y + 1;
}
{% endhighlight %}

These are equivalent except in how they are used. Here is how the C version
(which uses pointers) is used:

{% highlight cpp %}
int x = 5, y = 8;
changeValues(&x, &y);
{% endhighlight %}

Note that you have to provide the "memory location" of the variables `x` and
`y` to use the function that has pointer parameters.

The C++ version (call-by-reference) can be used in a more straight-forward
manner:

{% highlight cpp %}
int x = 5, y = 8;
changeValues2(x, y);
{% endhighlight %}

This is why call-by-reference is useful; it makes the code a little simpler,
but has the same effect.

## The NULL pointer

Since any memory address (e.g. 1900, 3720446, whatever) may well be a valid
memory address, how do we indicate that a pointer points to nothing? We have
designated that the address 0 is an invalid address. There is data at address
0, but there's no chance that our little C++ program has legitimate access to
that address (the operating system manages stuff at the very early areas of
memory).

When do we want a pointer that points to nothing? Pointers are very common in
complex data structures; for example, a "linked list" (which we'll learn about
later) is composed of values and pointers; each pointer points to the next
value in the list. So, the last pointer should point to nothing (there is no
next value). Thus, that last pointer equals 0.

A lot of people write `px = 0` to point to address 0. Most C++ compilers also
let us write `px = NULL` (NULL is the same as 0) to make it quite clear in the
code that `px` points to nothing.

If a pointer points to an invalid location (a memory location not accessible by
our program), and that pointer is dereferenced, the program will crash with a
"segmentation fault."

{% highlight cpp %}
int *px = NULL;
cout << *px << endl; // crashes the program
{% endhighlight %}


<a href="http://xkcd.com/371/">
![xkcd comic](/images/xkcd-compiler-complaint.png "xkcd comic")
</a>

## Array variables "are" pointers

Whenever an array is created,

{% highlight cpp %}
int xs[5] = {3, 6, 1, 3, 0};
{% endhighlight %}

the values are stored in memory, and the location of the first value is stored
in the *pointer variable* `xs`. So `xs` is truly a pointer, but acts as an
array.

This means a 2D array is an array of pointers (in the first dimension) and
several arrays of values (in the second dimension). Thus, if we have

{% highlight cpp %}
int xs[3][2] = { {5, 6}, {2, 7}, {-1, -2} };
{% endhighlight %}

then `xs` is really a pointer to an array of pointers. `xs[0]` is a pointer; it
points to an array containing values 5 and 6. `xs[1]` is another pointer,
pointing to the array of values 2 and 7, etc. That makes `xs` a pointer to an
array of pointers.

Note that when we make arrays like this, we can't change the pointers. The
pointers are constant, so `xs = &something` is not allowed.

## Pointer arithmetic

The syntax `[]` is a shorthand for pointer arithmetic. What happens if we add 1
to a pointer (e.g. `px++`)? The memory address that is stored in `px` increases
by the count of the bytes used to store the type of thing that `px` points to.
So if `px` points to an integer, then `px++` increases `px` by 4 (since an
integer uses 4 bytes).

When an array is stored in memory, its values are stored consecutively. Thus we
can use pointer arithmetic to access array elements:

{% highlight cpp %}
int xs[5] = {3, 6, 1, 3, 0};

// equivalent statements
cout << xs[2] << endl;
cout << *(xs + 2) << endl;

// equivalent statements
xs[4] = 18;
*(xs + 4) = 18;
{% endhighlight %}

So we really don't need the `[]` syntax at all, but it is convenient.

## Conclusion

Any discussion of pointers is a bit esoteric without showcasing applications.
The real use for pointers will come when we discuss interesting data
structures, such as linked lists.


> **pointee** *n.* That, if anything, pointed at by a pointer.
>
> Many computer languages offer data types such as "pointer to data type T"
> where T itself can be a pointer type. Thus, pointees may well be pointers,
> yea even unto themselves. A pointer can be interpreted as the memory address
> of its pointee (the putative object residing at that place in memory). The
> devout hope, a sort of computer-scientific Calvinism, is that pointer and
> pointee values maintain this preordained relationship throughout the manifest
> volatilities that RAM and code are heir to. A symptom of widespread pointer
> paranoia is the fact that in C/C++, for example, zero-valued (or NULL)
> pointers are *non-grata*; they point *nowhere*, have no pointees, and noisily
> resist dereferencing. There is a growing backlash from the parsimonious who
> resent the fact that a perfectly respectable, physical byte at address 0 is
> pointlessly ghettoed. --- *The computer contradictionary*

