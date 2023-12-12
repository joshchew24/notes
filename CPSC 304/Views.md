# Views
- can be thought of as virtual tables created by masking over existing physical tables
```sql
CREATE VIEW <view name> <attributes in view> AS <view definition>
```
- \<view definition\> is defined in SQL
- views can be used as if they were regular tables
	- they exist for as long as you allow them to
	- if you want to use a temporary view, try [[Common Table Expressions (CTE)]]
- ***Materialized views***
	- actually exist on disk
	- normally views are computed whenever their alias is invoked
	- materialized views will store the tuples
## Rationale
- hide some data from users
	- e.g. UBC has one table for all students. CS department should only have access to CS students. Can be done using a view
- make some queries easier (nested)
- modularity of database
	- provide an 'interface' to the database
		- users don't need to think about how the tables are structured
		- underlying implementation can be changed

## Updating Views
- view updates must occur at the base table
	- difficult, ambiguous
- don't worry for CPSC 304

## Deleting Views
```sql
DROP VIEW <view name>
```
- does not affect any tuples of the underlying relation
- drop table can optionally have `RESTRICT` or `CASCADE
	- `RESTRICT`: will block the table from being dropped if there is a view on it
	- `CASCADE`: will recursively drop any views referencing the table