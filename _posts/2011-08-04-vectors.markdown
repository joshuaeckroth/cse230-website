---
title: Vectors
layout: default
---

Vectors are just improved arrays. As we have seen, arrays can be a real hassle.
Arrays don't know how big they are, they can't grow, and we can't "return" an
array from a function. C++ introduced vectors so that we can avoid those
issues.

Arrays are used more often, however, only because they are simple and the
computer can work with them more efficiently. But vectors are usually much more
convenient for the programmer. (Note that vectors are really just arrays with
some fancy functions.)

## Vector usage

Vectors are one of many "containers" introduced in C++. A container holds data.
Recall that when you created an array, you had to decide what kind of array it
was going to be (integers, doubles, etc.). The same is true with vectors,
except that the type is specified a little differently (as it is with all
"containers" in C++).

Here is how you create a vector full of integers:

{% highlight cpp %}
vector<int> myvec;
{% endhighlight %}

(Don't forget `#include <vector>` at the top of your files when you use
vectors).

If you want doubles instead:

{% highlight cpp %}
vector<double> vals;
{% endhighlight %}

It's not possible to have a vector full of doubles *and* ints, for example
("heterogeneous" containers are not possible).

You can put elements in your vector with several methods. Most common is
`push_back`:

{% highlight cpp %}
vector<double> vals;
vals.push_back(5.3);
vals.push_back(0.66);
{% endhighlight %}

More examples are shown below.

## Example 1 - simple vector

{% highlight cpp %}
#include <iostream>
#include <vector>  // this is necessary to use vectors
using namespace std;

int main()
{
    vector<int> vals;
    vals.push_back(5);
    vals.push_back(6);
    vals.push_back(1);

    cout << "Size of vals: " << vals.size() << endl;
    for(int i = 0; i < vals.size(); i++)
    {
        cout << "Value at index " << i
             << " is " << vals[i] << endl;
    }
    return 0;
}
{% endhighlight %}

Output:

<pre>
Size of vals: 3
Value at index 0 is 5
Value at index 1 is 6
Value at index 2 is 1
</pre>

## Example 2 - vector with initial values

You can create a vector of some specific size and give it an initial (repeated)
value. This is most often used to give a vector a bunch of zeros.

{% highlight cpp %}
#include <iostream>
#include <vector>
using namespace std;

int main()
{
    // create a vector of size 10,
    // with every element equal to 0
    vector<int> vals(10, 0);
    vals.push_back(5);
    vals.push_back(6);
    vals.push_back(1);

    cout << "Size of vals: " << vals.size() << endl;
    for(int i = 0; i < vals.size(); i++)
    {
        cout << "Value at index " << i
             << " is " << vals[i] << endl;
    }
    return 0;
}
{% endhighlight %}

Output:

<pre>
Size of vals: 13
Value at index 0 is 0
Value at index 1 is 0
Value at index 2 is 0
Value at index 3 is 0
Value at index 4 is 0
Value at index 5 is 0
Value at index 6 is 0
Value at index 7 is 0
Value at index 8 is 0
Value at index 9 is 0
Value at index 10 is 5
Value at index 11 is 6
Value at index 12 is 1
</pre>

## Example 3 - clearing a vector

The `clear()` function deletes all the values in the vector.

{% highlight cpp %}
#include <iostream>
#include <vector>
using namespace std;

int main()
{
    // create a vector of size 10,
    // with every element equal to 0
    vector<int> vals(10, 0);
    vals.push_back(5);
    vals.push_back(6);
    vals.push_back(1);

    cout << "Size of vals: " << vals.size() << endl;
    for(int i = 0; i < vals.size(); i++)
    {
        cout << "Value at index " << i
             << " is " << vals[i] << endl;
    }

    vals.clear();
    cout << "Size of vals: " << vals.size() << endl;

    return 0;
}
{% endhighlight %}

Output:

<pre>
Size of vals: 13
Value at index 0 is 0
Value at index 1 is 0
Value at index 2 is 0
Value at index 3 is 0
Value at index 4 is 0
Value at index 5 is 0
Value at index 6 is 0
Value at index 7 is 0
Value at index 8 is 0
Value at index 9 is 0
Value at index 10 is 5
Value at index 11 is 6
Value at index 12 is 1
Size of vals: 0
</pre>

## Example 4 - vector of strings

You can put anything in vectors, even strings (which are themselves
more-or-less vectors, too). (You can put vectors inside vectors, ad nauseum).

{% highlight cpp %}
#include <iostream>
#include <vector>
#include <string>
using namespace std;

int main()
{
    vector<string> names;
    names.push_back("Euler");
    names.push_back("Descartes");
    names.push_back("Turing");
    names.push_back("Church");

    cout << "Size of names: " << names.size() << endl;
    for(int i = 0; i < names.size(); i++)
    {
        cout << "Name at index " << i
             << " is " << names[i] << endl;
    }

    return 0;
}
{% endhighlight %}

Output:

<pre>
Size of names: 4
Name at index 0 is Euler
Name at index 1 is Descartes
Name at index 2 is Turing
Name at index 3 is Church
</pre>

## Example 5 - using the empty() function

The `clear()` function deletes all the values in the vector. The `empty()`
function tells us if a vector has no values.

{% highlight cpp %}
#include <iostream>
#include <vector>
#include <string>
using namespace std;

