---
layout: default
course_number: CS350
title: "Assignment 6: AA Tree"
---

This lab will implement a AA tree that stores arbitrary objects via class templates. The tree will dynamically allocate nodes as necessary for space efficiency. Pseudocode for the AA tree operations can be found in the original paper [Balanced Search Trees Made Simple" by Arne Andersson](http://user.it.uu.se/~arnea/ps/simp.pdf) from the Proc. Workshop on Algorithms and Data Structures, Springer Verlag, 1993.


<br>
###0. Getting Started
----------------------

If you don't already have one, create a directory on your **H:** drive named **CS350** (or anywhere else you choose). Navigate into this new directory and create a subdirectory named **assignments**.

Download [AATree.zip](AATree.zip), saving it into the **assignments** directory. 

Double-click on **AATree.zip** and extract the contents of the archive into a subdirectory called **AATree**.

For this lab, a static library has been provided (containing working versions of each method) to allow for testing of each class method independently. Any unimplemented methods in **AATree.cpp** will use the corresponding method from the library, thus you can implement the methods in any order. Be sure to test each method you implement individually against the library for proper operation which can be accomplished by uncommenting the appropriate **```#define```** in the file **Flags.h** (and commenting the **```#define ALL 1```**).  **DO NOT MODIFY ANY OF THE OTHER ```.h``` FILES INCLUDED WITH THE ASSIGNMENT**.
 
The class declaration is 

```cpp
	template <class T>
	class AATree
	{
	public:
    	// Constructor, destructor
		AATree();
		~AATree();
		
		// Public interface
		bool find(const T & x);
		const T & findMin();
		const T & findMax();
		void insert(const T & x);
		void remove(const T & x);
		bool isEmpty() const;
		void makeEmpty();
		void printTree();
        
	
		// Node fields (Private)
		Node<T> *root;          // root node
		Node<T> *bottomNode;    // sentinel node
		Node<T> *lastNode;      // used for delete
		Node<T> *deletedNode;   // used for delete

		// Utility methods (Private)
		Node<T> * findNode(const T & x, Node<T> * node);
		Node<T> * findMinNode(Node<T> * node);
		Node<T> * findMaxNode(Node<T> * node);
		Node<T> * insertNode(Node<T> * & node, const T & x);
		void removeNode(Node<T> * & node, const T & x);
		void skew(Node<T> * & node);
		void split(Node<T> * & node);
		void removeAllNodes(Node<T> * node);
		void printNodesInOrder(Node<T> * node);
	};
```



<br>
### 1. Constructor / Destructor
--------------------------------

Since the tree will grow dynamically as needed, the constructor simply needs to initialize the root node. The destructor will subsequently need to remove any nodes in the list.

**Tasks**

  * Add code to **```AATree()```** (in **```AATree.cpp```**) to initialize the **```bottomNode```** pointer (and ALL of its fields appropriately). Also assign the **```root```**, **```lastNode```**, and **```deletedNode```** pointers to the **```bottomNode```** (since there are no nodes in the tree yet). Set the level of the **```bottomNode```** to 0 since it will sit BELOW all of the level 1 nodes.  Set the left and right children of **```bottomNode```** to point to **```bottomNode```**.

  * Add code to **```~AATree()```** (in **```AATree.cpp```**) to free all **```Node```**s in the tree. Note: This operation should also deallocate the **```bottomNode```** that was allocated in the constructor.



<br>
###2. IsEmpty()
-----------------

A private method which simply returns a boolean indicating whether or not the current tree contains any valid nodes.
	
**Tasks**

  * Add a method named **```isEmpty()```** (do not forget to qualify it with the class name) that takes no parameters and returns a **```bool```** indicating *true* if the tree contains no nodes. Hint: Consider what **```root```** points to in the case of an empty tree.



<br>
###3. Find() / FindNode() [*Iterative*]
------------------------------------------

The find operation can be implemented in an iterative fashion identically to a generic binary search tree. Thus as long as the bottom of the tree or the desired value is not found continue traversing down the tree. If the desired value is less than the current node's value then follow the left child, otherwise follow the right child.

**Tasks**

  * Add a method named **```find()```** that returns a **```bool```** (do not forget to qualify it with the class name) that takes a single **```const```** reference to a **```T```** object parameter and determines if the value is in the tree. Hint: Consider using the **```findNode()```** method passing the **```root```** as an argument. Also, consider the return value of **```findNode()```** if the desired value is not found in the tree.

  * Add a method named **```findNode()```** (do not forget to qualify it with the class name) that takes a *pointer* to a **```Node```** indicating the starting node for the search and a single **```const```** reference to a **```T```** object parameter and returns a *pointer* to a **```Node```** that contains the desired value. Pseudocode for this *iterative* routine is given as

```
FIND-NODE-ITERATIVE(n, k)
1  while n != bottomNode and n.key != k
2     if k < n.key
3        n = n.left
4     else 
5        n = n.right
6  return n
```



<br>
###4. FindMin() / FindMinNode() [*Iterative*]
------------------------------------------------

Finding the minimum value in an AA tree is identical to the operation for a generic binary search tree.  That is, continually follow left branches until a leaf node is reached. That leaf node will contain the minimum value by the binary search tree invariants.

**Tasks**

  * Add a method named **```findMin()```** that returns a **```const```** reference to a **```T```** object indicating the minimum value or simply returning -1 if there is none (do not forget to qualify it with the class name) and takes no parameters. Hint: Consider using the **```findMinNode()```** method passing the **```root```** as an argument. 

  * Add a method named **```findMinNode()```** (do not forget to qualify it with the class name) that takes a *pointer* to a **```Node```** as a parameter for the starting point and returns a *pointer* to a **```Node```** that contains the minimum value. Pseudocode for this *iterative* routine is given below.  Note that you will want to modify/add to the pseudocode as necsesary to deal with the case when the tree is empty.

```
FIND-MINIMUM-NODE-ITERATIVE(n)
1  while n.left != bottomNode
2     n = n.left
3  return n
```



<br>
###5. FindMax() / FindMaxNode() [*Iterative*]
------------------------------------------------

Finding the maximum value in an AA tree is also identical to the operation for a generic binary search tree. That is, continually follow right branches until a leaf node is reached. That leaf node will contain the maximum value by the binary search tree invariants.

**Tasks**

  * Add a method named **```findMax()```** that returns a **```const```** reference to a **```T```** object indicating the maximum value or simply returning -1 if there is none (do not forget to qualify it with the class name) and takes no parameters. Hint: Consider using the **```findMaxNode()```** method passing the **```root```** as an argument. 

  * Add a method named **```findMaxNode()```** (do not forget to qualify it with the class name) that takes a *pointer* to a **```Node```** as a parameter for the starting point and returns a *pointer* to a **```Node```** that contains the maximum value. Pseudocode for this *iterative* routine is given below. Note that you will want to modify/add to the pseudocode as necsesary to deal with the case when the tree is empty.

```
FIND-MAXIMUM-NODE-ITERATIVE(n)
1  while n.right != bottomNode
2     n = n.right
3  return n
```



<br>
###6. Insert() / InsertNode() [*Recursive*]
------------------------------------------------

To insert a value into an AA tree recursively, like a generic BST the node is initially inserted at the proper leaf location. To rebalance the tree as the recursion is unrolled, a **```skew()```** and **```split()```** operation is called at each parent node.  Do not allow insertion of duplicate values -- simply ignore duplicates.

**Tasks**

  * Add a **```void```** method named **```insert()```** (do not forget to qualify it with the class name) that takes a **```const```** reference to a **```T```** object parameter and inserts a **```Node```** containing the data at the appropriate place in the tree. Hint: Consider using the **```insertNode()```** method passing the **```root```** as an argument.

  * Add a method named **```insertNode()```** (do not forget to qualify it with the class name) that takes a *reference to a pointer* to a **```Node```** for the starting point and a **```const```** reference to a **```T```** object as parameters and returns a *pointer* to the newly inserted **```Node```**. Use the pseudocode from Andersson's paper as a reference for this routine.  You can ignore the **```ok```** variable in Andersson's pseudocode.  If the value to be inserted already exists in the tree, simply return the **```Node```** that contains that value (this is slightly different than the pseudocode in Andersson's paper).



<br>
###7. Remove() / RemoveNode() [*Recursive*]
----------------------------------------------

The process for removal of a node from an AA tree initially requires a recursive search that keeps track of other nodes necessary for the deletion process. Once the desired node is found, as the recursion is unrolled a sequence of three **```skew()```** and two **```split()```** operations are called at each parent to rebalance the tree.

**Tasks**

  * Add a **```void```** method named **```remove()```** (do not forget to qualify it with the class name) that takes a **```const```** reference to a **```T```** object parameter and removes a **```Node```** containing the data. Hint: Consider using the **```removeNode()```** method passing the **```root```** as an argument.

  * Add a **```void```** method named **```removeNode()```** (do not forget to qualify it with the class name) that takes a *reference to a pointer* to a **```Node```** for the starting point and a **```const```** reference to a **```T```** object as parameters. Use the pseudocode from Andersson's paper as a reference for this routine.  Once again, you can ignore the **```ok```** variable in Andersson's pseudocode.



<br>
###8. Skew() / Split()
----------------------------------------------

Two key operations to the maintenance of an AA tree are **```skew()```** and **```split()```** which perform various rotations and level updates.

**Tasks**

  * Add a **```void```** method named **```skew()```** (do not forget to qualify it with the class name) that takes a *reference to a pointer* to a **```Node```** and performs the skew operation. Use the pseudocode from Andersson's paper as a reference for this routine.

  * Add a **```void```** method named **```split()```** (do not forget to qualify it with the class name) that takes a *reference to a pointer* to a **```Node```** and performs the split operation. Use the pseudocode from Andersson's paper as a reference for this routine.



<br>
###9. MakeEmpty() / RemoveAllNodes() [*Recursive*]
-----------------------------------------------------

Deallocation of all the nodes in the tree can be done via a *post-order* traversal. After both child subtrees are deallocated, deallocate the current node.
	
**Tasks**

  * Add a **```void```** method named **```makeEmpty()```** (do not forget to qualify it with the class name) that takes no parameters and deallocates all nodes from the tree. Hint: Consider using the **```removeAllNodes()```** method. Also, consider where the **```root```** should point once all the tree nodes are removed.

  * Add a **```void```** method named **```removeAllNodes()```** (do not forget to qualify it with the class name) that takes a *pointer* to a **```Node```** representing the root of the subtree and implements a *recursive post-order* traversal of the tree that deallocates leaf nodes. A Java implementation of post-order traversal can be found in the notes for [Tree Traversal](../lectures/Tree_Traversal_lecture.pdf).



<br>
###10. Compiling and running the program
-----------------------------------------------

Once you have completed implementing any of the above methods (the remaining unimplemented ones will be drawn from the static library):

In a terminal window, navigate to the directory containing the source file and run the command **```make```** to compile.

To run the test program run the command **```./AATree```**.

Congratulations, you have just implemented an AA tree C++ data structure with templates!



<br>
###11. Grading Criteria
------------------------

**125 points**

* Constructor - **5 points**
* Destructor - **5 points**
* isEmpty() - **5 points**
* find() - **5 points**
* findNode() - **5 points**
* findMin() - **5 points**
* findMinNode() - **5 points**
* findMax() - **5 points**
* findMaxNode() - **5 points**
* insert() - **5 points**
* insertNode() - **15 points**
* remove() - **5 points**
* removeNode() - **20 points**
* skew() - **10 points**
* split() - **10 points**
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
