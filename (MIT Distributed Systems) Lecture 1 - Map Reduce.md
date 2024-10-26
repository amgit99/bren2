
| Property         | Value                        |
| ---------------- | ---------------------------- |
| 📅 Date          | 20-10-2024, 13:48            |
| 🏷️ Tags         | #systems #disributed_systems |
| 🔗 Related Notes |                              |
Infrastructure can be generally divided into three categories,
- Data Storage Systems
- Communication Systems
- Computation Systems

One of our goals is to have abstractions, providing interfaces to the above mentioned systems, interfaces which are known and similar to their non-distributed counterparts. All this while hiding the fact that they are performant, fault-tolerant distributed systems underneath.

<span style="color:#e1db3d">Topics/Tools</span> to solve the issues with distributed systems:
- Remote Procedure Calls
- Threads
- Locks & Concurrency Control
##### Scalability
Singlehandedly the most important reason to do distributed computing, the power of simply adding compute horizontally and getting equivalent boost in speed up is the desired goal.
##### Fault Tolerance
When running a lot of systems together, rare occurrences which don't show at a small scale that often, become ever-present. A system that goes down once a year is good enough, but when we have 1000 similar machines, the rate goes to roughly 3 failures per day.
Requirements of fault tolerant systems:
- <span style="color:#e1db3d">Availability</span>: How much of the infrastructure can fail and the system can still perform the intended task.
- <span style="color:#e1db3d">Recoverability</span>: After a repair, will it resume working the way it did ?
- <span style="color:#e1db3d">Efficiency with NV storage</span>: For recovering from failure, we need to save the state of the system before failure, on something that does not need power to function. Hence NV storage, but it is painfully slow, so we are better off using NV storage as less as possible.
- <span style="color:#e1db3d">Replication</span>: Management of replicated copies for recovering is quite difficult as well.
##### Consistency
When creating a distributed system, there is bound to be replication of data, so all copies of the data must have the same values. 
Consistency is defined in levels, for a key value store with two operations `put(key, value)` and `get(key)` , consistency can be defined as, getting the value of key "k" updated by the last put operation. This often is very hard to achieve, if such stringent consistency constraints are lifted, the system is much more easier to implement and performant.
Another challenge is placing the multiple copies in independent locations, both copies of a data item can not be on the same rack. The replicas must be as far as possible.
### Map Reduce
Originally designed at google (2004), the problem that they were was ranking pages, which even back then was a huge job. The problem essentially was sorting the data (which is severely downplaying it) but it took a long time, so they wanted to parallelize it for bringing down the runtime. Now hiring a ton of distributed systems specialists is not the way to go, as they might not be so good at other things. So, they wanted something that their average developer can do. Hence, they came up with the framework for distributed programming.
<span style="color:#e1db3d">Rough Overview</span>: The entire idea is to have two functions the <span style="color:#e1db3d">Map</span> and the <span style="color:#e1db3d">Reduce</span>, both having some job, apart from this there is a map helper and a reduce helper. You have the input split up into multiple independent parts, the 
