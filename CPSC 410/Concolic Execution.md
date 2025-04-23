# Concolic Execution
- combination of [[Symbolic Execution]] and [[Dynamic Program Slicing|Dynamic Program Analysis]]
- concolic = **conc**rete + symb**olic**
- **during** concrete execution, also **calculate and record symbolic states** and **path conditions**
	- analysis state contains
		- symbolic state
		- path conditions
		- concretization conditions
## Steps
1. Given a concrete input, execute the program concretely and symbolically at the same time, gathering path conditions over the symbolic state.
2. flip last unexplored path condition to get input covering new path
3. solve new path condition: if satisfiable, get solution + return to step 1
	- if not, go back to step 2 and try flipping next-last unexplored condition
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
```java
int double(int v) {          // Concrete                Symbolic             Path
	return 2*v;
}
void test_me(int x, int y) { // (x->22, y->11)          (x->U, y->V)         {}
	int z = double(y);       // (x->22, y->11, z->22)   (x->U, y->V, z->2*V) {}
	if (z == x) {            // (x->22, y->11, z->22)   (x->U, y->V, z->2*V) {2*V==U}
		if (x > y+10) {      // (x->22, y->11, z->22)   (x->U, y->V, z->2*V) {2*V==U, U>V+10}
			assert false;
		}
	}
}
```