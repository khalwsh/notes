##### CRUD Operations
- Create / insert
- Read (SELECT)
- UPDATE
- DELETE

##### Query Engine

![[Pasted image 20250304152441.png]]

SQL parsing
```sql
SELECT name , phone from user WHERE satuts = 'active' and age > 20
```

for the system to identify the query , it identify it's tokens
![[Pasted image 20250304152637.png]]

parsing process
![[Pasted image 20250304153253.png]]
when the parsing finish with no syntax errors it generate Parse Tree (it doesn't catch all errors)
for example
![[Pasted image 20250304153648.png]]

Query Analyzer 
it primary jobs is to do semantic checks (tables exist , column exist , types , Ambiguous reference , check functions and parameters) if all right then it generate first query plan
![[Pasted image 20250304160628.png]]

query analyzer uses catalog to do its job , what is catalog? collection of meta data about table , attributes , indexes , everything in database it stored as system table and we can query it , it updated automatically.

Query Optimizer 
it's primary job to choose the best available plan to execute the query , it does this by calculating cost for each plan and try to choose the best plan (it is not guarantee that the chosen plan is the optimal but it try to) , the optimizer depend on the database statistics to estimate the cost

![[Pasted image 20250304170055.png]]


Query Execution
The actual retrieval of the result data
##### query life cycle
![[Pasted image 20250304171055.png]]