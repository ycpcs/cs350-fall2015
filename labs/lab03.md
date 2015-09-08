---
layout: default
course_number: CS350
title: "Lab 3: Value semantics in C++"
---


###Getting Started
------------------

Download [CS350_Lab03.zip](CS350_Lab03.zip).  Unzip it.  
In a terminal window, change directory to the **```CS350_Lab03```** directory.

Using a text editor, open the file **```main.cpp```**.

To compile the test program, run the command **```make```**.

To run the program, run the command **```./lab03```**.



<br>
###Your Task
--------------

Complete the implementation of the **```Histogram```** class.
You can re-use most of your implementation from [Lab 1](lab01.html).

The only difference between the **```Histogram```** class in Lab 3 (as compared to Lab 1) is that in this lab, the class will have a copy constructor and assignment operator.

The copy constructor should initialize the **```Histogram```** object so that it is an exact copy of the **```Histogram```** passed as the **```const```** reference parameter **```other```**.

The assignment operator should modify the **```Histogram```** object so that it becomes an exact copy of the **```Histogram```** passed as the **```const```** reference parameter **```rhs```**.

<br>
###Hints
--------------

Read the test code in **```main.cpp```**.  Make sure that you understand at which points in the program the copy constructor and assignment operator are used, and what is supposed to happen.

Each **```Histogram```** object will need to have its own private storage array.

You will need **two** fields in each **```Histogram```** object: the pointer to the array of **```int```** elements storing the counts, and an **```int```** field to keep track of how many buckets there are.  Fields (**```m_counts```** and **```m_numBuckets```**)
are specified in the class for this purpose.

The copy constructor should initialize the object so that its array is
the same size, and has the same contents, as **```other```**.

The assignment operator should de-allocate the current array and allocate a new array with the same size and contents as **```rhs```**.


<br>
###Testing
--------------

Run the test program.  If it prints

<pre>
End of main: everything is OK?
</pre>

Then all of the operations succeeded.

**Important**: Just because all of the operations succeeded doesn't mean that the class is completely correct.  If you forgot to delete the storage array for any of the **```Histogram```** objects, then programs using the class will have a memory leak.

One good way to check for memory leaks is to add code like the
following at points where dynamic memory (such as arrays) is allocated
or deallocated:

<pre>
std::cout << "Allocating array of " << m_numBuckets << " elements" << std::endl;
</pre>

<pre>
std::cout << "Deallocating array of " << m_numBuckets << " elements" << std::endl;
</pre>

Check that the allocation and deallocation messages indicate that all of the memory allocated was freed.