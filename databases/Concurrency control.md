Concurrency control is the set of mechanisms that a database management system (DBMS) uses to ensure that transactions—independent units of work—can execute concurrently without compromising the consistency, isolation, and integrity of the data. It’s crucial for performance (to utilize resources efficiently) and correctness (to prevent anomalies).

---

## 1. Why Concurrency Control Is Needed

In a multi‑user environment, many transactions may try to read and write the same data items at the same time. Without proper coordination, this can lead to:

- **Lost Updates**: Two transactions read the same item, both modify it, and the last write “wins,” discarding the first change.
    
- **Dirty Reads**: A transaction reads data written by another uncommitted transaction; if that transaction aborts, the reader has “seen” invalid data.
    
- **Non‑repeatable Reads**: A transaction reads the same row twice and gets different values because another transaction modified (and committed) it in between.
    
- **Phantom Reads**: A transaction re‑executes a query and finds that new rows (phantoms) have appeared because another transaction inserted them.
    

These are the classic ANSI SQL isolation anomalies that concurrency control must prevent or control.

---

## 2. Fundamental Properties (ACID)

Concurrency control helps ensure the **A**tomicity, **C**onsistency, **I**solation, and **D**urability of transactions. In particular:

- **Isolation**: Each transaction should behave “as if” it were running alone, without interference.
    
- **Consistency**: Transactions must bring the database from one valid state to another, preserving integrity constraints.
    

---

## 3. Lock‑Based Protocols

### 3.1 Shared (S) and Exclusive (X) Locks

- **S‑lock**: Allows multiple transactions to read an item concurrently.
    
- **X‑lock**: Allows a single transaction to write; excludes all other reads and writes.
    

### 3.2 Two‑Phase Locking (2PL)

1. **Growing phase**: A transaction acquires all needed locks, never releasing any.
    
2. **Shrinking phase**: After releasing its first lock, it can acquire none.
    

A stricter variant, **Strict 2PL**, holds all X‑locks until commit/abort. This guarantees **conflict‑serializability** (history equivalent to some serial execution) and avoids dirty reads.

### 3.3 Dealing with Deadlocks

Lock‑based schemes can deadlock—two transactions each waiting for a lock held by the other. DBMSs detect deadlocks via a waits‑for graph and abort one victim, or prevent them via timeout or lock ordering.

---

## 4. Timestamp‑Based Protocols

Each transaction T is assigned a unique timestamp TS(T). For each data item, the system tracks the largest read‑TS and write‑TS. Operations are ordered by timestamps:

- **Read rule**: A read by T on item X is allowed if TS(T) ≥ write‑TS(X); else T is aborted.
    
- **Write rule**: A write by T on X is allowed if TS(T) ≥ read‑TS(X) and TS(T) ≥ write‑TS(X); else T is aborted.
    

This enforces a global order of transactions and also guarantees serializability, but can suffer from excessive aborts.

---

## 5. Optimistic Concurrency Control (OCC)

OCC assumes conflicts are rare and proceeds without locks in three phases:

1. **Read phase**: T reads and buffers updates, without locking.
    
2. **Validation phase**: At commit time, check whether T’s reads overlap writes of other committed transactions; if so, abort.
    
3. **Write phase**: If validation succeeds, apply buffered updates.
    

OCC works well when contention is low, because it avoids locking overhead.

---

## 6. Multiversion Concurrency Control (MVCC)

MVCC maintains multiple versions of each data item, each tagged with a timestamp or transaction ID. Readers access a snapshot consistent with their start time, never blocked by writers. Writers create new versions without overwriting old ones. Variants include:

- **Snapshot Isolation**: Each transaction sees the database as of its start. Writes succeed if no write‑write conflicts with concurrent transactions.
    
- **Serializable MVCC**: Adds conflict checks to guarantee full serializability.
    

MVCC is widely used (e.g., PostgreSQL, Oracle, MySQL’s InnoDB) because it provides high concurrency with low read latency.

---

## 7. SQL Isolation Levels

ANSI SQL defines four standard isolation levels—each permitting a set of anomalies:

|Isolation Level|Prevents Dirty Reads?|Prevents Non‑repeatable Reads?|Prevents Phantoms?|
|---|---|---|---|
|Read Uncommitted|No|No|No|
|Read Committed|Yes|No|No|
|Repeatable Read|Yes|Yes|No|
|Serializable|Yes|Yes|Yes|

Higher levels mean stricter isolation but lower concurrency and higher overhead.

---

## 8. Putting It All Together

- **Choose a protocol** based on your workload: heavy reads may favor MVCC; heavy writes may lean toward locking or OCC.
    
- **Set an isolation level** that balances correctness and performance. Many OLTP systems run at Read Committed or Snapshot Isolation for speed, accepting some anomalies.
    
- **Monitor and tune**: watch for lock contention, deadlocks, and long‑running transactions holding locks or version snapshots.