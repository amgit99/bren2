### Chapter 1: Reliability, Scalability and Maintainability
A data intensive application is usually built from a set of the following building blocks which each address a problem:
- Store data so that they, or another application, can find it again later (databases)
- Remember the result of an expensive operation, to speed up reads (caches)
- Allow users to search data by keyword or filter it in various ways (search indexes)
- Send a message to another process, to be handled asynchronously (stream processing)
- Periodically crunch a large amount of accumulated data (batch processing)
Applications for this are well abstracted and well made so that developers may not bother to rewrite data storage applications themselves or any other above mentioned technology for that matter. But its not that simple, different use-cases demand different designs of these systems and working with more than one could be tricky for interoperability.
##### Reliability : :
The things that can go wrong are called <span style="color:#e1db3d">faults</span>, and systems that anticipate faults and can cope with them are called<span style="color:#e1db3d"> fault-tolerant</span> or <span style="color:#e1db3d">resilient</span>. A fault is a component of the system not acting according to its spec, while a failure is the entire system going down and not producing any results.
Counterintuitively, in such fault-tolerant systems, it can make sense to increase the rate of faults by triggering them deliberately, (for example, by randomly killing individual processes without warning). Many critical bugs are actually due to poor error handling, by <span style="color:#e1db3d">deliberately inducing faults, you ensure that the fault-tolerance machinery is continually exercised and tested</span>, which can increase your confidence that faults will be handled correctly when they occur naturally. <span style="color:#984bd2">The Netflix Chaos Monkey</span> is an example of this approach.
The faults are roughly classified into three categories, Hardware, Software and Human.
Hardware failure means, disk/RAM/power failure, backups for all of these should be in place with minimal downtime.
Software failure is refers to the bugs in the code where every possible case for the input is not accounted for. Eventually such things emerge and cause failure, better coding practices and extensive testing is the solution. Human errors are ones where incorrect values are put in fields which expose software bugs, remedy is to train people better.
##### Scalability:
It is the term we use to describe a system’s ability to cope with increased load.
Describing the load: It may be requests per second to a web server, the ratio of reads to writes in a database, the number of simultaneously active users in a chat room, the hit rate on a cache.

<span style="color:#e1db3d">The Twitter Example</span>: Twitter faces over 4.6k requests/sec on average, over 12k requests/sec at peak. With that being said the first approach is to have a global storage of all the tweets and to update everyones feed is to look up all the followers of a person and gather all the tweets that are posted in a certain time frame. This is done using an expensive query.
The second approach is to have a per user cache, which holds the user's feed. When a user posts a tweet, the caches of all this followers is to be updated. This is much better than a single database running an expensive query.
But celebrities pose an issue for the second problem. The people that have millions of followers, their tweets can singlehandedly increase the load with this operation. So a hybrid combination of the first and the second approach is used.

 <span style="color:#e1db3d">Latency</span> and <span style="color:#e1db3d">response time</span> are often used synonymously, but they are not the same. The response time is what the client sees, besides the actual time to process the request (the service time), it includes network delays and queueing delays. Latency is the duration that a request is waiting to be handled, during which it is latent, awaiting service.
A common way to denote latency is using <span style="color:#e1db3d">percentile and threshold values</span>, there is a threshold set and the percentile value indicates for how many instances of the service met the threshold. 
A system is <span style="color:#e1db3d">elastic</span> if it can automatically add and shut down computing resources according to the need.

- Having stateless distributed systems is obviously easier logic wise than stateful systems, this makes vertical scaling more easier to implement cause stateful applications are easier to write.
- Distributed data systems will become the future, no matter what the type of load is or the application.
- Large scale applications will be specific, there is no one-fits-all architecture for all systems. ( a system that is designed to handle 100,000 requests per second, each 1 kB in size, looks very different from a system that is designed for 3 requests per minute, each 2 GB in size, even though the two systems have the same data throughput. )
##### Maintainability:
Maintainability has many facets, but in essence it’s about making life better for the engineering and operations teams who need to work with the system. Good abstrac‐ tions can help reduce complexity and make the system easier to modify and adapt for new use cases. Good operability means having good visibility into the system’s health, and having effective ways of managing it.

- Reducing complexity greatly improves the maintainability of software, and thus simplicity should be a key goal for the systems we build.
- A good abstraction can also be used for a wide range of different applications.

### Chapter 2: Data Models and Query Languages

![[Screenshot 2024-04-11 at 12.04.09 PM.png]]

> The limits of my language are the limits of my world

The above quote means the capabilities of an application are bounded by the data model that it chooses to represent its data. All systems around us are built on top of abstractions, layered architectures where APIs of these layers define what functionality they provide, everything is dictated by that API. It forms the model of data transfer between the layers and choosing the right one is essential.

<span style="color:#e1db3d">Relational Model Vs Document Model</span>: The Relational model has stood the test of time for about 30 years now, which is huge for any tech trend, many came and many perished that challenged the Relational Model. The latest one in line would be the NoSQL databases, the need that seemed to fulfill was -
- Need for scalability for large datasets must be easier
- Wide spread free and open-source standard for database products
- Specialized Query operations that are not supported by traditional DBMSs
- More dynamic and expressive Data-Model
It turned out that both shall now co-exist providing their own strengths, this is called *polyglot persistence*
For applications where data like resume are stored. The contents have a document schema, so databases like MongoDB, RethinkDB, CouchDB, Espresso might be a perfect fit for the application. This is due to the high overhead of the ORM layer which is needed for the SQL databases. JSON representation is good for locality, this means an entire resume entity is store in continuous memory and a fetch is easier as compared to the distributed attributes in tables in a relational database.

<span style="color:#e1db3d">Many-to-one and Many-to-Many relationships</span>: In the resume section the addresses should have system defined IDs, this means the user cant just input some arbitrary address. It should conform to some predefined set of addresses. This is tedious but a future proof design. Now the addresses values can be stored by their assigned indexes. Such practices also promote consistent style and spelling across all users, avoids ambiguity and makes updating and search much easier.
Such O2M and M2M relationships are better off modeled by relational databases, this is in context to database *normalization*. This is because joins are easy in relational databases data can be retrieved quickly. Even if the initial version of an application fits well in a join-free document model, data has a tendency of becoming more interconnected as features are added to applications. Even if we decide to denormalize the issue of consistency comes up.

It’s worth pointing out that the idea of grouping related data together for locality is not limited to the document model. For example, Google’s Spanner database offers the same locality properties in a relational data model, by allowing the schema to declare that a table’s rows should be interleaved (nested) within a parent table. Oracle allows the same, using a feature called multi-table index cluster tables. The column-family concept in the Bigtable data model (used in Cassandra and HBase) has a similar purpose of managing locality.