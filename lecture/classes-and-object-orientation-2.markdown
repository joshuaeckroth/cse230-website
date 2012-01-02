---
title: Classes and object orientation (part 2)
layout: default
---

These lecture notes spell out the details of creating and using
classes in C++.

## A more concrete example

Actually writing a game in C++ (like we discussed in
[Classes and object orientation](/lecture/classes-and-object-orientation.html))
is too much work for right now. Let's bring the discussion back to
something very basic: a Shape class and some subclasses.

![Shape diagram](/images/shape-uml.png "Shape diagram")

Now we'll write the C++ code. First, the Shape class.

{% highlight cpp %}
class Shape
{
public:
    double x;
    double y;
};
{% endhighlight %}

We use the keyword `public` for technical reasons (alternatives are
`private` and `protected`). Eventually we'll learn why we need that,
but for now just consider it to be a necessary addition in order for
the inheritance to work properly.

Next, the `Rectangle` class. On the first line, we'll write `Rectangle
: public Shape` to indicate that `Rectangle` inherits properties and
methods from `Shape` (`public` again, to make the inheritance
work). We'll also write a function header for the `area` method.

{% highlight cpp %}
class Rectangle : public Shape
{
public:
    double width;
    double height;

    double area();
};
{% endhighlight %}

The other classes are similarly written:

{% highlight cpp %}
class Ellipse : public Shape
{
public:
    double major_axis;
    double minor_axis;

    double area();
};

class Triangle : public Shape
{
public:
    double side1;
    double side2;
    double angle_between;

    double area();
};
{% endhighlight %}

Let's write the code for the `area` functions. We could have written
the code in the class declaration, but it's more common to write the
code *outside* the class declaration (actually, in `.cpp` files,
whereas the class declarations above are in `.h` files). If we just
write `void area() { ... }` then the compiler won't know which `area`
function we are defining (is it Rectangle's?  is it Triangle's?). So
we use the `::` syntax to indicate which class method we are defining.

{% highlight cpp %}
double Rectangle::area()
{
    return width * height;
}

double Ellipse::area()
{
    return 3.1415926 * major_axis * minor_axis;
}

double Triangle::area()
{
    return 0.5 * side1 * side2 * sin(angle_between);
}
{% endhighlight %}

Here is a simple `main` function to test some of these features:

{% highlight cpp %}
int main()
{
    Triangle t;
    t.x = 5;
    t.y = 4;
    t.side1 = 3;
    t.side2 = 4;
    t.angle_between = 3.1415926/2.0;

    cout << t.area() << endl;
    cout << t.x << "," << t.y << endl;
}
{% endhighlight %}

We see on the screen:

<pre>
6
5,4
</pre>

## Constructors

Constructors are special functions that are used to give an object
some initial values. Our `Rectangle` class, for example, has the
properties `width`, `height`, `x`, and `y` (the latter two are
inherited from the `Shape` class). Let's say we want to provide a
constructor so that a programmer can create a new `Rectangle` instance
and provide these values all at once.

There are several kinds of constructors. For now, we'll just worry
about the simple kind. This simple kind of constructor is a function
that has the same name as the class (`Rectangle`) and returns
*nothing*. It has no return type! Not even `void`. Weird, huh? You'll
see that C++'s object-orientation features require some gymnastic
maneuvers to get all the details right.

Here is our `Rectangle` class, with a constructor header added.

{% highlight cpp %}
class Rectangle : public Shape
{
public:
    double width;
    double height;

    Rectangle(double _width, double _height,
              double _x, double _y);
    double area();
};
{% endhighlight %}

We can define the constructor later like so:

{% highlight cpp %}
Rectangle::Rectangle(double _width, double _height,
                     double _x, double _y)
{
    width = _width;
    height = _height;
    x = _x;
    y = _y;
}
{% endhighlight %}

Notice we name the parameters `_width`, `_height`, etc. with
underscores in front in order to still be able to refer to the
Rectangle class variables `width`, `height`, etc.

Now, when the `Rectangle` class is used, say inside the `main` method,
this particular constructor can be used to create a new instance:

{% highlight cpp %}
int main()
{
    Rectangle r(10.0, 10.0, 9.3, 2.3);
    cout << r.width << endl;
    cout << r.height << endl;
    // etc...
}
{% endhighlight %}

Of course, the constructor can do anything; it's a normal
function. This example was just a simple constructor that set some
values; this kind of constructor is very common, however.

## Printing an object

Our `Rectangle`, `Triangle`, `Ellipse`, and `Shape` classes, as they
exist at this point, cannot be printed. We would like to create a
`Rectangle` called `r` and use code like `cout << r << endl;` to print
information about the rectangle.

C++ makes that task too complicated for the moment, but we can achieve
something close to that. Just define a `void` method called, perhaps,
`print()` and use `cout` to show the object information on the screen.

{% highlight cpp %}
class Rectangle : public Shape
{
public:
    double width;
    double height;

    Rectangle(double _width, double _height,
              double _x, double _y);
    double area();
    void print();
};
{% endhighlight %}

And somewhere below is the function:

{% highlight cpp %}
void Rectangle::print()
{
    cout << "Rectangle " << width << " by " << height
         << " at position " << x << "," << y << endl;
}
{% endhighlight %}

This `print` function can be used in `main` (or wherever):

{% highlight cpp %}
int main()
{
    Rectangle r(10.0, 10.0, 9.3, 2.3);
    r.print();
}
{% endhighlight %}

We get the following result:

<pre>
Rectangle 10 by 10 at position 9.3,2.3
</pre>

## Reading an object

We want to create a new shape based on input from the user. To do so,
we'll create a function that's *not* a class member function but
rather an external function (which should probably still be mentioned
in the particular shape's `.h` file and programmed in the `.cpp`
file). The reason not to use a class member function is that we should
not be required to create an object before we read its values; what
values would we use to create the object if we don't yet know its
values?

{% highlight cpp %}
class Rectangle : public Shape
{
public:
    double width;
    double height;

    Rectangle(double _width, double _height,
              double _x, double _y);
    double area();
    void print();
};

void readRectangle(); // <--- new
{% endhighlight %}

The `readRectangle` function may be defined as follows.

{% highlight cpp %}
Rectangle readRectangle()
{
    int width, height, x, y;
    cin >> width >> height >> x >> y;
    // now defer to the constructor
    return Rectangle(width, height, x, y);
}
{% endhighlight %}

Here is a use of the `readRectangle` function:

{% highlight cpp %}
cout << "Enter rectangle's new width, height, x, y: ";
Rectangle r = readRectangle();  // we use the read function here
r.print();
cout << endl;
{% endhighlight %}

## Methods that return new objects

Let's make a method inside the `Rectangle` class that rotates the
rectangle 90 degrees (assume the `x` and `y` coordinates indicate the
center of the rectangle, so rotating the rectangle around x,y will not
change these values).

{% highlight cpp %}
class Rectangle : public Shape
{
public:
    double width;
    double height;

    Rectangle();
    Rectangle(double _width, double _height,
              double _x, double _y);
    double area();
    Rectangle flip();  // <--- new
    void print();
};

Rectangle readRectangle();
{% endhighlight %}

{% highlight cpp %}
Rectangle Rectangle::flip()
{
    // put height first, width second, to rotate 90 degrees
    return Rectangle(height, width, x, y);
}
{% endhighlight %}

Here is how we might use it:

{% highlight cpp %}
Rectangle r(8.0, 12.0, 9.3, 2.3);
r.print();
cout << endl;

Rectangle r_flipped = r.flip();
r_flipped.print();
cout << endl;
{% endhighlight %}

And here is the result:

<pre>
Rectangle 8 by 12 at position 9.3,2.3
Rectangle 12 by 8 at position 9.3,2.3
</pre>

## Constructors and inheritance

When a class inherits from another class, often the subclass will want
to have its own constructors that refer back to the parent class's
constructors. For example, consider the following class hierarchy:

![Bank Account UML diagram](/images/bankaccount-uml.png "Bank Account UML diagram")

The `BankAccount` class should have a constructor that allows the
`owner` and `balance` properties to be set:

{% highlight cpp %}
class BankAccount
{
    string owner;   // these are private
    double balance;

public:
    BankAccount();
    BankAccount(string _owner);
    BankAccount(string _owner, double _balance);

    // ... other methods
};

// default constructor
BankAccount::BankAccount()
{
    owner = "";
    balance = 0.0;
}

BankAccount::BankAccount(string _owner)
{
    owner = _owner;
    balance = 0.0;
}

BankAccount::BankAccount(string _owner, double _balance)
{
    owner = _owner;
    balance = _balance;
}
{% endhighlight %}

Now, the `MoneyMarketAccount` class should use those constructors in its
own constructors, like so:

{% highlight cpp %}
class MoneyMarketAccount : public BankAccount
{
    int numWithdraws; // private data

    public:
    MoneyMarketAccount();
    MoneyMarketAccount(string _owner);
    MoneyMarketAccount(string _owner, double _balance);
};

MoneyMarketAccount::MoneyMarketAccount()
  : BankAccount()
{
    numWithdraws = 0;
}

MoneyMarketAccount::MoneyMarketAccount(string _owner)
  : BankAccount(_owner)
{
    numWithdraws = 0;
}

MoneyMarketAccount::MoneyMarketAccount(string _owner, double _balance)
  : BankAccount(_owner, _balance)
{
    numWithdraws = 0;
}
{% endhighlight %}

In order to call the parent's constructor, we use the `:` followed by
the parent's constructor function call (e.g. `BankAccount(_owner)`).

## Public and private data and methods

C++ allows a programmer to protect data and methods in a class from
being accessed by other code that's not part of the class. If we
create a class but don't specify anything as `public:` then it's all
private.

The `BankAccount` class above has two private data members (indicated
by the minus signs in the diagram): `owner` and `balance`. Why should
those be private? We don't want to allow other classes or other code
to decide that the owner has changed, or that the balance has changed
without using the `deposit` or `withdraw` function.

Here is how we specify that those variables are private:

{% highlight cpp %}
class BankAccount
{
private: // not necessary; default is private
    string owner;
    double balance;

    // ...
};
{% endhighlight %}

The construct for `BankAccount` (as shown above) sets the values for
`owner` and `balance`, and the `deposit` and `withdraw` functions
change the balance.  However, given a `BankAccount` object
`myaccount`, how do we find out the owner and the balance? These are
private data members, so we are not allowed to do this: `cout <<
myaccount.balance << endl;`

Instead, we would add "getters" and "setters":

{% highlight cpp %}
class BankAccount
{
private: // not necessary; default is private
    string owner;
    double balance;

public:
    string getOwner();
    void setOwner(string _owner);
    double getBalance();

    // ...
};
{% endhighlight %}

Now we can find out the balance like this: `cout <<
myaccount.getBalance() << endl;` Notice there is no function for
changing the balance (except via `deposit` and `withdraw`, which are
not shown here).

A class may also have private methods. For example, maybe both the
`deposit` and `withdraw` methods call a method named
`notifyOwnerByEmail` that send deposit/withdraw receipts to the
owner. This method should *only* be used after a deposit or withdraw,
so there is no reason for other code to call `notifyOwnerByEmail`
directly. Thus, that method should be private, so that only code that
is part of the `BankAccount` class can use the method.

If all constructors are marked as private, then no instances of the
object can be created. This is a rarely-used but sometimes necessary
feature.

> **class** *n. & adj.*
> [Origin: Latin *classis* via *calare*, "to call to arms."] **1**
> *n.* (Object-orienteering) Data members encapsulated with a set of
> methods dying to get at them. **2** *n.* (Marxism) A subset of
> society encapsulated with a set of methods for exploiting and
> exterminating both itself and other subsets of society. **3** *n.*
> (Style) Someth'n' you jest plain got or don't. **4** *adj.* (Of a
> struggle) iterative, as in the attempted modularization of
> real-world activities. -- *The computer contradictionary*

