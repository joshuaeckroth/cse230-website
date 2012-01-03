---
layout: default
title: Input/output
---

You remember using `cout` to display a message. You can use `cin` to do
the opposite: get input from the user. It works like this (notice the
`>>` of `cin`, which are opposite those of `cout`):

{% highlight cpp %}
int x;
cout << "Give me a number: ";
cin >> x;
cout << "You gave me the number " << x << endl;
{% endhighlight %}

You can use `cin` for integer types, floating point types, string types,
and others. It's easiest if you ask the user to enter one value at a
time, pressing Enter between each value.

To acquire multiple values at once, just string them together with
`cin`:

{% highlight cpp %}
int x;
double y;
short z;
cout << "Enter three numeric values: ";
cin >> x >> y >> z;
{% endhighlight %}

This is equivalent to:

{% highlight cpp %}
int x;
double y;
short z;
cout << "Enter three numeric values: ";
cin >> x;
cin >> y;
cin >> z;
{% endhighlight %}

More examples:

{% highlight cpp %}
double a;
int x;
bool p;

cout << "Enter a decimal value: ";
cin >> a;
   
cout << "Enter an integer value: ";
cin >> x;
    
cout << "Enter a 0 for FALSE, anything else for TRUE: ";
cin >> p;
{% endhighlight %}

`cin` only collects input up to the first space or newline. It can be
used to obtain multiple inputs. It knows when to delimit (i.e. start
looking for the next input) when it reaches a space or newline (or
tab).

Here's the same example as above, but using just one `cin`:

{% highlight cpp %}
double a;
int x;
bool p;
    
cout << "Enter a decimal, integer, and boolean value: ";
cin >> a >> x >> p;
{% endhighlight %}

We can get strings in the usual way:

{% highlight cpp %}
string word;

cout << "Enter a word: ";
cin >> word;
{% endhighlight %}

However, using that technique, you cannot get strings that have
spaces. To get strings that have spaces in them, we have to use this
method:

{% highlight cpp %}
string s;

// get a whole line of text from the user
// and save into the variable s
getline(cin, s);
{% endhighlight %}

That method gets a whole line of text, which could have spaces.

## Printing with precision

When printing "floating-point values" (such as floats, doubles, etc.)
we often need to show a specific number of digits after the decimal
point. This is known as the "precision" of the number. The actual
precision of the value will not change; we will only change the
printed precision. The following will show three digits after the
decimal point:
 
{% highlight cpp %}
cout.precision(3);
cout.setf(ios::fixed, ios::floatfield);
{% endhighlight %}

Here is a complete example:

{% highlight cpp %}
#include <iostream>
using namespace std;
int main()
{
    double x;
    cout << "Enter value for x: ";
    cin >> x;
    
    cout.precision(3);
    cout.setf(ios::fixed, ios::floatfield);
    
    cout << "You entered " << x << endl;
    return 0;
}
{% endhighlight %}

For example,

<pre>
Enter value for x: 4.444444
You entered 4.444
</pre>

<pre>
Enter value for x: 0.0000001
You entered 0.000
</pre>

<pre>
Enter value for x: 123.45678
You entered 123.457
</pre>

Notice how the last printout rounded up; the value of "x" inside the
program has not changed, however.



