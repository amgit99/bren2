# The Linux Systems Programming Handbook :

<span style="color:#e1db3d">A Zombie process</span> : When a process forks a child process, it can invoke the `wait()` system call to check if the child has finished execution. Any process can stop running using the `exit()` system call which causes it to relinquish its resources. If a child process has terminated by the `exit()` but the parent process is yet to check it by using wait, then the process is called a Zombie process.
The list of processes are saved as a circular doubly linked list of structs called `task_struct` in the linux kernel.
There are 5 states that a process can be, this is given by the `state` attribute of the `task_struct`
## Chapter 2 :
The job of the OS can be broken down into two parts, a resource manager(process scheduling, I/O, Virtual Memory) and the abstraction layer that lets the user seamlessly interact with the system (device drivers, file system, GUI, shell).

Kernel Mode vs User Mode : Areas of virtual memory can be marked as being part of user space or kernel space.
Jobs of the kernel(modern) : Process Scheduling, Memory Management, File System, Creating and Termination of Processes, Access to devices, Networking, System Calls(API).

<span style="color:#e1db3d">A shell</span> : It is a special-purpose program designed to read commands typed by a user and execute appropriate programs in response to those commands. Such a program is sometimes known as a<span style="color:#e1db3d"> command interpreter</span>. There were many famous ones, Bourne shell, C shell, Korn Shell, Bourne Again Shell(GNU remake of original). The shells are designed not merely for interactive use, but also for the interpretation of shell scripts, which are text files containing shell commands. For this purpose, each of the shells has the facilities typically associated with programming languages: variables, loop and conditional statements, I/O commands, and functions.

Users and Groups : Every user of the system has a unique login name (username) and a corresponding numeric user ID (UID). For each user, these are defined by a line in the system password file, `/etc/passwd`, which includes the following additional information:

- Group ID: the numeric group ID of the first of the groups of which the user is a member.
- Home directory: the initial directory into which the user is placed after logging in.
- Login shell: the name of the program to be executed to interpret user commands.

The password record may also include the user’s password, in encrypted form. However, for security reasons, the password is often stored in the <span style="color:#e1db3d">separate shadow password file</span>, which is readable only by privileged users.

<span style="color:#e1db3d">Groups</span> are for administrative purposes, each group is identified by a single line in the system group file, /etc/group, which includes the following information:

- Group name: the (unique) name of the group.
- Group ID (GID): the numeric ID associated with this group.
- User list: a comma-separated list of login names of users who are members of this group (and who are not otherwise identified as members of the group by virtue of the group ID field of their password file record).

Directories and Files : There is a single hierarchy in linux for files, which forms a tree structure, the external disks simply attach themselves as subtrees to the main tree. Files can be of several types, there are the simple files that are the ones with ordinary data, there are also other types of objects also categorized as files like, devices, directories, pipes, sockets, symbolic links. Directories are tables of file_name ←→ reference pairs, this file_name reference pair is called as a link. Directories can have links to other directories, it has a link “.” and a link “..” for self and parent directory. 

Symbolic Link/Soft links : This seems like a new name to a file, its a name ←→ reference pair that references another link, that might have a different name.

Path name : It is a string consisting of an optional initial slash (/) followed by a series of filenames separated by slashes. All but the last of these component filenames identifies a directory (or a symbolic link that resolves to a directory)

Each process has a current working directory (sometimes just referred to as the pro- cess’s working directory or current directory). This is the process’s “current location” within the single directory hierarchy, and it is from this directory that relative path- names are interpreted for the process.

Every process has a directory as its working dir. Child processes inherit the working directories of parents, the working dir of the shell is mentioned in the `/etc/passwd` file and can be changed by `cd`.

Process termination and termination status : A process can request termination or be killed by a signal, with the help of the `exit()` family of system calls, a non negative integer is passed to the exit based on the condition of the termination, the signal number is sent if signal is involved, that forms the exit code/status.

*Real user ID* and *real group ID* : These identify the user and group to which the process belongs.

*Effective user ID* and *effective group ID* : These two IDs (in conjunction with the supplementary group IDs discussed in a moment) are used in determining the permissions that the process has when accessing protected resources such as files and interprocess communication objects

## Chapter 3 :

This chapter discusses System calls and Library functions. Steps involved in executing a system call. 

