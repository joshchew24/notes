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
```java
						// symbolic state                  path conds      failure conds
void foo(int a, int b) {// (a->U, b->V)  
	int x = a;          // (a->U, b->V, x->U)
	int y = b;          // (a->U, b->V, x->U, y->V
	int z = 0;          // (a->U, b->V, x->U, y->V, z->0)
	
	if (y > x) {        // (a->U, b->V, x->U, y->V, z->0)     {V>U}
		z = 2;          // (a->U, b->V, x->U, y->V, z->2)     {V>U}
	} else {            // (a->U, b->V, x->U, y->V, z->0)     {V<=U}
		y = y + 3;      // (a->U, b->V, x->U, y->V+3, z->0)   {V<=U}
		z = -2;         // (a->U, b->V, x->U, y->V+3, z->-2)  {V<=U}
	}

	if (y >= x) {
		z = z + y;
		assert x != z;
	}
}