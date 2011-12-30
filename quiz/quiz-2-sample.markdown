---
title: Quiz 2 sample
layout: default
---

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


