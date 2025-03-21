# Exercise Sheet 5
## Question 1
### (a) 
```java
                       // totalDependencies                 controlFlowDependencies
1	int a = 4;         // (a -> {1})                        []
2	int b = 0;         // (a -> {1}, b -> {2})              []
3	b = b + 3;         // (a -> {1}, b -> {2, 3})           []
4	a = a + a;         // (a -> {1, 4}, b -> {2, 3})        []
5	if (a < a - 1) {   // (a -> {1, 4}, b -> {2, 3})        [5]
6	  b = 17;          5
7	}
8	print b;           3, 6
```

### (b)
to preserve the value of `b` at line 8, we would need all lines to ensure all possible definitions of `b`. Even though line 6 is always false, static slicing conservatively includes this line.
### (c)
1. `a = 4`
2. `b = 0`
3. `b = b + 3 -> b = 3`
4. `a = a + a -> a = 8`
5. `if (a < a - 1) -> if (8 < 7)`, evalutes to false so original line 6 is not executed
6. `print b` -> prints `3`
Dynamic dependencies
```java
1	int a = 4;        
2	int b = 0;
3	b = b + 3;         2
4	a = a + a;         1
5	if (a < a - 1) {   4
6	  b = 17;          5
7	}
8	print b;           3, 6
```