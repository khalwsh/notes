Query Engine
![[Pasted image 20250310150239.png]]

tries to change plans and estimate costs to choose the optimal one (not necessarily very optimal , but better)

so basically it do two things
- plan Enumeration
- cost comparison

Logical Nodes
- An abstract operation - cannot be executed
- Corresponds to relation algebra 
- Eg: join , scan , Aggregation , .. etc
Physical nodes
- An concrete algorithm / implementation
- can be executed has a cost function
- Eg: NL join , hash join , full scan , index scan , sort-hash based Aggregation ,... etc

Rule based optimization:
it takes the first plan take from Analyzer then apply set of rules to it to optimize the plan generally it convert to better plan , for Example Filter push down.

![[Pasted image 20250310154947.png]]

also we can do Expression Simplifications instead of calculating the expression each time for each row , we simplify it to save CPU time

![[Pasted image 20250310155240.png]]

Empty Subtree Elimination 
![[Pasted image 20250310155315.png]]

Cost based optimization
- join order
- Access path selection (Full scan , index scan , etc)
- join Algorithms (NL , hash , merge)
- Aggregation algorithms
- sorting algorithms
![[Pasted image 20250310160434.png]]

Bottom-up optimization
for the given query , enumerate and compare sub plans with
- 1 table
- 2 tables
- 3 tables
- etc
and finally choose the best

![[Pasted image 20250310161115.png]]

Advantages
- simple to implement
Disadvantages
- Adding constraints about output , sort , etc is not easy need to consider each table with different states
- Usually considers only left-deep trees
IBM system R , DB2 , MySQL , Postgres , most open-source DBMSs


Top-down optimization
- Start from logical plan (after applying rule based optimization)
- apply more rules to generate other logical and physical alternatives
- Estimate cost and choose cheapest alternative
Advantages
- sort order , output columns , etc are an essential part of the framework
- more plan alternatives
Disadvantages
- complex
used in Volcano/Cascades framework , SQL server , Greenplum DB

