

##### Nested loop join
```
for each tuple r in R: # outer 
	for each tuple s in S: # inner
		if (condition): output row
```

it is faster to let the smaller table as the outer table
- it is generally considered the worst join algorithm because for every outer row , we have a full scan of the inner input to find the matches

if their is an index on the inner table , we can optimize this scan by searching the index instead of table scan
```
for each tuple r in R: # outer 
	SS = Search Index for r.id
	for each tuple s in SS: # inner
		if (condition): output row
```

##### Merge join
```
sort first table
sort second table
merge using 2 pointers
```

##### Hash join
used in only Equality joins 
```
build hash table on S.id
foreach tuple r in R:
	ss = Search hash table for r.id
	foreach tuple s in ss:
		output row
```

