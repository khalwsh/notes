
### DB types

- Relational Database (SQL databases) --> most popular
	- A **Relational Database (RDB)** is a type of database that organizes data into **tables** (also called relations) with rows and columns. It follows the **relational model**, which was introduced by **Edgar F. Codd** in 1970.
	  
	-  Key Features of Relational Databases

		1. **Tables (Relations):** Data is stored in structured tables with columns (attributes) and rows (records).
		2. **Primary Keys:** Each table has a unique identifier (primary key) to distinguish records.
		3. **Foreign Keys:** Establish relationships between tables by referencing primary keys from other tables.
		4. **Structured Query Language (SQL):** RDBs use SQL for querying and managing data.
		5. **ACID Compliance:** Ensures **Atomicity, Consistency, Isolation, and Durability**, making transactions reliable.
		6. **Normalization:** Reduces redundancy and ensures data integrity through table structuring.
	
	- Examples
		- Oracle
		- MYSQL
		- Microsoft SQL-Server
		- PostgreSQL
		  
- Graph
	- Graph databases use graph structures with nodes, edges, and properties to represent and store data. They are optimized for handling relationships between entities, such as social networks, recommendation systems, and network analysis.
	  
	- Examples
		- **Neo4j** – A popular graph database widely used for graph processing and analysis.
		- **Arango DB** – A multi-model database that supports graph, document, and key-value data models.
		- **Amazon Neptune** – A fully managed graph database service provided by AWS, supporting both property graphs and RDF graphs.
		- **Orient DB** – A multi-model database that combines graph, document, and object-oriented data models.
		  
- Key-Value
	- Key-value databases store data as key-value pairs, where each key is unique, and the value can be any data type (e.g., string, JSON, binary, etc.). They are designed for fast lookups by keys.
	
	- Examples
		- **Redis** – An in-memory key-value store known for its performance and support for various data structures.
		- **Amazon DynamoDB** – A managed NoSQL database service that provides fast and predictable performance with scalability, primarily using a key-value model.
		- **Riak** – A distributed NoSQL database that uses a key-value store for data retrieval.
		- **Berkeley DB** – A high-performance embedded key-value database.
	
	
- Column Store (Columnar Database)
	- Columnar databases store data by columns rather than rows, making them ideal for analytics and read-heavy workloads, as they allow efficient querying of large datasets by focusing on specific columns.
		
	- Examples
		- **Apache Cassandra** – A highly scalable NoSQL database that uses a column-family data model, commonly used for large-scale applications.
		- **HBase** – An open-source, distributed, column-oriented NoSQL database built on top of Hadoop, designed for large-scale data storage.
		- **Google Bigtable** – A scalable NoSQL database that stores data in a column-family format, used by Google for services like Search and Maps.
		- **Click House** – An open-source columnar database management system optimized for real-time analytics.
		  
--------
### Relational Databases

In a **relational database**, a **schema** refers to the **structure** or **blueprint** that defines the organization of data within the database. It specifies how the data is logically grouped and organized, and it includes the tables, columns, relationships, constraints, and other elements that define how the data is stored and accessed.

### **Components of a Schema:**

1. **Tables (Relations):** The schema defines the tables that hold the data, with each table consisting of rows and columns. The schema specifies the column names, data types, and other attributes.
	-  **Table Schema in a Relational Database**

		A **table schema** is the **structure** or **definition** of a specific table within a relational database. It describes the **columns, data types, constraints, keys, and relationships** for the table.

		**Components of a Table Schema:**
		
		1. **Table Name** – The name of the table in the database.
		2. **Columns (Fields)** – The attributes that define the data stored in the table.
		3. **Data Types** – Specifies the type of data that each column can hold (e.g., `INT`, `VARCHAR`, `DATE`).
		4. **Constraints** – Rules enforced on columns to ensure data integrity.
		    - **PRIMARY KEY** – Uniquely identifies each row.
		    - **FOREIGN KEY** – Establishes relationships between tables.
		    - **NOT NULL** – Ensures a column cannot contain `NULL` values.
		    - **UNIQUE** – Ensures all values in a column are distinct.
		    - **CHECK** – Enforces specific conditions on column values.
		    - **DEFAULT** – Specifies a default value for a column.
		5. **Indexes** – Improve query performance by allowing faster lookups.
    