The application program makes a system call by invoking a wrapper function in the C library. This wrapper makes available all the arguments to the system call trap handling routine. The wrapper gets them via the stack which it places in the appropriate registers as per the sys-call requirements. The sys-call number is placed in the `%eax` register to let the kernel know which sys-call is called. The wrapper executes `int 0x80` instruction to switch from user mode to kernel mode. 
![Untitled](Untitled.png)

Error codes : A system call returns -1 if an error has occured, the type of error can then be resolved by checking the variable `errno` which is set according to the error. Some system calls have -1 as legitimate return values, ambiguity can be resolved by setting `errno` to 0 before the call and checking if it has changed, then the return value of -1 is actually an error.
## Chapter 4 :
<span style="color:#e1db3d">File descriptor</span> : a (usually small) nonnegative integer. File descriptors are used to refer to all types of open files, including pipes, FIFOs, sockets, terminals, devices, and regular files. Each process has its own set of file descriptors.

0 - standard input, 1 - standard output, 2 - standard error, these are the popular file descriptors.
if `open()` succeeds then it will use the lowest available fd number available .

# UC Berkeley Course :

## Lecture 1: Introduction

What constitutes an OS is changing with time. Its purpose ? Its an <span style="color:#e1db3d">illusionist</span> ! It essentially wants to create an environment with essentially infinite memory and a processor idle to serve the users process. It abstracts the hardware, the processor is seen as a bunch of threads, the physical memory is seen as an address space, the network card is seen as sockets, the secondary storage is seen as a file system. On top of these abstraction there is the abstraction of processes, which are like virtual machines that let your program run and use the above mentioned abstraction to use the hardware and get things done.

Docker: A way to wrap up multiple such environments and run them 

Processes: Address Space, One or more threads of control executing in that address space, open files, sockets…..![Untitled](Untitled%201.png)
Along with being an illusionist, it also acts as a referee, that regulates the the usage of hardware by any one process.
## Lecture 2: Four fundamental OS concepts
The four fundamental OS concepts : Threads, Address Spaces, Processes, Dual Mode Operation.
<span style="color:#e1db3d">Threads are virtual cores</span>, they are resident in the actual core if its that threads turn, so multiple threads are multiplexed in time. The switch from one thread to another is the place where the OS takes over. 
![Untitled](Untitled%202.png)Address Spaces The set of accessable address. There are 2^64 addresses in a 64-bit processor, some of them are actually backed by DRAM, and they have some state associated with them. Writing to an address can have the following consequences:
![Untitled](Untitled%203.png)![Untitled](Untitled%204.png)
In this case the threads can access the entire memory, there is no protection. 
<span style="color:#e1db3d">Simple Protection:</span> For every thread save the base and bound, and apply checks on every access, to make sure that every thread just wants to access the memory allocated to it.
![Untitled](Untitled%205.png)
Such explicit assigning will cause fragmentation. So we we come up with paging, where we divide the entire address virtual address space, supported by the processor, into chunks of equal size that can be brought into memory.

<span style="color:#e1db3d">Process</span>: Restricted Execution Environment. It owns some part of resources (memory, files) and has only access to those resources. It encapsulates one or more threads. Translation is the key idea that allows isolation. 
Process can have many threads of execution, each thread has its <span style="color:#e1db3d">own stack and registers</span>, the code, data, files are shared between threads.

Dual Mode Operation: ![Untitled](Untitled%206.png)

Three cases where the OS can switch to kernel mode, sys-call, interrupt, trap/exception. 
<span style="color:#e1db3d">Context Switch</span>: Process A running → timer goes off → switch to kernel mode → save process A PCB → load process B PCB → switch back to user mode and voila ! we have switched to process B.
## Lecture 3: Threads and Processes

The translation of the virtual memory of a process to the physical memory offers security. The dual mode operation allows processes to not tamper with their translation units. Since these privileges are not granted to the average process running in user mode.

A process is an instance of a running program. Multiprocessing means having multiple CPUs each running individually, Multiprogramming is time multiplexing of the CPU for an illusion of having multiple CPUs. 

A thread can be in one of three states READY, RUNNING, BLOCKED.

The libc is the mostly standardized C library that has wrappers around system calls.

Fork-Join patter: A thread creates multiple threads and the threads are joined when they finish execution, and the parent thread waits on these threads until this happens. 

