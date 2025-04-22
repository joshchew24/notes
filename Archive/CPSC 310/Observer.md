# Observer
- simplifies state update notifications
- decouple the state changing actions from their visual representation in other components
- leverages [[Dependency Inversion Principle||dependency inversion]] to implement [[Inversion Of Control | inversion of control]]
	- e.g. 
		- situation
			- watcherA and watcherB contain duplicated watching code
			- watchee contains notification code, and is contains dependencies on watchers to update them
		- all classes violate #srp by containing "update" code
		- watchee violates #dip by depending on concrete classes and their implementations of notify
- enables one-to-many relationship between objects
	- many observers, one subjet
- relationships can be added dynamically
	- dynamically add observers to a subject
- subjects are not tightly coupled to observers
- **main design question: push or pull?**
	- push: subject forwards data (e.g. `notify(this.text)`)
	- pull: subject forwards themself (e.g. `notifiy(this)`)

### Typical violations
- duplicate code (between watchers) - SRP violation
- switch on type (in subject) - OCP violation
- subject depends on observers - DIP violation
## Abstraction
- subject and observer
	- containing pulled up `update` and `notify` methods
## Polymorphism
- observer `update()`