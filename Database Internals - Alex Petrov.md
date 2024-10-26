## Part 1 : Storage Engines
The primary job of database management systems (the term DBMS, databases, database systems are used interchangeably) is to store user data of applications reliably and make it available when needed.
Databases are modular systems, <span style="color:#e1db3d">a transport layer</span> for accepting requests, <span style="color:#e1db3d">a query planner</span> for figuring out the best way to execute a query, <span style="color:#e1db3d">an execution engine</span> that actually does the execution of the query, and <span style="color:#e1db3d">a storage engine</span>, that manages how the data is stored on NV storage and brought into memory for processing.
The storage engine offers a simple API for data manipulation, what ever complex query is requested, it's effect can be replicated with basic operations like *create*, *update*, *delete* and *retrieve*. So we can view the other modules working on top of the storage engine which provides an API to the data of the database. The storage engines also offer a <span style="color:#e1db3d">schema</span>, <span style="color:#e1db3d">query language</span>, <span style="color:#e1db3d">indexing</span>, <span style="color:#e1db3d">transactions</span>, etc.
The modular nature allows various databases to borrow the storage engine component from a good open source database and focus on the other modules that they want to implement differently.

Choosing a right DB is critical, this becomes apparent later when a migration is required, leading to massive amounts of application code changes. The storage engine has a huge influence on the performance of an application, so knowing what is going on the inside is crucial to know how it serves out application needs.
<span style="color:#e1db3d">Important Application Variables</span>:
- Schema and Record Sizes.
- Number of Clients.
- Types of queries and access patterns.
- Rates of the read and write queries.
- Expected changes in any of these variables.
<span style="color:#e1db3d">Important questions to ask the database</span>:
- Does the database support the required queries ?
- Can it handle the given amount of data ?
- How many read and write operations can a single node handle ?
- How many nodes are needed ?
- How do we expand the cluster given the expected growth rate ?
- How does maintenance looks like ?