Threads that share the same state(heap, globals, data) may be scheduled in any possible execution order which leads to inconsistent results if proper locking practices are not applied.

Locks are objects that are accquired by a thread that allow it to perform a task without the worry of another thread interfereing with the execution.

The `init` process is the first one that is created by the kernel, every other process is a child of init.

System Calls:

- fork: Copies a process and start running. The identical process is running in its own address space.

If a process is killed, and it has child processes, the orphan processes are then adapted by the parents of the killed process, all the way upto `init`.

## Lecture 4: Files and I/O

Semaphores: An integer that cannot take a negative value. There are two functions on a semaphore P( ) and V( ). That decrement and increment a semaphore atomically. With different values of Semaphores we can use it differently.

![Untitled](Untitled%207.png)

The thread running the ThreadJoin block does not get to decrement the already 0 valued semaphore and goes to sleep, only when the ThreadFinish function finished the value of the semaphore is incremented and thw ThreadJoin fn is signalled to wake. This behaviour is like the actual thread join patter.

The `sigaction` object is instanciated, it is used to send signals.

![Untitled](Untitled%208.png)

The absolute paths are the ones that start with “/”. They are relative to home.

The rest are relative to the PWD. the “~” however makes it absolute.

the ~/boo.txt and ~xyz/boo.txt are different. The first is for the current user and the second is for the user xyz. 

The EOF is -1, 32 1s, this is 

The open and create return integers, which are offsets in a table that holds file descriptors. This is for security reasons. `fread()` is much more sophesticated and checks if the file is in the buffers and sets optimal buffer sizes. User level buffering is much better, system calls are 25x slower than normal function calls.

![Untitled](Untitled%209.png)

## Lecture 5: IPC, Pipes and Sockets

Communication between Processes: Shared memory, the memory translation modules for processes that wish to communicate both map some addresses to a same physical page in memory, this can now be used to communicate. Another method is that the kernel provides an in-memory queue to which processes read and write. Such a queue is called Pipe in the UNIX like operating systems. When the producer writes to the pipe, signals can be used to wake the consumer. 

We can pipe, which gives us 2 FDs, One that can be used to read and the other for write.

Sockets are standardized by POSIX, they can be used to talk on the same device, IPC, the same job can be done with pipes, but sockets work on 

A socket has 2 queues for the input and output, while a pipe has just one. On accepting a connection the connection can be pawned off to a different process or a thread, based on the level of protection required.

Sockets Flow : socket → bind → listen → accept → close .

## Lecture 6: Sync : : Concurrency and Mutual Exclusion

The states in which a process can be, are queue data structures. The threads are the ones that actually execute so when a context switch happens, if the new thread and the current thread belong to the same process, the environment and the page tables need not be updated, just the stack and locks and files. 
`yeild()` system call is used to relinquish control of the cpu. 
<span style="color:#e1db3d">The kernel has its own stack and a place to store its own execution context</span>. This is how it changes the contents of the current thread with the new thread while its running, cause its running out a different location.

![Untitled](Untitled%2010.png)

The `switch()` function of the kernel copies everything one by one, and this is where something beautiful happens, in the above case there are only two threads, when one calls `yeild()` the other is scheduled. So when thread S called run_new_thread and it calls switch, the switch routine copies all the data of thread T to the stack and as it returns the CPU has morphed into the running state of thread T. 

![Untitled](Untitled%2011.png)
To make sure that a user process will not hog all the CPU, there are interrupts and times that go off and unconditionally puts the kernel into execution. 

Locks are values that a thread can acquire and release, these operations are atomic so only one thread can get hold of the lock, critical section software should be written 

## Lecture 6.5 : Sync : : Concurrency and Mutual Exclusion (2)

Busy Waiting solutions are not so good, they just end up wasting cycles, waiting process should just go to sleep. Semaphores are just general locks, where the value can be greater than 1 as in case of locks. The `P()` AND `V()` are the functions that we can call on the semaphore, that decrement and increment the value, these operations are atomic which ensure only a certain number of times the semaphore is taken.

The bounded buffer problem: We will need three semaphores, 2 counting and 1 binary. They are, remaining slots, full slots, and a mutex.

![Untitled](Untitled%2012.png)

