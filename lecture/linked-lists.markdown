---
title: Linked lists
layout: default
---

As we have learned, arrays and vectors are used to manage lots of
values with a single "variable" (plus an index for each
element). Arrays and vectors in C++ are very efficient (accessing
individual elements takes virtually no time at all), but inserting new
elements in the middle (and growing the array or vector) takes a lot
of time: the whole array/vector has to be copied into a larger one
before the insertion can take place.

An alternative to arrays/vectors is linked lists. Linked lists are not
as efficient at "jumping to the middle" and grabbing a value, but they
can grow and shrink without requiring copying the whole list.

**(Note: download all the code in these lecture notes as a complete
program: [list.cpp](/code/list-cpp.html))**

## How a linked list works

A linked list is composed of "nodes." Each node is a value plus a
pointer to the next node. Here is the typical linked list diagram:

![linked list](/images/linked-list.png "linked list")

So a linked list is a chain of "node" types of things. Each node must
have (at least) two things: a value and a pointer to the next
node. The value is necessary because the list is supposed to have
values in it; the value can be anything, of course (a `double`, an
`int`, whatever; maybe even something complicated like a linked list!
though that's a little funny to think about).  The pointer is
necessary in order to create the chain.

{% highlight cpp %}
class Node {
public:
    double value;
    Node* pnext;
};
{% endhighlight %}

Now we can make a single node:

{% highlight cpp %}
Node* n = new Node;
n->value = -130.569;
{% endhighlight %}

In oder to keep track of the contents/length of the list, we create
another class:

{% highlight cpp %}
class List {
public:
    Node* first;
    int count;
};
{% endhighlight %}

And now we can make a bona fide list:

{% highlight cpp %}
List* mylist = new List;
mylist->first = n;
mylist->count = 1;
{% endhighlight %}

There we have it. Our first linked list! It only has one node (one
value), so it's not much of a list.

## Linking nodes

Let's make another node, so we can link the first to the second. And
then we'll make a third, and get the equivalent of the diagram above.

{% highlight cpp %}
Node* n2 = new Node;
n2->value = 10.586;

Node* n3 = new Node;
n3->value = -74.30;

n->pnext = n2;
n2->pnext = n3;

mylist->count = 3;
{% endhighlight %}

Now we have the equivalent of the diagram above.

## A menagerie of functions

It was tedious to make each node and then link them together. Let's
make functions that do this for us. Since these functions will be
creating node variables using the `new` operator, we'll create a
function that deletes an entire list (because when you use `new` you
gotta remember to `delete`).

## Node at some position (counting from 0)

{% highlight cpp %}
// return a pointer to the i'th node
Node* node_at(List* list, int i)
{
    assert(i < list->count);

    Node* n = list->first;
    for(int j = 0; j < i; j++)
    {
        n = n->pnext;
    }
    return n;
}
{% endhighlight %}

## Value at some position (counting from 0)

{% highlight cpp %}
double val_at(List* list, int i)
{
    return (node_at(list, i))->value;
}
{% endhighlight %}

## Insert front

{% highlight cpp %}
// add a new node at the beginning of a list
void insert_front(List* list, double value)
{
    Node* n = new Node;
    n->value = value;
    n->pnext = list->first;
    list->first = n;
    list->count++;
}
{% endhighlight %}

## Push back

{% highlight cpp %}
// add a new node to the end of the list
void push_back(List* list, double value)
{
    if(list->count == 0)
    {
        insert_front(list, value);
    }
    else
    {
        Node* n = new Node;
        n->value = value;
        Node* nlast = node_at(list, list->count - 1);
        nlast->pnext = n;
        list->count++;
    }
}
{% endhighlight %}

## Insert before some position

{% highlight cpp %}
// insert a new node before the i'th node in the list
void insert_before(List* list, int i, double value)
{
    // don't bother if i is too small or too large
    if(i < 0 || i > list->count) return;

    if(i == 0)
    {
        Node* n = new Node;
        n->value = value;
        n->pnext = list->first;
        list->first = n;
        list->count++;
    }
    else
    {
        Node* n = node_at(list, i-1);
        Node* n2 = new Node;
        n2->value = value;
        n2->pnext = n->pnext;
        n->pnext = n2;
        list->count++;
    }
}
{% endhighlight %}

## Remove some position

{% highlight cpp %}
void remove_at(List* list, int i)
{
    if(i < 0 || i >= list->count) return;

    if(i == 0)
    {
        Node* toDelete = list->first;
        list->first = toDelete->pnext;
        delete toDelete;
        list->count--;
    }
    else
    {
        Node* prev = node_at(list, i-1);
        Node* toDelete = prev->pnext;
        prev->pnext = toDelete->pnext;
        delete toDelete;
        list->count--;
    }
}
{% endhighlight %}

<a href="http://xkcd.com/379/">
![xkcd comic](/images/xkcd-forgetting.png "xkcd comic")
</a>

## Print the list

{% highlight cpp %}
// print all the values
void print_list(List* list)
{
    cout.precision(1);
    cout.setf(ios::fixed, ios::floatfield);

    cout << "{";
    Node* n = list->first;
    for(int i = 0; i < list->count; i++)
    {
        cout << n->value;
        if(i < (list->count - 1))
        {
            cout << ", ";
        }
        n = n->pnext;
    }
    cout << "}" << endl;
}
{% endhighlight %}

## Delete list

{% highlight cpp %}
// free up all the memory used by the list
void delete_list(List* list)
{
    Node* n = list->first;
    Node* n2;
    for(int i = 0; i < list->count; i++)
    {
        n2 = n;
        n = n->pnext;
        delete n2;
    }
    list->count = 0;
}
{% endhighlight %}

Here is an example of how such functions can be used:

{% highlight cpp %}
int main()
{
    List* mylist = new List;
    mylist->count = 0;

    cout << "empty list: ";
    print_list(mylist);

    cout << "insert front 7.3: ";
    insert_front(mylist, 7.3);
    print_list(mylist);

    cout << "insert 1.2 before position 0: ";
    insert_before(mylist, 0, 1.2);
    print_list(mylist);

    cout << "insert 9.3 before position 1: ";
    insert_before(mylist, 1, 9.3);
    print_list(mylist);

    cout << "delete list, then print: ";
    delete_list(mylist);
    print_list(mylist);

    cout << "add 4.0, 3.0 to front, 5.0 to back: ";
    insert_front(mylist, 4.0);
    insert_front(mylist, 3.0);
    push_back(mylist, 5.0);
    print_list(mylist);

    cout << "val_at(0): " << val_at(mylist, 0) << endl;
    cout << "val_at(1): " << val_at(mylist, 1) << endl;

    cout << "add 2.0 to front, 6.0 to back: ";
    insert_front(mylist, 2.0);
    push_back(mylist, 6.0);
    print_list(mylist);

    cout << "insert 4.5 before position 3: ";
    insert_before(mylist, 3, 4.5);
    print_list(mylist);

    cout << "insert 0.0 before position 6 (i.e., at end): ";
    insert_before(mylist, 6, 0.0);
    print_list(mylist);

    cout << "remove_at(0): ";
    remove_at(mylist, 0);
    print_list(mylist);

    cout << "remove_at(2): ";
    remove_at(mylist, 2);
    print_list(mylist);

    cout << "remove_at(4) (i.e., remove end): ";
    remove_at(mylist, 4);
    print_list(mylist);

    cout << "remove_at(-1) (should do nothing): ";
    remove_at(mylist, -1);
    print_list(mylist);

    cout << "remove_at(2): ";
    remove_at(mylist, 2);
    print_list(mylist);
 
    cout << "delete list, then print: ";
    delete_list(mylist);
    print_list(mylist);

    return 0;
}
{% endhighlight %}

This is what we see:

<pre>
empty list: {}
insert front 7.3: {7.3}
insert 1.2 before position 0: {1.2, 7.3}
insert 9.3 before position 1: {1.2, 9.3, 7.3}
delete list, then print: {}
add 4.0, 3.0 to front, 5.0 to back: {3.0, 4.0, 5.0}
val_at(0): 3.0
val_at(1): 4.0
add 2.0 to front, 6.0 to back: {2.0, 3.0, 4.0, 5.0, 6.0}
insert 4.5 before position 3: {2.0, 3.0, 4.0, 4.5, 5.0, 6.0}
insert 0.0 before position 6 (i.e., at end): {2.0, 3.0, 4.0, 4.5, 5.0, 6.0, 0.0}
remove_at(0): {3.0, 4.0, 4.5, 5.0, 6.0, 0.0}
remove_at(2): {3.0, 4.0, 5.0, 6.0, 0.0}
remove_at(4) (i.e., remove end): {3.0, 4.0, 5.0, 6.0}
remove_at(-1) (should do nothing): {3.0, 4.0, 5.0, 6.0}
remove_at(2): {3.0, 4.0, 6.0}
delete list, then print: {}
</pre>

