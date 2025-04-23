# Dynamic Program Slicing
- different from [[Static Program Slicing]]
	- all **dynamically-bound** attributes are available for analysis
	- can only **analyse a single execution** at a time
## Examples
### Loop and Implicit Dependencies
#### First Loop Iteration
```java
															
/*CFD*/                     // TD
/*[]*/	    1	int n = 4;      // (n -> {1})
/*[]*/	    2	int z = 42;     // (n -> {1}, z -> {2})
/*[]*/	    3	int y = 7;      // (n -> {1}, z -> {2}, y -> {3})
/*[]*/	    4	int x = 0;      // (n -> {1}, z -> {2}, y -> {3}, x -> {4})
	        5	for (int i = 1; i != n; i++) 
/*[{1,5}]*/ 6   {               // (n -> {1}, z -> {2}, y -> {3}, x -> {4}, i->{5})
/*[{1,5}]*/ 7		x = x + y;  // (n -> {1}, z -> {2}, y -> {3}, x -> {1,3,4,5,7}, i->{5})
/*[{1,5}]*/ 8		y = z;  // (n -> {1}, z -> {2}, y -> {1,2,5,8}, x -> {1,3,4,5,7}, i->{5})
/*[]*/      9	}           // (n -> {1}, z -> {2}, y -> {1,2,5,8}, x -> {1,3,4,5,7}, i->{5})
```

#### Subsequent Iterations
```java
															
/*CFD*/                     // TD
/*[]*/	    1	int n = 4;      // (n -> {1})
/*[]*/	    2	int z = 42;     // (n -> {1}, z -> {2})
/*[]*/	    3	int y = 7;      // (n -> {1}, z -> {2}, y -> {3})
/*[]*/	    4	int x = 0;      // (n -> {1}, z -> {2}, y -> {3}, x -> {4})
	        5	for (int i = 1; i != n; i++) 
/*[{1,5}]*/ 6   {             // (n->{1}, z->{2}, y->{1,2,5,8}, x->{1,2,3,4,5,7,8}, i->{1,5})
/*[{1,5}]*/ 7		x = x + y;// (n->{1}, z->{2}, y->{1,2,5,8}, x->{1,2,3,4,5,7,8}, i->{1,5})
/*[{1,5}]*/ 8		y = z;    // (n->{1}, z->{2}, y->{1,2,5,8}, x->{1,2,3,4,5,7,8}, i->{1,5})
/*[]*/      9	}             // (n->{1}, z->{2}, y->{1,2,5,8}, x->{1,2,3,4,5,7,8}, i->{1,5})
```
- **note**: `y` doesn't depend on 3, because we get **more precise** information dynamically
- since this program has no variable inputs/parameters, this is the only possible execution
	- final dependency analysis: 
		- TD: `(n->{1}, z->{2}, y->{1,2,5,8}, x->{1,2,3,4,5,7,8}, i->{1,5})`
### Lecture Exercise
- choose an initial value for `a`
	- e.g. 1
```java
void foo (int a) {  // TD                         CFD
1   int x = 42;     // (x->{1})  
2   int y = a;      // (x->{1}, y->{2})
3   int z = 4;      // (x->{1}, y->{2}, z->{3})
4   int w = 14;     // (x->{1}, y->{2}, )

5   if (y > 0) {
6       y = z;
7       x = x + w;
8   } else {
9   }

10  if (y < 0) {
11      x = y;
12  }

13  print(x);
}
```