## Lecture 7: Sync 2

![Untitled](Untitled%2013.png)

## Lecture 8: Sync 3 ****Atomic Instructions (Con't), Monitors, Readers/Writers****

![Untitled](Untitled%2014.png)

The lock has a waitlist, that holds the list of threads that are waiting on the lock.

The switch procedure, triggered by a timer interrupt also disables interrupts and then performs the context switch. 

![Untitled](Untitled%2015.png)

Disabling interrupts on all processors requires messege passing, which is very expensive operation. 

![Untitled](Untitled%2016.png)

The following is a lock free implementation of a usecase of adding a node to the start of a linked list.

![Untitled](Untitled%2017.png)

In the above example, we load the the address of the head in the register R1 and make our node point to the current head. Now, when the `compare&swap()` runs, its given the following arguments: register R1, the address of our object and the root. It checks if the root still points at the same node as the one we stored in R1 (meaning no-one has inserted anything in betweeen) since that load instruction and the execution of the atomic compare&swap. The the root is made to point at the new node, hence adding it to the linked list.

![Untitled](Untitled%2018.png)

This will cause busy waiting, the threads that dont get hold of the lock simply keep wasting cycles. To beat this we can have test&set to work on a seperate variable, called a guard that is accquired and then the lock is accquired and, if the lock is not availabe the thread goes to sleep whithout releasing the guard. This is same as disabling interrupts for system locks.

![Untitled](Untitled%2019.png)

## Lecture 10: Scheduling

Scheduling is necessary for Minimizing Response time and Maximizing Throughput and making it fair for all the users. 

FCFS has the convoy effect, many tiny burst processes get stuck behind the chonky process.

Round Robin : + Fair to all processes + responsive | - context switch overhead.

Priority Based Scheduling : many queues, each corresponding to priority levels, priorities can be donated. During locking, if a low priority process has a lock then the higher priority process that is waiting on the lock essentially finds itself in a priority inverted state.

## Lecture 13: Memory Management

Reasons for this Memory Multiplexing :                                                                                      Protection - It must be ensured that one process does not access the data of another process. Various parts of the memory pages can be given different behaviour (i.e. Read Only, Modify…) Translation - Address reusing can be done, several processes now can have their own virtual address space that are independant of each other. This means the same value of an address can be used by a process which will later be translated.

When accessing any part of the virtual memory, many thing can happend :                                                 One could access any values that are at the location, or one could trigger an I/O access operation (if there is memory mapped I/O involved), there could be a segfault if the memory between the stack and the heap is accessed, or one could communicate with another process (IPC).

`sbrk` system call is used to allocate more memory to the heap.

![Untitled](Untitled%2020.png)

Segmentation : Since we cannot find contigious space everytime in memory due to fragmentation the program is divided into segments that individually can be stored at different areas in the main memory, this reduces fragmentation, but every segment requires bound checks and translation. This still does not give us granular control as they vary in size and internal fragmentation (process not using all the allocated space) is also an issue. Swapping these segments in and out of memory is expensive if the number of processes increase in memory. This brings us to general address translation that we now know i.e. Paging, where the quanta of data transfer between disk and memory is fixed(page) and the entire virtual address space is divided into pages of a fixed size.

![Untitled](Untitled%2021.png)

## Lecture 14: Memory Management 2 (Virtual Memory)

Page Table : The page table is the translation unit that holds mapping of a virtual address to an actual address. There can be a lot of virtual pages (for a modern 64-bit machine) but only few of them correspond to actual pages in memory, these pages that have their physical counterparts are indexed starting from 0. So if a virtual page that doesnt actually correspond to any valid memory will have a virtual page ID/Index that will be too high and hence the bounds checking will throw an error. The virtual address can be divided into two parts, the offset into a page (lower order bits) and a virtual page index/ID, this ID is used to index into the page table which stores the corresponding physical address. The table has metadata like valid bits, permissions, etc. 

![Untitled](Untitled%2022.png)

The page table is stored in ther kernel heap, the PCB has pointers to this page table. To deal with the extreme sparseness of the page tables, we have multilevel page tables, where the virtual address is further divided and every part is used to index at each level. The page table itself is stored as pages in secondary storage and those are accessed by higher levels of the page table, and the parts of virtual address are used to index these pages.

![Untitled](Untitled%2023.png)

