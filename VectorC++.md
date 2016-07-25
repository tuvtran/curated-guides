# Introduction to Vectors in C++

<br>

![](http://i.imgur.com/iFBKVZI.png)
##### Source : My own experience and knowledge in college and after reading various online material.

<br>

##### Table of Contents:
1. [What the heck is Vectors in C++](#id-section1)
2. [Isnt vector a data type?](#id-section2)
3. [How do we use vectors ?](#id-section3)
4. [Advanced operations on vectors](#id-section4)
<div id='id-section1'/>

## What the heck is Vectors in C++ ? 

In any kinds of programming languages, we can store various data of the same type in an array. However, basic types of array never serve what we need sufficiently. It is either hard to operate since we are coding from the ground up or impossible to implement due to some restrictions (i.e: hardware, performance....). Smart people, therefore came up with dynamically allocated array ( read more [here](https://github.com/tuvttran/curated-guides/blob/master/PointerC%2B%2B.md#pointers-and-dynamic-memory) ) but still, find their codes not relevant enough sometimes and the process of utilizing dynamically allocated array prone to errors, hard to maintain....

Now, they want something **standardized**, **easy to use** yet **perform efficiently** ( little or no compromise here!).

And here comes the **Vector** .

To sum up, vectors are like dynamic array, but much much more powerful and easier to use.
<div id='id-section2'/>

## Wait, but I can't vector + name_of_variable! That's not a data type?

No. Vector is built in C++ Standard Template Library ( [C++ STL](https://en.wikipedia.org/wiki/Standard_Template_Library) ).

In short (because this guide is not about STL), C++ STL is a powerful library of templates which provides general-purpose templatized classes and functions that implement many popular and commonly used algorithms and data structures like vectors, lists, queues, and stacks (more on these later).

So yeah, now you understand that vector is **standardized** ! And it's much more than a data type.
<div id='id-section3'/>

## How do we use vectors ?

That's a good question! Vectors are meant to be **easy to use** so no worry here.

1. Firstly, you need to call the vector from the header file.

```c++
#include <vector>
```
and remeber the 
```c++
using namespace std
```
because vetor belongs to Standard Template Library.

2. Secondly, let's consider the syntax of initializing a vector.

```c++
vector<int> A;
```
Easily guess that one out , the code follows this rule : vector<data_type> variable_name.

Vector doesnt require you to add the size of the data (it's dynamic).But if you'd like to, -> **vector<data_type> variable_name (number_of_elements)**.

Oh, and if you'd like the intial vector to store something, you are welcome to add in -> **vector<data_type> variable_name  (number_of_elements , something_to_fulfill)**

For example :

```c++
vector<int> A (10, 5);
```

will make a vector of ten elements, each element's value is 5. Now let's consider another example :

```c++
vector<int> A (10,5);
vector<int> B (A);
```

**Now this one will copy the whole vector A to vector B.**

3. Thirdly, learn how to access vector elements.

We can refer to individual elements in the vector using square brackets to provide a subscript or index, as shown below:

```c++
vector<int> A (10,5);
A[3];
```

Please note that if you initially define the vector to have n elements , **you cannot access the n+1 elements** the program will crash!!!!

A safer way to access elements is by using **vector.at(index)**. If you go out of scope, this will throw an exception and exceptions are useful when you know how to handle them.

```c++
vector<int> A (10,5);
A.at(3);
```

Bigger example, show you how to traverse the vector with a loop:
```c++
#include <iostream>
#include <vector>

using namespace std;

int main(int argc, char** argv) {
	
	/*  Initialize vector of 10 copies of the integer 5 */
	vector<int> vectorOne(10,5);
	
	/*  run through the vector and display each element, if possible */
	for (long index=0; index<20; ++index) {
		try {
			cout << "Element " << index << ": " << vectorOne.at(index) << endl;
		}
		catch (exception& e) {
			cout << "Element " << index << ": index exceeds vector dimensions." << endl;
		}
		//see how exceptions are handled?
	}
	
	return 0; //exit
}
```
<div id='id-section4'/>

## Advanced operations on vectors

### Input 

To input data into vector, use **Vector.push_back(data)**. An empty vector will gradually put data in as you use this function more and more. In details, if i have n elements in the vector, whenever i call **vector.push_back(data)**, the vector will look if it has any empty space inside. If yes, put that data in. If no, make new space and put that data in ( it's dynamic !!! remember? ). 

```c++
vector<int> A(10); 
A.push_back(25); // will add 25 to A[0]
```

Big example incoming. This will demonstrate how **push_back** work in action. Try to guess the purpose of the codes below : 

```c++
#include <iostream>
#include <iomanip>
#include <vector>    // Needed to define vectors
using namespace std;

int main()
{
   vector<int> hours;      // hours is an empty vector
   vector<double> payRate; // payRate is an empty vector
   int numEmployees;       
   int index;              

   // Take employee number from users
   cout << "How many employees do you have? ";
   cin >> numEmployees;

   // Data input
   cout << "Enter the hours worked by " << numEmployees;
   cout << " employees and their hourly rates.\n";
   for (index = 0; index < numEmployees; index++)
   {
      int tempHours;    // To hold the number of hours entered
      double tempRate;  // To hold the payrate entered

      cout << "Hours worked by employee #" << (index + 1);
      cout << ": ";
      cin >> tempHours;
      hours.push_back(tempHours);      // Add data in vector hours.
      cout << "Hourly pay rate for employee #";
      cout << (index + 1) << ": ";
      cin >> tempRate;
      payRate.push_back(tempRate);  // Add data in vector payRate
   }

   //In ra giá trị 
   cout << "Here is the gross pay for each employee:\n";
   cout << fixed << showpoint << setprecision(2);
   for (index = 0; index < numEmployees; index++)
   {
      double grossPay = hours[index] * payRate[index];
      cout << "Employee #" << (index + 1);
      cout << ": $" << grossPay << endl;
   }
   return 0;
}  
```

This is the output 

```
How many employees do you have? 3 [enter]
Enter the hours worked by 3 employees and their hourly rates.
Hours worked by employee #1: 40 [enter]
Hourly pay rate for employee #1: 12.63 [enter]
Hours worked by employee #2: 25 [enter]
Hourly pay rate for employee #2: 10.35 [enter]
Hours worked by employee #3: 45 [enter]
Hourly pay rate for employee #3: 22.65 [enter]

Here is the gross pay for each employee:
Employee #1: $505.20
Employee #2: $258.75
Employee #3: $1019.25
Press any key to continue . . .
```

### Size

The size of a vector is accessed via **vector.size()**. 

```c++
vector<int> A(10);
int numberOfValues = A.size(); //will equal to 10
```

Now let's take a closer look at this example 

```c++
#include <iostream>
#include <vector>
using namespace std;

// Function prototype
void showValues(vector<int>);

int main()
{
   vector<int> values;

   // Add 7 values in vector
   for (int count = 0; count < 7; count++)
      values.push_back(count * 2);
   
   //Print all the values
   showValues(values);
   return 0;
}

//**************************************************
// Pay attention to the showValue function            
// It will print to console every value of the whole vector                   
//**************************************************

void showValues(vector<int> vect)
{
   for (int count = 0; count < vect.size(); count++)
      cout << vect[count] << endl;
}  
```

The function shows you how vector **size** can be put in action. It is very useful when you want to make a loop that traverse the whole vector or control the elements of a vector (just like an array).

### Output

To get an element out of the vector, we use **vector.pop_back()**. Please note that when we use **pop_back()**, the last value of the vector will get popped out. Therefore, the vector now no longer stores that data and it is important to remember this as we should avoid direct access to this element afterwards.

Let's consider this example so that you can understand how **vector.pop_back()** is used 

```c++
using namespace std;

int main()
{
   vector<int> values;

   //Insert/Input values
   values.push_back(1);
   values.push_back(2);
   values.push_back(3);
   cout << "The size of values is " << values.size() << endl;
   
   //Here come the pop_back().
   cout << "Popping a value from the vector...\n";
   values.pop_back();
   cout << "The size of values is now " << values.size() << endl;
   
   // Continue to pop_back().
   cout << "Popping a value from the vector...\n";
   values.pop_back();
   cout << "The size of values is now " << values.size() << endl;
   
   //Continue to pop_back().
   cout << "Popping a value from the vector...\n";
   values.pop_back();
   cout << "The size of values is now " << values.size() << endl;
   return 0;
}  
```

Here's the output to the above code: 

```
The size of values is 3
Popping a value from the vector...
The size of values is now 2
Popping a value from the vector...
The size of values is now 1
Popping a value from the vector...
The size of values is now 0
Press any key to continue . . .
```

### Erase

The **Vector.erase(index)** will erase the desired element. For example:

```c++
vector<int> A(10,5);
A.erase(3); // will delete element at index 3 ( the fourth element in the vector)
```

Or you can delete many elements at once by using **vector.erase(begin, end)**. For example:

```c++
vector<int> A(10,5);
A.erase(3,5); //will delete element at index 3, 4, 5 ( the fourth, fifth, sixth one)
```

### Clear 

If you want to completely delete the whole vector, or return it to its original state where no value is assigned in each element, usually your brain comes up with **pop_back()** or a loop deleting each value ( assume you desire to apply what you have learnt above ! ) . However, that costs time and generate unnecessary lines of codes where we can just use **vector.clear()** to clear all the vector data.

```c++
#include <iostream>
#include <vector>
using namespace std;

int main()
{
   vector<int> values(100);

   cout << "The values vector has "
        << values.size() << " elements.\n";
   cout << "I will call the clear member function...\n";
   values.clear();
   cout << "Now, the values vector has "
        << values.size() << " elements.\n";
   return 0;
}  
```

and guess what the output is ...

```
The values vector has 100 elements.
I will call the clear member function...
Now, the values vector has 0 elements.
Press any key to continue . . .
```

Simple as eating a cake, isn't it?
Plus, if you want to check whether the current vector is empty or not, you can call out **vector.empty()**. Here's a quick look at how to implement it : 

```c++
if(A.empty() == true){
       cout << "No values in A \n";
} 
```
### Resize

This **vector.resize(size , new_default_element)** helps you resize a vector (and set new default . Consider the below example to see how resizing is put into action :

```c++
#include <iostream>
#include <vector>

using namespace std;

int main(int argc, char** argv) {
	
	/*  Initialize vector of 10 copies of the integer 5 */
	vector<int> vectorOne(10,5);
	
	/*  Display size of vector */
	cout << "Size of vector is " << vectorOne.size() << " elements." << endl;
	
	/*  run through the vector and display each element, using size() to determine index boundary */
	for (long index=0; index<(long)vectorOne.size(); ++index) {
		cout << "Element " << index << ": " << vectorOne.at(index) << endl;
	}
	
	/*  Change size of vector - element removal */
	vectorOne.resize(7);
	
	/*  Display size of vector */
	cout << "Size of vector is " << vectorOne.size() << " elements." << endl;
	
	/*  run through the vector and display each element, using size() to determine index boundary */
	for (long index=0; index<(long)vectorOne.size(); ++index) {
		cout << "Element " << index << ": " << vectorOne.at(index) << endl;
	}
	
	return 0;
}
```

This is the output :

```
Size of vector is 10 elements.
Element 0: 5
Element 1: 5
Element 2: 5                        
Element 3: 5
Element 4: 5
Element 5: 5
Element 6: 5
Element 7: 5
Element 8: 5
Element 9: 5
Size of vector is 7 elements.
Element 0: 5
Element 1: 5
Element 2: 5
Element 3: 5
Element 4: 5
Element 5: 5
Element 6: 5   
```

### Other good stuffs

* **Vector.begin()** returns an iterator to the start of the vector.
* **Vector.end()** returns an iterator to the end of the vector.
* **Vector.at(index)** returns the element at some index in the vector.
* **Vector.swap(vector2)** swap the content of two vectors.
* **Vector.front()** accesses the first element.
* **Vector.back()** accesses the last element.
* **Vector.capacity()** returns the number of elements that the vector can hold before more space is allocated. It is very important to remember here that, unlike raw arrays, most of the memory management for vectors is performed silently during construction or manipulation of the container. **The **reserve()** method only allocates memory, but leaves it uninitialized. It only affects **capacity()**, but **size()** will be unchanged.** 
Consider this : 

```c++
#include <iostream>
#include <vector>
 
using namespace std;
 
int main()
{
    vector< int > my_vect;
    my_vect.reserve( 10 );
    my_vect.push_back( 1 );
    my_vect.push_back( 2 );
    my_vect.push_back( 3 );
 
    cout << my_vect.capacity() << "\n";
    cout << my_vect.size() << "\n";
 
    return 0;
}
```

The output here is : 
```
10
3
```

Now you see the difference between capacity and size although they seem to be similar to each other.
![](https://www.securecoding.cert.org/confluence/download/attachments/20087026/vector-clipped.jpg?version=1&modificationDate=1239994411000&api=v2)

<br>

## END OF GUIDES. HAVE FUN WITH VECTORS! 


##### Reference for examples 
[1](http://www.dreamincode.net/forums/topic/33631-c-vector-tutorial/),[2](http://diendan.congdongcviet.com/threads/t5227::cach-su-dung-vector-danh-cho-newbie-trong-lap-trinh-cpp.cpp),[3](http://www.tutorialspoint.com/cplusplus/cpp_stl_tutorial.htm)

