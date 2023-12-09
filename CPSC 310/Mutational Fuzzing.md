# Mutational Fuzzing
- developer defines *mutations*
	- operations that mutates an input to something slightly different
	- e.g. for string-like inputs `<bar>f</bar>
		- can apply a set of standard mutations
			- insert-char `<bar>fk</bar>` (invalid)
			- delete-char `<ar>f</bar>` (invalid)
			- duplicate-char `<baar>f</bar>` (invalid)
		- can apply different combinations to a single *valid* seed input
			- create many valid and invalid inputs

## Challenges
- selecting seed inputs to mutate
- defining mutation set
- defining policy to determine whether and how often mutations can be combined