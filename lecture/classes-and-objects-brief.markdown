---
title: Classes and objects (brief introduction)
layout: default
---

At this point in the class, all we have seen are
[variables and types](/lecture/variables-and-types.html). We'll look
at "classes" very briefly here, so we can become familiar with the
concept before we truly exploit it.

A class defines a new type. Like `int` is a type, we can create our
own type, maybe called `Person`. (We typically capitalize types that
we create with classes.) Class types are always built out of other
types; you can think of a class as a composite of other types.

For example, here we define the `Person` class:

{% highlight cpp %}
// this goes above "main()"

class Person
{
public:
    string name;
    int age;       // in years
    double height; // in cm
    double weight; // in kg
};
{% endhighlight %}

Once we have defined the class, we can create *objects* of that
type. In this case, the class is `Person` (a general idea or
classification) and each object will represent some *particular*
person:

{% highlight cpp %}
int main()
{
    Person vignesh;
    vignesh.name = "Vignesh S.";
    vignesh.age = 25;
    vignesh.height = 177;
    vignesh.weight = 68;
    
    cout << vignesh.name << " weighs " << vignesh.weight << " kg." << endl;
    
    return 0;
}
{% endhighlight %}

Classes will appear again in the
[function lecture notes](/lecture/functions.html) where we'll add
functions *inside* the classes.
