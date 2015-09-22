---
layout: default
course_number: CS350
title: "Assignment 3: Skip List"
---


This lab will implement a skip list that stores arbitrary objects via class templates. The list will dynamically allocate nodes as necessary for space efficiency.

This lab will implement a skip list that stores arbitrary objects via class templates. The list will dynamically allocate nodes as necessary for space efficiency. Pseudocode for the skip list operations can be found in the original paper [Skip Lists: A Probabilistic Alternative to Balanced Trees](skiplists.pdf) in the June 1990 issue of *Communications of the ACM*.



<br>
###0. Getting Started
----------------------

If you don't already have one, create a directory on your **H:** drive named **CS350** (or anywhere else you choose). Navigate into this new directory and create a subdirectory named **assignments**.

Download [SkipList.zip](SkipList.zip), saving it into the **assignments** directory. 

Double-click on **SkipList.zip** and extract the contents of the archive into a subdirectory called **SkipList**.

For this lab, a static library has been provided (containing working versions of each method) to allow for testing of each class method independently. Any unimplemented methods in **SkipList.cpp** will use the corresponding method from the library, thus you can implement the methods in any order. Be sure to test each method you implement individually against the library for proper operation which can be accomplished by uncommenting the appropriate **```#define```** in the file **Flags.h** (and commenting the line containing **```#define ALL 1```**).  **DO NOT MODIFY ANY OF THE OTHER ```.h``` FILES INCLUDED WITH THE ASSIGNMENT**.
  

The class declaration is 

```cpp
template <class T>
class SkipList
{
public:
    SkipList();
    ~SkipList();

    // Public interface methods
    bool find(const T & x) const;
    void insert(const T & x);
    void remove(const T & x);
    bool isEmpty() const;
    void makeEmpty();
    void printList();

    // (Private) Head array
    Node<T> *head;
    static const int maxHeight = 5;

    // (Private) utility methods
    int randomLevel();
    double getRandomNumber();
};
```



<br>
### 1. Constructor / Destructor
--------------------------------

Since the list will grow dynamically as needed, the only purpose of the constructor is to create and initialize the sentinel (i.e. dummy) node.

**Tasks**

  * Add code to **```SkipList()```** (in **```SkipList.cpp```**) to *dynamically* allocate **```head```** as a  **```Node```**. The data field for the **```head```** node can be initialized with a value of **```T()```**.  The **```next```** array in a **```Node```** should be made **```maxHeight```** size (which can be done via the **```Node```** constructor). Do not forget to set the **```height```** field appropriately. For the **```head```** node, the **```height```** will represent the current highest level in the skiplist and **NOT** the maximum height of the skiplist (which is stored in the static member variable **```maxHeight```**).  When a  **```SkipList```** is first created, the highest level is 1.
  * Add code to **```~SkipList()```** (in **```SkipList.cpp```**) to free all **```Node```**s in the list and then deallocate **```head```**. 



<br>
###2. Find()
-----------------

The advantage of a skip list is the efficiency of the search operation of O(lg n). This is accomplished by using the multi-level **```next```** pointer array and the search process which has three cases starting at the highest level of the **```head```** pointer:

	
	1. If the value in the next node is *less than* the desired value, then move the current node to the next node and repeat the search process.

	2. If the value in the next node is *greater than* the desired value, then drop a level in the current node and repeat the search process.	

	3. If the value in the next node is the desired value then the value is found.


**Tasks**

* Add a method named **```find()```** that returns a **```bool```** (do not forget to qualify it with the class name) that takes a single **```const```** *reference* to a **```T```** object parameter and determines if the value is in the list. **Hint:** Use Pugh's pseudocode as a reference.



<br>
###3. Insert()
----------------

Since a skip list maintains elements in *sorted order*, to insert an element requires determining where the element belongs in the list, i.e. searching for the correct location. Hence the insert operation will also be O(lg n). To accomplish an insert, we must first determine what level the new node will be inserted at up to one level greater than the current max level of the skip list (usually by a repeated coin toss). Then a procedure similar to search is performed, but since the data structure is implemented with *singly-linked lists*, a local array of pointers is needed to ensure all links are updated correctly when the new node is inserted.

