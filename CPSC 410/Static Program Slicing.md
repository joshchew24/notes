---
aliases:
  - Value-Agnostic Program Slicing
---
# Static Program Slicing
- no tracking of variable values in the analysis itself
- **at each program point, which statements could have affected the current values of which variables**?
	- i.e. keep track of what statements affect each variable
- can create **program slices**
	- slice of the program in which all **irrelevant statements have been deleted**
	- helps answer **queries**
		- e.g. "what gets printed on the last line of a program?"
		- i.e. can tell us values of variables at specific program points
## Information
### Control Flow Dependency Stack
- tracks lines which **might** have affected values of all conditional branches/loops currently being analysed
- a stack of sets of integers (relevant statement/line numbers)
### Total Dependency Map (TD Map)
- tracks lines which have affected values of variables
	- **overestimates** conditionals
- data dependencies **and** control-flow dependencies
- a map from variable names to sets of integers (relevant statement/line numbers)
## Analysis Rules
### If-then-else branches
1. Push onto the current CFD stack the set $TD(y_0)∪TD(y_1)∪…∪TD(y_n)$ where $y_0,y_1,…,y_n$ are the variables read in the if-condition 
2. Copy the map from **before** the if-then-else to the beginnings of both blocks.
3. Step through the block statements recursively to get maps for the ends of both blocks
	1. The TD map after if-then-else maps each variable to the union of its mappings at the end of each block
4. Pop the head of CFD to get the CFD for after the if-then-else
### Loops and Implicit Dependencies
1. Process the loop body step by step (as above, starting from the map just after line 6, here)
2. Go back to the head of the loop (just after line 6, here); rewrite each variable’s mapping as:
	- The union of its current mapping and its mapping at the end of the loop body
	- <Handling of control-flow dependencies…>

Iterate 1. and 2. until nothing changes. Copy the resulting map (at loop head) to after the loop
and pop the head off the stack

## Examples
### If, no Else
```java
/*CFD*/		void foo(int a) {      // TD
			1	int x = 42;        // (x -> {1})
/*[]*/	    2	int y = a;         // (x -> {1}, y -> {2})
/*[]*/      3	int z = 4;         // (x -> {1}, y -> {2}, z -> {3})
/*[]*/      4	int w = 14;        // (x -> {1}, y -> {2}, z -> {3}, w -> {4})

/*[{2}]*/   5	if (y > 0) {       // (x -> {1}, y -> {2}, z -> {3}, w -> {4})
/*[{2}]*/   6		y = z;         // (x -> {1}, y -> {2,3,6}, z -> {3}, w -> {4})
/*[{2}]*/   7		x = x + w;     // (x -> {1,2,4,7}, y -> {2,3,6}, z -> {3}, w -> {4})
/*[{2}]*/   8	} else { //empty   // (x -> {1}, y -> {2}, z -> {3}, w -> {4})
/*[]*/      9	}                  // (x -> {1,2,4,7}, y -> {2,3,6}, z -> {3}, w -> {4})
			}
```
### If, Else
```java
/*CFD*/		void foo(int a) {      // TD
			1	int x = 42;        // (x -> {1})
/*[]*/	    2	int y = a;         // (x -> {1}, y -> {2})
/*[]*/      3	int z = 4;         // (x -> {1}, y -> {2}, z -> {3})
/*[]*/      4	int w = 14;        // (x -> {1}, y -> {2}, z -> {3}, w -> {4})

/*[{2}]*/   5	if (y > 0) {       // (x -> {1}, y -> {2}, z -> {3}, w -> {4})
/*[{2}]*/   6		y = z;         // (x -> {1}, y -> {2,3,6}, z -> {3}, w -> {4})
/*[{2}]*/   7		x = x + w;     // (x -> {1,2,4,7}, y -> {2,3,6}, z -> {3}, w -> {4})
/*[{2}]*/   8	} else { //empty   // (x -> {1}, y -> {2}, z -> {3}, w -> {4})
/*[{2}]*/   9       y = x;         // (x -> {1}, y -> {1,2,9}, z -> {3}, w -> {4})
/*[]*/      10	}                  // (x -> {1,2,4,7}, y -> {1,2,3,6,9}, z -> {3}, w -> {4})
			}
```
### If, Else, If
```java
/*CFD*/		    void foo(int a) {      // TD
			    1	int x = 42;        // (x -> {1})
/*[]*/	        2	int y = a;         // (x -> {1}, y -> {2})
/*[]*/          3	int z = 4;         // (x -> {1}, y -> {2}, z -> {3})
/*[]*/          4	int w = 14;        // (x -> {1}, y -> {2}, z -> {3}, w -> {4})

/*[{2}]*/       5	if (y > 0) {       // (x -> {1}, y -> {2}, z -> {3}, w -> {4})
/*[{2}]*/       6		y = z;         // (x -> {1}, y -> {2,3,6}, z -> {3}, w -> {4})
/*[{2}]*/       7		x = x + w;     // (x -> {1,2,4,7}, y -> {2,3,6}, z -> {3}, w -> {4})
/*[{2}]*/       8	} else { //empty   // (x -> {1}, y -> {2}, z -> {3}, w -> {4})
/*[{2}, {4}]*/  9       if (w < 4) {   // (x -> {1}, y -> {2}, z -> {3}, w -> {4})
/*[{2}, {4}]*/  10          y = x;     // (x -> {1}, y -> {1,2,4,10}, z -> {3}, w -> {4})
/*[{2}]*/       11    	}
/*[]*/          12	}               // (x -> {1,2,4,7}, y -> {1,2,3,6,9}, z -> {3}, w -> {4})
				}
```