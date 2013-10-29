---
title: Homework 2
layout: default
---

From the book, page 298, question 5.

Write a program that tells what coins to give out for any amount of change from
1 cent to 99 cents. For example, if the amount is 86 cents, the output would be
something like the following:

<pre>
86 cents can be given as
3 quarter(s) 1 dime(s) and 1 penny(pennies)
</pre>

Use coin denominations of 25 cents (quarters), 10 cents (dimes), and 1 cent
(pennies). Do not use nickel and half-dollar coins. Your program will use the
following function (among others if you wish):

{% highlight cpp %}
void compute_coin(int coin_value, int& number, int& amount_left);
// Precondition: 0 < coin_value < 100; 0 <= amount_left < 100.
//
// Postcondition: number has been set equal to the maximum number
// of coins denomination coin_value cents that can be obtained
// from amount_left; amount_left has been decreased by the value
// of the coins, that is, decreased by number*coin_value.
{% endhighlight %}

For example, suppose the value of the variable `amount_left` is 86. Then, after
the following call, the value of `number` will be 3 and the value of
`amount_left` will be 11 (because if you take 3 quarters from 86 cents, that
leaves 11 cents):

{% highlight cpp %}
compute_coins(25, number, amount_left);
{% endhighlight %}

*Hint:* Use integer division and the `%` operator to implement this function.

Include a loop that lets the user repeat this computation for new input values
until the user says he or she wants to end the program. You will need to figure
out how you want the user to quit the program: either by typing a `char` like
Y or N, or typing a special number like -1.

Handle errors in the following way: if the user enters a value less than 0 or
greater than or equal to 100 (which violates the function's precondition), then
tell the user that's an invalid value, and repeat the loop that asks for
another input value.

