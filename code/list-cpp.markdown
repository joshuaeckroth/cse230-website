---
title: list.cpp (Linked list code)
layout: default
---

{% highlight cpp %}
#include <iostream>
#include <cassert>
using namespace std;

class Node {
public:
    double value;
    Node* pnext;
};

class List {
public:
    Node* first;
    int count;
};


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

double val_at(List* list, int i)
{
    return (node_at(list, i))->value;
}

// add a new node at the beginning of a list
void insert_front(List* list, double value)
{
    Node* n = new Node;
    n->value = value;
    n->pnext = list->first;
    list->first = n;
    list->count++;
}
 
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

