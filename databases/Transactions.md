what is Transaction ? it is a group of logical operations that must be executed together or we can define it as a sequence of Read , Write operations on DB objects.
![[Pasted image 20250325144352.png]]

In SQL (DBMS - specific)
- Usually starts with BEGIN
- Ends with COMMIT or ROLLBACK

![[Pasted image 20250325145415.png]]

ACIP properties 
- Atomicity
	- Each transaction is all or nothing
- Consistency 
	- Data should be valid according to all defined rules
- Isolation
	- Transactions do not affect each others
- Durability 
	- committed data would not be lost , even after power failure

Common Problems with Concurrent Transactions
- Lost update ---> one update erase a second update
- Dirty Read ---> read value that is not committed yet
- Non-repeatable Read  ---> reading same variable with different values in same transaction

Isolation levels
- Read Uncommitted
	- No isolation - best throughput
	- Any transaction can read (uncommitted) data written by other transactions
- Read committed
	- Each operation in a transaction can only read committed data (at the time of that operation).
	- Multiple read operations may still read different values.
- Repeatable Read
	- All reads within the transaction only read committed data at the beginning of the transaction
- Serializable 
	- highest isolation level - worst throughput
	- concurrently executing transactions appears to be executing serially


Conflicting operations , a group of operations is conflicting if
- they belong to different transactions
- They access the same object
- At least one of them is a write operation

Recoverable Schedule
- if T1 writes X then T2 reads / writes X
- then T1 must commit before T2 commits
Look for the W-R and W-W conflicts -- the commits must be in the same order

Cascadeless Schedule : subset of recoverable schedules , with no cascading roll back

to check if a Schedule is Cascadeless schedule then we do the following
- look for the W-R , W-W conflicts - the commits must be in the same order
- for any W-R conflicts , the first transaction commits before the second reads
![[Pasted image 20250325175955.png]]

