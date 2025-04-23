# Symbolic Execution
- technique for exploring **sets of program executions** ***all at once***
- can be considered like a **value-sensitive** [[Static Program Slicing]]
## Concept
1. track input/unknown values as **symbolic values**
	- i.e. give names to unknowns
	- track a **symbolic state** in the analysis
	- map program variables to **symbolic expressions**
2. track set of **path conditions**
	- i.e. constraints representing **conditions** which **must be true** in order to **reach the current program point**
## Terminology
### Symbolic Values
- **names** representing values that are not **known statically**
- use **capital-letters**
- for **methods**, use **fresh** symbolic values for **each** of its **inputs**
- e.g. `U`, `V`
### Symbolic Expression
- **expression** over symbolic values, constants, and operators
- e.g. 
	- `U` 
	- `U + 7`
	- `U < V + 1`
### Symbolic State
- **mapping** from program **variables** to **symbolic expressions**
- used to represent **sets** of **many runtime states** all at once
- e.g.
	- `(a -> U, b -> U+2)`
		- represents runtime states where `b`'s value is 2 larger than `a`'s
### Symbolic Evaluation
- symbolic evaluation of a **program expression in a symbolic state** results in a **symbolic expression** by **replacing all variables** in the **program** expression with **their mappings** in the symbolic state
- e.g. 
	- program expression `a + b + 7`
	- symbolic state: `(a->U, b->U+2)`
	- symbolic evaluation: `U + (U + 2) + 7`
		- simplified: `2*U + 9`
## Advantages
- **never** need to **approximate** when handling if-then-else
- can optimise using **path conditions**
	- for new branch, if path conditions are not satisfiable, just skip
## Disadvantages
- requires **exponentially** many symbolic executions of paths through the program (in number of branches)
	- i.e. number of combinations grows exponentially
- **cannot** be simply extended to **handle loops**
	- treating loop-heads like conditions creates infinite branches
	- need to **approximate**
## Loop-handling
1. Find the set of variables assigned to in the loop body
2. Copy symbolic state before the loop, updating each assigned-to variable to map to a fresh symbolic value
3. Add to the path conditions the symbolic evaluation of the loop condition in this new symbolic state
4. Use this as the abstract state for start of loop body; analyse the loop body just once and then stop
5. Repeat steps 2-3 but this time symbolically evaluating the negation of the loop condition
6. Use the resulting abstract state to continue after loop
## Examples
### Simple Symbolic Execution
```java
							// symbolic state                         failure condition
void foo(int a, int b) {    // (a->U, b->V)
    int x = a;              // (a->U, b->V, x->U)
    int y = b;              // (a->U, b->V, x->U, y->V)
    int z = 0;              // (a->U, b->V, x->U, y->V, z->0)
    y = y + 3;              // (a->U, b->V, x->U, y->V+3, z->0)
    z = -2                  // (a->U, b->V, x->U, y->V+3, z->-2)
    z = z + y;              // (a->U, b->V, x->U, y->V+3, z->V+1)
    assert x != z;          // (a->U, b->V, x->U, y->V+3, z->V+1)     U == V + 1
}
```
### Simple Symbolic Execution 2
```java
							// symbolic state                         failure condition
void foo(int a, int b) {    // (a->U, b->V)
    int x = a;              // (a->U, b->V, x->U)
    int y = b;              // (a->U, b->V, x->U, y->V)
    int z = 0;              // (a->U, b->V, x->U, y->V, z->0)
    z = 2                   // (a->U, b->V, x->U, y->V, z->2)
    z = z + y;              // (a->U, b->V, x->U, y->V, z->V)
    assert x != z;          // (a->U, b->V, x->U, y->V, z->V)     U == V + 2
}
```
- satisfiable, depends on initial values of `a` and `b`
### Larger Example with branches
#### Branches 1,1
```java
						// symbolic state                  path conds      failure conds
void foo(int a, int b) {// (a->U, b->V)  
	int x = a;          // (a->U, b->V, x->U)
	int y = b;          // (a->U, b->V, x->U, y->V
	int z = 0;          // (a->U, b->V, x->U, y->V, z->0)
	
	if (y > x) {        // (a->U, b->V, x->U, y->V, z->0)     {V>U}
		z = 2;          // (a->U, b->V, x->U, y->V, z->2)     {V>U}
	} else {
		y = y + 3;
		z = -2;
	}

	if (y >= x) {       // (a->U, b->V, x->U, y->V, z->2)     {V>U, V>=U}
		z = z + y;      // (a->U, b->V, x->U, y->V, z->V+2)   {V>U, V>=U}
		assert x != z;  // (a->U, b->V, x->U, y->V, z->V+2)   {V>U, V>=U}   {U == V+2}
	}
}
```
#### Branches 2,1
```java
						// symbolic state                  path conds      failure conds
void foo(int a, int b) {// (a->U, b->V)  
	int x = a;          // (a->U, b->V, x->U)
	int y = b;          // (a->U, b->V, x->U, y->V
	int z = 0;          // (a->U, b->V, x->U, y->V, z->0)
	
	if (y > x) {
		z = 2;
	} else {            // (a->U, b->V, x->U, y->V, z->0)     {V<=U}
		y = y + 3;      // (a->U, b->V, x->U, y->V+3, z->0)   {V<=U}
		z = -2;         // (a->U, b->V, x->U, y->V+3, z->-2)  {V<=U}
	}

	if (y >= x) {       // (a->U, b->V, x->U, y->V+3, z->-2)  {V<=U, V+3>=U}
		z = z + y;      // (a->U, b->V, x->U, y->V+3, z->V+1) {V<=U, V+3>=U}
		assert x != z;  // (a->U, b->V, x->U, y->V+3, z->V+1) {V<=U, V+3>=U}  {U == V+1}
	}
}
```
- set of constraints: `V <= U`, `V + 3 >= U`, `V + 1 != U`
	- is this violatable? (i.e. can our correctness property by violated)