All of this allows for Demand Paging where only the necessary pages of a process are kept in main memory, while the rest reside on secondary storage. 

![Untitled](Untitled%2024.png)

Caching is done for the page table for quicker accesses. TLB is the name of the cache. The three Cs of cache misses(Compulsory misses, Capacity misses, Conflict misses).

## Lecture 15: Memory Management 3 (Caching and Demand Paging)

Direct Mapped is faster than the Set Associative and Fully Associative cache. The reason is the length of the critical path, There will be a comparator in all the caches and then following that there will be a Multiplexer to select the best one out of the set. Direct cache avoids this.

The TLB and the cache can actually work in parallel, the virtual address is divided and the first part, the index is used to find the physical address and the offset is used to find the data item in cache, if its present.

Demand Paging is like treating the memory as a cache for the secondary storage. If a page is needed and is not in memory, the page is brought and is kept in memory, just like the cache

`FindBlock` is used to return the disk block, we provide it with the Pid and page number.

## Lecture 16: Demand Paging

Page Fault: Page table entries in the page table, have valid and invalid bits as markers. When there is a page fault, the OS must find the page in secondary memory, then it must find a page slot that is not in use, if the page that we are replacing, if the dirty bit is set of that page, we must write it back to secondary storage. Then we load the new page (possibly simultaneously as we store the old page). This will take millions of instructions worth of time and the process is put to sleep. The TLB must be updated all the way up, about the existence of the new page. The old page table entry that brought us here to an invalid page must be invalidated too. 

![Untitled](Untitled%2025.png)

The size of the cache shows a Zipfs law sort of distribution, a very tiny cache has very substantial effect but there is quickly diminishing returns as the size increases. If there is just 1 page fault in 1000 page accesses, the speed of the the DRAM slows down by a factor of 40! The number of faults heavily depend on the access patterns of the application, and less on the size of DRAM.    This means the replacement policy is critical.

Prefetching is can be dont to prevent the cumpulsory misses. Capacity misses means that the size is the bottleneck, adding more DRAM might not be so fast, instead one may reduce the space allocated to each process in the system. Conflict misses dont exist, Policy misses, this is when a page in memory is kicked out due to a bad replacement policy.

FIFO - Can be bad, the most used page might be brought in first and its dumb to evict it.

Random - This can be good in fully associative cache, also easy to implement but unpredictible.

The optimal policy is, to remove the page that will not be used for the longest time in the future. But this is not possible to implement, LRU is good too, using past to predict the future. The locality dictates whats not used for a long time is less likely to be used again. The issue with LRU is implementation, linked list with latest one at head and evict from tail, but that is expensive.

LRU in case of 3 empty page slots and 4 alternating pages, prerforms in its worst case. FIFO performs worse on increasing memory(Belady’s anamoly). 

LRU Clock Algorithm : This alogrithm tries to approximate the LRU algorithm. All the blocks in the memory are arranged in a circular linked list, and a clock hand (a pointer) moves through the list on every page fault, trying to find old pages to kick out, there is a bit that is set when a page is loaded/accessed, it indicates that the page is freshly used, when a hand reached a fresh page, it sets its bit to 0 denoting that the page is somewhat stale and moves on to look for a stale page to kick out.

## Lecture 17: Demand Paging(finish), general I/O, Storage Devices


#### I/O 
A chip without I/O is like a disembodied brain, since there are so many types we need a common interface to interact with I/O. <span style="color:#e1db3d">A Bus</span> is set of communication wires, there are many devices on the bus, there are protocols that governs how comms happen.
Peripherals Connections Interconnect Express(PCIe) : no longer a parallel bus, multiple fast serial channels.![[Screenshot 2023-12-13 at 3.40.23 PM.png]]Most of the things get hidden by the device driver, so the kernel doesn't need to know anything below the driver level. The device controller, the card that holds the buffers, special registers etc, the inputs are fed through a bus and usually has separate lines for interrupts.
<span style="color:#e1db3d">Port-Mapped I/O</span> : x86, special instructions for I/O.
Memory Mapped I/O : the device is mapped to the address space, deal with it just like memory.

## Lecture 18: General I/O, Storage Devices, Performance
DMA : We tell the controller to transfer something to memory from t


## Lecture 23: Networking and TCP
