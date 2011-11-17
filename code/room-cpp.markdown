---
title: room.cpp (HW 7 code)
layout: default
---

{% highlight cpp %}
#include <iostream>
#include <string>
#include <map>
using namespace std;

class Room
{
public:
    string name;
    map<string, Room*> exits;
};

int main()
{
    Room a, b, c, d;
    a.name = "Library";
    b.name = "Kitchen";
    c.name = "Hallway";
    d.name = "Dungeon";
    a.exits["east"] = &b;
    b.exits["west"] = &a;
    b.exits["downstairs"] = &c;
    c.exits["upstairs"] = &b;
    c.exits["south"] = &d;
    d.exits["east"] = &a;

    Room *cur_room = &a;

    while(true)
    {
        cout << "You are in the: " << cur_room->name << endl;

        cout << "Enter the name of an exit: ";
        string choice;
        cin >> choice;
        map<string,Room*>::iterator pos = cur_room->exits.find(choice);
        if(pos != cur_room->exits.end())
        {
            cur_room = pos->second;
        }
        else
        {
            cout << "Not a valid choice." << endl;
        }
    }
}
{% endhighlight %}
