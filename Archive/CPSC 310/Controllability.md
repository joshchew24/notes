# Controllability
- tests dynamically execute the system
- must be able to programatically control the SUT/CUT
- tradeoff between what code it is *possible* vs *efficient* to write a test for
- common mechanism to improve controllability
	- add additional parameters to methods or constructors
		- enables dependency injection
		- allows more expressive/complete inputs