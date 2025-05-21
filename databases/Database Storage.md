
### DBMS Structure

![[Pasted image 20250209023940.png]]

### Storage types

##### volatile and Non-volatile storage 
- volatile 
	- need power all the time  , often optimized for random access
	- read per byte
- Non-volatile 
	- persistent storage  , often optimized for sequential access
	- read blocks or pages

![[Pasted image 20250209024221.png]]

![[Pasted image 20250209024310.png]]

### Design Goals

- Manage data that exceed the available memory
- Minimize reading / writing to disk (expensive)
- When accessing data on disk , maximize sequential access


### Files and pages

In database systems, **files** and **pages** are fundamental concepts that define how data is physically stored and retrieved on disk.

##### 1. Files in a Database

A **file** in a database refers to a collection of pages that store data, indexes, or metadata. The database engine manages files to efficiently read, write, and organize data , and it uses System-specific file format that OS can't understand the contents and it can use just a single file or multiple files

- **Types of Database Files**:
    1. **Data Files** – Store actual table data.
    2. **Index Files** – Store B-trees, hash indexes, or other indexing structures.
    3. **Log Files** – Store transaction logs for recovery purposes.
    4. **Temporary Files** – Used for sorting and intermediate computations.

For example, in MySQL:

- InnoDB stores data in `.ibd` files.
- MyISAM stores data in `.MYD` files and indexes in `.MYI` files.

In PostgreSQL:

- Tables are stored in separate file segments in the `/data/base/` directory.

##### 2. Pages in a Database

A **page** is the smallest unit of storage in a database file. When a database needs to read or write data, it operates on pages rather than individual rows , each page has it's unique id and DBMS can map a page id to physical location.

- **Typical Page Size**: Common sizes are **4 KB, 8 KB, 16 KB**, or even larger, depending on the DBMS.
- **Types of Pages**:
    1. **Data Pages** – Store actual table rows.
    2. **Index Pages** – Store B-tree or hash-based indexes.
    3. **Transaction Log Pages** – Store changes before committing to ensure durability (ACID).
    4. **Free Space Pages** – Track unused space.
    5. **LOB (Large Object) Pages** – Store large objects like images or long text.

For example, in **PostgreSQL**, each table has a set of **8 KB pages**, and rows are stored within these pages.

##### Relationship Between Files and Pages

- A **database file** consists of **multiple pages**.
- When a query requests data, the **DBMS fetches entire pages** from disk into memory (buffer pool).
- If a record is updated, the entire page is marked **dirty** and written back to disk.
- Indexes also use pages to speed up searches.

##### Why Pages Matter?

- **Performance**: Databases read/write pages in bulk, reducing disk I/O overhead.
- **Caching**: Frequently accessed pages are kept in memory (buffer pool).
- **Compression**: Pages can be compressed to save space.
- **Concurrency Control**: Pages are locked for consistent reads and writes.


### File Organization

##### Types
- Heap File Organization
- Tree File Organization  , used in Indexing (range queries)
- Sequential/Sorted File Organization (ISAM) , used in Batch Processing , Tape storage
- Hashing File Organization , used in Indexing (equality searches)
##### Heap File Organization in Databases

A **Heap File Organization** is a simple storage mechanism where records are stored in **no particular order** within data pages. This is the default storage mechanism in many database systems, especially when no clustering or indexing is applied.

---

##### 1. How Heap File Organization Works

- Records (rows) are inserted **wherever free space is available** in the data pages.
- There is **no predefined order** for how records are stored.
- A **page directory** or a **free space tracker** helps locate available space for new records.
- When a record is deleted, its space is marked as **free** but not immediately reclaimed.

---

##### 1. Structure of a Heap File

A heap file consists of **multiple pages**, and each page contains records. Here's how the structure typically looks:

2. **Heap File** (Collection of Pages)
    - **Page 1**
        - Record 1
        - Record 2
    - **Page 2**
        - Record 3
        - Free space
    - **Page 3**
        - Record 4
        - Record 5

- Each record is assigned a **Record ID (RID)**, which consists of:
    - **Page Number** (where the record is stored)
    - **Slot Number** (position within the page)

---

##### 3. Operations on Heap Files

###### (a) Insert Operation

- The DBMS finds a page with free space (or allocates a new page).
- The new record is placed in any available slot.
- The system updates the page directory to reflect changes.

###### (b) Search Operation

