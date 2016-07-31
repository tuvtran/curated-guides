# Object-Oriented Programming in C++

Note: This guide **does not** intend to include everything about OOP programming, but only some special features of C++ in OOP.

## Defining Classes:

```c++
class Student {
	private:
		int age;
		string name;

	public:
		string getName() {
			return name;
		}
}
```

Like in other OOP languages, classes provide a blueprint for instantiating objects to interact with one another. In a class, there are three types of access control modifiers:
* public
* private
* protected

In C++, a ```struct``` by default makes all of its members public, while a ```class``` makes all of its members private, so we have to specify which is private and which is public.

### Constructor and Destructor:

Constructor and destructor provide us the customizability to dictate what happens when an object is allocated or de-allocated.

A constructor is invoked **automatically** when an object of the class is allocated
* Convenient way to initialize data members
* Just like any other member function
 * Accepts optional input parameters
 * Can be used to perform tasks other than initialization too

A destructor is invoked **automatically** when an object of the class is de-allocated
* Convenient way to do book-keeping/cleaning-up before de-allocating object
* Accepts no parameters
* Can be used to perform other tasks before de-allocating

```c++
class Vector {
	private:
		double x, y, z;
	public:
		Vector (double vx, double vy, double vz): x(vx), y(vy), z(vz) {		// This is initialization list for constructor
			return;
		}

		// This is the default constructor
		Vector () {
			x = 0;
			y = 0;
			z = 0;
			return;
		}
	
		~Vector () {
			cout << "Destroying object..." << endl;
		}

		double getX() {
			return x;
		}

		double getY() {
			return y;
		}

		double getZ() {
			return z;
		}

		// An interesting implementation of sum operation function
		Vector sum(const Vector &v) {
			return Vector(x + v.x, y + v.y, z + v.z);
		}
}
```

Note:

* Constructor and destructor need to be public
* If we only declare the object, the default constructor will be invoked
* Multiple destructors of the same class are not allowed in C++
* A default constructor will do nothing in its body
* If we provide a non-default constructor, but not a default one, C++ compiler will NOT provide a bare-bone default constructor => error. **BEST PRACTICE: DEFINE DEFAULT CONSTRUCTORS**

C++ also allows default parameters for constructors

```c++
Vector (double vx = 0, double vy = 0, double vz = 0) {
	x = vx;
	y = vy;
	z = vz;
	return;
}
```

### Copy constructor:

Suppose a new object is created by making a copy of another object of the same class

```c++
Vector v1(1.0, 2.0, 3.0);
Vector v2 = v1;
```

Regular assignment statements **do not need** copy since they do not create a new object.

```c++
Vector v;
v = a.scale(2.0);
```

A copy constructor must be specified separately from an ordinary constructor

```c++
Vector (const Vector& src) {
	x = src.x;
	y = src.y;
	z = src.z;
}
```

If you have not defined a copy constructor, the C++ compiler will create a default copy constructor

Another copy constructor:

```c++
class myString {
	public:
		char *cArray;
		int length;

		// default constructor
		myString (const char initString[]) { ... } 

		~myString () {
			delete[] cArray;
			return;
		}

		// copy constructor
		myString (const myString &source): length(source.length) {
			if (cArray == NULL) {
				// handle error
			} else {
				for (int i = 0; i <= length; i++) {
					cArray[i] = source.cArray[i];
				}
			}

			return;
		}
};
```

## Operator Overloading:

C++ provides us with a way to override the default operators such as +, -, /, * when interacting with objects.

Suppose @ is an infix operator (e.g. +, -, /, *, %,...), in C++, the expression X@Y is equivalent to X.operator@(Y)

Defining custom ooperators for ```Vector```

