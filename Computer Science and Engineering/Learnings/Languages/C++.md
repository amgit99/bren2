# Compilation
The source (.cpp) text file is fed to the C preprocessor, everything that starts with # is handled by the preprocessor, this means copy-pasting header files from `#include` statements, evaluating `#IFDEF` statements, `#DEFINE` replacements. This spits out a source.i file which goes to the compiler. The compiler will give us the machine code directly or the corresponding assembly code as a source.s which is given to an assembler which gives us the final machine code.

The assembler will give out the source.o (object) files. This is the where the linker comes into play, which takes in other object files that we may have in our project, precompiled libraries and glues them together. The linker also links DLLs which the program expects at runtime. The linker gives us the final executable file.![[Pasted image 20231208135123.png]]
## Preprocessor Statements : :
`#INCLUDE` : :
`#DEFINE` : :
`#PRAGMA` : :
`#IF`/`#IFDEF`/`#IFNDEF` : : 
# Language Features
## Classes
`struct` is just a class that has its items public by default. We can have a struct with just some data items and functions that take in references to the instantiated object and alter this data. If functions are moved to the class, they act as member functions. 

<span style="color:#e1db3d">Access Specifiers</span> : There are three access specifiers `public`, `private` and `protected`. These can be used to control access of regions of the structs and classes. They affect visibility, but only in place to avoid accidents, they don't protect from malicious attempts at access. As can no feature in a language that gives explicit control of memory like C/C++.
`public` :  the data declared in this region will be visible to all.
`private` : the data specified in this region is visible to only member methods and friend functions.
`protected` : the data declared in this region is visible to member functions as well as heirs of the class. ^6f4867
```
class num{
private:
	int x = 9;
	int y = 10;
	char str[5] = "popl"; 
};

int main(){

    num n1;

	cout << "Address of the class :: " << &n1 << endl;
	cout << "val of the x :: " << *((int*)&n1) << endl;
	cout << "val of the y :: " << *((int*)((char*)&n1 + 4)) << endl;
	cout << "val of the ptr :: " << (char*)((char*)&n1 + sizeof(int)*2) << endl;
	
	return 0;
}
```
The code above can access the private data items, lets try that with functions, that will need us to learn how to cast to some function pointer. Sadly there is no trace of the functions with the object as one is instantiated, only the data items.

When a class is defined, any char[] strings that are defined with it in the class are stored only once, all the `char*` pointers point to the same string stored in the text segment of the memory.

A variable is `static` if its part of a class but not a part of every object, i.e the memory needed for this is just for one instance and not for every object that is created of this class. Like constant strings. This is used to avoid dependence on some global variable that could be forgotten or hard to track, and instead have that variable in the context of the class.
a `static` function will have <span style="color:#e1db3d">access to all static members</span> of the class but will not be associated with any object.  A <span style="color:#e1db3d">static member</span> is referred to by the name of the class and not some object.

`explicit` this keyword is used to prevent C++ to do implicit conversion on the time, while instantiating an object. 
```c++
class Date {
	int d, m, y;
public:
	explicit Date(int dd, int mm, int yy);
	friend XYX(int a);
};

Date::Date(int dd, int mm, int yy){ 
	d = dd;
	m = mm;
	y = yy;
} // we cannot write explicit here again in the definition.

int main(){

    Date birthday1{14,6,1999};    // WORKS
    Date birthday2 = {14,6,1999}; // ERROR
    Date birthday3(14,6,1999);    // WORKS

	return 0;
}
```
All the three styles of instantiating an object are valid when the explicit keyword is not used, but with explicit, the second one will throw an error, cause we are implicitly casting the triplet to a Date object before assigning it to birthday2. 
The <span style="color:#e1db3d">curly brackets</span> can be used to pass down an object because of the way any class or struct is stored in memory, the memory is exactly equal to the space taken by the data items, and they get assigned the values passed in the curlies in the order that they are declared in the class.

`const` is used for mutability, this is to make a class immutable by forcing it on the methods. The keyword `const` is the part of the type, so it should be present after the argument list in the definition and the declaration. 
`mutable` keyword is used to declare variables that can be changed by even if the objects themselves/methods that changing them are `const`.

```
class Date {
	int d, m, y;
public:
	int getDay(); 
	int getMonth() const; 
};

Date::getDay(){ 
	return ++d;          // This is allowed
}     
Date::getMonth() const { 
	return ++m;         // This is not allowed
}  
```
The way to give default values to the data items of the class is so:
```
class Date {
	int d{today.d};
	int m{today.m};
	int y{today.y};
public:
	// constructors and methods
};
```
A `friend` class is one that has access to the private data of some class, in which it is defined as friend. Similarly a `friend` function has access to the private data, it can be a global function or a method of a different class. The access specifiers have no effect on the this.
```
class A{
	int a;
	friend Class B;
	friend C::getA();
};
```
<span style="color:#e1db3d">Pointers to data members of a class</span> : The class must be specified along with the member name and is a part of the pointer type. This is necessary for the offset in memory that the pointer holds. The pointer needs to be associated with an object in order to access data items
```
class Data{
	int A;
	// rest
};

Data *dd, D;
dd = &D;     // pointer to class object

int Data::*ptr = &Data::A;   // pointer to class member (int A)
cout << D.*ptr << endl;

```

<span style="color:#e1db3d">Function overloading</span> : Functions whose signature has the same data types and only varies with `const`, `volatile` are ambiguous and cannot be resolved. A function and a function pointer are treated the same for an argument to a function, i.e that cannot be a differentiating. Functions differing 
## Construction Cleanup Copy Move : :
There are five instances where an object is either copied or moved :
- As the source of an assignment
- As an object initializer
- As a function argument
- As a function return value
- As an exception
The assignment operators work on objects and  have a predefined behaviour for all types of objects. The `=` operator copies all the contents of an object to a different location in memory. This means all data items, including pointers values, this means any arrays that are stored in the object aren't truly copied but the same memory is pointed to by the copy object. This is a <span style="color:#e1db3d">shallow copy</span> and to fix this the `=` operator must be overloaded and new space should be assigned for all such memory. 
Constructors can be private, but if the only constructor is private one cant create any objects.
When a constructor is created, the compiler does not create a default one for the class. So all methods of creating an object should match the template of the constructor created.
There is a <span style="color:#e1db3d">default copy constructor</span> that is called when the `=` operator is used. This does the job of sequentially shallow copying all the data of the object to the new object. We cannot make a constructor that takes a only a class object by value as it will create ambiguity with the default constructor. ^4b6f35
## Inheritance : :
Access specifiers in inheritance : The access specifiers have the above mentioned role inside the class, but when a derived class is inheriting from some base class, the access modifier is used to <span style="color:#e1db3d">regulate the flow of information & permissions</span> down the line of inheritance.
Usage :  `Class Car : public Vehicle { };`
If the access specifier is public like in the case above, all the public and protected members are inherited by the base class, and their access specifiers remain the same. <span style="color:#e1db3d">The private members cannot be inherited by an class, they are limited to the base class.</span>
If the access specifier is protected, all the public and protected members of the base class will be protected in the derived class.
if the access specifier is private, all the public and protected members of the base class will be private in the derived class and hence will ensure no further flow of information down the line of inheritance.
#### <span style="color:#984bd2">Any pointer of the Base class can be assigned the address of an object of the base class or the derived class.</span>
