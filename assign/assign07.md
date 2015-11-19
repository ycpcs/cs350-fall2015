---
layout: default
course_number: CS350
title: "Assignment 7: Binary Heap"
---

This lab will implement a binary heap that stores arbitrary objects via class templates. The heap will use a templated array backing structure whose size will be dynamically adjusted depending on the size of the heap being stored.


<br>
###0. Getting Started
----------------------

If you don't already have one, create a directory on your **H:** drive named **CS350** (or anywhere else you choose). Navigate into this new directory and create a subdirectory named **assignments**.

Download [BinHeap.zip](BinHeap.zip), saving it into the **assignments** directory. 

Double-click on **BinHeap.zip** and extract the contents of the archive into a subdirectory called **BinHeap**.

For this lab, a static library has been provided (containing working versions of each method) to allow for testing of each class method independently. Any unimplemented methods in **BinHeap.cpp** will use the corresponding method from the library, thus you can implement the methods in any order. Be sure to test each method you implement individually against the library for proper operation which can be accomplished by uncommenting the appropriate **```#define```** in the file **Flags.h** (and commenting the **```#define ALL 1```**).  **DO NOT MODIFY ANY OF THE OTHER ```.h``` FILES INCLUDED WITH THE ASSIGNMENT**.

The class declaration is 

```cpp
	template <class T>
	class BinHeap
	{
	public:
		// Constructor, destructor
		BinHeap();
		~BinHeap();
		
		// Public interface
		void buildHeap(const T* arr, int size);
		void insert(const T & x);
		T removeMin();
		void makeEmpty();
		void printHeap();

		// (Private) fields
		T* heapArray;
		int maxSize;
		int heapSize;
		
		// (Private) utility methods
		bool isEmpty();
		int leftIndex(int idx);
		int rightIndex(int idx);
		int parentIndex(int idx);
		void percolateUp(int idx);
		void percolateDown(int idx);
		void resizeArray(int newSize);
	};
```

For this implementation, **```maxSize```** is the maximum size *heap* that the backing array can accomodate (**not** including the sentinel value) while **```heapSize```** is the actual number of elements currently in the *heap*.



<br>
### 1. Constructor / Destructor
--------------------------------

Since the backing array will change dynamically as needed, the constructor should simply allocate an initial backing array for a heap that can hold 1 element and set the two size fields appropriately. Remember, no elements are stored in the 0th index of the backing array.  The destructor will then need to deallocate the backing array.

**Tasks**

  * Add code to **```BinHeap()```** (in **BinHeap.cpp**) to dynamically allocate a backing array of of **```T```** elements with sufficient size for a single element heap. Be sure to also initialize the two size fields in the class accordingly. Since the heap indicies start at 1, set a sentinel value of -1 into the 0 index element of the backing array. Hint: Consider how many elements you need to initially allocate to accomodate both the sentinel value and one heap element. 

  * Add code to **```~BinHeap()```** (in **BinHeap.cpp**) to deallocate the backing array (note this will remove all the elements automatically).



<br>
###2. IsEmpty()
-----------------

A private method which simply returns a boolean indicating whether or not the current *heap* contains any valid elements.
	
**Tasks**

* Add code to the **```isEmpty()```** method that takes no parameters and returns a **```bool```** indicating *true* if the heap contains no elements. Hint: This check should not be dependent on the size of the *backing array*.



<br>
###3. MakeEmpty()
------------------

This utility method should simply remove all the existing elements in the heap.

**Tasks**

  * Add code to the **```makeEmpty()```** to remove all the elements from the heap. Hint: This can easily be done by simply deallocating the existing backing array and then reallocating a new backing array similar to in the constructor.



<br>
###4. ResizeArray()
--------------------

This utility method will simply allocate a new backing array and copy the *valid* elements from the old heap into the new array.

**Tasks**

  * Add code to the **```resizeArray()```** method that takes an **```int```** parameter representing the new size for the largest heap the array can store. It should copy the *valid* elements from the current heap's backing array and then *deallocate* the old backing array. Hint: Remember that the 0th index of the backing array does not store a heap element.  Also, remember to update the new maximal heap size of the new backing array in the appropriate class field (note the size of the *heap* should not change).



<br>
###5. LeftIndex() / RightIndex() / ParentIndex()
------------------------------------------------

Since we are representing the heap using an array, the indices for the left/right/parent nodes for a given node are simple computations. Recall from class that:

```cpp
left(i) = 2*i
	
right(i) = 2*i + 1
	
parent(i) = i/2
```

