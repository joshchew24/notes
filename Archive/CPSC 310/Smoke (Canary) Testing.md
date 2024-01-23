# Smoke (Canary) Testing
- subset of test suite
	- executes quickly
	- highly reliable
	- high effectiveness
- try to expose a fault as quickly as possible
	- defer running large swaths of unnecessary tests for system known to already be broken
	- i.e. avoid running slow integration and end-to-end tests