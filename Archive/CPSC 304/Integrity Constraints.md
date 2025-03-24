# Integrity Constraints
## Review
- IC describes conditions that every *legal instance* of a relation must satisfy
	- inserts/deletes/updates that violate IC's are disallowed
	- ensure application semantics 
		- e.g. `sid` is a key
	- prevent inconsistencies
		- e.g. `sname` has to be a string
		- e.g. `age` must be < 200
## Types
- domain constraints
- primary key constraints
- foreign key constraints
- [[General Integrity Constraints|general constraints]]