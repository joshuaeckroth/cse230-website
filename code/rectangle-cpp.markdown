---
title: rectangle.cpp (Rectangle code)
layout: default
---

{% highlight cpp %}
#include <iostream>
#include <fstream>
using namespace std;

class Rectangle
{
private:
    double width, height;
public:
    Rectangle(double w, double h);
    double area() const;
    Rectangle flip() const;
    Rectangle grow(double amount) const;
    bool smaller(const Rectangle &other) const;
    void output(ostream &out) const;
};

void Rectangle::output(ostream &out) const
{
    for(int i = 0; i < (int)height; i++)
    {
        for(int j = 0; j < (int)width; j++)
        {
            out << "*";
        }
        out << endl;
    }
}

Rectangle Rectangle::flip() const
{
    double w, h;
    w = height;
    h = width;
    return Rectangle(w, h);
}

Rectangle Rectangle::grow(double amount) const
{
    return Rectangle(width + amount, height + amount);
}

// Rectangle a is smaller than Rectangle b if
// a's area is less than or equal to b's area
bool Rectangle::smaller(const Rectangle &other) const
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

double Rectangle::area() const
{
    return width * height;
}


Rectangle input(istream &in)
{
    double w, h;
    in >> w >> h;
    return Rectangle(w, h);
}

int main()
{
    ofstream fout;
    fout.open("myoutput.txt");

    ifstream fin;
    fin.open("rectangles.txt");

    int num;
    fin >> num;
    for(int i = 0; i < num; i++)
    {
        Rectangle r = input(fin);
        r.output(fout);
    }

    fin.close();
    fout.close();

    return 0;
}
{% endhighlight %}