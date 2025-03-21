# Exercise Sheet 5
## Question 1
### (a) 
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

### (b)
to preserve the value of `b` at line 8, we would need all lines to ensure all possible definitions of `b`. Even though line 6 is always false, static slicing conservatively includes this line.
### (c)
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