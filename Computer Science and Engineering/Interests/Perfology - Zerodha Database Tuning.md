##### A Postgres lifestyle @ Zerodha
Having gone through a lot of databases, they have yet to come across something as resilient as Postgres.
Postgres well documents what changes they are going to make, great community.

##### Background for a Stock Broker
A software for people to trade, this mean dealing with financial data, ledgers/PL reports etc.
The systems at zerodha are read only through out the day and become write only at night. This is taking into consideration the fact that the markets are active between 9:15 and 15:30.
The trading platform Kite has a transactional DB, this one is read-write throughout the day, but the console platform mentioned above that consolidates all the trades of the day, is read-only in the morning and becomes write only at night. This is the context behind all the optimizations and tweaks that the people at zerodha do to get the most out of postgres.

While scaling up, the older schemas will be rendered useless, things need to be re-written, things don't work out.

Zerodha does not have a master-slave, replica architecture. They have one database, they have <span style="color:#7030a0">sharded it using foreign database wrapper</span> the data is split on the basis of time periods. Their back up is in S3 archives.
Do not keep unnecessary data in the database, keep it in bucket storage.

##### How to manage Big(?) Data
- <span style="color:#e1db3d">Index but don't overdo it</span>: Indexing is not always a solution, indexes are heavy on memory so indexing for every type of query is not optimal, some may need it some may not. Processes that are not user-facing, run in the background might not need indexing, partial indexes are good too! somehow help in <span style="color:#7030a0">categorizing ??? </span>
- <span style="color:#e1db3d">Materializer Views FTW</span>: Normalization is good only till the data is small, the join is a very taxing operation. The tradeoff of space vs query execution time must be considered. Materialized views act as cashes and using them everywhere is good.
- <span style="color:#e1db3d">Understanding Data Better</span>: Choose the db according to the data, no a fancy new db and force fit the data into the database.
- <span style="color:#e1db3d">Understanding the query planner</span>: Understanding the queries where multiple joins are used, also the order and sorting are important. This can be helpful to decide where the indexes can go in the most efficient way. Understanding the query planner is important to all optimizations.
- <span style="color:#e1db3d">Regular vacuuming</span>: <span style="color:#7030a0">Auto-vacuuming ???</span> is not optimal (compaction of table after there are spaces left behind after deleting records@).
##### Postgres as a caching layer
Since there are no replicas, there is no load-balancing. There is a second postgres server that sits on top of the main DB and acts like a caching layer.
<span style="color:#e1db3d">The Kite Platform</span>: It uses Postgres for the market watch(seeing current prices of the selected stocks) they plan on moving to ScyllaDB. The console and the Kite DB have very little to do with each other. The data comes to the console in terms of a zip from the exchange. Kite has the requirements of being extremely fast at read and write, 100% uptime (lol), and this works throughout the day. The cache for Kite is Redis, Redis is actually used everywhere, except for the console (since there is no pagination and sorting)

##### Sharding
Generally there is a master-slave architecture...the one like in VPN master servers directing to the authoritative servers. Zerodha believes there is no reason for the extra information used at the top level nodes to direct the query to the correct shard. They instead split on the basis of months, this is connected by an <span style="color:#7030a0">fdb wrapper</span>, but this might be falling apart, so they will be moving on to better schemes.
Some give aways for bugs and something going wrong is by setting hard limits on query execution times, a certain type of query must not exceed a given amount of time. This tells us that something is wrong.

##### Questions
Postgres saves data in a directory. Does it come across file system limitations, Postgres has never run into such issues.
All of their apps are sitting on top of caching layers, not on a db, its not fair to expect an app to scale on top of terabytes of db.
Everything should hit the caching layer, and everything should be <span style="color:#e1db3d">async</span>.