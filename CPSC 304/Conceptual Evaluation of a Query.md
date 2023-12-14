# Conceptual Evaluation of a Query
1. compute the cross-product of *relation-list*
2. keep only tuples that satisfy *qualification*
3. partition the remaining tuples into groups by the value of attributes in *grouping-list*
4. keep only the groups that satisfy *group-qualification* (expressions in *group-qualification* must have a single value per group)
5. delete fields that are not in *target-list*
6. generate one answer tuple per qualifying group

## Query Optimization
order of evaluation is changed by a query optimizer to reduce costly operations like joins/cross product