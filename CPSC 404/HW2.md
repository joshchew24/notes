# HW2
## Question 1
- $\text{Studies(} \underline{StudyID} \text{, StudyDescription, YearOfStudy, Ethnicity, Gender, NoOfCases, CancerType, Intake, HazardRatio, ConfidenceInterval)}$
- relation: 10 million records ($10 \times 10^6$)
- record: 250 bytes
- studies conducted over 50 years
- $StudyID$ attribute: 20 byte string
- $YearOfStudy$ attribute: 4 bytes
- pointers: 10 bytes
- dense, clustered, alt2 B+ Tree index on $StudyID$
- dense, unclustered, alt2 B+ Tree index on $YearOfStudy$
- page size: 4K
- buffer size: 400 pages
### (a)
```sql
SELECT *
FROM Studies
WHERE StudyID > 'A%' AND StudyID < 'N%'
```
## Question 2
```sql
SELECT S.sname S.age R.bid
FROM Reserves R, Sailors S
WHERE R.sid = S.sid AND R.duration > 5 AND S.rating = 10
```
- $Reserves(\underline{sid} (7), \underline{date} (8), \underline{bid} (8), duration (2))$
	- $1\,000\,000\;(10^6)$ tuples
	- duration: 1-10
- $Sailors(\underline{sid} (7), sname (33), rating (3), age (2))$
	- $100\,000$ tuples
	- rating: 1-10
	- ages: 21-40
- $sid$ is a foreign key of $Reserves$
- functional dependency: $\{sname, age\} \rightarrow sid$ for $Sailors$
	- i.e. if two sailors have same name and age, they have the same ID
- page size: 4096 bytes
- buffer size: 50 pages
## Question 3
- $R\bowtie_{R.a=S.b}S$
- $R$ has $40\,000$ tuples over 1000 pages (40 tuples/page)
- $S$ has 8000 tuples over 400 pages (20 tuples/page)
- $S.b$ is PK of $S$
- both relations unsorted, no index
- $B = 52$

