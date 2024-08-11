One thing different from the Access Modifiers in C++ is that we can use multiple.

`private`: This used before any attribute or method would mean, the usage of that that attribute or method can only happen from the code inside that class. Any calls or references to said entity from the code that is outside the curly braces of the class definition will not be allowed.
`internal`: This has the scope of the Assembly, any entity marked internal can be accessed from within the assembly.
`public`: This is accessible anywhere, from any place in the entire project, this entity can be referenced.
`protected`: This has to do with inheritance, if an entity is protected it can be called/referenced from the same class or any of its sub-classes that inherit from it.

`private protected`: This behaves in different ways depending on the assemblies, If the base and the subclass are in the same assembly, it acts like protected and if the are in different assemblies, it acts like private.
`protected internal`: This too changes behaviour depending on assemblies. Within the assembly it acts like protected, and for any other class that inherits it (could be in a different assembly) it acts like internal.