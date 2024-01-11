# Composite
## Goal
enable a group of objects to be treated as if it were one
## Summary
- represent whole-part hierarchies
- enable clients to treat individual and grouped objects similarly
- encourages extension 
	- by having an explicit extension point (component)
- can make it hard to restrict relationships
	- only specific things can be groupableer
## Common Code Smells / Violation
- duplicate code: multiple loops for any operation - #srp violation
- "inverted" switch on type: need to loop through different types separately - #dip violation
- adding new types becomes shotgun surgery
![[Pasted image 20231201024405.png]]

## Considerations
- where should add/remove be?
	- in component
		- clients oblivious to whether or not they have a composite (good, they can treat all components equally)
		- violates LSP and ISP for leaves
	- in composite
		- breaks client obliviousness
		- enables client safety (cannot add/remove from leaf)
- differentiating between leaves and composites needlessly increases complexity
	- `hasChildren()` or `isLeaf()`
	- decide between transparency and safety for the composite operations

## Abstraction
- component interface

## Polymorphism
- `operation()`
	- e.g. traverse a wiki
		- both articles and pages have `traverse()`
		- article traverse may have some of it's own code, then call `traverse` on its children
		- page traverse may print its  content