2. **Columns (Attributes):** Each table in the schema will have a defined set of columns. The schema specifies the names and data types of these columns, such as integer, text, or date.
    
3. **Primary Key:** The schema defines the primary key for each table, which is a unique identifier for each record in the table. This ensures that no two records are identical.
    
4. **Foreign Key:** The schema also defines relationships between tables using foreign keys. A foreign key in one table refers to the primary key in another table, establishing a link between the two.
    
5. **Indexes:** The schema may define indexes to optimize data retrieval based on specific columns, improving query performance.
    
6. **Constraints:** Constraints in the schema define rules that data must follow, such as:
    
    - **NOT NULL** (ensuring a column cannot have a NULL value)
    - **UNIQUE** (ensuring all values in a column are distinct)
    - **CHECK** (ensuring values meet certain conditions)
    - **DEFAULT** (providing default values for columns when no data is specified)
7. **Views:** A schema can define views, which are virtual tables created by querying one or more tables, offering a simplified or customized representation of data.

```sql
CREATE TABLE Department (
    department_id INT PRIMARY KEY,
    department_name VARCHAR(100) NOT NULL
);

CREATE TABLE Employee (
    employee_id INT PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    department_id INT,
    salary DECIMAL(10, 2),
    FOREIGN KEY (department_id) REFERENCES Department(department_id)
);
```

In this example:

- **Department** and **Employee** tables are part of the schema.
- **department_id** is a primary key for the Department table.
- **department_id** in the Employee table is a foreign key referencing the Department table.
- **salary** column in Employee uses a **DECIMAL** data type.

In summary, a schema provides the **organizational framework** that defines how data is stored, managed, and related within a relational database.


#### Difference Between Schema and Table Schema in a Relational Database

| Feature        | Schema                                                                                                                                                                            | Table Schema                                                                                                                       |
| -------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| **Definition** | A schema is the overall **structure** of a database that defines how data is organized, including tables, relationships, constraints, views, indexes, and other database objects. | A table schema is the **structure of a specific table**, defining its columns, data types, constraints, and keys.                  |
| **Scope**      | Covers the entire **database** (all tables, relationships, views, procedures, etc.).                                                                                              | Covers a **single table** within the schema.                                                                                       |
| **Components** | Includes **multiple tables**, relationships, foreign keys, views, indexes, and more.                                                                                              | Includes **columns**, data types, primary keys, foreign keys, constraints, and indexes for one table.                              |
| **Purpose**    | Defines how the entire database is structured and how tables interact with each other.                                                                                            | Defines the specific structure of one table and how data is stored in it.                                                          |
| **Example**    | A schema might contain multiple related tables, such as `Employee`, `Department`, and `Salary`, defining relationships between them.                                              | A table schema for `Employee` defines its columns (`employee_id`, `first_name`, `last_name`, `email`, etc.) and their constraints. |

--------
### Relational Algebra

#### **What is Relational Algebra?**
Relational Algebra is a **formal query language** used to retrieve and manipulate data in a **relational database**. It provides a mathematical foundation for **SQL queries** and operates on **relations (tables)** by applying a set of operations to produce new relations.

#### **Types of Relational Algebra Operators**
Relational Algebra consists of two main types of operations:

1. **Basic (Fundamental) Operators** – Selection, Projection, Cartesian Product, Union, Difference.
2. **Derived (Additional) Operators** – Intersection, Join, Division, Assignment, Renaming.

#### **Basic Operators in Relational Algebra**

These are the core operations that form the foundation of relational algebra.

##### **1. Selection (σ - Sigma)**

- **Purpose**: Filters rows (tuples) based on a condition.
    
- **Notation**: `σ_condition(R)` → Selects rows from relation **R** where the condition is **true**.
#### 2.**Projection (π - Pi)**

- **Purpose**: Selects specific **columns (attributes)** from a table.
- **Notation**: `π_column1, column2 (R)` → Retrieves only the given columns from relation **R**.
#### **3. Cartesian Product (×)**

- **Purpose**: Combines every row from the first table with every row from the second table (results in a large number of combinations).
    
- **Notation**: `R × S`
##### **4. Union (∪)**

- **Purpose**: Combines results of two relations **with the same attributes** (removes duplicates).
    
- **Notation**: `R ∪ S`
#### **5. Set Difference (−)**

