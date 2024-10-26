
| Property         | Value                                        |
| ---------------- | -------------------------------------------- |
| 📅 Date          | 15-09-2024, 21:32                            |
| 🏷️ Tags         | [[C++_]]                                     |
| 🔗 Related Notes | [[Programming in Modern C++, NPTEL IIT KGP]] |
##### Recap 01
According to the new standard of C, the return type can not be `void` .
`void` is also a type, it is used where the return type is not significant. Using the pointer to a void, `void*` is used when the type of data in the memory is to be interpreted by the user.
<span style="color:#e1db3d">Type Modifiers</span> are `short`, `long`, `signed`, `unsigned`
A <span style="color:#e1db3d">declaration</span> has the grammar of type followed by name of the variable.
<span style="color:#e1db3d">Initialization</span> is the setting of the initial value to the variable at its declaration or later.
<span style="color:#e1db3d">Literals</span> are the fixed values of some of the built in type eg `505`, `0173`, `0xF2` ...
- `212       // int, decimal literal`
- `0137      // int, octal literal`
- `0b1010    // int, binary literal`
- `0xF2      // int, hex literal`
- `3.14      // double, float literal`
- `'x'       // char, character literal`
- `"Hello"   // char*, Strign literal`
In C\*9, literals are <span style="color:#e1db3d">constants</span> and have all types same but `const`

<span style="color:#e1db3d">Expressions</span>, in the program expressions are entities that have some <span style="color:#e1db3d">value</span>. A literal is an expression, so is a variable, expressions connected by an operator is an expression and it too has a value. A <span style="color:#e1db3d">function call</span> is an expression <span style="color:#e1db3d">only if its return type is not void</span>.
Examples: `i = 10` , `i == 3`, `i == j ? 1 : 2` etc. cause this can be assigned to something else. This is because they have a value. 
A <span style="color:#e1db3d">Statement</span> is a command for a specific action, it has <span style="color:#e1db3d">no value</span>. An expression <span style="color:#e1db3d">terminated by a semicolon</span> is a statement. <span style="color:#e1db3d">Control structures</span> like if, if-else, switch, for, while, do-while, goto, continue, break, return are statements.
Examples: `;` , ` i = 10;` ,  `i == 3;`

##### Recap 02
- C supports two types of containers, <span style="color:#e1db3d">Arrays</span> and <span style="color:#e1db3d">Structs</span>. Unions are specialized structures where only one out of all members can be populated at a time.
- C supports two types of <span style="color:#e1db3d">addressing</span>. One is <span style="color:#e1db3d">indexing</span> i.e. relative to some start point in an array, and the other is <span style="color:#e1db3d">referential</span> where the address of a pointer can be stored and manipulated as a value.
- Arrays are stored in a<span style="color:#e1db3d"> row-major</span> format.
- <span style="color:#e1db3d">Union</span> is a special structure that has the space for the largest data member, and holds only one member at a time.
- Pointers are what makes C special, they are very powerful and the root of almost all problems known to man. They type pointer has a duality with the array container, such that a pointer can be used like a reference to an array, and the array reference has properties of a pointer. This feature has been abused by programmers, at least they think they have until they shoot themselves in the foot.
- Functions are procedures, take arguments and return values that could have any type or void. A function with empty parameter list in declaration may be passed any number of parameters and it is valid code, to prevent this one must add `void` in place of the parameter list. There is <span style="color:#e1db3d">no function overloading</span> in C.
- <span style="color:#e1db3d">Call by reference is not supported in C. However, arrays are passed by reference.</span>
- Function Pointers are a part of C, syntax is ` return_type (*name) (type param1, type param2 ...)`.


