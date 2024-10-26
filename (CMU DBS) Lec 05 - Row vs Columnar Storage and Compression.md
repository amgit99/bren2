
| Property         | Value                   |
| ---------------- | ----------------------- |
| 📅 Date          | 22-10-2024, 18:11       |
| 🏷️ Tags         | #databases #compression |
| 🔗 Related Notes |                         |
Agenda:
- Database Workloads
- Storage Models
- Startree Talk
##### Database Workloads
<span style="color:#e1db3d">OLTP vs OLAP</span> : see [[Chapter 3 - Storage and Retrieval]]
##### Storage Models
A database's storage model has a big effect on what kind of queries it serves best. It defines how tuples are stored on disk and in memory. 
There are a few choices:
- <span style="color:#e1db3d">N-ary Storage Model (NSM)</span>: Also called a row-store, all the tuples stored in a contiguous locations, excellent for OLTP workloads as they are usually write heavy where new data (whole tuples) are inserted into the database. Any queries with * in the`SELECT` clause will benefit from such a storage model that access the entire table requires tons of useless data to be brought into memory. Downsides wrt OLAP are obvious.
- <span style="color:#e1db3d">Decomposition Storage Model (DSM)</span>: Also called a column-store, all the entries for a single attribute are stored contiguously in a single page. There is also a bitmap associated with every column, that acts as a flag for null values. Every column has its own set of pages. Now this scheme introduces the scope for smartly storing the column values, we can now perform <span style="color:#e1db3d">compression</span> as the data may have repeating patterns, being from the same domain. One of the solutions is <span style="color:#e1db3d">dictionary compression</span> (assigning unique numeric values to data types like strings and storing it in a separate table) this allows the entries to have a fixed size (32 or 64 bits).
- <span style="color:#e1db3d">Hybrid Storage Model (PAX Partition Attributes Across)</span>: As the name suggests, its a mix, <span style="color:#e1db3d">its columnar storage</span> but not the entire column is taken at once, <span style="color:#e1db3d">just a chunk (called a row group)</span>, all the columns that belong to this set of rows (row group) are stored one after the other. and then the process is repeated again for the remaining rows. We can perform compression on the internal row groups, this allows more tuples to be brought into memory when a page is loaded.
##### Compression
There are various schemes where certain queries (finding counts) are tried to <span style="color:#e1db3d">run on the compressed data</span> itself, and not on the decompressed original data.
Strategies like <span style="color:#e1db3d">run-length encoding</span> are very useful when we have sorted columns.
<span style="color:#e1db3d">Bitmap Encoding</span>, if we know things about the domain of the values, like age (0-110) in that case we don't need to save this as a 32 bit integer, and instead just just use 6 bits to store this data. To deal with cases where majority lies in a range and there are few outliers, we can have a separate table with offset to the entry and the outlier value.
Dictionary Compression is where we assign a number to values like strings, mostly names etc. With this in mind, we can assign the numbers based on the lexicographic ordering of the values, this allows us to convert string matching type of queries to range search queries, which can be run way faster if we also store some metadata like counts in the dictionary.

## StarTree founder talk - Intro to Apache Pinot
// bouncer // 

