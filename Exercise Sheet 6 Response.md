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

  while(z < 42) {                                           {}
    z = z + 1;                    // (x -> U, z -> U + 8)
    assert z <= 42; // (I)
  }
  if(z == 12) {
    assert false;   // (II)
  } else {
    assert z <= 12;  // (III)
  }
}
```