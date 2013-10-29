---
title: Homework 3
layout: default
---

From the book, page 440, question 13.

Skills needed to complete this assignment:

  - Using [conditionals](/lecture/conditionals.html) and
    [loops](/lecture/loops.html)

  - Writing [functions](/lecture/functions.html)

  - Storing values in [vectors](/lecture/vectors.html), and
    representing 2D matrices in 1D vectors

The mathematician
[John Horton Conway](http://en.wikipedia.org/wiki/John_Horton_Conway)
invented the "Game of Life" (not the board game). Though not a "game"
in any traditional sense, it provides interesting behavior that is
specified with only a few rules. This homework asks you to write a
program that allows you to specify an initial configuration. The
program follows the rules of LIFE to show the continuing behavior of
the configuration.

LIFE is an organism that lives in a discrete, two-dimensional
world. While this world is actually unlimited, we don't have that
luxury, so we restrict the world to 80 characters wide by 22 character
positions high. If you have access to a larger screen, by all means
use it. It is your choice to make the edges "wrap around" or not.

This world is a 1D or 2D vector (of `char` or `bool` perhaps) with
each cell capable of holding one LIFE cell. Generations mark the
passing of time. Each generation brings births and deaths to the LIFE
community. The births and deaths follow the following set of rules.

  * We define each cell to have eight *neighbor* cells. The neighbors
    of a cell are the cells directly above, below, to the right, to
    the left, diagonally above to the right and left, and diagonally
    below to the right and left. Be careful when checking for
    neighbors on the edges; you can decide whether an edge cell has
    alive or dead neighbors beyond the edge.

  * If an occupied cell has zero or one neighbors, it dies of
    *loneliness*. If an occupied cell has more than three neighbors,
    it dies of *overcrowding*.

  * If an empty cell has exactly three occupied neighbor cells, there
    is a *birth* of a new cell to replace the empty cell.

  * Births and deaths are instantaneous and occur at the changes of
    generation. A cell dying for whatever reason may help cause birth,
    but a new born cell cannot resurrect a cell that is dying, nor
    will a cell's death prevent the death of another, say, by reducing
    the local population.

Examples available from the article <a
href="http://www.ibiblio.org/lifepatterns/october1970.html">Scientific
American 223 (October 1970): p. 120-123</a>

These examples are stolen from <a
href="http://en.wikipedia.org/wiki/Conway%27s_Game_of_Life">Wikipedia</a>.

<div style="font-size: 80%">
  <table border="0" cellpadding="6"
         style="margin-left:auto;margin-right:auto; border: 0;">
    <tr style="border-bottom: 0;">
      <td>
        <table class="wikitable">
          <tr>
            <th colspan="2">Still lifes</th>
          </tr>
          <tr>
            <td>Block</td>
            <td><a href="http://www.wikipedia.org/wiki/File:Game_of_life_block_with_border.svg" class="image"><img alt="Game of life block with border.svg" src="//upload.wikimedia.org/wikipedia/commons/thumb/9/96/Game_of_life_block_with_border.svg/66px-Game_of_life_block_with_border.svg.png" width="66" height="66" /></a></td>
          </tr>
          <tr>
            <td>Beehive</td>
            <td><a href="http://www.wikipedia.org/wiki/File:Game_of_life_beehive.svg" class="image"><img alt="Game of life beehive.svg" src="//upload.wikimedia.org/wikipedia/commons/thumb/6/67/Game_of_life_beehive.svg/98px-Game_of_life_beehive.svg.png" width="98" height="82" /></a></td>
          </tr>
          <tr>
            <td>Loaf</td>
            <td><a href="http://www.wikipedia.org/wiki/File:Game_of_life_loaf.svg" class="image"><img alt="Game of life loaf.svg" src="//upload.wikimedia.org/wikipedia/commons/thumb/f/f4/Game_of_life_loaf.svg/98px-Game_of_life_loaf.svg.png" width="98" height="98" /></a></td>
          </tr>
          <tr>
            <td>Boat</td>
            <td><a href="http://www.wikipedia.org/wiki/File:Game_of_life_boat.svg" class="image"><img alt="Game of life boat.svg" src="//upload.wikimedia.org/wikipedia/commons/thumb/7/7f/Game_of_life_boat.svg/82px-Game_of_life_boat.svg.png" width="82" height="82" /></a></td>
          </tr>
        </table>
      </td>
      <td rowspan="2">
        <table class="wikitable">
          <tr>
            <th colspan="2">Oscillators</th>
          </tr>
          <tr>
            <td>Blinker</td>
            <td><a href="http://www.wikipedia.org/wiki/File:Game_of_life_blinker.gif" class="image"><img alt="Game of life blinker.gif" src="//upload.wikimedia.org/wikipedia/commons/9/95/Game_of_life_blinker.gif" width="82" height="82" /></a></td>
          </tr>
          <tr>
            <td>Toad</td>
            <td><a href="http://www.wikipedia.org/wiki/File:Game_of_life_toad.gif" class="image"><img alt="Game of life toad.gif" src="//upload.wikimedia.org/wikipedia/commons/1/12/Game_of_life_toad.gif" width="98" height="98" /></a></td>
          </tr>
          <tr>
            <td>Beacon</td>
            <td><a href="http://www.wikipedia.org/wiki/File:Game_of_life_beacon.gif" class="image"><img alt="Game of life beacon.gif" src="//upload.wikimedia.org/wikipedia/commons/1/1c/Game_of_life_beacon.gif" width="98" height="98" /></a></td>
          </tr>
          <tr>
            <td>Pulsar</td>
            <td><a href="http://www.wikipedia.org/wiki/File:Game_of_life_pulsar.gif" class="image"><img alt="Game of life pulsar.gif" src="//upload.wikimedia.org/wikipedia/commons/0/07/Game_of_life_pulsar.gif" width="137" height="137" /></a></td>
          </tr>
        </table>
      </td>
    </tr>
    <tr style="border: 0px;"0>
      <td>
        <table class="wikitable">
          <tr>
            <th colspan="2">Spaceships</th>
          </tr>
          <tr>
            <td>Glider</td>
            <td><a href="http://www.wikipedia.org/wiki/File:Game_of_life_animated_glider.gif" class="image"><img alt="Game of life animated glider.gif" src="//upload.wikimedia.org/wikipedia/commons/f/f2/Game_of_life_animated_glider.gif" width="84" height="84" /></a></td>
          </tr>
          <tr>
            <td>Lightweight spaceship</td>
            <td><a href="http://www.wikipedia.org/wiki/File:Game_of_life_animated_LWSS.gif" class="image"><img alt="Game of life animated LWSS.gif" src="//upload.wikimedia.org/wikipedia/commons/3/37/Game_of_life_animated_LWSS.gif" width="126" height="98" /></a></td>
          </tr>
        </table>
      </td>
      <td></td>
    </tr>
  </table>
</div>

These may prove interesting starting conditions (see Wikipedia for
more options).

<table align="center" style="text-align: center; border: 0;">
  <tr style="border-bottom: 0;">
    <td>
      <div class="thumb tright">
        <div class="thumbinner" style="width:84px;"><a href="http://www.wikipedia.org/wiki/File:Game_of_life_fpento.svg" class="image"><img alt="" src="//upload.wikimedia.org/wikipedia/commons/thumb/1/1c/Game_of_life_fpento.svg/82px-Game_of_life_fpento.svg.png" width="82" height="82" class="thumbimage" /></a>
          <div class="thumbcaption">F-pentomino</div>
        </div>
      </div>
    </td>
    <td>
      <div class="thumb tright">
        <div class="thumbinner" style="width:164px;"><a href="http://www.wikipedia.org/wiki/File:Game_of_life_diehard.svg" class="image"><img alt="" src="//upload.wikimedia.org/wikipedia/commons/thumb/9/99/Game_of_life_diehard.svg/162px-Game_of_life_diehard.svg.png" width="162" height="82" class="thumbimage" /></a>
          <div class="thumbcaption">Diehard</div>
        </div>
      </div>
    </td>
    <td>
      <div class="thumb tright">
        <div class="thumbinner" style="width:148px;"><a href="http://www.wikipedia.org/wiki/File:Game_of_life_acorn.svg" class="image"><img alt="" src="//upload.wikimedia.org/wikipedia/commons/thumb/b/b9/Game_of_life_acorn.svg/146px-Game_of_life_acorn.svg.png" width="146" height="82" class="thumbimage" /></a>
          <div class="thumbcaption">Acorn</div>
        </div>
      </div>
    </td>
  </tr>
</table>

You will "hard code" a starting configuration. The user need not be
able to provide a different starting configuration (that would just
complicate your program).

*Suggestions:* Look for stable configurations. That is, look for
communities that repeat patterns continually. The number of
configurations in the repetition is called the *period*. There are
configurations that are fixed, which continue without change. A
possible project is to find such configurations.

*Hints:* Define a `void` function named `generation` that takes the
vector we call `world` (call-by-reference or using a pointer), which
contains the current (or initial) configuration. The function scans
the vector and modifies the cells, marking the cells with births and
deaths in accord with the rules listed earlier. This involves
examining each cell in turn, either killing the cell, letting it live,
or if the cell is empty, deciding whether a cell should be born. Note
that the game will not work if your code counts neighbors in the same
vector it is modifying. A copy of the `world` vector must be created
before it is modified, and this copy is used to count neighbors, while
cells are turned on or off in the original vector.

There should be a function `display` that accepts the vector `world`
and displays the grid on the screen. Some sort of time delay is
appropriate between calls to `generation` and `display`. To do this,
your program should generate and display the next generation when you
press Return/Enter. You are at liberty to automate this (put in a real
time delay, and not wait for the user to press a key), but automation
is not necessary for the program.

If you want to "delay" the display of your grid, rather than wait for
the user to type something and press enter before displaying the next
grid, then you will need to pause or "sleep" your program somehow. If
you are using Microsoft Windows, do this:

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
[http://www.qotile.net/blog/wp/?p=600](http://www.qotile.net/blog/wp/?p=600)
to see some cool 3D visualizations of the game over time. A ridiculous
amount of further information is available here:
[http://www.conwaylife.com/wiki/index.php?title=Main_Page](http://www.conwaylife.com/wiki/index.php?title=Main_Page)

Download <a href="http://golly.sourceforge.net/">Golly</a> (for
Windows, Mac, Linux) to play with cellular automata and some amazing
creations by researchers.

Here is an app that generates music based on cellular automata
principles: [Otmata](http://www.earslap.com/projectslab/otomata).

Cellular automata is serious research; see a
[list of journals](http://uncomp.uwe.ac.uk/genaro/Cellular_Automata_Repository/Journals.html)
that publish articles about cellular automata.
