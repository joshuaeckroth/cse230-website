---
title: Classes and object orientation (part 2)
layout: default
---

These lecture notes spell out the details of creating and using classes in C++.

## A more concrete example

Actually writing a game in C++ (like we discussed in [Classes and object
orientation](/lecture/classes-and-object-orientation.html)) is too much work
for right now. Let's bring the discussion back to something very basic: a Shape
class and some subclasses.

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

(Notice the closing semicolon. We did this on `struct` declaration as well.) We
use the keyword `public` for technical reasons (alternatives are `private` and
`protected`). Eventually we'll learn why we need that, but for now just
consider it to be a necessary addition in order for the inheritance to work
properly.

Next, the Rectangle class. On the first line, we'll write `Rectangle : public
Shape` to indicate that Rectangle inherits properties and methods from Shape
(`public` again, to make the inheritance work). We'll also write a function
header for the `area` method.

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

Let's write the code for the `area` functions. We could have written the code
in the class declaration, but it's more common to write the code *outside* the
class declaration (actually, in `.cpp` files, whereas the class declarations
above are in `.h` files). If we just write `void area() { ... }` then the
compiler won't know which `area` function we are defining (is it Rectangle's?
is it Triangle's?). So we use the `::` syntax to indicate which class method we
are defining.

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

Constructors are special functions that are used to give an object some initial
values. Our Rectangle class, for example, has the properties `width`, `height`,
`x`, and `y` (the latter two are inherited from the Shape class). Let's say we
want to provide a constructor so that a programmer can create a new Rectangle
instance and provide these values all at once.

There are several kinds of constructors. For now, we'll just worry about the
simple kind. This simple kind of constructor is a function that has the same
name as the class (Rectangle) and returns *nothing*. It has no return type! Not
even `void`. Weird, huh? You'll see that C++'s object-orientation features
require some gymnastic maneuvers to get all the details right.

Here is our Rectangle class, with a constructor header added.

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

Notice we name the parameters `_width`, `_height`, etc. with underscores in
front in order to still be able to refer to the Rectangle class variables
`width`, `height`, etc.

Now, when the Rectangle class is used, say inside the `main` method, this
particular constructor can be used to create a new instance:

{% highlight cpp %}
int main()
{
    Rectangle r(10.0, 10.0, 9.3, 2.3);
    cout << r.width << endl;
    cout << r.height << endl;
    // etc...
}
{% endhighlight %}

Of course, the constructor can do anything; it's a normal function. This
example was just a simple constructor that set some values; this kind of
constructor is very common, however.

## Printing an object

Our Rectangle, Triangle, Ellipse, and Shape classes, as they exist at this
point, cannot be printed. It's not possible to create a Rectangle called `r`
and use code like `cout << r << endl;`

But we want that to be possible, or at least something close to that. There are
lots of technical details that are involved in making this work. Technically,
`cout` is an object and `<<` is an operator which may be redefined by a
function, and `<<` works on streams, which are a certain kind of object
(class).

But regardless of all that, here is the basic recipe. You need to make a void
function (you choose the name) that has a single `ostream` call-by-reference
parameter. First we'll add the function to the class declaration:

{% highlight cpp %}
class Rectangle : public Shape
{
    public:
    double width;
    double height;

    Rectangle(double _width, double _height,
              double _x, double _y);
    double area();
    void print(ostream &out);
};
{% endhighlight %}

And somewhere below is the function:

{% highlight cpp %}
void Rectangle::print(ostream &out)
{
    // notice the following is "out" not "cout"
    out << "Rectangle " << width << " by " << height
        << " at position " << x << "," << y;
}
{% endhighlight %}

Instead of `cout`, the parameter `out` is used to output the object's
information. The parameter `out` may refer to a file, not the screen; `cout`
only refers to the screen.

This `print` function can be used in `main` (or wherever):

{% highlight cpp %}
int main()
{
    Rectangle r(10.0, 10.0, 9.3, 2.3);
    r.print(cout);
}
{% endhighlight %}

The code `r.print(cout)` is just saying that the `print` function of the
Rectangle class should be executed, with input `cout` (the "output stream") and
Rectangle instance `r`.

We get the following result:

<pre>
Rectangle 10 by 10 at position 9.3,2.3
</pre>


## Reading an object

This is similar to what was done previously. The Rectangle class declaration is
modified:

{% highlight cpp %}
class Rectangle : public Shape
{
    public:
    double width;
    double height;

    Rectangle(double _width, double _height,
              double _x, double _y);
    double area();
    void print(ostream &out);
    void read(istream &in); // <--- new
};
{% endhighlight %}

The `read` function may be defined as follows.

{% highlight cpp %}
void Rectangle::read(istream &in)
{
    in >> width >> height >> x >> y;
}
{% endhighlight %}

Note we use `in` instead of `cin`. The rectangle information may actually be
coming from a file, not the keyboard. By using an `ifstream` object as an input
to the function, we can treat `cin` and file input the same.

Here is a use of the `read` function:

{% highlight cpp %}
Rectangle r(10.0, 10.0, 9.3, 2.3);
r.print(cout);
cout << endl;

cout << "Enter rectangle's new width, height, x, y: ";
r.read(cin);  // we use the read function here
r.print(cout);
cout << endl;
{% endhighlight %}

Notice in that example that a Rectangle instance was created (`r`) with
particular values (10.0, etc.), but then its information was read from the user
and those values were changed. If we expect to get the rectangle's values from
the user, then we should not be required to specify them ahead of time. In
other words, we want a constructor that has no parameters. Such a constructor
is provided for free (it simply does nothing) *only when* other constructors
have not been defined. Since we defined another constructor (which has four
`double` parameters), the empty default constructor is no longer provided for
free. So we'll just add it:

{% highlight cpp %}
class Rectangle : public Shape
{
    public:
    double width;
    double height;

    Rectangle();  // <--- new
    Rectangle(double _width, double _height,
              double _x, double _y);
    double area();
    void print(ostream &out);
    void read(istream &in);
};
{% endhighlight %}

Here is its code.

{% highlight cpp %}
Rectangle::Rectangle()
{
    width = height = x = y = 0.0;
}
{% endhighlight %}

This default constructor just sets all the properties to a default value.

Now, when a Rectangle object is created with no parameters, it is this default
constructor that is called.

{% highlight cpp %}
Rectangle r;
r.print(cout);
{% endhighlight %}

We see this on the screen:

<pre>
Rectangle 0 by 0 at position 0,0
</pre>

## Methods that return new objects

Let's make a method (function) inside the Rectangle class that rotates the
rectangle 90 degrees (assume the `x` and `y` coordinates indicate the center of
the rectangle, so rotating the rectangle around x,y will not change these
values).

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
    void print(ostream &out);
    void read(istream &in);
};
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
r.print(cout);
cout << endl;

Rectangle r_flipped = r.flip();
r_flipped.print(cout);
cout << endl;
{% endhighlight %}

And here is the result:

<pre>
Rectangle 8 by 12 at position 9.3,2.3
Rectangle 12 by 8 at position 9.3,2.3
</pre>

## Constructors and inheritance

When a class inherits from another class, often the subclass will want to have
its own constructors that refer back to the parent class's constructors. For
example, consider the following class hierarchy:

![Bank Account UML diagram](/images/bankaccount-uml.png "Bank Account UML diagram")

The BankAccount class should have a constructor that allows the `owner` and
`balance` properties to be set:

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

Now, the MoneyMarketAccount class should use those constructors in its own
constructors, like so:

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

In order to call the parent's constructor, we use the `:` followed by the
parent's constructor function call (e.g. `BankAccount(_owner)`).


## Public and private data and methods

C++ allows a programmer to protect data and methods in a class from being
accessed by other code that's not part of the class. If we create a class but
don't specify anything as `public:` then it's all private.

The BankAccount class above has two private data members (indicated by the
minus signs in the diagram): `owner` and `balance`. Why should those be
private? We don't want to allow other classes or other code to decide that the
owner has changed, or that the balance has changed without using the `deposit`
or `withdraw` function.

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

The construct for BankAccount (as shown above) sets the values for `owner` and
`balance`, and the `deposit` and `withdraw` functions change the balance.
However, given a BankAccount object `myaccount`, how do we find out the owner
and the balance? These are private data members, so we are not allowed to do
this: `cout << myaccount.balance << endl;`

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

Now we can find out the balance like this: `cout << myaccount.getBalance() <<
endl;` Notice there is no function for changing the balance (except via
`deposit` and `withdraw`, which are not shown here).

A class may also have private methods. For example, maybe both the `deposit`
and `withdraw` methods call a method named `notifyOwnerByEmail` that send
deposit/withdraw receipts to the owner. This method should *only* be used after
a deposit or withdraw, so there is no reason for other code to call
`notifyOwnerByEmail` directly. Thus, that method should be private, so that
only code that is part of the BankAccount class can use the method.

If all constructors are marked as private, then no instances of the object can
be created. This is a rarely-used but sometimes necessary feature.

> **class** *n. & adj.* [Origin: Latin *classis* via *calare*, "to call to
> arms."] **1** *n.* (Object-orienteering) Data members encapsulated with a set
> of methods dying to get at them. **2** *n.* (Marxism) A subset of society
> encapsulated with a set of methods for exploiting and exterminating both
> itself and other subsets of society. **3** *n.* (Style) Someth'n' you jest
> plain got or don't. **4** *adj.* (Of a struggle) iterative, as in the
> attempted modularization of real-world activities. -- 
> *The computer contradictionary*

