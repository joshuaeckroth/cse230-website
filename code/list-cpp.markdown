---
title: list.cpp (Linked list code)
layout: default
---

{% highlight cpp %}
#include <iostream>
#include <cassert>
#include <cmath>
using namespace std;

typedef struct node {
    double value;
    node* pnext;
} node;

// let's make a new type: a pointer to a node is a 'list'
typedef node* list;
 
// this function adds a new node at the beginning of a list;
// it receives the pointer to the 'head' of an existing list
// and changes that pointer; that's why the first parameter
// is a call-by-reference parameter
void insert_front(list &head, double value)
{
    node *n = new node;
    n->value = value;
    n->pnext = head;
 
    // a 'list' is a 'node*' so make head equal
    // to the pointer to the node we created
    head = n;
}
 
// this function adds a new node to the end of the list;
// to do so, it has to find the end of the list first;
// note it may change the head pointer, if the list
// is empty; so head is call-by-reference
void push_back(list &head, double value)
{
    // push_back == insert_front when we have no list
    if(head == NULL)
    {
        insert_front(head, value);
    }
    else
    {
        // find the end of the list
        node *n = head;
        while(n->pnext != NULL)
        {
            // go to next node so long as there is one
            n = n->pnext;
        }
 
        // by the way, that loop could have been done
        // with the following 'for' loop
        // node *n;
        // for(n = head; n->pnext != NULL; n = n->pnext);
 
        // now n points to the last node;
        // make a new node
        node *n2 = new node;
        n2->value = value;
        n2->pnext = NULL;
 
        // link the prior last node ('n')
        // to this new last node ('n2')
        n->pnext = n2;
    }
}

void insert_before(list &head, int n, double value)
{
    if(n < 0) return;

    if(head == NULL || n == 0)
    {
        insert_front(head, value);
        return;
    }

    // find the (n-1)'st node
    node *tmp = head;
    int i = 0;
    while(i < (n-1) && tmp->pnext != NULL)
    {
        tmp = tmp->pnext;
        i++;
    }

    // now tmp points to the node we
    // should insert *after*
    node *n2 = new node;
    n2->value = value;
    n2->pnext = tmp->pnext;
    tmp->pnext = n2;
}

void remove_nth(list &head, int n)
{
    if(n < 0 || head == NULL) return;

    // remove first element, so head
    // needs to change
    if(n == 0)
    {
        node *oldhead = head;
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
        node *tmp = head;
        node *prev;
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

void reverse(list &head)
{
    node *n = head;
    node *tmp;

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

// count length of list
int length(list head)
{
    int i = 0;
    while(head != NULL)
    {
        head = head->pnext;
        i++;
    }
    return i;
}

// return nth element (counting from 0);
// can't be called on an empty list and
// n must be a valid position
double nth(list head, int n)
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

// return the position of the first value
// that matches (within epsilon range)
// a particular value; (we use epsilon
// because doubles don't have exact values);
// returns -1 if the value was not found
int find(list head, double value, double epsilon)
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

 
// print all the values
void print_list(list head)
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
 
// free up all the memory used by the list
void delete_list(list &head)
{
    node *n;
    while(head != NULL)
    {
        n = head;
        head = head->pnext;
        delete n;
    }
    head = NULL;
}


int main()
{
    list mylist = NULL;

    cout << "empty list: ";
    print_list(mylist);

    cout << "reversed: ";
    reverse(mylist);
    print_list(mylist);

    cout << "length: " << length(mylist) << endl;

    cout << "insert 1.0 before 8: ";
    insert_before(mylist, 8, 1.0);
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

