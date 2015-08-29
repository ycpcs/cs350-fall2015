---
layout: default
course_number: CS350
title: Intro to C++
---

For this course we will be implementing a variety of data structures using C++. Like Java, C++ is an object-oriented programming language. However since it is an extension of C, there are several key differences - most notably:

  - Rather than requiring separate overloaded constructors, often a single constructor with *default* parameters can be used.
  - Constructors typically use *initialization lists* (utilizing the parameters) for many of the class fields.
  - Classes have a *destructor* that is called when an object is destroyed, i.e. by going out of scope, and is responsible for any necessary cleanup particularly of any dynamically allocated memory.
  - Often the class declaration (i.e. interface) will be placed in a *header* file (usually with extension **```.h```**) with the method definitions being placed in an *implementation* file (usually with extension **```.cpp```**). The method definitions must be qualified with the class name in the implementation file.
  - The **```new```** operator is used for dynamic memory allocation, but returns an *address* which must be assigned to a *pointer* of appropriate type.
  - Any dynamically allocated memory must be freed using the **```delete```** operator to avoid memory leaks.



<br>
###Example
------------------

Below is an example Java implementation of a simple class that contains the various coin denominations with a few simple interface methods


```cpp
public class Coins {
    private int pennies;
    private int nickels;
    private int dimes;
    private int quarters;
    
    // Constructors
    public Coins() {
        pennies = 0;
        nickels = 0;
        dimes = 0;
        quarters = 0;
    }
    
    public Coins(int p, int n, int d, int q) {
        pennies = p;
        nickels = n;
        dimes = d;
        quarters = q;
    }
    
    // Interface methods
    public int findCentsValue() {
        int cents;    
        cents = 25*quarters + 10*dimes + 5*nickels + pennies;    
        return cents;
    }
        
    public int findDollars() {
        int cents = findCentsValue();
        return cents/100;
    }
        
    public int findChange() {
        int cents = findCentsValue();
        return cents%100;
    }
}
```



<br>    
The equivalent C++ class declaration (in **```Coins.h```**) would be

```cpp
class Coins {
private:
    int pennies;
    int nickels;
    int dimes;
    int quarters;
        
public:
    // Constructor
    Coins(int p=0, int n=0, int d=0, int q=0):
        pennies(p),nickels(n),dimes(d),quarters(q){};
        
    // Destructor
    ~Coins(){};
        
    // Interface methods
    int findCentsValue();
    int findDollars();
    int findChange();
};
```

    
<br>
Note that only a single constructor is necessary as the equivalent Java no parameter constructor simply uses all the default parameter values. Also rather than each member variable and method's visibility needing to be individually specified, C++ visibility can be specified in blocks.

The implementation for the methods (in **```Coins.cpp```**) would be


```cpp
#include "Coins.h"
    
int Coins::findCentsValue() {
    int cents;
    cents = 25*quarters + 10*dimes + 5*nickels + pennies;
    return cents;
}
    
int Coins::findDollars() {
    int cents = findCentsValue();
    return cents/100;
}
    
int Coins::findChange() {
    int cents = findCentsValue();
    return cents%100;
}
```

    
<br>    
Note that each method name is qualified with the name of the class (and no visibility specifiers are present in the definitions).

Instantiation an object of type ```Coins``` can be done in several ways:

```cpp
Coins c1;                       // Static allocation using all default constructor values
Coins c2(1,1,1,1);              // Static allocation using specified constructor values
Coins *c3 = new Coins();        // Dynamic allocation using all default constructor values
Coins *c4 = new Coins(1,1,1,1); // Dynamic allocation using specified constructor values
```


<br>
Note that **```c3```** and **```c4```** are *pointers* to **```Coins```** objects. To call the methods for the statically allocated objects, the *dot operator* is used. For calls to the methods in the dynamically allocated objects (since it is done *indirectly* through the pointer), the *arrow operator* is used. For example,


```cpp
int c1Cents = c1.findCentsValue();
int c2Dollars = c2.findDollars();
int c3Cents = c3->findCentsValue();
int c4Change = c4->findChange();
```


<br>
For the dynamically allocated objects, to prevent memory leaks there would need to be corresponding calls

```cpp
delete c3;
delete c4;
```

    
<br>
To dynamically allocate an array, the **```new```** operator is used with the desired array size as

```cpp
int *a = new int[size];
```

    
where the integer array **```a```** is dynamically allocated with **```size```** elements.

To free a dynamically allocated array, simply specify that the **```delete```** operator is freeing an array as

```cpp
delete[] a;
```

