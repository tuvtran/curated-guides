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
}
```

Note:

* Constructor and destructor need to be public
* If we only declare the object, the default constructor will be invoked
* Multiple destructors of the same class are not allowed in C++