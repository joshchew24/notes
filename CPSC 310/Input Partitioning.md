# Input Partitioning
- partition input space into equivalence classes

e.g. equivalence classes: 0-4, 5-18, 19-64, 65+
```
/**
 * Returns the PNE ticket price for the corresponding age.
 * 
 * REQUIRES: age is a natural number
 * EFFECTS:  if age < 5, return 0
 *           if age >= 5 and age < 19, return 20
 *           if age >= 19 and age < 65, return 40
 *           if age >= 65, return 15
 */
function getTicketPrice(age: number): number {
    // implementation hidden
}
```

e.g. equivalence classes: positive perfect squares, positive imperfect squares, negative numbers
```
/**
 * Returns the square root if the input number is a perfect square. 
 * Throws a NotPerfectSquareError if input number is negative or is not a perfect square.
 */
function getPerfectSquareRoot(input: number): number {
    // implementation hidden
}
```

e.g. equivalence classes: (A, positive), (A, negative), (B positive), (B negative)
```
enum Player {
    A, B
}

/**
 * Returns whether the given player is allowed to move with the given speed.  
 * Player A can only have a positive speed (moving right) and 
 * Player B can only have a negative speed (moving left).
 */
function isValidDirection(player: Player, speed: number): boolean {
    // implementation hidden
}
```

## Strengths
- function implementation is not required to validate its behaviour
	- testing is more flexible
	- avoid confirmation bias
	- test from "client perspective"
- provides systematic means to derive inputs that likely correspond to different code blocks within an implementation
- simplifies large input domains to a more approachable set of inputs

## Caveats
- dependent on specification
	- inputs partitions can miss important cases
		- if spec is incomplete
		- if implementation deviates from spec
- sometimes inputs are less interesting than outputs
	- try [[Output Partitioning]]
- defects often arise at boundaries
	- often combined with [[Boundary Value Analysis]]