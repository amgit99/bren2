
| Property         | Value              |
| ---------------- | ------------------ |
| 📅 Date          | 25-10-2024, 10:07 |
| 🏷️ Tags         |                    |
| 🔗 Related Notes |                    |
This lecture deals with how the data is moved back and forth between memory and disk.
The same logic applies, 
- <span style="color:#e1db3d">Spacial control</span>: The goal is to keep pages that are used together as physically close together as possible on disk.
- <span style="color:#e1db3d">Temporal Control</span>: This refers to keeping the pages that we need the most in memory as long as they are need and not evicting them by some naive policy that does not take the execution plan into consideration.
##### Buffer Pool Organization
<span style="color:#e1db3d">Frames</span>: These are the slots in the buffer pool of the memory. The slots are of the same size as that of the page. For databases that have multiple page sizes for various tables, there are multiple smaller buffer pools.
<span style="color:#e1db3d">Page Table</span>: This a table that keeps a track of what pages are present in the buffer pool and saves pointers to the pages. Additionally stores a dirty flag, access tracking information, pin/Latch/reference counter. Pin is the mechanism used to prevent it from eviction. Latch is what tells us that the page is begin modified so its pointer is not to bed shared to any thread of the execution engine, as one is currently working on it (like a lock, but db ppl call it a latch). The term "lock" is not used because at the database level there is also a concept of locks (there are several protocols, variants of 2PL for consistency) for protecting the logical contents form transactions. This is at the implementation level, there is a whole new layer of consistency issues here.
- Locks: For logical consistency, held for the duration of the transaction, must have functionality for rollback.
- Latches: Protects critical sections during execution of operations, held for the entire time an execution engine thread is running, rollback is not required.
<span style="color:#e1db3d">Page Directory vs Page Table</span>: 
- The page directory is the mapping from the page ids to the actual location on secondary storage, the page ids might be obtained from indexes. The page directory can not be completely stored in volatile storage, if its lost or corrupted, the entire contents of the database are lost.
- The page table is the mapping of the page id to its location in the buffer pool. It tracks the current state of the buffer pool (an in memory data structure) so it is entirely stored in memory.

##### Why never use `mmap` 
 Problems with giving the OS control over memory:
 - <span style="color:#e1db3d">Transaction Safety</span>: If a single transaction requires multiple changes to be made which span across multiple pages, after a set of changes begin done on a page the OS might flush it out randomly, but the remaining changes might fail and everything must be rolled back. This will incur a huge overhead to bring it back and figure out what changes persisted and revert it to the original value.
 - <span style="color:#e1db3d">I/O Stalls</span>: The DBMS does not know what is present in the memory, as we can not look at the OS page table, The OS will stall a thread on a page fault.
 - <span style="color:#e1db3d">Error Handling</span>: The places that could break down are now much more in number than if a tiny chunk is managed by the database system.
 - <span style="color:#e1db3d">Performance Issues</span>: OS data structure contention, TLB shoot-downs.
 
 Where `mmap` may be good is places where there is read heavy workload.

Solution #1
- `madvice` tell OS how you expect to read certain pages.
- `mlock` tell the OS that memory ranges can not be paged out.
- `msync` Tell the OS to flush memory ranges out to disk.