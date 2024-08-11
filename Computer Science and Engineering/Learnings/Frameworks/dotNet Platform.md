In the early days there was Visual Basic, but that was not a very serious programming language. Apart from that Microsoft only had C++, but given the language's unrestricted nature there are a lot of errors, mostly due to null pointer exceptions, hence it was not very productive.
At that time Java showed the "write once run everywhere" promise, so Microsoft had to come up with the <span style="color:#e1db3d">CLR</span> (common language runtime), this was a common runtime for C++, Java (in very early stages). Later the open-source community came up with <span style="color:#e1db3d">Mono</span>, the inter platform framework that was getting better than .NET Framework. 
Later Microsoft made everything open-source, these tools for developers were made free cause they realized that the tools will no longer be the source of revenue, but cloud is the future. For this exact reason Microsoft embraced linux as it the operating system for the cloud.

<span style="color:#e1db3d">.NET Core</span> is the open-source, cross platform, minimalistic version of the .NET Framework.
The different platforms grew so apart that it got confusing, so Microsoft had to come up with a base set of APIs common to all frameworks, this was the <span style="color:#e1db3d">.NET Standard</span>.
Then everything was unified, one base class library, one framework for all languages over all platforms. This was simply called <span style="color:#e1db3d">.NET 5</span>.

The even number version are the <span style="color:#e1db3d">LTS versions</span>.
<span style="color:#e1db3d">ildasm</span> is the .Net intermediate language (MSIL) dis-assembler.

#### Solutions, Project System, target Framework, CLI
<span style="color:#e1db3d">NuGet</span> is the package manager for C#. The <span style="color:#e1db3d">target framework moniker</span> is essential to tell the type of project.

A <span style="color:#e1db3d">Solution</span> is a collection of <span style="color:#e1db3d">projects</span>.
The Projects have a type, it could be a class library (collection of functionality, for shared purposes), or an console app (one with UI that consumes the functionality in other libraries). A project may contain common functionality that can be used by other projects, for this we must add a reference of the library project in the project that intends to use the library functionality.
When we add a reference, in the `.csproj` file we can see the entry of the reference.
Namespaces are present for isolation, which helps with common names for symbols, a prefix must be added while using any entity to resolve which namespace it belongs to. We can also use the `using` keyword for avoid writing the prefix as in C++.

#### C# 
Actually to be called C++++, the four pluses arranged in a grid creating the # sign.

Basics:
- [[CS Type System]]
- 

A <span style="color:#e1db3d">record</span> is syntactic sugar for making simple classes much more easier to read.
```
class Hero{
	public Hero(string firstName, string LastName, string heroName, bool canFly){
		FirstName = firstName;
		...
	}
	public string FirstName { get; }
	public string LastName  { get; }
	public string HeroName  { get; set; }
	public bool CanFly      { get; set; }
}                                                                                   ------- (1)

record Hero(string firstName, string LastName, string heroName, bool canFly);       ------- (2)
```
<span style="color:#e1db3d">(1)</span> This is a generic simple class definition.
<span style="color:#e1db3d">(2)</span> This is a record, provides same functionality and is much simpler looking.

The struct instances are always stored on the stack, but class instances are completely stored on the heap. 
<span style="color:#e1db3d">Instantiating</span>: So drawing parallels to C++, one can say that instantiating a C# struct object is like instantiating a regular C++ struct/class object (there isn't any difference apart from OOP parts), however instantiating a C# class object is like instantiating a C++ struct/class object on heap using a pointer (using the `new` keyword). 
When a C# struct object is instantiated, it is entirely on the stack memory, `int`, `double` etc are structs by definition. The C# class objects however are instantiated entirely on the heap, including all primitive datatype fields of the class, what resides on the stack is a single reference to the memory in the heap.

<span style="color:#e1db3d">The Garbage Collector</span>: The [[CS GC]] will run and remove all the leaking memory (all the heap allocated memory from the class object instances).

<span style="color:#e1db3d">Optional Data</span>: There is `null` in C#, and nullable types are ones with the datatype name followed by a "?", eg: `int?`, `string?` etc.
Having a `string? text = null;` means that the reference has taken space in the stack but no heap memory is allocated for the class object `string`. Later when an assignment is done the memory is allocated. Re-assigning it to `null` frees the memory allocated on the heap as well.

`IEnumerable` is the interface of that many collections implement, it means all those collections have forward, backward, readonly enumerability. 

<span style="color:#e1db3d">Advanced Topics</span>:
- [[CS Access Modifiers ]]
- [[CS Delegates and Lambdas]]
- [[CS Events]]
- [[CS Generics]]
- [[CS async await]]
- [[CS LINQ]]
- 