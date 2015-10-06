---
layout: default
course_number: CS350
title: "Lab 7: Binary search trees"
---


###Getting Started
------------------

Download [CS350_Lab07.zip](CS350_Lab07.zip).  Unzip it.  
In a terminal window, change directory to the **```CS350_Lab07```** directory.

Using a text editor, open the file **```main.cpp```**.

To compile the test program, run the command **```make```**.

To run the program, run the command **```./lab07```**.


<br>
###Your Task
-------------

The **```Node```** class implements a binary search tree where the
keys are integer values.

Your task is to implement the **```printKeysInOrder```** and **```find```** methods.

The **```printKeysInOrder```** method should do an in-order traversal,
printing each key using **```std::cout```**.  Print all of the keys on a single line.

The **```find```** method should find the node containing the key equal to the parameter, **```searchKey```**.  It should return **```NULL```** if there is no node containing the search key.

When you run the program, it will create a random binary search
tree, print its keys (using **```printKeysInOrder```**), and then
allow the user to search for specified keys.

Example run (user input in **bold**):

<pre>
The BST contains the following keys:
17 18 19 22 23 24 25 30 32 33 40 41 42 43 44 45 
Enter a key to search for (-1 to quit): <b>20</b>
Not found
Enter a key to search for (-1 to quit): <b>25</b>
Found!
Enter a key to search for (-1 to quit): <b>-1</b>
</pre>


<br>
####Hints

For the **```find```** method, compare the search key to the node that the **```find```** method is being called on.  If the keys are equal, the search is done.  Otherwise, continue the search recursively in the left or right subtrees as appropriate.  Make sure you check whether the left or right subtree exists before attempting to continue the search.
