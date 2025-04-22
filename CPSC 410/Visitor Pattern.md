# Visitor Pattern
![[Pasted image 20250421203945.png]]
- object contains many elements
- client wants to get info from the object
	- calls `element.accept(visitor)` on every element of the object
- elements dispatch the request back to the visitor
	- calls `visitor.visit(this)`
		- tells the visitor the correct `visit` method to use based on its type