Aggregation : combining and summarizing multiple data records into a single result set based on some criteria.
![[Pasted image 20250309013303.png]]

query example
![[Pasted image 20250309013704.png]]

Aggregation Steps 
- split input rows into groups
	- only if there is a Group BY or Distinct
	- otherwise treat every thing as 1 big group
- Combine values within each group
	- may use functions: COUNT , MIN , MAX , SUM , AVG , ....
- output one row per group

Sort-Based Aggregation
- sort by Grouping Key
- Scanning the table and only output the value not equal to last value seen
![[Pasted image 20250309020105.png]]

![[Pasted image 20250309020343.png]]
sort base aggregation need the data to be sorted first , the optimizer often doesn't use sort-base aggregation unless it is already sorted , like the data came from (index scan , merge join)


Hash-based aggregation
we iterate on the grouping key and apply hash function on it to tell which bucket we put this data in 
![[Pasted image 20250309020839.png]]

