Indexes in databases are specialized data structures that improve the speed of data retrieval operations on a table, much like an index in a book helps you quickly locate information without reading every page.

### What Are Database Indexes?

- **Definition:** An index is a data structure (commonly a B-tree, hash table, or other structure) that stores a sorted subset of data from one or more columns of a table, along with pointers to the actual rows in the table.
- **Analogy:** Think of it as a table of contents or an index in a book, which allows you to quickly jump to the section where your data resides without scanning every page (or row).

### How Do Indexes Work?

- **Data Organization:** When an index is created on a column, the database organizes the column’s values in a way that makes searching faster. For instance, in a B-tree index, data is sorted in a tree structure that allows logarithmic time complexity for searches.
- **Pointer System:** Instead of storing the full row data, the index holds pointers (or references) to the rows in the main table. This way, once the relevant index entry is found, the database can quickly locate and retrieve the complete row.

### Types of Indexes

- **Clustered Indexes:**
    - **Definition:** A clustered index determines the physical order of data in the table. Each table can have only one clustered index because the data rows themselves can be sorted in only one order.
    - **Use Case:** Often used for primary keys.
- **Non-Clustered Indexes:**
    - **Definition:** These are separate from the data rows. A non-clustered index contains pointers to the data rather than reordering the physical data in the table.
    - **Use Case:** Useful when you need quick lookups on columns that are frequently searched but aren’t necessarily the primary key.
- **Unique Indexes:**
    - **Purpose:** Enforces uniqueness of the values in the indexed column(s), ensuring that no duplicate values exist.
- **Composite Indexes:**
    - **Definition:** An index on multiple columns that can help speed up queries involving several columns in the WHERE clause.
- **Full-Text Indexes:**
    - **Purpose:** Optimized for searching text-based data by breaking down text into tokens and allowing fast search operations for keywords.
- Converging index
	- it is not a type but it is a case when you get the query answer by only scanning the index , no need to check the physical data

### Benefits and Trade-Offs

- **Performance Gains:**
    - **Query Speed:** Dramatically reduces the time taken to locate data, especially in large tables.
    - **Efficiency:** Minimizes the need for full table scans, which can be costly in terms of time and resources.
- **Maintenance Costs:**
    - **Storage Overhead:** Indexes require additional disk space.
    - **Impact on Write Operations:** Every time data is inserted, updated, or deleted, the corresponding indexes must also be updated, which can slow down these operations.

### Practical Considerations

- **Indexing Strategy:**
    - **Selective Columns:** It's best to index columns that are frequently used in search conditions (WHERE clauses) or in joins.
    - **Balance:** Too many indexes can degrade performance on write operations, so it’s essential to strike a balance.
- **Monitoring and Tuning:**
    - Regularly monitor query performance and adjust indexes as needed based on usage patterns and data changes.

Indexes are a powerful tool in a database designer’s toolkit, dramatically enhancing read performance when used appropriately. However, they require careful planning and management to ensure that the benefits outweigh the overhead they introduce.

(conceptual view)
![[Pasted image 20250226194132.png]]

##### B-tree
- sorted 
- Each node can have up to k children (and k - 1 keys)
- balanced
- every node (other than the root) must be at least half full
 
![[Pasted image 20250226235943.png]]

##### B+ tree
- same as B-tree with extra features
- Data pointers in the leaf nodes only
- Leaf nodes link to each other (possibly in both directions)

![[Pasted image 20250227000857.png]]
##### hash index
consists of 2 main components hash table , hash function
![[Pasted image 20250227002452.png]]
##### composite indexes
index on attributes (a1 , a2 , ... , an) can be used to answer any prefix search 
![[Pasted image 20250227012106.png]]

----------------

##### Primary key Vs Row id

 What is a Primary Key?

- **Definition:**  
    - A primary key is a column—or a set of columns—that you explicitly define in your table schema to uniquely identify each record. It must contain unique, non-null values.
- **Purpose:**  
    - It enforces entity integrity by ensuring that each row is uniquely identifiable from a logical or business perspective (e.g., a CustomerID or OrderID).
    
- **Characteristics:**
    - **User-defined:** Chosen based on business logic.
    - **Indexed:** Automatically indexed to boost query performance.
    - **Stability:** Typically remains stable over the life of the record, though changes are possible (with cascading implications on related tables).


 What is a Row ID?
- **Definition:**  
    - A row ID (or rowid) is an internal, system-generated identifier that points to the physical location of the row in storage. Many database systems provide this as a hidden or pseudo column.
    
- **Purpose:**  
    - It is used by the database engine to quickly locate and retrieve data on the physical storage level.
    
- **Characteristics:**
    - **System-generated:** Not explicitly defined by the user; it's automatically assigned by the DBMS.
    - **Physical Locator:** Reflects the row's physical position in the database rather than its logical content.
    - **Potential Volatility:** The rowid might change if the database reorganizes or moves rows (making it less suitable for long-term external referencing).

| Aspect         | Primary Key                                            | Row ID                                                                                             |
| -------------- | ------------------------------------------------------ | -------------------------------------------------------------------------------------------------- |
| **Definition** | User-defined unique identifier based on data           | System-generated physical identifier                                                               |
| **Purpose**    | Ensures logical uniqueness and enforces data integrity | Facilitates fast data retrieval at the storage level                                               |
| **Usage**      | Used in application logic, relationships, and indexing | Typically used internally by the DBMS; available for debugging or advanced queries in some systems |
| **Stability**  | Designed to be stable and meaningful over time         | May change if the physical storage of the table is reorganized                                     |
| **Visibility** | Clearly defined in the schema                          | Often hidden or provided as a pseudo column                                                        |
 Practical Examples
- **SQLite:**  
    Every row has a unique rowid. If you declare an `INTEGER PRIMARY KEY`, SQLite uses that column as an alias for the rowid.
    
- **Oracle:**  
    The `ROWID` pseudo column shows the physical address of a row. However, it isn’t intended for long-term use in application logic because the row's physical location might change.
    
- **MySQL:**  
    When using InnoDB, the primary key acts as a clustered index that determines the physical ordering of rows. MySQL doesn’t expose a traditional rowid like Oracle or SQLite.


