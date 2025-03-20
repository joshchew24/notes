---
aliases:
  - Aggregation
---
# Aggregation Evaluation
- see also [[Aggregation]]
## Without [[Group By Evaluation|Group By]]
- generally requires [[Table Scan]]
- given [[Indexes|Index]] whose [[Search Key]] includes all attributes in `SELECT` and `WHERE` clause, may suffice to do [[Index Scan]]