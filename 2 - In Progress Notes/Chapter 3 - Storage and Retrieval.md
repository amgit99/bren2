
| Property         | Value              |
| ---------------- | ------------------ |
| 📅 Date          | 13-09-2024, 11:18 |
| 🏷️ Tags         |                    |
| 🔗 Related Notes |                    |
![[ddia_chapter3_map.png]]
The previous chapter dealt with the format of data that the database required and the methods to retrieve it. This chapter deals with the how the data is actually stored in the database. This is essential because knowing the general working of the internals can aid in further increasing the performance of the application. This is because databases provides several options to the user to change the internals according to the application, like indexing in the relational databases.

##### Indexing
It is the practice of having an additional structure that is almost always smaller in terms of size and is derived from the original data, so for every record we have a smaller derived record that resides in the index and points to the actual record. This is useful when there are searches performed based on the that derived record, as the search space is reduced due to the smaller size.
This is where the application programmer's input comes into play, he can make indexes based on the observed query patterns.

// ==add from CMU course and Navhate.==
#### Transaction Processing or Analytics
In the earlier days making a write to the database usually meant that there was some transaction that took place, some kind of reservation or a purchase. The applications changed but the name "transaction" stuck around. 

*A transaction needn’t necessarily have ACID (atomicity, consistency, isolation, and durability) properties. Transaction processing just means allowing clients to make low-latency reads and writes as opposed to batch processing jobs, which only run periodically (for example, once per day).*

For modern applications the databases rely on indexes for fetching and updating of records, the request for such type of access patters has gone up and they are given the name Online Transaction Processing (OLTP).
As Databases grew, people recognized the value of the data gathered from the users. Business Analysts started using this data for business intelligence, this did not require a single record to be fetched or updated but the values of a single or subset of attributes for the entirety of the data. This type of access pattern was named Online Analytical Processing (OLAP). 

![[oltp_vs_olap.png]]
There is a third category HTAP, this is a hybrid between these two type of access patterns.
##### Data Warehousing
There are various transaction processing systems that an enterprise needs, these need to be live, available and performant at all times, having analysis done on such systems is not feasible. Hence, we have data warehouses, specialized storages for analysis.
Its a periodically updated, read only, snap shot of the OLTP database. The process of moving the newly added data from the OLTP database to the warehouse is called <span style="color:#e1db3d">ELT(extract-transform-load)</span>. The warehouses need their own optimizations and the strategies that are good for the OLTP counterparts, don't work well with OLAP demands, Hence there are different engines that power such warehouses.
The core engines that power OLAP and OLTP databases are gradually diverging as they become better, since the basic requirements are different. This is leading to the vendors not including both functionality in their database products.
##### Stars and Snowflakes: Schema for Analytics
The enterprises use a star/snowflake model, the central entity is called the fact table, it is a table of events that the all the customers perform, like going to a page, scrolling, purchasing something, all of these qualify as events and are stored sequentially. This gives most flexibility for analysis. The columns in this fact table are foreign key references to the dimension tables, these store information of the specific attributes, where the attributes could represent who, what, where, when, how, and why of the event.
The fact tables can have several hundreds of columns, that each have large dimension tables, these have large amounts of metadata themselves. 
When a query is run for gathering information, it is usually for a large subset of records and involve not more than five attributes. This means an entire horizontal record being fetched for just 4-5 attributes is wasteful, this is magnified as a bunch of recored are needed, so we resort to columnar storage.
The values in the columns are fairly repetitive, the number of distinct values are way lower than the number of entries, sophisticated compression techniques are used to save space. Eg: *bitmap encoding* 