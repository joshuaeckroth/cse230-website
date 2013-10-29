---
title: Homework 6
layout: default
---

From the book, p. 875 q. 9.

Skills needed to complete this assignment:

  - Creating classes and using object-oriented program design
    ([lecture notes (part 1)](/lecture/classes-and-object-orientation.html)
    and
    [lecture notes (part 2)](/lecture/classes-and-object-orientation-2.html))

  - Using polymorphism ([lecture notes](/lecture/polymorphism.html))

  - Splitting code into several files
    ([lecture notes](/lecture/splitting-code.html))

Banks have many different types of accounts often with different rules
for fees associated with transactions such as withdrawals. Customers
are allowed to transfer funds between accounts incurring the
appropriate fees associated with withdrawal of funds from one account.

Write a program with a base class for a bank account and two derived
classes (as described below) representing accounts with different
rules for withdrawing funds. Also write a function that transfers
funds from one account (of any type) to another. A transfer is a
withdrawal from one account and a deposit into the other. Since the
transfer can be done at any time with any type of account the withdraw
function in the classes must be virtual. The transfer function utlizes
polymorphism in order to transfer funds between *any* subclass of
`BankAccount`. So the transfer function should receive `BankAccount`
pointers as the "from" and "to" bank accounts.

Write a main function that creates two accounts (one from
`MoneyMarketAccount` and one from `CDAccount`) and tests the transfer
function.

For the classes, create a base class called `BankAccount` that has the name of
the owner of the account (a `string`) and the balance in the account (a
`double`) as data members. Include member functions `deposit` and `withdraw`
(each with a `double` for the amount as an argument) and accessor functions
`getName` and `getBalance`. `deposit` will add the amount to the balance
(assuming the deposit amount is nonnegative) and `withdraw` will subtract the
amount from the balance (assuming the withdraw amount is nonnegative and less
than or equal to the balance).

Also create a class called `MoneyMarketAccount` that is derived from
`BankAccount`. In a `MoneyMarketAccount` the user gets 2 free withdrawals in a
given period of time (don't worry about the time; just allow a maximum of 2
free withdrawals *ever*). After the free withdrawals have been used, a
withdrawal fee of $1.50 is deducted from the balance per withdrawal. Hence, the
class must have a data member to keep track of the number of withdrawals. It
also must override the `withdraw` definition.

Finally, create a `CDAccount` class (to model a Certificate of Deposit) derived
from `BankAccount` which in addition to having the name and balance, also has
an interest rate. CDs incur penalties for early withdrawal of funds. Assume
that a withdrawal of funds (any amount) incurs a penalty of 25% of the annual
interest earned on the account (just assume that the annual interest is equal
to the interest rate times the current balance). Assume the amount withdrawn
plus the penalty are deduced from the account balance. Again, the `withdraw`
function must override the one in the base class.

For all three classes, the withdraw function should return a `bool` indicating
the status (false if amount is negative (that's really a deposit) or
insufficient funds for the withdrawal to take place). The deposit function
should return a `bool` as well; deposit should return `false` if the amount to
deposit is negative (that's really a withdraw). The point of the `bool` return
values is to indicate success or failure, because these functions may refuse to
actually withdraw or deposit if conditions aren't right (we don't want to allow
users of the bank account classes to get away with making a withdraw look like
a deposit or vice versa). For the purposes of this exercise, do not worry about
other functions and properties of these accounts (such as when and how interest
is paid).

A strategy for finishing this assignment is to work on the `BankAccount` and
`MoneyMarketAccount` classes first. Then, when those are working, add the
`CDAccount` class.

Here is a diagram of the classes.

![Bank Account UML diagram](/images/bankaccount-uml.png "Bank Account UML diagram")

Note, if you want `balance` not to be `public` in the `BankAccount` class then
you'll actually need to make it `protected`. This ensures that it will be
accessible by subclasses but remain private in the subclasses.

## Example execution

<pre>
Enter Josh's MoneyMarketAccount balance: 100
Enter amount to withdraw from Josh: 20
That worked.
Josh's new balance: 80
Enter amount to withdraw from Josh (again): 30
That worked.
Josh's new balance: 50
Enter Tracy's CDAccount balance: 50
Enter Tracy's CDAccount interest rate: 0.5
Enter amount to deposit into Tracy's account: 30
That worked.
Tracy's new balance: 80
Enter amount to transfer from Tracy to Josh: 60
That worked.
Tracy's new balance: 10
Josh's new balance: 110
Enter amount to transfer from Josh to Tracy: 100
That worked.
Tracy's new balance: 110
Josh's new balance: 8.5
</pre>

## Common compiler errors

<pre>
/tmp/ccJDhhaM.o: In function `BankAccount::BankAccount()':
CDAccount.cpp:(.text._ZN11BankAccountC2Ev[_ZN11BankAccountC5Ev]+0x13):
undefined reference to `vtable for BankAccount'
/tmp/ccJDhhaM.o: In function `BankAccount::~BankAccount()':
CDAccount.cpp:(.text._ZN11BankAccountD2Ev[_ZN11BankAccountD5Ev]+0x13):
undefined reference to `vtable for BankAccount'
</pre>

This means you have `virtual bool withdraw(double amount)` or similar
in `BankAccount.h` but you forgot to add `= 0` at the end of that. We
need the `= 0` so that `withdraw()` is a "pure virtual" function.

<pre>
main.cpp: In function `int main()':
main.cpp:48:50: error: cannot allocate an object of abstract type `CDAccount'
CDAccount.h:6:7: note: 
  because the following virtual functions are pure within `CDAccount':
BankAccount.h:13:18: note: virtual bool BankAccount::withdraw(double)
main.cpp:48:15: error:
  cannot declare variable `acct2' to be of abstract type `CDAccount'
CDAccount.h:6:7: note: since type `CDAccount' has pure virtual functions
</pre>

This means you forgot to provide the code for a pure virtual function
(such as `withdraw()` or `deposit()`) in one of your subclasses.
