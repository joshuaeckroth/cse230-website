---
title: Homework 3
layout: default
---

From the book, page 440, question 13.

The mathematician [John Horton
Conway](http://en.wikipedia.org/wiki/John_Horton_Conway) invented the "Game of
Life" (not the board game). Though not a "game" in any traditional sense, it
provides interesting behavior that is specified with only a few rules. This
homework asks you to write a program that allows you to specify an initial
configuration. The program follows the rules of LIFE to show the continuing
behavior of the configuration.

LIFE is an organism that lives in a discrete, two-dimensional world. While this
world is actually unlimited, we don't have that luxury, so we restrict the
array to 80 characters wide by 22 character positions high. If you have access
to a larger screen, by all means use it. It is your choice to make the edges
"wrap around" or not.

This world is an array with each cell capable of holding one LIFE cell.
Generations mark the passing of time. Each generation brings births and deaths
to the LIFE community. The births and deaths follow the following set of rules.

* We define each cell to have eight *neighbor* cells. The neighbors of a cell
are the cells directly above, below, to the right, to the left, diagonally
above to the right and left, and diagonally below to the right and left. Be
careful when checking for neighbors on the edges; you can decide whether an
edge cell has alive or dead neighbors beyond the edge.

* If an occupied cell has zero or one neighbors, it dies of *loneliness*. If an
occupied cell has more than three neighbors, it dies of *overcrowding*.

* If an empty cell has exactly three occupied neighbor cells, there is a
*birth* of a new cell to replace the empty cell.

* Births and deaths are instantaneous and occur at the changes of generation. A
cell dying for whatever reason may help cause birth, but a new born cell cannot
resurrect a cell that is dying, nor will a cell's death prevent the death of
another, say, by reducing the local population.

*Notes:* Some configurations grow from relatively small starting
configurations. Others move across the region. It is recommended that for text
output you use a rectangular array of `char` with 80 columns and 22 rows to
store the LIFE world's successive generations. Use an asterisk `*` to indicate
a living cell, and use a blank (space) to indicate an empty (or dead) cell.

Examples:

<pre>
***
</pre>
becomes
<pre>
*
*
*
</pre>
then becomes
<pre>
***
</pre>
again, and so on.

You will "hard code" a starting configuration. The user need not be able to
provide a different starting configuration (that would just complicate your
program).

*Suggestions:* Look for stable configurations. That is, look for communities
that repeat patterns continually. The number of configurations in the
repetition is called the *period*. There are configurations that are fixed,
which continue without change. A possible project is to find such
configurations.

*Hints:* Define a `void` function named `generation` that takes the array we
call `world`, an 80-column by 22-row array of `char`, which contains the
initial configuration. The function scans the array and modifies the cells,
marking the cells with births and deaths in accord with the rules listed
earlier. This involves examining each cell in turn, either killing the cell,
letting it live, or if the cell is empty, deciding whether a cell should be
born. There should be a function `display` that accepts the array `world` and
displays the array on the screen. Some sort of time delay is appropriate
between calls to `generation` and `display`. To do this, your program should
generate and display the next generation when you press Return/Enter. You are
at liberty to automate this (put in a real time delay, and not wait for the
user to press a key), but automation is not necessary for the program.

If you want to "delay" the display of your grid, rather than wait for the user
to type something and press enter before displaying the next grid, then you
will need to pause or "sleep" your program somehow. If you are using Microsoft
Windows, do this:

{% highlight cpp %}
#include <windows.h>

// ...

Sleep(800); // pause 800 ms
{% endhighlight %}

On Linux or Mac OS X, do this:

{% highlight cpp %}
#include <unistd.h>

// ...

usleep(800000); // pause 800 ms
{% endhighlight %}

Go to
[http://www.qotile.net/blog/wp/?p=600](http://www.qotile.net/blog/wp/?p=600) to
see some cool 3D visualizations of the game over time. This site
[http://www.earslap.com/projectslab/otomata](http://www.earslap.com/projectslab/otomata)
plays music based on the Game of Life. A ridiculous amount of further
information is available here:
[http://www.conwaylife.com/wiki/index.php?title=Main_Page](http://www.conwaylife.com/wiki/index.php?title=Main_Page) 
