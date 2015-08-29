---
layout: default
course_number: CS350
title: "Lab 1: Histogram in C++"
---


###Getting Started
------------------

Download [CS350_Lab01.zip](CS350_Lab01.zip).  Unzip it.  In a terminal window, change directory to the **```CS350_Lab01```** directory.


Using a text editor, open the files **```Histogram.h```**, **```Histogram.cpp```**, and **```main.cpp```**.

To compile the test program, run the command **```make```**.

To run the test program run the command **```./lab01.exe```**.

<br>
###Your Task
------------

Complete the **```Histogram```** class.  The class declaration (in **```Histogram.h```**)
should look like this:

```cpp
class Histogram {
private:
    // *fields go here...*

    public:
    Histogram(int numBuckets);
    ~Histogram();

    void increaseCount(int bucket);
    int getCount(int bucket);
};
```

The idea is that a **```Histogram```** object counts the number of occurrences of events, where each event is identified by an integer *bucket*.  The **```numBuckets```** parameter to the constructor specifies how many buckets the **```Histogram```** object will have.  The first bucket has index 0, the second has index 1, etc.

You will need to add implementations for the constructor, destructor, **```increaseCount```**, and **```getCount```** methods in **```Histogram.cpp```**.

<br>
###Testing
-----------

The source file **```main.cpp```** has a test program that reads a line of text and uses a **```Histogram```** object to count the number of occurrences of the letters ```A-Z```, then prints a bar graph.

Example run (user input in **bold**):


<pre>
Enter text, followed by QUIT on its own line
<b>Oh freddled gruntbuggly</b>
<b>Thy micturations are to me</b>
<b>As plurdled gabbleblotchits on a lurgid bee.</b>
<b>Groop I implore thee, my foonting turlingdromes</b>
<b>And hooptiously drangle me with crinkly bindlewurdles,</b>
<b>Or I will rend thee in the gobberwarts with my blurglecruncheon,</b>
<b>See if I don't!</b>
<b>QUIT</b>
A: ========
B: =========
C: =====
D: =============
E: =========================
F: ===
G: ===========
H: ==========
I: ==================
J: 
K: =
L: ==================
M: =======
N: ===============
O: ==================
P: ====
Q: 
R: ===================
S: ========
T: =================
U: ==========
V: 
W: =====
X: 
Y: ======
Z: 
</pre>


<br>
###Hints and approach
----------------------

Use a dynamically-allocated array of **```int```** elements to store the counts
for each bucket.  Declare a field whose type is **```int```**\*  to store
the counts.  The constructor should create a dynamically allocated
array in the constructor and assign it to the **```int```**\* field.

**Important**: The array elements are not automatically initialized
to 0!  You will need to do this explicitly using a loop.

**Important**: don't forget to use the **```delete[]```** operator to
de-allocate the array!

Implement the program incrementally, as follows:

  - Start by making all of the methods no-ops: the **```getCount```** method should always return 0
  - Next, allocate the array in the constructor and delete it in the destructor; make sure the program still runs
  - Finally, implement the **```increaseCount```** and **```getCount```** methods so that they use the array

