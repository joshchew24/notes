# Concolic Execution
- combination of [[Symbolic Execution]] and [[Dynamic Program Slicing|Dynamic Program Analysis]]
- **specifically aimed at test-case generations**
- concolic = **conc**rete + symb**olic**
- **during** concrete execution, also **calculate and record symbolic states** and **path conditions**
	- analysis state contains
		- symbolic state
		- path conditions
		- concretization conditions
## Steps
1. Given a **concrete input**, execute the program **concretely** and **symbolically** at the **same time**, gathering path conditions over the symbolic state.
	- if symbolic state contains **unsupported expression**, **concretize** the relevant variables and add concretization condition
2. flip last unexplored path condition to get input covering new path
3. solve new path condition: if satisfiable, get solution + return to step 1
	- use solution as inputs for next run
	- if not, go back to step 2 and try flipping next-last unexplored condition
## Advantages of Concretization
- concretization can get us **unstuck** from **operations unsupported** by symbolic execution
	- rely on SMT solver (symbolic execution) for solutions/solve path conditions
		- doesn't support **all logic**
			- e.g. modulo, external calls
## Loop-handling
- do bounded exploration of loops
	- i.e. bound number of iterations to `N`
	- record branch conditions at **each iteration**, building up a long path for loops
	- **don't flip loop exit condition** when iterations exceed `N`
## Examples
### Example 1
```java
int double(int v) {
	return 2*v;
}
void test_me(int x, int y) {
	int z = double(y);
	if (z == x) {
		if (x > y+10) {
			assert false;
		}
	}
}
```
#### Branch 1 Step by Step
![[Pasted image 20250422222954.png]]
##### Step 1
- given concrete input, execute program concretely and symbolically
- inputs: `x=3` and `y=7`
```java
int double(int v) {          // Concrete                Symbolic             Path
	return 2*v;
}
void test_me(int x, int y) { // (x->3, y->7)          (x->U, y->V)         {}
	int z = double(y);       // (x->3, y->7, z->14)   (x->U, y->V, z->2*V) {}
	if (z == x) {            // (x->3, y->7, z->14)   (x->U, y->V, z->2*V) {2*V!=U}
		if (x > y+10) {
			assert false;
		}
	}                        // (x->3, y->7, z->14)   (x->U, y->V, z->2*V) {2*V!=U}           
}
```
##### Step 2
- flip last unexplored path condition to get input covering new path
```java
int double(int v) {          // Concrete                Symbolic             Path
	return 2*v;
}
void test_me(int x, int y) { // (x->3, y->7)          (x->U, y->V)         {}
	int z = double(y);       // (x->3, y->7, z->14)   (x->U, y->V, z->2*V) {}
	if (z == x) {            // (x->3, y->7, z->14)   (x->U, y->V, z->2*V) {2*V!=U}
		if (x > y+10) {
			assert false;
		}
	}                        // (x->3, y->7, z->14)   (x->U, y->V, z->2*V) {2*V==U} 
}
```
- flipped condition: `2*V !=U` became `2*V == U`
##### Step 3
- solve new path condition
	- if satisfiable, get a solution and return to step 1
- new path condition: `2*V == U`
	- satisfiable: `V = 2`, `U = 1`
#### Branch 2
![[Pasted image 20250422223003.png]]
- inputs: `x=2`, `y=1`
```java
int double(int v) {          // Concrete             Symbolic             Path
	return 2*v;
}
void test_me(int x, int y) { // (x->2, y->1)         (x->U, y->V)         {}
	int z = double(y);       // (x->2, y->1, z->2)   (x->U, y->V, z->2*V) {}
	if (z == x) {            // (x->2, y->1, z->2)   (x->U, y->V, z->2*V) {2*V==U}
		if (x > y+10) {      // (x->2, y->1, z->2)   (x->U, y->V, z->2*V) {2*V==U, U<=V+10}
			assert false;
		}
	}                        // (x->2, y->1, z->2)   (x->U, y->V, z->2*V) {2*V==U, U>V+10}            
}
```
- after flip, conditions are `2*V == U` and `U > V + 10`
	- is it satisfiable? yes
		-  `U = 22` , `V = 11`
#### Branch 3
![[Pasted image 20250422223008.png]]
- inputs: `x=22`, `y=11`
```java
int double(int v) {          // Concrete                Symbolic             Path
	return 2*v;
}
void test_me(int x, int y) { // (x->22, y->11)          (x->U, y->V)         {}
	int z = double(y);       // (x->22, y->11, z->22)   (x->U, y->V, z->2*V) {}
	if (z == x) {            // (x->22, y->11, z->22)   (x->U, y->V, z->2*V) {2*V==U}
		if (x > y+10) {      // (x->22, y->11, z->22)   (x->U, y->V, z->2*V) {2*V==U, U>V+10}
			assert false;    !! ASSERTION ERROR
		}
	} 
}
```
### Advantages of Concretization
#### Initial
```java
int foo(int v) {             // Concrete                Symbolic                  Path
	return v*v%(50);
}
void test_me(int x, int y) { // (x->22, y->7)           (x->U, y->V)              {}
	int z = foo(y);          // (x->22, y->7, z->49)    (x->U, y->V, z-> V*V%50)  {}
	if (z == x) {            // (x->22, y->7, z->49)    (x->U, y->V, z-> V*V%50)  {V*V%50 != U}
		if (x > y+10) {
			assert false;  
		}                    // (x->22, y->7, z->49)    (x->U, y->V, z-> V*V%50)  {V*V%50 != U}
	}
}
```
- flip last condition: `V*V % 50 == U`
	- satisfiable? cannot solve
	- stuck? **concretize**
		- replace `V` with `7`
