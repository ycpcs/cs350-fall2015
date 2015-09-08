---
layout: default
course_number: CS350
title: "Lab 4: Benchmarking vector and list"
---


###Getting Started
------------------

Download [CS350_Lab04.zip](CS350_Lab04.zip).  Unzip it.  
In a terminal window, change directory to the **```CS350_Lab04```** directory.

Using a text editor, open the file **```main.cpp```**.

To compile the test program, run the command **```make```**.

To run the program, run the command **```./lab04```**.



<br>
###Your Task
--------------

Your task is to measure the amount of time it takes to add *N*
elements to the *front* of a C++ **```std::vector```** and **```std::list```** collections, for varying values of *N*.

The goal is to see how the cost of adding *N* elements grows as
*N* increases.

C++ **```std::vector```** collections use an array as the underlying storage.  They are similar to **```ArrayList```**s in Java.

C++ **```std::list```** collections use a doubly-linked list as the underlying storage. They are similar to **```LinkedList```**s in Java.


<br>
###Step 1: Hypothesis
------------------

Write down a hypoothesis stating, for **```std::vector```**s and **```std::list```**s, how the running time of adding *N* elements to the front of the sequence will increase as *N* increases.  State a big-O upper bound on the growth of the running time for each kind of collection.

Also, write down the reason why you believe the running time will increase in they way you predicted.


<br>
###Step 2: Benchmarking
--------------------

Complete **```main.cpp```** so that it measures the amount of time required to add 10,000 to 100,000 elements to a **```std::vector```** and **```std::list```** by increments of 10,000.  The provided **```main.cpp```** includes a benchmarking loop and print statements to print output in the form


<pre>
<i>n</i>, <i>vectime</i>, <i>listtime</i>
</pre>


where *n* is the number of elements, *vectime* is the amount of time
needed to add *n* elements to the front of a **```std::vector```**, and *listtime* is the amount of time needed to add *n* elements to the front of a **```std::list```**.

Use the provided **```utime```** function to get a timestamp measured in microseconds.  Your code to collect a single timing data point should look something like this:

```cpp
uint64_t begin, end;

begin = utime();
  // 
  // the computation you want to time
  //
end = utime();

uint64_t elapsed = end - begin;
```

To add a single value to the front of a **```std::vector```**, use code like the following:

```cpp
std::vector<int> v;
  // ...
v.insert(v.begin(), 42);
```

To add a single element to the front of a **std::list**, use code like the
following:

```cpp
std::list<int> list;
  // ...
list.push_front(42);
```

These examples assume that the element type is **```int```**.


<br>
###Step 3: Analysis
----------------

Visualize your data by plotting it as a line chart in Excel,
with the **```std::vector```** and **```std::list```** data represented as separate series.

Does the data you collected support your hypothesis?
