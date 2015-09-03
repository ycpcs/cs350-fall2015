---
layout: default
course_number: CS350
title: "Lab 2: Postfix expression evaluator"
---


###Getting Started
------------------

Download [CS350_Lab02.zip](CS350_Lab02.zip).  Unzip it.  
In a terminal window, change directory to the **```CS350_Lab02```** directory.

Using a text editor, open the file **```main.cpp```**.

To compile the test program, run the command **```make```**.

To run the program, run the command **```./lab02```**.



<br>
###Postfix expressions
------------------

A postfix expression has operands and operators.  Unlike the infix
expressions you are used to, where the operator goes between the
operands, in a postfix expression the operator goes *after* the
operands.

Example:

<pre>
Infix: 2 + 3

Postfix: 2 3 +
</pre>


Postfix expressions are interesting because there is never any need to
use grouping (i.e., parentheses) to specify the order of operations:
the order of operations is always explicit.

Example:

<pre>
Infix: (2 + 3) * 4

Postfix: 2 3 + 4 *
</pre>

Another example:

<pre>
Infix: 2 + (3 * 4)

Postfix: 2 3 4 * +
</pre>


<br>
###Evaluating a postfix expression
-------------------------------

It is very easy to evaluate a postfix expression using a stack.

The operands and operators are processed in order.  Operands are pushed
onto the stack.  When an operator is seen, two operands are popped from
the stack, the operator is applied to the operands, and the result is
pushed onto the stack.

When evaluation is complete, the overall result of evaluating the
expression will be on the top of the stack.


<br>
###Your Task
--------------

Use the C++ **```std::stack```** stack class to implement a postfix expression
evaluator.  API documentation for this class is here:

>[http://www.cplusplus.com/reference/stack/stack/](http://www.cplusplus.com/reference/stack/stack/)


The file **```main.cpp```** already includes code to process tokens (operands and
operators).  When the token **```END```** is seen, the program assumes that the
expression is finished.

You will need to add the stack operations to carry out the evaluation algorithm
described above.

Use the following **```std::stack```** methods:

  - the **```push```** method pushes a value onto the stack
  - the **```pop```** method pops the top value from the stack, *but does not return it*
  - the **```top```** method returns the top value on the stack, *but does not pop it*

Example run (user input in **bold**):

<pre>
<b>2 3 + 4 *</b>
<b>END</b>
Result: 20
</pre>


Another example run (user input in **bold**):

<pre>
<b>2 3 4 * +</b>
<b>END</b>
Result: 14
</pre>