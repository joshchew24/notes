# Generator-Based Fuzzing
- improvement of [[Random Fuzzing]]
- grey-box
	- knows the input specifications of the SUT
- requires developer write an input generator
	- still random
	- valid for the SUT
- e.g. XML parser
	- input generator will create random XML structures 
		- instead of purely random strings
		- XML may have random tags, random nesting, etc.

## Challenges
- developer must write code for an effective input generator
- generator may be overly constrained with its output range
