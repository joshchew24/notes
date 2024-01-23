# Output Partitioning
- like [[Input Partitioning]], but partitioning is driven by function outputs
- allows tester to analyze the function from a different perspective
e.g. 
```ts
/**
 * Turns a number of seconds into a 24 hour time format.
 *
 * REQUIRES: 0 <= seconds < 86400
 *
 * EFFECTS: returns a string of the format "HH:MM:SS:, where H,M,S are digits.
 * 	           if H==0, return string format "MM:SS"
 *	           if H==0 && M==0, return string format "SS"
 */
function timeAfterSeconds(seconds: number): string {
    // implementation hidden
}
```
- equivalence classes:
	- Output string has format "HH:MM:SS"
	- Output string has format "MM:SS"
	- Output string has format "SS"

e.g. 
```ts
/**
 * Given a start time and a finish time (both in seconds), return the speed of a 100 meter short distance run. Throw an exception if startTime < finishTime.
 *
 * REQUIRES: startTime, finishTime >= 0
 * 
 */
function speedOfShortDistance(startTime: number, finishTime, number): number {
    // implementation hidden
}
```
- inputs are just non-negative numbers
- outputs constraint that `finishTime` is later than `startTime`
	- two cases: valid and error
	- could also test boundary case (`finishTime == startTime`)
		- *should* be treated same as the already-validated error case, but worth checking
		- [[Boundary Value Analysis]]

## Strengths
- focuses tests on observable outputs
	- users expect the kinds of outputs they observe to be correct
- encourages test case diversity
	- different from [[Input Partitioning]]

## Caveats
- functions usually have fewer outputs than inputs
	- i.e. input set typically maps to a smaller set of outputs
	- input space is more explicit, therefore easier to test more thoroughly
- challenging to derive inputs to achieve desired output
	- especially in erroneous situations
	- e.g. output behaviour dictates how system should behave when disk space is exhausted
		- hard to test, requires mocking?