**Tasks**

  * Add code to the **```leftIndex()```** method that takes an **```int```** parameter for the index of an input node and returns an **```int```** for the index of the left child of the input node. You may assume that the parameter represents a valid index value in the heap.

  * Add code to the **```rightIndex()```** method that takes an **```int```** parameter for the index of an input node and returns an **```int```** for the index of the right child of the input node. You may assume that the parameter represents a valid index value in the heap.

  * Add code to the **```parentIndex()```** method that takes an **```int```** parameter for the index an input node and returns an **```int```** for the index of the parent of the input node. You may assume that the parameter represents a valid index value in the heap.



<br>
###6. MinChild()
------------------------------------------------

For a min-heap represented with a backing array, it is convenient to have a utility method that can determine the index of a node's minimum child.  This method will be useful for **```percolateDown()```**.

**Tasks**

  * Add code to the **```minChild()```** method that takes an **```int```** parameter for the index of an input node and returns an **```int```** for the index of the minimum child of the input node. If the input node has no children, then return -1.  You may assume that the parameter represents a valid index value in the heap.



<br>
###7. Insert() / PercolateUp()
-------------------------------

To insert a new element into a heap, we can simply place it at the index of the backing array *after* the last valid heap element, and then *percolate up* this element.

**Tasks**

  * Add code to the **```percolateUp()```** method that takes an **```int```** parameter of the index of the current node. The method should continue to swap the element with its parent as long as the value of the parent is *greater than* the value of the node (or the node reaches the top of the heap). 

  * Add code to the **```insert()```** method that takes a **```const```** reference to a **```T```** object as a parameter. The method should place the element into the backing array and percolate it up the heap. Hint: If the backing array is full when trying to insert the new element, *double* the size of the backing array *before* inserting the new element (consider using the **```resizeArray()```** method). 



<br>
###8. RemoveMin() / PercolateDown()
------------------------------------

Removing elements from a heap is straightforward since the minimum element is *guaranteed* to be the *top* element of the heap. However, removing this element will then leave a hole in the heap that needs to be percolated down to the bottom. One way of doing this is to simply *swap* the top element with the *last* valid element in the heap (decrementing the size of the heap) and then percolating down this new top element which will restore the heap properties.

**Tasks**

  * Add code to the **```percolateDown()```** method that takes an **```int```** parameter of the index of the current node. The method should continually swap the node with the *smaller* of its two children until the node is either smaller than (or equal to) *both* children or at the *bottom* of the heap.  Hint: To determine which child node is smaller, consider using the **```minChild()```** method.

  * Add code to the **```removeMin()```** method that takes no parameters and returns a **```T```** object from the top of the heap (or -1 if there is no minimum element). The method should swap the top element (to be deleted) with the last *valid* element in the heap (if there is one) and then percolate the new top element down the heap. Hint: If the size of the *reduced* heap is less that a *third* of the backing array, the backing array should be cut in *half* (consider using the resizeArray() method).



<br>
###9. BuildHeap()
------------------

This public method will construct a heap from a given array of elements. Hence it will need to copy the elements from the parameter into the backing array (resizing the backing array appropriately) and then build a heap from the backing array.

**Tasks**

  * Add code to the **```buildHeap()```** method that takes a **```const T```** array parameter and the **```int```** size of this array (since C++ arrays do not have a size method). Size the backing array appropriately (consider using the **```resizeArray()```** method) such that the heap matches the size of the parameter array, copy the elements into the backing array, and then build the heap using the **```percolateDown()```** method. Hint: Remember that the elements in the array parameter begin at 0 while the valid heap elements in the backing array should begin at 1.



<br>
###10. Compiling and running the program
-----------------------------------------------

Once you have completed implementing any of the above methods (the remaining unimplemented ones will be drawn from the static library):

In a terminal window, navigate to the directory containing the source file and run the command **```make```** to compile.

To run the test program run the command **```./BinHeap```**.

Congratulations, you have just implemented an binary heap C++ data structure with templates!



<br>
###11. Grading Criteria
------------------------

**80 points**

* Constructor - 5 points
* Destructor - 5 points
* isEmpty() - 2 points
* makeEmpty() - 2 points
* resizeArray() - 5 points
* leftIndex() - 2 points
* rightIndex() - 2 points
* parentIndex() - 2 points
* minChild() - 5 points
* insert() - 10 points
* percolateUp() - 10 points
* removeMin() - 10 points
* percolateDown() - 10 points
* buildHeap() - 10 points



<br>
###12. Submitting to Marmoset
--------------------------------

**BE SURE TO REMOVE ALL DEBUG OUTPUT FROM YOUR METHODS PRIOR TO SUBMISSION!**  

The only method that should produce output is the provided **```printHeap()```**.

When you are done, run the following command from your terminal in the source directory for the project:

	make submit

You will be prompted for your Marmoset username and password,
which you should have received by email.  Note that your password will
not appear on the screen.

**DO NOT MANUALLY ZIP YOUR PROJECT AND SUBMIT IT TO MARMOSET.  
YOU MUST USE THE ```make submit``` COMMAND**.
