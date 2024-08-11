
#### Conceptual Background
C# supports both, <span style="color:#e1db3d">parallel</span> and <span style="color:#e1db3d">async</span> programming. C# backend servers have thread pools, the number of threads is equal to the number of cores in the machine. Classically the .NET servers were not async and horrible, the nodeJS servers however, having only one thread, were async, and were vastly more performant than their on async counterparts.

<span style="color:#e1db3d">Task</span>:  

#### Efficient async await, Filip Ekberg
We have CPU bound and I/O bound, we ideally need these to happen in parallel. When some I/O bound operations are done we need <span style="color:#e1db3d">events</span> to inform us about its completion, later .NET incorporated this in the language to consume async APIs.

<span style="color:#e1db3d">Task Parallel Library</span>: Here they introduce a `Task` this takes care of work asynchronously, this could be done parallel on a different thread and if there are no threads available, this will be put in a queue and run later. These threads are software threads, they are managed by the runtime. When `Task.Run(() => { });` is called this returns a task instance that can be subscribed to. 
There are a few problems with this, having a lot of workflows on background threads may raise exceptions from the operating system for not leaving the user with a good experience, also every invocation of `Run()` consumes a thread, which is one thread less for accepting requests.
So we should rather use APIs that rely on async principles.

<span style="color:#00b050">Example</span>: If we have a UI element, a small window that shows the output of a database operation. If we perform the query synchronously on the same thread, that window will freeze up until the operation is done, this is not good user experience.
If we simply add `Task.Run( *database operation* );` to the code, this will solve the issue of bad UX, but it wont display anything to the UI element. This is because the new thread does not have access to the UI element.

<span style="color:#e1db3d">Never use</span> `Thread.Sleep();` <span style="color:#e1db3d">for dealing with race<span style="color:#e1db3d"> </span>conditions</span>.


