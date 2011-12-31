---
title: Quiz 1 sample
layout: default
---

Read the following code, paying particular attention to the
comments. If a comment indicates that code should be added, write code
in the blank space following the comment. Your code should perform the
task described in the comments.

{% highlight cpp %}
#include <iostream>

// gives us access to exp(), pow(), sqrt(), log(), log10(), etc.
#include <cmath>
using namespace std;

int main()
{
    int x, y, z;
    double a, b, c;
    bool p;

    // get user input for the integers
    cout << "Enter x, y: ";
    cin >> x >> y;

    // get user input for the doubles
    cout << "Enter a, b: ";
    cin >> a >> b;


    // add code: set z equal to the sum of x and y


    
    // add code: set c equal to the square root
    // of the sum of a and b



    // add code: set c equal to the natural log of b



    // add code: set p equal to true if x is less than y,
    // false otherwise



    // add code: set p equal to true if x is divisible by 3,
    // false otherwise



    // add code: set p equal to true if x is not equal to y,
    // false otherwise



    return 0;
}
{% endhighlight %}


Given that `bool p = true, q = false`, is the following expression
true or false?

{% highlight cpp %}
!((p && q) || (p && !q)) && p
{% endhighlight %}

Given that `int x = 4, y = 3` and `double z = 1.1`, is the following
expression true or false?

{% highlight cpp %}
((x >= y) && !(x/y > z)) || (x%y < z)
{% endhighlight %}

In the following code, for what values of `z` (an integer) make the
message `Burp` (and no other message) appear only once on the screen?

{% highlight cpp %}
if(z < 0)
{
    if(z < 3)
    {
        cout << "Belch" << endl;
    }
    if(z < 4)
    {
        cout << "Burp" << endl;
    }
    else
    {
        cout << "Blech" << endl;
    }
}
else if(z == 0)
{
    cout << "Burp" << endl;
}
else
{
    if(z == -1)
    {
        cout << "Belch" << endl;
    }
    else
    {
        cout << "Burp" << endl;
    }
}
{% endhighlight %}

In the following code, how many times is the conditional of the loop
evaluated?

{% highlight cpp %}
int a = 5, b = 6;
while(a < b)
{
    a = a % (b - a);
    a++;
    b--;
    a = a / b;
}
{% endhighlight %}

In the previous code, what are the final values of `a` and `b`?