#### Concretized
```java
int foo(int v) {             // Concrete           Symbolic              Path      concr. conds
	return v*v%(50);
}
void test_me(int x, int y) { // (x->22,y->7)       (x->U,y->V)           {}       
	int z = foo(y);          // (x->22,y->7,z->49) (x->U,y->V,z->V*V%50) {}          {V==7}
	if (z == x) {            // (x->22,y->7,z->49) (x->U,y->V,z->V*V%50) {V*V%50!=U} {V==7}
		if (x > y+10) {
			assert false;  
		}                    // (x->22,y->7,z->49) (x->U,y->V,z->V*V%50) {V*V%50!=U} {V==7}
	}
}
```
- last condition: `49%50 !=U`
- flipped: `49 == U`
	- satisfiable? yes
	- solution: `U = 49`, `V = 7`
#### Retry with New Inputs from last run
```java
int foo(int v) {             // Concrete           Symbolic              Path      concr. conds
	return v*v%(50);
}
void test_me(int x, int y) { // (x->49,y->7)       (x->U,y->V)           {}       
	int z = foo(y);          // (x->49,y->7,z->49) (x->U,y->V,z->V*V%50) {}          {V==7}
	if (z == x) {            // (x->49,y->7,z->49) (x->U,y->V,z->V*V%50) {49==U}     {V==7}
		if (x > y+10) {      // (x->49,y->7,z->49) (x->U,y->V,z->V*V%50) {49==U,U>V+10} {V==7}
			assert false;    !! assertion error
		}
	}
}
```
### Loop
- loop exploration upper bound: `N=2`
- inputs: `z=2`, `n=2`
#### Iteration 1
```java
                    //   Concrete State   Symbolic State   Path
void foo(int z, int n) {// (z->2, n->2)   (z->U, n->V)     {}
	while(n > 0) {    // (z->2, n->2)     (z->U, n->V)     {V>0}
		z = z + 2;    // (z->4, n->2)     (z->U+2, n->V)   {V>0}
		n = n - 1;    // (z->4, n->1)     (z->U+2, n->V-1) {V>0}
	}
	if (z > 14) {
		assert false;
	}
}
```
#### Iteration 2
```java
                    //   Concrete State   Symbolic State   Path
void foo(int z, int n) {// (z->2, n->2)   (z->U, n->V)     {}
	while(n > 0) {    // (z->4, n->1)     (z->U+2, n->V-1) {V>0, V-1>0}
		z = z + 2;    // (z->6, n->1)     (z->U+4, n->V-1) {V>0, V-1>0}
		n = n - 1;    // (z->6, n->0)     (z->U+4, n->V-2) {V>0}
	}
	if (z > 14) {
		assert false;
	}
}
```
#### Iteration 3 (exit)
```java
                    //   Concrete State   Symbolic State   Path
void foo(int z, int n) {// (z->2, n->2)   (z->U, n->V)     {}
	while(n > 0) {    // (z->6, n->0)     (z->U+4, n->V-2) {V>0,V-1>0,V-2<=0}
		z = z + 2;
		n = n - 1;
	}
	if (z > 14) {    // (z->6, n->0)      (z->U+4, n->V-2) {V>0,V-1>0,V-2<=0, U+4<=14}
		assert false;
	}                // (z->6, n->0)      (z->U+4, n->V-2) {V>0,V-1>0,V-2<=0, U+4<=14}
}
```
- flip last condition: `U+4<=14` becomes `U+4 > 14`
	- total: `(V > 0) & (V-1 > 0) & (V-2 <= 0) & (U+4 > 14)`
	- satisfiable? yes
		- `U=11` and `V=2`
#### Next Run with new inputs (skipped to iteration 3)
```java
                    //   Concrete State   Symbolic State   Path
void foo(int z, int n) {// (z->11, n->2)   (z->U, n->V)     {}
	while(n > 0) {    // (z->15, n->0)     (z->U+4, n->V-2) {V>0,V-1>0,V-2<=0}
		z = z + 2;
		n = n - 1;
	}
	if (z > 14) {    // (z->15, n->0)      (z->U+4, n->V-2) {V>0,V-1>0,V-2<=0, U+4<=14}
		assert false; !! ASSERTION ERROR
	}
}
```