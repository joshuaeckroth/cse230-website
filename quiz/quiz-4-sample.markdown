---
title: Quiz 4 sample
layout: default
---

Quiz 4 will ask you to "trace code" involving arrays and functions.

## Example 1: What does it print?

{% highlight cpp %}
int xs[5] = {6, 7, 8, 9, 10};
int ys[5] = {2, 2, 2, 2, 2};
for(int i = 0; i < 3; i++)
{
    ys[4-i] = xs[i];
    for(int j = 0; j < 2; j++)
    {
        xs[j] = ys[j] + xs[i];
    }
    cout << ys[i] << "," << xs[i] << endl;
}
{% endhighlight %}

## Example 2: What does it print?

{% highlight cpp %}
#include <iostream>
using namespace std;

void bar(int xs[], int size)
{
    for(int i = 0; i < size; i++)
    {
        xs[i] = xs[size-i-1];
    }
}

int main()
{
    int vals[5] = {-10, 34, 27, 7, 2};
    for(int i = 0; i < 5; i++)
    {
        cout << vals[i] << " ";
    }
    cout << endl;

    bar(vals, 3);  // yes, I want 3 rather than 5

    for(int i = 0; i < 5; i++)
    {
        cout << vals[i] << " ";
    }
    cout << endl;

    return 0;
}
{% endhighlight %}
