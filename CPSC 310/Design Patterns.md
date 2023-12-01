# Design Patterns
- people have developed design patterns to address commonly encountered problems

### Potential downsides
- easy to force a pattern into a context where it doesn't make sense
- abstraction layers decrease understandability to ease evolution
- abstraction layer for pattern may be more complex than non-patterned solution

### Benefits
- leverage many people's existing knowledge
- enhance flexibility for future change
- ease communication by using shared vocabulary
- increase reusability of developed code

### Design Pattern Format
#### Problem
- intent, motivation, applicability
#### Solution
- structure, participants, collaborations, implementation
#### Consequences
- warnings, known uses, related patterns

### Object-Oriented Design Patterns
generally use these two foundational technologies
- Abstraction
	- create more generic classes/interfaces
- Polymorphism
	- use dynamic substitution
#### How to Understand a Design Pattern
1. **What principle violations** motivate the emergence of the design pattern solution?
2. **Find the abstraction**: Which class or classes should be added at the top of the hierarchy
3. **Find the polymorphism/obliviousness**: Which methods are present in that class/interface, and how must they be overridden in concrete subtypes?
4. **Figure out when the polymorphic method(s) are called**: What is the trigger for the behaviour in the pattern
### Categories
### Creational
- deal with object creation mechanisms
- trying to create objects in a manner suitable to the situation
- [[Factory]]
- [[Singleton]]
- 
#### Structural
- ease design by identifying simple way to realize relationships between entities
- [[Composite]]
#### Behavioural
- identify common communication patterns between objects, and realize these patterns
- [[Observer]]
- [[Strategy]]
- [[State]]
- 
