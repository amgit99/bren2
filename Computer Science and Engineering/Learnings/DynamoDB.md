Its a NoSQL database, optimized for performance at scale.

It distributed data across several locations making it robust. Its a managed database, meaning the hardware allocation, security is taken care of by the database and doesn't need user intervention. It can scale out horizontally by assigning new nodes and distributing data on it. It has high availability and durability.

Ideal for application with known access patterns. Access is through APIs and ORMs, authorization is through [[IAM]].
It has great integration with other AWS services. Cost effective usage based payment model.

##### Tables, Items, Attributs, Indexes
Tables are collections of items. Items are collections of attributes or key-value pairs. Each item has a key and the value can be a complex object.
There is notion of Primary Key, it is a set of attributes that are globally unique. This key can be divided into sub-parts which are the Partition Key and Sort Key, both not necessarily unique.
There are indexes in DynamoDB, we can create Global Secondary Indexes, which helps us to reduce the number of accesses of the table, which in turn reduces the cost. This is an example where knowing the access patterns is useful.