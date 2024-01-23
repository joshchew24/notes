# Test-driven Development (TDD)
- automated testing is always a key part of the development process
	- agile approaches rely on quick refactoring
		- can be risky if not easily validated
## Steps
1. Add a test
	- by thinking about testing from the outset, developers are more likely to build [[Controllability|controllable]] and [[Observability|observable]] code
	- shifts emphasis from implementing the body of functions to their API signatures
2. Run the tests to ensure they fail
	- important to know failures are the ones you expect
3. Write the code/run the tests
	- make the tests pass correctly
	- explicitly avoid extraneous functionality
		- if it was not extraneous, there would be a test for it
4. Refactor the code
	- once tests are passing, refactor
	- improve existing implementation using knowledge gained from previous step

## Challenges
- hard to write all tests before starting development
- most experienced devs write test skeletons before implementing
	- ensures that the code provides API needed to validate
	- ensures clients have access to the necessary API to use the new feature