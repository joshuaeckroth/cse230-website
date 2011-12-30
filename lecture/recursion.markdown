---
layout: default
title: Recursion
---

<blockquote>
A child couldn't sleep, so her mother told her a story about a little frog,
<br/>
&nbsp; who couldn't sleep, so the frog's mother told her a story about a little bear,
<br/>
&nbsp; &nbsp; who couldn't sleep, so the bear's mother told her a story about a little weasel...
<br/>
&nbsp; &nbsp; &nbsp; who fell asleep.
<br/>
&nbsp; &nbsp; ...and the little bear fell asleep;
<br/>
&nbsp; ...and the little frog fell asleep;
<br/>
...and the child fell asleep.
</blockquote>

A recursive function (or self-recursive function) is a function that calls
itself. The concept is familiar to mathematicians, who define the factorial
function as the following: `n! = n*(n-1)!` with the special case that `0! = 1`.
Since the `!` is used in the right side of the equal sign (in the first
equation), the definition is recursive: it refers to itself. This may stink of
circularity, but so long as the recursive case (right side of the equals sign)
gets *closer* to the base case (`0! = 1`, which has no recursion), then it'll
all work out.

Here is our definition, in C++, of the factorial function just defined:

{% highlight cpp %}
int factorial(int n)
{
    if(n == 0)
    {
        return 1;
    }
    else
    {
        return (n * factorial(n - 1));
    }
}
{% endhighlight %}

Notice that the code matches perfectly with the mathematical definition. This
is quite nice, since it's clean and simple (like math, usually).

Anything that can be done with recursion can be done with regular loops, and
vice versa. Consider the simple algorithm of adding two numbers that involves
adding 1 each time:

{% highlight cpp %}
int a = 10, b = 21;
for(; b > 0; b--)
{
    a = a + 1;
}
{% endhighlight %}

That loop just adds one each time to `a`, and decreases 1 from `b` until `b` is
0. The result is the value of `b` is added to `a`. Not the most efficient
method of adding numbers, but it demonstrates an iterative method to perform
addition.

This same iterative method can be done recursively instead, using no loops:

{% highlight cpp %}
int add(int a, int b)
{
    if(b == 0)
    {
        return a;
    }
    else
    {
        return (1 + add(a, b - 1));
    }
}
{% endhighlight %}

So far, we have not seen the benefit of recursion. But it's important to note
that we can have recursion, and not have loops, but be able to accomplish all
the same tasks. In fact, some programming languages ("functional languages"
such as Lisp) sometimes completely abandon loops (no `while` or `for` loops)
and instead make recursion really easy. With recursion, you can do a task
repeatedly, if needed. But also with recursion, lots of hard problems become
much simpler.

> Recursion is the root of computation since it trades description for
> time. -- *Alan J. Perlis, Epigrams on Programming*

## Chess, Tic Tac Toe, Checkers, etc.

How does a robot play a board game? It uses recursion. Consider the following general technique for Chess:

- See if there is a winning move (check-mate move); if so, return the value "YES!".
- If not, make a list of possible moves.
- For each possible move, pretend to make that move, and start over checking for moves (recursive call).
- If any of the recursive calls returned "YES!", also return "YES!"
- Otherwise, return "NO :("

The result of this recursive procedure is that the best move, the move that
could eventually lead to a win (a "YES!"), is chosen. If no path results in a
good move, well, the robot's already lost the game (no move eventually leads to
a win), so it should resign. A "search tree" was constructed and searched; for
any "tree" in computer science, a recursive procedure is used to analyze it
because each branch of a tree looks like a new tree. Self-similarity of
different components of a problem domain is a prime reason recursion is used.

## Sorting

Another example of recursion is the Quicksort algorithm, which is used to sort
lists of things (numbers, names, etc.). Quicksort basically works as follows:

* Look at the current list: is there only one element? If so, it's already sorted (yay!).
* Otherwise, if there is more than one element, choose some random element called a *pivot* and remove it, then collect all the stuff in the list that's less than the pivot (make a new list) and all the stuff that's greater than the pivot (make another new list).
* Sort each of these smaller lists (using Quicksort; here is the recursion).
* Once those smaller lists are sorted, put the original list back together again like so: less-than list (which is now sorted) + pivot + greater-than list (which is now sorted). Now you're done!

It seems that the algorithm is almost too simple: where is the actual sorting
taking place? It's deceptively simple, especially compared to algorithms that
don't use recursion. Additionally, this algorithm is one of the fastest we know
about, and is used almost universally for sorting lists.

## The stack

When a function calls another function, it cannot proceed until that other
function has completed. So a recursive function call waits on the recursive
call to finish. Thus, if a function calls itself *n* times, then there are
*n-1* functions waiting around for their recursive calls to finish. These
waiting functions are pushed onto the "stack" (like a stack of plates, each
function waits on top of the other). The deepest function call is at the top of
the stack, and when that function finishes, it gets "popped off" the top of the
stack, so that the previous function can proceed on its way.

If a function calls itself recursively too many times, the stack (which is just
computer memory, and thus finite) may "overflow," and the whole program
crashes. So we need to be careful to write recursive algorithms that don't get
too deep (generally it takes thousands or more recursive calls to overflow the
stack).

> One of our great features of our compiler is that it happens to turn
> out that it is very easy to have a good recursive function in it. I
> am very fond of them. They are hardly used by
> customers. Nevertheless, it is very important that they are in. The
> reason is that they give us possibilities that make the tool
> inspiring. -- E. W. Dijkstra, 1962

## Picture-in-a-picture

> **Tortoise:** That's the word I was looking for! "POPPING-TONIC" is what it's
> called, and if you remember to carry a bottle of it in your right hand as you
> swallow the pushing-potion, it too will be pushed into the picture; then,
> whenever you get a hankering to "pop" back out into real life, you need only
> take a swallow of popping-tonic, and presto! You're back in the real world,
> exactly where you were before you pushed yourself in.

> **Achilles:** That sounds very interesting. What would happen it you took
> some popping-tonic without having previously pushed yourself into a picture?

> **Tortoise:**  I don't precisely know, Achilles, but I would be rather wary
> of horsing around with these strange pushing and popping liquids. Once I had
> a friend, a Weasel, who did precisely what you suggested--and no one has
> heard from him since.

> **Achilles:** That's unfortunate. Can you also carry along the bottle of
> pushing-potion with you?

> **Tortoise:** Oh, certainly. Just hold it in your left hand, and it too will
> get pushed right along with you into the picture you're looking at.

> **Achilles:** What happens if you then find a picture inside the picture
> which you have already entered, and take another swig of pushing-potion?

> **Tortoise:** Just what you would expect: you wind up inside that
> picture-in-a-picture.

> **Achilles:** I suppose that you have to pop twice, then, in order to
> extricate yourself from the nested pictures, and re-emerge back in real life.

> **Tortoise:** That's right. You have to pop once for each push, since a push
> takes you down inside a picture, and a pop undoes that.

> **Achilles:** You know, this all sounds pretty fishy to me... Are you sure
> you're not just testing the limits of my gullibility?

> **Tortoise:** I swear! Look-here are two phials, right here in my pocket. If
> you're willing, we can try them. What do you say? --
> *G&ouml;del, Escher, Bach: An eternal golden braid*