- Since records are unordered, searching requires a **full table scan** (O(n) complexity).
- If an **index** exists, searching becomes faster.

###### (c) Update Operation

- If the updated record size **fits within the page**, it is updated in place.
- If it **exceeds the available space**, it may be moved to a new page, and a **forwarding pointer** is left in the original location.

###### (d) Delete Operation

- The record slot is marked as **free**, but the space is not reclaimed immediately.
- If too many records are deleted, the page may be reorganized.

---

##### 4. Advantages of Heap File Organization

✅ **Fast Insertions** – New records can be added anywhere there is space.  
✅ **Simple Management** – No need to maintain order.  
✅ **Efficient for Small Tables** – If a table has few records, full scans are not costly.

---

##### 5. Disadvantages of Heap File Organization

❌ **Slow Searches** – Without an index, searching requires a full table scan.  
❌ **Fragmentation** – Deleted records leave gaps that lead to wasted space.  
❌ **Poor Sequential Access** – Since records are unordered, range queries are inefficient.

---

##### 6. Where is Heap File Organization Used?

- **Temporary Tables**: When order is unnecessary.
- **Logging Systems**: Where inserts are frequent.
- **Small Tables**: Where full scans are not expensive.
- **Databases Without Indexing**: Heap files work well when no indexes are required.

---

##### 7. Heap vs. Other File Organizations

|**Feature**|**Heap File**|**Sorted File**|**Hash File**|
|---|---|---|---|
|**Insertion**|Fast (O(1))|Slow (O(n))|Fast (O(1))|
|**Search**|Slow (O(n))|Fast (O(log n))|Fast (O(1))|
|**Sorting Needed?**|No|Yes|No|
|**Best Use Case**|Small tables, logs|Range queries, sorted data|Key-based lookups|

---

##### 8. Optimizing Heap Files

- **Use Indexes** to speed up searches.
- **Vacuuming/Reorganization** to reclaim space from deleted records.
- **Partitioning** to divide large heap tables into smaller, manageable chunks.

----------
## Database Storage
![[Pasted image 20250224192716.png]]
### pages structures
#### Slotted pages
Header maintains
- number of slots
- offset of last used slot
Each slot correspond to tuple
Tuples are added starting from the end of the page
![[Pasted image 20250224184759.png]]

Each one of the pointers up left point to the tuple start and when you delete tuple it looks like this
![[Pasted image 20250224184954.png]]

we also can optimize the performance by periodically after a time we reorder the pages to get rid of the internal fragmentation  

##### Record ID
- Unique for every tuple
- Represents physical location of tuple
	- some encoding of (file id , page id , slot number)
	- internal index
- Usually not stored with the tuple
![[Pasted image 20250224185717.png]]
for this example Record id will give (file id , page id , slot number) so you can access it even when the structure is complicated like that.

Insert new tuple: Read page directory , find page with free slots , Read data page , find empty space in page and add tuple.

update existing tuple: Read page directory , find page , read data page , check slot array and find tuple , check if data fits if yes override tuple otherwise delete tuple and insert new tuple with the new value

##### Challenges
- Disk I/O
	- 2 pages for insert
	- 2-4 pages for an update
- Random Disk I / O
	- update multiple tuples in separate pages
- Fragmentation
	- unused tuple and slot spaces
---------------
#### Log-Structured Storage
- Also known as Log-structure merge trees (LSM trees)
- Record changes rather than storing the tuples in pages
	- Each entry: SET or DELETE operation on a tuple
	- Must include the tuple id

![[Pasted image 20250224190906.png]]
this system used in (cassandra , RocksDB , Apache HBASE)

once the page filled we write it to disk

Read tuple with given id
- find newest entry with that id
- scan log from end to beginning 
![[Pasted image 20250224191328.png]]
- Scanning is inefficient so we can use index
![[Pasted image 20250224191406.png]]

We can optimize by using compaction
![[Pasted image 20250224191615.png]]

--------------------
#### Index-Organized Tables (IOT)
- Store the actual data inside an index (primary key)
- faster reads
- slower writes
- Reduced storage
used in (SQL-Server , Oracle , MySQL , ....)
------------
#### Tuples Storage
- A sequence of bytes
- Header + Data
- interpreted by the DBMS using schema information
![[Pasted image 20250224192923.png]]

How the System interpret tuple like this
![[Pasted image 20250224193404.png]]

attributes that need more than 64bits we store it in different page and in it's location we store pointer to this page