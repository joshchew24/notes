# Mutation Testing
- evaluate the sensitivity of a test suite
	- a sensitive suite should be able to detect intentionally-introduced changes to the CUT
- mutation operators create mutant versions of the CUT
	- tend to be simple
	- change bound checks
		- `a >= arr.length` to `a > arr.length`
	- change boolean values
		- `if (isReady) {}` to `if (!isReady) {}`

## Challenge
- generating mutants that are likely to fail
- each mutant must be evaluated independently
	- 1000 mutants * 1000 test cases = 1 000 000 test cases ran

`kill score = # mutants killed / total # mutants
