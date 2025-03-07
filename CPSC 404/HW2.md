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