int main()
{
    vector<string> names;
    names.push_back("Euler");
    names.push_back("Descartes");
    names.push_back("Turing");
    names.push_back("Church");

    names.clear();

    if(names.empty())
    {
        cout << "Names vector is empty." << endl;
    }
    else
    {
        cout << "Names vector is not empty." << endl;
    }

    return 0;
}
{% endhighlight %}

Output:

<pre>
Names vector is empty.
</pre>

## Example 6 - sorting

Vectors can sort themselves (using the "quick sort" technique).

{% highlight cpp %}
#include <iostream>
#include <vector>
#include <string>
#include <algorithm>
using namespace std;

int main()
{
    vector<string> names;
    names.push_back("Euler");
    names.push_back("Descartes");
    names.push_back("Turing");
    names.push_back("Church");

    sort(names.begin(), names.end());

    for(int i = 0; i < names.size(); i++)
    {
        cout << "Name at index " << i
             << " is " << names[i] << endl;
    }

    return 0;
}
{% endhighlight %}

Output:

<pre>
Name at index 0 is Church
Name at index 1 is Descartes
Name at index 2 is Euler
Name at index 3 is Turing
</pre> 

## Example 7 - random shuffling

The reverse of sorting is shuffling; sometimes useful to randomize the order of
our data for experiments.

{% highlight cpp %}
#include <iostream>
#include <vector>
#include <string>
#include <algorithm>
#include <ctime>
#include <cstdlib>
using namespace std;

int main()
{
    // initialize the random number generator "seed"
    // to the current time (in seconds); this way,
    // the seed is different each time we run the program
    srand(time(NULL));

    vector<string> names;
    names.push_back("Euler");
    names.push_back("Descartes");
    names.push_back("Turing");
    names.push_back("Church");
    names.push_back("Curry");
    names.push_back("Gauss");
    names.push_back("Riemann");
    names.push_back("McCarthy");
    names.push_back("Hopper");
    names.push_back("Lovelace");

    sort(names.begin(), names.end());

    cout << "--Sorted names:" << endl;
    for(int i = 0; i < names.size(); i++)
    {
        cout << names[i] << endl;
    }
    cout << endl;

    random_shuffle(names.begin(), names.end());

    cout << "--Randomly shuffled names:" << endl;
    for(int i = 0; i < names.size(); i++)
    {
        cout << names[i] << endl;
    }

    return 0;
}
{% endhighlight %}

Output:

<pre>
--Sorted names:
Church
Curry
Descartes
Euler
Gauss
Hopper
Lovelace
McCarthy
Riemann
Turing

--Randomly shuffled names:
Curry
Hopper
Turing
Descartes
Gauss
Lovelace
McCarthy
Church
Euler
Riemann
</pre>

## Example 8 - passing a vector to a function

When you pass a vector to the function, the whole package is copied and given
to the function; so if you change the vector in the function, the rest of the
world is not affected. Also note that while you have to provide a function the
size of the array when you use arrays and functions together, the vector
already knows how big it is, so we don't need a "size" parameter.

{% highlight cpp %}
#include <iostream>
#include <vector>
using namespace std;

int sum(vector<int> vals)
{
    int sum = 0;
    for(int i = 0; i < vals.size(); i++)
    {
        sum += vals[i];
    }
    return sum;
}

int main()
{
    vector<int> xs;
    
    for(int i = 0; i < 1000; i++)
    {
        xs.push_back(i + 1);
    }

    cout << "Sum of integers 1 to 1000: " << sum(xs) << endl;

    return 0;
}
{% endhighlight %}

Output:

<pre>
Sum of integers 1 to 1000: 500500
</pre>

## Example 11 - returning a vector from a function

While arrays cannot be returned by a function, vectors can, in the usual way:

{% highlight cpp %}
#include <iostream>
#include <vector>
using namespace std;

vector<double> repeatThree(vector<double> vals)
{
    vector<double> repeatedVals;

    for(int i = 0; i < vals.size(); i++)
    {
        repeatedVals.push_back(vals[i]);
        repeatedVals.push_back(vals[i]);
        repeatedVals.push_back(vals[i]);
    }
    
    return repeatedVals;
}

int main()
{
    vector<double> vals;
    vals.push_back(1.4);
    vals.push_back(2.3);
    vals.push_back(5.3);
    vals.push_back(6.2);

    vector<double> newVals = repeatThree(vals);

    for(int i = 0; i < newVals.size(); i++)
    {
        cout << newVals[i] << endl;
    }

    return 0;
}
{% endhighlight %}

## Example 12 - passing a vector by reference to a function

{% highlight cpp %}
#include <iostream>
#include <vector>
using namespace std;

void repeatThree(vector<double> &vals)
{
    vector<double> repeatedVals;

    for(int i = 0; i < vals.size(); i++)
    {
        repeatedVals.push_back(vals[i]);
        repeatedVals.push_back(vals[i]);
        repeatedVals.push_back(vals[i]);
    }
    
    vals = repeatedVals;
}

int main()
{
    vector<double> vals;
    vals.push_back(1.4);
    vals.push_back(2.3);
    vals.push_back(5.3);
    vals.push_back(6.2);
    
    repeatThree(vals);

    for(int i = 0; i < vals.size(); i++)
    {
        cout << vals[i] << endl;
    }

    return 0;
}
{% endhighlight %}

## Strings are (just like) vectors

Interestingly enough, strings are just vectors of `char` things (more or less).
We can use the same vector functions on strings (mostly). For example, we can
ask a string its size (`s.size()`), reverse it (`s.reverse()`), etc.

