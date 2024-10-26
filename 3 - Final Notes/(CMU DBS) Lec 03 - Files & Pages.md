
| Property         | Value                             |
| ---------------- | --------------------------------- |
| 📅 Date          | 15-10-2024, 11:06                 |
| 🏷️ Tags         | #databases                        |
| 🔗 Related Notes | [[(CMU DBS) Lec 02 - Modern SQL]] |
<span style="color:#e1db3d">Summary</span>: How everything is stored on the disk. How individual bits are stored on the disk. How they are brought into memory for processing. 
When operating at this level we must take into consideration the speeds, addressability and nature (persistence) of memory, the famous hierarchy of memory comes into play. 
Persistence is important (duh) so almost all of the storage will be on disks, disks are horrible at random reads and writes. So we need to <span style="color:#e1db3d">maximize sequential reads</span> over random access ones. So algorithms must try to reduce the random writes.

<span style="color:#e1db3d">Disk Level</span>: The operating system is the API to the secondary storage, those are the disks. This can sometimes cause impedance, so having full control over a chunk of memory (RAM) that belongs to the process that is the dbms, without the OS begin involved in paging and virtual memory management is much more efficient.
On the secondary storage, various DBs store the contents differently, there can either be one single OS file (inside of which the db knows the offsets to make sense of the data) or multiple OS files across multiple directories. The database follows a unified unit of data, called a page, the size is fixed for any sort of information that is stored on disk, weather it is related to the dbms (config files or data structures of the dbms) or the actual data of the user (the one in the tables/indexes to tables). Any data transfer from the disk to the memory happens in chunks of size equal to the page size.

<span style="color:#e1db3d">Memory Level</span>: At this level, we have a space called the <span style="color:#e1db3d">buffer pool</span>. This is usually some multiple of the previously mentioned page size. Chunks of data of said page size are brought into memory to be operated on. Now all of this sounds awfully lot like the paging supported by the operating system, but DBs still take the trouble to re-create the functionality themselves. This is for more fine grained control over the memory by getting to modify the page size, and eviction policies for this buffer bool (essentially a cache) based on the application requirements and the query planner. It might be the case that the average page eviction strategy might greedily remove a page from memory but with the input from the query planner, one can make a more informed decision about the future of a page ( to evict or not to evict ).

###### Problem 1: How the DBMS represents the database in files on disk
There are one or more files on disk, sqlite is a single file db, while others are spread across several files, and each of them have their own proprietary format. The data files of one database can usually not be used with other databases. Although, databases like DuckDB are offering the functionality. There has been a rise in open source data formats like parquet etc. that various databases are becoming compliant to. 
Sometimes very high-end database systems (like Oracle's ASM) even ignore the underlying file system and provide its own file system and volume management.

<span style="color:#e1db3d">Pages</span>: Its a fixed size block of data, it can contain tuples, meta-data, indexes, log-records, they don't mix types (half of the page for tuples and the rest for indexes...), some systems have self contained pages, almost all the context required to make sense of the the data on the page is on the page itself (this is good for disaster recovery).
The OS files that the db uses to store information are logically broken down into pages, by using the page size, we can jump to offsets in the file.
There is table that holds the description of the page (page id) and its physical address. This acts as an indirection, when we need to modify the description of the page, it can be done at one place.

Different Notions of Pages:
- <span style="color:#e1db3d">Hardware Page</span>: This is at the lowest level, this is what the hardware exposes to you, the <span style="color:#e1db3d">largest size of an atomic write</span> that it can do. Usually 4KB.
- <span style="color:#e1db3d">OS pages</span>: This is the paging scheme we all know so well, provided on top of the hardware by the OS, this enables virtual memory, the page size is usually same as the hardware page size, i.e. 4KB.
- <span style="color:#e1db3d">Database Pages</span>: This is built on top of the OS pages. Some database systems allow different page sizes for different tables.

Page Structure Architecture:
- <span style="color:#e1db3d">Heap File Organization</span>: A heap file is an unordered collection of pages, with the tuples stored in a random order, the storage manager provides create/get/write/delete functionality for pages. There is metadata to track file locations and free space availability. Theres a page directory (a hash table) that keeps track of all the database objects, these are the tables, indexes, basically it has the information of every page that is a part of the db and whats on it. It itself is stored as pages, the location of which is not random and updating is done with caution.
- Tree File Organization
- Sequential / Sorted File Organization (ISAM)
- Hashing File Organization
###### Page Layout
![[db_page_design.png]]
Theres a naive way of arranging tuples in a page, apart from the header we can just randomly place them in contiguous memory locations. Adding new entries will require us to know how many tuples are present so that we can jump that offset and store the newer one there. But this fall apart when deletes are involved, there will be gaps that are left behind which need a full scan to find (for compaction), other than that we need some sort of an index to find individual tuples efficiently. Also when compaction happens and tuples are moved, their offsets in the page changes, which will mess up all the indexes that store the page id and the page offsets of the tuples, and all of that will need updating.

<span style="color:#e1db3d">Slotted Pages</span>: We have a slot array at the start of the page and the tuples arranged in reverse from the bottom of the page, both grow towards each other. Whatever indexing scheme is used now has to store the <span style="color:#e1db3d">page id and the slot array index</span>. The Slot array simply has the offset of the tuple it is supposed to hold information about. <span style="color:#e1db3d">The indexes of the slot array in no way correspond to the offsets of the tuples</span>. This designs solves the two problems with the naive approach, it allows us to scan a tiny amount of data to locate free spots once tuples are removed from the page to add newer ones. And when compaction is required, the tuples can be moved around in the page and its offsets in the slot array can be updated, this means the indexes and other data structures need not be modified.
![[slotted_db_page.png]]
##### NEON database:
Neon is a storage system, it intercepts the page read write requests to the operating system and Neon fetches it.