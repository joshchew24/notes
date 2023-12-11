# Testing
- detects presence, *not absence*, of bugs
- helps to write [[Testability|testable]] code

## Process
steps 1-7 can occur many times before code is ready to commit
1. Develop some new code.
	- either test or product code
2. Deploy to testing environment.
	- may be skipped on small teams
3. Run your tests.
4. Test fails.
5. Try to fix the failure through more development.
	- either test or product code
6. Deploy to testing environment.
7. Run the tests again.
	- may be skipped on small teams
8. Test passes.
9. Commit/push change.
10. Pull new changes.
![[Pasted image 20231208190941.png]]

## Test Levels
Tests can range in size, complexity, execution duration, repeatability, difficulty to write/maintain/debug
### [[Unit Testing]]
#### [[Black Box Testing]]
#### [[Glass Box Testing]]
### [[Integration Testing]]
### [[End-to-End Testing]]
### [[AB Testing]]
### [[Smoke (Canary) Testing]]

## Other Approaches
### [[Fuzz Testing]]