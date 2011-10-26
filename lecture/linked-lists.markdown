---
title: Linked lists
layout: default
---

As we have learned, arrays are used to manage lots of values with a
single "variable" (plus an index for each element). Arrays, in C++,
are very efficient (accessing individual elements takes virtually no
time at all), but they are difficult to use (an array cannot "grow,"
for instance).

An alternative to arrays is linked lists. Linked lists are not as
efficient (grabbing a value from the middle of the list is not as
efficient as it would be with an array) but they are more flexible:
they can grow and shrink at will.

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

We saw "structures" earlier (at the end of the
[Multidimensional arrays](/lecture/multidimensional-arrays.html)
lecture notes). We can define a list structure (which stores `double`
values, just to choose something) and simultaneously make a new type
called `list` like this:

{% highlight cpp %}
struct list {
    double value;
    list* pnext;
};
{% endhighlight %}

Now we can make a single node (which has no "next" node, so `pnext`
points to nothing):

{% highlight cpp %}
list mynode;
mynode.value = -130.569;
mynode.pnext = NULL;
{% endhighlight %}

There we have it. Our first linked list! It only has one node (one
value), so it's not much of a list.

## Linking nodes

Let's make another node, so we can link the first to the second. And
then we'll make a third, and get the equivalent of the diagram above.

{% highlight cpp %}
list mynode2;
mynode2.value = 10.586;
mynode2.pnext = NULL;

list mynode3;
mynode3.value = -74.30;
mynode3.pnext = NULL;
{% endhighlight %}

Wait, that's not right; they aren't linked. We need `mynode` and
`mynode2` to link to their next node:

{% highlight cpp %}
mynode.pnext = &mynode2;
mynode2.pnext = &mynode3;
{% endhighlight %}

Now we have the equivalent of the diagram above.

Note, because this will come up again in just a minute, that if you
have a pointer to a structure, such as a pointer to a node:

{% highlight cpp %}
list *head = &mynode;
{% endhighlight %}

then to access the elements of the structure, you can use `->` rather
than dereference `*` followed by `.` like so:

{% highlight cpp %}
// these are equivalent:
head->value = 14.0;
(*head).value = 14.0;
{% endhighlight %}

It's just an extra syntax for convenience.

## A menagerie of functions

It was tedious to make each node and then link them together. Let's
make functions that do this for us. Since these functions will be
creating node variables, we'll use the `new` operator, and we'll
create a function that deletes an entire list (because when you use
`new` you gotta remember to `delete`).

## Insert front

{% highlight cpp %}
// this function adds a new node at the beginning of a list;
// it receives the pointer to the 'head' of an existing list
// and changes that pointer; that's why the first parameter
// is a call-by-reference parameter
void insert_front(list *&head, double value)
{
    list *n = new list;
    n->value = value;
    n->pnext = head;
 
    head = n;
}
{% endhighlight %}

## Push back

{% highlight cpp %}
// this function adds a new node to the end of the list;
// to do so, it has to find the end of the list first;
// note it may change the head pointer, if the list
// is empty; so head is call-by-reference
void push_back(list *&head, double value)
{
    // push_back == insert_front when we have no list
    if(head == NULL)
    {
        insert_front(head, value);
    }
    else
    {
        // find the end of the list
        list *n = head;
        while(n->pnext != NULL)
        {
            // go to next node so long as there is one
            n = n->pnext;
        }
 
        // by the way, that loop could have been done
        // with the following 'for' loop
        // list *n;
        // for(n = head; n->pnext != NULL; n = n->pnext);
 
        // now n points to the last node;
        // make a new node
        list *n2 = new list;
        n2->value = value;
        n2->pnext = NULL;
 
        // link the prior last node ('n')
        // to this new last node ('n2')
        n->pnext = n2;
    }
}
{% endhighlight %}

## Insert before some position

