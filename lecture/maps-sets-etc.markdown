---
title: Maps, sets, etc.
layout: default
---

We saw [vectors](/lecture/vectors.html) previously. Vectors are part of the
"Standard Template Library" (STL) -- "standard" because vectors are part of
every C++ installation, "template" because the vector class is a template class
(you can put any type of thing in a vector, but you have to choose the type for
each vector and stick with it).

There are several other "containers" in the STL, and they are designed to
support much of the same functionality of vectors. First we'll learn about
maps.

## Maps

Maps are like vectors in that they are STL containers. However, while vectors
only store single values, map store key-value pairs. Imagine you want to store
word frequencies: each word (a string) is a key, and the frequency (an integer)
is the value. We'd make such a map like this:

{% highlight cpp %}
#include <map>
// ...
map<string, int> wordfreqs;
{% endhighlight %}

Then, to add stuff to the map, we could use the `insert` function (which is
also available for vectors) but what's probably more natural is to use the `[]`
syntax:

{% highlight cpp %}
wordfreqs["the"] = 1503032;
{% endhighlight %}

The `[]` syntax is "overloaded" by the map class; that means the map class has
redefined what `[]` does (previously, we only used `[]` on arrays). The result
of the above code is that a new key-value pair "the" and 1503032 is put into
the map. If we later wanted to get at the value 1503032 (e.g. to print the
value), we could just write `wordfreqs["the"]` as you might expect.

## Map iterators

In order to look at all the key-value pairs in a map, we need a way to
"iterate" through the map. All the STL containers (vectors, maps, sets, etc.)
have their own specific "iterator" types. To make an iterator, write the same
"template type" as when you created the map, but then put `::iterator` after
it, like so:

{% highlight cpp %}
map<string, int>::iterator it;
{% endhighlight %}

Now `it` can iterate through a map that has string-int key-value pairs. The
iterator need not be named `it`, but that is very common. The iterator acts
like a pointer to the elements of the container. The statement `++it` (or
`it++`, which is less common) moves the iterator to the next element.

Here is how you use an iterator (for any STL container):

{% highlight cpp %}
for(it = wordfreqs.begin(); it != wordfreqs.end(); ++it)
{
    // ... (treat 'it' as a pointer)
}
{% endhighlight %}

That `for` loop basically means: let `it` iterate through `wordfreqs`, starting
at the beginning and stopping once it passes the end.

Because a map has key-value pairs, we have to use `first` and `second` to get
to the key and value (respectively). So `it->first` is the key of some
key-value pair, and `it->second` is the value. We change the iterator (`++it`)
to point to the next key-value pair, until we run out of them.

Here is a complete example. Note that it also includes the `find` function,
which works on any STL container, and returns an iterator. If the iterator
equals the "end" of the `wordfreqs` map, then that means the key was not found
in the map. If the returned iterator does not equal the end, then we can use
the iterator to get the value for that key.

{% highlight cpp %}
#include <iostream>
#include <string>
#include <map>
using namespace std;

int main()
{
    map<string, int> wordfreqs;

    wordfreqs["the"] = 1503032;
    wordfreqs["and"] = 35234;
    wordfreqs["irregardless"] = 2324;
    wordfreqs["regardless"] = 605;
    wordfreqs["irrespective"] = 82;

    cout << "wordfreqs has " << wordfreqs.size()
         << " elements." << endl << endl;

    // print words & freqs
    map<string, int>::iterator it;
    for(it = wordfreqs.begin(); it != wordfreqs.end(); ++it)
    {
        cout << "'" << it->first << "' appeared "
             << it->second << " times." << endl;
    }

    cout << endl;

    // search for a key
    map<string, int>::iterator pos;
    cout << "Is 'irrespective' in the wordfreqs list?" << endl;
    pos = wordfreqs.find("irrespective");
    if(pos != wordfreqs.end())
    {
        cout << "Yes! Its frequency is "
             << pos->second << "." << endl;
    }
    else
    {
        cout << "No." << endl;
    }

    return 0;
}
{% endhighlight %}

Here is the output:

<pre>
wordfreqs has 5 elements.

'and' appeared 35234 times.
'irregardless' appeared 2324 times.
'irrespective' appeared 82 times.
'regardless' appeared 605 times.
'the' appeared 1503032 times.

Is 'irrespective' in the wordfreqs list?
Yes! Its frequency is 82.
</pre>


## Sets

Sets are just like vectors (store single values) but don't keep repeats. Their
usage is quite similar to maps and vectors (which is the point):

{% highlight cpp %}
#include <iostream>
#include <string>
#include <set>
using namespace std;

int main()
{
    string s;
    set<string> words;

    while(s != "quit")
    {
        cout << "Enter a word, or 'quit' to quit: ";
        cin >> s;
        if(s != "quit")
        {
            words.insert(s);
        }
    }

    cout << endl;
    cout << "You entered (without repeats):" << endl;
    set<string>::iterator it;
    for(it = words.begin(); it != words.end(); ++it)
    {
        // use it like a pointer to a string
        cout << *it << endl;
    }

    return 0;
}
{% endhighlight %}

Output:

<pre>
Enter a word, or 'quit' to quit: happy
Enter a word, or 'quit' to quit: happy
Enter a word, or 'quit' to quit: joy
Enter a word, or 'quit' to quit: joy
Enter a word, or 'quit' to quit: quit

You entered (without repeats):
happy
joy
</pre>