- **Purpose**: Returns all tuples in **R** that are **not in** S.
- **Notation**: `R - S`

#### Derived (Additional) Operators**

These operators are built using fundamental operators.

### **6. Intersection (∩)**

- **Purpose**: Finds common tuples between two relations.
- **Notation**: `R ∩ S`
#### **7. Join (⨝)**

- **Purpose**: Combines tables based on a **common attribute** (like SQL `JOIN`).
    
- **Types**:
    
    - **Theta Join (θ-join)**: Uses conditions like `=` or `<` to join tables.
    - **Natural Join (⋈)**: Automatically joins using common attributes.

### **8. Division (÷)**

- **Purpose**: Used to find tuples in one relation that are related to **all tuples** in another relation.
#### 9. **Rename (P)**
- Purpose: Used to change name of attribute(s) in a relation
#### **Why is Relational Algebra Important?**

1. **Foundation for SQL Queries** – SQL is based on relational algebra operations.
2. **Query Optimization** – Helps optimize database queries for faster performance.
3. **Mathematical Basis for DBMS** – Provides a theoretical framework for relational databases.
4. **Ensures Data Integrity** – Helps define operations that preserve the integrity of data.

| Feature              | Relational Algebra  | SQL |
|----------------------|--------------------|-----|
| **Type**            | Procedural (Step-by-step operations) | Declarative (Describes what to retrieve) |
| **Foundation**      | Theoretical foundation of relational databases | Practical query language used in DBMS |
| **Execution**       | Uses mathematical operations on relations | Executed by a database engine |
| **Optimization**    | Manual query optimization possible | DBMS automatically optimizes queries |
| **Operations**      | Uses operators like **σ (Selection), π (Projection), × (Cartesian Product), ∪ (Union), ⨝ (Join)** | Uses `SELECT`, `WHERE`, `JOIN`, `GROUP BY`, `HAVING` |
| **Data Representation** | Works with **relations (tables)** conceptually | Works with **tables and rows** in a DBMS |
| **Flexibility**     | More **rigid**, requires explicit steps | More **flexible**, allows subqueries and indexing |
| **Use Case**        | Used in **database theory** and query optimization | Used in **real-world applications** to interact with databases |

### **Tree Representation for Relational Algebra** 🌳

#### **What is Tree Representation in Relational Algebra?**

Tree representation is a **graphical way** to visualize how a relational algebra query is executed. It helps in understanding **query execution order**, **operator precedence**, and **query optimization** in databases.

- **Nodes (Circles/Boxes)** → Represent relational algebra operators (`σ`, `π`, `×`, `⨝`, `∪`, `-`).
- **Leaves (Bottom nodes)** → Represent the **base relations** (tables).
- **Edges (Lines)** → Represent the **flow of data** from base tables through relational operations.

Example 1: Simple Selection & Projection Query

```sql
SELECT name, department 
FROM Employee 
WHERE age > 30;
```

it's relational Algebra expression
```scss
π name, department (σ age > 30 (Employee))
```

```
         π name, department
               │
        σ age > 30
               │
          Employee
```

 **Execution Flow (Bottom to Top)**:

1. **Employee** (Base relation)
2. **σ age > 30** → Filters rows where `age > 30`

Example 2: Selection + Join + Projection
```sql
SELECT Employee.name, Department.department_name
FROM Employee 
JOIN Department ON Employee.department_id = Department.department_id
WHERE Employee.age > 30;
```

Relational Algebra Expression
```SCSS
π Employee.name, Department.department_name (σ Employee.age > 30 (Employee ⨝ Employee.department_id = Department.department_id Department))
```

```
            π Employee.name, Department.department_name
                           │
                 σ Employee.age > 30
                           │
        ⨝ Employee.department_id = Department.department_id
                    /          \
           Employee         Department

```

 **Execution Flow (Bottom to Top)**:

1. **Employee** and **Department** (Base relations)
2. **⨝ (Join)** → Matches rows where `Employee.department_id = Department.department_id`
3. **σ Employee.age > 30** → Filters employees older than 30
4. **π Employee.name, Department.department_name** → Extracts only required columns

 **Why Use Tree Representation?**

1. **Clear Execution Order** – Helps understand how queries are processed step by step.
2. **Query Optimization** – DBMS can reorder operations to execute queries faster.
3. **Visual Debugging** – Easier to spot inefficient queries.