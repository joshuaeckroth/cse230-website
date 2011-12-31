---
title: Quiz 2 sample
layout: default
---

Study the following code. It *should* meet the following
specification, but it does not. Your task is to rewrite the code so
that it is correct.

The function `is_prime` determines whether its input is a prime
number. Recall that a prime number is defined as those integers
(greater than 1) that are divisible by only themselves and 1 (of
course 1 divides every integer). So 7 is prime because the only
numbers that divide 7 are 7 itself and 1. But 9 is not prime because 9
is divisible by 3.

Here is a program that has the `is_prime` function and a `main`
function. There are several errors in both functions. Rewrite the code
so it is correct.

{% highlight cpp %}
include <iostream>
using namespace std;

int main()
{
    char c = 'Y';
    while(c == 'Y');
    {
        cout << Enter a number: << endl;
        cin >> n;

        bool p = is_prime(n)
        if(p)
        {
             cout << n << " is not prime." << endl;
        }
        else
        {
             cout << n << " is prime." << endl;
        }

        cout << "Do it again? Enter Y or N:"
        cin >> c;
    }
    return 0;
}

void is_prime()
{
    int n;
    for(int i = 1; i < n; i++)
    {
        if(i % n == 0)
        {
            prime = false;
        }
        else
        {
            prime = true;
        }
    }
    return prime;
}
{% endhighlight %}
