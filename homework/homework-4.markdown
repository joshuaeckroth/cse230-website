---
title: Homework 4
layout: commentable
---

From the book, page 525, question 5. Due July 25, 11pm.

You run four computer labs. Each lab contains computer stations that are
numbered as shown in the table below:

Lab number | Computer station numbers 
-----------|-------------------------
1          | 1--5
2          | 1--6
3          | 1--4
4          | 1--3


Each user has a unique five-digit ID number. Whenever a user logs on, the
user's ID, lab number, and the computer station number are transmitted to your
system. For example, if user 49193 logs onto station 2 in lab 3, then your
system receives (49193, 2, 3) as input data. Similarly, when a user logs off a
station, then your system receives the lab number and computer station number.

Write a computer program that could be used to track, by lab, which user is
logged onto which computer. For example, if user 49193 is logged into station 2
in lab 3 and user 99577 is logged into station 1 of lab 4, then your system
might display the following:

<pre>
Lab number  Computer stations
1           1: empty 2: empty 3: empty 4: empty 5: empty
2           1: empty 2: empty 3: empty 4: empty 5: empty 6: empty
3           1: empty 2: 49193 3: empty 4: empty
4           1: 99577 2: empty 3: empty
</pre>

Create a menu that allows the administrator to simulate the transmission of
information by manually typing in the login or logoff data. Whenever someone
logs in or out, the display should be updated. Also write a search option so
that the administrator can type in a user ID and the system will output what
lab and station number the user is logged into, or "None" if the user ID is not
logged into any computer station.

You should use a fixed array of length 4 for the labs. Each array entry points
to a dynamic array that stores the user login information for each respective
computer station.

The structure is shown in the figure below. This structure is sometimes called
a ragged array since the columns are of unequal length (unlike a simple 2D
array "matrix").

![Ragged array](/images/ragged-array.png "Ragged array")

I was emailed by a student during the Spring quarter with this question:

> I have read through homework assignment 4 and I just can't figure out what
exactly to do. I don't think I understand what it is you want.

> I'm having a really hard time even figuring out what question to ask to
clarify this for me. Is there any more information you can give me? Such as
what all is to be automated, so it's done from the running program, and what is
to be actually changed in the code each time?

Here is my reply.

> Primarily, your program displays who is logged into what machine in what lab.
The data is stored in arrays of some variety.

> Your program should display the information, then below that provide a menu
of options; it's up to you how this this done. E.g.,

> ...lab assignments shown...

> Type l to login, L to logout, s to search.

> If you type l, it asks for the user id and which lab & station. If you type L
it asks which user id. If you type s, it asks which user id and tells if the
user is logged in somewhere, and if so, which lab/station. The updated display
is shown if l or L is typed.

> You can decide how it looks and works; that's an example.

