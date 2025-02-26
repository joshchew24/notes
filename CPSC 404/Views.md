# Views
- predetermined queries
	- not necessarily **materialized**
		- i.e. does not store data physically in the database. it's a **virtual table** that **dynamically retrieves data from underlying tables** whenever queried
	- issuing a query on a view is a composition of the query + the view query
		- both are evaluated at runtime
	- can be profitably materialized
		- i.e. precomputed
		- if view is used a lot, efficient to **store the query** and run queries against it
		- whenever **base tables** change, must **recompute the view**
- restrict access to different classes of users to different subsets of the database