{% highlight cpp %}
void insert_before(list *&head, int n, double value)
{
    if(n < 0) return;

    if(head == NULL || n == 0)
    {
        insert_front(head, value);
        return;
    }

    // find the (n-1)'st node
    list *tmp = head;
    int i = 0;
    while(i < (n-1) && tmp->pnext != NULL)
    {
        tmp = tmp->pnext;
        i++;
    }

    // now tmp points to the node we
    // should insert *after*
    list *n2 = new list;
    n2->value = value;
    n2->pnext = tmp->pnext;
    tmp->pnext = n2;
}
{% endhighlight %}

## Remove some position

{% highlight cpp %}
void remove_nth(list *&head, int n)
{
    if(n < 0 || head == NULL) return;

    // remove first element, so head
    // needs to change
    if(n == 0)
    {
        list *oldhead = head;
        head = head->pnext;
        delete oldhead;
    }
    else
    {
        // if n!= 0, then walk through the list
        // for n steps, remembering the previous
        // node (there will be a prev node since
        // n != 0); if the list is shorter than n
        // elements, do nothing (just return)
        int i = 0;
        list *tmp = head;
        list *prev;
        while(i < n && tmp != NULL)
        {
            prev = tmp;
            tmp = tmp->pnext;
            i++;
        }

        // list is shorter than n elements
        if(tmp == NULL) return;

        // if we're here, we can do the removal
        prev->pnext = tmp->pnext;
        delete tmp;
    }
}
{% endhighlight %}

<a href="http://xkcd.com/379/">
![xkcd comic](/images/xkcd-forgetting.png "xkcd comic")
</a>

## Reverse the list

{% highlight cpp %}
void reverse(list *&head)
{
    list *n = head;
    list *tmp;

    // special case: point front to NULL
    if(n != NULL)
    {
        // save where it pointed to first
        tmp = n->pnext;

        n->pnext = NULL;

        // move to next element
        n = tmp;
    }

    // head will point to prior element,
    // n points to current element,
    // tmp points to next element
    while(n != NULL)
    {
        // get next element
        tmp = n->pnext;

        // set new next to prior
        n->pnext = head;

        // set prior = current
        head = n;

        // set current = next
        n = tmp;
    }
}
{% endhighlight %}

## Length

{% highlight cpp %}
// count length of list
int length(list *head)
{
    int i = 0;
    while(head != NULL)
    {
        head = head->pnext;
        i++;
    }
    return i;
}
{% endhighlight %}

## Get the n<sup>th</sup> element

{% highlight cpp %}
// return nth element (counting from 0);
// can't be called on an empty list and
// n must be a valid position
double nth(list *head, int n)
{
    assert(head != NULL);

    int i = 0;
    while(i < n && head != NULL)
    {
        head = head->pnext;
        i++;
    }
    // require that the nth element exists
    assert(head != NULL);

    return head->value;
}
{% endhighlight %}

## Find position of some value

{% highlight cpp %}
// return the position of the first value
// that matches (within epsilon range)
// a particular value; (we use epsilon
// because doubles don't have exact values);
// returns -1 if the value was not found
int find(list *head, double value, double epsilon)
{
    int i = -1;
    while(head != NULL)
    {
        i++;

        // if absolute value of difference
        // is less than epsilon, then we
        // found our element
        if(fabs(head->value - value) < epsilon)
        {
            break;
        }

        // if we didn't 'break', go to next
        head = head->pnext;
    }
    // ran out of list, never found it
    if(head == NULL)
        i = -1;

    return i;
}
{% endhighlight %}

## Print list

{% highlight cpp %}
// print all the values
void print_list(list *head)
{
    cout.precision(1);
    cout.setf(ios::fixed, ios::floatfield);

    cout << "{";
    while(head != NULL)
    {
        cout << head->value;
        if(head->pnext != NULL)
            cout << ", ";
        head = head->pnext;
    }
    cout << "}" << endl;
}
{% endhighlight %}

## Delete list

{% highlight cpp %}
// free up all the memory used by the list
void delete_list(list *&head)
{
    list *n;
    while(head != NULL)
    {
        n = head;
        head = head->pnext;
        delete n;
    }
    head = NULL;
}
{% endhighlight %}

