C# has functions as first class objects in the language. A <span style="color:#e1db3d">delegate</span> in C# is essentially the signature of a function, it could be a placeholder for all the functions that match a signature(return type and ordered argument list). Delegates are cleaner ways of doing <span style="color:#e1db3d">function pointers from C</span>. With a delegate you can define the blueprint/signature of a function sort of like an interface, give it some name and then pass that around in code. Now, any function which conforms to that interface/blueprint/signature can be used where the delegate is used.

The delegate can be a part of the arguments for a method and we can pass on any function that matches that signature. This is very similar to the [[Strategy Pattern]].

Using a delegate we can create an anonymous function
```
delegate int MathOp(int x, int y);

static void DoSomething(int x, int y, MathOp fun){
	var result = fun(x, y);
	Console.WriteLine(result);
}

static void Main(string[] args){
	DoSomething(5, 10, delegate (int x, int y) { return x + y }); -------- (1)
	DoSomething(5, 10, (int x, int y) => { return x - 7 }); -------------- (2)
	DoSomething(5, 10, (int x, int y) => x + y); ------------------------- (3)
	DoSomething(5, 10, (x, y) => x - y); --------------------------------- (4)
}
```
<span style="color:#e1db3d">(1)</span> From the above example we can see that we can send an anonymous function as a parameter and the compiler figures out the return type. The name is assigned by the compiler, one may use a debugger to check the name of that function.
<span style="color:#e1db3d">(2)</span> The syntax can be changed to be written in a way more analogous to the syntax of the modern lambdas.
<span style="color:#e1db3d">(3)</span> One may skip the curly braces if the function body contains a single line.
<span style="color:#e1db3d">(4)</span> Now we have something that looks like a modern lambda. Easy to read (for someone who is aware) and short.

A <span style="color:#e1db3d">method group<span style="color:#e1db3d"></span></span> is a set of functions with the same name but different signatures. Essentially overloads of a same function. If there are multiple overloads for a function and we pass that function name as an argument, if the argument is specified using a delegate, the overload that matches that delegate's signature is used.
We can have multiple delegates with the same signatures but different names.

One key difference between C++ function pointers and the delegates is that the C++ function pointers only store a memory location that is later interpreted by the compiler as the start of the procedure/subroutine. Here the delegate not only has the reference to the function but also the specific instance of object that is associated with it. This means we are not forced to use static methods to go where a delegate sits, we can pass non static ones and the output may vary based on the state of the object. <span style="color:#e1db3d">This is similar to the member function pointers of C++</span>.
The delegate has two properties the <span style="color:#e1db3d">target</span> and the <span style="color:#e1db3d">method</span>, the target is the instance of the object and the method is the method of the class that the object is associated with. If the delegate is a placeholder for a static method then the target is `null`.

##### Special Delegates
<span style="color:#e1db3d">Action</span>: This is a delegate defined in the System Library, it has a special signature, its return type is <span style="color:#e1db3d">void</span>. It is generic, so we can pass types as parameters and we will have corresponding delegate. Its actually a family of 16 delegates that Microsoft has provided each with increasing number of generic type parameters, all named Action. They all have the return type of `void`.
```
delegate void Action();
delegate void Action<T1>(T1 t1);
...
delegate void Action<T1, T2, T3, ... , T16> (T1 t1, T2 t2, ... T16 t16);    // yeah redeculous!
```


