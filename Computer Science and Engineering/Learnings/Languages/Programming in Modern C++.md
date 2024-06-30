Lot of the parts are covered in the [[C++]] file. There is another version of notes for this course : https://enshrined-halibut-60a.notion.site/cpp-a0d21aa303204f53b79f9dc5ebc50b4c
## Week 1 :
### Lecture 1:
### Lecture 2: 
### Lecture 3: 
### Lecture 4: 
### Lecture 5: 
### Tutorial 1: Build Pipeline

## Week 2 :
### Lecture 6: Constants and `inline` Functions
`#define` is used to define manifest constants or macros, they are replaced by the C Preprocessor, the compiler never actually sees any define statements or the things that are defined. 
The <span style="color:#e1db3d">NULL</span> in c++ is a manifest constant i.e `#define NULL 0`

<span style="color:#e1db3d">Constness</span>: Declaring a variable `const int a = 5;` means, the contents of the memory cell referenced by `a` cannot be changed. This acts as a different data type and a pointer to this should be of the type `const int*`. This is a <span style="color:#e1db3d">const pointer</span>, this is different from a <span style="color:#e1db3d">constant pointer</span> which is defined as follows `int* const ptr` this means the memory location of `ptr` cannot be changed, hence it can not point to a different integer. The both can be combined as follows `const int* const ptr = &a;` where the pointer is constant and points to a `const int`.

<span style="color:#984bd2">The simple rule is, which side of the star does the word const lie ?</span> 
This is important because `int const *` is same as `const int*` so the relative position of `*` and keyword `const` is what that matters.

`const` is better than `#define` due to the fact that its type safe and can be caught by the debugger. 

<span style="color:#e1db3d">Volatile</span>: This is a keyword that one adds to a variable to tell the compiler that said variable can change from an external source and hence any compiler optimizations that are being done must exclude this variable.
<span style="color:#e1db3d">Macros vs inline functions</span>: Macros have some pitfalls, since they are simply copy the text. This may cause problems of precedence, type checking etc. One can use `inline` functions, they are functions which are evaluated at compile time like macros.
Inlining may not work, its a choice done by the compiler, they should not be recursive.

### Lecture 7: References and Pointers
<span style="color:#e1db3d">References</span> just like <span style="color:#e1db3d">const</span>ants must be initialized. So declarations like :
	`int& i;` correct one is `int& i = j;`
	`int& j = 5;`
	`int& k = i + j;`  
will not work. This is because there is no memory location associated with the right side of the assignment operator. However, we can add the word `const` to change that.
	`const int& i = 5;`
	`const int& j = i + k;` 
the above cases will work, here the compiler will assign a memory location and put the number in it and the reference `i` will point to that location, also in the case of the result of the expression.
We use a <span style="color:#e1db3d">const reference</span> so send data to functions to prevent them from changing the original data.
A value can be assigned to a function call when the function returns a reference, but never should one return references to local variables of the function.![[Pasted image 20240224195102.png]]An array of references is not allowed.

### Lecture 8: Default Parameters and Function Overloading
<span style="color:#e1db3d">Good Practice</span> : Have the default parameters in the header files where the prototype is declared, its not necessary to have them in the definition as well.
C++ introduces <span style="color:#e1db3d">static polymorphism</span> or <span style="color:#e1db3d">compile time polymorphism</span> we can see this in <span style="color:#e1db3d">function overloading</span>, the binding of the function call to the actual operation happens based on not only the function name, but also the parameter types.

<span style="color:#e1db3d">Ambiguity</span>: The return values are not enough for eliminating ambiguity for function calls as the compiler can not figure out the binding based on just the function call. Another case is when a function has few default parameters and when calling that function with just the non default ones, the function call may resemble a different function with that exact number of parameters, such ambiguity cannot be resolved either.

