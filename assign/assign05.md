---
layout: default
course_number: CS350
title: "Assignment 5: Binary Search Tree"
---

This lab will implement a binary search tree that stores arbitrary objects via class templates. The tree will dynamically allocate nodes as necessary for space efficiency.


<br>
###0. Getting Started
----------------------

If you don't already have one, create a directory on your **H:** drive named **CS350** (or anywhere else you choose). Navigate into this new directory and create a subdirectory named **assignments**.

Download [BST.zip](BST.zip), saving it into the **assignments** directory. 

Double-click on **BST.zip** and extract the contents of the archive into a subdirectory called **BST**.

For this lab, a static library has been provided (containing working versions of each method) to allow for testing of each class method independently. Any unimplemented methods in **BST.cpp** will use the corresponding method from the library, thus you can implement the methods in any order. Be sure to test each method you implement individually against the library for proper operation which can be accomplished by uncommenting the appropriate **```#define```** in the file **Flags.h** (and commenting the line containing **```#define ALL 1```**).  **DO NOT MODIFY ANY OF THE OTHER ```.h``` FILES INCLUDED WITH THE ASSIGNMENT**.
 
The class declaration is 

```cpp
	template <class T>
	class BST
	{
	public:
		// Constructor, destructor
		BST();
		~BST();
    
		// Public interface
		bool isEmpty();
		bool find(const T & x);
		const T & findMin();
		const T & findMax();
		void insert(const T & x);
		void remove(const T & x);
		void makeEmpty();
		void printTree();
		    
		// Root node (Private)
		Node<T> *root;
		int numNodes;
	
		// Utility methods (Private)
		Node<T> * findNode(Node<T> * node, const T & x);
		Node<T> * findMinNode(Node<T> * node);
		Node<T> * findMaxNode(Node<T> * node);
		Node<T> * findSuccessor(Node<T> * node);
		Node<T> * findParentOf(const T & x);
		Node<T> * insertNode(Node<T> * node, const T & x);
		void removeAllNodes(Node<T> * node);
		void printNodesInOrder(Node<T> * node);
	};
```

**NOTE:** Use **NULL** for any null pointers.



<br>
### 1. Constructor / Destructor
--------------------------------

Since the tree will grow dynamically as needed, the constructor simply needs to initialize the root node. The destructor will subsequently need to remove any nodes in the tree.

**Tasks**

  * Add code to **```BST()```** (in **```BST.cpp```**) to initialize the **```root```** pointer and set the number of nodes appropriately.  Note: At this point we *do not* need to dynamically allocate a node since we have not actually inserted any values into the tree.
  
  * Add code to **```~BST()```** (in **```BST.cpp```**) to free all **```Node```**s in the tree.  Note: This operation will also free the root node (if it contains a value) thus we should not separately deallocate the root since it was not dynamically allocated in the constructor.



<br>
###2. IsEmpty()
-----------------

A public method which simply returns a boolean indicating whether or not the current tree contains any valid nodes.
	
**Tasks**

* Add a method named **```isEmpty()```** (do not forget to qualify it with the class name) that takes no parameters and returns a **```bool```** indicating *true* if the tree contains no nodes. Hint: Consider what **```root```** points to in the case of an empty tree.



<br>
###3. Find() / FindNode() [ *Recursive* ]
------------------------------------------

The find operation can be implemented in a recursive fashion according to the following rules starting at the root:


    1. If the node is **NULL** then terminate the search returning **false**, i.e. the value is not found.
    2. If the value in the node is equal to the desired value then terminate the search returning **true**, i.e. the value is found.
    3. If the search value is *less than* the value in the node, then recurse to the *left* child.
    4. If the search value is *greater than* the value in the node, then recurse to the *right* child.


**Tasks**

  * Add a method named **```find()```** that returns a **```bool```** (do not forget to qualify it with the class name) that takes a single **```const```** reference to a **```T```** object parameter and determines if the value is in the tree. Hint: Consider using the **```findNode()```** method passing the **```root```** as an argument.
  
  * Add a method named **```findNode()```** (do not forget to qualify it with the class name) that takes a *pointer* to a **```Node```** indicating the starting node for the search and a single **```const```** reference to a **```T```** object parameter and returns a *pointer* to a **```Nod```e** that contains the desired value. Pseudocode for this *recursive* routine is given as

```
FIND-NODE(n, k)
1  if n == NULL or k == n.key
2     return n
3  else if k < n.key
4     return FIND-NODE(n.left, k)
5  else 
6     return FIND-NODE(n.right, k)
```



