# Exercise Sheet 6 Response
## Question 1
### (a)
#### (I)
`z > 42`
#### (II)
Always produces runtime error (asserts false)
#### (III)
`z > 12`
### (b)
Line II is creates an assertion violation if the condition `z == 12` above it evaluates to true. i.e. it has an implicit failure condition of `z == 12`.
### (c)
```
                                  // symbolic state        path conditions    failure condition
void foo(int x) {                 // (x -> U)               {}
  int z = x + 7;                  // (x -> U, z -> U + 7)   {}

  while(z < 42) {                 // (x -> U, z -> V)       {V < 42}
    z = z + 1;                    // (x -> U, z -> V + 1)   {V < 42}
	assert z <= 42; // (I)           (x -> U, z -> V + 1)   {V < 42}          V > 42
  }
  if(z == 12) {                   // (x -> U, z -> S)       {S == 42}
    assert false;   // (II)       // (x -> U, z -> S)       {S == 42}         true
  } else {                        // (x -> U, z -> S)      {S != 42}
    assert z <= 12;  // (III)     // (x -> U, z -> S)      {S != 42}         S > 12
  }
}
```
### (d)
Track the value of z to see if they are satisfied that would lead to an assertion violation. There should be an assertion violation on line III.

## Question 2
```
                                  // symbolic state        path conditions    failure condition
void foo(int x) {                 // (x -> U)               {}
  int z = x + 7;                  // (x -> U, z -> U + 7)   {}
  
  while(z < 42) {                 // (x -> U, z -> U + 8)   {U + 8 < 42}
    z = z + 1;                    // (x -> U, z -> U + 9)   {U + 8 < 42}
  }
  if(z == x + 7) {                // (x -> U, x -> )
    assert false;   // (A)
  } else {
    assert false;   // (B)
  }
}
```
### (a)
### (b)
### (c)
### (d)
### (e)
### (f)
### (g)