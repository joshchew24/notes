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
  } else {                        // (x -> U, z -> S)       {S != 42}
    assert z <= 12;  // (III)     // (x -> U, z -> S)       {S != 42}         S > 12
  }
}
```
### (d)
Track the value of z to see if they are satisfied that would lead to an assertion violation. There should be an assertion violation on line III.

## Question 2
```
                                  // symbolic state        path conditions    failure condition
void foo(int x) {                 // (x -> U)              {}
  int z = x + 7;                  // (x -> U, z -> U + 7)  {}
  
  while(z < 42) {                 // (x -> U, z -> V)      {V < 42}
    z = z + 1;                    // (x -> U, z -> V)      {V < 42}
  }
  if(z == x + 7) {                // (x -> U, z -> S)      {S == U + 7}
    assert false;   // (A)        // (x -> U, z -> S)      {S == U + 7}        true
  } else {                        // (x -> U, z -> S)      {S != U + 7}
    assert false;   // (B)        // (x -> U, z -> S)      {S != U + 7}        true
  }
}
```
### (a)
`x -> U`, `z -> U + 7` initially, `z -> V` in the loop and `z -> S` after the loop. The sets overlap because z is initialized to U, then changed to V for the loop, then S after. To reach B, `x` must be `x < 36` . `U` and `S` are involved with reaching path B.
### (b)
It is possible because if `x < 10`, then `z < 17`, so the loop will be executed and grow `z` to 41. `S` will satisfy the path condition for B
### (c)
It is not possible to reach B where `x > 100`, because `z` initializes to a value `> 42`, skipping the loop and thus `S` satisfies the path condition for A
### (d)
```
                            // concrete state       symbolic state         path conditions
void foo(int x) {           // (x -> 34)            (x -> U)
  int z = x + 7;            // (x -> 34, z -> 41)   (x -> U, z -> U + 7)

  while(z < 42) {           // (x -> 34, z -> 41)   (x -> U, z -> V)       {V < 42}
    z = z + 1;              // (x -> 34, z -> 42)   (x -> U, z -> V + 1)   {V < 42}
  }
  if(z == x + 7) {          // (x -> 34, z -> 42)   (x -> U, z -> U + 8)   {U + 8 == U + 7}
    assert false;   // (A)  // (x -> 34, z -> 42)   (x -> U, z -> U + 8)   {U + 8 == U + 7}
  } else {                  // (x -> 34, z -> 42)   (x -> U, z -> U + 8)   {U + 8 != U + 7}
    assert false;   // (B)  // (x -> 34, z -> 42)   (x -> U, z -> U + 8)   {U + 8 != U + 7}
  }
}
```
Concrete values: `(x -> 34, z -> 42)`, Symbolic values: `(x -> U, z -> S)`, Path conditions: `{U + 8 != U + 7}`
### (e)
Since we know the concrete value of `z` based on the input, our path conditions for the assertions are using the same symbolic variable `U`. This acfects accuracy of test generation because it only gives path conditions for a specific input.
### (f)
`C'` is the same condition `{U + 8 != U + 7}` as the last explored path condition, which is satisfiable. We can start with `x -> 35`. The path conditions on the last conditional become `{U + & == U + 7}` and `{U + 7 != U + 7}`.
### (g)
Having a fixed input value is similar to the bounded loop unrolling and loop invariant approaches, because we have a deterministic program execution. 