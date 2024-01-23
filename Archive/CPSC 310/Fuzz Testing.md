# Fuzz Testing
- programmatically generate inputs to test
	- usually random, sometimes with some type of strategy
		- i.e. black-box, grey-box, glass-box
		- can use feedback (e.g. code coverage) to improve input generation
- often used to test that SUT does not crash
	- i.e. verify the error-handling of the SUT

## Abstract Process
1. generate input
2. execute SUT with generated input
3. observe if SUT crashed
4. if successful, repeat

## Approaches
### Black-box
- [[Random Fuzzing]]
### Grey-box
- [[Generator-Based Fuzzing]]
- Property-based 
	- similar to generator-based
	- richer assertions in the SUT
- Grammar-based
### Glass-box
- [[Mutational Fuzzing]]