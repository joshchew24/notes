# Exercise Sheet 5 Response
## Question 1
### (a) 
```java
                       // totalDependencies                 controlFlowDependencies
1	int a = 4;         // (a -> {1})                        []
2	int b = 0;         // (a -> {1}, b -> {2})              []
3	b = b + 3;         // (a -> {1}, b -> {2, 3})           []
4	a = a + a;         // (a -> {1, 4}, b -> {2, 3})        []
5	if (a < a - 1) {   // (a -> {1, 4}, b -> {2, 3})        [1,4]
6	  b = 17;          // (a -> {1, 4}, b -> {2, 3, 6})     [1,4]
7	}
8	print b;           // (a -> {1, 4}, b -> {2, 3, 6})     [1,4]
```
### (b)
to preserve the value of `b` at line 8, we would need all lines to ensure all possible definitions of `b`, so the program slice would look the same as the original. this is because static slicing will conservatively keep the if-condition, and therefore line 6 as a psosible dependency for `b`. 
### (c)
Dynamic dependencies
```java
                       // totalDependencies                 controlFlowDependencies
1	int a = 4;         // (a -> {1})                        []
2	int b = 0;         // (a -> {1}, b -> {2})              []
3	b = b + 3;         // (a -> {1}, b -> {2, 3})           []
4	a = a + a;         // (a -> {1, 4}, b -> {2, 3})        []
5	if (a < a - 1) {   // (a -> {1, 4}, b -> {2, 3})        [1,4]
6	  b = 17;          // (a -> {1, 4}, b -> {2, 3})        [1,4]
7	}
8	print b;           // (a -> {1, 4}, b -> {2, 3})    
```
### (d)
```java
1	int a = 4;
2	int b = 0;
3	b = b + 3;
4	a = a + a;
8	print b;     
```
### (e)
In static analysis, since we conservatively estimate program paths, the if-condition gets kept as a possible dependency for b. In dynamic analysis, since we can evaluate and update the dependency maps as we go through each statement, we can remove irrelevant statements from the slice. The if-condition always evaluates to false, so our dynamic analysis will catch this and remove the code that will never be run. 
### (f)
There is no user input, so the program will run the same every time. Also, there are no loops, so we don't need to perform multiple iterations.
## Question 2
### (a)
Unlike for loops, while loops don't have an explicit "increment" step, and the loop condition variable may depend on many statements within the loop body. The rules will need to track all dependencies of the loop condition variable.
### (b)
The first iteration of the loop body will not be dependent on any conditions. Any further iterations would behave the same as above.
### (c)
Aliasing variables would require maintaining equivalent mapping between names and line dependencies. This means updating both mappings, even if only one alias receives an update (assuming the alias was performed by reference and not copy). 