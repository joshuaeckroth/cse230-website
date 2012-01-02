---
title: rectangle.cpp (Rectangle code)
layout: default
---

{% highlight cpp %}
#include <iostream>
using namespace std;

class Rectangle
{
private:
    double width, height;
public:
    Rectangle(double w, double h);
    double area();
    Rectangle flip();
    Rectangle grow(double amount);
    bool smaller(Rectangle &other);
    void print();
};

void Rectangle::print()
{
    for(int i = 0; i < (int)height; i++)
    {
        for(int j = 0; j < (int)width; j++)
        {
            cout << "*";
        }
        cout << endl;
    }
}

Rectangle Rectangle::flip()
{
    double w, h;
    w = height;
    h = width;
    return Rectangle(w, h);
}

Rectangle Rectangle::grow(double amount)
{
    return Rectangle(width + amount, height + amount);
}

// Rectangle a is smaller than Rectangle b if
// a's area is less than or equal to b's area
bool Rectangle::smaller(Rectangle &other)
{
    double myarea = area();
    double other_area = other.area();
    return (myarea <= other_area);
}

Rectangle::Rectangle(double w, double h)
{
    if(w <= 0.0 || h <= 0.0)
    {
        width = height = 1.0;
    }
    else
    {
        width = w;
        height = h;
    }
}

double Rectangle::area()
{
    return width * height;
}


Rectangle readRectangle()
{
    double w, h;
    cin >> w >> h;
    return Rectangle(w, h);
}
{% endhighlight %}
