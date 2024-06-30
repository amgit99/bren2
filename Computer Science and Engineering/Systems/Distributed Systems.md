# MIT 8.824
### Lecture 1: Introduction
Never go distributed when one can solve the problem on a single system. Given that, one can go for distributed systems for the following reasons:
- Parallellism
- Fault Tolerance
- Physical Locality ( geographically far away )
- Security / Isolation
Challenges:
- Concurrency
- Partial Failure
- Performance ( 100 machines doesn't mean 100x speed-up )
- Underlying network failure occurs in strange patterns.

Infrastructure that comes up a lot when defining the Distributed Systems are Data Storage Systems, Communication Systems, Computation Systems. Ideally we would like these systems to have such sophisticated abstractions, that would mean that they have the same interface as that of their non-distributed versions, this is sadly very rare.

The topics that come up while building Distributed Systems are:
- <span style="color:#e1db3d">Implementation of such systems</span> - This involves study of RPC(remote procedure calls), threads, concurrency control.
- <span style="color:#e1db3d">Performance</span> - This involves the study of scalability, 2x systems = 2x throughput
- <span style="color:#e1db3d">Fault Tolerance</span> - Failures that are rare, now become very frequent and constant.(Availability, Recoverability, Replication, NV storage)
- <span style="color:#e1db3d">Consistency</span> - Same data across all copies, there are classes of consistency, strong and weak. Strong means the readers are guaranteed the latest written copy. Enforcing this is very expensive.

##### MapReduce
This was developed at Google to perform computing on giant amounts of data, like creating indexes on the contents of the web