# Random Fuzzing
- generate completely random inputs for SUT
	- typically generates input of the correct type
- black-box, knows nothing about the SUT

## Advantages
- simple to implement
- can generate unexpected inputs
	- cover cases that testers may not think of
	- exercises code paths that are unlikely to be reached in typical usage

## Disadvantages
- inputs may be *too* random
	- e.g. fuzzing an XML parser
		- random fuzzing is very unlikely to generate strings that are valid XML
	- i.e. with programs that require highly structured inputs
		- random fuzzing unlikely to reach logic beyond input validation