**Tasks**

  * Add a **```void```** method named **```insert()```** (do not forget to qualify it with the class name) that takes a parameter of type **```T```** and inserts a **```Node```** containing the data at the appropriate place in the list. You will need to use a local **```update```** array of pointers during the search that remembers the current node when the process drops levels. **Hint:** Use Pugh's pseudocode as a reference.
  * Add a method named **```randomLevel()```** (do not forget to qualify it with the class name) that takes no parameters and returns an **```int```** between 1 and the current highest level of the skip list plus 1 (and no greater than the **```maxHeight```** of the skip list). Use the provided **```getRandomNumber()```** function to generate (deterministic) random numbers and a threshold of 0.5 for continuing to increment the level. **Hint:** Use Pugh's pseudocode as a reference. 



<br>
###4. Remove()
----------------

This operation will remove a node from the list (if it exists) and again since it performs a similar search procedure will run in O(lg n). Like the insert process, a local array of pointers will need to be created to appropriately update the other nodes in the list when the desired node is deleted. Also if the node to be deleted is the only one currently at the highest level, then the highest level of the skip list should correspondingly be decremented.

**Tasks**

  * Add a **```void```** method named **```remove()```** (do not forget to qualify it with the class name) that takes a **```const```** *reference* to a **```T```** object parameter and removes the node that contains the given value (or does nothing if the value does not exist in the list). You will need to use a local **```update```** array of pointers during the search that remembers the current node when the process drops levels. **Hint:** Use Pugh's pseudocode as a reference.



<br>
###5. IsEmpty()
-----------------

A private method which simply returns a boolean indicating whether or not the current list contains any valid, i.e. non-head, nodes.
	
**Tasks**

  * Add a method named **```isEmpty()```** (do not forget to qualify it with the class name) that takes no parameters and returns a **```bool```** indicating *true* if the list contains no non-head nodes, i.e. when the list is empty.

  
  
<br>
###6. MakeEmpty()
------------------

This method should deallocate all the nodes in the list and reallocate a new head node.
	
**Tasks**

  * Add a **```void```** method named **```makeEmpty()```** (do not forget to qualify it with the class name) that takes no parameters. It should traverse the list removing each non-head node *individually* and then reallocate a new **```head```** node. **Hint:** Level 0 is a continuous linked list with all the nodes (and thus the entire pointer array for a particular node can be deallocated once the level 0 next pointer is placed in a temp pointer variable.)



<br>
### 7. Compiling and running the program
-----------------------------------------------

Once you have completed implementing any of the above methods (the remaining unimplemented ones will be drawn from the static library):

In a terminal window, navigate to the directory containing the source file
and run the command **```make```** to compile.

To run the test program run the command **```./SkipList```**.

Congratulations, you have just implemented a rather complex C++ data structure that uses templates!



<br>
###10. Grading Criteria
------------------------

**100 points**

* Constructor - **5 points**
* Destructor - **5 points**
* find() - **20 points**
* insert() - **25 points**
* randomLevel() - **5 points**
* remove() - **25 points**
* isEmpty() - **5 points**
* makeEmpty() - **10 points**



<br>
###10. Submitting to Marmoset
--------------------------------

**BE SURE TO REMOVE ALL DEBUG OUTPUT FROM YOUR METHODS PRIOR TO SUBMISSION!**  The only method that should produce output is **```printList()```** (and any library methods).

When you are done, run the following command from your terminal in the source directory for the project:

	make submit

You will be prompted for your Marmoset username and password,
which you should have received by email.  Note that your password will
not appear on the screen.

**DO NOT MANUALLY ZIP YOUR PROJECT AND SUBMIT IT TO MARMOSET.  
YOU MUST USE THE ```make submit``` COMMAND**.