Here is an example of how such functions can be used:

{% highlight cpp %}
int main()
{
    list *mylist = NULL;

    cout << "empty list: ";
    print_list(mylist);

    cout << "reversed: ";
    reverse(mylist);
    print_list(mylist);

    cout << "length: " << length(mylist) << endl;

    cout << "insert 1.0 before 0: ";
    insert_before(mylist, 0, 1.0);
    print_list(mylist);

    cout << "delete list, then print: ";
    delete_list(mylist);
    print_list(mylist);

    cout << "add 4.0, 3.0 to front, 5.0 to back: ";
    insert_front(mylist, 4.0);
    insert_front(mylist, 3.0);
    push_back(mylist, 5.0);
    print_list(mylist);

    cout << "nth(0): " << nth(mylist, 0) << endl;
    cout << "nth(1): " << nth(mylist, 1) << endl;

    cout << "add 2.0 to front, 6.0 to back: ";
    insert_front(mylist, 2.0);
    push_back(mylist, 6.0);
    print_list(mylist);

    cout << "insert 4.5 before position 3: ";
    insert_before(mylist, 3, 4.5);
    print_list(mylist);

    cout << "index of 4.0: " << find(mylist, 4.0, 0.001) << endl;
    cout << "index of 4.5: " << find(mylist, 4.5, 0.001) << endl;
    cout << "index of 6.0: " << find(mylist, 6.0, 0.001) << endl;
    cout << "index of 2.0: " << find(mylist, 2.0, 0.001) << endl;
    cout << "index of 8.0: " << find(mylist, 8.0, 0.001) << endl;

    cout << "length: " << length(mylist) << endl;

    cout << "reverse: ";
    reverse(mylist);
    print_list(mylist);

    cout << "reverse (again): ";
    reverse(mylist);
    print_list(mylist);

    cout << "remove_nth(0): ";
    remove_nth(mylist, 0);
    print_list(mylist);

    cout << "remove_nth(2): ";
    remove_nth(mylist, 2);
    print_list(mylist);

    cout << "remove_nth(5) (should do nothing): ";
    remove_nth(mylist, 5);
    print_list(mylist);

    cout << "remove_nth(-1) (should do nothing): ";
    remove_nth(mylist, -1);
    print_list(mylist);

    cout << "remove_nth(2): ";
    remove_nth(mylist, 2);
    print_list(mylist);
 
    cout << "delete list, then print: ";
    delete_list(mylist);
    print_list(mylist);

    cout << "index of 8.0: " << find(mylist, 8.0, 0.001) << endl;

    return 0;
}
{% endhighlight %}

This is what we see:

<pre>
empty list: {}
reversed: {}
length: 0
insert 1.0 before 0: {1.0}
delete list, then print: {}
add 4.0, 3.0 to front, 5.0 to back: {3.0, 4.0, 5.0}
nth(0): 3.0
nth(1): 4.0
add 2.0 to front, 6.0 to back: {2.0, 3.0, 4.0, 5.0, 6.0}
insert 4.5 before position 3: {2.0, 3.0, 4.0, 4.5, 5.0, 6.0}
index of 4.0: 2
index of 4.5: 3
index of 6.0: 5
index of 2.0: 0
index of 8.0: -1
length: 6
reverse: {6.0, 5.0, 4.5, 4.0, 3.0, 2.0}
reverse (again): {2.0, 3.0, 4.0, 4.5, 5.0, 6.0}
remove_nth(0): {3.0, 4.0, 4.5, 5.0, 6.0}
remove_nth(2): {3.0, 4.0, 5.0, 6.0}
remove_nth(5) (should do nothing): {3.0, 4.0, 5.0, 6.0}
remove_nth(-1) (should do nothing): {3.0, 4.0, 5.0, 6.0}
remove_nth(2): {3.0, 4.0, 6.0}
delete list, then print: {}
index of 8.0: -1
</pre>