Overload Resolution: This is done by a series of steps filtering functions until a single function is left, else it throws an error during compilation.
The steps are as follows :
\- Candidate functions( the ones who match by name )
\- Viable functions ( matching by # of parameters )
\- Best viable function ( by exact match for parameter types )

The criteria for <span style="color:#e1db3d">tie breakers</span> are as follows:
\- Exact Match ( lvalue to rvalue conversion, array to pointer conversion, function to pointer, convert pointer to `const` pointer ) These are loosely considered as exact matches.
\- Promotion ( types are promoted to a type of larger size int -> long long )
\- Standard Type Conversion ( conversion where precision is not same as before )
\- User Defined Type Conversion (  )
### Lecture 9: Operator Overloading
<span style="color:#e1db3d">Operators Vs Functions</span>: There is not a real difference between them as a entities, both perform some task, take in arguments and return some result (not necessary in case of functions). However the subtle differences are as follows, operators have a defined behaviour, they can be prefix, postfix or infix and they always return a value.
C++ introduces the `operator` keyword, one can use the following syntax instead of the regular operator syntax:
	`a + b;` can be replaced by `operator+(a, b);`
	`a = b;` can be replaced by `operator=(a, b);`
	`c = a + b;` is same as as `operator=(c, operator+(a, b));`
Rules of overloading :
\- No new operators can be created.
\- Intrinsic properties are the same, arity, precedence, associativity.
\- To distinguish between unary prefix and postfix operator, add an extra int to the function arguments as a hint to the compiler as so: 
`MyType& operator++(MyType& a, int);`
\- Some cannot be overloaded, `::` and `.` and `*` and `sizeof` and `?`.
### Lecture 10: Dynamic Memory Allocation
<span style="color:#e1db3d">Memory allocation in C</span>: There are three functions to allocate, `malloc`, `realloc` and `calloc` and the `free` function to free that memory.
<span style="color:#e1db3d">Malloc</span> - `void* malloc(size_t size_in_bytes);` this is the function signature of malloc, takes in a parameter of type size_t indicating number of bytes to be allocated and return a void pointer pointing to the start location of the memory. Returns `NULL` when the allocation is not successful.
<span style="color:#e1db3d">Calloc</span> - `void* malloc(int #elements, size_t size_of_element);`Similar to malloc takes in two parameters number of elements and size of each element and initializes the entire memory to 0.
<span style="color:#e1db3d">Realloc</span> - `void* realloc(void* ptr, size_t size_in_bytes);`This takes in a pointer to an existing chunk of memory and extends it or chops it down based on the new size provided. The extension might not always happen and the entire array may be moved to a different location leading to the address changing.

`new` is an operator C++, its not type oblivious. 
`operator new` is a function in C++(not the operator function of `new`) It behaves exactly like `malloc`
`placement new` - this weird ass operator takes a pointer to some memory(could be anywhere) and then gives us back a pointer of the desired type.
```
unsigned char* arr[8];
int* ptr = new (arr) int(5);
```
![[Pasted image 20240226133514.png]]


### Tutorial 2: Build Pipeline
<span style="color:#e1db3d">Preprocessor</span>: Include file headers, macro expansion, conditional compilation and line control, It works on `.c`, `.cpp` and `.h` files and produces `.i` files.
<span style="color:#e1db3d">Compiler</span>: Translates C++ code to Assembly, takes `.i` files and produces `.s` files.
<span style="color:#e1db3d">Assembler</span>: Assembly code to Machine/Object code, takes `.s` files and produces `.o` files.
<span style="color:#e1db3d">Linker</span>: It finds out all the symbols across different translation units and precompiled `.o` files and relates them. takes `.o`, `.so` files and produces `.out` executable.

## Week 3 :
### Lecture 11: Classes and Objects
`class` brings massive improvements over `struct` of C++ by adding OOP features and member methods. The class defines a `namespace` which encapsulates the methods. 
Member methods are always invoked with some object of that class. The methods can be called from external functions like main if they are `public`.

Every instance of a class can be distinguished by it address which is given by the `this`
keyword in the class scope. This is a constant pointer, i.e `T* const this = &object`
`this`  is only available in the member functions.
The `this` pointer is implicitly passed to the non-static member functions as so:
	`class X{int fun(int a); ...};` --> `int X::fun(T* const this, int a);`
	`X a; a.fun(5);` -->  `X::fun(&a, 5);`
### Lecture 12: Access Specifiers
[[#^6f4867]]
### Lecture 13: Constructors Destructors and Object lifetime
A default constructor and default destructor is provided by the compiler.
The constructor does not have am implicit `this` parameter.
If there is not dynamic allocation done by us then there is no real use for a destructor.
The destructor is called when the object goes out of scope.
more here - [[C++#^4b6f35]]
### Lecture 14: Copy Constructors 
The only way to write a copy constructor is by a reference,
`MyType(const MyType& original);` this is the ideal case with the `const` specifier. If one makes a <span style="color:#e1db3d">constructor with call-by-value</span>, it would lead to an <span style="color:#e1db3d">infinite recursion</span>.
Since the call-by-value need a copy and a copy constructor for its execution.
<span style="color:#e1db3d">Shallow copy</span> is the default copy behaviour of objects, this means the pointers in the class are copied as well. Now, if a destructor involves `delete` to free the dynamically allocated memory, every destructor would try to free the same memory.
<span style="color:#e1db3d">Self copy</span>: this is the special case where assignments like `s1 = s1;` occur. These may occur due bad programming. But copying methods which involves freeing the memory of the first object and allocating memory according the dynamic contents of the second object will fail, because both of the objects are same. A simple `this == &s1` must be done before moving forward.
### Lecture 15: `const`ness
Having const objects is tricky, the `this` pointer of a normal object is of 
type - `MyType* const` i.e. a constant pointer, it will point to the same object always. When we add a const in front of the object instantiation, `this` pointer takes the 
form - `const MyType* const`. This will cause issues because all the member methods implicitly expect a pointer of type `MyType* const` and not `const MyType* const`. So there will be a type mismatch and all methods will not work.

<span style="color:#e1db3d">Constant Member Functions</span>: To get around the above issue, we can define a function to be constant as so - `void print() const { cout << this->a << endl; }`

### Lecture 16: `static` data members
It is associated with a class and not an object, it shared by all objects, it is initialized before `main()` starts and is destroyed after `main()` ends.
We need scope resolution operator `::` to access these functions.
The order in which static data members are listed in the class does not matter. It is the order in which they are initialized is what matters.
A static member function can not be `const` ![[Pasted image 20240228162054.png]]
<span style="color:#e1db3d">Singleton</span>: A famous design patterns, where only one object of a class should exist, we can use `static`  data and functions to get this done.
```
class Printer{
	Printer(){} // a private Constructor.
	~Printer(){}
public:
	static Printer& get_printer(){
		static Printer printer(); // initilized once and used again
		return printer;
	}
	void print(string& s){ cout << s << endl; }
} // Meyer's Singleton

int main(){
	Printer::get_printer().print("singleton!");
}
```

## Week 4 :
### Lecture 17: `friend`
![[Pasted image 20240228211422.png]]![[Pasted image 20240228213123.png]]
## Lecture 18: Operator overloading 
Member functions are better than global functions for overloading.
Overloading the post increment operator takes a parameter `int` to tell the compiler, as the syntax for pre and post is same.
```c++
class MyClass{
	MyClass(int d): data(d) {}
	
	MyClass& operator++(){
		++data;
		return *this;
	}
	
	MyClass operator++(int){
		MyClass temp(data);
		++data;
		return temp;
	}
};
```

