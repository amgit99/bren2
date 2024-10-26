![[dbland.png]]
> The limits of my language are the limits of my world

The above quote means the capabilities of an application are bounded by the data model that it chooses to represent its data. All systems around us are built on top of abstractions, layered architectures where APIs of these layers define what functionality they provide, everything is dictated by that API. It forms the model of data transfer between the layers and choosing the right one is essential.

These abstractions allow different groups of people for example, the engineers at the database vendor and the application developers using their database to work together effectively.

<span style="color:#e1db3d">Relational Model Vs Document Model</span>: The Relational model has stood the test of time for about 30 years now, which is huge for any tech trend, many came and many perished that challenged the Relational Model. The latest one in line would be the NoSQL databases, the need that seemed to fulfill was -
- Need for scalability for large datasets must be easier
- Wide spread free and open-source standard for database products
- Specialized Query operations that are not supported by traditional DBMSs
- More dynamic and expressive Data-Model
It turned out that both shall now co-exist providing their own strengths, this is called <span style="color:#e1db3d">polyglot persistence</span>
For applications where data like resume are stored. The contents have a document schema, so databases like MongoDB, RethinkDB, CouchDB, Espresso might be a perfect fit for the application. 
This is due to the high overhead of the ORM layer which is needed for the SQL databases. (Since the objects are not stored as they come, but in columns by their attributes, this operation is expensive and this condition is called <span style="color:#e1db3d">impedance mismatch</span>) JSON representation is good for locality, this means an entire resume entity is store in continuous memory and a fetch is easier as compared to the distributed attributes in tables in a relational database.

<span style="color:#e1db3d">Many-to-one and Many-to-Many relationships</span>: In the resume section the addresses should have system defined IDs, this means the user cant just input some arbitrary address. It should conform to some predefined set of addresses. This is tedious but a future proof design. Now the addresses values can be stored by their assigned indexes. Such practices also promote consistent style and spelling across all users, avoids ambiguity and makes updating and search much easier.

Using an ID is a great practice, when you use an ID, the information that is meaningful to humans (such as the word Philanthropy) is stored in only one place, and everything that refers to it uses an ID (which only has meaning within the database). This has a lot of advantages, updating is easy, we need to just change one copy. 

Such O2M and M2M relationships are better off modeled by relational databases, this is in context to database *normalization*. This is because joins are easy in relational databases data can be retrieved quickly. Even if the initial version of an application fits well in a join-free document model, data has a tendency of becoming more interconnected as features are added to applications. Even if we decide to denormalize the issue of consistency comes up.

The more the data is connected, the more the document model becomes awkward. With large number of connections redundancy becomes a factor and normalization of the relational model becomes useful. Another analogy is that relational models are like static, compile time type checking and the document models with no predefined schema are like dynamic runtime type checking. Both have their merits.

It’s worth pointing out that the idea of grouping related data together for locality is not limited to the document model. For example, Google’s Spanner database offers the same locality properties in a relational data model, by allowing the schema to declare that a table’s rows should be interleaved (nested) within a parent table. Oracle allows the same, using a feature called multi-table index cluster tables. The column-family concept in the Bigtable data model (used in Cassandra and HBase) has a similar purpose of managing locality.

All modern relational databases have added JSON document support. A hybrid route is optimal route to take in the future.