```c++
class Vector {
	private:
		double x, y, z;
	public:
		/*
			Constructor
			Destructor
			Copy constructor
		*/
		Vector operator+(const Vector &b) const {		// use const because function cannot change the receiver object
			return Vector(x + b.x, y + b.y, z + b.z);
		}	

		Vector operator*(const double factor) const { 
			return Vector(x*factor, y*factor, z*factor);
		}

		// copy assignment operator
		Vector& operator=(const Vector &rhs) {
			x = rhs.x;
			y = rhs.y;
			z = rhs.z;

			// "this" denotes a pointer to the receiver object
			return *this;
		}
};

// Non-member operator function
Vector operator*(const double factor, const Vector &b) {
	// reference to the member function
	return (b * factor);
}
```

## Friend functions and classes:

Normally, private members of a class are accessible only to member functions of a class. However, sometimes we may want to bypass this by allowing access to functions or classes lying beyond the scope of the current function.

```c++
class Student {
	private:
		int grades[10];
	public:
		int age;
		string name;

		/*
			constructor
			destructor
			...
		*/	
		
		// this friend function could be in public or private section of class Student
		friend double getSumGrades(Student &s1);

		// we need friend classes when various members of a class may need access to private members of the current class
		friend class StudentGrader;
};

double getSumGrades(Student &s1) {
	// function content
}

class Student Grader {
	// content here	
};
```

## Static Data Members:

Note: instantiating static members right after class definition

```c++
class Point {
	private:
		double x, y;
	public:
		static int count;

		Point() {
			count++;
			return;
		}	

		Point(double a, double b): x(a), y(b) {
			count++;
			return;
		}
};

int Point::count = 0;			// scope resolution operator
```

## Inheritance:

```c++
class A : public B {

}
```

### Access Control in Inheritance:

| Base Class Member | public       | protected    | private      |
|-------------------|--------------|--------------|--------------|
| private           | inaccessible | inaccessible | inaccessible |
| protected         | protected    | protected    | private      |
| public            | public       | protected    | private      |

### Access mthods of base class using derived class:

```c++
class base {
	// content	

	void printInfo() {
		cout << "Print info from base class" << endl;;
	}
};

class subBase: public base {
	public:
		// ...

		void printInfo() {
			// print info from base class
			base::printInfo();
			cout << "Print info from sub base class" << endl;;		
			return;
		}
}
```

### Constructor for derived class:

Without default constructor for base class (parameterised constructor) and without explicit base constructor invocation in derived class, there would be an **error**

```c++
class base {
	// content	

	void printInfo() {
		cout << "Print info from base class" << endl;;
	}
};

class subBase: public base {
	public:
		// ...
		
		// Either
		subBase(int x): base(x) {
			// content
		}

		// or specifying a default constructor in the base class

		void printInfo() {
			// print info from base class
			base::printInfo();
			cout << "Print info from sub base class" << endl;;		
			return;
		}
}
```

Note:
* Constructor of parent gets call first, while destructor of children gets called first
* Derived classes **do not** inherit assignment operator from the base class
* Object of derived class can be assigned to object of base class (although there might be a loss of information)
* We cannot assign object of base class to object of derived class, since essentially they are two different objects, but this can be circumscribed with the following
```c++
ExampleClass& operator=(const ExampleClass& obj) {
     ExampleBaseClass::operator=(obj);
     return *this;
}
```
* If we want derived class to inherit assignment operator
```c++
class savings : public base {
	public:
		int age;
		long int ATM;
		savings(int x, int y): base(x), age(y) {
			return;
		}	
		using base::operator=;
};
```

### Multiple Inheritance:

```c++
class C : public A, public B {
	// content here
};
```

### Diamond Inheritance:

* There can be ambiguity as to which constructor of the two middle classes will get called
* There is only one constructor called, which resides in the last class

```c++
class A {
	// content	
};

class B : virtual public A {
	// content	
};

class C : virtual public A {
	// content
};

class D : public B, public C {
	// content
	public:
		D() {
			A();
		}
};
```

In the example above, class D will have the constructor of class A called, not class B or C