<br>
###4. FindMin() / FindMinNode() [ *Recursive* ]
------------------------------------------------

Finding the minimum value in a binary search tree simply involves recursing down the left branches until a leaf node is reached (which will be the minimum by the binary search tree invariants).

**Tasks**

  * Add a method named **```findMin()```** that returns a **```const```** reference to a **```T```** object indicating the minimum value (do not forget to qualify it with the class name) and takes no parameters. If no minimum value exists (i.e. the tree is empty) you can terminate the program since **```findMin()```** should never be called on an empty tree. You can do this using a single [**```assert```** statement](http://www.cplusplus.com/reference/cassert/assert/).  Hint: Consider using the **```findMinNode()```** method passing the **```root```** as an argument
  
  * Add a method named **```findMinNode()```** (do not forget to qualify it with the class name) that takes a *pointer* to a **```Node```** as a parameter for the starting point and returns a *pointer* to a **```Node```** that contains the minimum value. Pseudocode for this *recursive* routine is given as

```
FIND-MINIMUM-NODE(n)
1  if n.left == NULL
2     return n
3  else 
4     return FIND-MINIMUM-NODE(n.left)
```


<br>
###5. FindMax() / FindMaxNode() [ *Recursive* ]
------------------------------------------------

Finding the maximum value in a binary search tree simply involves recursing down the right branches until a leaf node is reached (which will be the maximum by the binary search tree invariants).

**Tasks**

  * Add a method named **```findMax()```** that returns a **```const```** reference to a **```T```** object indicating the maximum value (do not forget to qualify it with the class name) and takes no parameters. If no maximum value exists (i.e. the tree is empty) you can terminate the program since **```findMax()```** should never be called on an empty tree. You can do this using a single [**```assert```** statement](http://www.cplusplus.com/reference/cassert/assert/).  Hint: Consider using the **```findMaxNode()```** method passing the **```root```** as an argument. 
  
  * Add a method named **```findMaxNode()```** (do not forget to qualify it with the class name) that takes a *pointer* to a **```Node```** as a parameter for the starting point and returns a *pointer* to a **```Node```** that contains the maximum value. Pseudocode for this *recursive* routine is given as

```
FIND-MAXIMUM-NODE(n)
1  if n.right == NULL
2     return n
3  else
4     return FIND-MAXIMUM-NODE(n.right)
```


<br>
###6. Insert() / InsertNode() [ *Recursive* ]
------------------------------------------------

To insert a value into a binary search tree (assuming it does not exist), which must occur at a leaf, a recursive procedure similar to search is performed until a **```NULL```** child is reached. Then the respective child pointers are assigned as the recursion unrolls.

**Tasks**

  * Add a **```void```** method named **```insert()```** (do not forget to qualify it with the class name) that takes a **```const```** reference to a **```T```** object parameter and inserts a **```Node```** containing the data at the appropriate place in the tree.  Hint: Consider using the **```insertNode()```** method passing the **```root```** as an argument. Since we are **not** using a sentinel root node, also consider how to handle the case where we are trying to insert into an empty tree. Finally be sure to increment the **```numNodes```** field if a new node is successfully inserted into the tree.

  * Add a method named **```insertNode()```** (do not forget to qualify it with the class name) that takes a *pointer* to a **```Node```** for the starting point and a **```const```** reference to a **```T```** object as parameters and returns a *pointer* to a **```Node```**. A Java implementation for this *recursive* routine can be found in the notes for [Binary Search Trees](../lectures/Binary_Search_Trees.pdf). Finally be sure to increment the **```numNodes```** field if a new node is successfully inserted into the tree.



<br>
###7. FindSuccessor() / FindParentOf() [ *Iterative* ]
-------------------------------------------------------

Two useful utility routines for **```remove()```** are a method to find the successor of a given node and a method to find the parent of a given node. 

**```FindSuccessor()```**

The *successor* node is the one with the *smallest* value in the node's *right* subtree. Hence, **```FindSuccessor()```** simply starts at the node's *right child* and continually follows the left branches until the minimum node is found.


**```FindParentOf()```**

Finding the parent node for a given value involves a similar process as searching except that the search terminates when one of the *child* nodes of the current node contains the desired value (or both children are **```NULL```** indicating the value is not in the tree).

**Tasks**

  * Add a method named **```findSuccessor()```** (do not forget to qualify it with the class name) that takes a *pointer* to a **```Node```** indicating the starting point and returns a *pointer* to a **```Node```** for the successor to that starting node. Write an *iterative* method that begins at the starting node's **```right```** child and follows the **```left```** children as far as possible down the tree.

  * Add a method named **```findParentOf()```** (do not forget to qualify it with the class name) that takes a a **```const```** reference to a **```T```** object as a parameter and returns a *pointer* to a **```Node```** which is the *parent* of the node containing the parameter value. Write the method such that it begins at the **```root```** and performs an *iterative* search until the *child* of the current node contains the desired value (at which point the current node is the *parent*).



<br>
###8. Remove()
---------------

The process for removal of a node from a binary search tree depends on one of four cases:


    1. Case 1: The value to remove is not in the tree - do nothing.
    2. Case 2: The node to remove has *no children*, i.e. is a leaf node - simply remove the node
    3. Case 3: The node to remove has *one child* - replace the node with its child
    4. Case 4: The node to remove has *two children* - replace the node with its *successor*


**Tasks**

  * Add a **```void```** method named **```remove()```** (do not forget to qualify it with the class name) that takes a **```const```** reference to a **```T```** object parameter and removes the node that contains the given value (or does nothing if the value does not exist in the list). Hints:

	* For *Case 1*: do nothing. 
	* For *Case 2*: the *child* pointer of the *parent* of the node containing the desired value needs to be updated, consider using the **```findParentOf()```** method. 
	* For *Case 3*: a strategy is to replace the *value* in the node to be deleted with the *value* in its (single) child, reassign the node's *pointers* to reference the *child*s subtrees, and then deallocate the *child* node.
	* For *Case 4*: first find the *successor* node (consider using the **```findSuccessor()```** method which must be valid since we know the node has two children in this case), copy the successor's data into the current node, and then *remove* the successor (which must have either zero children or one right child).
	* Be sure to decrement the **```numNodes```** field if a node is successfully removed from the tree. 


<br>
###9. MakeEmpty() / RemoveAllNodes() [ *Recursive* ]
-----------------------------------------------------

Deallocation of all the nodes in the tree can be done via a *post-order* traversal, i.e. once both child subtrees are deallocated we can deallocate the root.
	
**Tasks**

  * Add a **```void```** method named **```makeEmpty()```** (do not forget to qualify it with the class name) that takes no parameters and deallocates all nodes from the tree.  Hint: Consider using the **```removeAllNodes()```** method. Be sure to reset the tree fields appropriately.
  
  * Add a **```void```** method named **```removeAllNodes()```** (do not forget to qualify it with the class name) that takes a *pointer* to a **```Node```** representing the root of the subtree and implements a *recursive post-order* traversal of the tree that deallocates leaf nodes.  A Java implementation of post-order traversal can be found in the notes for [Tree Traversal](../lectures/Tree_Traversal_lecture.pdf).



<br>
###10. Compiling and running the program
-----------------------------------------------

Once you have completed implementing any of the above methods (the remaining unimplemented ones will be drawn from the static library):

In a terminal window, navigate to the directory containing the source file
and run the command **```make```** to compile.

To run the test program run the command **```./BST```**.

Congratulations, you have just implemented a binary search tree C++ data structure with templates!



<br>
###11. Grading Criteria
------------------------

**125 points**

* Constructor - **5 points**
* Destructor - **5 points**
* isEmpty() - **5 points**
* find() - **5 points**
* findNode() - **10 points**
* findMin() - **5 points**
* findMinNode() - **5 points**
* findMax() - **5 points**
* findMaxNode() - **5 points**
* insert() - **5 points**
* insertNode() - **15 points**
* findSuccessor() - **10 points**
* findParentOf() - **10 points**
* remove() - **20 points**
* makeEmpty() - **5 points**
* removeAllNodes() - **10 points**



<br>
###12. Submitting to Marmoset
--------------------------------

**BE SURE TO REMOVE ALL DEBUG OUTPUT FROM YOUR METHODS PRIOR TO SUBMISSION!**  

The only methods that should produce output are the provided **```printTree()```** and **```printNodesInOrder()```**.

When you are done, run the following command from your terminal in the source directory for the project:

	make submit

You will be prompted for your Marmoset username and password,
which you should have received by email.  Note that your password will
not appear on the screen.

**DO NOT MANUALLY ZIP YOUR PROJECT AND SUBMIT IT TO MARMOSET.  
YOU MUST USE THE ```make submit``` COMMAND**.
