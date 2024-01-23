# Null Values
- tuples may have a null value for some of their attributes
- `IS NULL` and `IS NOT NULL` can be used to check
	- e.g. 
```sql
SELECT name
FROM Student
WHERE age IS NULL
```
- in arithmetic expressions, null operands will result in null
	- e.g. 5 + null = null

## Three Valued Logic
- null requires new truth value `unknown`
- `IS UNKNOWN` evalutes to true if predicate P evalutes to `unknown`
- NOT unknown = unknown

| OR | unknown | false   | true |
| ------- | ------- | ------- | ---- |
| unknown | unknown | unknown | true     |

| AND     | unknown | false | true |
| ------- | ------- | ----- | ---- |
| unknown | unknown | false | unknown |

- any comparison with `null` returns `unknown`
	- e.g. `5 < null`, `null <> null`, `null = null`
- result of `where` clause predicate is treated as `false` if it evaluates to `unknown`
- all aggregate operations except `COUNT(*)` ignore tuples with null values on the aggregated attributes