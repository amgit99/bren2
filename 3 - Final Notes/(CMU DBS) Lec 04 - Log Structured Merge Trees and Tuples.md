
| Property         | Value                                |
| ---------------- | ------------------------------------ |
| 📅 Date          | 17-10-2024, 14:15                    |
| 🏷️ Tags         | #databases                           |
| 🔗 Related Notes | [[(CMU DBS) Lec 03 - Files & Pages]] |
<span style="color:#e1db3d">Tuple Oriented Storage</span>: Given the previous slotted page architecture, we now see how adding new tuples and retrieval works,
- <span style="color:#e1db3d">Inserting a new tuple</span>: We have the page directory, a table that holds the values of all pages, and how much they are filled. Using this we find the first page with free space, use the page id to get the page in memory (if its not) from secondary storage. Then we traverse the slot array to find the next free slot and add the tuple there, later all indexing data structures must be updated with this *(page id, slot id)* combination.
- <span style="color:#e1db3d">Updating an existing tuple</span>: We might be having the *(page id, slot id)* pair for the tuple that need updating, use this to fetch the correct page, an use the slot id to get the offset to the piece of data we are looking for. If tuples are of variable data, and the newer data doesn't fit, delete this and find a new place in the same page or a different one (here an update to the indexing data structures is required).

<span style="color:#e1db3d">Problems</span>:
- <span style="color:#e1db3d">Fragmentation</span>: Pages are not fully utilized, there is the space-time tradeoff between space utilization and time taken for compaction.
- <span style="color:#e1db3d">Useless Disk I/O</span>: For updating a single tuple, an entire page must be brought into memory, wasteful much!
- <span style="color:#e1db3d">Random Disk I/O</span>: Worst case scenario is when multiple tuples end up fetching multiple pages, as they are located on different pages.

Weird idea : What if the DBMS cannot overwrite data in pages? and only create new pages, may cloud dbs do this (HDFS, Google Colossus, S3 sorta).

##### Log Structured Merge Trees
The key idea here is, there can only be additions, we can only append to this data structure, and this causes the writes to be faster, but the read speeds might take a hit.
When we mean just appends, the changes are stored as logs, and to get the final value of an item we can just rerun all the logs (sounds inefficient but more on that later) . There is a tree style data structure in memory, once it gets filled the data is compacted and moved to the SS Table (secondary storage table) and this is arranged in a hierarchical sorted manner as well. 
see more: [[Log Structured Storage]] 
##### Index Organized Storage
Till now we have seen indexes as a black box, where we give the desired *key* to this black box and get back the *record id*, this is later used in the <span style="color:#e1db3d">summary table</span> of log structured storages system or the <span style="color:#e1db3d">page directory</span> of a tuple oriented storage system. Now instead of having this black box that gives us the *record id* of the data, and us retrieving it. We can merge these steps closer by having a common data structure that manages the indexing capabilities and also stores the final records.
see more: [[B and B+ trees]]
##### What is a Tuple
A tuple is a set of values, corresponding to a row in some table (too logical, blech). Its a bunch of bits, corresponding to the data type defined at the time of creation of the table, in contiguous locations. As a tuple can be of arbitrary length, when we consider word boundaries in memory, we prefer that every tuple starts a new word boundary, to make fetching easier, if a tuples spans multiple words, we try to pack data in such a way that even individual attributes don't overlap the word boundary.
The data types for ints, floats we have the same scheme for storage, databases offer decimals which do not have the precision errors of floats and doubles. Their size is dependent on the DB. Varchar, Text, Blobs ... depending on the size of these things they might have a pointer to a different page, timestamps are in milliseconds (UNIX time) is 32 or 64bit in size.