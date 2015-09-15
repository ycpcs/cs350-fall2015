---
layout: default
course_number: CS350
title: "Lab 5: Linked list implementation"
---


###Getting Started
------------------

Download [CS350_Lab05.zip](CS350_Lab05.zip).  Unzip it.  
In a terminal window, change directory to the **```CS350_Lab05```** directory.

Using a text editor, open the file **```main.cpp```**.

To compile the test program, run the command **```make```**.

To run the program, run the command **```./lab05```**.


<br>
###Your Task
-------------

Complete the program in **```main.cpp```** so that it stores the sequence of integer values entered by the user in a linked list.

Note that in this lab, you will create a "bare" linked list
consisting of **```Node```** objects.  You will use a **```head```** pointer to keep track of the first node in the linked list, and (for
the first two milestones) a **```tail```** pointer to keep track of the last node in the linked list.

*All of the code manipulating the linked list should be in the*
**```main```** *function.*  For this lab, you will not implement the linked list operations using functions/methods.  The idea is to focus
on the low-level manipulation of the links that connect the nodes
in the list.

The **```Node```** type, in its simplest form, is defined as follows:

```cpp
class Node {
public:
    Node(int v) : value(v), next(NULL) { }
    int value;
    Node *next;
};
```

There are three milestones.  **Important**: after you complete each
milestone, **save a copy of** **```main.cpp```**.  For example, after completing milestone 1, save your work in a file called **```main_ms1.cpp```**.


<br>
###Milestone 1
---------------

Store each integer value entered by the user in the linked list.
Each node in the list should store one value.

After the user has entered the sequence of values, the program
should print the values in order.

Example run (user input in **bold**):

<pre>
Enter integer values (-1 to end):
<b>9</b>
<b>0</b>
<b>1</b>
<b>2</b>
<b>5</b>
<b>-1</b>
Here are your values:
9
0
1
2
5
</pre>

<br>
####Hints

**```head```** should always point to the first node in the list, and **```tail```** should always point to the last node in the list.

Think about how the empty list should be represented.  Setting
both **```head```** and **```tail```** to **```NULL```** when the list is empty is the most straightforward approach.

To traverse the list in order to print the values, use a
**```Node```**\* variable as the loop variable.  On the first
iteration of the loop, the variable should point to the
first node in the list.  At the end of each iteration of the
loop, the variable should advance so that it points to the
next node in the list.  Think about how to express a loop condition
that will terminate the loop when there are no more nodes to
traverse.



<br>
###Milestone 2
------------------

For the second milestone:

* use a doubly-linked list
* in addition to printing the values in order, also print them in reverse order

Example run (user input in **bold**):

<pre>
Enter integer values (-1 to end):
<b>9</b>
<b>0</b>
<b>1</b>
<b>2</b>
<b>5</b>
<b>-1</b>
Here are your values:
9
0
1
2
5
Here are your values, reversed:
5
2
1
0
9
</pre>

<br>
####Hints

You will need to add a **```prev```** field to the **```Node```** class.

When adding a node to the list, make sure that the **```prev```** field of the inserted node points to the old tail node (if any).

Iterating backwards through a doubly-linked list is very
similar to iterating forward, except

* start at the end (tail), and
* follow the **```prev```** links instead of the **```next```** links


<br>
###Milestone 3
------------------

For the third milestone:

* get rid of the **```tail```** pointer
* use a single dummy (sentinel) node
* implement a circular doubly-linked list

This is the doubly-linked list representation that you will use in [Assignment 2](../assign/assign02.html).

 
