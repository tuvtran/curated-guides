# Introduction to Pointer in C++

##### Source: 
* MOOC **_CS101.2x Object-Oriented Programming_** by IIT Bombay

##### Table of Contents:


## Memory and Addresses in C++:

* Main memory is a sequence of physical storage locations
* Each location stores 1 byte (equivalent to 8 bits)
* Each location is uniquely identified by an address (in hexadecimal)
* When a program runs, the computer allocates a part of the memory to it, dividing into three parts:
 * Stack: call stack
 * Heap: dynamically allocated data
 * Code segment: store executable instructions

## Program variables:

```c++
int main() {
	int a;		// there will be 4 bytes for a in stack segment
				// each addressable memory location stores 1 byte
	float b;
	char c;
}
```

For example, when we declare an int variable (**a** in this case), the program will allocate 4 bytes to store an int value in the memory, BUT only the address of the first byte in the memory would be used as the address of the variable. 

* a will have a location in the memory with the address range 0x00402-0x00405, but only 0x00402 is returned when we call
```c++
int *ptrA = &a;
```
(more on that later)

## Pointer as address of a variable:

How do we get an address of a variable?

```c++
int a;
int *p;
p = &a;
```

**&a** is the address of **a** in memory.

In general, if **x** is a variable of type **T**, T * is the type pointer to T and **&x** being an expression of type T * gives address of **x**

_If the memory location of **a** spans from 0x00402 to 00x00405, what is the address of **a**?_  
That would be 0x00402 because only the address of the first among bytes is the value of the pointer.

![memory](https://s31.postimg.org/4bh0zy27f/Screen_Shot_2016_07_20_at_12_12_29_AM.png)

According to the snapshot, the address of ptrA is 0x0040a and the content of ptrA, which is the address of **a**, is 0x00402

### Can we have pointers to pointers?

**ptrA** is a variable of type **int** (pointer to int), then **&ptrA** is the address of **ptrA**, which is of type _**pointer to** pointer int_

_Note: we don't write (int *) *, but int **_

We can take as far as we want. int **** means pointer to pointer to pointer to int

### A C++ program for printing addresses:

```c++
#include <iostream>

using namespace std;

int main() {
	int a;
	float b;
	char c;

	int *ptrA;
	float *ptrB;
	char *ptrC;

	ptrA = &a;
	ptrB = &b;
	ptrC = &c;

	cout << "Address of a is: " << ptrA;
	cout << "Address of b is: " << ptrB;
	cout << "Address of c is: " << ptrC;

	return 0;
}
```

## Access content at a given address:

C++ provides a _content of_ operator: *

If **ptrA** is a program variable of type _int *_, _*ptrA_ gives the integer stored at address given by **ptrA**

```c++
int main() {
	int a;
	int *ptrA;

	a = 696969;
	ptrA = &a;
	cout << *ptrA;		// Prints the content of the address given by the value of ptrA
}
```

Since ptrA is a pointer to int, its value is address of an _integer (4 bytes)_, so read 4 bytes starting from ptrA

### Can we have dereferences of dereferences?

```c++
int main() {
	char c;
	char *ptrC;
	char **ptrPtrC;
	ptrC = &c;
	ptrPtrC = &ptrC;

	cin >> c;
	cout << *(*ptrPtrC);	// dereferecne of dereference

	return 0;
}
```

### How far can we nest dereferences?

* No pre-specified limit
* If x is a variable of type _int ****_, we can use up to four levels of dereferencing of x


## Caveats when dereferencing:

* Certain memory addresses outside the part of memory allocated to process by system, and dereferencing such addresses leads to runtime error (segmentation violation)
* Memory location with address 0x0 is never within anyuser process' memory space
 * Dereferencing 0x0 will certainly cause program to crash