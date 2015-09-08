---
layout: default
course_number: CS350
title: "Assignment 1: Integer Array Stack"
---

In this assignment, you will implement a stack data structure that contains (non-negative) integer values. The stack will be backed by an array which will be dynamically resized for space efficiency. 

###Getting Started
------------------

If you don't alraedy have one, create a directory on your **H:** drive named **CS350** (or anywhere else you choose). Navigate into this new directory and create a subdirectory named **assignments**.

Download [ArrayStack.zip](ArrayStack.zip), saving it into the **assignments** directory. 

Double-click on **ArrayStack.zip** and extract the contents of the archive into a subdirectory called **ArrayStack**

For this lab, a static library has been provided (containing working versions of each method) to allow for testing of each class method independently. Any unimplemented methods in **IntArrayStack.cpp** will use the corresponding method from the library, thus you can implement the methods in any order. Be sure to test each method you implement individually against the library for proper operation which can be accomplished by uncommenting the appropriate **```#define```** in the file **Flags.h** (and commenting the line containing **```#define ALL 1```**).  **DO NOT MODIFY ANY OF THE OTHER ```.h``` FILES INCLUDED WITH THE ASSIGNMENT**.
 

The class declaration is 

```cpp
class IntArrayStack
{
private:
    // Class variables
    int *stack;
    int maxSize;
    int top;

    // (Private) utility methods
    void resize(int newSize);
    
public:
    IntArrayStack();
    ~IntArrayStack();
    
    // Public interface
    void push(int x);
    int pop();
    int peek();
    void emptyStack();
    bool isEmpty();

    void printStack();
    int getCapacity();
    int getSize();
    void toArray(int* arr);
};
```

<br>
### 1. Constructor / Destructor
--------------------------------

Since the backing array will be resizable for space efficiency, we will need to dynamically allocate it in the constructor and then deallocate it in the destructor.

**Tasks**

  - Add code to **```IntArrayStack()```** (in **IntArrayStack.cpp**) to dynamically allocate an initial array of size 1. Do not forget to also set the **```maxSize```** and **```top```** indicies appropriately.  Use a value of **```-1```** to initialize your value for **```top```**.
  - Add code to **```~IntArrayStack()```** to free the memory pointed to by **```stack```**. Note: **```stack```** is an *array* so deallocate it appropriately.



<br>
### 2. Push()
-------------

Inserting elements, (i.e. *pushing* them), onto a stack only occurs at the top of the stack (i.e. first in, last out). In order to have the stack use space efficiently, the backing array should grow dynamically as necessary. Use the following rule to determine when to expand the array:

  - If the stack is full, double the size of the backing array.

**Tasks**

  - Add a **```void```** method named **```push()```** (don't forget to qualify it with the class name) that takes a single **```int```** as an argument and pushes it onto the top of the stack. Hint: **```top```** keeps track of the *array index* of the most recently inserted element.  When the stack is empty, **```top = -1```**.  The first integer inserted into the stack should be stored at index 0 of the backing array.  Since you are dynamically allocating array space, don't forget to check if the current stack array is full, before attempting to add the newest element to the stack.  If the stack is full, double the size of the backing array via the **```resize()```** method.


<br>
### 3. Peek()
---------------
Peeking at the stack allows you to see what is on the top of the stack.  Peek should not modify the stack in anyway, but rather simply return the topmost value.

**Tasks**

  - Add a method named **```peek()```** (don't forget to qualify it with the class name) with no paramaters that returns the element at the top of the stack.  Hint: **```top```** keeps track of the *index* of the most recently inserted element.  Make sure the stack is not empty before attempting to retrieve the top element.  If the stack is empty, then return **```-1```**.



<br>
### 4. Pop()
--------------

Removing elements, (i.e. *popping* them, from a (non-empty) stack only occurs at the top of the stack. In order to have the stack use space efficiently, the backing array should shrink dynamically as necessary. Use the following rule to determine when to contract the array:

  - If the stack is less than one-third full, half the size of the backing array.

**Tasks**

  - Add a method named **```pop()```** (don't forget to qualify it with the class name) with no parameters that returns the element at the top of the stack.  Hint: **```top```** keeps track of the *index* of the most recently inserted element.  Make sure the stack is not empty before attempting to retrieve the top element.  If the stack is empty, then return **```-1```**.  Since you are dynamically allocating array space, do not forget to check if the *new* stack size is less than *one-third* of the backing array size, *halving* the size of the backing array if necessary via the **```resize()```** method.



<br>
### 5. EmptyStack()
----------------------

This operation should remove all the elements from the current stack.

**Tasks**

  - Add a **```void```** method named **```emptyStack()```** (don't forget to qualify it with the class name) that takes no parameters and clears the stack.  Hint: This can be done efficiently by simply resizing the existing array to 1 and resetting the **```top```** index.


<br>
### 6. IsEmpty()
------------------

This utility method is useful to check for an empty stack.

**Tasks**

 - Add a method named **```isEmpty()```** (don't forget to qualify it with the class name) that takes no parameters and returns a boolean value indicating whether or not the stack is empty. Hint: Consider the value of **```top```** that indicates an empty stack.


<br>
### 7. Resize()
----------------

In order to dynamically adjust the size of the backing array, we first note that following the rules discussed in sections 2 and 4 will never lose elements currently in the backing array. Thus the steps to resize the backing array are:

	1. Allocate a new temporary array of the appropriate new size.
	2. Copy all the valid elements from the old array to the new.
	3. Free the memory for the old array (while the pointer is still valid).
	4. Reassign the old array pointer to the new array address.
	
**Tasks**

  - Add a **```void```** method named **```resize()```** (don't forget to qualify it with the class name) that takes one parameter for the new backing array size that changes the size of **```stack```** to the new size with the same values as the original version.  Don't forget to update the **```maxSize```** variable.

  
<br>
### 8. Compiling and running the program
-------------------------------------------

Once you have completed implementing any of the above methods (the remaining unimplemented ones will be drawn from the static library):

To compile the test program, run the command **```make```**.

To run the test program run the command **```./IntArrayStack```**.

Congratulations, you have just written your first C++ data structure!


<br>
###9. Grading Criteria
------------------------

**50 points**

* Constructor - 5 points
* Destructor - 5 points
* push() - 10 points
* pop() - 10 points
* emptyStack() - 5 points
* isEmpty() - 5 points
* resize() - 10 points


<br>
###10. Submitting to Marmoset
--------------------------------

**BE SURE TO REMOVE ALL DEBUG OUTPUT FROM YOUR METHODS PRIOR TO SUBMISSION!**  The only method in your **IntArrayStack.cpp** file that should print output is **```printStack()```**.  Do **NOT** modify the units test that are printed from the **main.cpp** file.

When you are done, run the following command from your terminal in the source directory for the project:

	make submit

You will be prompted for your Marmoset username and password,
which you should have received by email.  Note that your password will
not appear on the screen.

**DO NOT MANUALLY ZIP YOUR PROJECT AND SUBMIT IT TO MARMOSET.  
YOU MUST USE THE ```make submit``` COMMAND**.
