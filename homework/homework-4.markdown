---
title: Homework 4
layout: default
---

From the book, page 525, question 5.

You run four computer labs. Each lab contains computer stations that are
numbered as shown in the table below:

| Lab number | Computer station numbers |
|------------|--------------------------|
| 1 | 1--5 |
| 2 | 1--6 |
| 3 | 1--4 |
| 4 | 1--3 |


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

